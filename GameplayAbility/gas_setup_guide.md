# Unreal Engine 5: GAS（Gameplay Ability System）実装ガイド

マルチプレイヤーゲームを想定した、プレイヤー（PlayerState管理）と敵（Character自身が管理）のGAS初期化・アビリティ付与のベストプラクティス。

---

## 1. 共通の AttributeSet クラス (`MyAttributeSet`)

### MyAttributeSet.h
```cpp
#pragma once

#include "CoreMinimal.h"
#include "AttributeSet.h"
#include "AbilitySystemComponent.h"
#include "MyAttributeSet.generated.h"

#define ATTRIBUTE_ACCESSORS(ClassName, PropertyName) \
	GAMEPLAYATTRIBUTE_PROPERTY_GETTER(ClassName, PropertyName) \
	GAMEPLAYATTRIBUTE_VALUE_GETTER(PropertyName) \
	GAMEPLAYATTRIBUTE_VALUE_SETTER(PropertyName) \
	GAMEPLAYATTRIBUTE_VALUE_INITTER(PropertyName)

UCLASS()
class YOURGAME_API UMyAttributeSet : public UAttributeSet
{
	GENERATED_BODY()

public:
	UMyAttributeSet();

	virtual void GetLifetimeReplicatedProps(TArray<FLifetimeProperty>& OutLifetimeProps) const override;

	UPROPERTY(BlueprintReadOnly, Category = "Attributes", ReplicatedUsing = OnRep_Health)
	FGameplayAttributeData Health;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, Health);

protected:
	UFUNCTION()
	virtual void OnRep_Health(const FGameplayAttributeData& OldHealth);
};
```

### MyAttributeSet.cpp
```cpp
#include "MyAttributeSet.h"
#include "Net/UnrealNetwork.h"

UMyAttributeSet::UMyAttributeSet()
{
	InitHealth(100.0f);
}

void UMyAttributeSet::GetLifetimeReplicatedProps(TArray<FLifetimeProperty>& OutLifetimeProps) const
{
	Super::GetLifetimeReplicatedProps(OutLifetimeProps);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, Health, COND_None, REPNOTIFY_Always);
}

void UMyAttributeSet::OnRep_Health(const FGameplayAttributeData& OldHealth)
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, Health, OldHealth);
}
```

---

## 2. プレイヤー側の実装（PlayerState & Character）

### MyPlayerState.h
```cpp
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/PlayerState.h"
#include "AbilitySystemInterface.h"
#include "MyPlayerState.generated.h"

class UAbilitySystemComponent;
class UMyAttributeSet;

UCLASS()
class YOURGAME_API AMyPlayerState : public APlayerState, public IAbilitySystemInterface
{
	GENERATED_BODY()

public:
	AMyPlayerState();

	virtual UAbilitySystemComponent* GetAbilitySystemComponent() const override;
	UMyAttributeSet* GetAttributeSet() const { return AttributeSet; }

protected:
	UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = "GAS")
	TObjectPtr<UAbilitySystemComponent> AbilitySystemComponent;

	UPROPERTY()
	TObjectPtr<UMyAttributeSet> AttributeSet;
};
```

### MyPlayerState.cpp
```cpp
#include "MyPlayerState.h"
#include "AbilitySystemComponent.h"
#include "MyAttributeSet.h"

AMyPlayerState::AMyPlayerState()
{
	NetUpdateFrequency = 100.0f;
	AbilitySystemComponent = CreateDefaultSubobject<UAbilitySystemComponent>(TEXT("AbilitySystemComponent"));
	AbilitySystemComponent->SetReplicationMode(EGameplayEffectReplicationMode::Mixed);

	AttributeSet = CreateDefaultSubobject<UMyAttributeSet>(TEXT("AttributeSet"));
}

UAbilitySystemComponent* AMyPlayerState::GetAbilitySystemComponent() const
{
	return AbilitySystemComponent;
}
```

### MyPlayerCharacter.h
```cpp
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Character.h"
#include "AbilitySystemInterface.h"
#include "MyPlayerCharacter.generated.h"

UCLASS()
class YOURGAME_API AMyPlayerCharacter : public ACharacter, public IAbilitySystemInterface
{
	GENERATED_BODY()

public:
	AMyPlayerCharacter();

	virtual UAbilitySystemComponent* GetAbilitySystemComponent() const override;
	virtual void PossessedBy(AController* NewController) override;
	virtual void OnRep_PlayerState() override;

protected:
	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "GAS")
	TArray<TSubclassOf<class UGameplayAbility>> DefaultAbilities;

	void GiveDefaultAbilities();
};
```

