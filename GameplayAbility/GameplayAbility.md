# ゲームプレイアビリティシステム

## 目次
- [ゲームプレイアビリティシステム](#ゲームプレイアビリティシステム)
	- [目次](#目次)
	- [アビリティシステムの概要](#アビリティシステムの概要)
		- [アビリティシステムコンポーネント](#アビリティシステムコンポーネント)
		- [アトリビュートセット](#アトリビュートセット)
		- [ゲームプレイアビリティ](#ゲームプレイアビリティ)
		- [アビリティタスク](#アビリティタスク)
		- [ゲームプレイエフェクト](#ゲームプレイエフェクト)
		- [ゲームプレイキュー](#ゲームプレイキュー)
		- [ゲームプレイタグ](#ゲームプレイタグ)
	- [アビリティシステムの環境構築](#アビリティシステムの環境構築)
		- [プラグインの有効化](#プラグインの有効化)
		- [Gameplay Abilitiesプラグインを有効化](#gameplay-abilitiesプラグインを有効化)
		- [Build.csにモジュールを追加](#buildcsにモジュールを追加)
	- [アビリティシステムのクラス構成](#アビリティシステムのクラス構成)
		- [プレイヤー](#プレイヤー)
		- [エネミー](#エネミー)
	- [アビリティシステムのレプリケーション](#アビリティシステムのレプリケーション)
	- [アビリティの作成](#アビリティの作成)
	- [アトリビュートセットの作成](#アトリビュートセットの作成)
	- [アビリティシステムコンポーネントの作成](#アビリティシステムコンポーネントの作成)
	- [プレイヤーステートの作成](#プレイヤーステートの作成)
	- [キャラクターの作成](#キャラクターの作成)
		- [ベースキャラクターの作成](#ベースキャラクターの作成)
		- [プレイヤーキャラクターの作成](#プレイヤーキャラクターの作成)
			- [アビリティシステムの初期化](#アビリティシステムの初期化)
		- [エネミーキャラクターの作成](#エネミーキャラクターの作成)
	- [アビリティーアクター情報の初期化](#アビリティーアクター情報の初期化)
	- [レプリケーションモード](#レプリケーションモード)

## アビリティシステムの概要
アビリティシステムは以下のパーツから構成されています。  
これらが連携することによりアビリティシステムの機能を実現しています。
### アビリティシステムコンポーネント
アビリティシステムの基本となるコンポーネントです。対象のアクターにアタッチしてアビリティシステム全般の制御を行います。
### アトリビュートセット
キャラクターが保持するSTR,INT,DEX,HP,MPなどのパラメータセットです。アビリティシステムコンポーネントと同様に対象のアクターにアタッチして保持、管理します。
### ゲームプレイアビリティ
ジャンプ、攻撃、防御などのアクションを記述するクラスです。アクターはアビリティシステムコンポーネントにアビリティを登録することでアクションが実行できるようになります。  
アビリティは１つのクラスとして独立したコードで書けるのでアクター本体の実装に依存せずに機能を実装できます。また、アタッチ、デタッチするだけでアクターへの能力付与、削除が簡単にできるのでメンテナンス性に優れます。
### アビリティタスク
ゲームプレイアビリティで使われるワーカースレッドのようなものです。ゲームプレイアビリティはアビリティ開始、終了、中断などタイミングでコールバックを受けるので、そこに必要なロジックを記述できますが、呪文や溜め攻撃のような開始～待機～発動といった時間経過を伴うものはゲームプレイアビリティy内でアビリティタスクを生成して処理を行います。
### ゲームプレイエフェクト
アトリビュートの値を変更させるものです。アトリビュートの値は通常、直接書き換えるものではなく、アビリティシステムコンポーネントにゲームプレイエフェクトを適用することにより変化させます。例えば敵からダメージを受けてHPを減少させる場合、HPを減らすゲームプレイエフェクトを作成してアビリティシステムコンポーネントに適用させます。また、ゲームプレイエフェクトは持続時間なども設定できるので一定期間アトリビュート値を変化させるバフ、デバフなどにも使用できます。
### ゲームプレイキュー
ゲームプレイエフェクトを適用したことにより発生するパーティクルやSEなどのエフェクトを定義します。ゲームプレイキューはレプリケートに対応しているので、対象のアクターにゲームプレイエフェクトを適用すると各クライアントでも自動的にエフェクトを表示してくれます。
### ゲームプレイタグ
ゲームプレイアビリティシステム全般にわたって使用されるタグ定義です。ゲームプレイアビリティの起動からキャラクターのステート管理、イベント発生時の通知パラメータなど幅広く利用されます。

## アビリティシステムの環境構築
アビリティシステムを使用するためには以下の環境設定が必要になります。

### プラグインの有効化
GameplayAbilityを使うためにはプラグインを以下の手順で有効化する必要があります。  

### Gameplay Abilitiesプラグインを有効化
エディタのプラグインで、Gameplay Abilitiesを有効化にします。  
エディタを再起動させます

### Build.csにモジュールを追加  
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyProject.Build.cs
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```csharp
// Copyright Epic Games, Inc. All Rights Reserved.

using UnrealBuildTool;

public class Eta : ModuleRules
{
	public Eta(ReadOnlyTargetRules Target) : base(Target)
	{
		PCHUsage = PCHUsageMode.UseExplicitOrSharedPCHs;
	
		PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore", "EnhancedInput", "GameplayAbilities" });

		PrivateDependencyModuleNames.AddRange(new string[] { "ImGui", "GameplayTags", "GameplayTasks" });

		// .hと.cppを同一フォルダに配置してインクルードを通す設定
		PublicIncludePaths.AddRange(new string[] { ModuleDirectory });

		// Uncomment if you are using Slate UI
		// PrivateDependencyModuleNames.AddRange(new string[] { "Slate", "SlateCore" });

		// Uncomment if you are using online features
		// PrivateDependencyModuleNames.Add("OnlineSubsystem");

		// To include OnlineSubsystemSteam, add it to the plugins section in your uproject file with the Enabled attribute set to true
	}
}
```
</div>
<br>

## アビリティシステムのクラス構成
アトリビュートを持ちアクションを行うのはアクターなので、アクターにアビリティシステムコンポーネントとアトリビュートセットを持たせるのが自然な形なのですが、その実装だとプレイヤーが操作しているキャラクターが死んでリスポーンする場合などに問題になります。  
そこでライフタイムが一代限りの敵などのキャラクターについてはアビリティシステムコンポーネントとアトリビュートを直接持たせ、プレイヤーキャラクターについてはプレイヤーステートでアビリティシステムコンポーネントとアトリビュートを保持し、キャラクター側にはそのポインタを持たせる設計にします。
### プレイヤー
- プレイヤーステート
  - アビリティシステムコンポーネント
  - アトリビュートセット
- プレイヤーキャラクター
  - アビリティシステムコンポーネントのポインタ(プレイヤーステート保持)
  - アトリビュートセットのポインタ(プレイヤーステート保持)
### エネミー
- キャラクター
  - アビリティシステムコンポーネント
  - アトリビュートセット

## アビリティシステムのレプリケーション
アビリティシステムはネットワーク対応で３つのレプリケーションモードがあります。このモードによりゲームプレイエフェクトのレプリケーションのされ方が変わります。
- 完全 (Full)
  - アクティブなGEのすべての詳細（持続時間、スタック数、タグのカウントなど）をすべてのクライアントにレプリケートします。
  - プレイヤーステータスを他者から完全に把握する必要がある場合に向いていますが、ネットワーク帯域の負荷が高くなります。
- 混合 (Mixed)
  - 所有している本人（Local Player / Owner）には「完全詳細」を送り、他のプレイヤーや観戦者には「最小限（Minimal）」の情報だけを送ります。
  - 自分のUIには正確なクールダウンやバフ・デバフの残り時間を表示させつつ、他人からは見えないようにしてネットワーク負荷を抑える、プレイヤーキャラクターの標準的な推奨設定です
- 最小 (Minimal)
  - 所有権に関わらず、付与されているタグやゲームプレイキュー（Gameplay Cue）の情報のみを最小限レプリケートします。
  - 内部的なダメージ計算や詳細なスタック数を他のクライアントが知る必要のない、数多くスポーンするAIや敵キャラクター（Enemy）に最適です。

上記を鑑みて今回のソースではアビリティシステムコンポーネントについてはプレイヤーキャラクターのMixed、エネミーキャラクターについてはMinimalを設定します。  

なお、アビリティシステムコンポーネントのレプリケーションモードに混合(Mixed)を使う場合、以下の注意点があります。

- アビリティシステムコンポーネントの初期化関数InitAbilityActorInfoで指定するオーナーアクターはコントローラークラスである必要がある。
- プレイヤーステートのオーナーはコントローラーなので、プレイヤーステートをオーナーアクターに指定するのは問題ない。
- オーナーアクターがプレイヤーコントローラーやプレイヤーステートではない場合は、オーナーアクターのオーナーにはSetOwnerでコントローラーを指定する必要がある。

今回のソースでは、プレイヤーキャラクターが混合 (Mixed)を使用するため、InitAbilityActorInfoで指定するオーナーアクターはプレイヤーステートになります。

## アビリティの作成
プロジェクト用のアビリティクラスを作成します。
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyGameplayAbility.h
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "Abilities/GameplayAbility.h"
#include "MyGameplayAbility.generated.h"

/**
 * 
 */
UCLASS()
class ETA_API UMyGameplayAbility : public UGameplayAbility
{
	GENERATED_BODY()
	
};
```
</div>
<br>
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyGameplayAbility.cpp
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.


#include "Characters/Common/AbilitySystem/Abilities/MyGameplayAbility.h"

```
</div>
<br>

## アトリビュートセットの作成
アトリビュートセットクラスを作成し、以下の実装を行います。
- ゲームで使用するアトリビュートの定義
  - プライマリアトリビュート  
    キャラクターの基本的な能力  
    Strength,Intelligenceなど
  - セカンダリアトリビュート  
    プライマリアトリビュートから計算されるアトリビュート  
    MaxHealth,MaxMana,Armor,ArmorPenetrationなど
  - バイタルアトリビュート  
    動的に変化するアトリビュート  
    Health,Manaなど
  - メタアトリビュート  
    レプリケートしないサーバー上の計算で使われるアトリビュート  
    IncomingDamage,IncomingXPなど

  各プロパティには以下の設定を行います
  - マクロを使ってアクセッサを用意  
  - プロパティ指定子ReplicatedUsingでレプリケートコールバックを登録
- GetLifetimeReplicatedPropsのオーバーライド  
  DOREPLIFETIME_CONDITION_NOTIFYでアトリビュートのレプリケート設定を指定
- PreAttributeChangeのオーバーライド  
  ゲームプレイエフェクトの評価(マグニチュード計算、ExecutionCalculation実行)により変更すべきアトリビュートと値が決定されます。その後、各アトリビュートに対して「これから値を変える」段階でこの関数が呼ばれます。  
  ここではセットしようとしている新しい値に対し補正、クランプをかけることが可能です。  
  例)HealthとHealthMax、ManaとManaMaxなどのクランプ処理を行う
- PostGameplayEffectExecuteのオーバーライド  
  GameplayEffect適用後の呼ばれる処理。
  各アトリビュートに対して「値が変わった」段階でこの関数が呼ばれます。  
  アトリビュート変更後の
  - 最終的なクランプ
  - メタアトリビュートによる計算と結果の反映
  - アトリビュート変更に付随する追加処理全般  
    ゲームプレイエフェクトの適用、UIやエフェクトの表示処理

  などに使います。

<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyAttributeSet.h
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
// Fill out your copyright notice in the Description page of Project Settings.

#pragma once

#include "CoreMinimal.h"
#include "AttributeSet.h"
#include "AbilitySystemComponent.h"
#include "MyAttributeSet.generated.h"

// アトリビュートのアクセス用マクロ
#define ATTRIBUTE_ACCESSORS(ClassName, PropertyName) \
	GAMEPLAYATTRIBUTE_PROPERTY_GETTER(ClassName, PropertyName) \
	GAMEPLAYATTRIBUTE_VALUE_GETTER(PropertyName) \
	GAMEPLAYATTRIBUTE_VALUE_SETTER(PropertyName) \
	GAMEPLAYATTRIBUTE_VALUE_INITTER(PropertyName)

/**
 * ゲームで使用するアトリビュートの設定クラス
 */
UCLASS()
class ETA_API UMyAttributeSet : public UAttributeSet
{
	GENERATED_BODY()
	
public:
	/*
	 * プライマリーアトリビュート
	 */
	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_Strength, Category = "Primary Attributes")
	FGameplayAttributeData Strength;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, Strength);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_Intelligence, Category = "Primary Attributes")
	FGameplayAttributeData Intelligence;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, Intelligence);
	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_Resilience, Category = "Primary Attributes")

	FGameplayAttributeData Resilience;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, Resilience);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_Vigor, Category = "Primary Attributes")
	FGameplayAttributeData Vigor;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, Vigor);

	/*
	 * セカンダリーアトリビュート
	 */
	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_Armor, Category = "Secondary Attributes")
	FGameplayAttributeData Armor;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, Armor);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_ArmorPenetration, Category = "Secondary Attributes")
	FGameplayAttributeData ArmorPenetration;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, ArmorPenetration);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_BlockChance, Category = "Secondary Attributes")
	FGameplayAttributeData BlockChance;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, BlockChance);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_CriticalHitChance, Category = "Secondary Attributes")
	FGameplayAttributeData CriticalHitChance;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, CriticalHitChance);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_CriticalHitDamage, Category = "Secondary Attributes")
	FGameplayAttributeData CriticalHitDamage;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, CriticalHitDamage);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_CriticalHitResistance, Category = "Secondary Attributes")
	FGameplayAttributeData CriticalHitResistance;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, CriticalHitResistance);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_HealthRegeneration, Category = "Secondary Attributes")
	FGameplayAttributeData HealthRegeneration;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, HealthRegeneration);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_ManaRegeneration, Category = "Secondary Attributes")
	FGameplayAttributeData ManaRegeneration;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, ManaRegeneration);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_MaxHealth, Category = "Vital Attributes")
	FGameplayAttributeData MaxHealth;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, MaxHealth);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_MaxMana, Category = "Vital Attributes")
	FGameplayAttributeData MaxMana;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, MaxMana);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_FireResistance, Category = "Resistance Attributes")
	FGameplayAttributeData FireResistance;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, FireResistance);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_LightningResistance, Category = "Resistance Attributes")
	FGameplayAttributeData LightningResistance;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, LightningResistance);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_ArcaneResistance, Category = "Resistance Attributes")
	FGameplayAttributeData ArcaneResistance;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, ArcaneResistance);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_PhysicalResistance, Category = "Resistance Attributes")
	FGameplayAttributeData PhysicalResistance;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, PhysicalResistance);

	/*
	 * バイタルアトリビュート
	 */
	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_Health, Category = "Vital Attributes")
	FGameplayAttributeData Health;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, Health);

	UPROPERTY(BlueprintReadOnly, ReplicatedUsing = OnRep_Mana, Category = "Vital Attributes")
	FGameplayAttributeData Mana;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, Mana);

	/*
	 * メタアトリビュート
	 *
	 * GASメタアトリビュートという型が用意されているわけではない
	 * レプリケートせず、サーバー上で計算を行うためだけ用意するアトリビュート
	 */
	 // 到着ダメージ。この値に防御やパリーなどの要素を加味して受けるダメージを計算する
	UPROPERTY(BlueprintReadOnly, Category = "Meta Attributes")
	FGameplayAttributeData IncomingDamage;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, IncomingDamage);

	// 取得経験値
	UPROPERTY(BlueprintReadOnly, Category = "Meta Attributes")
	FGameplayAttributeData IncomingXP;
	ATTRIBUTE_ACCESSORS(UMyAttributeSet, IncomingXP);

	/*
	 * プライマリーアトリビュートのレプリケーション関数
	 */
	UFUNCTION()
	void OnRep_Strength(const FGameplayAttributeData& OldStrength) const;

	UFUNCTION()
	void OnRep_Intelligence(const FGameplayAttributeData& OldIntelligence) const;

	UFUNCTION()
	void OnRep_Resilience(const FGameplayAttributeData& OldResilience) const;

	UFUNCTION()
	void OnRep_Vigor(const FGameplayAttributeData& OldVigor) const;

	/*
	 * セカンダリーアトリビュートのレプリケーション関数
	 */
	UFUNCTION()
	void OnRep_Armor(const FGameplayAttributeData& OldArmor) const;

	UFUNCTION()
	void OnRep_ArmorPenetration(const FGameplayAttributeData& OldArmorPenetration) const;

	UFUNCTION()
	void OnRep_BlockChance(const FGameplayAttributeData& OldBlockChance) const;

	UFUNCTION()
	void OnRep_CriticalHitChance(const FGameplayAttributeData& OldCriticalHitChance) const;

	UFUNCTION()
	void OnRep_CriticalHitDamage(const FGameplayAttributeData& OldCriticalHitDamage) const;

	UFUNCTION()
	void OnRep_CriticalHitResistance(const FGameplayAttributeData& OldCriticalHitResistance) const;

	UFUNCTION()
	void OnRep_HealthRegeneration(const FGameplayAttributeData& OldHealthRegeneration) const;

	UFUNCTION()
	void OnRep_ManaRegeneration(const FGameplayAttributeData& OldManaRegeneration) const;

	UFUNCTION()
	void OnRep_MaxHealth(const FGameplayAttributeData& OldMaxHealth) const;

	UFUNCTION()
	void OnRep_MaxMana(const FGameplayAttributeData& OldMaxMana) const;

	/*
	* バイタルアトリビュートのレプリケーション関数
	*/
	UFUNCTION()
	void OnRep_Health(const FGameplayAttributeData& OldHealth) const;

	UFUNCTION()
	void OnRep_Mana(const FGameplayAttributeData& OldMana) const;

	/*
	 * 抵抗値アトリビュートのレプリケーション関数
	 */
	UFUNCTION()
	void OnRep_FireResistance(const FGameplayAttributeData& OldFireResistance) const;

	UFUNCTION()
	void OnRep_LightningResistance(const FGameplayAttributeData& OldLightningResistance) const;

	UFUNCTION()
	void OnRep_ArcaneResistance(const FGameplayAttributeData& OldArcaneResistance) const;

	UFUNCTION()
	void OnRep_PhysicalResistance(const FGameplayAttributeData& OldPhysicalResistance) const;

	// アトリビュートのレプリケーション設定
	virtual void GetLifetimeReplicatedProps(TArray<FLifetimeProperty>& OutLifetimeProps) const override;

	// GameplayEffect適用前の処理
	virtual void PreAttributeChange(const FGameplayAttribute& Attribute, float& NewValue) override;

	// GameplayEffect適用後の処理
	virtual void PostGameplayEffectExecute(const FGameplayEffectModCallbackData& Data) override;

	// アトリビュート変更後の処理
	virtual void PostAttributeChange(const FGameplayAttribute& Attribute, float OldValue, float NewValue) override;

