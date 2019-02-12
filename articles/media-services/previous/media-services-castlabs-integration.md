---
title: Castlabs gebruiken om te Widevine-licenties leveren aan Azure Media Services | Microsoft Docs
description: Dit artikel wordt beschreven hoe u Azure Media Services (AMS) kunt gebruiken voor het leveren van een stroom die dynamisch wordt versleuteld met AMS met zowel PlayReady als Widevine DRM's. De PlayReady-licentie is afkomstig van licentieserver Media Services PlayReady en Widevine-licentie wordt geboden door castLabs licentieserver.
services: media-services
documentationcenter: ''
author: Mingfeiy
manager: femila
editor: ''
ms.assetid: 2a9a408a-a995-49e1-8d8f-ac5b51e17d40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: Mingfeiy;willzhan;Juliako
ms.openlocfilehash: 0b3d8759f13f48e5fa95ff709fa283ed41e0ea25
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56003207"
---
# <a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a>castLabs gebruiken om Widevine-licenties te leveren aan Azure Media Services 
> [!div class="op_single_selector"]
> * [Axinom](media-services-axinom-integration.md)
> * [castLabs](media-services-castlabs-integration.md)
> 
> 

## <a name="overview"></a>Overzicht

Dit artikel wordt beschreven hoe u Azure Media Services (AMS) kunt gebruiken voor het leveren van een stroom die dynamisch wordt versleuteld met AMS met zowel PlayReady als Widevine DRM's. De PlayReady-licentie is afkomstig van licentieserver Media Services PlayReady en Widevine-licentie wordt geboden door **castLabs** licentieserver.

Als u wilt afspelen terug streaming-inhoud die is beveiligd door CENC (PlayReady en/of Widevine), kunt u [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Zie [AMP document](http://amp.azure.net/libs/amp/latest/docs/) voor meer informatie.

Het volgende diagram ziet u een op hoog niveau Azure Media Services en de architectuur van castLabs-integratie.

![Integratie](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

## <a name="typical-system-set-up"></a>Typische systeem instellen

* Media-inhoud wordt opgeslagen in AMS.
* Sleutel-id's van inhoud sleutels worden opgeslagen in zowel castLabs als AMS.
* castLabs en AMS hebben tokenverificatie ingebouwd. De volgende secties worden besproken verificatietokens. 
* Wanneer een client een aanvraag om de video te streamen, de inhoud die dynamisch wordt versleuteld met **Common Encryption** (CENC) en dynamisch verpakt door AMS Smooth Streaming en DASH. We bieden ook versleuteling van PlayReady M2TS elementaire stream voor HLS streaming-protocol.
* PlayReady-licentie is opgehaald uit de AMS-licentieserver en Widevine-licentie wordt opgehaald uit de licentieserver castLabs. 
* Media Player wordt automatisch besluit welke licentie om op te halen op basis van de client-platforms. 

## <a name="authentication-token-generation-for-getting-a-license"></a>Genereren van tokens voor het ophalen van een licentie voor verificatie

CastLabs zowel AMS ondersteuning voor token JWT (JSON Web Token)-indeling die wordt gebruikt om een licentie. 

### <a name="jwt-token-in-ams"></a>JWT-token in AMS

De volgende tabel beschrijft de JWT-token in AMS. 

| Verlener | Tekenreeks van de uitgever van de gekozen Secure Token Service (STS) |
| --- | --- |
| Doelgroep |Tekenreeks van de doelgroep van de gebruikte STS |
| Claims |Een set claims |
| NotBefore |Start de geldigheid van het token |
| Verloopt op |Einde geldigheid van het token |
| SigningCredentials |De sleutel die wordt gedeeld door de Server in de PlayReady-licentie, castLabs licentieserver en STS, wordt zowel symmetrisch als asymmetrisch sleutel. |

### <a name="jwt-token-in-castlabs"></a>JWT-token in castLabs

De volgende tabel beschrijft de JWT-token in castLabs. 

| Name | Description |
| --- | --- |
| optData |Een JSON-tekenreeks met informatie over u. |
| crt |Een JSON-tekenreeks met informatie over de asset de licentierechten info en afspelen. |
| IAT |De huidige datum en tijd in epoche. |
| jti |Een unieke id over dit token (elke token kan alleen worden één keer gebruikt in het systeem castLabs). |

## <a name="sample-solution-setup"></a>Voorbeeld-oplossing instellen

De [voorbeeld van een oplossing](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) uit twee projecten bestaat:

* Een consoletoepassing die kan worden gebruikt om DRM-beperkingen instellen voor een asset al opgenomen, voor zowel PlayReady als Widevine.
* Een webtoepassing die handen van tokens, die kunnen worden gezien als een zeer vereenvoudigde versie van een STS.

De consoletoepassing gebruiken:

1. Wijzig de app.config om in te stellen AMS-referenties, castLabs referenties, de configuratie van de STS en gedeelde sleutel.
2. Upload een Asset naar AMS.
3. De UUID van de geüploade activa ophalen en wijzig regel 32 in het bestand Program.cs:
   
      var objIAsset = _context. Assets.Where (x = > x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf '). FirstOrDefault();
4. Gebruik een AssetId voor de naamgeving van de asset in het systeem castLabs (regel 44 in het bestand Program.cs).
   
   U moet instellen AssetId voor **castLabs**; dit moet een alfanumerieke tekenreeks met unieke.
5. Voer het programma uit.

De Web-toepassing (STS) gebruiken:

1. Het bestand web.config wijzigen in setup castlabs leverancier-ID, de configuratie van de STS en de gedeelde sleutel.
2. Implementeren naar Azure Websites.
3. Navigeer naar de website.

## <a name="playing-back-a-video"></a>Een video afspelen

Als u wilt afspelen van een video versleuteld met algemene versleuteling (PlayReady en/of Widevine) kunt u de [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Wanneer de console-app wordt uitgevoerd, wordt de inhoud sleutel-ID en de Manifest-URL weergegeven.

1. Een nieuw tabblad geopend en start de STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2. Ga naar [Azure MediaPlayer](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
3. In de streaming-URL plakken.
4. Klik op de **geavanceerde opties** selectievakje.
5. In de **Protection** vervolgkeuzelijst, selecteer PlayReady en/of Widevine.
6. Plak het token dat u hebt verkregen via de STS in het tekstvak Token. 
   
   De licentieserver castLab hoeft niet de "Bearer =" voorvoegsel voor het token. Dus verwijder die vóór het indienen van het token.
7. De speler worden bijgewerkt.
8. De video moet worden afgespeeld.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

