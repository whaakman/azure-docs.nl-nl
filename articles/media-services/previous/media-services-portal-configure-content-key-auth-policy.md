---
title: Een autorisatiebeleid voor inhoudssleutels configureren met behulp van Azure portal | Microsoft Docs
description: Informatie over het configureren van een autorisatiebeleid voor een inhoudssleutel.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: ee82a3fa-c34b-48f2-a108-8ba321f1691e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: juliako
ms.openlocfilehash: 7dd056042b841e54c18ee1a667c44cfa11d77a61
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56164597"
---
# <a name="configure-a-content-key-authorization-policy"></a>Een autorisatiebeleid voor inhoudssleutels configureren
[!INCLUDE [media-services-selector-content-key-auth-policy](../../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Overzicht
 U kunt Azure Media Services MPEG-DASH, Smooth Streaming en HTTP Live Streaming (HLS)-streams met Advanced Encryption Standard (AES) beveiligd met behulp van 128-bits versleutelingssleutels leveren of [PlayReady digital rights management (DRM)](https://www.microsoft.com/playready/overview/). Met Media Services, kunt u ook versleuteld met Widevine DRM DASH-streams leveren. Zowel PlayReady als Widevine zijn versleuteld volgens de specificatie Common Encryption (ISO/IEC 23001-7 CENC).

Media Services biedt ook een leveringsservice voor sleutels/licenties waarvan clients sleutels voor AES of PlayReady/Widevine-licenties om af te spelen de versleutelde inhoud kunnen ophalen.

In dit artikel laat zien hoe het gebruik van Azure portal het autorisatiebeleid voor inhoudssleutels configureren. De sleutel kan later worden gebruikt voor het dynamisch versleutelen van uw inhoud. Op dit moment kunt u coderen HLS, MPEG-DASH en Smooth Streaming-indelingen. U kunt geen progressieve downloads coderen.

Wanneer een speler vraagt om een stroom die is ingesteld op dynamisch worden versleuteld, Media Services de geconfigureerde sleutel gebruikt voor het versleutelen van uw inhoud dynamisch met behulp van versleuteling met AES of DRM. Voor het ontsleutelen van de stream, wordt door de speler de sleutel van de sleutelleveringsservice aangevraagd. Om te bepalen dat of de gebruiker is gemachtigd om op te halen van de sleutel, de service beoordeelt wat de autorisatiebeleidsregels die u hebt opgegeven voor de sleutel.

Als u van plan bent meerdere inhoudssleutels hebben of een URL sleutels/licenties delivery service dan de belangrijkste leveringsservice voor Media Services wilt opgeven, gebruikt u de Media Services .NET SDK of REST-API's. Zie voor meer informatie:

* [Een autorisatiebeleid voor inhoudssleutels configureren met behulp van de Media Services .NET SDK](media-services-dotnet-configure-content-key-auth-policy.md)
* [Een autorisatiebeleid voor inhoudssleutels configureren met behulp van de Media Services REST-API](media-services-rest-configure-content-key-auth-policy.md)

### <a name="some-considerations-apply"></a>Enkele overwegingen zijn van toepassing
* Wanneer uw Media Services-account is gemaakt, wordt er een standaard streaming-eindpunt met de status Gestopt aan uw account toegevoegd. Als u wilt uw inhoud wilt streamen en te profiteren van dynamische pakketten en dynamische versleuteling, moet uw streaming-eindpunt in de status 'Running'. 
* Uw asset moet een set adaptive bitrate MP4s of adaptive bitrate Smooth Streaming-bestanden bevatten. Zie voor meer informatie, [een asset coderen](media-services-encode-asset.md).
* De sleutelleveringsservice caches ContentKeyAuthorizationPolicy en de verwante objecten (opties en beperkingen) voor 15 minuten. U kunt een ContentKeyAuthorizationPolicy maken en geef voor het gebruik van een beperking van de token, testen en vervolgens het beleid bijwerken naar de beperking die wordt geopend. Dit proces duurt ongeveer 15 minuten voordat de beleid-switches op de geopende versie.
* Een Media Services streaming-eindpunt stelt de waarde van de CORS Access-Control-Allow-Origin-header in voorbereidende antwoord als het jokerteken '\*'. Deze waarde werkt goed samen met de meeste spelers, met inbegrip van Azure Media Player, Roku en JWPlayer en anderen. Evenwel sommige spelers met dash.js werken niet omdat, met referenties-modus is ingesteld op "bevatten", XMLHttpRequest in hun dash.js is niet toegestaan voor het jokerteken '\*' als de waarde van de Access-Control-Allow-Origin. Als tijdelijke oplossing voor deze beperking in dash.js, als host fungeert voor de client van een enkel domein, kunt Media Services opgeven dat domein in de voorbereidende antwoordkop. Open een ondersteuningsticket in via de Azure-portal voor hulp.

## <a name="configure-the-key-authorization-policy"></a>Het autorisatiebeleid voor inhoudssleutels configureren
Als u wilt het autorisatiebeleid voor inhoudssleutels configureren, selecteert u de **CONTENT PROTECTION** pagina.

Media Services ondersteunt meerdere manieren om gebruikers te verifiëren die sleutels aanvragen. Het autorisatiebeleid voor inhoudssleutels kan zijn geopend, token of IP-beperkingen voor autorisatie. (IP kan worden geconfigureerd met REST of de .NET SDK).

### <a name="open-restriction"></a>Open beperking
De open beperking betekent dat het systeem levert de sleutel aan iedereen die sleutels aanvragen. Deze beperking kan handig zijn voor testdoeleinden.

![OpenPolicy][open_policy]

### <a name="token-restriction"></a>Beperking van token
Als u beleid met de tokenbeperking, selecteer de **TOKEN** knop.

Beleid met de tokenbeperking moet vergezeld gaan van een token dat is uitgegeven door een beveiligingstokenservice (STS). Media Services ondersteunt tokens in de simple web tokens ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) en JSON Web Token (JWT) geformatteerd. Zie voor meer informatie, [JWT verificatie](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media Services biedt geen STS. U kunt een aangepaste STS voor het uitgeven van tokens. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie van de tokenbeperking. Als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de inhoudssleutel, retourneert de sleutelleveringsservice van Media Services de versleutelingssleutel voor de client.

Wanneer u de tokenbeperking beleid configureert, moet u de primaire verificatiesleutel, uitgever en doelgroep parameters opgeven. De primaire verificatiesleutel bevat de sleutel die het token is ondertekend. De uitgever wordt de STS die de token uitgeeft. De doelgroep (ook wel bereik) beschrijft de intentie van de token of de resource voor het token wordt toegang tot geautoriseerd. De Media Services-sleutelleveringsservice valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

### <a name="playready"></a>PlayReady
Wanneer u uw inhoud met PlayReady beveiligt, is een van de dingen die u nodig hebt om op te geven in het autorisatiebeleid voor een XML-tekenreeks die de PlayReady-licentiesjabloon definieert. Standaard wordt het volgende beleid ingesteld:

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

U kunt selecteren de **XML-bestand importeren** knop en geef een ander XML-bestand dat aan de XML-schema dat is gedefinieerd voldoet in de [overzicht van sjablonen voor Media Services PlayReady-licentie](media-services-playready-license-template-overview.md).

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

