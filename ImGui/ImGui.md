# ImGuiの導入

## プラグインの取得
### zipのダウンロード
[UnrealImGui](https://github.com/IDI-Systems/UnrealImGui)でCode->Download ZIPで、zipをダウンロードします。
## ファイルの配置
zipを展開して、ディレクトリ名をImGuiに変更しプロジェクトのPluginsフォルダに配置します。  
※なければプロジェクト直下(uprojectファイルがある場所)にPluginsフォルダを作る。

```powersell
MyProject/Plugins/ImGui/
```
## slnファイルの作り直し
.uprojectファイルからslnファイルを作り直します。

## ビルドと実行確認
Visual Studioを起動し、ビルドを通します。
エディタを起動したらプラグインでImGuiが有効になっていることを確認します。
ゲームを実行しコンソールコマンドで
```powersell
>ImGui.ToggleDemo
```
と入力してデモウィンドウが開閉することを確認します。

## 依存関係の設定(Build.cs)
ImGuiのモジュールを認識させるため、{ProjectName}.Build.csにImGuiモジュールを追加します。
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
    
        PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore" });

		PrivateDependencyModuleNames.AddRange(new string[] {  });

        // ImGuiモジュールを追加
        PrivateDependencyModuleNames.AddRange(new string[] { "ImGui" }); 
    }
}
```
</div>

## デバッグメニューの実装(C++)
ImGuiウィンドウを描画するコードをActorやActorComponentに実装します。ここでは最もシンプルな「ActorクラスのTick内で描画する」方法を解説します。
<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyDebugActor.h
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
// MyDebugActor.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"

// ImGuiが有効かチェックしインクルード
#ifdef IMGUI_API
    #define WITH_IMGUI 1
    #include <imgui.h>
#else
    #define WITH_IMGUI 0
#endif

#include "MyDebugActor.generated.h"

UCLASS()
class MYPROJECT_API AMyDebugActor : public AActor
{
    GENERATED_BODY()
    
public:	
    AMyDebugActor();
    virtual void Tick(float DeltaTime) override;

private:
    // デバッグ用の変数例
    bool bGodMode = false;
    float PlayerSpeed = 600.0f;
};
```
</div>

<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyDebugActor.cpp
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
// MyDebugActor.cpp
#include "MyDebugActor.h"

AMyDebugActor::AMyDebugActor()
{
    PrimaryActorTick.bCanEverTick = true;
}

void AMyDebugActor::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);

#if WITH_IMGUI
    // 1. デバッグメニューのウィンドウを作成
    ImGui::Begin("Debug Menu");

    ImGui::Text("Game Status Debugger");
    ImGui::Separator();

    // 2. チェックボックス (無敵モードなどの切り替え)
    if (ImGui::Checkbox("God Mode", &bGodMode))
    {
        // チェックが変更された時の処理をここに書く
        UE_LOG(LogTemp, Log, TEXT("God Mode: %s"), bGodMode ? TEXT("ON") : TEXT("OFF"));
    }

    // 3. スライダー (プレイヤーの移動速度などの調整)
    if (ImGui::SliderFloat("Player Speed", &PlayerSpeed, 100.0f, 2000.0f))
    {
        // スライダーが動いた時の処理
    }

    // 4. ボタン (カスタムイベントのトリガー)
    if (ImGui::Button("Reset Player Position"))
    {
        // 座標リセット処理などを実行
        UE_LOG(LogTemp, Warning, TEXT("Position Reset Button Pressed!"));
    }

    ImGui::End();
#endif
}
```
</div>

## 実行時の動かし方
1. 上記の AMyDebugActor を レベル上に配置 してゲームを起動（PIE）します。
1. デフォルト設定ではゲームの操作が優先され、ImGuiウィンドウをクリックできません。コンソール（「@」または「`」キー）を開き、以下のコマンドを打ちます。
    1. ImGui.ToggleInput ：マウスカーソルが解放され、ImGuiウィンドウを操作できるようになります。
	1. もう一度打つとゲーム操作に戻ります。
