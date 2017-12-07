---
title: Autorisatiebeleid voor Inhoudssleutels met de Azure portal configureren | Microsoft Docs
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
ms.openlocfilehash: 36ec76718d21cac5ae3203f1c6d4b8af2aacb9ed
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="configure-content-key-authorization-policy"></a>Autorisatiebeleid voor Inhoudssleutels configureren
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Overzicht
Microsoft Azure Media Services kunt u MPEG-DASH-, Smooth Streaming- en HTTP-Live-Streaming (HLS)-streams beveiligd met Advanced Encryption Standard (AES) (met behulp van coderingssleutels 128-bits) leveren of [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS kunt u ook versleuteld met Widevine DRM DASH-streams leveren. Zowel PlayReady als Widevine zijn versleuteld volgens de specificatie Common Encryption (ISO/IEC 23001-7 CENC).

Media Services biedt ook een **Service voor het leveren van sleutel/licentie** waarvan clients kunt verkrijgen AES-sleutels of PlayReady/Widevine-licenties de versleutelde inhoud af te spelen.

In dit artikel laat zien hoe de Azure portal gebruiken voor het autorisatiebeleid voor inhoudssleutels configureren. De sleutel kan later worden gebruikt voor het dynamisch versleutelen van uw inhoud. Momenteel kunt u de volgende streaming-indelingen coderen: HLS, MPEG DASH en Smooth Streaming. U kunt progressief downloaden niet coderen.

Wanneer een speler een stroom die is ingesteld aanvraagt op dynamisch worden versleuteld, gebruikt Media Services de geconfigureerde sleutel voor het dynamisch versleutelen van uw inhoud met behulp van versleuteling AES of DRM. Voor het ontsleutelen van de stroom, vraagt Windows media player de sleutel van de service sleutellevering. De service beoordeelt om te bepalen of de gebruiker is gemachtigd om op te halen van de sleutel, de autorisatie-beleidsregels die u hebt opgegeven voor de sleutel.

Als u wilt meerdere inhoudssleutels hebben of wilt opgeven als een **Service voor het leveren van sleutel/licentie** URL dan de Media Services-service sleutellevering Media Services .NET SDK of REST-API's gebruiken.

[Autorisatiebeleid voor Inhoudssleutels met Media Services .NET SDK configureren](media-services-dotnet-configure-content-key-auth-policy.md)

[Autorisatiebeleid voor Inhoudssleutels met Media Services REST-API configureren](media-services-rest-configure-content-key-auth-policy.md)

### <a name="some-considerations-apply"></a>Hierbij geldt het volgende:
* Wanneer uw AMS-account wordt gemaakt een **standaard** streaming-eindpunt wordt toegevoegd aan uw account in de **gestopt** status. Om te beginnen uw inhoud te streamen en te profiteren van dynamische pakketten en dynamische versleuteling, uw streaming-eindpunt is in de **met** status. 
* Uw asset moet een set adaptive bitrate MP4s of adaptive bitrate Smooth Streaming-bestanden bevatten. Zie voor meer informatie [een asset coderen](media-services-encode-asset.md).
* De service voor het leveren van de sleutel in de cache opgeslagen ContentKeyAuthorizationPolicy en de verwante objecten (beleidsopties en beperkingen) gedurende 15 minuten.  Als u een ContentKeyAuthorizationPolicy maken en geef voor het gebruik van een '' tokenbeperking, test deze, en werk vervolgens het beleid op 'Open' beperking, duurt het ongeveer 15 minuten voordat het beleid wordt overgeschakeld naar de 'Geopende' versie van het beleid.
* Streaming-eindpunt AMS stelt u de waarde van de header CORS 'Access Control-toestaan-oorsprong' voorbereidende reactie als het jokerteken '\*'. Dit werkt goed samen met de meeste spelers, waaronder onze Azure Media Player, Roku en JW en anderen. Evenwel sommige spelers die gebruikmaken van dashjs werken niet omdat met referenties modus ingesteld op "bevatten", XMLHttpRequest in hun dashjs geen jokerteken staat '\*' als de waarde van ' ' Access Control-toestaan-oorsprong '. Als een andere manier om deze beperking in dashjs, als u de client van een enkel domein host kunt Azure Media Services opgeven dat domein in de voorbereidende response-header. U kunt bereiken door een ondersteuningsticket via Azure portal te openen.

## <a name="how-to-configure-the-key-authorization-policy"></a>Procedure: het autorisatiebeleid voor inhoudssleutels configureren
Als u wilt het autorisatiebeleid voor inhoudssleutels configureren, selecteert u de **INHOUDSBEVEILIGING** pagina.

Media Services ondersteunt meerdere manieren om gebruikers te verifiëren die sleutels aanvragen. Het autorisatiebeleid voor inhoudssleutels kan hebben **openen**, **token**, of **IP** autorisatiebeperkingen (**IP** kan worden geconfigureerd met de REST- of .NET SDK).

### <a name="open-restriction"></a>Open beperking
De **openen** beperking betekent dat het systeem biedt de sleutel voor iedereen die een sleutel aanvraag indient. Deze beperking kan handig zijn voor testdoeleinden.

![OpenPolicy][open_policy]

### <a name="token-restriction"></a>Tokenbeperking
Als u beleid met de tokenbeperking, drukt u op de **TOKEN** knop.

De **token** beleid voor de beperkte vergezeld van een token dat is uitgegeven door een **Secure Token Service** (STS). Media Services ondersteunt tokens in de **Simple Web Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) indeling en **JSON Web Token** (JWT)-indeling. Zie voor informatie [JWT tokenverificatie](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media Services biedt geen **Secure Token Services**. U kunt een aangepaste STS maken of gebruikmaken van Microsoft Azure ACS voor het probleem van tokens. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel- en claims die u hebt opgegeven in de configuratie van de tokenbeperking. De sleutellevering van Media Services wordt de versleutelingssleutel geretourneerd naar de client als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de inhoudssleutel. Zie voor meer informatie [gebruik Azure ACS voor het probleem van tokens](http://mingfeiy.com/acs-with-key-services).

Bij het configureren van de **token** beperkt beleid, moet u de primaire **verificatie sleutel**, **verlener** en **doelgroep** parameters. De primaire **verificatie sleutel** bevat de sleutel die het token is ondertekend, **verlener** is de secure token service die de token uitgeeft. De **doelgroep** (ook wel genoemd **bereik**) beschrijft de intentie van het token of de resource die is gemachtigd voor toegang tot het token. De Media Services-service sleutellevering valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

### <a name="playready"></a>PlayReady
Bij het beveiligen van uw inhoud met **PlayReady**, een van de dingen die u wilt opgeven in het autorisatiebeleid is een XML-tekenreeks die definieert de sjabloon PlayReady-licenties. Standaard wordt het volgende beleid ingesteld:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1"><LicenseTemplates> <PlayReadyLicenseTemplate> <AllowTestDevices>true</AllowTestDevices> <ContentKey i:type="ContentEncryptionKeyFromHeader" /> <LicenseType>Nonpersistent</LicenseType> <PlayRight> <AllowPassingVideoContentToUnknownOutput>toegestane</AllowPassingVideoContentToUnknownOutput> </PlayRight> </PlayReadyLicenseTemplate> </LicenseTemplates></PlayReadyLicenseResponseTemplate>

U kunt klikken op de **beleid-xml importeren** knop en geeft u een andere XML-bestand dat voldoet aan de XML-Schema gedefinieerd [hier](media-services-playready-license-template-overview.md).

## <a name="next-steps"></a>Volgende stappen
Media Services-leertrajecten bekijken.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

