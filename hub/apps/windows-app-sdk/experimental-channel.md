---
title: Latest experimental channel release notes for the Windows App SDK
description: Learn about the latest experimental releases of the Windows App SDK.
ms.topic: article
ms.date: 04/25/2024
keywords: windows win32, windows app development, project reunion, experimental, windows app sdk
ms.localizationpriority: medium
---

# Latest experimental channel release notes for the Windows App SDK

> [!IMPORTANT]
> The experimental channel is **not supported** for use in production environments, and apps that use the experimental releases cannot be published to the Microsoft Store.

The experimental channel includes releases of the Windows App SDK with [experimental channel features](release-channels.md#features-available-by-release-channel) in early stages of development. APIs for experimental features have the [Experimental](/uwp/api/Windows.Foundation.Metadata.ExperimentalAttribute) attribute. If you call an experimental API in your code, you will receive a build-time warning. All APIs in the experimental channel are subject to extensive revisions and breaking changes. Experimental features and APIs may be removed from subsequent releases at any time.

**Important links**:

- If you'd like to upgrade an existing app from an older version of the Windows App SDK to a newer version, see [Update existing projects to the latest release of the Windows App SDK](update-existing-projects-to-the-latest-release.md).
- For documentation on experimental releases, see [Install tools for preview and experimental channels of the Windows App SDK](preview-experimental-install.md).

**Experimental channel release note archive:**

- [Experimental channel release notes for the Windows App SDK 1.6](release-notes-archive/experimental-channel-1.6.md)
- [Experimental channel release notes for the Windows App SDK 1.5](release-notes-archive/experimental-channel-1.5.md)
- [Experimental channel release notes for the Windows App SDK 1.4](release-notes-archive/experimental-channel-1.4.md)
- [Experimental channel release notes for the Windows App SDK 1.3](release-notes-archive/experimental-channel-1.3.md)
- [Experimental channel release notes for the Windows App SDK 1.2](release-notes-archive/experimental-channel-1.2.md)
- [Experimental channel release notes for the Windows App SDK 1.0](release-notes-archive/experimental-channel-1.0.md)
- [Experimental channel release notes for the Windows App SDK 0.8](release-notes-archive/experimental-channel-0.8.md)

## Version 1.7 Experimental (1.7.0-experimental1)

This is the latest release of the experimental channel.

To download, retarget your WinAppSDK NuGet version to `1.7.241114004-experimental1`.

### New CameraCaptureUI API

A new CameraCaptureUI API makes it easier to capture photos and videos in your WinAppSDK app. For more info, see GitHub issue [#4721](https://github.com/microsoft/WindowsAppSDK/issues/4721).

### New Authentication API

A new `OAuth2Manager` API provides a streamlined solution for web authentication, offering OAuth 2.0 capabilities with full feature parity across all Windows platforms supported by WinAppSDK. For more info, see GitHub issue [#4772](https://github.com/microsoft/WindowsAppSDK/issues/4772).

### New Background Task support

A new `BackgroundTaskBuilder` API brings integrated support for background task registration to your WinAppSDK apps. For more info, see GitHub issue [#4822](https://github.com/microsoft/WindowsAppSDK/issues/4822).

### New APIs for 1.7-experimental1

This release includes the following new and modified experimental APIs:

```C#
Microsoft.Security.Authentication.OAuth

    AuthFailure
    AuthRequestParams
    AuthRequestResult
    AuthResponse
    ClientAuthentication
    CodeChallengeMethodKind
    OAuth2Manager
    OAuthContract
    TokenFailure
    TokenFailureKind
    TokenRequestParams
    TokenRequestResult
    TokenResponse
```

```C#
Microsoft.UI.Composition

    CompositionNotificationDeferral
    CompositionProjectedShadow
        MaxOpacity
        MinOpacity
        OpacityFalloff

    CompositionProjectedShadowCaster
        AncestorClip
        Mask

    CompositionProjectedShadowDrawOrder
    CompositionProjectedShadowReceiver
        DrawOrder
        Mask
```

```C#
Microsoft.UI.Composition.Experimental

    ExpCompositionVisualSurface
    ExpExpressionNotificationProperty
    IExpCompositionPropertyChanged
    IExpCompositionPropertyChangedListener
    IExpCompositor
    IExpVisual
```

```C#
Microsoft.UI.Content

    AutomationOptions
    ChildContentLink
    ContentAppWindowBridge
    ContentDisplayOrientations
    ContentEnvironmentStateChangedEventArgs
        DidDisplayScaleChange

    ContentExternalBackdropLink
    ContentExternalOutputLink
    ContentIsland
        Children
        Compositor
        Connected
        ConnectionInfo
        ConnectRemoteEndpoint
        Create
        Disconnected
        FindAllForCompositor
        FragmentRootAutomationProvider
        GetByVisual
        IsRemoteEndpointConnected
        NextSiblingAutomationProvider
        ParentAutomationProvider
        PreviousSiblingAutomationProvider
        Root
        TransformMatrix

    ContentIslandEnvironment
        AutomationOption
        CurrentOrientation
        DisplayScale
        NativeOrientation
        ThemeChanged

    ContentSite
        Compositor
        SetContentNodeParent
        SetIsInputPassThrough
        SiteVisual
        TransformMatrix
        TryGetAutomationProvider

    ContentSiteAutomationProviderRequestedEventArgs
    ContentSiteEnvironment
        CurrentOrientation
        DisplayScale
        NativeOrientation
        NotifyThemeChanged

    ContentSiteView
        TransformMatrix

    CoreWindowSiteBridge
    CoreWindowTopLevelWindowBridge
    DesktopChildSiteBridge
        AcceptRemoteEndpoint
        ConnectionInfo
        IsRemoteEndpointConnected
        RemoteEndpointConnecting
        RemoteEndpointDisconnected
        RemoteEndpointRequestedStateChanged

    DesktopSiteBridge
        TryCreatePopupSiteBridge

    EndpointConnectionEventArgs
    EndpointRequestedStateChangedEventArgs
    IContentIslandEndpointConnectionPrivate
    IContentLink
    IContentNodeOwner
    IContentSiteBridge2
    IContentSiteBridgeAutomation
    IContentSiteBridgeEndpointConnectionPrivate
    PopupWindowSiteBridge
    ProcessStarter
    ReadOnlyDesktopSiteBridge
    SystemVisualSiteBridge
```

```C#
Microsoft.UI.Input

    InputKeyboardSource
        GetForWindowId

    InputLayoutPolicy
    InputLightDismissAction
        GetForIsland

    InputPointerActivationBehavior
    InputPointerSource
        ActivationBehavior
        DirectManipulationHitTest
        GetForVisual
        GetForWindowId
        RemoveForVisual
        TouchHitTesting
        TrySetDeviceKinds

    ProximityEvaluation
    TouchHitTestingEventArgs
```

```C#
Microsoft.UI.Input.Experimental

    ExpInputSite
    ExpPointerPoint
```

```C#
Microsoft.UI.Windowing

    AppWindow
        DefaultTitleBarShouldMatchAppModeTheme

    DisplayArea
        GetMetricsFromWindowId
```

```C#
Microsoft.UI.Xaml

    XamlIsland
    XamlRoot
        TryGetContentIsland
```

```C#
Microsoft.UI.Xaml.Automation.Peers

    AutomationEvents
        Notification

    InkCanvasAutomationPeer
    PagerControlAutomationPeer
```

```C#
Microsoft.UI.Xaml.Controls

    ContentDialogPlacement
        UnconstrainedPopup

    DoInkPresenterWork
    ElementFactory
    FlowLayout
    FlowLayoutAnchorInfo
    FlowLayoutLineAlignment
    FlowLayoutState
    IApplicationViewSpanningRects
    IndexPath
    InkCanvas
    ISelfPlayingAnimatedVisual
    ItemContainer
        CanUserInvoke
        CanUserInvokeProperty
        CanUserSelect
        CanUserSelectProperty
        ItemInvoked
        MultiSelectMode
        MultiSelectModeProperty

    ItemContainerInteractionTrigger
    ItemContainerInvokedEventArgs
    ItemContainerMultiSelectMode
    ItemContainerUserInvokeMode
    ItemContainerUserSelectMode
    LayoutPanel
    NumberBox
        InputScope
        InputScopeProperty
        TextAlignment
        TextAlignmentProperty

    PagerControl
    PagerControlButtonVisibility
    PagerControlDisplayMode
    PagerControlSelectedIndexChangedEventArgs
    PagerControlTemplateSettings
    ProgressRing
        DeterminateSource
        DeterminateSourceProperty
        IndeterminateSource
        IndeterminateSourceProperty

    RecyclePool
    RecyclingElementFactory
    ScrollingScrollStartingEventArgs
    ScrollingZoomStartingEventArgs
    ScrollView
        ScrollStarting
        ZoomStarting

    SelectionModel
    SelectionModelChildrenRequestedEventArgs
    SelectionModelSelectionChangedEventArgs
    SelectTemplateEventArgs
    StackLayout
        IsVirtualizationEnabled
        IsVirtualizationEnabledProperty

    StackLayoutState
    TitleBar
    TitleBarAutomationPeer
    TitleBarTemplateSettings
    UniformGridLayoutState
```

```C#
Microsoft.UI.Xaml.Controls.Primitives

    ScrollPresenter
        ScrollStarting
        ZoomStarting
```

```C#
Microsoft.Windows.ApplicationModel.Background

    BackgroundTaskBuilder
    BackgroundTaskContract
```

```C#
Microsoft.Windows.ApplicationModel.Background.UniversalBGTask

    Task
```

```C#
Microsoft.Windows.ApplicationModel.WindowsAppRuntime

    DeploymentManager
        Repair

    DeploymentStatus
        PackageRepairFailed

    ReleaseInfo
    RuntimeInfo
    VersionInfoContract
```

```C#
Microsoft.Windows.Media.Capture

    CameraCaptureUI
    CameraCaptureUIContract
    CameraCaptureUIMaxPhotoResolution
    CameraCaptureUIMaxVideoResolution
    CameraCaptureUIMode
    CameraCaptureUIPhotoCaptureSettings
    CameraCaptureUIPhotoFormat
    CameraCaptureUIVideoCaptureSettings
    CameraCaptureUIVideoFormat
```

```C#
Microsoft.Windows.Storage

    ApplicationData
        GetForUnpackaged
```

### Bug fixes

This release includes the following bug fixes:

- Changed `SplitButton` so touch input now matches the behavior of mouse input. For more info, see GitHub issue [#178](https://github.com/microsoft/microsoft-ui-xaml/issues/178).
- Changed cascading menus so sub menus now open immediately if clicked. For more info, see GitHub issue [#939](https://github.com/microsoft/microsoft-ui-xaml/issues/939).
- Fixed an issue where opening a `ComboBox` which is in a flyout closes all flyouts. For more info, see GitHub issue [#1467](https://github.com/microsoft/microsoft-ui-xaml/issues/1467).
- Fixed an issue where `SwipeControl` would randomly crash in a `ListView`. For more info, see GitHub issue [#2527](https://github.com/microsoft/microsoft-ui-xaml/issues/2527).
- Fixed an issue where drag-and-drop only a `ListViewItem` would leave it in the wrong visual state. For more info, see GitHub issue [#3458](https://github.com/microsoft/microsoft-ui-xaml/issues/3458).
- Fixed an issue in `StackLayout` so that it respects the ItemsRepeater.HorizontalAlignment and ItemsRepeater.VerticalAlignment properties (when StackLayout.Orientation is Vertical and Horizontal respectively). The old layout behaved as if the ItemsRepeater alignment was Stretch. With the fix, the layout results in items aligned to the right when the Right alignment is used, for example. For more info, see GitHub issue [#3842](https://github.com/microsoft/microsoft-ui-xaml/issues/3842).
- Fixed an issue where deleting items in the `ItemsRepeater`'s source would not generate items which moved up into view. For more info, see GitHub issue [#6661](https://github.com/microsoft/microsoft-ui-xaml/issues/6661).
- Fixed an issue where the right Alt key would not show keytips for Access Keys. For more info, see GitHub issue [#8447](https://github.com/microsoft/microsoft-ui-xaml/issues/8447). **Note:** This may result in key events for the right Alt key no longer being delivered to handles in the app or controls. 
- Fixed a crash where `UniformGridLayout` would sometimes pick a wrong layout anchor and cause infinite layout passes when scrolling backwards. For more info, see GitHub issue [#9199](https://github.com/microsoft/microsoft-ui-xaml/issues/9199).
- Fixed an issue where setting `NavigationFailedEventArgs.Handled` to True would still throw an exception. For more info, see GitHub issue [#9632](https://github.com/microsoft/microsoft-ui-xaml/issues/9632).
- Fixed an issue where `TabView` would not apply any specified `CornerRadius`. For more info, see GitHub issue [#9846](https://github.com/microsoft/microsoft-ui-xaml/issues/9846).
- Fixed a potential layout cycle crash in `StackLayout`. For more info, see GitHub issue [#9852](https://github.com/microsoft/microsoft-ui-xaml/issues/9852).
- Fixed a potential crash in `ItemsView` when removing items. For more info, see GitHub issue [#9868](https://github.com/microsoft/microsoft-ui-xaml/issues/9868).

## Related topics

- [Stable channel](stable-channel.md)
- [Preview channel](preview-channel.md)
- [Install tools for the Windows App SDK](set-up-your-development-environment.md)
- [Create your first WinUI 3 project](../winui/winui3/create-your-first-winui3-app.md)
- [Use the Windows App SDK in an existing project](use-windows-app-sdk-in-existing-project.md)
- [Deploy apps that use the Windows App SDK](../package-and-deploy/index.md#use-the-windows-app-sdk)
