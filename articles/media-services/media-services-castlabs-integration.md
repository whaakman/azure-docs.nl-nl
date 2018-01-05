---
title: Met behulp van castLabs Widevine-licenties te leveren met Azure Media Services | Microsoft Docs
description: Dit artikel wordt beschreven hoe u Azure Media Services (AMS) kunt gebruiken voor het leveren van een stroom die dynamisch wordt versleuteld met AMS met PlayReady en Widevine DRM's. De licentie PlayReady afkomstig is van de licentieserver Media Services PlayReady en Widevine-licentie wordt geleverd door de licentieserver castLabs.
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 2a9a408a-a995-49e1-8d8f-ac5b51e17d40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: Mingfeiy;willzhan;Juliako
ms.openlocfilehash: 5b69e804809f834e81221fb2787a997a52dbe286
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a>castLabs gebruiken om Widevine-licenties te leveren aan Azure Media Services
> [!div class="op_single_selector"]
> * [Axinom](media-services-axinom-integration.md)
> * [castLabs](media-services-castlabs-integration.md)
> 
> 

## <a name="overview"></a>Overzicht
Dit artikel wordt beschreven hoe u Azure Media Services (AMS) kunt gebruiken voor het leveren van een stroom die dynamisch wordt versleuteld met AMS met PlayReady en Widevine DRM's. De licentie PlayReady afkomstig is van de licentieserver Media Services PlayReady en Widevine-licentie is geleverd door **castLabs** licentieserver.

Voor het afspelen streaming inhoud beveiligd met CENC (PlayReady en/of Widevine), kunt u [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Zie [AMP document](http://amp.azure.net/libs/amp/latest/docs/) voor meer informatie.

Het volgende diagram ziet u een hoog niveau Azure Media Services en castLabs integratiearchitectuur.

![Integratie](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

## <a name="typical-system-set-up"></a>Typische system instellen
* Media-inhoud wordt opgeslagen in AMS.
* Sleutel-id's van inhoud sleutels worden opgeslagen in zowel castLabs als AMS.
* castLabs en AMS hebben tokenverificatie ingebouwd. De volgende secties worden besproken verificatietokens. 
* Wanneer een client wil de video te streamen, de inhoud die dynamisch wordt versleuteld met **Common Encryption** (CENC) en AMS Smooth Streaming-en DASH dynamisch verpakt. We bieden ook PlayReady M2TS elementaire stroom versleuteling voor streaming HLS-protocol.
* PlayReady-licentie is opgehaald uit de AMS-licentieserver en Widevine-licentie is opgehaald uit de licentieserver castLabs. 
* Media Player besluit automatisch welke licentie ophalen op basis van de mogelijkheden van het platform client. 

## <a name="authentication-token-generation-for-getting-a-license"></a>Verificatie-token genereren voor het verkrijgen van een licentie
Zowel castLabs en AMS ondersteuning voor token JWT (JSON Web Token)-indeling gebruikt een licentie verlenen. 

### <a name="jwt-token-in-ams"></a>JWT-token in AMS
De volgende tabel beschrijft de JWT-token in AMS. 

| Verlener | Tekenreeks van de verlener van het gekozen Secure Token Service (STS) |
| --- | --- |
| Doelgroep |Tekenreeks van de doelgroep van het gebruikte STS |
| Claims |Een set claims |
| NotBefore |Start de geldigheid van het token |
| Verloopt op |Einde geldigheid van het token |
| SigningCredentials |De sleutel die wordt gedeeld door PlayReady-licentieserver castLabs licentieserver en STS, wordt zowel symmetrisch als asymmetrisch sleutel. |

### <a name="jwt-token-in-castlabs"></a>JWT-token in castLabs
De volgende tabel beschrijft de JWT-token in castLabs. 

| Naam | Beschrijving |
| --- | --- |
| optData |Een JSON-tekenreeks met informatie over u. |
| CRT |Een JSON-tekenreeks met informatie over de asset de licentierechten voor gegevens en afspelen. |
| IAT |De huidige datum/tijd in epoche. |
| jti |Een unieke id over dit token (elke token kan slechts eenmaal worden gebruikt in het systeem castLabs). |

## <a name="sample-solution-set-up"></a>Voorbeeldoplossing instellen
De [oplossing steekproef](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) bestaat uit twee projecten:

* Een consoletoepassing die DRM beperkingen instellen voor een asset al opgenomen, voor zowel PlayReady als Widevine kan worden gebruikt.
* Een webtoepassing die handen uit beveiligingstokens die kunnen worden gezien als een zeer vereenvoudigde versie van een STS.

De consoletoepassing gebruiken:

1. Wijzig de app.config om AMS-referenties, castLabs referenties, STS-configuratie en gedeelde sleutel in te stellen.
2. Upload een Asset naar AMS.
3. De UUID van het geüploade activum ophalen en wijzig regel 32 in het bestand Program.cs:
   
      var objIAsset = _context. Assets.Where (x = > x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf '). FirstOrDefault();
4. Gebruik een item-id hebt voor de naamgeving van de activa in het systeem castLabs (44 regel in het bestand Program.cs).
   
   U moet instellen item-id hebt voor **castLabs**; het moet een unieke alfanumerieke tekenreeks.
5. Voer het programma.

De Web-toepassing (STS) gebruiken:

1. Wijzigen van het bestand web.config in setup castlabs verkoper-ID, de STS-configuratie en de gedeelde sleutel.
2. Implementeren naar Azure Websites.
3. Navigeer naar de website.

## <a name="playing-back-a-video"></a>Afspelen van video
Voor het afspelen van video versleuteld met common encryption (PlayReady en/of Widevine), kunt u de [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Wanneer de console-app wordt uitgevoerd, wordt de inhoud sleutel-ID en de URL Manifest teruggestuurd.

1. Een nieuw tabblad openen en start de STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2. Ga naar [Azure mediaspeler](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
3. In de streaming-URL plakken.
4. Klik op de **geavanceerde opties** selectievakje.
5. In de **beveiliging** vervolgkeuzelijst, selecteer PlayReady en/of Widevine.
6. Plak het token dat u hebt verkregen van de STS in het tekstvak voor de Token. 
   
   De licentieserver castLab hoeft niet de ' Bearer = "voorvoegsel voor het token. Dus verwijder die vóór het verzenden van het token.
7. Windows media player bijwerken.
8. De video moet worden afgespeeld.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

