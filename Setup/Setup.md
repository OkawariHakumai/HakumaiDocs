# セットアップ

## エンジンにエディタシンボルを追加
VisualStudioでデバッグを行うためにエディタのシンボルをインストール
<div style="display: flex; gap: 20px; margin: 20px 0; flex-wrap: wrap;">
	<img src="./images/InstallOption.png" width="300" alt="インストールオプション">
	<img src="./images/InstallSymbol.png" width="300" alt="インストールシンボル">
</div>

## VisualStudioのデフォルトエンコードをutf8に設定
.uproectと同じフォルダに.editorconfigを保存
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  .editorconfig
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```ini
# Unreal Engine EditorConfig
# トップレベルのEditorConfigファイル
root = true

# すべてのファイルのデフォルト設定
[*]
charset = utf-8
end_of_line = crlf
insert_final_newline = true
trim_trailing_whitespace = true
tab_width = 4
indent_size = 4
dotnet_style_qualification_for_field = false:silent
dotnet_style_qualification_for_property = false:silent
dotnet_style_qualification_for_method = false:silent
dotnet_style_qualification_for_event = false:silent
dotnet_style_parentheses_in_arithmetic_binary_operators = always_for_clarity:silent
dotnet_style_parentheses_in_other_binary_operators = always_for_clarity:silent
dotnet_style_parentheses_in_relational_binary_operators = always_for_clarity:silent
dotnet_style_parentheses_in_other_operators = never_if_unnecessary:silent

# C++ファイル (.h, .cpp, .hpp, .c)
[*.{cpp,h,hpp,c,inl}]
charset = utf-8
indent_style = tab
indent_size = 4
tab_width = 4

# C#ファイル (Build.cs, Target.cs など)
[*.cs]
charset = utf-8
indent_style = tab
indent_size = 4
tab_width = 4

# Unreal ビルドスクリプト
[*.{Build.cs,Target.cs}]
charset = utf-8
indent_style = tab
indent_size = 4

# JSON、設定ファイル
[*.{json,uplugin,uproject}]
charset = utf-8
indent_style = tab
indent_size = 4

# INI設定ファイル
[*.ini]
charset = utf-8
indent_style = space
indent_size = 4

# XML、XAML
[*.{xml,config}]
charset = utf-8
indent_style = tab
indent_size = 2

# Markdown、テキストファイル
[*.{md,txt}]
charset = utf-8
trim_trailing_whitespace = true

# Yamlファイル
[*.{yml,yaml}]
charset = utf-8
indent_style = space
indent_size = 2

# Shaderファイル
[*.{usf,ush,hlsl}]
charset = utf-8
indent_style = tab
indent_size = 4
[*.cs]
#### 命名スタイル ####

# 名前付けルール

dotnet_naming_rule.interface_should_be_begins_with_i.severity = suggestion
dotnet_naming_rule.interface_should_be_begins_with_i.symbols = interface
dotnet_naming_rule.interface_should_be_begins_with_i.style = begins_with_i

dotnet_naming_rule.types_should_be_pascal_case.severity = suggestion
dotnet_naming_rule.types_should_be_pascal_case.symbols = types
dotnet_naming_rule.types_should_be_pascal_case.style = pascal_case

dotnet_naming_rule.non_field_members_should_be_pascal_case.severity = suggestion
dotnet_naming_rule.non_field_members_should_be_pascal_case.symbols = non_field_members
dotnet_naming_rule.non_field_members_should_be_pascal_case.style = pascal_case

# 記号の仕様

dotnet_naming_symbols.interface.applicable_kinds = interface
dotnet_naming_symbols.interface.applicable_accessibilities = public, internal, private, protected, protected_internal, private_protected
dotnet_naming_symbols.interface.required_modifiers = 

dotnet_naming_symbols.types.applicable_kinds = class, struct, interface, enum
dotnet_naming_symbols.types.applicable_accessibilities = public, internal, private, protected, protected_internal, private_protected
dotnet_naming_symbols.types.required_modifiers = 

dotnet_naming_symbols.non_field_members.applicable_kinds = property, event, method
dotnet_naming_symbols.non_field_members.applicable_accessibilities = public, internal, private, protected, protected_internal, private_protected
dotnet_naming_symbols.non_field_members.required_modifiers = 

# 命名スタイル

dotnet_naming_style.begins_with_i.required_prefix = I
dotnet_naming_style.begins_with_i.required_suffix = 
dotnet_naming_style.begins_with_i.word_separator = 
dotnet_naming_style.begins_with_i.capitalization = pascal_case

dotnet_naming_style.pascal_case.required_prefix = 
dotnet_naming_style.pascal_case.required_suffix = 
dotnet_naming_style.pascal_case.word_separator = 
dotnet_naming_style.pascal_case.capitalization = pascal_case

dotnet_naming_style.pascal_case.required_prefix = 
dotnet_naming_style.pascal_case.required_suffix = 
dotnet_naming_style.pascal_case.word_separator = 
dotnet_naming_style.pascal_case.capitalization = pascal_case
csharp_indent_labels = one_less_than_current
csharp_using_directive_placement = outside_namespace:silent
csharp_style_conditional_delegate_call = true:suggestion
csharp_style_var_for_built_in_types = false:silent
csharp_style_var_when_type_is_apparent = false:silent
csharp_style_var_elsewhere = false:silent
csharp_prefer_simple_using_statement = true:suggestion
csharp_prefer_braces = true:silent
csharp_style_namespace_declarations = block_scoped:silent

[*.vb]
#### 命名スタイル ####

# 名前付けルール

dotnet_naming_rule.interface_should_be_i_で始まる.severity = suggestion
dotnet_naming_rule.interface_should_be_i_で始まる.symbols = interface
dotnet_naming_rule.interface_should_be_i_で始まる.style = i_で始まる

dotnet_naming_rule.型_should_be_パスカル_ケース.severity = suggestion
dotnet_naming_rule.型_should_be_パスカル_ケース.symbols = 型
dotnet_naming_rule.型_should_be_パスカル_ケース.style = パスカル_ケース

dotnet_naming_rule.フィールド以外のメンバー_should_be_パスカル_ケース.severity = suggestion
dotnet_naming_rule.フィールド以外のメンバー_should_be_パスカル_ケース.symbols = フィールド以外のメンバー
dotnet_naming_rule.フィールド以外のメンバー_should_be_パスカル_ケース.style = パスカル_ケース

# 記号の仕様

dotnet_naming_symbols.interface.applicable_kinds = interface
dotnet_naming_symbols.interface.applicable_accessibilities = public, friend, private, protected, protected_friend, private_protected
dotnet_naming_symbols.interface.required_modifiers = 

dotnet_naming_symbols.型.applicable_kinds = class, struct, interface, enum
dotnet_naming_symbols.型.applicable_accessibilities = public, friend, private, protected, protected_friend, private_protected
dotnet_naming_symbols.型.required_modifiers = 

dotnet_naming_symbols.フィールド以外のメンバー.applicable_kinds = property, event, method
dotnet_naming_symbols.フィールド以外のメンバー.applicable_accessibilities = public, friend, private, protected, protected_friend, private_protected
dotnet_naming_symbols.フィールド以外のメンバー.required_modifiers = 

# 命名スタイル

dotnet_naming_style.i_で始まる.required_prefix = I
dotnet_naming_style.i_で始まる.required_suffix = 
dotnet_naming_style.i_で始まる.word_separator = 
dotnet_naming_style.i_で始まる.capitalization = pascal_case

dotnet_naming_style.パスカル_ケース.required_prefix = 
dotnet_naming_style.パスカル_ケース.required_suffix = 
dotnet_naming_style.パスカル_ケース.word_separator = 
dotnet_naming_style.パスカル_ケース.capitalization = pascal_case

dotnet_naming_style.パスカル_ケース.required_prefix = 
dotnet_naming_style.パスカル_ケース.required_suffix = 
dotnet_naming_style.パスカル_ケース.word_separator = 
dotnet_naming_style.パスカル_ケース.capitalization = pascal_case
```

