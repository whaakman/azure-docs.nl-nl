---
title: De prestaties verbeteren door het comprimeren van bestanden in Azure CDN | Microsoft Docs
description: Informatie over het verbeteren van bestand overdrachtssnelheid en laden van de pagina prestaties verbeteren door het comprimeren van bestanden in Azure CDN.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: v-deasim
ms.openlocfilehash: bdff57275cf123079004ada732fe782d98399d71
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260393"
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>De prestaties verbeteren door het comprimeren van bestanden in Azure CDN
Bestandscompressie is een eenvoudige en effectieve methode voor het bestand overdrachtssnelheid verbeteren en laden van de pagina prestaties verbeteren doordat de bestandsgrootte voordat deze wordt verzonden van de server. Bestandscompressie kunt bandbreedtekosten te verlagen en een responsievere gebruikerservaring te bieden voor uw gebruikers.

Er zijn twee manieren bestandscompressie inschakelen:

- Compressie inschakelen op de bronserver. In dit geval Azure CDN langs de gecomprimeerde bestanden is geslaagd en zorgt ervoor dat deze aan clients die deze aanvragen.
- Compressie inschakelen rechtstreeks op de servers CDN pop-locatie (*compressie op elk gewenst moment*). In dit geval de CDN comprimeert van de bestanden en voor de eindgebruikers fungeert, zelfs als ze niet zijn gecomprimeerd door de bronserver.

> [!IMPORTANT]
> Azure CDN configuratiewijzigingen kunnen even duren worden doorgegeven via het netwerk: 
- Voor profielen van **Azure CDN Standard van Microsoft** is het doorgeven gewoonlijk binnen 10 minuten voltooid. 
- Profielen van **Azure CDN Standard van Akamai** worden doorgaans binnen één minuut doorgegeven. 
- Voor **Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** profielen, doorgeven voltooid gewoonlijk in 10 minuten. 
>
> Als u compressie voor het eerst uitvoert voor uw CDN-eindpunt instelt, kunt u overwegen wachten op van 1 tot 2 uur voordat u om te controleren of dat de compressie-instellingen zijn doorgegeven aan de POP's oplossen.
> 
> 

## <a name="enabling-compression"></a>Compressie inschakelen
De CDN-lagen standard en premium bieden dezelfde functionaliteit compressie, maar de gebruikersinterface verschilt. Zie voor meer informatie over de verschillen tussen standard en premium-CDN-lagen [overzicht van Azure CDN](cdn-overview.md).

### <a name="standard-cdn-profiles"></a>Standaard CDN-profielen 
> [!NOTE]
> Deze sectie geldt voor **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Verizon**, en **Azure CDN Standard van Akamai** profielen.
> 
> 

1. Selecteer het CDN-eindpunt dat u wilt beheren in de pagina CDN-profiel.
   
    ![CDN-profiel-eindpunten](./media/cdn-file-compression/cdn-endpoints.png)
   
    De pagina CDN-eindpunt wordt geopend.
2. Selecteer **compressie**.

    ![Selectie van CDN-compressie](./media/cdn-file-compression/cdn-compress-select-std.png)
   
    De pagina compressie wordt geopend.
3. Selecteer **op** compressie inschakelen.
   
    ![Opties voor compressie CDN-bestand](./media/cdn-file-compression/cdn-compress-standard.png)
4. Gebruik de standaard-MIME-typen of wijzigt u de lijst toe te voegen of te verwijderen van de MIME-typen.
   
   > [!TIP]
   > Hoewel het mogelijk is, is het niet raadzaam compressie toepassen op gecomprimeerde indelingen. Bijvoorbeeld, ZIP, MP3, MP4 of JPG.
   > 
 
5. Wanneer u klaar bent, selecteer **opslaan**.

### <a name="premium-cdn-profiles"></a>Premium-CDN-profielen
> [!NOTE]
> Deze sectie geldt alleen voor **Azure CDN Premium van Verizon** profielen.
> 

1. Selecteer in de pagina CDN-profiel **beheren**.
   
    ![Selecteer CDN beheren](./media/cdn-file-compression/cdn-manage-btn.png)
   
    Hiermee opent u de CDN-beheerportal.
2. Beweeg de muisaanwijzer over de **HTTP grote** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **Cache-instellingen** doel. Selecteer **compressie**.

    ![Selectie van CDN-compressie](./media/cdn-file-compression/cdn-compress-select.png)
   
    De opties voor compressie worden weergegeven.
   
    ![Opties voor compressie CDN-bestand](./media/cdn-file-compression/cdn-compress-files.png)
3. Compressie inschakelen door het selecteren **compressie ingeschakeld**. Voer de MIME-typen die u wilt comprimeren als een door komma's gescheiden lijst (zonder spaties) in de **bestandstypen** vak.
   
   > [!TIP]
   > Hoewel het mogelijk is, is het niet raadzaam compressie toepassen op gecomprimeerde indelingen. Bijvoorbeeld, ZIP, MP3, MP4 of JPG.
   > 
    
4. Wanneer u klaar bent, selecteer **Update**.

## <a name="compression-rules"></a>Regels voor compressie

### <a name="azure-cdn-standard-from-microsoft-profiles"></a>Azure CDN Standard van Microsoft-profielen