### MyPlayerCharacter.cpp
```cpp
#include "MyPlayerCharacter.h"
#include "MyPlayerState.h"
#include "AbilitySystemComponent.h"

AMyPlayerCharacter::AMyPlayerCharacter() {}

UAbilitySystemComponent* AMyPlayerCharacter::GetAbilitySystemComponent() const
{
	AMyPlayerState* PS = GetPlayerState<AMyPlayerState>();
	return PS ? PS->GetAbilitySystemComponent() : nullptr;
}

void AMyPlayerCharacter::PossessedBy(AController* NewController)
{
	Super::PossessedBy(NewController);

	AMyPlayerState* PS = GetPlayerState<AMyPlayerState>();
	if (PS)
	{
		PS->GetAbilitySystemComponent()->InitAbilityActorInfo(PS, this);
		GiveDefaultAbilities();
	}
}

void AMyPlayerCharacter::OnRep_PlayerState()
{
	Super::OnRep_PlayerState();

	AMyPlayerState* PS = GetPlayerState<AMyPlayerState>();
	if (PS)
	{
		PS->GetAbilitySystemComponent()->InitAbilityActorInfo(PS, this);
	}
}

void AMyPlayerCharacter::GiveDefaultAbilities()
{
	if (!HasAuthority()) return;

	UAbilitySystemComponent* ASC = GetAbilitySystemComponent();
	if (!ASC) return;

	for (TSubclassOf<UGameplayAbility>& AbilityClass : DefaultAbilities)
	{
		if (AbilityClass)
		{
			ASC->GiveAbility(FGameplayAbilitySpec(AbilityClass, 1, INDEX_NONE, this));
		}
	}
}
```

---

## 3. 敵（AI）側の実装（Character自身が保持）

### MyEnemyCharacter.h
```cpp
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Character.h"
#include "AbilitySystemInterface.h"
#include "MyEnemyCharacter.generated.h"

class UAbilitySystemComponent;
class UMyAttributeSet;

UCLASS()
class YOURGAME_API AMyEnemyCharacter : public ACharacter, public IAbilitySystemInterface
{
	GENERATED_BODY()

public:
	AMyEnemyCharacter();

	virtual UAbilitySystemComponent* GetAbilitySystemComponent() const override;

protected:
	virtual void BeginPlay() override;

	UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = "GAS")
	TObjectPtr<UAbilitySystemComponent> AbilitySystemComponent;

	UPROPERTY()
	TObjectPtr<UMyAttributeSet> AttributeSet;

	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "GAS")
	TArray<TSubclassOf<class UGameplayAbility>> DefaultAbilities;

	void GiveDefaultAbilities();
};
```

### MyEnemyCharacter.cpp
```cpp
#include "MyEnemyCharacter.h"
#include "AbilitySystemComponent.h"
#include "MyAttributeSet.h"

AMyEnemyCharacter::AMyEnemyCharacter()
{
	AbilitySystemComponent = CreateDefaultSubobject<UAbilitySystemComponent>(TEXT("AbilitySystemComponent"));
	AbilitySystemComponent->SetReplicationMode(EGameplayEffectReplicationMode::Minimal);

	AttributeSet = CreateDefaultSubobject<UMyAttributeSet>(TEXT("AttributeSet"));
}

UAbilitySystemComponent* AMyEnemyCharacter::GetAbilitySystemComponent() const
{
	return AbilitySystemComponent;
}

void AMyEnemyCharacter::BeginPlay()
{
	Super::BeginPlay();

	if (AbilitySystemComponent)
	{
		AbilitySystemComponent->InitAbilityActorInfo(this, this);
		
		if (HasAuthority())
		{
			GiveDefaultAbilities();
		}
	}
}

void AMyEnemyCharacter::GiveDefaultAbilities()
{
	if (!HasAuthority() || !AbilitySystemComponent) return;

	for (TSubclassOf<UGameplayAbility>& AbilityClass : DefaultAbilities)
	{
		if (AbilityClass)
		{
			AbilitySystemComponent->GiveAbility(FGameplayAbilitySpec(AbilityClass, 1, INDEX_NONE, this));
		}
	}
}
```

---

## 💡 重要なマルチプレイ開発の原則
1. **PlayerStateにASCを置く理由**: キャラクター死亡時・リスポーン時にデータをリセットさせないため。
2. **`InitAbilityActorInfo()` のタイミング**: サーバー（PossessedBy）とクライアント（OnRep_PlayerState）の両方で実行を担保する。
3. **`GiveAbility()` はサーバー限定**: チート防止および正常なレプリケーションのため、必ず `HasAuthority()` のガード下で実行する。