1. （任意）毎回コマンドを打つのが面倒な場合は、プロジェクト設定（Project Settings）の Plugins ➔ ImGui から「Toggle Input」用のショートカットキー（例: Shift + Alt + T）を登録しておくと快適になります。

## マネージャの導入と日本語対応
デバッグウィンドウは複数のウィンドウを扱う必要があるため、それらを効率よく一括管理するにはマネージャークラス（UGameInstanceSubsystem などを継承）を1つ作成し、各ウィンドウの描画処理（ラムダ式や関数ポインタ、または専用のインターフェース）を登録・集中管理する設計が最も綺麗で保守しやすくなります。  
特に UGameInstanceSubsystem を使うと、ゲームの起動から終了まで永続し、どこからでもアクセスできるため最適です。  
複数の独立したデバッグウィンドウを一括でトグル表示管理できるマネージャーの具体的な実装例です。

### マネージャクラスの実装
このマネージャクラスは以下の機能を持ちます。
- デバッグメニュー表示の切り替え
  - Shift+Alt+I
  - Gamepad L3+R3
- デバッグメニュー表示中の入力モード切り替え(ImGui/Game)
  - Shift+Alt+O
  - Gamepad Option
- 日本語表示  
コンテンツフォルダに以下の日本語フォントファイルを置くと日本語が表示できます(utf-8)  
Content/Fonts/NotoSansJP-Regular.ttf

<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  ImGuiGameInstanceSubsystem.h
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
#pragma once

#include "CoreMinimal.h"
#include "Subsystems/GameInstanceSubsystem.h"

#ifdef IMGUI_API
#define WITH_IMGUI 1
#include <imgui.h>
#else
#define WITH_IMGUI 0
#endif

#include "ImGuiGameInstanceSubsystem.generated.h"

struct FImGuiWindowInfo
{
	FString Title;
	bool bIsOpen = false;
	TFunction<void()> RenderCallback;

	FVector2D InitialSize = FVector2D::ZeroVector;
	FVector2D SavedSize = FVector2D::ZeroVector;
	bool bHasSavedSize = false;
	bool bInitialSizeApplied = false;
};

UCLASS()
class ETA_API UImGuiGameInstanceSubsystem : public UGameInstanceSubsystem
{
	GENERATED_BODY()

public:
	virtual void Initialize(FSubsystemCollectionBase& Collection) override;
	virtual void Deinitialize() override;

	void RegisterWindowLambda(const FString& Title, TFunction<void()> RenderCallback, const FVector2D& InitialSize = FVector2D::ZeroVector, bool bDefaultOpen = false);
	void RegisterWindow(const FString& Title, void (UImGuiGameInstanceSubsystem::*RenderFunc)(), UImGuiGameInstanceSubsystem* Instance, const FVector2D& InitialSize = FVector2D::ZeroVector, bool bDefaultOpen = false);
	inline void RegisterWindow(const FString& Title, void (UImGuiGameInstanceSubsystem::*RenderFunc)(), bool bDefaultOpen = false)
	{
		RegisterWindow(Title, RenderFunc, this, FVector2D::ZeroVector, bDefaultOpen);
	}

	// タイトル指定でウィンドウを開閉/トグルする public API
	void OpenWindow(const FString& Title);
	void CloseWindow(const FString& Title);
	void ToggleWindow(const FString& Title);

private:
	void EnableImGuiGamepadNavigation(bool bEnable);
	void RenderDebugUI();
	void SetAllWindowsVisibility(bool bVisible);

	void RenderDebugMenu();

	TArray<FImGuiWindowInfo> RegisteredWindows;
	bool bGlobalVisibility = false;

	float CustomSpeedScale = 1.0f;

