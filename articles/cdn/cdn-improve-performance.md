---
title: De prestaties verbeteren door het comprimeren van bestanden in Azure CDN | Microsoft Docs
description: Informatie over het verbeteren van bestand overdrachtssnelheid en verhoogt de laadprestaties van de pagina door de bestanden in Azure CDN te comprimeren.
services: cdn
documentationcenter: 
author: dksimpson
manager: akucer
editor: 
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: mazha
ms.openlocfilehash: 77d889f5d56ed839665588cf359b73e0f9ad28b5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>De prestaties verbeteren door het comprimeren van bestanden in Azure CDN
Compressie is een eenvoudige en effectieve methode voor het verbeteren van bestand overdrachtssnelheid en laadprestaties van de pagina vergroten door bestandscompressie voordat deze wordt verzonden van de server. Het verlaagt de bandbreedtekosten en biedt een responsievere ervaring voor uw gebruikers.

Er zijn twee manieren compressie inschakelen:

* Compressie inschakelen op de bronserver. In dit geval de CDN wordt doorgegeven via de gecomprimeerde bestanden en zorgt ervoor dat deze aan clients die deze aanvragen.
* Compressie rechtstreeks op de servers van de rand CDN inschakelen. In dit geval de CDN comprimeert van de bestanden en aan eindgebruikers, fungeert, zelfs als ze niet zijn gecomprimeerd door de bronserver.

> [!IMPORTANT]
> CDN-configuratiewijzigingen kunnen even duren worden doorgegeven via het netwerk. Voor <b>Azure CDN van Akamai</b> profielen, doorgeven voltooid gewoonlijk in onder één minuut.  Voor <b>Azure CDN van Verizon</b> profielen, doorgeven voltooit meestal binnen 90 minuten. Als u compressie voor het eerst uitvoert voor uw CDN-eindpunt instelt, kunt u overwegen wachten op van 1 tot 2 uur voordat u om te controleren of dat de compressie-instellingen zijn doorgegeven aan de POP's oplossen.
> 
> 

## <a name="enabling-compression"></a>Compressie inschakelen
De categorieën Standard en Premium-CDN bieden dezelfde functionaliteit compressie, maar de gebruikersinterface verschilt. Zie voor meer informatie over de verschillen tussen de categorieën Standard en Premium-CDN [overzicht van Azure CDN](cdn-overview.md).

### <a name="standard-tier"></a>Standaardlaag
> [!NOTE]
> Deze sectie geldt voor **Azure CDN Standard van Verizon** en **Azure CDN Standard van Akamai** profielen.
> 
> 

1. Selecteer het CDN-eindpunt dat u wilt beheren in de pagina CDN-profiel.
   
    ![CDN-profiel-eindpunten](./media/cdn-file-compression/cdn-endpoints.png)
   
    De pagina CDN-eindpunt wordt geopend.
2. Selecteer **compressie**.

    ![CDN-profiel-eindpunten](./media/cdn-file-compression/cdn-compress-select-std.png)
   
    De pagina compressie wordt geopend.
3. Selecteer **op** compressie inschakelen.
   
    ![CDN-opties voor compressie](./media/cdn-file-compression/cdn-compress-standard.png)
4. Gebruik de standaardtypen indeling of wijzigt u de lijst toe te voegen of te verwijderen van de indelingstypen.
   
   > [!TIP]
   > Hoewel het mogelijk is, is het niet raadzaam compressie toepassen op gecomprimeerde indelingen. Bijvoorbeeld, ZIP, MP3, MP4 of JPG.
   > 
 
5. Wanneer u klaar bent, selecteer **opslaan**.

### <a name="premium-tier"></a>Premiumlaag
> [!NOTE]
> Deze sectie geldt alleen voor **Azure CDN Premium van Verizon** profielen.
> 
> 

1. Selecteer in de pagina CDN-profiel **beheren**.
   
    ![Knop CDN-profiel beheren](./media/cdn-file-compression/cdn-manage-btn.png)
   
    Hiermee opent u de CDN-beheerportal.
2. Beweeg de muisaanwijzer over de **HTTP grote** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **Cache-instellingen** doel. Selecteer **compressie**.

    ![Compressie bestandsselectie](./media/cdn-file-compression/cdn-compress-select.png)
   
    Opties voor compressie worden weergegeven.
   
    ![Opties voor compressie](./media/cdn-file-compression/cdn-compress-files.png)
3. Compressie inschakelen door het selecteren **compressie ingeschakeld**. Voer de MIME-typen die u wilt comprimeren als een door komma's gescheiden lijst (zonder spaties) in de **bestandstypen** vak.
   
   > [!TIP]
   > Hoewel het mogelijk is, is het niet raadzaam compressie toepassen op gecomprimeerde indelingen. Bijvoorbeeld, ZIP, MP3, MP4 of JPG.
   > 
    