Voor **Azure CDN Standard van Microsoft** profielen, alle bestanden in aanmerking komen voor compressie. Echter, een bestand moet van een MIME-type dat is [geconfigureerd voor compressie](#enabling-compression).

Deze profielen ondersteunen de volgende coderingen voor compressie:
- gzip (GNU zip)
- brotli 
 
Als de aanvraag meer dan één compressietype ondersteunt, hebben deze compressietypen voorrang op brotli compressie.

Wanneer een aanvraag voor een asset gzip-compressie en de resultaten van de aanvraag in een cache ontbreekt, voert Azure CDN gzip-compressie van de asset rechtstreeks op de POP-server. Daarna wordt het gecomprimeerde bestand geleverd uit de cache.

### <a name="azure-cdn-from-verizon-profiles"></a>Azure CDN van Verizon profielen

Voor **Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** profielen, alleen in aanmerking komende bestanden zijn gecomprimeerd. Als u in aanmerking komen voor compressie, moet een bestand:
- Niet groter zijn dan 128 bytes
- Kleiner dan 1 MB
 
Deze profielen ondersteunen de volgende coderingen voor compressie:
- gzip (GNU zip)
- VERKLEINEN
- Bzip2
- brotli 
 
Als de aanvraag meer dan één compressietype ondersteunt, hebben deze compressietypen voorrang op brotli compressie.

Als een aanvraag voor een asset brotli compressie opgeeft (http-header is `Accept-Encoding: br`) en de aanvraag resulteert in een cache ontbreekt, Azure CDN brotli compressie van de asset rechtstreeks op de POP-server uitvoert. Daarna wordt het gecomprimeerde bestand geleverd uit de cache.

### <a name="azure-cdn-standard-from-akamai-profiles"></a>Azure CDN Standard van Akamai profielen

Voor **Azure CDN Standard van Akamai** profielen, alle bestanden in aanmerking komen voor compressie. Echter, een bestand moet van een MIME-type dat is [geconfigureerd voor compressie](#enabling-compression).

Deze profielen gzip compressie coderen alleen wordt ondersteund. Wanneer het eindpunt van een profiel een gzip-gecodeerd bestand aanvraagt, wordt het altijd aangevraagd vanuit de oorsprong, ongeacht de clientaanvraag. 

## <a name="compression-behavior-tables"></a>Compressie gedrag tabellen
De volgende tabellen beschrijven Azure CDN compressie gedrag voor elk scenario:

### <a name="compression-is-disabled-or-file-is-ineligible-for-compression"></a>Compressie wordt uitgeschakeld of het bestand is niet in aanmerking voor compressie
| Clients worden aangevraagd-indeling (via de header Accept-Encoding) | In de cache opgeslagen bestandsindeling | Het CDN-antwoord naar de client | Notes&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
| --- | --- | --- | --- |
| Gecomprimeerd |Gecomprimeerd |Gecomprimeerd | |
| Gecomprimeerd |Niet-gecomprimeerde |Niet-gecomprimeerde | |
| Gecomprimeerd |Niet in cache |Wel of niet gecomprimeerd |Het antwoord van de oorsprong wordt bepaald of CDN een compressie uitvoert. |
| Niet-gecomprimeerde |Gecomprimeerd |Niet-gecomprimeerde | |
| Niet-gecomprimeerde |Niet-gecomprimeerde |Niet-gecomprimeerde | |
| Niet-gecomprimeerde |Niet in cache |Niet-gecomprimeerde | |

### <a name="compression-is-enabled-and-file-is-eligible-for-compression"></a>Compressie is ingeschakeld en het bestand komt in aanmerking voor compressie
| Clients worden aangevraagd-indeling (via de header Accept-Encoding) | In de cache opgeslagen bestandsindeling | CDN-antwoord naar de client | Opmerkingen |
| --- | --- | --- | --- |
| Gecomprimeerd |Gecomprimeerd |Gecomprimeerd |CDN-transcodes tussen ondersteunde indelingen. |
| Gecomprimeerd |Niet-gecomprimeerde |Gecomprimeerd |CDN voert een compressie. |
| Gecomprimeerd |Niet in cache |Gecomprimeerd |CDN voert een compressie uit als de oorsprong een niet-gecomprimeerde bestand retourneert. <br/>**Azure CDN van Verizon** niet-gecomprimeerde bestand op de eerste aanvraag wordt doorgegeven en vervolgens worden gecomprimeerd en slaat het bestand voor volgende aanvragen. <br/>Bestanden met de `Cache-Control: no-cache` header nooit worden gecomprimeerd. |
| Niet-gecomprimeerde |Gecomprimeerd |Niet-gecomprimeerde |CDN voert een decompressie. |
| Niet-gecomprimeerde |Niet-gecomprimeerde |Niet-gecomprimeerde | |
| Niet-gecomprimeerde |Niet in cache |Niet-gecomprimeerde | |

## <a name="media-services-cdn-compression"></a>Media Services-CDN-compressie
Compressie is ingeschakeld voor eindpunten die zijn ingeschakeld voor het streamen van Media Services CDN, standaard voor de volgende MIME-typen: 
- vnd.ms-toepassing-sstr + xml 
- toepassing/dash + xml
- application/vnd.Apple.mpegurl
- toepassing/f4m + xml 

## <a name="see-also"></a>Zie ook
* [Problemen met CDN-bestandscompressie oplossen](cdn-troubleshoot-compression.md)    