</div>

## cpp,hファイルを同一フォルダ内でインクルード
Source/{ProjectName}/{ProjectName}.Build.csでインクルードパスにModuleDirectoryを追加
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyProject.Build.cs
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```csharp
// Copyright Epic Games, Inc. All Rights Reserved.

using UnrealBuildTool;

public class MyProject : ModuleRules
{
	public MyProject(ReadOnlyTargetRules Target) : base(Target)
	{
		PCHUsage = PCHUsageMode.UseExplicitOrSharedPCHs;
	
		PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore", "EnhancedInput" });

		PrivateDependencyModuleNames.AddRange(new string[] {  });

		// .hと.cppを同一フォルダに配置してインクルードを通す設定
		PublicIncludePaths.AddRange(new string[] { ModuleDirectory });
	}
}
```

</div>

## エディタ環境の設定
### ソースコードエディタの指定
「一般→ソースコード」でソースコードエディタを指定します。
通常はVisualStudioを指定。
### ライブコーディングの無効化
「一般→ライブコーディング」で「ライブコーディングの有効化」をオフ。
### 新規追加C++クラスの自動コンパイルオフ
「一般→未分類→ホットリロード」で「新たに追加されたC++クラスを自動的にコンパイル」をオフ。
### アセットエディタの起動場所を指定
「一般→アピアランス」で「アセットエディタの起動場所」を「Main Window」に設定。