private:
};
```
</div>
  
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyAttributeSet.cpp
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
// Fill out your copyright notice in the Description page of Project Settings.


#include "Characters/Common/AbilitySystem/MyAttributeSet.h"
#include "GameplayEffectExtension.h"
#include "Net/UnrealNetwork.h"

// プライマリーアトリビュート
void UMyAttributeSet::OnRep_Strength(const FGameplayAttributeData& OldStrength) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, Strength, OldStrength);
}

void UMyAttributeSet::OnRep_Intelligence(const FGameplayAttributeData& OldIntelligence) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, Intelligence, OldIntelligence);
}

void UMyAttributeSet::OnRep_Resilience(const FGameplayAttributeData& OldResilience) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, Resilience, OldResilience);
}

void UMyAttributeSet::OnRep_Vigor(const FGameplayAttributeData& OldVigor) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, Vigor, OldVigor);
}

// セカンダリーアトリビュート
void UMyAttributeSet::OnRep_Armor(const FGameplayAttributeData& OldArmor) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, Armor, OldArmor);
}

void UMyAttributeSet::OnRep_ArmorPenetration(const FGameplayAttributeData& OldArmorPenetration) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, ArmorPenetration, OldArmorPenetration);
}

void UMyAttributeSet::OnRep_BlockChance(const FGameplayAttributeData& OldBlockChance) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, BlockChance, OldBlockChance);
}

void UMyAttributeSet::OnRep_CriticalHitChance(const FGameplayAttributeData& OldCriticalHitChance) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, CriticalHitChance, OldCriticalHitChance);
}

void UMyAttributeSet::OnRep_CriticalHitDamage(const FGameplayAttributeData& OldCriticalHitDamage) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, CriticalHitDamage, OldCriticalHitDamage);
}

void UMyAttributeSet::OnRep_CriticalHitResistance(const FGameplayAttributeData& OldCriticalHitResistance) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, CriticalHitResistance, OldCriticalHitResistance);
}

void UMyAttributeSet::OnRep_HealthRegeneration(const FGameplayAttributeData& OldHealthRegeneration) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, HealthRegeneration, OldHealthRegeneration);
}

void UMyAttributeSet::OnRep_ManaRegeneration(const FGameplayAttributeData& OldManaRegeneration) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, ManaRegeneration, OldManaRegeneration);
}

void UMyAttributeSet::OnRep_MaxHealth(const FGameplayAttributeData& OldMaxHealth) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, MaxHealth, OldMaxHealth);
}

void UMyAttributeSet::OnRep_MaxMana(const FGameplayAttributeData& OldMaxMana) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, MaxMana, OldMaxMana);
}

// バイタルアトリビュート
void UMyAttributeSet::OnRep_Health(const FGameplayAttributeData& OldHealth) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, Health, OldHealth);
}

void UMyAttributeSet::OnRep_Mana(const FGameplayAttributeData& OldMana) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, Mana, OldMana);
}

/*
 * 抵抗値アトリビュート
 */
void UMyAttributeSet::OnRep_FireResistance(const FGameplayAttributeData& OldFireResistance) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, FireResistance, OldFireResistance);
}

void UMyAttributeSet::OnRep_LightningResistance(const FGameplayAttributeData& OldLightningResistance) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, LightningResistance, OldLightningResistance);
}

void UMyAttributeSet::OnRep_ArcaneResistance(const FGameplayAttributeData& OldArcaneResistance) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, ArcaneResistance, OldArcaneResistance);
}

void UMyAttributeSet::OnRep_PhysicalResistance(const FGameplayAttributeData& OldPhysicalResistance) const
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UMyAttributeSet, PhysicalResistance, OldPhysicalResistance);
}

void UMyAttributeSet::GetLifetimeReplicatedProps(TArray<FLifetimeProperty>& OutLifetimeProps) const
{
	Super::GetLifetimeReplicatedProps(OutLifetimeProps);

	// プライマリーアトリビュート
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, Strength, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, Intelligence, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, Resilience, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, Vigor, COND_None, REPNOTIFY_Always);

	// セカンダリーアトリビュート
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, Armor, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, ArmorPenetration, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, BlockChance, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, CriticalHitChance, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, CriticalHitDamage, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, CriticalHitResistance, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, HealthRegeneration, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, ManaRegeneration, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, MaxHealth, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, MaxMana, COND_None, REPNOTIFY_Always);


	// 抵抗値アトリビュート
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, FireResistance, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, LightningResistance, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, ArcaneResistance, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, PhysicalResistance, COND_None, REPNOTIFY_Always);

	// バイタルアトリビュート
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, Health, COND_None, REPNOTIFY_Always);
	DOREPLIFETIME_CONDITION_NOTIFY(UMyAttributeSet, Mana, COND_None, REPNOTIFY_Always);
}

void UMyAttributeSet::PreAttributeChange(const FGameplayAttribute& Attribute, float& NewValue)
{
	Super::PreAttributeChange(Attribute, NewValue);
	if (Attribute == GetHealthAttribute())
	{
		NewValue = FMath::Clamp(NewValue, 0.f, GetMaxHealth());
	}
	if (Attribute == GetManaAttribute())
	{
		NewValue = FMath::Clamp(NewValue, 0.f, GetMaxMana());
	}
}

void UMyAttributeSet::PostGameplayEffectExecute(const FGameplayEffectModCallbackData& Data)
{
	Super::PostGameplayEffectExecute(Data);

	// アトリビュートをクランプする
	if (Data.EvaluatedData.Attribute == GetHealthAttribute())
	{
		SetHealth(FMath::Clamp(GetHealth(), 0.f, GetMaxHealth()));
	}
	if (Data.EvaluatedData.Attribute == GetManaAttribute())
	{
		SetMana(FMath::Clamp(GetMana(), 0.f, GetMaxMana()));
	}

	// メタアトリビュートを使った計算
}

// アトリビュート変更後の処理
void UMyAttributeSet::PostAttributeChange(const FGameplayAttribute& Attribute, float OldValue, float NewValue)
{
	Super::PostAttributeChange(Attribute, OldValue, NewValue);
}
```
</div>
<br>