4. Wanneer u klaar bent, selecteer **Update**.

## <a name="compression-rules"></a>Regels voor compressie

### <a name="azure-cdn-from-verizon-profiles-both-standard-and-premium"></a>Azure CDN van Verizon profielen (standaard en premium)

Voor **Azure CDN van Verizon** profielen, alleen in aanmerking komende bestanden zijn gecomprimeerd. Als u in aanmerking komen voor compressie, moet een bestand:
- Groter zijn dan 128 bytes.
- Kleiner dan 1 MB zijn.
 
Ondersteuning voor deze profielen **gzip** (GNU zip), **deflate**, **bzip2**, of **br** codering (Brotli). Als de aanvraag meer dan één compressietype ondersteunt, hebben deze compressietypen voorrang op Brotli compressie.

Wanneer een aanvraag voor een asset geeft Brotli codering (de HTTP-header bevat `Accept-Encoding: br`) en de aanvraag resulteert in een cache ontbreekt, Azure CDN voert Brotli compressie van het actief op de bronserver. Daarna wordt het gecomprimeerde bestand geleverd rechtstreeks uit de cache.

### <a name="azure-cdn-from-akamai-profiles"></a>Azure CDN van Akamai profielen

Voor **Azure CDN van Akamai** profielen, alle bestanden in aanmerking komen voor compressie. Echter, een bestand moet van een MIME-type dat is [geconfigureerd voor compressie](#enabling-compression).

Deze profielen ondersteunen alleen **gzip** codering. Wanneer een profieleindpunt aanvragen **gzip** gecodeerde bestanden, ze altijd worden aangevraagd vanuit de oorsprong, ongeacht de clientaanvraag. 

## <a name="compression-behavior-tables"></a>Compressie gedrag tabellen
De volgende tabellen beschrijven Azure CDN compressie gedrag voor elk scenario:

### <a name="compression-is-disabled-or-file-is-ineligible-for-compression"></a>Compressie wordt uitgeschakeld of het bestand is niet in aanmerking voor compressie
| Clients worden aangevraagd-indeling (via de header Accept-Encoding) | In de cache opgeslagen bestandsindeling | CDN-antwoord naar de client | Opmerkingen |
| --- | --- | --- | --- |
| Gecomprimeerd |Gecomprimeerd |Gecomprimeerd | |
| Gecomprimeerd |Niet-gecomprimeerde |Niet-gecomprimeerde | |
| Gecomprimeerd |Niet in cache |Wel of niet gecomprimeerd |Is afhankelijk van het antwoord van de oorsprong |
| Niet-gecomprimeerde |Gecomprimeerd |Niet-gecomprimeerde | |
| Niet-gecomprimeerde |Niet-gecomprimeerde |Niet-gecomprimeerde | |
| Niet-gecomprimeerde |Niet in cache |Niet-gecomprimeerde | |

### <a name="compression-is-enabled-and-file-is-eligible-for-compression"></a>Compressie is ingeschakeld en het bestand komt in aanmerking voor compressie
| Clients worden aangevraagd-indeling (via de header Accept-Encoding) | In de cache opgeslagen bestandsindeling | CDN-antwoord naar de client | Opmerkingen |
| --- | --- | --- | --- |
| Gecomprimeerd |Gecomprimeerd |Gecomprimeerd |CDN-transcodes tussen ondersteunde indelingen |
| Gecomprimeerd |Niet-gecomprimeerde |Gecomprimeerd |CDN voert compressie |
| Gecomprimeerd |Niet in cache |Gecomprimeerd |CDN uitvoert compressie als de oorsprong een niet-gecomprimeerde bestand retourneert. **Azure CDN van Verizon** niet-gecomprimeerde bestand op de eerste aanvraag wordt doorgegeven en vervolgens worden gecomprimeerd en slaat het bestand voor volgende aanvragen. Bestanden met de Cache-Control: Nee-cache-header nooit worden gecomprimeerd. |
| Niet-gecomprimeerde |Gecomprimeerd |Niet-gecomprimeerde |CDN voert decompressie |
| Niet-gecomprimeerde |Niet-gecomprimeerde |Niet-gecomprimeerde | |
| Niet-gecomprimeerde |Niet in cache |Niet-gecomprimeerde | |

## <a name="media-services-cdn-compression"></a>Media Services CDN Compression
Compressie is ingeschakeld voor eindpunten die zijn ingeschakeld voor het streamen van Media Services CDN, standaard voor de volgende MIME-typen: 
- vnd.ms-toepassing-sstr + xml 
- toepassing/dash + xml
- application/vnd.Apple.mpegurl
- toepassing/f4m + xml. 

U kunt geen- of uitschakelen compressie voor deze MIME-typen met behulp van de Azure-portal.  

## <a name="see-also"></a>Zie ook
* [Problemen met CDN-bestandscompressie oplossen](cdn-troubleshoot-compression.md)    

