---
title: Naslag Gids voor insluitende lezers SDK
titleSuffix: Azure Cognitive Services
description: Naslag informatie voor de insluitende lezer-SDK
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 1d9fc20055fe3adb571b5a77330cc6537998cb5f
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69534481"
---
# <a name="immersive-reader-sdk-reference"></a>Naslag Gids voor insluitende lezers SDK

De insluitende lezer SDK is een Java script-bibliotheek waarmee u de insluitende lezer kunt integreren in uw webtoepassing.

## <a name="functions"></a>Functies

De SDK stelt één functie `ImmersiveReader.launchAsync(token, subdomain, content, options)`beschikbaar.

### <a name="launchasync"></a>launchAsync

Hiermee start u de insluitende `iframe` lezer binnen een in uw webtoepassing.

```typescript
launchAsync(token: string, subdomain: string, content: Content, options?: Options): Promise<HTMLDivElement>;
```

#### <a name="parameters"></a>Parameters

| Name | Type | Description |
| ---- | ---- |------------ |
| `token` | string | Het Azure AD-verificatie token. Zie de [instructies voor Azure AD-verificatie](./azure-active-directory-authentication.md). |
| `subdomain` | string | Het aangepaste subdomein van uw insluitende lezer-resource in Azure. Zie de [instructies voor Azure AD-verificatie](./azure-active-directory-authentication.md). |
| `content` | [Inhoud](#content) | Een object met de inhoud die in de insluitende lezer moet worden weer gegeven. |
| `options` | [Opties](#options) | Opties voor het configureren van bepaald gedrag van de insluitende lezer. Optioneel. |

#### <a name="returns"></a>Retourneert

Retourneert een `Promise<HTMLDivElement>` die wordt omgezet wanneer de insluitende lezer wordt geladen. De `Promise` wordt omgezet in een `div` -element `iframe` waarvan alleen een onderliggend element de pagina insluitende lezer bevat.

#### <a name="exceptions"></a>Uitzonderingen

De geretourneerde `Promise` wordt met een [`Error`](#error) object afgewezen als de insluitende lezer niet kan worden geladen. Zie de [fout codes](#error-codes)voor meer informatie.

## <a name="types"></a>Typen

### <a name="content"></a>Inhoud

Bevat de inhoud die in de insluitende lezer moet worden weer gegeven.

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

| MIME-type | Description |
| --------- | ----------- |
| tekst/zonder opmaak | Tekst zonder opmaak. |
| Application/MathML + XML | MathML (wiskundige Markup Language). [Meer informatie](https://developer.mozilla.org/en-US/docs/Web/MathML).
| application/vnd. openxmlformats-officedocument. WordprocessingML. document | Micro soft Word. docx-indelings document.

### <a name="options"></a>Opties

Bevat eigenschappen die bepaald gedrag van de insluitende lezer configureren.

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
| BadArgument | Het opgegeven argument is ongeldig. `message` Zie voor meer informatie. |
| Time-out | De insluitende lezer kan niet worden geladen binnen de opgegeven time-out. |
| TokenExpired | Het opgegeven token is verlopen. |
| Beperkt | De limiet voor de aanroep frequentie is overschreden. |

## <a name="launching-the-immersive-reader"></a>De insluitende lezer starten

De SDK biedt standaard stijlen voor de knop voor het starten van de insluitende lezer. Gebruik het `immersive-reader-button` kenmerk Class om deze stijl in te scha kelen.

```html
<div class='immersive-reader-button'></div>
```

### <a name="optional-attributes"></a>Optionele kenmerken

Gebruik de volgende kenmerken om het uiterlijk van de knop te configureren.

| Kenmerk | Description |
| --------- | ----------- |
| `data-button-style` | Hiermee wordt de stijl van de knop ingesteld. `icon`Kan, `text`of. `iconAndText` Standaard ingesteld op `icon`. |
| `data-locale` | Hiermee stelt u de land instelling `en-US`, `fr-FR`bijvoorbeeld,. Wordt standaard ingesteld op Engels. |
| `data-icon-px-size` | Hiermee stelt u de grootte van het pictogram in pixels. De standaard waarde is 20px. |

## <a name="browser-support"></a>Browser ondersteuning

Gebruik de meest recente versies van de volgende browsers voor de beste ervaring met de insluitende lezer.

* Microsoft Edge
* Internet Explorer 11
* Google Chrome
* Mozilla Firefox
* Apple Safari

## <a name="next-steps"></a>Volgende stappen

* De insluitende [Lezer-SDK op github](https://github.com/microsoft/immersive-reader-sdk) verkennen
* [Snelstart: Een web-app maken waarmee de insluitende lezerC#wordt gestart ()](./quickstart.md)