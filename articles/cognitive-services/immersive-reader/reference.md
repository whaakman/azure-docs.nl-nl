---
title: Naslaginformatie over boeiende Reader-SDK
titleSuffix: Azure Cognitive Services
description: Naslaginformatie voor de overweldigende lezer SDK
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 485e8626af4266492e02d4f9fbe4af486e10c082
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718385"
---
# <a name="immersive-reader-sdk-reference"></a>Naslaginformatie over boeiende Reader-SDK

De overweldigende Reader-SDK is een JavaScript-bibliotheek waarmee u de overweldigende lezer integreren in uw webtoepassing.

## <a name="functions"></a>Functions

De SDK wordt aangegeven dat een enkele functie `ImmersiveReader.launchAsync(token, resourceName, content, options)`.

### <a name="launchasync"></a>launchAsync

Start de overweldigende lezer binnen een `iframe` in uw webtoepassing.

```typescript
launchAsync(token: string, resourceName: string, content: Content, options?: Options): Promise<HTMLDivElement>;
```

#### <a name="parameters"></a>Parameters

| Name | Type | Description |
| ---- | ---- |------------ |
| `token` | string | Het toegangstoken verkregen van de aanroep aan de `issueToken` eindpunt. |
| `resourceName` | string | Gereserveerd. Moet worden ingesteld op `null`. |
| `content` | [Inhoud](#content) | Een object met de inhoud in de overweldigende lezer moet worden weergegeven. |
| `options` | [Opties](#options) | Opties voor het configureren van bepaald gedrag van de overweldigende lezer. Optioneel. |

#### <a name="returns"></a>Geeft als resultaat

Retourneert een `Promise<HTMLDivElement>` die wordt omgezet wanneer de overweldigende lezer wordt geladen. De `Promise` wordt omgezet naar een `div` waarvan alleen onderliggende element een `iframe` element dat de pagina boeiende lezer bevat.

#### <a name="exceptions"></a>Uitzonderingen

De geretourneerde `Promise` geweigerd met een [ `Error` ](#error) object als de overweldigende lezer niet laden. Zie voor meer informatie de [foutcodes](#error-codes).

## <a name="types"></a>Types

### <a name="content"></a>Inhoud

De inhoud moet worden weergegeven in de overweldigende Reader bevat.

```typescript
{
    title?: string;      // Title text shown at the top of the Immersive Reader (optional)
    chunks: [ {          // Array of chunks
        content: string; // Plain text string
        lang?: string;   // Language of the text, e.g. en, es-ES (optional). Language will be detected automatically if not specified.
        mimeType?: string; // MIME type of the content (optional). Defaults to 'text/plain' if not specified.
    } ];
}
```

#### <a name="supported-mime-types"></a>Ondersteunde MIME-typen

| MIME-Type | Description |
| --------- | ----------- |
| text/plain | Tekst zonder opmaak. |
| toepassing/mathml + xml | Wiskundige Markup Language (MathML). [Meer informatie](https://developer.mozilla.org/en-US/docs/Web/MathML).

### <a name="options"></a>Opties

Bevat de eigenschappen die bepaald gedrag van de overweldigende lezer configureren.

```typescript
{
    uiLang?: string;   // Language of the UI, e.g. en, es-ES (optional). Defaults to browser language if not specified.
    timeout?: number;  // Duration (in milliseconds) before launchAsync fails with a timeout error (default is 15000 ms).
    uiZIndex?: number; // Z-index of the iframe that will be created (default is 1000)
    useWebview?: boolean; // Use a webview tag instead of an iframe, for compatibility with Chrome Apps (default is false).
}
```

### <a name="error"></a>Fout

Bevat informatie over de fout.

```typescript
{
    code: string;    // One of a set of error codes
    message: string; // Human-readable representation of the error
}
```

#### <a name="error-codes"></a>Foutcodes

| Code | Description |
| ---- | ----------- |
| BadArgument | Opgegeven argument is ongeldig, Zie `message` voor meer informatie. |
| Time-out | De overweldigende lezer kan niet worden geladen in de opgegeven time-out. |
| TokenExpired| Het opgegeven token is verlopen. |

## <a name="launching-the-immersive-reader"></a>Starten van de overweldigende lezer

De SDK biedt standaard stijl voor de knop voor het starten van de overweldigende lezer. Gebruik de `immersive-reader-button` klassekenmerk voor het inschakelen van deze stijl.

```html
<div class='immersive-reader-button'></div>
```

### <a name="optional-attributes"></a>Optionele kenmerken

Gebruik de volgende kenmerken op het uiterlijk van de knop configureren.

| Kenmerk | Description |
| --------- | ----------- |
| `data-button-style` | Hiermee stelt u de stijl van de knop. Kan `icon`, `text`, of `iconAndText`. Standaard ingesteld op `icon`. |
| `data-locale` | Hiermee stelt u de landinstelling, bijvoorbeeld `en-US`, `fr-FR`. Standaard ingesteld op Engels. |
| `data-icon-px-size` | Hiermee stelt de grootte van het pictogram in pixels. De standaardkleur 20px. |

## <a name="browser-support"></a>Browserondersteuning

De meest recente versies van de volgende browsers gebruiken voor de beste ervaring met de overweldigende lezer.

* Microsoft Edge
* Internet Explorer 11
* Google Chrome
* Mozilla Firefox
* Apple Safari

## <a name="next-steps"></a>Volgende stappen

* Verken de [boeiende lezer SDK op GitHub](https://github.com/Microsoft/immersive-reader-sdk)
* [Snelstart: Een web-App waarmee de overweldigende lezer wordt gestart (C#)](./quickstart.md)