## プロジェクト設定
### 著作権情報の設定
「プロジェクト→説明」で「著作権情報」にコピーライトを記載。
これによりUEでソースコードを作成した際の上部コメントに入る文字列を指定。
```cpp
// Copyright MyGames
```

## 基礎コードのセットアップ
### ログの設定
ログのカテゴリを新規に作成することができます。
自プロジェクト専用のログカテゴリーを作っておくと、フィルタリングで自分が出力したログだけを見ることができます。ログカテゴリはSource/{ProjectName}/直下にヘッダとcppファイルを用意して定義します。
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyLogChanne.h
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```ini
#pragma once

#include "CoreMinimal.h"
#include "Logging/LogMacros.h"

// LogMyのカテゴリー定義
DECLARE_LOG_CATEGORY_EXTERN(LogMy, Log, All);
```
</div>

<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyLogChannel.cpp
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```ini
#include "MyLogChannels.h"

// LogMyのカテゴリー定義
DEFINE_LOG_CATEGORY(LogMy);
```
</div>

### コリジョンチャンネルの設定
エディタ上ではオブジェクトチャンネル、コリジョンチャンネルはプロジェクト設定で名前を付けることができますがC++には反映されないため、ソースコードで独自に定義する必要があります。  
これはSource/{ProjectName}/{ProjectName}/直下に最初から存在する{ProjectName}.h,{ProjectName}.cppに定義を書く加えます。
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyProject.h
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```ini
// Copyright Epic Games, Inc. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"

// トレースチャンネル、オブジェクトチャンネルは両方ともECC_GameTraceChannel?を共有する(1～18)
// エディタ上の設定と割り当てられたGameTraceChannelの対応はDefaultConfig.iniを参照

//トレースチャンネルのユーザー定義
#define ECC_CharacterMesh		ECC_GameTraceChannel1
#define ECC_CharacterMeshBlock	ECC_GameTraceChannel2
#define ECC_Climbable			ECC_GameTraceChannel3
#define ECC_Vaultable			ECC_GameTraceChannel4
#define ECC_Water				ECC_GameTraceChannel5
#define ECC_UI					ECC_GameTraceChannel6
#define ECC_Fadeable			ECC_GameTraceChannel7
#define ECC_IKTrace				ECC_GameTraceChannel8
#define ECC_Interactable		ECC_GameTraceChannel9

// オブジェクトチャンネルのユーザー定義
#define ECC_Player				ECC_GameTraceChannel10
#define ECC_Enemy				ECC_GameTraceChannel11
#define ECC_NPCs				ECC_GameTraceChannel12
```
</div>

### ゲームプレイタグの設定
プロジェクトで扱うゲームプレイタグを作成します。  
ゲームプレイタグはプロジェクト設定のiniファイルで静的に作成できますが、ここではC++で扱いやすくするためにソースコード上で動的に定義します。  
ファイルはSource/{ProjectName}/直下にヘッダとcppフィルを用意して定義します。
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyGameplayTags.h
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
// Copyright HakumaiGames

#pragma once

#include "CoreMinimal.h"
#include "GameplayTagContainer.h"

/**
 * AuraGameplayTags
 *
 * Singleton containing native Gameplay Tags
 */

struct FAuraGameplayTags
{
public:
	static const FAuraGameplayTags& Get() { return GameplayTags; }
	static void InitializeNativeGameplayTags();

	// プライマリータグ(キャラクターの基本的な属性)
	FGameplayTag Attributes_Primary_Strength;
	FGameplayTag Attributes_Primary_Intelligence;
	FGameplayTag Attributes_Primary_Resilience;
	FGameplayTag Attributes_Primary_Vigor;

