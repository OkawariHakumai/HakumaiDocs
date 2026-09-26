# ゲームプレイアビリティシステム

# 目次
- [ゲームプレイアビリティシステム](#ゲームプレイアビリティシステム)
- [目次](#目次)
- [アビリティシステムの概要](#アビリティシステムの概要)
	- [ゲームプレイタグ](#ゲームプレイタグ)
	- [アビリティシステムコンポーネント](#アビリティシステムコンポーネント)
	- [アトリビュートセット](#アトリビュートセット)
	- [ゲームプレイアビリティ](#ゲームプレイアビリティ)
	- [アビリティタスク](#アビリティタスク)
	- [ゲームプレイエフェクト](#ゲームプレイエフェクト)
	- [ゲームプレイキュー](#ゲームプレイキュー)
- [アビリティシステムの環境構築](#アビリティシステムの環境構築)
	- [プラグインの有効化](#プラグインの有効化)
	- [Gameplay Abilitiesプラグインを有効化](#gameplay-abilitiesプラグインを有効化)
	- [Build.csにモジュールを追加](#buildcsにモジュールを追加)
- [アビリティシステムのクラス構成](#アビリティシステムのクラス構成)
	- [プレイヤー](#プレイヤー)
	- [エネミー](#エネミー)
- [ゲームプレイタグ(ダイナミックタグ)の作成](#ゲームプレイタグダイナミックタグの作成)
	- [ソースコード](#ソースコード)
- [アビリティの作成](#アビリティの作成)
	- [ソースコード](#ソースコード-1)
- [アトリビュートセットの作成](#アトリビュートセットの作成)
	- [アトリビュートセットの実装](#アトリビュートセットの実装)
	- [ゲームプレイエフェクト適用前の処理](#ゲームプレイエフェクト適用前の処理)
	- [ゲームプレイエフェクト適用後の処理](#ゲームプレイエフェクト適用後の処理)
	- [アトリビュート変更前の処理](#アトリビュート変更前の処理)
	- [アトリビュート変更後の処理](#アトリビュート変更後の処理)
	- [アトリビュートセットのソースコード](#アトリビュートセットのソースコード)
- [アビリティシステムコンポーネントの作成](#アビリティシステムコンポーネントの作成)
	- [アビリティシステムのレプリケーション](#アビリティシステムのレプリケーション)
	- [アビリティの付与機能](#アビリティの付与機能)
		- [通常アビリティとは](#通常アビリティとは)
		- [パッシブアビリティとは](#パッシブアビリティとは)
		- [アビリティスペックとは](#アビリティスペックとは)
	- [アビリティごとの処理](#アビリティごとの処理)
	- [アセットタグからアビリティスペックを取得](#アセットタグからアビリティスペックを取得)
	- [アビリティスペックからアセットタグを取得](#アビリティスペックからアセットタグを取得)
	- [汎用イベントをアビリティへ通知](#汎用イベントをアビリティへ通知)
	- [アビリティのアクティベート](#アビリティのアクティベート)
	- [アビリティのレベル設定](#アビリティのレベル設定)
	- [ソースコード](#ソースコード-2)
- [プレイヤーステートの作成](#プレイヤーステートの作成)
	- [コンストラクタ](#コンストラクタ)
	- [レプリケーションの設定](#レプリケーションの設定)
	- [アビリティシステムコンポーネントの取得関数](#アビリティシステムコンポーネントの取得関数)
	- [ソースコード](#ソースコード-3)
- [キャラクターのベース作成](#キャラクターのベース作成)
	- [アビリティシステムコンポーネントとアトリビュートセット](#アビリティシステムコンポーネントとアトリビュートセット)
	- [初期付与するアビリティとエフェクト](#初期付与するアビリティとエフェクト)
	- [エフェクトの適用関数](#エフェクトの適用関数)
	- [アビリティシステムの初期化](#アビリティシステムの初期化)
	- [ソースコード](#ソースコード-4)
- [プレイヤーキャラクターの作成](#プレイヤーキャラクターの作成)
	- [アビリティシステムの初期化](#アビリティシステムの初期化-1)
	- [ソースコード](#ソースコード-5)
- [エネミーキャラクターの作成](#エネミーキャラクターの作成)
	- [ソースコード](#ソースコード-6)
- [プレイヤーコントローラーの作成](#プレイヤーコントローラーの作成)
	- [ゲームモードの作成](#ゲームモードの作成)
- [ゲームインスタンスの作成](#ゲームインスタンスの作成)
- [起動確認](#起動確認)
	- [プロジェクト設定](#プロジェクト設定)
	- [デバッグ表示](#デバッグ表示)
- [ゲームプレイエフェクト](#ゲームプレイエフェクト-1)
	- [ゲームプレイエフェクトの特徴](#ゲームプレイエフェクトの特徴)
		- [データのみ](#データのみ)
		- [Blueprintベースで作成する](#blueprintベースで作成する)
		- [モディファイアとエグゼキューションによるアトリビュートを変更](#モディファイアとエグゼキューションによるアトリビュートを変更)
		- [期間ポリシー](#期間ポリシー)
			- [Instant](#instant)
			- [Has Duration](#has-duration)
			- [Inifinit](#inifinit)
		- [スタッキング](#スタッキング)
		- [ゲームプレイタグの追加](#ゲームプレイタグの追加)
		- [アビリティの付与](#アビリティの付与)
	- [ゲームプレイエフェクトの適用](#ゲームプレイエフェクトの適用)
		- [ゲームプレイエフェクトスペック](#ゲームプレイエフェクトスペック)
		- [ゲームプレイエフェクトのワークフロー](#ゲームプレイエフェクトのワークフロー)
	- [モディファイアによるアトリビュート変更](#モディファイアによるアトリビュート変更)
		- [スケーラブルフロートによる変更](#スケーラブルフロートによる変更)
		- [スケーラブルフロートとカーブテーブルによる変更](#スケーラブルフロートとカーブテーブルによる変更)
		- [アトリビュートによる変更](#アトリビュートによる変更)
			- [計算の順序](#計算の順序)
			- [アトリビュート取得方法](#アトリビュート取得方法)
			- [使用するアトリビュートの値](#使用するアトリビュートの値)
			- [アトリビュートの有効期間](#アトリビュートの有効期間)
		- [呼び出し元の設定による変更](#呼び出し元の設定による変更)
		- [MMC(Modiier Magnitude Calculations)による変更](#mmcmodiier-magnitude-calculationsによる変更)
		- [ExecutionCalculationによる変更](#executioncalculationによる変更)
		- [モディファイアの計算順序](#モディファイアの計算順序)
		- [モディファイアの係数](#モディファイアの係数)
	- [ゲームプレイエフェクトの適用と削除](#ゲームプレイエフェクトの適用と削除)
	- [ゲームプレイエフェクトのスタッキング](#ゲームプレイエフェクトのスタッキング)
	- [アトリビュート変更の検知](#アトリビュート変更の検知)
	- [ゲームプレイエフェクト変更の検知](#ゲームプレイエフェクト変更の検知)
	- [ゲームプレイエフェクトのコンポーネント](#ゲームプレイエフェクトのコンポーネント)
- [ゲームプレイイベント](#ゲームプレイイベント)

# アビリティシステムの概要
アビリティシステムは以下のパーツから構成されています。  
これらが連携することによりアビリティシステムの機能を実現しています。
## ゲームプレイタグ
ゲームプレイアビリティシステム全般にわたって使用されるタグです。  
アビリティシステムコンポーネント、ゲームプレイエフェクト、ゲームプレイアトリビュートなどに付与することができ、ゲームプレイアビリティの起動、キャラクターの状態管理、イベント発生時の通知パラメータなど幅広く利用されます。  
## アビリティシステムコンポーネント
アビリティシステムの基本となるコンポーネントです。対象のアクターにアタッチしてアビリティシステム全般の制御を行います。
## アトリビュートセット
キャラクターが保持するSTR,INT,DEX,HP,MPなどのパラメータセットです。アビリティシステムコンポーネントと同様に対象のアクターにアタッチして保持、管理します。
## ゲームプレイアビリティ
ジャンプ、攻撃、防御などのアクションを記述するクラスです。アクターはアビリティシステムコンポーネントにアビリティを登録することでアクションが実行できるようになります。  
アビリティは１つのクラスとして独立したコードで書けるのでアクター本体の実装に依存せずに機能を実装できます。また、アタッチ、デタッチするだけでアクターへの能力付与、削除が簡単にできるのでメンテナンス性に優れます。
## アビリティタスク
ゲームプレイアビリティで使われるワーカースレッドのようなものです。ゲームプレイアビリティはアビリティ開始、終了、中断などタイミングでコールバックを受けるので、そこに必要なロジックを記述できますが、呪文や溜め攻撃のような開始～待機～発動といった時間経過を伴うものはゲームプレイアビリティy内でアビリティタスクを生成して処理を行います。
## ゲームプレイエフェクト
アトリビュートの値を変更させるものです。アトリビュートの値は通常、直接書き換えるものではなく、アビリティシステムコンポーネントにゲームプレイエフェクトを適用することにより変化させます。例えば敵からダメージを受けてHPを減少させる場合、HPを減らすゲームプレイエフェクトを作成してアビリティシステムコンポーネントに適用させます。また、ゲームプレイエフェクトは持続時間なども設定できるので一定期間アトリビュート値を変化させるバフ、デバフなどにも使用できます。
## ゲームプレイキュー
ゲームプレイエフェクトを適用したことにより発生するパーティクルやSEなどのエフェクトを定義します。ゲームプレイキューはレプリケートに対応しているので、対象のアクターにゲームプレイエフェクトを適用すると各クライアントでも自動的にエフェクトを表示してくれます。

# アビリティシステムの環境構築
アビリティシステムを使用するためには以下の環境設定が必要になります。

## プラグインの有効化
GameplayAbilityを使うためにはプラグインを以下の手順で有効化する必要があります。  

## Gameplay Abilitiesプラグインを有効化
エディタのプラグインで、Gameplay Abilitiesを有効化にします。  
エディタを再起動させます

## Build.csにモジュールを追加  
<div style="background-color: #333;">
  MyProject.Build.cs
</div>
<div style="max-height: 300px; overflow-y: auto;">

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

# アビリティシステムのクラス構成
アトリビュートを持ちアクションを行うのはアクターなので、アクターにアビリティシステムコンポーネントとアトリビュートセットを持たせるのが自然な形なのですが、その実装だとプレイヤーが操作しているキャラクターが死んでリスポーンする場合などに問題になります。  
そこでライフタイムが一代限りの敵などのキャラクターについてはアビリティシステムコンポーネントとアトリビュートを直接持たせ、プレイヤーキャラクターについてはプレイヤーステートでアビリティシステムコンポーネントとアトリビュートを保持し、キャラクター側にはそのポインタを持たせる設計にします。
## プレイヤー
- プレイヤーステート
  - アビリティシステムコンポーネント
  - アトリビュートセット
- プレイヤーキャラクター
  - アビリティシステムコンポーネントのポインタ(プレイヤーステート保持)
  - アトリビュートセットのポインタ(プレイヤーステート保持)
## エネミー
- キャラクター
  - アビリティシステムコンポーネント
  - アトリビュートセット
<br>

# ゲームプレイタグ(ダイナミックタグ)の作成
まずはゲームプレイアビリティの各所で使うことになるゲームプレイタグを作成します。  
ゲームプレイタグはプロジェクト設定のiniファイルで静的に作成できますが、ここではC++で扱いやすくするためにソースコード上で動的に定義します。  
ファイルはSource/{ProjectName}/直下にヘッダとcppフィルを用意して定義します。
## ソースコード
<div style="background-color: #333;">
  MyGameplayTags.h
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright HakumaiGames

#pragma once

#include "CoreMinimal.h"
#include "GameplayTagContainer.h"

/**
 * MyGameplayTags
 *
 * Singleton containing native Gameplay Tags
 */

struct FMyGameplayTags
{
public:
	static const FMyGameplayTags& Get() { return GameplayTags; }
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
	static FMyGameplayTags GameplayTags;
};
```
</div>
<br>
<div style="background-color: #333;">
  MyGameplayTags.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

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

# アビリティの作成
すべてのアビリティの基礎となるプロジェクト用のアビリティクラスを作成します。GameplayAbilityを派生させます。

## ソースコード
<div style="background-color: #333;">
  MyAbility.h
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "Abilities/GameplayAbility.h"
#include "MyAbility.generated.h"

/**
 * 
 */
UCLASS()
class ETA_API UMyAbility : public UGameplayAbility
{
	GENERATED_BODY()
	
public:
	// アビリティ付与時に設定するダイナミックタグ
	UPROPERTY(EditDefaultsOnly, Category = "Tags")
	FGameplayTag DynamicTag;
};
```
</div>
<br>
<div style="background-color: #333;">
  MyAbility.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Fill out your copyright notice in the Description page of Project Settings.


#include "AbilitySystem/Abilities/MyAbility.h"

```
</div>
<br>

# アトリビュートセットの作成
## アトリビュートセットの実装
アトリビュートセットでは以下の実装を行います。
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
- PostAttributeChangedのオーバーライド  
  ゲームプレイエフェクトの評価により各アトリビュートに対して「値が変わった」段階でこの関数が呼ばれます。  
  アトリビュート変更後の
  - 最終的なクランプ
  - メタアトリビュートによる計算と結果の反映
  - アトリビュート変更に付随する追加処理全般  
    ゲームプレイエフェクトの適用、UIやエフェクトの表示処理

  などに使います。

## ゲームプレイエフェクト適用前の処理
アトリビュートに対してゲームプレイエフェクトが適用された前にPreGameplayEffectExecuteという関数をオーバーライドして処理を挟むことができます。
```cpp
bool UMyAttributeSet::PreGameplayEffectExecute(struct FGameplayEffectModCallbackData &Data)
{
	return Super::PreGameplayEffectExecute(Data);
}
```

## ゲームプレイエフェクト適用後の処理
アトリビュートに対してゲームプレイエフェクトが適用された後にという関数をオーバーライドして処理を挟むことができます。
ここではエフェクトにより変更されたアトリビュートの値に応じてさまざまな処理を行います。  
最終的な値のクランプ、ダメージを受けたことによるノックバックや死亡、経験上昇によるレベルアップ処理などです。
```cpp
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
```

## アトリビュート変更前の処理
モディファイアによりアトリビュート変更が行われる直前にPreAttributeChangeという関数をオーバーライドして処理を挟むことができます。  
ここではセットしようとしている新しい値に対し補正、クランプをかけることが可能です(HealthとHealthMax、ManaとManaMaxなどのクランプなど)。  
ここでのクランプは「不正な値を遮断」する意味があります。
```cpp
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
```


## アトリビュート変更後の処理
モディファイアによりアトリビュート変更が行われた後にPostAttributeChangeという関数をオーバーライドして処理を挟むことができます。  
ここでは計算後に最終的にセットしようとしている新しい値を見て、それに対して何か別の値の変更を行いたい場合に便利です。  
例えばレベルアップ時にヘルス、マナ全回復フラグをセットしておき、最大ヘルス・最大マナの変化と一緒に全回復させたい場合などに、ここに処理を挟みます。  
```cpp
void UMyAttributeSet::PostAttributeChange(const FGameplayAttribute& Attribute, float OldValue, float NewValue)
{
	Super::PostAttributeChange(Attribute, OldValue, NewValue);

	// 体力全回復フラグの処理
	if (Attribute == GetMaxHealthAttribute() && bTopOffHealth)
	{
		SetHealth(GetMaxHealth());
		bTopOffHealth = false;
	}
	// マナ全回復フラグの処理
	if (Attribute == GetMaxManaAttribute() && bTopOffMana)
	{
		SetMana(GetMaxMana());
		bTopOffMana = false;
	}
}
```

## アトリビュートセットのソースコード
<div style="background-color: #333;">
MyAttributeSet.h
</div>
<div style="max-height: 300px; overflow-y: auto;">

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
	virtual bool PreGameplayEffectExecute(struct FGameplayEffectModCallbackData& Data) { return true; }

	// GameplayEffect適用後の処理
	virtual void PostGameplayEffectExecute(const FGameplayEffectModCallbackData& Data) override;

	// アトリビュート変更前の処理
	virtual void PreAttributeChange(const FGameplayAttribute& Attribute, float& NewValue) override;

	// アトリビュート変更後の処理
	virtual void PostAttributeChange(const FGameplayAttribute& Attribute, float OldValue, float NewValue) override;

private:
	// ヘルスとマナを全回復するかどうかのフラグ
	bool bTopOffHealth = false;
	bool bTopOffMana = false;
};
```
</div>
<br>

<div style="background-color: #333;">
  MyAttributeSet.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
//  Copyright MyGameCompany. All Rights Reserved.

#include "AbilitySystem/MyAttributeSet.h"
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

# アビリティシステムコンポーネントの作成
アビリティシステムの基幹となるアビリティシステムコンポーネントを作成します。  
AbilitySystemComponentを派生させてプロジェクト用のアビリティシステムコンポーネントを作成し、以下の機能を持たせます。

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

## アビリティの付与機能
通常アビリティとパッシブアビリティをアビリティシステムに付与します。  
必要に応じてキャラクタークラスから呼び出されることを想定しています。
```cpp
// ASCにアビリティ付与
void AddCharacterAbilities(const TArray<TSubclassOf<UGameplayAbility>>& Abilities, int32 Level = 1);
// ASCにパッシブアビリティ付与
void AddCharacterPassiveAbilities(const TArray<TSubclassOf<UGameplayAbility>>& PassiveAbilities, int32 Level = 1);
```
### 通常アビリティとは
通常アビリティは必要に応じて都度アクティベート処理が実行されます。
### パッシブアビリティとは
パッシブアビリティは、付与と同時にアクティベートが行われるアビリティになります。
### アビリティスペックとは
アビリティスペックはコンポーネントにアビリティが付与されたときに内部で生成・保持される「付与済みアビリティのレコード」です。付与されたアビリティに対する一種のハンドルのようなものです。  
アビリティのクラス、レベル、入力ID（ホットキー割当て）、一意のハンドルなどを保持していて、このスペックを使ってアビリティを起動/停止したり、参照・削除することができます。

## アビリティごとの処理
所有している各アビリティごとにコールバックを呼び出します。
```cpp
// アビリティごとに処理を行う
void ForEachAbility(const FForEachAbility& Delegate);
```
## アセットタグからアビリティスペックを取得
アビリティにはアセットタグを設定することができます。  
この関数では指定したアセットタグを持つアビリティのスペックを取得します。  
```cpp
// アセットタグからアビリティスペックを取得する
FGameplayAbilitySpec* GetSpecFromAssetTag(const FGameplayTag& AssetTag);
```
## アビリティスペックからアセットタグを取得
アビリティスペックから、対象のアビリティのタグを取得することができます。  
アセットタグとダイナミックタグの2種類のタグを取得する関数があります。
```cpp
// AbilitySpecから指定した名前を持つのアセットタグ取得する
FGameplayTag GetAssetTagByNameFromSpec(const FGameplayAbilitySpec& AbilitySpec, const FName& TagName) const;

// AbilitySpecから指定した名前を持つのアセットタグ取得する
FGameplayTag GetDynamicTagByNameFromSpec(const FGameplayAbilitySpec& AbilitySpec, const FName& TagName) const;
```
## 汎用イベントをアビリティへ通知
アビリティシステムはアビリティに対して汎用イベントを送ることができます。  
アビリティ側ではWaitInputPress / WaitInputRelease / WaitGenericEventなどのイベント待ち受け処理によりイベントの発生を受け取って処理を実行することができます。  
送り先としてアビリティのアセットタグ、ダイナミックを指定する２関数があります。
```cpp
// 汎用レプリケートイベントをアビリティへ通知する
// 主にWaitInputPress / WaitInputRelease / WaitGenericEvent 等で待機しているアビリティへ入力や汎用イベントを伝える用
void InvokeReplicatedEventByTag(const FGameplayTag& AssetTag, EAbilityGenericReplicatedEvent::Type EventType);
void InvokeReplicatedEventByDynamicTag(const FGameplayTag& DynamicTag, EAbilityGenericReplicatedEvent::Type EventType);
```

## アビリティのアクティベート
付与されたアビリティをアクティベートします。アセットタグによる起動、ダイナミックタグによる起動の2種類があります。
```cpp
// アビリティをタグでアクティブ化する
UFUNCTION(BlueprintCallable, Category = "Abilities")
bool TryActivateAbilityByAssetTag(const FGameplayTag& AssetTag, bool bAllowRemoteActivation = true);
UFUNCTION(BlueprintCallable, Category = "Abilities")
bool TryActivateAbilityByDynamicTag(const FGameplayTag& DynamicTag, bool bAllowRemoteActivation = true);
```

## アビリティのレベル設定
アビリティにはレベルが存在します。  
アビリティシステムがアトリビュートに変化を加えようとしたときに、エフェクトスペックを生成して対象のアビリティシステムに適用するのですが、このエフェクトペックにアビリティのレベルを指定すると、エフェクトの効果量がレベルに応じたものになります。  
また、アビリティ側の内部実装で、アビリティスペックのレベルを参照してダメージ、クールダウンタイム、射撃数などの挙動をスケールさせることもできます。
```cpp
// アビリティのレベルを更新（クライアントはサーバーに要求する）
UFUNCTION(BlueprintCallable, Category = "Abilities")
void SetAbilityLevel(TSubclassOf<UGameplayAbility> AbilityClass, int32 NewLevel);

```

## ソースコード
<div style="background-color: #333;">
  MyAbilitySystemComponent.h
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "AbilitySystemComponent.h"
#include "GameplayTagContainer.h"
#include "MyAbilitySystemComponent.generated.h"

// 前方宣言
struct FGameplayAbilitySpec;

// アビリティが付与されたときにブロードキャストするためのデリゲートの型を定義
DECLARE_MULTICAST_DELEGATE(FAbilitiesGiven);
// アビリティごとに処理を行うためのデリゲートの型を定義
DECLARE_DELEGATE_OneParam(FForEachAbility, const FGameplayAbilitySpec&);

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

	// アビリティごとに処理を行う
	void ForEachAbility(const FForEachAbility& Delegate);

	// アセットタグからアビリティスペックを取得する
	FGameplayAbilitySpec* GetSpecFromAssetTag(const FGameplayTag& AssetTag);

	// AbilitySpecから指定した名前を持つのアセットタグ取得する
	FGameplayTag GetAssetTagByNameFromSpec(const FGameplayAbilitySpec& AbilitySpec, const FName& TagName) const;

	// AbilitySpecから指定した名前を持つのアセットタグ取得する
	FGameplayTag GetDynamicTagByNameFromSpec(const FGameplayAbilitySpec& AbilitySpec, const FName& TagName) const;

	// 汎用レプリケートイベントをアビリティへ通知する
	// 主にWaitInputPress / WaitInputRelease / WaitGenericEvent 等で待機しているアビリティへ入力や汎用イベントを伝える用
	void InvokeReplicatedEventByTag(const FGameplayTag& AssetTag, EAbilityGenericReplicatedEvent::Type EventType);
	void InvokeReplicatedEventByDynamicTag(const FGameplayTag& DynamicTag, EAbilityGenericReplicatedEvent::Type EventType);

	// アビリティをタグでアクティブ化する
	UFUNCTION(BlueprintCallable, Category = "Abilities")
	bool TryActivateAbilityByAssetTag(const FGameplayTag& AssetTag, bool bAllowRemoteActivation = true);
	UFUNCTION(BlueprintCallable, Category = "Abilities")
	bool TryActivateAbilityByDynamicTag(const FGameplayTag& DynamicTag, bool bAllowRemoteActivation = true);

	// アビリティのレベルを更新（クライアントはサーバーに要求する）
	UFUNCTION(BlueprintCallable, Category = "Abilities")
	void SetAbilityLevel(TSubclassOf<UGameplayAbility> AbilityClass, int32 NewLevel);

	// アビリティが付与されたときにブロードキャストするデリゲート
	FAbilitiesGiven AbilitiesGivenDelegate;

protected:
	// 実行可能アビリティの配列がレプリケートされたときに呼び出される関数をオーバーライド
	virtual void OnRep_ActivateAbilities() override;

private:
	// サーバーRPC：クライアントからの要求をサーバーで処理する
	UFUNCTION(Server, Reliable)
	void ServerSetAbilityLevel(TSubclassOf<UGameplayAbility> AbilityClass, int32 NewLevel);

	// サーバー上で実際にレベルを変更する内部実装
	void SetAbilityLevelInternal(TSubclassOf<UGameplayAbility> AbilityClass, int32 NewLevel);

	// アビリティ付与済みフラグ
	bool bStartupAbilitiesGiven = false;
};
```
</div>
<br>
<div style="background-color: #333;">
  MyAbilitySystemComponent.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.


#include "AbilitySystem/MyAbilitySystemComponent.h"
#include "Abilities/MyAbility.h"

// ASCにアビリティ付与
void UMyAbilitySystemComponent::AddCharacterAbilities(const TArray<TSubclassOf<UGameplayAbility>>& Abilities, int32 Level)
{
	for (TSubclassOf<UGameplayAbility> AbilityClass : Abilities)
	{
		FGameplayAbilitySpec AbilitySpec = FGameplayAbilitySpec(AbilityClass, Level);
		if (const UMyAbility* MyAbility = Cast<UMyAbility>(AbilitySpec.Ability))
		{
			// DynamicAbilityTagsにアクション入力タグを設定
			AbilitySpec.GetDynamicSpecSourceTags().AddTag(MyAbility->DynamicTag);
			GiveAbility(AbilitySpec);
		}
	}
	// アビリティ付与完了後にフラグを立ててデリゲートをブロードキャスト
	bStartupAbilitiesGiven = true;
	AbilitiesGivenDelegate.Broadcast();
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

void UMyAbilitySystemComponent::ForEachAbility(const FForEachAbility& Delegate)
{
	FScopedAbilityListLock ActiveScopeLock(*this);
	for (const FGameplayAbilitySpec& AbilitySpec : GetActivatableAbilities())
	{
		if (!Delegate.ExecuteIfBound(AbilitySpec))
		{
			UE_LOG(LogTemp, Error, TEXT("Failed to execute delegate in %hs"), __FUNCTION__);
		}
	}
}

FGameplayAbilitySpec* UMyAbilitySystemComponent::GetSpecFromAssetTag(const FGameplayTag& AssetTag)
{
	FScopedAbilityListLock ActiveScopeLoc(*this);
	for (FGameplayAbilitySpec& AbilitySpec : GetActivatableAbilities())
	{
		for (FGameplayTag Tag : AbilitySpec.Ability.Get()->GetAssetTags())
		{
			if (Tag.MatchesTag(AssetTag))
			{
				return &AbilitySpec;
			}
		}
	}
	return nullptr;
}

FGameplayTag UMyAbilitySystemComponent::GetAssetTagByNameFromSpec(const FGameplayAbilitySpec& AbilitySpec, const FName& TagName) const
{
	for (FGameplayTag Tag : AbilitySpec.Ability.Get()->GetAssetTags())
	{
		if (Tag.MatchesTag(FGameplayTag::RequestGameplayTag(TagName)))
		{
			return Tag;
		}
	}
	return FGameplayTag();
}

FGameplayTag UMyAbilitySystemComponent::GetDynamicTagByNameFromSpec(const FGameplayAbilitySpec& AbilitySpec, const FName& TagName) const
{
	for (FGameplayTag Tag : AbilitySpec.GetDynamicSpecSourceTags())
	{
		if (Tag.MatchesTag(FGameplayTag::RequestGameplayTag(TagName)))
		{
			return Tag;
		}
	}
	return FGameplayTag();
}

void UMyAbilitySystemComponent::InvokeReplicatedEventByTag(const FGameplayTag& AssetTag, EAbilityGenericReplicatedEvent::Type EventType)
{
	if (!AssetTag.IsValid()) return;

	FScopedAbilityListLock ActiveScopeLoc(*this);
	for (FGameplayAbilitySpec& AbilitySpec : GetActivatableAbilities())
	{
		if (AbilitySpec.Ability.Get()->GetAssetTags().HasTagExact(AssetTag))
		{
			// アクティベート可能なアビリティで、指定のインプットタグを持つアビリティに対して処理を行う
			AbilitySpecInputPressed(AbilitySpec);
			// WaitInputPress / WaitInputRelease / WaitGenericEvent で処理を受け取れるようにInvokeReplicatedEventを呼び出す
			if (AbilitySpec.IsActive())
			{
				TArray<UGameplayAbility*> AbilityInstances = AbilitySpec.GetAbilityInstances();
				for (UGameplayAbility* AbilityInstance : AbilityInstances)
				{
					InvokeReplicatedEvent(EventType, AbilitySpec.Handle, AbilityInstance->GetCurrentActivationInfo().GetActivationPredictionKey());
				}
			}
		}
	}
}

void UMyAbilitySystemComponent::InvokeReplicatedEventByDynamicTag(const FGameplayTag& DynamicTag, EAbilityGenericReplicatedEvent::Type EventType)
{
	if (!DynamicTag.IsValid()) return;

	FScopedAbilityListLock ActiveScopeLoc(*this);
	for (FGameplayAbilitySpec& AbilitySpec : GetActivatableAbilities())
	{
		if (AbilitySpec.GetDynamicSpecSourceTags().HasTagExact(DynamicTag))
		{
			// アクティベート可能なアビリティで、指定のインプットタグを持つアビリティに対して処理を行う
			AbilitySpecInputPressed(AbilitySpec);
			// WaitInputPress / WaitInputRelease / WaitGenericEvent で処理を受け取れるようにInvokeReplicatedEventを呼び出す
			if (AbilitySpec.IsActive())
			{
				TArray<UGameplayAbility*> AbilityInstances = AbilitySpec.GetAbilityInstances();
				for (UGameplayAbility* AbilityInstance : AbilityInstances)
				{
					InvokeReplicatedEvent(EventType, AbilitySpec.Handle, AbilityInstance->GetCurrentActivationInfo().GetActivationPredictionKey());
				}
			}
		}
	}
}

bool UMyAbilitySystemComponent::TryActivateAbilityByAssetTag(const FGameplayTag& AssetTag, bool bAllowRemoteActivation)
{
	if (!AssetTag.IsValid()) return false;

	bool bActivatedAny = false;
	FScopedAbilityListLock ActiveScopeLoc(*this);
	for (FGameplayAbilitySpec& AbilitySpec : GetActivatableAbilities())
	{
		if (AbilitySpec.Ability.Get()->GetAssetTags().HasTagExact(AssetTag))
		{
			AbilitySpecInputPressed(AbilitySpec);
			if (!AbilitySpec.IsActive())
			{
				if (TryActivateAbility(AbilitySpec.Handle, bAllowRemoteActivation))
				{
					bActivatedAny = true;
				}
			}
		}
	}
	return bActivatedAny;
}

bool UMyAbilitySystemComponent::TryActivateAbilityByDynamicTag(const FGameplayTag& DynamicTag, bool bAllowRemoteActivation)
{
	if (!DynamicTag.IsValid()) return false;

	bool bActivatedAny = false;
	FScopedAbilityListLock ActiveScopeLoc(*this);
	for (FGameplayAbilitySpec& AbilitySpec : GetActivatableAbilities())
	{
		if (AbilitySpec.GetDynamicSpecSourceTags().HasTagExact(DynamicTag))
		{
			AbilitySpecInputPressed(AbilitySpec);
			if (!AbilitySpec.IsActive())
			{
				if (TryActivateAbility(AbilitySpec.Handle, bAllowRemoteActivation))
				{
					bActivatedAny = true;
				}
			}
		}
	}
	return bActivatedAny;
}

void UMyAbilitySystemComponent::SetAbilityLevel(TSubclassOf<UGameplayAbility> AbilityClass, int32 NewLevel)
{
	// クライアントから呼ばれた場合はサーバーRPCで要求する
	if (!GetOwner())
	{
		return;
	}

	if (!GetOwner()->HasAuthority())
	{
		ServerSetAbilityLevel(AbilityClass, NewLevel);
		return;
	}

	// サーバー上なら直接実行
	SetAbilityLevelInternal(AbilityClass, NewLevel);
}

void UMyAbilitySystemComponent::ServerSetAbilityLevel_Implementation(TSubclassOf<UGameplayAbility> AbilityClass, int32 NewLevel)
{
	// サーバー実行
	SetAbilityLevelInternal(AbilityClass, NewLevel);
}

void UMyAbilitySystemComponent::SetAbilityLevelInternal(TSubclassOf<UGameplayAbility> AbilityClass, int32 NewLevel)
{
	if (!AbilityClass) return;

	// 指定クラスに一致するすべてのスペックのレベルを更新
	for (FGameplayAbilitySpec& Spec : GetActivatableAbilities())
	{
		if (Spec.Ability && Spec.Ability->GetClass() == AbilityClass)
		{
			Spec.Level = NewLevel;
			// 必要ならここで AbilityInstance の再初期化や通知を行う
		}
	}

	// 変更をクライアントに反映させるためにオーナーのネットワーク更新を促す
	if (GetOwner())
	{
		GetOwner()->ForceNetUpdate();
	}
}

// 実行可能アビリティの配列がレプリケートされたときに呼び出される関数をオーバーライド
void UMyAbilitySystemComponent::OnRep_ActivateAbilities()
{
	Super::OnRep_ActivateAbilities();

	// AbilitiesGivenDelegateはクライアントでは呼び出されないため(AddCharacterAbilitiesはサーバーでのみ呼び出される)
	// クライアントではActivatableAbilitiesがレプリケートされたときにアビリティ付与のデリゲートをブロードキャストする
	if (!bStartupAbilitiesGiven)
	{
		bStartupAbilitiesGiven = true;
		AbilitiesGivenDelegate.Broadcast();
	}
}
```
</div>
<br>


# プレイヤーステートの作成
プロジェクト用のプレイヤーステータクラスを作成します。  
[アビリティシステムのクラス構成](#アビリティシステムのクラス構成)で紹介した通り、プレイヤーステートはプレイヤーキャラクター用のアビリティシステムコンポーネントとアトリビュートを保持し、IAbilitySystemInterfaceを継承します。
## コンストラクタ
コンストラクタではアビリティシステムコンポーネントとアトリビュートの生成を行い、ネットワーク対応のためレプリケートを有効にします。プレイヤーステートは比較的頻度高く同期が必要なのでNetUpdateFrequencyを100に指定します。  
NetUpdateFrequencyはレプリケートする秒間の頻度で、100を指定すると1秒間に100回(0.01秒間隔)で更新を試みる設定になります。
## レプリケーションの設定
GetLifetimeReplicatedProps関数でどのプロパティをレプリケートするかの設定を登録します。  
レプリケートしたい変数を追加する場合は、ここにDOREPLIFETIMEマクロで変数を登録します。
## アビリティシステムコンポーネントの取得関数
IAbilitySystemInterfaceに定義されているGetAbilitySystemComponent関数をオーバーライドします。  
シンプルに自身が保持しているコンポーン年とを返すだけです。

## ソースコード
<div style="background-color: #333;">
  MyPlayerState.h
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "AbilitySystemInterface.h"
#include "GameFramework/PlayerState.h"
#include "MyPlayerState.generated.h"

// 前方宣言
class UAbilitySystemComponent;
class UAttributeSet;

/**
 * 
 */
UCLASS()
class ETA_API AMyPlayerState : public APlayerState, public IAbilitySystemInterface
{
	GENERATED_BODY()
	
public:
	AMyPlayerState();

	// レプリケーションの設定
	virtual void GetLifetimeReplicatedProps(TArray<FLifetimeProperty>& OutLifetimeProps) const override;

	// AbilitySystemInterfaceの実装
	virtual UAbilitySystemComponent* GetAbilitySystemComponent() const override;

	// AttributeSetを取得する関数
	UAttributeSet* GetAttributeSet() const { return AttributeSet; }

protected:
	// BPからアクセスできるようにVisibleAnywherを追加
	UPROPERTY(VisibleAnywhere)
	TObjectPtr<UAbilitySystemComponent> AbilitySystemComponent;

	UPROPERTY()
	TObjectPtr<UAttributeSet> AttributeSet;
};
```
</div>
<br>
<div style="background-color: #333;">
  MyPlayerState.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.


#include "MyPlayerState.h"
#include "AbilitySystem/MyAbilitySystemComponent.h"
#include "AbilitySystem/MyAttributeSet.h"


AMyPlayerState::AMyPlayerState()
{
	// アビリティシステムコンポーネント作成
	AbilitySystemComponent = CreateDefaultSubobject<UMyAbilitySystemComponent>("AbilitySystemComponent");
	AbilitySystemComponent->SetIsReplicated(true);
	AbilitySystemComponent->SetReplicationMode(EGameplayEffectReplicationMode::Mixed);

	// アトリビュートセット作成
	AttributeSet = CreateDefaultSubobject<UMyAttributeSet>("AttributeSet");

	// ネットワーク更新頻度を設定
	SetNetUpdateFrequency(100.f);
}

// レプリケーションの設定
void AMyPlayerState::GetLifetimeReplicatedProps(TArray<FLifetimeProperty>& OutLifetimeProps) const
{
	Super::GetLifetimeReplicatedProps(OutLifetimeProps);

	// レプリケーションする設定
	// DOREPLIFETIME(AMyPlayerState, MemberVariable);
}

UAbilitySystemComponent* AMyPlayerState::GetAbilitySystemComponent() const
{
	return AbilitySystemComponent;
}
```
</div>
<br>

# キャラクターのベース作成
ベースキャラクターにはキャラクターとしてふるまうために必要な機能を実装します。  
## アビリティシステムコンポーネントとアトリビュートセット
アビリティシステムコンポーネントとアトリビュートセットは、キャラクターとしては必須なものですがプレイヤーとエネミーで所有者や初期化タイミングが変わるため、ベース側ではポインタのみ保持する形にします。
```cpp
UPROPERTY()
TObjectPtr<UAbilitySystemComponent> AbilitySystemComponent;

UPROPERTY()
TObjectPtr<UAttributeSet> AttributeSet;
```
## 初期付与するアビリティとエフェクト
キャラクターが最初から所持しているアビリティとエフェクトクラスの配列をメンバ変数に所持します。
これらの変数は、キャラクタークラスをBPに派生させて設定してもOKですし、SetupDefaultAbilitiesAndEffectsをオーバーライドして設定しても動くようにしておきます。
```cpp
class ETA_API AMyCharacter : public ACharacter, public IAbilitySystemInterface
{
protected:
	// 初期付与するアビリティとエフェクトを設定する関数
	virtual void SetupDefaultAbilitiesAndEffects();

private:
	// キャラクターに初期付与するアビリティ
	UPROPERTY(EditAnywhere, Category = "Abilities")
	TArray<TSubclassOf<UGameplayAbility>> DefaultAbilities;

	// キャラクターに初期付与するパッシブアビリティ
	UPROPERTY(EditAnywhere, Category = "Abilities")
	TArray<TSubclassOf<UGameplayAbility>> DefaultPassiveAbilities;

	// キャラクターに初期付与するゲームプレイエフェクト
	UPROPERTY(EditAnywhere, Category = "Effects")
	TArray<TSubclassOf<UGameplayEffect>> DefaultEffects;
}

void AMyCharacter::SetupDefaultAbilitiesAndEffects()
{
	// DefaultAbilities.Add(MyAbility::StaticClass());

	// DefaultPassiveAbilities.Add(MyPassiveAbility::StaticClass());

	// DefaultEffects.Add(MyGameplayEffect::StaticClass());
}
```
## エフェクトの適用関数
エフェクトを自分自身に適用する関数です。  
以下の3ステップが行われます。
- アビリティシステムコンポーネントからコンテキストハンドルを作成
- エフェクトクラス、レベル、コンテキストハンドルからエフェクトスペックハンドルを作成
- エフェクトスペックハンドルを使って自分自身にエフェクトを適用
```cpp
class ETA_API AMyCharacter : public ACharacter, public IAbilitySystemInterface
{
protected:
	// ゲームプレイエフェクトを自身に適用
	void ApplyEffectToSelf(TSubclassOf<UGameplayEffect> GameplayEffectClass, float Level) const;
}

void AMyCharacter::ApplyEffectToSelf(TSubclassOf<UGameplayEffect> GameplayEffectClass, float Level) const
{
	check(IsValid(GetAbilitySystemComponent()));
	check(GameplayEffectClass);

	// エフェクトは以下の3つのステップで適用される

	// ASCからコンテキストハンドルを作成
	FGameplayEffectContextHandle ContextHandle = GetAbilitySystemComponent()->MakeEffectContext();
	ContextHandle.AddSourceObject(this);	// エフェクトのソースオブジェクトは自分自身

	// クラス、レベル、コンテキストハンドルからスペックハンドルを作成
	const FGameplayEffectSpecHandle SpecHandle = GetAbilitySystemComponent()->MakeOutgoingSpec(GameplayEffectClass, Level, ContextHandle);

	// スペックハンドルを使ってASCに適用
	GetAbilitySystemComponent()->ApplyGameplayEffectSpecToTarget(*SpecHandle.Data.Get(), GetAbilitySystemComponent());
}
```
## アビリティシステムの初期化
アビリティシステムの初期化関数です。  
アビリティシステムはアビリティシステムコンポーネント、アトリビュートセット、使用者すべてが揃わないと初期化できないため、ベースクラスではそれらを引数にした関数(InitializeAbilitySystem)のみ用意し、派生クラスから適切なタイミングで呼び出します。  
```cpp
class ETA_API AMyCharacter : public ACharacter, public IAbilitySystemInterface
{
protected:
	// エフェクト初期化
	virtual void InitializeDefaultEffects() const;

	// アビリティ初期化
	void InitializeDefaultAbilities();

	// アビリティシステムの初期化(ASCのOwner,Avatorが生成されたときに呼び出す)を行う
	virtual void InitializeAbilitySystem(AActor* InOwnerActor, AActor* InAvatarActor);
};

void AMyCharacter::InitializeDefaultEffects() const
{
	for(auto & EffectClass : DefaultEffects)
	{
		ApplyEffectToSelf(EffectClass, 1.f);
	}
}

void AMyCharacter::InitializeDefaultAbilities()
{
	// デフォルトアビリティをASCに付与
	UMyAbilitySystemComponent* MyASC = CastChecked<UMyAbilitySystemComponent>(AbilitySystemComponent);
	if (!HasAuthority()) return;

	MyASC->AddCharacterAbilities(DefaultAbilities);
	MyASC->AddCharacterPassiveAbilities(DefaultPassiveAbilities);
}

void AMyCharacter::InitializeAbilitySystem(AActor* InOwnerActor, AActor* InAvatarActor)
{
	check(InOwnerActor);
	check(InAvatarActor);

	// InOwnerActorがAMyPlayerStateの場合は、AMyPlayerStateからASCとAttributeSetを取得
	if (AMyPlayerState* MyPlayerState = Cast<AMyPlayerState>(InOwnerActor))
	{
		// PlayerStateからASCとAttributeSetを取得
		if (!AbilitySystemComponent)
		{
			AbilitySystemComponent = MyPlayerState->GetAbilitySystemComponent();
		}
		if (!AttributeSet)
		{
			AttributeSet = MyPlayerState->GetAttributeSet();
		}
	}
	check(AbilitySystemComponent);
	check(AttributeSet);

	// ASCのOwnerとAvatarを設定
	AbilitySystemComponent->InitAbilityActorInfo(InOwnerActor, InAvatarActor);

	// キャラクターに初期付与するアビリティとエフェクトを設定
	SetupDefaultAbilitiesAndEffects();

	// デフォルトエフェクトを適用
	InitializeDefaultEffects();

	// デフォルトアビリティを付与
	InitializeDefaultAbilities();

	// ASC登録時のデリゲートをブロードキャスト
	OnAscRegistered.Broadcast(AbilitySystemComponent, AttributeSet);
}
```

## ソースコード
<div style="background-color: #333;">
  MyCharacter.h
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "AbilitySystemInterface.h"
#include "GameFramework/Character.h"
#include "MyCharacter.generated.h"

// 前方宣言
class UAbilitySystemComponent;
class UAttributeSet;
class UGameplayEffect;
class UGameplayAbility;

// ASCが登録されたときに発行するデリゲート
DECLARE_MULTICAST_DELEGATE_TwoParams(FOnASCRegistered, UAbilitySystemComponent*, UAttributeSet*)

UCLASS()
class ETA_API AMyCharacter : public ACharacter, public IAbilitySystemInterface
{
	GENERATED_BODY()

public:
	// Sets default values for this character's properties
	AMyCharacter();

	// プロパティのレプリケーションで使用する関数
	virtual void GetLifetimeReplicatedProps(TArray<FLifetimeProperty>& OutLifetimeProps) const;

	/** IAbilitySystemInterface */
	virtual UAbilitySystemComponent* GetAbilitySystemComponent() const override { return AbilitySystemComponent; }
	UAttributeSet* GetAttributeSet() const { return AttributeSet; }
	/** end IAbilitySystemInterface */


protected:
	UPROPERTY()
	TObjectPtr<UAbilitySystemComponent> AbilitySystemComponent;

	UPROPERTY()
	TObjectPtr<UAttributeSet> AttributeSet;

	// ASC登録時にブロードキャストするデリゲート
	FOnASCRegistered OnAscRegistered;

	// 初期付与するアビリティとエフェクトを設定する関数
	virtual void SetupDefaultAbilitiesAndEffects();

	// ゲームプレイエフェクトを自身に適用
	void ApplyEffectToSelf(TSubclassOf<UGameplayEffect> GameplayEffectClass, float Level) const;

	// エフェクト初期化
	virtual void InitializeDefaultEffects() const;

	// アビリティ初期化
	void InitializeDefaultAbilities();

	// アビリティシステムの初期化(ASCのOwner,Avatorが生成されたときに呼び出す)を行う
	virtual void InitializeAbilitySystem(AActor* InOwnerActor, AActor* InAvatarActor);

private:
	// キャラクターに初期付与するアビリティ
	UPROPERTY(EditAnywhere, Category = "Abilities")
	TArray<TSubclassOf<UGameplayAbility>> DefaultAbilities;

	// キャラクターに初期付与するパッシブアビリティ
	UPROPERTY(EditAnywhere, Category = "Abilities")
	TArray<TSubclassOf<UGameplayAbility>> DefaultPassiveAbilities;

	// キャラクターに初期付与するゲームプレイエフェクト
	UPROPERTY(EditAnywhere, Category = "Effects")
	TArray<TSubclassOf<UGameplayEffect>> DefaultEffects;
};
```
</div>
<br>
<div style="background-color: #333;">
  MyCharacter.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.


#include "Characters/Base/MyCharacter.h"
#include "AbilitySystem/MyAbilitySystemComponent.h"
#include "Controller/Player/MyPlayerState.h"


// Sets default values
AMyCharacter::AMyCharacter()
{
 	// Set this character to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = false;

}

// プロパティのレプリケーションで使用する関数
void AMyCharacter::GetLifetimeReplicatedProps(TArray<FLifetimeProperty>& OutLifetimeProps) const
{
	Super::GetLifetimeReplicatedProps(OutLifetimeProps);

	// DOREPLIFETIME(AMyCharacter, bIsStunned);
}


void AMyCharacter::ApplyEffectToSelf(TSubclassOf<UGameplayEffect> GameplayEffectClass, float Level) const
{
	check(IsValid(GetAbilitySystemComponent()));
	check(GameplayEffectClass);

	// エフェクトは以下の3つのステップで適用される

	// ASCからコンテキストハンドルを作成
	FGameplayEffectContextHandle ContextHandle = GetAbilitySystemComponent()->MakeEffectContext();
	ContextHandle.AddSourceObject(this);	// エフェクトのソースオブジェクトは自分自身

	// クラス、レベル、コンテキストハンドルからスペックハンドルを作成
	const FGameplayEffectSpecHandle SpecHandle = GetAbilitySystemComponent()->MakeOutgoingSpec(GameplayEffectClass, Level, ContextHandle);

	// スペックハンドルを使ってASCに適用
	GetAbilitySystemComponent()->ApplyGameplayEffectSpecToTarget(*SpecHandle.Data.Get(), GetAbilitySystemComponent());
}

void AMyCharacter::InitializeDefaultEffects() const
{
	for(auto & EffectClass : DefaultEffects)
	{
		ApplyEffectToSelf(EffectClass, 1.f);
	}
}

void AMyCharacter::InitializeDefaultAbilities()
{
	// デフォルトアビリティをASCに付与
	UMyAbilitySystemComponent* MyASC = CastChecked<UMyAbilitySystemComponent>(AbilitySystemComponent);
	if (!HasAuthority()) return;

	MyASC->AddCharacterAbilities(DefaultAbilities);
	MyASC->AddCharacterPassiveAbilities(DefaultPassiveAbilities);
}

void AMyCharacter::SetupDefaultAbilitiesAndEffects()
{
	
	// DefaultAbilities.Add(MyAbility::StaticClass());

	// DefaultPassiveAbilities.Add(MyPassiveAbility::StaticClass());

	// DefaultEffects.Add(MyGameplayEffect::StaticClass());
}

void AMyCharacter::InitializeAbilitySystem(AActor* InOwnerActor, AActor* InAvatarActor)
{
	check(InOwnerActor);
	check(InAvatarActor);

	// InOwnerActorがAMyPlayerStateの場合は、AMyPlayerStateからASCとAttributeSetを取得
	if (AMyPlayerState* MyPlayerState = Cast<AMyPlayerState>(InOwnerActor))
	{
		// PlayerStateからASCとAttributeSetを取得
		if (!AbilitySystemComponent)
		{
			AbilitySystemComponent = MyPlayerState->GetAbilitySystemComponent();
		}
		if (!AttributeSet)
		{
			AttributeSet = MyPlayerState->GetAttributeSet();
		}
	}
	check(AbilitySystemComponent);
	check(AttributeSet);

	// ASCのOwnerとAvatarを設定
	AbilitySystemComponent->InitAbilityActorInfo(InOwnerActor, InAvatarActor);

	// キャラクターに初期付与するアビリティとエフェクトを設定
	SetupDefaultAbilitiesAndEffects();

	// デフォルトエフェクトを適用
	InitializeDefaultEffects();

	// デフォルトアビリティを付与
	InitializeDefaultAbilities();

	// ASC登録時のデリゲートをブロードキャスト
	OnAscRegistered.Broadcast(AbilitySystemComponent, AttributeSet);
}

```
</div>
<br>

# プレイヤーキャラクターの作成

## アビリティシステムの初期化  
プレイヤーキャラクターの場合、アビリティシステムコンポーネントとアトリビュートセットのオーナーはプレイヤーステートなので、この２つが揃うのはサーバーではPossessedBy、クライアントではOnRep_PlayerStateが呼ばれた時点となります。そのタイミングでアビリティーシステム初期化関数であるInitializeAbilitySystemを呼びます。  
またプレイヤーキャラクターにはカメラが必要なのでスプリングアームとカメラのコンポーネントを追加しておきます。
## ソースコード

<div style="background-color: #333;">
  MyPlayerCharacter.h
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "Characters/Base/MyCharacter.h"
#include "MyPlayerCharacter.generated.h"

// 前方宣言
class UCameraComponent;
class USpringArmComponent;

/**
 * 
 */
UCLASS()
class ETA_API AMyPlayerCharacter : public AMyCharacter
{
	GENERATED_BODY()
	
public:
	AMyPlayerCharacter();
	virtual void PossessedBy(AController* NewController) override;
	virtual void OnRep_PlayerState() override;

	// Called to bind functionality to input
	virtual void SetupPlayerInputComponent(class UInputComponent* PlayerInputComponent) override;

private:
	// カメラコンポーネント
	UPROPERTY(VisibleAnywhere)
	TObjectPtr<UCameraComponent> CameraComponent;

	// スプリングアームコンポーネント
	UPROPERTY(VisibleAnywhere)
	TObjectPtr<USpringArmComponent> CameraBoom;
};
```
</div>
<br>
<div style="background-color: #333;">
  MyPlayerCharacter.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.


#include "Characters/Player/MyPlayerCharacter.h"
#include "GameFramework/PlayerState.h"
#include "Camera/CameraComponent.h"
#include "GameFramework/SpringArmComponent.h"
#include "GameFramework/CharacterMovementComponent.h"


AMyPlayerCharacter::AMyPlayerCharacter()
{
	// スプリングアームコンポーネントの作成
	CameraBoom = CreateDefaultSubobject<USpringArmComponent>("CameraBoom");
	CameraBoom->SetupAttachment(GetRootComponent());
	CameraBoom->SetUsingAbsoluteRotation(true);
	CameraBoom->bDoCollisionTest = false;

	// カメラコンポーネントの作成
	CameraComponent = CreateDefaultSubobject<UCameraComponent>("CameraComponent");
	CameraComponent->SetupAttachment(CameraBoom, USpringArmComponent::SocketName);
	CameraComponent->bUsePawnControlRotation = false;

	// キャラクター移動設定
	GetCharacterMovement()->bOrientRotationToMovement = true;			/* 移動方向を向く */
	GetCharacterMovement()->RotationRate = FRotator(0.f, 400.f, 0.f);	/* ローテーションレート指定 */
	GetCharacterMovement()->bConstrainToPlane = true;					/* 常に地面に沿って移動する */
	GetCharacterMovement()->bSnapToPlaneAtStart = true;					/* 開始時に地面に接地設定 */

	/* コントローラーの回転角を使わない */
	bUseControllerRotationPitch = false;
	bUseControllerRotationRoll = false;
	bUseControllerRotationYaw = false;
}

void AMyPlayerCharacter::PossessedBy(AController* NewController)
{
	Super::PossessedBy(NewController);

	// サーバーではPossessedByが呼ばれたタイミングでPlayerStateが設定される
	// ASCの初期化を行う
	InitializeAbilitySystem(GetPlayerState(), this);
}

void AMyPlayerCharacter::OnRep_PlayerState()
{
	Super::OnRep_PlayerState();

	// クライアントではOnRep_PlayerStateが呼ばれたタイミングでPlayerStateが設定される
	// ASCの初期化を行う
	InitializeAbilitySystem(GetPlayerState(), this);
}

// Called to bind functionality to input
void AMyPlayerCharacter::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
	Super::SetupPlayerInputComponent(PlayerInputComponent);
}

```
</div>
<br>

# エネミーキャラクターの作成
エネミーキャラクターの場合、アビリティシステムコンポーネントとアトリビュートセットはコンストラクタ内で自分自身で生成・保持します。アビリティーシステム初期化関数のInitializeAbilitySystemはBeginPlayで呼びます。  
PossessedByはAIControllerに所有されたときのBehaviorTreeの初期化をコメントアウトで残してあります。
## ソースコード
<div style="background-color: #333;">
  MyEnemyCharacter.h
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "Characters/Base/MyCharacter.h"
#include "MyEnemyCharacter.generated.h"

// 前方宣言
#if 0
class UBehaviorTree;
class AMyAIController;
#endif

/**
 * 
 */
UCLASS()
class ETA_API AMyEnemyCharacter : public AMyCharacter
{
	GENERATED_BODY()

public:
	// コンストラクタでデフォルト値を設定
	AMyEnemyCharacter();

	// コントローラーがこのキャラクターを所持したときに呼ばれる関数
	virtual void PossessedBy(AController* NewController) override;

	// ゲーム開始時またはスポーン時に呼ばれる関数
	virtual void BeginPlay() override;

#if 0
protected:
	// 敵の挙動が記述されているBehaviorTree
	UPROPERTY(EditAnywhere, Category = "AI")
	TObjectPtr<UBehaviorTree> BehaviorTree;

	// このエネミーを所持しているAIコントローラー
	UPROPERTY()
	TObjectPtr<AMyAIController> MyAIController;
#endif
};
```
</div>
<br>
<div style="background-color: #333;">
  MyEnemyCharacter.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.


#include "MyEnemyCharacter.h"
#include "AbilitySystem/MyAbilitySystemComponent.h"
#include "AbilitySystem/MyAttributeSet.h"
#include "GameFramework/CharacterMovementComponent.h"
#if 0
#include "BehaviorTree/BehaviorTree.h"
#include "BehaviorTree/BlackboardComponent.h"
#endif

AMyEnemyCharacter::AMyEnemyCharacter()
{
	// AbilitySystemComponentとAttributeSetを作成
	AbilitySystemComponent = CreateDefaultSubobject<UMyAbilitySystemComponent>("AbilitySystemComponent");
	AbilitySystemComponent->SetIsReplicated(true);
	AbilitySystemComponent->SetReplicationMode(EGameplayEffectReplicationMode::Minimal);

	AttributeSet = CreateDefaultSubobject<UMyAttributeSet>("AttributeSet");

	// コントローラーの回転をキャラクターにそのままコピーしないように設定
	bUseControllerRotationPitch = false;
	bUseControllerRotationRoll = false;
	bUseControllerRotationYaw = false;

	// コントローラーの向きに合わせてキャラクターがRotation Speedで滑らかに回転するように設定
	GetCharacterMovement()->bUseControllerDesiredRotation = true;


}

void AMyEnemyCharacter::PossessedBy(AController* NewController)
{
	Super::PossessedBy(NewController);

	// 敵AIはサーバーでのみ実行、クライアントはレプリケートされるだけ
	if (!HasAuthority()) return;
#if 0
	// コントローラーに所持されたときにBlakboardを初期化しBehaviorTreeを実行
	MyAIController = Cast<AMyController>(NewController);
	MyController->GetBlackboardComponent()->InitializeBlackboard(*BehaviorTree->BlackboardAsset);
	MyController->RunBehaviorTree(BehaviorTree);
	// Blackboardの各キーを初期化
	// MyController->GetBlackboardComponent()->SetValueAsBool(FName("HitReacting"), false);
#endif
}

void AMyEnemyCharacter::BeginPlay()
{
	// Enemyは自身がASCのOwnerでありAvatarでもあるので、BeginPlayでASCを初期化
	InitializeAbilitySystem(this, this);
}
```
</div>
<br>

# プレイヤーコントローラーの作成
プレイヤーコントローラーは特別なことはしません。プレイヤーコントローラクラスを派生させてプロジェクト用のプレイヤーコントローラーを作成します。
<div style="background-color: #333;">
  MyPlayerController.h
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "GameFramework/PlayerController.h"
#include "MyPlayerController.generated.h"

/**
 * 
 */
UCLASS()
class ETA_API AMyPlayerController : public APlayerController
{
	GENERATED_BODY()
	
};
```
</div>
<br>
<div style="background-color: #333;">
  MyPlayerController.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.


#include "Controller/Player/MyPlayerController.h"

```
</div>
<br>

## ゲームモードの作成
ゲームモードはコンストラクタでデフォルトのプレイヤーコントローラークラス、プレイヤーステートクラス、ポーンクラスに作成したものを設定します。
<div style="background-color: #333;">
  MyGameMode.h
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "GameFramework/GameModeBase.h"
#include "MyGameMode.generated.h"

/**
 * ゲームモード：PlayerController, PlayerState, DefaultPawn を設定
 */
UCLASS()
class ETA_API AMyGameMode : public AGameModeBase
{
	GENERATED_BODY()

public:
	AMyGameMode();
};
```
</div>
<br>
<div style="background-color: #333;">
  MyGameMode.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.

#include "GameModes/MyGameMode.h"
#include "Controller/Player/MyPlayerController.h"
#include "Controller/Player/MyPlayerState.h"
#include "Characters/Player/MyPlayerCharacter.h"

AMyGameMode::AMyGameMode()
{
	// プレイヤーコントローラ、プレイヤーステート、デフォルトポーンを設定
	PlayerControllerClass = AMyPlayerController::StaticClass();
	PlayerStateClass      = AMyPlayerState::StaticClass();
	DefaultPawnClass      = AMyPlayerCharacter::StaticClass();
}
```
</div>
<br>

# ゲームインスタンスの作成
ゲームインスタンスは特別なことはしません。GameInstanceクラスを派生させてプロジェクト用のゲームインスタンスを作成します。
<div style="background-color: #333;">
  MyGameInstance.h
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "Engine/GameInstance.h"
#include "MyGameInstance.generated.h"

/**
 * 
 */
UCLASS()
class ETA_API UMyGameInstance : public UGameInstance
{
	GENERATED_BODY()
	
};
```
</div>
<br>
<div style="background-color: #333;">
  MyGameInstance.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.


#include "GameInstance/MyGameInstance.h"

```
</div>
<br>


# 起動確認
## プロジェクト設定
プロジェクト設定でデフォルトのゲームインスタンスとゲームモードを作成したものにしてプレイ開始します。
## デバッグ表示
コンソールコマンドで
```sh
showdebug abilitysystem
```
を使うとアビリティシステムをセットアップしたキャラクターのアトリビュート情報が確認できます。

# ゲームプレイエフェクト
ゲームプレイエフェクトはアトリビュートやゲームプレイタグに対して「何のアトリビュートをどう変化させるか」「何のタグを付与するか」という設定のリストが書かれたデータクラスです。  
これを対象のアビリティシステムコンポーネントに適用することによりキャラの基本パラメータの設定や、ダメージによるヒットポイントの減産処理を行います。  
ゲームプレイエフェクトには適用期間があるので無限にしてパラメータのベース値として利用したり、期間を設定してバフのように扱ったり、即時反映にしてHealthのダメージ減算に使ったりと、アビリティシステムのパラメータ計算の中枢を担っています。

## ゲームプレイエフェクトの特徴
ゲームプレイエフェクトの特徴を挙げると以下のようなものがあります。
### データのみ
コードは含まないデータだけの構造体です。
### Blueprintベースで作成する
サブクラス化せずゲームプレイエフェクトをそのまま使用します。  
Blueprintで作ると楽ですが、もちろんC++でも作成可能です。
### モディファイアとエグゼキューションによるアトリビュートを変更
ゲームプレイエフェクトの主たる機能であるアトリビュートの変更ですが、モディファイアとエグゼキューションという２つの機能により実現しています。
- モディファイア  
マグニチュードという値を使って操作タイプごとの計算を行いアトリビュート値を変更させる  
複雑なカスタム計算を使ってアトリビュートを変更することも可能
  - マグニチュード  
    アトリビュート計算に使われる値  
    マグニチュード値自体も、以下の計算タイプで計算させることができる
    - ScalableFloat  
      フロート値による指定  
      そのままマグニチュード値として扱うことも、テーブルを参照して値を引くこともできる
      - HarcoreValue  
        指定された値をそのままマグニチュードとして扱う
      - Table  
        データテーブルを使ってアトリビュート値からマグニチュード値を引く
    - AttributeBased  
      他のアトリビュートの値をベースに計算する  
      プレイヤーの最大HPをStrengthと同じとしたり、その10倍の値とするなど  
	  単一のアトリビュート値をもとにした計算しかできないので、複数の値で計算する場合はMMCを使う
    - MMC(CustomCalculationClass)  
      様々なアトリビュートや外部の値を使って複雑な計算を行いマグニチュードを計算する方法  
      計算を実行する関数を持つクラスを作成して指定
    - Set by Caller  
      エフェクトを適用しようとしている呼び出し元にマグニチュードを計算させる  
      キーと値のペアになっていて、名前またはゲームプレイタグに関連付けられたマグニチュードを割り当てる  
      コーディングによりマグニチュードを計算したい場合に便利
  - 操作タイプ  
    マグニチュードとアトリビュートをどう計算するか以下の操作タイプで指定する
    - Add  
    足し算。シンプルに与えられたマグニチュードの値をアトリビュートの値に足す  
    マグニチュードがマイナスであれば引き算になる
    - Multiply  
    掛け算  
    アトリビュートに対してマグニチュードの値と掛け算をう
    - Divide  
    割り算  
    アトリビュートに対してマグニチュードの値と割り算を行う
    - Override  
    上書き  
    単純に現在のアトリビュートをマグニチュードの値で上書き
- エグゼキューション(GameplayEffect Execution Calculation)  
  アトリビュートを変更するための最も強力な方法  
  コーディングで様々なアトリビュートや数値から動的にゲームプレイエフェクトを生成して適用させる  
  エフェクトはいくらでも作って適応できるので同時に複数のアトリビュートを変更することができる
### 期間ポリシー
ゲームプレイエフェクトは、その有効期間を設定する期間ポリシー(Duration Policy)が存在します。  
期間ポリシーにかかわらずエフェクトを削除したい売は[こちら](#ゲームプレイエフェクトの適用と削除)を参照してください。
#### Instant  
1回限りのアクションで即座に適用されるイン スタントエフェクトで、ベース値を書き換えるます。
#### Has Duration  
ゲームプレイエフェクトが有効になる期間を指定できます。  
一定期間Attributeを変更し、期間が終了したら元の値に戻ります。  
バフなどに用いられます。
#### Inifinit  
ゲームプレイエフェクトの効果を永続的に適用したままにします。  
期間適用する必要があるかわからない場合やゲームプレイに応じて削除する必要がある場合に役立ちます。  
セカンダリアトリビュートなどにも用いられます。  
### スタッキング
ゲームプレイフェクトはスタック可能で、どのようなルールでスタックさせるかの独自のポリシーを設定できます。  
詳細は[こちら](#ゲームプレイエフェクトのスタッキング)で説明しています。
### ゲームプレイタグの追加
ゲームプレイエフェクトが有効な間、対象のアビリティシステムコンポーネントにゲームプレイタグを追加することができます。
### アビリティの付与
ゲームプレイエフェクトが有効な間、指定したアビリティを付与することもできます。
## ゲームプレイエフェクトの適用
### ゲームプレイエフェクトスペック
ゲームプレイエフェクトはどのアトリビュートをどのように変化させるかを記したクラスであるのに対し、ゲームプレイエフェクトスペックは実際にそれがインスタンス化したメモリ上の動的オブジェクトになります。これを対象に適用することによりゲームプレイエフェクトの効果が実行されます。  
スペックは、エフェクトの参照に加え「誰が」「誰に」「どのレベルで」発動したかという状況の情報(コンテキスト)を持っています。  
例えば魔法使いがファイアボールを敵にあてた場合
- ゲームプレイエフェクトスペック(敵に与える効果全般データ)
  -  ゲームプレイエフェクト(参照)  
     ファイアボールが与えるダメージのルール
  -  計算済みの数値
  -  ゲームプレイエフェクトコンテキストハンドル(発生状況)
     -  誰が(Instigator)→葉法使い
     -  何で(Causer)→ファイアボールアクター
     -  どのように当たったか→HitResult

といった構成の情報の持ち方になります。  
### ゲームプレイエフェクトのワークフロー
実際にゲームプレイエフェクトが適用されるワークフローは  
1. ゲームプレイエフェクトコンテキストハンドルの作成  
   ```cpp
   UAbilitySystemComponent::MakeEffectContext()
   ```   
2. ゲームプレイエフェクトスペックの作成
   ```cpp
   UAbilitySystemComponent::MakeOutgoingSpec(TSubclassOf<UGameplayEffect> GameplayEffectClass, float Level, FGameplayEffectContextHandle Context)  
   ```
3.  ゲームプレイエフェクトスペックの適用  
    ```cpp
    UAbilitySystemComponent::ApplyGameplayEffectSpecToTarget(const FGameplayEffectSpec &Spec, UAbilitySystemComponent *Target, FPredictionKey PredictionKey)
	```
といった流れになります。

## モディファイアによるアトリビュート変更
モディファイアはアトリビュートをどのように変化させるかを定義したものです。  
値を直接指定するもの、アトリビュート同士の演算を使うもの、呼び出し元で設定するもの、複雑な計算を行うものなど様々な種類があります。

### スケーラブルフロートによる変更
プライマリアトリビュートの初期値設定などに適している最もシンプルなものです。
モディファイアに、アトリビュート、演算方法、値を指定して、エフェクトのモディファイア配列に追加します。
<div style="background-color: #333;">
  PlayerPrimaryAttributesEffect.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
// Copyright MyGameCompany. All Rights Reserved.

#include "PlayerPrimaryAttributesEffect.h"
#include "Characters/Base/AbilitySystem/MyAttributeSet.h"
#include "GameplayEffectTypes.h"

UPlayerPrimaryAttributesEffect::UPlayerPrimaryAttributesEffect()
{
	// オーバーライドモディファイアでプライマリーアトリビュートの初期値を設定する
	FGameplayModifierInfo NewMod;
	NewMod.Attribute = UMyAttributeSet::GetStrengthAttribute();
	NewMod.ModifierOp = EGameplayModOp::Override;
	NewMod.ModifierMagnitude = FScalableFloat(10.f);
	Modifiers.Add(NewMod);
}
```
</div>
<br>
実際に使用する際は、AMyCharacter::ApplyEffectToSelfで直接エフェクトを適用するか、AMyCharacter::SetupDefaultAbilitiesAndEffectsでデフォルトで適用するアトリビュートに追加しておき、初期化時に適用してもらいます。
<br>
<br>

<div style="background-color: #333;">
  MyPlayerCharacter.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
void AMyPlayerCharacter::SetupDefaultAbilitiesAndEffects()
{
	Super::SetupDefaultAbilitiesAndEffects();

	// PlayerPrimaryAttributesEffect をデフォルトエフェクトに追加
	DefaultEffects.AddUnique(UPlayerPrimaryAttributesEffect::StaticClass());
}
```
</div>
<br>

### スケーラブルフロートとカーブテーブルによる変更
設定した値を時間としてカーブテーブルから値を引いたものを適用するケースです。  
RPGなどでレベルごとに最大HPが上がっていく場合、レベルと最大HPの変化をカーブテーブルで表現しておけば、直値にレベルを指定することにより、そのレベルでの最大HPをアトリビュートに設定することができます。
<div style="background-color: #333;">
  PlayerPrimaryAttributesEffect.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
UPlayerPrimaryAttributesEffect::UPlayerPrimaryAttributesEffect()
{
	/*
	 *	CSV文字列からカーブデータを構築する
	 */
	UCurveTable* StrengthCurveTable = NewObject<UCurveTable>();
	// CSV形式のデータ（例: 行名, タイム=値, タイム=値...）
	FString CsvString = TEXT("StrengthCurve, 0.0=0.0, 1.0=100.0, 2.0=200.0, 3.0=35.0");
	// 補間タイプを指定してテーブルデータを生成
	StrengthCurveTable->CreateTableFromCSVString(CsvString, ERichCurveInterpMode::RCIM_Cubic);

	/* アセットらカーブデータを構築する場合
	static const FString CurveTablePath = TEXT("CurveTable'/Game/Data/MyCurveTable.MyCurveTable'");
	ConstructorHelpers::FObjectFinder<UCurveTable> CurveTableFinder(*CurveTablePath);
	UCurveTable* StrengthCurveTable = CurveTableFinder.Object;
	*/

	// ScalableFloatにカーブテーブルを設定する
	FGameplayModifierInfo NewMod;
	NewMod.Attribute = UMyAttributeSet::GetStrengthAttribute();
	NewMod.ModifierOp = EGameplayModOp::Override;
	FScalableFloat Scalable;
	Scalable.Value = 1.0f;	// カーブの横軸の値を指定
	Scalable.Curve.RowName = FName(TEXT("StrengthCurve"));
	Scalable.Curve.CurveTable = StrengthCurveTable;
	NewMod.ModifierMagnitude = Scalable;
	Modifiers.Add(NewMod);
}
```
</div>
<br>


### アトリビュートによる変更
他のアトリビュートを使って計算を行い対象となるアトリビュート値を決定する方法です。  
単一の値から計算することしかできない点に注意が必要です。  
一つのプライマリアトリビュートをベースにして計算されるセカンダリアトリビュートなどで使われます。
#### 計算の順序
演算は「事前加算 → 係数乗算 → 事後加算」の順に行われます。  
例えば  
```
Armor = (Resilience + 2) * 0.25 + 6
```
という計算であれば、モディファイアの計算タイプにAttributeBasedに指定して、
- PreAdd = 2
- Coefficient = 0.25
- PostAdd = 6。
というモディファイアを設定します。

#### アトリビュート取得方法
また、計算元となるアトリビュートをどのように取得するかの設定をFGameplayEffectAttributeCaptureDefinitionという構造体で指定します。

- どのアトリビュート値を使うか  
  アトリビュートのクラスを指定
- アトリビュートを取得する対象  
  アトリビュートを与える側か、与えられる側かを選びます。  
  セカンダリアトリビュートはどちらも同じアクターですが、意味合い的に「対象の」という部分が強いのでTargetを指定します。
- スナップショットを取るか  
  通常はfalseで大丈夫です。  
  キャラクターが弾を発射する場合、敵に与えるダメージエフェクトを弾に持たせることになりますが、着弾時に発射元のキャラクターが死亡などの理由で存在し続けているとは限りません。発射元キャラクターがいない状態でダメージ計算時を行うと計算に必要なアトリビュートが取れなくなるので、そういった場合にスナップショットをtrueにして、エフェクト生成時のアトリビュート値を保存しておきます。

#### 使用するアトリビュートの値
計算元となるアトリビュートの度の値を使うかも設定できます。
通常はマグニチュード値を使いますが、エフェクトがかかる前のベース値やエフェクトによるボーナス値(マグニチュード値 - ベース値)を使うこともできます。

#### アトリビュートの有効期間
セカンダリーアトリビュートは永続的にプライマリーアトリビュートから計算されるのでDurationPolicyはInfiniteにします。

<div style="background-color: #333;">
  MySecondaryAttributesEffect.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
UMySecondaryGameplayEffect::UMySecondaryGameplayEffect()
{
	FGameplayModifierInfo Mod;
	Mod.Attribute = UMyAttributeSet::GetArmorAttribute();
	Mod.ModifierOp = EGameplayModOp::Override;

	// AttributeBased の設定
	FAttributeBasedFloat AttrBased;
	AttrBased.BackingAttribute = FGameplayEffectAttributeCaptureDefinition(
		UMyAttributeSet::GetResilienceAttribute(),
		EGameplayEffectAttributeCaptureSource::Target,
		false /* bSnapshot */);
	// ↑今回の計算はスナップショットを取らない(Resilienceの値が変化したらArmorの値も変化する)
	// 　弾などに設定するエフェクトは発射キャラクターのアトリビュートを発射時にキャプチャしておく必要があるのでtrueにする

	// Armor = (Resilience * 0.25 + 2) + 6
	AttrBased.Coefficient = 0.25f;
	AttrBased.PreMultiplyAdditiveValue = 2.0f;
	AttrBased.PostMultiplyAdditiveValue = 6.0f;

	// 計算元のアトリビュート値はマグニチュードを使う(他にベース値やボーナス値(マグニチュード-ベース値)を使う指定もある)
	AttrBased.AttributeCalculationType = EAttributeBasedFloatCalculationType::AttributeMagnitude;

	Mod.ModifierMagnitude = FGameplayEffectModifierMagnitude(AttrBased);
	Modifiers.Add(Mod);

	// セカンダリーアトリビュートは永続的にプライマリーアトリビュートから計算されるので
	// DurationPolicyはInfiniteにする
	DurationPolicy = EGameplayEffectDurationType::Infinite;
}
```
</div>
<br>
<br>

プライマリアトリビュートと同様に、実際に使用する際は、AMyCharacter::ApplyEffectToSelfで直接エフェクトを適用するか、AMyCharacter::SetupDefaultAbilitiesAndEffectsでデフォルトで適用するアトリビュートに追加しておき、初期化時に適用してもらいます。
<div style="background-color: #333;">
  MyPlayerCharacter.cpp
</div>
<div style="max-height: 300px; overflow-y: auto;">

```cpp
void AMyPlayerCharacter::SetupDefaultAbilitiesAndEffects()
{
	Super::SetupDefaultAbilitiesAndEffects();

	// MyPlayerPrimaryAttributesEffect をデフォルトエフェクトに追加
	DefaultEffects.AddUnique(UMyPlayerPrimaryAttributesEffect::StaticClass());
	// MySecondaryGameplayEffect をデフォルトエフェクトに追加
	DefaultEffects.AddUnique(UMySecondaryGameplayEffect::StaticClass());
}
```
</div>
<br>

### 呼び出し元の設定による変更
マグニチュードを呼び出し元で計算して指定する方式です。  
ゲームプレイエフェクトスペックハンドルを作成した後、UAbilitySystemBlueprintLibrary::AssignTagSetByCallerMagnitude関数で、スペックハンドルにゲームプレイタグとfloat値のペアを書き込みます。  
ゲームプレイエフェクトではモディファイアを追加し、計算タイプを「Set by Caller」に設定して、データタグにスペックハンドルに書き込んだゲームプレイタグを設定します。  
これによりマグニチュードがスペックハンドルに書き込んだゲームプレイタグに対応するfloat値になります。  
ScalableFloatやアトリビュートベースのような決まった計算ではなく、C++でゲームプレイエフェクト生成時に自由にマグニチュード計算したい場合に便利です。  
TODO


### MMC(Modiier Magnitude Calculations)による変更
TODO

### ExecutionCalculationによる変更
TODO

### モディファイアの計算順序
TODO

### モディファイアの係数
TODO

## ゲームプレイエフェクトの適用と削除
TODO

## ゲームプレイエフェクトのスタッキング
ゲームプレイフェクトはスタック可能で、どのようなルールでスタックさせるかの独自のポリシーを設定できます。  
TODO

## アトリビュート変更の検知
アビリティシステムコンポーネントはアトリビュート値が変化したときに発生するデリゲートを持っています。
TODO

## ゲームプレイエフェクト変更の検知
アビリティシステムコンポーネントはゲームプレイエフェクトが適用・除去されたときに呼び出されるデリゲートを持っています。
TODO

## ゲームプレイエフェクトのコンポーネント
ゲームプレイエフェクトには専用のコンポーネントが追加できます。  
これによりアトリビュートの変更以外に様々な機能を持たせることができます。  
以下がコンポーネントのリストです。
- UIデータ(テキストのみ)  
  テキストのみを含むUIデータを持たせます。  
  これは主に、UGameplayEffectUIDataのサブクラスの例として使用します。
  ゲームにはテキストのみが必要な場合は、このクラスを使用するのが妥当です。
  追加のデータを含めるためには、UGameplayEffectUIDataのカスタムサブクラスを作成します。
- このエフェクトにあるタグ(アセットタグ)  
  ゲームプレイエフェクトが持つ(所有する)タグを設定できます。これらはゲームプレイエフェクトが有効な間アビリティシステムコンポーネントで保持されますが、アクターには転送「されません」。  
  タグは継承されたタグに対して「追加するタグ」と「削除するタグ」を指定できるので、設定次第ではエフェクト適用中に特定のタグを無効化するといったことも可能です。
- このエフェクトを適用/継続するにはタグが必要  
  このエフェクトを適用・継続するために必要なターゲット(ゲームプレイエフェクトのオーナー)のタグ要件を設定できます。
- このエフェクトを適用するチャンス  
  ゲームプレイエフェクトの適用条件に確立を設定できます。
- カスタムでこのエフェクトを適用可能  
  CustomApplicationRequirement関数を処理して、このGameplayEffectを適用するかどうかを決定できます。
- ゲームプレイアビリティを付与  
  アクティブな間に追加のゲームプレイアビリティをゲームプレイエフェクトのターゲットに適用します。
- タグ付きのアビリティをブロック
  オーナーのゲームプレイエフェクトのターゲットアクタに対するゲームプレイタグに基づいて、ゲームプレイアビリティのアクティベーションのブロックを処理します
- ターゲットアクタにタグを付与  
  ゲームプレイエフェクトのターゲット(「オーナー」という場合もあります)にタグを付与します。このタグはアクターに付与されるもので、アビリティシステムコンポーネントには付与「されません」。
- 他のエフェクトに対する耐性  
  他のゲームプレイエフェクトスペックの適用をブロックします。  
  アビリティシステムコンポーネントにグローバルハンドラを登録することで他のゲームプレイエフェクトスペックの適用をブロックします。
- 他のエフェクトを除去  
  特定の条件に基づいて、他のゲームプレイエフェクトを削除します
- 追加のエフェクトを適用  
  特定の条件下(または条件なし)で他のゲームプレイエフェクトを適用します。

# ゲームプレイイベント