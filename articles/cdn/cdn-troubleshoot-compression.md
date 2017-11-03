---
title: Het oplossen van problemen bestandscompressie in Azure CDN | Microsoft Docs
description: Problemen oplossen met Azure CDN bestandscompressie.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: a6624e65-1a77-4486-b473-8d720ce28f8b
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 5ef8a8262eb40aa827161764f03a63d031e43273
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-cdn-file-compression"></a>Problemen met CDN-bestandscompressie oplossen
Dit artikel helpt u problemen oplossen met [CDN bestandscompressie](cdn-improve-performance.md).

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op [de Azure MSDN en de Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook kunt u ook een incident voor ondersteuning van Azure bestand. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en klik op **Get Support**.

## <a name="symptom"></a>Symptoom
Compressie voor uw eindpunt is ingeschakeld, maar de bestanden niet-gecomprimeerde worden geretourneerd.

> [!TIP]
> Om te controleren of de bestanden worden geretourneerd gecomprimeerde, die u wilt gebruiken, een hulpprogramma zoals [Fiddler](http://www.telerik.com/fiddler) of uw browser [hulpprogramma's voor ontwikkelaars](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).  Controleer de HTTP-antwoordheaders geretourneerd met uw cache CDN-inhoud.  Als er een header met de naam is `Content-Encoding` met een waarde van **gzip**, **bzip2**, of **deflate**, uw inhoud wordt gecomprimeerd.
> 
> ![Inhoud-Encoding-header](./media/cdn-troubleshoot-compression/cdn-content-header.png)
> 
> 

## <a name="cause"></a>Oorzaak
Er zijn verschillende mogelijke oorzaken, inclusief:

* De aangevraagde inhoud is niet in aanmerking komen voor compressie.
* Compressie is niet ingeschakeld voor het aangevraagde type.
* De HTTP-aanvraag bevat geen aanvragen van een geldige compressietype header.

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing
> [!TIP]
> Net als bij het implementeren van nieuwe eindpunten configuratiewijzigingen CDN enige tijd worden doorgegeven via het netwerk.  Normaal gesproken worden wijzigingen toegepast binnen 90 minuten.  Als dit de eerste keer dat u compressie voor uw CDN-eindpunt hebt ingesteld, kunt u overwegen 1 tot 2 uur wachten om er zeker van te zijn dat de instellingen zijn doorgegeven aan de POP's compressie. 
> 
> 

### <a name="verify-the-request"></a>Controleer of de aanvraag
Er moet eerst een snelle controle van de aanvraag doen.  U kunt uw browser [hulpprogramma's voor ontwikkelaars](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) om de aanvragen weer te geven.

* Controleer of de aanvraag wordt verzonden naar uw eindpunt-URL, `<endpointname>.azureedge.net`, en niet uw oorsprong.
* Controleer of de aanvraag bevat een **Accept-Encoding** header en de waarde voor deze koptekst bevat **gzip**, **deflate**, of **bzip2**.

> [!NOTE]
> **Azure CDN van Akamai** profielen alleen ondersteuning voor **gzip** codering.
> 
> 

![CDN-aanvraagheaders](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Controleer of de compressie-instellingen (standaard CDN-profiel)
> [!NOTE]
> Deze stap is alleen van toepassing als uw CDN-profiel is een **Azure CDN Standard van Verizon** of **Azure CDN Standard van Akamai** profiel. 
> 
> 

Navigeer naar uw eindpunt in de [Azure-portal](https://portal.azure.com) en klik op de **configureren** knop.

* Controleer of compressie is ingeschakeld.
* Controleer of het MIME-type voor de inhoud wordt gecomprimeerd is opgenomen in de lijst met gecomprimeerde indelingen.

![CDN compressie-instellingen](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Controleer of de compressie-instellingen (Premium-CDN-profiel)
> [!NOTE]
> Deze stap is alleen van toepassing als uw CDN-profiel is een **Azure CDN Premium van Verizon** profiel.
> 
> 

Navigeer naar uw eindpunt in de [Azure-portal](https://portal.azure.com) en klik op de **beheren** knop.  De aanvullende portal wordt geopend.  Beweeg de muisaanwijzer over de **HTTP grote** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **Cache-instellingen** doel.  Klik op **compressie**. 

* Controleer of compressie is ingeschakeld.
* Controleer of de **bestandstypen** lijst bevat een lijst door komma's gescheiden (zonder spaties) met MIME-typen.
* Controleer of het MIME-type voor de inhoud wordt gecomprimeerd is opgenomen in de lijst met gecomprimeerde indelingen.

![CDN premium compressie-instellingen](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a>Controleer of dat de inhoud in cache wordt opgeslagen
> [!NOTE]
> Deze stap is alleen van toepassing als uw CDN-profiel is een **Azure CDN van Verizon** profiel (Standard of Premium).
> 
> 

Controleer met ontwikkelhulpprogramma's van uw browser de antwoordheaders om te controleren of dat het bestand in cache wordt opgeslagen in de regio waar deze wordt aangevraagd.

* Controleer de **Server** antwoordheader.  De header moet de indeling **Platform (ID POP-Server)**, zoals in het volgende voorbeeld.
* Controleer de **X-Cache** antwoordheader.  De header moet worden gelezen **bereikt**.  

![CDN-antwoordheaders](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a>Controleer of het bestand voldoet aan de vereiste grootte
> [!NOTE]
> Deze stap is alleen van toepassing als uw CDN-profiel is een **Azure CDN van Verizon** profiel (Standard of Premium).
> 
> 

Als u in aanmerking komen voor compressie, een bestand moet voldoen aan de volgende vereiste grootte:

* Groter zijn dan 128 bytes.
* Kleiner dan 1 MB.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Controleer de aanvraag op de bronserver voor een **Via** koptekst
De **Via** HTTP-header wordt aangegeven dat de webserver dat de aanvraag wordt doorgegeven via een proxyserver.  Microsoft IIS-webservers standaard antwoorden niet comprimeren wanneer de aanvraag bevat een **Via** header.  Als u wilt dit gedrag negeren door het volgende te doen:

* **IIS 6**: [ingesteld HcNoCompressionForProxies = 'FALSE' in de IIS-Metabase-eigenschappen](https://msdn.microsoft.com/library/ms525390.aspx)
* **IIS 7 en hoger**: [zowel **noCompressionForHttp10** en **noCompressionForProxies** op False in de serverconfiguratie](http://www.iis.net/configreference/system.webserver/httpcompression)

