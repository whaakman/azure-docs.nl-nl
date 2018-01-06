---
title: Een autorisatiebeleid voor inhoudssleutels configureren via de Azure portal | Microsoft Docs
description: Informatie over het configureren van een verificatiebeleid voor een inhoudssleutel.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ee82a3fa-c34b-48f2-a108-8ba321f1691e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: a0ab954bda3340b9010b16f54e343933808cc463
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="configure-a-content-key-authorization-policy"></a>Een autorisatiebeleid voor inhoudssleutels configureren
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Overzicht
 U kunt Azure Media Services MPEG-DASH-, Smooth Streaming- en HTTP Live Streaming (HLS)-streams met Advanced Encryption Standard (AES) is beveiligd met een 128-bits versleutelingssleutels leveren of [PlayReady DRM (DRM)](https://www.microsoft.com/playready/overview/). Met Media Services kunt u ook versleuteld met Widevine DRM DASH-streams leveren. Zowel PlayReady als Widevine zijn versleuteld volgens de specificatie met common encryption (ISO/IEC 23001-7 CENC).

Media Services biedt ook een Service voor het leveren van sleutel/licentie van waaruit clients AES-sleutels of PlayReady/Widevine-licenties te spelen de versleutelde inhoud kunnen ophalen.

In dit artikel laat zien hoe de Azure portal gebruiken voor het autorisatiebeleid voor inhoudssleutels configureren. De sleutel kan later worden gebruikt voor het dynamisch versleutelen van uw inhoud. U kunt op dit moment versleutelen HLS, MPEG DASH en Smooth Streaming-indelingen. U kunt progressief downloaden niet coderen.

Wanneer een speler een stroom die is ingesteld aanvraagt op dynamisch worden versleuteld, gebruikt Media Services de geconfigureerde sleutel voor het versleutelen van uw inhoud dynamisch met behulp van versleuteling AES of DRM. Voor het ontsleutelen van de stroom, vraagt Windows media player de sleutel van de service sleutellevering. De service beoordeelt om te bepalen of de gebruiker is gemachtigd om op te halen van de sleutel, de autorisatie-beleidsregels die u hebt opgegeven voor de sleutel.

Als u wilt meerdere inhoudssleutels hebben of een sleutel/licentie delivery service URL dan de sleutellevering Media Services-service wilt opgeven, gebruikt u de Media Services .NET SDK of de REST-API's. Zie voor meer informatie:

* [Een autorisatiebeleid voor inhoudssleutels configureren met behulp van de Media Services .NET SDK](media-services-dotnet-configure-content-key-auth-policy.md)
* [Een autorisatiebeleid voor inhoudssleutels configureren met behulp van de REST-API van Media Services](media-services-rest-configure-content-key-auth-policy.md)

### <a name="some-considerations-apply"></a>Enkele overwegingen zijn van toepassing
* Wanneer uw Media Services-account is gemaakt, wordt een standaard streaming-eindpunt toegevoegd aan uw account in de status 'Gestopt'. Om te beginnen uw inhoud te streamen en te profiteren van dynamische pakketten en dynamische versleuteling, moet uw streaming-eindpunt in de status 'Actief'. 
* Uw asset moet een set adaptive bitrate MP4s of adaptive bitrate Smooth Streaming-bestanden bevatten. Zie voor meer informatie [een asset coderen](media-services-encode-asset.md).
* De service sleutellevering caches ContentKeyAuthorizationPolicy en de verwante objecten (beleidsopties en beperkingen) gedurende 15 minuten. U kunt een ContentKeyAuthorizationPolicy maken en aangeven dat u wilt gebruiken een token beperking, testen en het beleid vervolgens bijwerken naar de beperking die wordt geopend. Dit proces duurt ongeveer 15 minuten voordat de beleid-switches op de geopende versie.
* Een Media Services streaming-eindpunt stelt u de waarde van de header CORS Access Control-toestaan-oorsprong in voorbereidende antwoord als het jokerteken '\*'. Deze waarde werkt goed samen met de meeste spelers, waaronder Azure Media Player, Roku en JWPlayer en anderen. Evenwel sommige spelers die gebruikmaken van dash.js werken niet omdat de referenties modus ingesteld op "bevatten" XMLHttpRequest in hun dash.js het jokerteken mag geen '\*' als de waarde van de Access Control-toestaan-oorsprong. Als een oplossing voor deze beperking in dash.js als host van de client uit één domein: kunt Media Services opgeven dat domein in de voorbereidende response-header. Open een ondersteuningsticket via de Azure portal voor assistentie.

## <a name="configure-the-key-authorization-policy"></a>Het autorisatiebeleid voor inhoudssleutels configureren
Als u wilt het autorisatiebeleid voor inhoudssleutels configureren, selecteert u de **INHOUDSBEVEILIGING** pagina.

Media Services ondersteunt meerdere manieren om gebruikers die sleutels aanvragen te verifiëren. Het autorisatiebeleid voor inhoudssleutels kan zijn geopend, token of beperkingen voor IP-autorisatie. (IP kan worden geconfigureerd met REST of de .NET SDK.)

### <a name="open-restriction"></a>Open beperking
De beperking die wordt geopend, betekent dat het systeem biedt de sleutel voor iedereen die een sleutel aanvraag indient. Deze beperking kan handig zijn voor testdoeleinden.

![OpenPolicy][open_policy]

### <a name="token-restriction"></a>Tokenbeperking
Als u beleid met de tokenbeperking, selecteer de **TOKEN** knop.

Beleid met de tokenbeperking moet vergezeld gaan van een token dat is uitgegeven door een beveiligingstokenservice (STS). Media Services ondersteunt tokens in het eenvoudige web token ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) en JSON Web Token (JWT) geformatteerd. Zie voor meer informatie [JWT verificatie](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media Services biedt niet STS. U kunt een aangepaste STS maken of gebruiken van Azure Access Control Service voor het probleem van tokens. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel- en claims die u hebt opgegeven in de configuratie van de tokenbeperking. Als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de inhoudssleutel, retourneert de sleutellevering van Media Services de versleutelingssleutel voor de client. Zie voor meer informatie [Azure Access Control Service gebruiken voor het probleem van tokens](http://mingfeiy.com/acs-with-key-services).

Wanneer u het beleid token beperkt configureert, moet u de verificatie van de primaire sleutel, uitgever en doelgroep parameters opgeven. De verificatie van de primaire sleutel bevat de sleutel die het token is ondertekend met. De certificaatverlener is de STS die de token uitgeeft. De doelgroep (ook wel bereik genoemd) beschrijft de intentie van het token of de resource het token gemachtigd voor toegang tot. De Media Services-service sleutellevering valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

### <a name="playready"></a>PlayReady
Wanneer u uw inhoud met PlayReady beveiligt, is een van de dingen die u wilt opgeven in het autorisatiebeleid een XML-tekenreeks die definieert de sjabloon PlayReady-licenties. Standaard wordt het volgende beleid ingesteld:

    <PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
          <LicenseTemplates>
            <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices>
              <ContentKey i:type="ContentEncryptionKeyFromHeader" />
              <LicenseType>Nonpersistent</LicenseType>
              <PlayRight>
                <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput>
              </PlayRight>
            </PlayReadyLicenseTemplate>
          </LicenseTemplates>
        </PlayReadyLicenseResponseTemplate>

U kunt selecteren de **beleid-xml importeren** knop en geef een andere XML-bestand dat aan het XML-schema dat is gedefinieerd voldoet in de [Media Services PlayReady licentie sjabloon overzicht](media-services-playready-license-template-overview.md).

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

