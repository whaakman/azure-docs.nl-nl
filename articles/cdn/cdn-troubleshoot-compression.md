---
title: In Azure CDN-bestandscompressie oplossen | Microsoft Docs
description: Problemen met Azure CDN-bestandscompressie oplossen.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: a6624e65-1a77-4486-b473-8d720ce28f8b
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 2a41316eadb43145628d6c625935c751bfbc6ad6
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57531521"
---
# <a name="troubleshooting-cdn-file-compression"></a>Problemen met CDN-bestandscompressie oplossen
Dit artikel helpt u problemen oplossen met [CDN-bestandscompressie](cdn-improve-performance.md).

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op [het Azure MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning voor Azure site](https://azure.microsoft.com/support/options/) en klikt u op **ontvang ondersteuning**.

## <a name="symptom"></a>Symptoom
Compressie voor het eindpunt is ingeschakeld, maar de bestanden niet-gecomprimeerde worden geretourneerd.

> [!TIP]
> Als u wilt controleren of uw bestanden worden geretourneerd gecomprimeerde, moet u een hulpprogramma zoals [Fiddler](https://www.telerik.com/fiddler) of van uw browser [hulpprogramma's voor ontwikkelaars](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).  Controleer de HTTP-antwoordheaders geretourneerd met uw cache CDN-inhoud.  Als er een header genaamd wordt `Content-Encoding` met een waarde van **gzip**, **bzip2**, of **deflate**, uw inhoud wordt gecomprimeerd.
> 
> ![Header Content-Encoding](./media/cdn-troubleshoot-compression/cdn-content-header.png)
> 
> 

## <a name="cause"></a>Oorzaak
Er zijn verschillende mogelijke oorzaken, met inbegrip van:

* De aangevraagde inhoud is niet in aanmerking komen voor compressie.
* Compressie is niet ingeschakeld voor het aangevraagde type.
* De HTTP-aanvraag bevat geen koptekst aanvragen van een ongeldig compressietype.

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing
> [!TIP]
> Net als bij het implementeren van nieuwe eindpunten, even CDN configuratiewijzigingen worden doorgegeven via het netwerk.  Normaal gesproken worden wijzigingen toegepast binnen 90 minuten.  Als dit de eerste keer dat u compressie voor uw CDN-eindpunt hebt ingesteld, moet u rekening houden met 1 tot 2 uur wachten om er zeker van te zijn dat de instellingen zijn doorgegeven aan de POP's compressie. 
> 
> 

### <a name="verify-the-request"></a>Controleer of de aanvraag
Eerst moeten we een snelle controle op de aanvraag doen.  U kunt van uw browser [hulpprogramma's voor ontwikkelaars](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) om de aanvragen weer te geven.

* Controleer of de aanvraag wordt verzonden naar uw eindpunt-URL, `<endpointname>.azureedge.net`, en niet de oorsprong.
* Controleer of de aanvraag bevat een **Accept-Encoding** -header en de waarde voor die header bevat **gzip**, **deflate**, of **bzip2**.

> [!NOTE]
> **Azure CDN van Akamai** profielen alleen ondersteuning voor **gzip** codering.
> 
> 

![CDN-aanvraagheaders](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profiles"></a>Controleer of de compressie-instellingen (standard CDN-profielen)
> [!NOTE]
> Deze stap geldt alleen als uw CDN-profiel een **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Verizon**, of **Azure CDN Standard van Akamai** profiel. 
> 
> 

Navigeer naar het eindpunt in de [Azure-portal](https://portal.azure.com) en klikt u op de **configureren** knop.

* Controleer of de compressie is ingeschakeld.
* Controleer of het MIME-type voor de inhoud wordt gecomprimeerd is opgenomen in de lijst met gecomprimeerde indelingen.

![CDN compressie-instellingen](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profiles"></a>Controleer of de compressie-instellingen (Premium CDN-profielen)
> [!NOTE]
> Deze stap geldt alleen als uw CDN-profiel een **Azure CDN Premium van Verizon** profiel.
> 
> 

Navigeer naar het eindpunt in de [Azure-portal](https://portal.azure.com) en klikt u op de **beheren** knop.  De aanvullende portal wordt geopend.  Beweeg de muisaanwijzer over de **HTTP grote** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **Cache-instellingen** flyout.  Klik op **compressie**. 

* Controleer of de compressie is ingeschakeld.
* Controleer of de **bestandstypen** lijst bevat een lijst met door komma's gescheiden (zonder spaties) van het MIME-typen.
* Controleer of het MIME-type voor de inhoud wordt gecomprimeerd is opgenomen in de lijst met gecomprimeerde indelingen.

![CDN premium compressie-instellingen](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached-verizon-cdn-profiles"></a>Controleer of de inhoud in de cache (Verizon CDN-profielen)
> [!NOTE]
> Deze stap geldt alleen als uw CDN-profiel een **Azure CDN Standard van Verizon** of **Azure CDN Premium van Verizon** profiel.
> 
> 

Controleer de antwoordheaders om te controleren of dat het bestand is in de cache opgeslagen in de regio waar deze wordt aangevraagd met hulpprogramma's voor ontwikkelaars van uw browser.

* Controleer de **Server** response-header.  De header moet de indeling hebben **Platform (POP-server/Server-ID)**, zoals te zien is in het volgende voorbeeld.
* Controleer de **X-Cache** response-header.  De header moet lezen **bereikt**.  

![CDN-antwoordheaders](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements-verizon-cdn-profiles"></a>Controleer of het bestand voldoet aan de vereiste grootte (Verizon CDN-profielen)
> [!NOTE]
> Deze stap geldt alleen als uw CDN-profiel een **Azure CDN Standard van Verizon** of **Azure CDN Premium van Verizon** profiel.
> 
> 

Een bestand moet voldoen aan de volgende grootte vereisten om in aanmerking komen voor compressie:

* Groter zijn dan 128 bytes.
* Kleiner dan 1 MB.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Controleer de aanvraag op de oorspronkelijke server voor een **Via** koptekst
De **Via** HTTP-header geeft aan dat de webserver dat de aanvraag wordt doorgegeven via een proxyserver.  Microsoft IIS-webservers standaard antwoorden niet gecomprimeerd wanneer de aanvraag bevat een **Via** header.  Als u wilt dit gedrag negeren, voert u het volgende:

* **IIS 6**: [Stel HcNoCompressionForProxies = "FALSE" in de IIS Metabase-eigenschappen](https://msdn.microsoft.com/library/ms525390.aspx)
* **IIS 7 en hoger**: [Stel zowel **noCompressionForHttp10** en **noCompressionForProxies** op False in de configuratie van de server](http://www.iis.net/configreference/system.webserver/httpcompression)