	// セカンダリータグ(プライマリータグから算出される)
	FGameplayTag Attributes_Secondary_Armor;
	FGameplayTag Attributes_Secondary_ArmorPenetration;
	FGameplayTag Attributes_Secondary_BlockChance;
	FGameplayTag Attributes_Secondary_CriticalHitChance;
	FGameplayTag Attributes_Secondary_CriticalHitDamage;
	FGameplayTag Attributes_Secondary_CriticalHitResistance;
	FGameplayTag Attributes_Secondary_HealthRegeneration;
	FGameplayTag Attributes_Secondary_ManaRegeneration;
	FGameplayTag Attributes_Secondary_MaxHealth;
	FGameplayTag Attributes_Secondary_MaxMana;

	// 受け取るXPタグ(獲得経験値の増減に使用)
	FGameplayTag Attributes_Meta_IncomingXP;

	// インプットタグ(入力の種別に利用)
	FGameplayTag InputTag_LMB;
	FGameplayTag InputTag_RMB;
	FGameplayTag InputTag_1;
	FGameplayTag InputTag_2;
	FGameplayTag InputTag_3;
	FGameplayTag InputTag_4;
	FGameplayTag InputTag_Passive_1;
	FGameplayTag InputTag_Passive_2;

	// ダメージタグ(ダメージ値を表すために使用)
	FGameplayTag Damage;		// 通常ダメージ
	FGameplayTag Damage_Fire;	// 火炎ダメージ
	FGameplayTag Damage_Lightning;	// 雷撃ダメージ
	FGameplayTag Damage_Arcane;		// 魔法ダメージ
	FGameplayTag Damage_Physical;	// 物理ダメージ

	// レジスタンスタグ(属性耐性を表す)
	FGameplayTag Attributes_Resistance_Fire;
	FGameplayTag Attributes_Resistance_Lightning;
	FGameplayTag Attributes_Resistance_Arcane;
	FGameplayTag Attributes_Resistance_Physical;

	// デバフタグ
	FGameplayTag Debuff_Burn;
	FGameplayTag Debuff_Stun;
	FGameplayTag Debuff_Arcane;
	FGameplayTag Debuff_Physical;

	FGameplayTag Debuff_Chance;
	FGameplayTag Debuff_Damage;
	FGameplayTag Debuff_Duration;
	FGameplayTag Debuff_Frequency;

	// アビリティタグ
	FGameplayTag Abilities_None;

	FGameplayTag Abilities_Attack;
	FGameplayTag Abilities_Summon;
	FGameplayTag Abilities_Fire_FireBolt;
	FGameplayTag Abilities_Fire_FireBlast;
	FGameplayTag Abilities_Lightning_Electrocute;
	FGameplayTag Abilities_Arcane_ArcaneShards;

	FGameplayTag Abilities_HitReact;

	// パッシブアビリティタグ
	FGameplayTag Abilities_Passive_HaloOfProtection;
	FGameplayTag Abilities_Passive_LifeSiphon;
	FGameplayTag Abilities_Passive_ManaSiphon;
	
	// プレイヤータグ(プレイヤーの状態管理に使用)
	FGameplayTag Player_Block_InputPressed;
	FGameplayTag Player_Block_InputHeld;
	FGameplayTag Player_Block_InputReleased;
	FGameplayTag Player_Block_CursorTrace;

	// ゲームプレイキュータグ
	FGameplayTag GameplayCue_FireBlast;

	// ステータスタグ(アビリティの状態管理に使用)
	FGameplayTag Abilities_Status_Locked;
	FGameplayTag Abilities_Status_Eligible;
	FGameplayTag Abilities_Status_Unlocked;
	FGameplayTag Abilities_Status_Equipped;

	// アビリティタイプタグ
	FGameplayTag Abilities_Type_Offensive;
	FGameplayTag Abilities_Type_Passive;
	FGameplayTag Abilities_Type_None;

	// クールダウンタグ(アビリティのクールダウン管理に使用)
	FGameplayTag Cooldown_Fire_FireBolt;

	// ソケットタグ(攻撃発生位置の分類に使用)
	FGameplayTag CombatSocket_Weapon;
	FGameplayTag CombatSocket_RightHand;
	FGameplayTag CombatSocket_LeftHand;
	FGameplayTag CombatSocket_Tail;

