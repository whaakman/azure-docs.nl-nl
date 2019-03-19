---
title: De prestaties verbeteren door bestanden in Azure CDN te comprimeren | Microsoft Docs
description: Informatie over het verbeteren van bestand overdrachtssnelheid en het laden van de pagina prestaties door uw bestanden in Azure CDN te comprimeren.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: magattus
ms.openlocfilehash: afe959e80b339db5112fa97fd79d0528390e3954
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58096449"
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>De prestaties verbeteren door bestanden in Azure CDN te comprimeren
Bestandscompressie is een eenvoudige maar effectieve methode voor het verbeteren van bestand overdrachtssnelheid en het laden van de pagina prestaties van een bestand door grootte te beperken voordat deze wordt verzonden vanaf de server. Bestandscompressie kunt bandbreedtekosten verlagen en biedt een beter te laten reageren ervaring voor uw gebruikers.

Er zijn twee manieren om in te schakelen bestandscompressie:

- Compressie inschakelen op de oorspronkelijke server. In dit geval Azure CDN de gecomprimeerde bestanden doorgeeft en zorgt ervoor dat deze aan clients die deze aanvragen.
- Compressie inschakelen rechtstreeks op het CDN POP-servers (*compressie op elk gewenst moment*). In dit geval het CDN worden gecomprimeerd de bestanden en fungeert voor de eindgebruikers, zelfs als ze niet zijn gecomprimeerd door de oorspronkelijke server.

> [!IMPORTANT]
> Azure CDN-configuratiewijzigingen kunnen even worden doorgegeven via het netwerk: 
> - Voor profielen van **Azure CDN Standard van Microsoft** is het doorgeven gewoonlijk binnen 10 minuten voltooid. 
> - Profielen van **Azure CDN Standard van Akamai** worden doorgaans binnen één minuut doorgegeven. 
> - Profielen van **Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** worden normaal gesproken binnen 10 minuten doorgegeven. 
> 
> Als u compressie voor het eerst voor uw CDN-eindpunt instelt, kunt u overwegen wacht 1-2 uur voordat u om te controleren of dat de compressie-instellingen zijn doorgegeven aan de POP's oplossen.

## <a name="enabling-compression"></a>Compressie inschakelen
De lagen standard en premium CDN bieden dezelfde functionaliteit compressie, maar de gebruikersinterface wijkt af. Zie voor meer informatie over de verschillen tussen lagen voor standard en premium CDN [overzicht van Azure CDN](cdn-overview.md).

### <a name="standard-cdn-profiles"></a>Standaard CDN-profielen 
> [!NOTE]
> In deze sectie is van toepassing op **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Verizon**, en **Azure CDN Standard van Akamai** profielen.
> 
> 

1. Selecteer in het CDN-profielpagina het CDN-eindpunt dat u wilt beheren.

    ![CDN-profiel-eindpunten](./media/cdn-file-compression/cdn-endpoints.png)

    De CDN-eindpunt-pagina wordt geopend.
2. Selecteer **compressie**.

    ![Selectie van CDN-compressie](./media/cdn-file-compression/cdn-compress-select-std.png)

    De compressie-pagina wordt geopend.
3. Selecteer **op** compressie inschakelen.

    ![Opties voor compressie CDN](./media/cdn-file-compression/cdn-compress-standard.png)
4. Gebruik de standaard-MIME-typen, of wijzigt u de lijst toevoegen of verwijderen van MIME-typen.

   > [!TIP]
   > Hoewel het mogelijk is, wordt het niet aanbevolen om toe te passen van compressie naar gecomprimeerde indelingen. Bijvoorbeeld, POSTCODE, MP3, MP4 of JPG.
   > 

   > [!NOTE]
   > Wijzigt de standaardlijst met MIME-typen is momenteel niet ondersteund in Azure CDN Standard van Microsoft.
   > 

5. Wanneer u klaar bent, selecteert u **opslaan**.

### <a name="premium-cdn-profiles"></a>Premium CDN-profielen
> [!NOTE]
> In deze sectie geldt alleen voor **Azure CDN Premium van Verizon** profielen.
> 

1. Selecteer in het CDN-profielpagina **beheren**.

    ![Selecteer CDN beheren](./media/cdn-file-compression/cdn-manage-btn.png)

    De CDN-beheerportal wordt geopend.
2. Beweeg de muisaanwijzer over de **HTTP grote** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **Cache-instellingen** flyout. Selecteer **compressie**.

    ![Selectie van CDN-compressie](./media/cdn-file-compression/cdn-compress-select.png)

    De compressie-opties worden weergegeven.

    ![Opties voor compressie CDN](./media/cdn-file-compression/cdn-compress-files.png)
3. Compressie inschakelen door te selecteren **compressie**. Geef op de MIME-typen die u wilt comprimeren als een lijst met door komma's gescheiden (zonder spaties) in de **bestandstypen** vak.

   > [!TIP]
   > Hoewel het mogelijk is, wordt het niet aanbevolen om toe te passen van compressie naar gecomprimeerde indelingen. Bijvoorbeeld, POSTCODE, MP3, MP4 of JPG.
   > 

4. Wanneer u klaar bent, selecteert u **Update**.

## <a name="compression-rules"></a>Regels voor compressie

### <a name="azure-cdn-standard-from-microsoft-profiles"></a>Azure CDN Standard van Microsoft-profielen