	// ImGui 用入力ロック用フラグ等（既存メンバ）...
	bool bInputLockedForImGui = false;
	bool bPrevShowMouseCursor = false;
	bool bPrevEnableClickEvents = false;
	bool bPrevEnableMouseOverEvents = false;
	bool bPrevImGuiModuleInputEnabled = false;
	bool bPrevImGuiKeyboardShared = false;
	bool bInputModeIsImGui = true;

#if WITH_IMGUI
	// フォント保持 / 登録管理
	TMap<FName, TArray<uint8>> CustomFontBuffers;
	TMap<FName, TSharedPtr<ImFontConfig>> CustomFontConfigs;

	// 登録した日本語フォント名（見つけてデフォルト化に使う）
	FName RegisteredJPFontName = NAME_None;
	bool bImGuiDefaultFontSet = false;

	bool LoadAndRegisterFont(const FString& ContentRelativePath, const FName& FontName, float SizePixels = 16.0f);
	void EnsureDefaultImGuiFontIfNeeded();
#endif
};
```
</div>

<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  ImGuiGameInstanceSubsystem.cpp
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
#include "ImGuiGameInstanceSubsystem.h"

#if WITH_IMGUI
#include "ImGuiDelegates.h"
#include "ImGuiModule.h"
#endif

#include "Misc/ConfigCacheIni.h"
#include "GameFramework/PlayerController.h"
#include "Engine/Engine.h"

#if WITH_IMGUI
#include "Misc/Paths.h"
#include "Misc/FileHelper.h"
#include "HAL/PlatformFilemanager.h"
#endif

namespace
{
	const TCHAR* ImGuiWindowSizesSection = TEXT("ImGuiWindowSizes");
	const TCHAR* SizeSeparator = TEXT(",");
	const float SizeEpsilon = 1e-3f;
}

void UImGuiGameInstanceSubsystem::Initialize(FSubsystemCollectionBase& Collection)
{
	Super::Initialize(Collection);

#if WITH_IMGUI
	if (UWorld* World = GetWorld())
	{
		FImGuiDelegates::OnWorldDebug(World).AddUObject(this, &UImGuiGameInstanceSubsystem::RenderDebugUI);
	}

	// メンバ関数を登録する場合は RegisterWindow を使う（ラムダは RegisterWindowLambda を使用）
	RegisterWindow(TEXT("Debug Menu"), &UImGuiGameInstanceSubsystem::RenderDebugMenu, true);

	// 日本語フォントを Content/Fonts 以下から自動登録し、フォントアトラス再構築を試みる。
	// ファイルが無ければ警告を出すだけで続行します。
	const FString JPFontRelative = TEXT("Fonts/NotoSansJP-Regular.ttf");
	const FName JPFontName = FName(TEXT("NotoSansJP"));
	if (LoadAndRegisterFont(JPFontRelative, JPFontName, 16.0f))
	{
		UE_LOG(LogTemp, Log, TEXT("ImGui: Japanese font '%s' registered."), *JPFontRelative);
	}
#endif
}

void UImGuiGameInstanceSubsystem::Deinitialize()
{
	RegisteredWindows.Empty();
	Super::Deinitialize();
}

void UImGuiGameInstanceSubsystem::RegisterWindowLambda(const FString& Title, TFunction<void()> RenderCallback, const FVector2D& InitialSize, bool bDefaultOpen)
{
	for (const auto& Window : RegisteredWindows)
	{
		if (Window.Title == Title) return;
	}

	FImGuiWindowInfo NewWindow;
	NewWindow.Title = Title;
	NewWindow.bIsOpen = bDefaultOpen;
	NewWindow.RenderCallback = RenderCallback;
	NewWindow.InitialSize = InitialSize;

	FString SavedValue;
	if (GConfig->GetString(ImGuiWindowSizesSection, *NewWindow.Title, SavedValue, GGameIni))
	{
		TArray<FString> Parts;
		SavedValue.ParseIntoArray(Parts, SizeSeparator, true);
		if (Parts.Num() >= 2)
		{
			float W = FCString::Atof(*Parts[0]);
			float H = FCString::Atof(*Parts[1]);
			NewWindow.SavedSize = FVector2D(W, H);
			NewWindow.bHasSavedSize = true;
		}
	}

	RegisteredWindows.Add(NewWindow);
}

void UImGuiGameInstanceSubsystem::RegisterWindow(const FString& Title, void (UImGuiGameInstanceSubsystem::* RenderFunc)(), UImGuiGameInstanceSubsystem* Instance, const FVector2D& InitialSize, bool bDefaultOpen)
{
	// Instance とメンバ関数ポインタをキャプチャして既存の TFunction 版に変換
	this->RegisterWindowLambda(Title, TFunction<void()>([Instance, RenderFunc]() { (Instance->*RenderFunc)(); }), InitialSize, bDefaultOpen);
}

void UImGuiGameInstanceSubsystem::SetAllWindowsVisibility(bool bVisible)
{
	for (auto& Window : RegisteredWindows)
	{
		Window.bIsOpen = bVisible;
	}
}

void UImGuiGameInstanceSubsystem::EnableImGuiGamepadNavigation(bool bEnable)
{
	// ImGui モジュールがロード済みならプロパティを変更
	if (FImGuiModule::IsAvailable())
	{
		FImGuiModule::Get().GetProperties().SetGamepadNavigationEnabled(bEnable);
		// 必要なら入力共有をオフにして ImGui に入力を渡す（ここは既定で共有にしている）
		FImGuiModule::Get().GetProperties().SetGamepadInputShared(true);
	}
}

void UImGuiGameInstanceSubsystem::RenderDebugUI()
{
#if WITH_IMGUI
	// ImGui フォントを必要ならセット（毎フレーム呼んでも最初の成功だけ有効）
	EnsureDefaultImGuiFontIfNeeded();

	// ワールドとプレイヤーコントローラーの安全な取得
	APlayerController* PC = nullptr;
	if (UWorld* World = GetWorld())
	{
		PC = World->GetFirstPlayerController();
		if (PC)
		{
			// メニューの表示/非表示をトグルするキー入力をチェック

			// Shift+Alt+I でトグル
			bool bToggleMenu = false;
			const bool bShiftDownForToggle = PC->IsInputKeyDown(EKeys::LeftShift) || PC->IsInputKeyDown(EKeys::RightShift);
			const bool bAltDownForToggle = PC->IsInputKeyDown(EKeys::LeftAlt) || PC->IsInputKeyDown(EKeys::RightAlt);
			if (bShiftDownForToggle && bAltDownForToggle && PC->WasInputKeyJustPressed(EKeys::I))
			{
				bToggleMenu = true;
			}
			// ゲームパッド左右スティック同時押しでトグル
			const bool bLeftDown =
				PC->IsInputKeyDown(EKeys::Gamepad_LeftThumbstick);
			const bool bRightDown =
				PC->IsInputKeyDown(EKeys::Gamepad_RightThumbstick);

			const bool bLeftJustPressed =
				PC->WasInputKeyJustPressed(EKeys::Gamepad_LeftThumbstick);
			const bool bRightJustPressed =
				PC->WasInputKeyJustPressed(EKeys::Gamepad_RightThumbstick);
			if (bLeftDown && bRightDown && (bLeftJustPressed || bRightJustPressed))
			{
				bToggleMenu = true;
			}

			// メニューの表示/非表示をトグル
			if (bToggleMenu)
			{
				bGlobalVisibility = !bGlobalVisibility;
				SetAllWindowsVisibility(bGlobalVisibility);

				// ここでメニューを “開く（trueにする）” 動作が発生した場合は
				// ImGui モジュールの入力を確実に有効化して ImGui モードへ復帰させる
				if (bGlobalVisibility)
				{
					// 保存してから有効化（復元用に保存）
					if (FImGuiModule::IsAvailable())
					{
						bPrevImGuiModuleInputEnabled = FImGuiModule::Get().GetProperties().IsInputEnabled();
						FImGuiModule::Get().GetProperties().SetInputEnabled(true);

						bPrevImGuiKeyboardShared = FImGuiModule::Get().GetProperties().IsKeyboardInputShared();
						FImGuiModule::Get().GetProperties().SetKeyboardInputShared(true);
					}
					// 明示的に ImGui 入力モードにする（これにより入力ロック判定などが働く）
					bInputModeIsImGui = true;
					EnableImGuiGamepadNavigation(bInputModeIsImGui);

					// メニューを開いた（true にした）とき、ウィンドウが一つも開いていなければ Debug Menu を自動オープン
					bool bAnyWindowOpenNow = false;
					for (const auto& W : RegisteredWindows)
					{
						if (W.bIsOpen)
						{
							bAnyWindowOpenNow = true;
							break;
						}
					}
					if (!bAnyWindowOpenNow)
					{
						OpenWindow(TEXT("Debug Menu"));
					}
				}
			}

			// デバッグメニュー表示の入力モードをトグル
			bool bToggleInputMode = false;
			if (bGlobalVisibility)
			{
				// Shift + Alt + O で入力モードをトグル
				if (bShiftDownForToggle && bAltDownForToggle && PC->WasInputKeyJustPressed(EKeys::O))
				{
					bToggleInputMode = true;
				}
				// ゲームパッドのオプションボタンで入力モードをトグル
				if (PC->WasInputKeyJustReleased(EKeys::Gamepad_Special_Right))
				{
					bToggleInputMode = true;
				}

			}

			if (bToggleInputMode)
			{
				bInputModeIsImGui = !bInputModeIsImGui;
				EnableImGuiGamepadNavigation(bInputModeIsImGui);
				if (GEngine)
				{
					const FString Msg = FString::Printf(TEXT("Input mode: %s"), bInputModeIsImGui ? TEXT("ImGui") : TEXT("Game"));
					GEngine->AddOnScreenDebugMessage(-1, 2.0f, FColor::Yellow, Msg);
				}
			}
		}
	}

	if (!bGlobalVisibility)
	{
		// UI 非表示時に ImGui 側で入力ロックが残っていたら解除
		if (bInputLockedForImGui && PC)
		{
			PC->SetShowMouseCursor(bPrevShowMouseCursor);
			PC->bEnableClickEvents = bPrevEnableClickEvents;
			PC->bEnableMouseOverEvents = bPrevEnableMouseOverEvents;

			FInputModeGameOnly GameOnly;
			PC->SetInputMode(GameOnly);

			PC->SetIgnoreLookInput(false);
			PC->SetIgnoreMoveInput(false);

			// ImGui モジュールの入力状態を復元
			if (FImGuiModule::IsAvailable())
			{
				FImGuiModule::Get().GetProperties().SetInputEnabled(bPrevImGuiModuleInputEnabled);
				FImGuiModule::Get().GetProperties().SetKeyboardInputShared(bPrevImGuiKeyboardShared);
			}

			bInputLockedForImGui = false;
		}
		return;
	}

	// 個別ウィンドウの描画
	for (auto& Window : RegisteredWindows)
	{
		if (!Window.bIsOpen) continue;

		// 初回表示時のサイズ決定（保存済みサイズを常に優先適用）
		if (!Window.bInitialSizeApplied)
		{
			FVector2D UseSize = FVector2D::ZeroVector;

			if (Window.bHasSavedSize)
			{
				UseSize = Window.SavedSize;
			}
			else if (!Window.InitialSize.IsNearlyZero())
			{
				UseSize = Window.InitialSize;
			}

			if (!UseSize.IsNearlyZero())
			{
				ImGui::SetNextWindowSize(ImVec2(UseSize.X, UseSize.Y), ImGuiCond_FirstUseEver);
			}

			Window.bInitialSizeApplied = true;
		}

		if (ImGui::Begin(TCHAR_TO_UTF8(*Window.Title), &Window.bIsOpen))
		{
			if (Window.RenderCallback)
			{
				Window.RenderCallback();
			}

			ImVec2 CurrentSize = ImGui::GetWindowSize();
			FVector2D CurrF(CurrentSize.x, CurrentSize.y);

			bool bSizeChanged = !CurrF.Equals(Window.SavedSize, SizeEpsilon);
			if (bSizeChanged)
			{
				Window.SavedSize = CurrF;
				Window.bHasSavedSize = true;

				FString Value = FString::Printf(TEXT("%f,%f"), Window.SavedSize.X, Window.SavedSize.Y);
				GConfig->SetString(ImGuiWindowSizesSection, *Window.Title, *Value, GGameIni);
				GConfig->Flush(false, GGameIni);
			}
		}
		ImGui::End();
	}

	// 全ウィンドウが閉じられている場合、自動で ImGui モードを終了する
	bool bAnyWindowOpen = false;
	for (const auto& Window : RegisteredWindows) { if (Window.bIsOpen) { bAnyWindowOpen = true; break; } }
	if (!bAnyWindowOpen && bInputModeIsImGui)
	{
		// ImGui モードを終了すると入力がゲームへ戻る
		bInputModeIsImGui = false;
		EnableImGuiGamepadNavigation(bInputModeIsImGui);

		// 重要: グローバル表示フラグも明示的にオフにしておく（これが無いと次のトグルで状態が反転してしまう）
		bGlobalVisibility = false;
		SetAllWindowsVisibility(false);

		// もし入力が ImGui 用にロックされているなら即座に復元する
		if (bInputLockedForImGui && PC)
		{
			PC->SetShowMouseCursor(bPrevShowMouseCursor);
			PC->bEnableClickEvents = bPrevEnableClickEvents;
			PC->bEnableMouseOverEvents = bPrevEnableMouseOverEvents;

			FInputModeGameOnly GameOnly;
			PC->SetInputMode(GameOnly);

			PC->SetIgnoreLookInput(false);
			PC->SetIgnoreMoveInput(false);

			// ImGui モジュールの入力状態を復元
			if (FImGuiModule::IsAvailable())
			{
				FImGuiModule::Get().GetProperties().SetInputEnabled(bPrevImGuiModuleInputEnabled);
				FImGuiModule::Get().GetProperties().SetKeyboardInputShared(bPrevImGuiKeyboardShared);
			}

			bInputLockedForImGui = false;
		}

		if (GEngine)
		{
			GEngine->AddOnScreenDebugMessage(-1, 2.0f, FColor::Yellow, TEXT("ImGui mode ended: all windows closed."));
		}
	}

	// ImGui がマウスを要求しているか
	ImGuiIO& IO = ImGui::GetIO();
	bool bWantMouse = IO.WantCaptureMouse;

	bool bAnyWindowOpen2 = bAnyWindowOpen;
	// 基本判定（以前の条件を維持）
	bool bBaseWant = bWantMouse || bGlobalVisibility || bAnyWindowOpen2;

	// トグル状態により強制する（false にすればゲームへの入力を優先）
	bool bWantMouseEffective = bInputModeIsImGui ? bBaseWant : false;

	if (PC)
	{
		if (bWantMouseEffective && !bInputLockedForImGui)
		{
			// 保存して UI 用に切り替え
			bPrevShowMouseCursor = PC->bShowMouseCursor;
			bPrevEnableClickEvents = PC->bEnableClickEvents;
			bPrevEnableMouseOverEvents = PC->bEnableMouseOverEvents;

			PC->SetShowMouseCursor(true);
			PC->bEnableClickEvents = true;
			PC->bEnableMouseOverEvents = true;

			FInputModeGameAndUI InputMode;
			InputMode.SetLockMouseToViewportBehavior(EMouseLockMode::DoNotLock);
			PC->SetInputMode(InputMode);

			PC->SetIgnoreLookInput(true);
			PC->SetIgnoreMoveInput(true);

			// ImGui モジュールの入力を有効化（SImGuiWidget が入力を受け取れるようにする）
			if (FImGuiModule::IsAvailable())
			{
				bPrevImGuiModuleInputEnabled = FImGuiModule::Get().GetProperties().IsInputEnabled();
				FImGuiModule::Get().GetProperties().SetInputEnabled(true);

				// キーボードイベントが PlayerController に届くよう共有を有効にする
				bPrevImGuiKeyboardShared = FImGuiModule::Get().GetProperties().IsKeyboardInputShared();
				FImGuiModule::Get().GetProperties().SetKeyboardInputShared(true);
			}

			bInputLockedForImGui = true;
		}
		else if (!bWantMouseEffective && bInputLockedForImGui)
		{
			// 復元（上と同じ処理）
			PC->SetShowMouseCursor(bPrevShowMouseCursor);
			PC->bEnableClickEvents = bPrevEnableClickEvents;
			PC->bEnableMouseOverEvents = bPrevEnableMouseOverEvents;

			FInputModeGameOnly GameOnly;
			PC->SetInputMode(GameOnly);

			PC->SetIgnoreLookInput(false);
			PC->SetIgnoreMoveInput(false);

			// ImGui モジュールの入力状態を復元
			if (FImGuiModule::IsAvailable())
			{
				FImGuiModule::Get().GetProperties().SetInputEnabled(bPrevImGuiModuleInputEnabled);
				FImGuiModule::Get().GetProperties().SetKeyboardInputShared(bPrevImGuiKeyboardShared);
			}

			bInputLockedForImGui = false;
		}
	}
#endif
}

void UImGuiGameInstanceSubsystem::OpenWindow(const FString& Title)
{
	for (auto& Window : RegisteredWindows)
	{
		if (Window.Title == Title)
		{
			Window.bIsOpen = true;
			return;
		}
	}
	// 未登録の場合は何もしない
}

void UImGuiGameInstanceSubsystem::CloseWindow(const FString& Title)
{
	for (auto& Window : RegisteredWindows)
	{
		if (Window.Title == Title)
		{
			Window.bIsOpen = false;
			return;
		}
	}
	// 見つからなければ何もしない
}

void UImGuiGameInstanceSubsystem::ToggleWindow(const FString& Title)
{
	for (auto& Window : RegisteredWindows)
	{
		if (Window.Title == Title)
		{
			Window.bIsOpen = !Window.bIsOpen;
			return;
		}
	}
	// 未登録の場合は何もしない
}

#if WITH_IMGUI
bool UImGuiGameInstanceSubsystem::LoadAndRegisterFont(const FString& ContentRelativePath, const FName& FontName, float SizePixels)
{
	const FString FullPath = FPaths::Combine(FPaths::ProjectContentDir(), ContentRelativePath);
	if (!FPlatformFileManager::Get().GetPlatformFile().FileExists(*FullPath))
	{
		UE_LOG(LogTemp, Warning, TEXT("ImGui: Font file not found: %s"), *FullPath);
		return false;
	}

	TArray<uint8> FileBuffer;
	if (!FFileHelper::LoadFileToArray(FileBuffer, *FullPath))
	{
		UE_LOG(LogTemp, Warning, TEXT("ImGui: Failed to load font file: %s"), *FullPath);
		return false;
	}

	// バッファを保持して lifetime を確保
	CustomFontBuffers.Add(FontName, MoveTemp(FileBuffer));
	TArray<uint8>& StoredBuffer = CustomFontBuffers[FontName];

	// ImFontConfig をデフォルト構築で初期化
	TSharedPtr<ImFontConfig> FontConfig = MakeShared<ImFontConfig>();
	*FontConfig = ImFontConfig();

	FontConfig->FontData = (void*)StoredBuffer.GetData();
	FontConfig->FontDataSize = static_cast<int>(StoredBuffer.Num());
	FontConfig->FontDataOwnedByAtlas = false; // 自前で保持する
	FontConfig->SizePixels = SizePixels;

	// 日本語グリフ範囲を使用
	ImGuiIO& IO = ImGui::GetIO();
	FontConfig->GlyphRanges = IO.Fonts->GetGlyphRangesJapanese();

	// 名前を安全にコピー
	FCStringAnsi::Strncpy(FontConfig->Name, TCHAR_TO_ANSI(*FontName.ToString()), sizeof(FontConfig->Name));

	// 登録
	FImGuiModule::Get().GetProperties().AddCustomFont(FontName, FontConfig);
	CustomFontConfigs.Add(FontName, FontConfig);

	// 再ビルド要求
	if (FImGuiModule::IsAvailable())
	{
		FImGuiModule::Get().RebuildFontAtlas();
	}

	// 登録したフォント名を保持（後でデフォルト化に使う）
	RegisteredJPFontName = FontName;
	bImGuiDefaultFontSet = false; // 再設定を許可

	return true;
}

void UImGuiGameInstanceSubsystem::EnsureDefaultImGuiFontIfNeeded()
{
	if (bImGuiDefaultFontSet || RegisteredJPFontName == NAME_None) return;

	ImFontAtlas* Atlas = ImGui::GetIO().Fonts;
	if (!Atlas) return;

	const char* TargetName = TCHAR_TO_ANSI(*RegisteredJPFontName.ToString());

	for (int i = 0; i < Atlas->Fonts.Size; ++i)
	{
		ImFont* F = Atlas->Fonts[i];
		if (!F) continue;

		const char* DebugName = F->GetDebugName();
		if (!DebugName) continue;

		if (FCStringAnsi::Strcmp(DebugName, TargetName) == 0)
		{
			ImGui::GetIO().FontDefault = F;
			bImGuiDefaultFontSet = true;
			return;
		}
	}
}
#endif

void UImGuiGameInstanceSubsystem::RenderDebugMenu()
{

#if WITH_IMGUI
	ImGui::Text("ヒットポイント: %d / %d", 15, 30);
	ImGui::SliderFloat("Speed Multiplier", &this->CustomSpeedScale, 0.5f, 3.0f);
#endif
}
```
</div>

