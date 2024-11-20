---
title: Customize the widget header area
description: Learn how to customize the header area of a Windows widget. 
ms.topic: article
ms.date: 11/12/2024
ms.localizationpriority: medium
---

# Customize the widget header area

Starting with Windows build [TBD - Build number], apps that implement Windows widgets can customize the header that is displayed for their widget in the Widgets Board, overriding the default presentation. Header customization is implemented in the Adaptive Card payload you pass to the OS from your widget provider, so the steps are the same regardless of the language your widget provider is implemented in. For a walkthrough of creating a widget provider, see [Implement a widget provider in a C# Windows App](implement-widget-provider-cs.md) or [Implement a widget provider in a win32 app (C++/WinRT)](implement-widget-provider-win32.md).

## The default header

By default, the widget header shows the display name and the icon specified in the app manifest file. The display name is specified with the **DisplayName** attribute of the **Definition** element and the icon is specified with an **Icon** element under **ThemeResources**. For more information about the widget app manifest file format, see [Widget provider package manifest XML format](widget-provider-manifest.md).

The following example shows a portion of the Adaptive Card JSON payload for a widget that uses the default presentation. In the sections below, examples will be provided that modify this template to override the default header.

```json
{ 
    "type": "AdaptiveCard", 
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json", 
    "version": "1.6", 
    "body": [
        ...
    ] 
  } 
```

## Override the display name string

You can override the value specified in the **DisplayName** element in the app manifest by adding a `header` field to with the new display name in the JSON payload before sending it to the widget host.

The following example demonstrates overriding the display name string.

```json
{ 
    "type": "AdaptiveCard", 
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json", 
    "version": "1.6", 
    "body": [
        ...
    ] ,
    "header": "Redmond Weather"
  } 
```

## Override the display name string and icon

To override both the display name string and the icon specified in the app manifest, add a `header` object with fields for `text` and `iconUrl`.

The following example demonstrates overriding the display name string and icon.

```json
{ 
    "type": "AdaptiveCard", 
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json", 
    "version": "1.6", 
    "body": [
        ...
    ] ,
    "header": { 
         "text": "Redmond weather", 
         "iconUrl": "https://contoso.com/weatherimage.png" 
      } 
  } 
```

## Set the header to be empty

Some widget providers may want to allow their full UX to expand into the header region of the widget, even though this area of the widget isn't actionable. For this scenario, you can set the header to be empty by setting the `header` feel to `null`. Note that the UX in the header is not clickable by the user.

The following example demonstrates setting an empty header.

```json
{ 
    "type": "AdaptiveCard", 
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json", 
    "version": "1.6", 
    "body": [
        ...
    ] ,
    "header": null
} 
```

## Override the header with Adaptive Card content

You can overwrite the header with UI components defined using the Adaptive Card schema. The specified content will be rendered within the 48px high header area.

The following example uses Adaptive Card components to render a combo box selector in the widget header.

```json
{ 
    "type": "AdaptiveCard", 
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json", 
    "version": "1.6", 
    "body": [
        ...
    ] ,
    "header": [
        { 
            "type": "Input.ChoiceSet", 
            "id": "WeatherSwitcher", 
            "choices": [ 
              { 
                "title": "Redmond", 
                "value": "Redmond" 
              }, 
              { 
                "title": "Bellevue", 
                "value": "Bellevue" 
              } 
            ], 
            "placeholder": "Pick location", 
            "value": "Bellevue" 
        } 
    ]
} 
```