## アビリティシステムコンポーネントの作成
アビリティシステムの基幹となるアビリティシステムコンポーネントを作成します。  
ここではキャラクターに通常のアビリティとパッシブアビリティの2種類を与える関数を用意します。パッシブアビリティは付与とアクティベートが同時に行われるアビリティになります。

<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyAbilitySystemComponent.h
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "AbilitySystemComponent.h"
#include "MyAbilitySystemComponent.generated.h"

/**
 * 
 */
UCLASS()
class ETA_API UMyAbilitySystemComponent : public UAbilitySystemComponent
{
	GENERATED_BODY()
	
public:
	// ASCにアビリティ付与
	void AddCharacterAbilities(const TArray<TSubclassOf<UGameplayAbility>>& Abilities, int32 Level = 1);
	// ASCにパッシブアビリティ付与
	void AddCharacterPassiveAbilities(const TArray<TSubclassOf<UGameplayAbility>>& PassiveAbilities, int32 Level = 1);

private:
	// アビリティ付与済みフラグ
	bool bStartupAbilitiesGiven = false;
};
```
</div>
<br>
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyAbilitySystemComponent.cpp
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.


#include "Characters/Common/AbilitySystem/MyAbilitySystemComponent.h"
#include "Abilities/MyGameplayAbility.h"

// ASCにアビリティ付与
void UMyAbilitySystemComponent::AddCharacterAbilities(const TArray<TSubclassOf<UGameplayAbility>>& Abilities, int32 Level)
{
	for (TSubclassOf<UGameplayAbility> AbilityClass : Abilities)
	{
		FGameplayAbilitySpec AbilitySpec = FGameplayAbilitySpec(AbilityClass, Level);
		if (const UMyGameplayAbility* MyAbility = Cast<UMyGameplayAbility>(AbilitySpec.Ability))
		{
			GiveAbility(AbilitySpec);
		}
	}
	// アビリティ付与完了後にフラグを立ててデリゲートをブロードキャスト
	bStartupAbilitiesGiven = true;
}

// ASCにパッシブアビリティ付与
void UMyAbilitySystemComponent::AddCharacterPassiveAbilities(const TArray<TSubclassOf<UGameplayAbility>>& PassiveAbilities, int32 Level)
{
	for (const TSubclassOf<UGameplayAbility> AbilityClass : PassiveAbilities)
	{
		// アビリティ付与と同時にアクティブ化も行う
		FGameplayAbilitySpec AbilitySpec = FGameplayAbilitySpec(AbilityClass, Level);
		GiveAbilityAndActivateOnce(AbilitySpec);
	}
}
```
</div>
<br>


