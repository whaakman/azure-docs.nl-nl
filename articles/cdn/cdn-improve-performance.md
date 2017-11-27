---
title: De prestaties verbeteren door het comprimeren van bestanden in Azure CDN | Microsoft Docs
description: Informatie over het verbeteren van bestand overdrachtssnelheid en verhoogt de laadprestaties van de pagina door de bestanden in Azure CDN te comprimeren.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: b4e27ec57543daed35811fff347f457b0dd2cd5c
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>De prestaties verbeteren door het comprimeren van bestanden in Azure CDN
Compressie is een eenvoudige en effectieve methode voor het verbeteren van bestand overdrachtssnelheid en laadprestaties van de pagina vergroten door bestandscompressie voordat deze wordt verzonden van de server. Het verlaagt de bandbreedtekosten en biedt een responsievere ervaring voor uw gebruikers.

Er zijn twee manieren compressie inschakelen:

* Compressie inschakelen op de bronserver. In dit geval de CDN wordt doorgegeven via de gecomprimeerde bestanden en zorgt ervoor dat deze aan clients die deze aanvragen.
* Compressie rechtstreeks op de servers van de rand CDN inschakelen. In dit geval de CDN comprimeert van de bestanden en aan eindgebruikers, fungeert, zelfs als ze niet zijn gecomprimeerd door de bronserver.

> [!IMPORTANT]
> CDN-configuratiewijzigingen kunnen even duren worden doorgegeven via het netwerk.  Voor <b>Azure CDN van Akamai</b> profielen, doorgeven voltooid gewoonlijk in onder één minuut.  Voor <b>Azure CDN van Verizon</b> profielen, uw wijzigingen toepassen meestal binnen 90 minuten.  Als u compressie voor het eerst uitvoert voor uw CDN-eindpunt instelt, kunt u overwegen wachten op van 1 tot 2 uur voordat u om te controleren of dat de compressie-instellingen zijn doorgegeven aan de POP's oplossen.
> 
> 

## <a name="enabling-compression"></a>Compressie inschakelen
> [!NOTE]
> De categorieën Standard en Premium-CDN bieden dezelfde functionaliteit compressie, maar de gebruikersinterface verschilt.  Zie voor meer informatie over de verschillen tussen de categorieën Standard en Premium-CDN [overzicht van Azure CDN](cdn-overview.md).
> 
> 

### <a name="standard-tier"></a>Standaardlaag
> [!NOTE]
> Deze sectie geldt voor **Azure CDN Standard van Verizon** en **Azure CDN Standard van Akamai** profielen.
> 
> 

1. Klik op het CDN-eindpunt dat u wilt beheren via de pagina CDN-profiel.
   
    ![CDN-profiel-eindpunten](./media/cdn-file-compression/cdn-endpoints.png)
   
    De pagina CDN-eindpunt wordt geopend.
2. Klik op de **configureren** knop.
   
    ![Knop CDN-profiel beheren](./media/cdn-file-compression/cdn-config-btn.png)
   
    De pagina configuratie van CDN wordt geopend.
3. Schakel **compressie**.
   
    ![CDN-opties voor compressie](./media/cdn-file-compression/cdn-compress-standard.png)
4. Gebruik de standaardtypen of wijzigt u de lijst door te verwijderen of toevoegen, bestandstypen.
   
   > [!TIP]
   > Hoewel het mogelijk is, is het niet raadzaam compressie toepassen op gecomprimeerde indelingen. Bijvoorbeeld, ZIP, MP3, MP4 of JPG.
   > 
 
5. Wanneer u klaar bent, klikt u op de **opslaan** knop.

### <a name="premium-tier"></a>Premiumlaag
> [!NOTE]
> Deze sectie geldt voor **Azure CDN Premium van Verizon** profielen.
> 
> 

1. Klik op de pagina CDN-profiel de **beheren** knop.
   
    ![Knop CDN-profiel beheren](./media/cdn-file-compression/cdn-manage-btn.png)
   
    Hiermee opent u de CDN-beheerportal.
2. Beweeg de muisaanwijzer over de **HTTP grote** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **Cache-instellingen** doel.  Klik op **compressie**.

    ![Compressie bestandsselectie](./media/cdn-file-compression/cdn-compress-select.png)
   
    Opties voor compressie worden weergegeven.
   
    ![Opties voor compressie](./media/cdn-file-compression/cdn-compress-files.png)