	// モンタージュタグ(アニメーションモンタージュの分類に使用)
	FGameplayTag Montage_Attack_1;
	FGameplayTag Montage_Attack_2;
	FGameplayTag Montage_Attack_3;
	FGameplayTag Montage_Attack_4;

	// ダメージタイプタグタグとレジスタンスタグのマッピング(ダメージタイプごとの耐性を取得するために使用)
	TMap<FGameplayTag, FGameplayTag> DamageTypesToResistances;
	// ダメージタイプタグとデバフタグのマッピング(ダメージタイプごとのデバフを取得するために使用)
	TMap<FGameplayTag, FGameplayTag> DamageTypesToDebuffs;

	// イベントタグ(アビリティの発動やエフェクトの適用などのイベントに使用)
	FGameplayTag Effects_HitReact;

private:
	static FAuraGameplayTags GameplayTags;
};
```
</div>
<br>
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyGameplayTags.cpp
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
// Copyright HakumaiGames


#include "MyGameplayTags.h"
#include "GameplayTagsManager.h"

FMyGameplayTags FMyGameplayTags::GameplayTags;

void FMyGameplayTags::InitializeNativeGameplayTags()
{
	/*
	 * Primary Attributes
	 */
	GameplayTags.Attributes_Primary_Strength = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Primary.Strength"),
		FString("Increases physical damage")
	);

	GameplayTags.Attributes_Primary_Intelligence = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Primary.Intelligence"),
		FString("Increases magical damage")
	);

	GameplayTags.Attributes_Primary_Resilience = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Primary.Resilience"),
		FString("Increases Armor and Armor Penetration")
	);

	GameplayTags.Attributes_Primary_Vigor = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Primary.Vigor"),
		FString("Increases Health")
	);

	/*
	 * Secondary Attributes
	 */

	GameplayTags.Attributes_Secondary_Armor = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Secondary.Armor"),
		FString("Reduces damage taken, improves Block Chance")
	);

	GameplayTags.Attributes_Secondary_ArmorPenetration = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Secondary.ArmorPenetration"),
		FString("Ignores Percentage of enemy Armor, increases Critical Hit Chance")
	);

	GameplayTags.Attributes_Secondary_BlockChance = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Secondary.BlockChance"),
		FString("Chance to cut incoming damage in half")
	);

	GameplayTags.Attributes_Secondary_CriticalHitChance = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Secondary.CriticalHitChance"),
		FString("Chance to double damage plus critical hit bonus")
	);

	GameplayTags.Attributes_Secondary_CriticalHitDamage = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Secondary.CriticalHitDamage"),
		FString("Bonus damage added when a critical hit is scored")
	);

	GameplayTags.Attributes_Secondary_CriticalHitResistance = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Secondary.CriticalHitResistance"),
		FString("Reduces Critical Hit Chance of attacking enemies")
	);

	GameplayTags.Attributes_Secondary_HealthRegeneration = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Secondary.HealthRegeneration"),
		FString("Amount of Health regenerated every 1 second")
	);

	GameplayTags.Attributes_Secondary_ManaRegeneration = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Secondary.ManaRegeneration"),
		FString("Amount of Mana regenerated every 1 second")
	);

	GameplayTags.Attributes_Secondary_MaxHealth = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Secondary.MaxHealth"),
		FString("Maximum amount of Health obtainable")
	);

	GameplayTags.Attributes_Secondary_MaxMana = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Secondary.MaxMana"),
		FString("Maximum amount of Mana obtainable")
	);

	/*
	 * Input Tags
	 */

	GameplayTags.InputTag_LMB = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("InputTag.LMB"),
		FString("Input Tag for Left Mouse Button")
	);

	GameplayTags.InputTag_RMB = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("InputTag.RMB"),
		FString("Input Tag for Right Mouse Button")
	);

	GameplayTags.InputTag_1 = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("InputTag.1"),
		FString("Input Tag for 1 key")
	);

	GameplayTags.InputTag_2 = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("InputTag.2"),
		FString("Input Tag for 2 key")
	);

	GameplayTags.InputTag_3 = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("InputTag.3"),
		FString("Input Tag for 3 key")
	);

	GameplayTags.InputTag_4 = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("InputTag.4"),
		FString("Input Tag for 4 key")
	);
	GameplayTags.InputTag_Passive_1 = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("InputTag.Passive.1"),
		FString("Input Tag Passive Ability 1")
	);

	GameplayTags.InputTag_Passive_2 = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("InputTag.Passive.2"),
		FString("Input Tag Passive Ability 2")
	);

	GameplayTags.Damage = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Damage"),
		FString("Damage")
	);

	/*
	 * Damage Types
	 */

	GameplayTags.Damage_Fire = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Damage.Fire"),
		FString("Fire Damage Type")
	);
	GameplayTags.Damage_Lightning = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Damage.Lightning"),
		FString("Lightning Damage Type")
	);
	GameplayTags.Damage_Arcane = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Damage.Arcane"),
		FString("Arcane Damage Type")
	);
	GameplayTags.Damage_Physical = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Damage.Physical"),
		FString("Physical Damage Type")
	);

	/*
	 * Resistances
	 */

	GameplayTags.Attributes_Resistance_Arcane = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Resistance.Arcane"),
		FString("Resistance to Arcane damage")
	);
	GameplayTags.Attributes_Resistance_Fire = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Resistance.Fire"),
		FString("Resistance to Fire damage")
	);
	GameplayTags.Attributes_Resistance_Lightning = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Resistance.Lightning"),
		FString("Resistance to Lightning damage")
	);
	GameplayTags.Attributes_Resistance_Physical = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Resistance.Physical"),
		FString("Resistance to Physical damage")
	);

	/*
	 * Debuffs
	 */

	GameplayTags.Debuff_Arcane = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Debuff.Arcane"),
		FString("Debuff for Arcane damage")
	);
	GameplayTags.Debuff_Burn = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Debuff.Burn"),
		FString("Debuff for Fire damage")
	);
	GameplayTags.Debuff_Physical = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Debuff.Physical"),
		FString("Debuff for Physical damage")
	);
	GameplayTags.Debuff_Stun = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Debuff.Stun"),
		FString("Debuff for Lightning damage")
	);

	GameplayTags.Debuff_Chance = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Debuff.Chance"),
		FString("Debuff Chance")
	);
	GameplayTags.Debuff_Damage = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Debuff.Damage"),
		FString("Debuff Damage")
	);
	GameplayTags.Debuff_Duration = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Debuff.Duration"),
		FString("Debuff Duration")
	);
	GameplayTags.Debuff_Frequency = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Debuff.Frequency"),
		FString("Debuff Frequency")
	);

	/*
	 * Meta Attributes
	 */

	GameplayTags.Attributes_Meta_IncomingXP = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Attributes.Meta.IncomingXP"),
		FString("Incoming XP Meta Attribute")
	);

	/*
	 * Map of Damage Types to Resistances
	 */
	GameplayTags.DamageTypesToResistances.Add(GameplayTags.Damage_Arcane, GameplayTags.Attributes_Resistance_Arcane);
	GameplayTags.DamageTypesToResistances.Add(GameplayTags.Damage_Lightning, GameplayTags.Attributes_Resistance_Lightning);
	GameplayTags.DamageTypesToResistances.Add(GameplayTags.Damage_Physical, GameplayTags.Attributes_Resistance_Physical);
	GameplayTags.DamageTypesToResistances.Add(GameplayTags.Damage_Fire, GameplayTags.Attributes_Resistance_Fire);

	/*
	 * Map of Damage Types to Debuffs
	 */
	GameplayTags.DamageTypesToDebuffs.Add(GameplayTags.Damage_Arcane, GameplayTags.Debuff_Arcane);
	GameplayTags.DamageTypesToDebuffs.Add(GameplayTags.Damage_Lightning, GameplayTags.Debuff_Stun);
	GameplayTags.DamageTypesToDebuffs.Add(GameplayTags.Damage_Physical, GameplayTags.Debuff_Physical);
	GameplayTags.DamageTypesToDebuffs.Add(GameplayTags.Damage_Fire, GameplayTags.Debuff_Burn);

	/*
	 * Effects
	 */

	GameplayTags.Effects_HitReact = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Effects.HitReact"),
		FString("Tag granted when Hit Reacting")
	);


	/*
	 * Abilities
	 */

	GameplayTags.Abilities_None = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.None"),
		FString("No Ability - like the nullptr for Ability Tags")
	);

	GameplayTags.Abilities_Attack = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Attack"),
		FString("Attack Ability Tag")
	); 
	GameplayTags.Abilities_Summon = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Summon"),
		FString("Summon Ability Tag")
	);

	/*
	 * Ofensive Spells
	 */

	GameplayTags.Abilities_Fire_FireBolt = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Fire.FireBolt"),
		FString("FireBolt Ability Tag")
	);

	GameplayTags.Abilities_Fire_FireBlast = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Fire.FireBlast"),
		FString("FireBlast Ability Tag")
	);

	GameplayTags.Abilities_HitReact = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.HitReact"),
		FString("Hit React Ability")
	);
	GameplayTags.Abilities_Lightning_Electrocute = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Lightning.Electrocute"),
		FString("Electrocute Ability Tag")
	);

	GameplayTags.Abilities_Arcane_ArcaneShards = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Arcane.ArcaneShards"),
		FString("Arcane Shards Ability Tag")
	);

	/*
	 * Passive Spells
	 */

	GameplayTags.Abilities_Passive_LifeSiphon = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Passive.LifeSiphon"),
		FString("Life Siphon")
	);
	GameplayTags.Abilities_Passive_ManaSiphon = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Passive.ManaSiphon"),
		FString("Mana Siphon")
	);
	GameplayTags.Abilities_Passive_HaloOfProtection = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Passive.HaloOfProtection"),
		FString("Halo Of Protection")
	);

	/*
	 * Ability Status
	 */
	GameplayTags.Abilities_Status_Eligible = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Status.Eligible"),
		FString("Eligible Status")
	);

	GameplayTags.Abilities_Status_Equipped = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Status.Equipped"),
		FString("Equipped Status")
	);

	GameplayTags.Abilities_Status_Locked = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Status.Locked"),
		FString("Locked Status")
	);

	GameplayTags.Abilities_Status_Unlocked = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Status.Unlocked"),
		FString("Unlocked Status")
	);


	/*
	 * Ability Types
	 */

	GameplayTags.Abilities_Type_None = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Type.None"),
		FString("Type None")
	);

	GameplayTags.Abilities_Type_Offensive = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Type.Offensive"),
		FString("Type Offensive")
	);

	GameplayTags.Abilities_Type_Passive = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Abilities.Type.Passive"),
		FString("Type Passive")
	);

	
	/*
	 * Cooldown
	 */

	GameplayTags.Cooldown_Fire_FireBolt = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Cooldown.Fire.FireBolt"),
		FString("FireBolt Cooldown Tag")
	);

	/*
	 * Combat Sockets
	 */

	GameplayTags.CombatSocket_Weapon = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("CombatSocket.Weapon"),
		FString("Weapon")
	);

	GameplayTags.CombatSocket_RightHand = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("CombatSocket.RightHand"),
		FString("Right Hand")
	);

	GameplayTags.CombatSocket_LeftHand = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("CombatSocket.LeftHand"),
		FString("Left Hand")
	);

	GameplayTags.CombatSocket_Tail = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("CombatSocket.Tail"),
		FString("Tail")
	);

	/*
	 * Montage Tags
	 */

	GameplayTags.Montage_Attack_1 = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Montage.Attack.1"),
		FString("Attack 1")
	);

	GameplayTags.Montage_Attack_2 = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Montage.Attack.2"),
		FString("Attack 2")
	);

	GameplayTags.Montage_Attack_3 = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Montage.Attack.3"),
		FString("Attack 3")
	);

	GameplayTags.Montage_Attack_4 = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Montage.Attack.4"),
		FString("Attack 4")
	);


	/*
	 * Player Tags
	 */

	GameplayTags.Player_Block_CursorTrace = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Player.Block.CursorTrace"),
		FString("Block tracing under the cursor")
	);

	GameplayTags.Player_Block_InputHeld = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Player.Block.InputHeld"),
		FString("Block Input Held callback for input")
	);

	GameplayTags.Player_Block_InputPressed = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Player.Block.InputPressed"),
		FString("Block Input Pressed callback for input")
	);

	GameplayTags.Player_Block_InputReleased = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("Player.Block.InputReleased"),
		FString("Block Input Released callback for input")
	);

	/*
	 * GameplayCues
	 */

	GameplayTags.GameplayCue_FireBlast = UGameplayTagsManager::Get().AddNativeGameplayTag(
		FName("GameplayCue.FireBlast"),
		FString("FireBlast GameplayCue Tag")
	);
}
```
</div>
<br>