## プレイヤーステートの作成
プロジェクト用のプレイヤーステータクラスを作成します。  
コンストラクタでアクターをレプリケートする頻度を指定するNetUpdateFrequencyを100に指定します。  
NetUpdateFrequencyはレプリケートする秒間の頻度で、100を指定すると1秒間に100回(0.01秒間隔)で更新を試みる設定になります。
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyPlayerState.h
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "GameFramework/PlayerState.h"
#include "MyPlayerState.generated.h"

/**
 * 
 */
UCLASS()
class ETA_API AMyPlayerState : public APlayerState
{
	GENERATED_BODY()
	
};
```
</div>
<br>
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyPlayerState.cpp
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.


#include "Player/MyPlayerState.h"

```
</div>
<br>


## キャラクターの作成
### ベースキャラクターの作成
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyCharacter.h
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Character.h"
#include "MyCharacter.generated.h"

UCLASS()
class ETA_API AMyCharacter : public ACharacter
{
	GENERATED_BODY()

public:
	// Sets default values for this character's properties
	AMyCharacter();

protected:
	// Called when the game starts or when spawned
	virtual void BeginPlay() override;

public:	
	// Called every frame
	virtual void Tick(float DeltaTime) override;

	// Called to bind functionality to input
	virtual void SetupPlayerInputComponent(class UInputComponent* PlayerInputComponent) override;

};
```
</div>
<br>
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyCharacter.cpp
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.


#include "Characters/Common/MyCharacter.h"

// Sets default values
AMyCharacter::AMyCharacter()
{
 	// Set this character to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = true;

}

// Called when the game starts or when spawned
void AMyCharacter::BeginPlay()
{
	Super::BeginPlay();
	
}

// Called every frame
void AMyCharacter::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);

}

// Called to bind functionality to input
void AMyCharacter::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
	Super::SetupPlayerInputComponent(PlayerInputComponent);

}

```
</div>
<br>

### プレイヤーキャラクターの作成
#### アビリティシステムの初期化  
アビリティシステムの初期化関数であるInitAbilityActorInfoを呼ぶためにはオーナーアクターとワールドに設置される物理的なアクターの２つが必要です。  
物理的なアクターはプレイヤーキャラクターなのですが、オーナーアクターはプレイヤーステートになるため、この２つが揃うのはPossessedByが呼ばれた時点となります。
### エネミーキャラクターの作成

## アビリティーアクター情報の初期化

## レプリケーションモード

キャラクターにはStrength,Intelligence,Health,Manaといったパラメータが存在します。  
ゲームプレイアビリティではこれらをアトリビュートというfloat値で表し、それを1セットにまとめたものをアトリビュートセットという形でキャラクターに持たせます。