3. Compressie inschakelen door te klikken op de **compressie ingeschakeld** keuzerondje.  Voer de MIME-typen die u wilt comprimeren als een door komma's gescheiden lijst (zonder spaties) in de **bestandstypen** textbox.
   
   > [!TIP]
   > Hoewel het mogelijk is, is het niet raadzaam compressie toepassen op gecomprimeerde indelingen. Bijvoorbeeld, ZIP, MP3, MP4 of JPG.
   > 
4. Wanneer u klaar bent, klikt u op de **Update** knop.

## <a name="compression-rules"></a>Regels voor compressie
Deze tabellen beschrijven Azure CDN compressie gedrag voor elk scenario.

> [!IMPORTANT]
> Voor **Azure CDN van Verizon** profielen (standaard en Premium), alleen in aanmerking komende bestanden zijn gecomprimeerd.  Als u in aanmerking komen voor compressie, moet een bestand:
> 
> * Groter zijn dan 128 bytes.
> * Kleiner dan 1 MB zijn.
> 
> Ondersteuning voor deze profielen **gzip** (GNU zip), **deflate**, **bzip2**, of **br** codering (Brotli). Voor het coderen van Brotli moet de compressie worden uitgevoerd op de oorsprong. De aanvraag voor het coderen van Brotli door de clientbrowser moet verzenden en het gecomprimeerde actief moet zijn gecomprimeerd aan de kant van de oorsprong eerst. 

> [!IMPORTANT]
> Voor **Azure CDN van Akamai** profielen, alle bestanden in aanmerking komen voor compressie. Een bestand moet echter een MIME-type dat is [geconfigureerd voor compressie](#enabling-compression).
> 
>Deze profielen ondersteunen alleen **gzip** codering. Wanneer een profieleindpunt aanvragen **gzip** gecodeerde bestanden, ze altijd worden aangevraagd vanuit de oorsprong, ongeacht de clientaanvraag. 

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Compressie uitgeschakeld of het bestand is niet in aanmerking voor compressie
| Aangevraagde indeling van de client (via de header Accept-Encoding) | In de cache-bestandsindeling | CDN-antwoord naar de client | Opmerkingen |
| --- | --- | --- | --- |
| Gecomprimeerd |Gecomprimeerd |Gecomprimeerd | |
| Gecomprimeerd |Niet-gecomprimeerde |Niet-gecomprimeerde | |
| Gecomprimeerd |Niet in cache |Wel of niet gecomprimeerd |Is afhankelijk van het antwoord van de oorsprong |
| Niet-gecomprimeerde |Gecomprimeerd |Niet-gecomprimeerde | |
| Niet-gecomprimeerde |Niet-gecomprimeerde |Niet-gecomprimeerde | |
| Niet-gecomprimeerde |Niet in cache |Niet-gecomprimeerde | |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Compressie is ingeschakeld en de bestandsnaam komt in aanmerking voor compressie
| Aangevraagde indeling van de client (via de header Accept-Encoding) | In de cache-bestandsindeling | CDN-antwoord naar de client | Opmerkingen |
| --- | --- | --- | --- |
| Gecomprimeerd |Gecomprimeerd |Gecomprimeerd |CDN-transcodes tussen ondersteunde indelingen |
| Gecomprimeerd |Niet-gecomprimeerde |Gecomprimeerd |CDN voert compressie |
| Gecomprimeerd |Niet in cache |Gecomprimeerd |CDN uitvoert compressie als oorsprong niet-gecomprimeerde retourneert.  **Azure CDN van Verizon** niet-gecomprimeerde bestand op de eerste aanvraag wordt doorgegeven en vervolgens worden gecomprimeerd en slaat het bestand voor volgende aanvragen.  Bestanden met de `Cache-Control: no-cache` header nooit worden gecomprimeerd. |
| Niet-gecomprimeerde |Gecomprimeerd |Niet-gecomprimeerde |CDN voert decompressie |
| Niet-gecomprimeerde |Niet-gecomprimeerde |Niet-gecomprimeerde | |
| Niet-gecomprimeerde |Niet in cache |Niet-gecomprimeerde | |

## <a name="media-services-cdn-compression"></a>Media Services-CDN-compressie
Compressie is ingeschakeld voor streaming-eindpunten van Media Services CDN is ingeschakeld voor de volgende inhoudstypen standaard: 
- vnd.ms-toepassing-sstr + xml 
- toepassing/dash + xml
- application/vnd.Apple.mpegurl
- toepassing/f4m + xml. 

U kunt- of uitschakelen compressie voor de genoemde typen met behulp van de Azure-portal.  

## <a name="see-also"></a>Zie ook
* [Problemen met CDN-bestandscompressie oplossen](cdn-troubleshoot-compression.md)    