### 各システムからのウィンドウ登録と利用方法
このマネージャーが完成したら、各アクタ（プレイヤー、敵、ステージ管理者など）は自身のデバッグ用UIをマネージャーに「登録（丸投げ）」するだけで良くなります。マネージャクラスに実装されたOpenWindow,CloseWindow,ToggleWindowで、指定したタイトルのウィンドウのオープン/クローズ制御が可能です。

<div style="background-color: #333; color: #fff; padding: 6px 12px; font-family: monospace; font-size: 13px; border-top-left-radius: 6px; border-top-right-radius: 6px; border-bottom: 1px solid #444; font-weight: bold;">
  MyCharacter.cpp
</div>
<div style="max-height: 300px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 5px; background-off: #f9f9f9;">

```cpp
void AMyCharacter::BeginPlay()
{
    Super::BeginPlay();

    // サブシステムを取得
    if (UGameInstance* GI = GetGameInstance())
    {
        if (UImGuiDebugManager* DebugManager = GI->GetSubsystem<UImGuiDebugManager>())
        {
            // ラムダ式を使って、このクラスのステータスを描画するウィンドウを登録
            DebugManager->RegisterWindow(TEXT("Player Status"), [this]()
            {
#if WITH_IMGUI
                // プレイヤーの変数を表示
                ImGui::Text("HP: %d / %d", this->CurrentHP, this->MaxHP);
                ImGui::SliderFloat("Speed Multiplier", &this->CustomSpeedScale, 0.5f, 3.0f);
#endif
            }, true); // 初期状態で開く
        }
    }
}
```
</div>