Voor **Azure CDN Standard van Microsoft** profielen, alleen in aanmerking komende bestanden zijn gecomprimeerd. Als u in aanmerking komen voor compressie, moet een bestand:
- Van het MIME-type dat is [geconfigureerd voor compressie](#enabling-compression).
- Niet groter zijn dan 1 KB
- Kleiner is dan 8 MB

Deze profielen ondersteunen de volgende aanduidingen van de compressie:
- gzip (GNU zip)
- brotli 

Als de aanvraag meer dan één compressietype ondersteunt, voorrang brotli compressie.

Wanneer een aanvraag voor een asset gzip-compressie en de resultaten van de aanvraag in een cache ontbreekt, voert Azure CDN gzip-compressie van de asset rechtstreeks op de POP-server. Daarna wordt het gecomprimeerde bestand geleverd uit de cache.

### <a name="azure-cdn-from-verizon-profiles"></a>Azure CDN van Verizon-profielen

Voor **Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** profielen, alleen in aanmerking komende bestanden zijn gecomprimeerd. Als u in aanmerking komen voor compressie, moet een bestand:
- Niet groter zijn dan 128 bytes
- Kleiner is dan 3 MB

Deze profielen ondersteunen de volgende aanduidingen van de compressie:
- gzip (GNU zip)
- DEFLATE
- bzip2
- brotli 

Als de aanvraag meer dan één compressietype ondersteunt, hebben deze compressietypen voorrang op brotli compressie.

Wanneer een aanvraag voor een asset brotli compressie bevat (HTTP-header is `Accept-Encoding: br`) en de resultaten van de aanvraag in een cache ontbreekt, Azure CDN brotli compressie van de asset rechtstreeks op de POP-server uitvoert. Daarna wordt het gecomprimeerde bestand geleverd uit de cache.

### <a name="azure-cdn-standard-from-akamai-profiles"></a>Azure CDN Standard van Akamai-profielen

Voor **Azure CDN Standard van Akamai** profielen, alle bestanden komen in aanmerking voor compressie. Echter, een bestand moet van een MIME-type dat is [geconfigureerd voor compressie](#enabling-compression).

Deze profielen ondersteuning voor gzip-compressie codering alleen. Wanneer het eindpunt van een profiel van een gzip-gecodeerd bestand aanvraagt, wordt het altijd aangevraagd bij de oorsprong, ongeacht de clientaanvraag. 

## <a name="compression-behavior-tables"></a>Compressie gedrag tabellen
De volgende tabellen beschrijven Azure CDN compressie gedrag voor elk scenario:

### <a name="compression-is-disabled-or-file-is-ineligible-for-compression"></a>Compressie is uitgeschakeld of het bestand is niet in aanmerking voor compressie
| Client aangevraagde-indeling (via Accept-Encoding-header) | In de cache opgeslagen-bestandsindeling | Het CDN-antwoord naar de client | Notes&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
| --- | --- | --- | --- |
| Gecomprimeerd |Gecomprimeerd |Gecomprimeerd | |
| Gecomprimeerd |Niet-gecomprimeerde |Niet-gecomprimeerde | |
| Gecomprimeerd |Niet in cache opgeslagen |Wel of niet gecomprimeerd |Het oorspronkelijke antwoord bepaalt of CDN een compressie uitvoert. |
| Niet-gecomprimeerde |Gecomprimeerd |Niet-gecomprimeerde | |
| Niet-gecomprimeerde |Niet-gecomprimeerde |Niet-gecomprimeerde | |
| Niet-gecomprimeerde |Niet in cache opgeslagen |Niet-gecomprimeerde | |

### <a name="compression-is-enabled-and-file-is-eligible-for-compression"></a>Compressie is ingeschakeld en het bestand komt in aanmerking voor compressie
| Client aangevraagde-indeling (via Accept-Encoding-header) | In de cache opgeslagen-bestandsindeling | CDN-antwoord naar de client | Opmerkingen |
| --- | --- | --- | --- |
| Gecomprimeerd |Gecomprimeerd |Gecomprimeerd |CDN transcodeert tussen ondersteunde indelingen. |
| Gecomprimeerd |Niet-gecomprimeerde |Gecomprimeerd |CDN voert een compressie. |
| Gecomprimeerd |Niet in cache opgeslagen |Gecomprimeerd |CDN wordt een compressie uitgevoerd als de oorsprong een niet-gecomprimeerd bestand retourneert. <br/>**Azure CDN van Verizon** het niet-gecomprimeerde bestand op de eerste aanvraag is geslaagd en vervolgens worden gecomprimeerd en slaat het bestand voor volgende aanvragen. <br/>Bestanden met de `Cache-Control: no-cache` header nooit worden gecomprimeerd. |
| Niet-gecomprimeerde |Gecomprimeerd |Niet-gecomprimeerde |CDN voert een decompressie. |
| Niet-gecomprimeerde |Niet-gecomprimeerde |Niet-gecomprimeerde | |
| Niet-gecomprimeerde |Niet in cache opgeslagen |Niet-gecomprimeerde | |

## <a name="media-services-cdn-compression"></a>Media Services-CDN-compressie
Compressie is ingeschakeld voor eindpunten ingeschakeld voor het streamen van Media Services CDN standaard voor de volgende MIME-typen: 
- application/vnd.ms-sstr+xml 
- application/dash+xml
- application/vnd.apple.mpegurl
- application/f4m+xml 

## <a name="see-also"></a>Zie ook
* [Problemen met CDN-bestandscompressie oplossen](cdn-troubleshoot-compression.md)    

