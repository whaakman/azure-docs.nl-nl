---
title: Uw inhoud beveiligen met behulp van Media Services Dynamic Encryption-Azure | Microsoft Docs
description: Dit artikel geeft een overzicht van de beveiliging van inhoud met dynamische versleuteling. Er wordt ook gepraat over streaming protocollen en versleutelings typen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 5d31e4a523fdedf9907e33c70638f07a08461ed1
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68310306"
---
# <a name="content-protection-with-dynamic-encryption"></a>Beveiliging van inhoud met dynamische versleuteling

U kunt Azure Media Services gebruiken voor het beveiligen van uw media vanaf het moment dat het verlaten van uw computer via opslag, verwerking en levering. Met Media Services kunt u uw Live en on-demand inhoud dynamisch versleutelen met Advanced Encryption Standard (AES-128) of een van de drie belangrijkste Digital Rights Management (DRM)-systemen: Micro soft PlayReady, Google Widevine en Apple FairPlay. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM (PlayReady, Widevine en FairPlay) licenties voor geautoriseerde clients. 

In Media Services v3 is een inhouds sleutel gekoppeld aan streaming-Locator (Zie [dit voor beeld](protect-with-aes128.md)). Als u de Media Services key delivery-service gebruikt, kunt u Azure Media Services de inhouds sleutel voor u laten genereren. U moet de inhouds sleutel zelf genereren als u de service voor de levering van sleutels gebruikt, of als u een scenario met een hoge Beschik baarheid wilt afhandelen waarbij u dezelfde inhouds sleutel moet hebben in twee data centers.

Wanneer een stroom wordt aangevraagd door een speler, Media Services maakt gebruik van de opgegeven sleutel voor het versleutelen van uw inhoud dynamisch met behulp van de lege sleutel AES of DRM-versleuteling. Voor het ontsleutelen van de stroom, vraagt de speler de sleutel van Media Services-sleutelleveringsservice of de sleutelleveringsservice die u hebt opgegeven. Om te beslissen of de gebruiker is gemachtigd om op te halen van de sleutel, de service beoordeelt wat het beleid voor het sleutels van inhoud die u hebt opgegeven voor de sleutel.

U kunt de REST-API of een Media Services-clientbibliotheek gebruiken om autorisatie en verificatie beleid voor uw licenties en sleutels te configureren.

De volgende afbeelding ziet u de Media Services content protection-werkstroom: 

![Inhoud beveiligen](./media/content-protection/content-protection.svg)

&#42;*dynamische versleuteling ondersteunt AES-128 "clear key', CBCS en CENC. Zie voor meer informatie de ondersteuningsmatrix [hier](#streaming-protocols-and-encryption-types).*

In dit artikel wordt uitgelegd concepten en terminologie die relevant zijn voor informatie over de beveiliging van inhoud met Media Services.

## <a name="main-components-of-a-content-protection-system"></a>Belangrijkste onderdelen van een systeem voor beveiliging van inhoud

Als u wilt uw systeem-/ toepassingsontwerp 'content protection' is voltooid, moet u volledig inzicht in het bereik van de inspanningen. De volgende lijst biedt een overzicht van de drie onderdelen die u moet implementeren. 

1. Azure Media Services-code
  
   Het [DRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs) -voor beeld laat zien hoe u een multi-DRM-systeem met Media Services v3 implementeert met behulp van .net. Ook wordt uitgelegd hoe u Media Services licentie/key delivery service gebruikt. U kunt elke asset met meerdere versleutelingstypen versleutelen (AES-128, PlayReady, Widevine, FairPlay). Zie [Streamingprotocollen en versleutelingstypen](#streaming-protocols-and-encryption-types) voor nuttige combinaties.
  
   Het voorbeeld wordt getoond hoe u:

   1. Een [beleid voor inhouds sleutels](content-key-policy-concept.md)maken en configureren. U maakt een **inhouds sleutel beleid** om te configureren hoe de inhouds sleutel (die beveiligde toegang tot uw assets biedt) wordt geleverd aan de eind clients.    

      * Definieer de licentie leverings autorisatie, waarbij u de logica van autorisatie controle opgeeft op basis van claims in JWT.
      * [PlayReady](playready-license-template-overview.md)-, [Widevine](widevine-license-template-overview.md)-en/of [Fairplay](fairplay-license-overview.md) -licenties configureren. Met de sjablonen kunt u rechten en machtigingen voor elk van de gebruikte DRMs configureren.

        ```
        ContentKeyPolicyPlayReadyConfiguration playReadyConfig = ConfigurePlayReadyLicenseTemplate();
        ContentKeyPolicyWidevineConfiguration widevineConfig = ConfigureWidevineLicenseTempate();
        ContentKeyPolicyFairPlayConfiguration fairPlayConfig = ConfigureFairPlayPolicyOptions();
        ```
   2. Maak een [streaming-Locator](streaming-locators-concept.md) die is geconfigureerd voor het streamen van het versleutelde activum. 
  
      De **streaming-Locator** moet worden gekoppeld aan een [streaming-beleid](streaming-policy-concept.md). In het voor beeld stellen we StreamingLocator. StreamingPolicyName in op het beleid "Predefined_MultiDrmCencStreaming". De PlayReady-en Widevine-versleuteling worden toegepast. de sleutel wordt aan de client voor afspelen geleverd op basis van de geconfigureerde DRM-licenties. Als u wilt dat uw stream ook met CBCS (FairPlay) wordt versleuteld, gebruikt u Predefined_MultiDrmStreaming.
      
      De streaming-Locator is ook gekoppeld aan het **inhouds sleutel beleid** dat is gedefinieerd.
    
   3. Maken van een test-token.

      De **GetTokenAsync** methode geeft over het maken van een test-token.
   4. De streaming-URL maken.

      De **GetDASHStreamingUrlAsync** methode laat zien hoe u de streaming-URL maken. In dit geval wordt de URL-stromen de **DASH** inhoud.

2. De speler met AES of DRM-client. Een videospeler-app op basis van een player SDK (systeemeigen of browsergebaseerde) moet voldoen aan de volgende vereisten:
   * Windows media player SDK biedt ondersteuning voor de benodigde DRM-clients
   * De Player SDK ondersteunt de vereiste streaming-protocollen: Vloeiend, streepje en/of HLS
   * Windows media player SDK moet kunnen zijn voor het afhandelen van een JWT-token wordt doorgegeven in licentie overname aanvraag
  
     U kunt een speler maken met behulp van de [API van Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/). Gebruik de [API van Azure Media Player ProtectionInfo](https://amp.azure.net/libs/amp/latest/docs/) om op te geven welke DRM-technologie kunt gebruiken voor verschillende DRM-platforms.

     Voor testen AES of CENC (Widevine en/of PlayReady) gecodeerde inhoud, kunt u [Azure Media Player](https://aka.ms/azuremediaplayer). Zorg ervoor dat u op 'Geavanceerde opties' en de opties voor versleuteling.

     Als u testen van FairPlay gecodeerde inhoud wilt, gebruikt u [deze test player](https://aka.ms/amtest). Windows media player ondersteunt Widevine, PlayReady, en FairPlay DRM's, evenals de AES-128 clear key-versleuteling. 
    
     U moet de juiste browser kiezen om verschillende DRMs te testen: Chrome/Opera/Firefox voor Widevine, micro soft Edge/IE11 voor PlayReady, Safari op macOS voor FairPlay.

3. Secure Token Service (STS), geeft u JSON Web Token (JWT) als het toegangstoken voor toegang tot back-end. U kunt de AMS-services voor het leveren van licentie gebruiken als de back endresource. Een STS heeft voor het definiëren van het volgende:

   * Verlener en doelgroep (of bereik)
   * Claims die afhankelijk van de zakelijke vereisten in de beveiliging van inhoud zijn
   * Symmetrisch als asymmetrisch controle voor controle van handtekening
   * Rollover van ondertekeningssleutel ondersteuning (indien nodig)

     U kunt [Dit hulp programma STS](https://openidconnectweb.azurewebsites.net/DRMTool/Jwt) gebruiken om de STS te testen, die ondersteuning biedt voor alle drie typen verificatie sleutel: symmetrisch, asymmetrisch of Azure AD met sleutel rollover. 

> [!NOTE]
> Het is zeer aanbevolen zich richten en elk onderdeel (hierboven beschreven) volledig hebt getest voordat u verplaatsen naar het volgende gedeelte. U kunt uw systeem 'content protection' testen met de hulpprogramma's opgegeven in de bovenstaande lijst.  

## <a name="streaming-protocols-and-encryption-types"></a>Protocollen voor streaming en versleutelingstypen

U kunt Media Services gebruiken om uw inhoud dynamisch versleuteld met AES clear key- of DRM-versleuteling met behulp van PlayReady, Widevine en FairPlay te leveren. Op dit moment kunt u de HTTP Live Streaming (HLS), MPEG DASH en Smooth Streaming-indelingen versleutelen. Elk protocol ondersteunt de volgende versleutelingsmethoden:

### <a name="hls"></a>HLS

Het HLS-protocol ondersteunt de volgende container indelingen en versleutelings schema's.

|Containerindeling|Versleutelingsmethode|Voor beeld van URL|
|---|---|---|
|Alle|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cbc)`|
|MPG2-TS |CBCS (FairPlay) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cbcs-aapl)`|
|CMAF(fmp4) |CBCS (FairPlay) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-cmaf,encryption=cbcs-aapl)`|
|MPG2-TS |CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cenc)`|
|CMAF(fmp4) |CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-cmaf,encryption=cenc)`|

HLS/CMAF + FairPlay (inclusief HEVC/H. 265) wordt ondersteund op de volgende apparaten:

* iOS V11 of hoger 
* iPhone 8 of hoger
* MacOS High Sierra met Intel 7 gen CPU

### <a name="mpeg-dash"></a>MPEG-DASH

Het MPEG-DASH-protocol ondersteunt de volgende container indelingen en versleutelings schema's.

|Containerindeling|Versleutelingsmethode|URL-voor beelden
|---|---|---|
|Alle|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-csf,encryption=cbc)`|
|CSF(fmp4) |CENC (Widevine + PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-csf,encryption=cenc)`|
|CMAF(fmp4)|CENC (Widevine + PlayReady)|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-cmaf,encryption=cenc)`|

### <a name="smooth-streaming"></a>Smooth Streaming

Het Smooth Streaming-Protocol ondersteunt de volgende container indelingen en versleutelings schema's.

|Protocol|Containerindeling|Versleutelingsmethode|
|---|---|---|
|fMP4|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(encryption=cbc)`|
|fMP4 | CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(encryption=cenc)`|

### <a name="browsers"></a>Browsers

Algemene browsers ondersteunen de volgende DRM-clients:

|Browser|Versleuteling|
|---|---|
|Chrome|Widevine|
|Micro soft Edge, IE 11|PlayReady|
|Firefox|Widevine|
|Opera|Widevine|
|Safari|FairPlay|

## <a name="control-content-access"></a>Toegang tot de inhoud van besturingselement

U kunt bepalen wie toegang heeft tot uw inhoud door het configureren van het beleid voor de inhoud van de sleutels. Media Services ondersteunt meerdere manieren om gebruikers te autoriseren die sleutels aanvragen. Het beleid voor de inhoud van de sleutels, moet u configureren. De client (speler) moet voldoen aan van het beleid voordat de sleutel kan worden geleverd aan de client. Het beleid voor de inhoud van de sleutels kan hebben **open** of **token** beperking. 

Met een tokenbeperking content key-beleid, is de inhoudssleutel alleen verzonden naar een client met daarin een geldige JSON Web Token (JWT) of simple webtoken (SWT) in de aanvraag voor sleutels/licenties. Dit token moet worden uitgegeven door een beveiligingstokenservice (STS). U kunt Azure Active Directory gebruiken als een STS of implementeren van een aangepaste STS. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel en probleem claims die u hebt opgegeven in de configuratie van de tokenbeperking. De belangrijkste leveringsservice voor Media Services retourneert de aangevraagde sleutels/licenties naar de client als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de sleutels/licenties.

Wanneer u beleid met de tokenbeperking configureert, moet u de primaire verificatiesleutel, uitgever en doelgroep parameters opgeven. De primaire verificatiesleutel bevat de sleutel die het token is ondertekend. De uitgever wordt de secure token service die de token uitgeeft. De doelgroep, ook wel genoemd bereik, het doel van het token wordt beschreven of de resource voor het token wordt toegang tot geautoriseerd. De Media Services-sleutelleveringsservice valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

Klanten gebruiken vaak een aangepaste STS om aangepaste claims in het token op te nemen om te selecteren tussen verschillende ContentKeyPolicyOptions met verschillende DRM-licentie parameters (een abonnements licentie versus een huur licentie) of een claim op te nemen die de inhouds sleutel bevat de id van de sleutel waarmee het token toegang verleent.

### <a name="token-replay-prevention"></a>Token replay voor komen

Met de functie voor het voor *komen* van tokens kunnen Media Services klanten een limiet instellen voor het aantal keren dat hetzelfde token kan worden gebruikt om een sleutel of licentie aan te vragen. De klant kan een claim van het type `urn:microsoft:azure:mediaservices:maxuses` in het token toevoegen, waarbij de waarde het aantal keren is dat het token kan worden gebruikt om een licentie of sleutel te verkrijgen. Bij alle volgende aanvragen met dezelfde token voor de sleutel levering wordt een niet-geautoriseerde reactie geretourneerd. Zie hoe u de claim toevoegt in het [DRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L601)-voor beeld.
 
#### <a name="considerations"></a>Overwegingen

* Klanten moeten controle hebben over het genereren van tokens. De claim moet in het token zelf worden geplaatst.
* Wanneer u deze functie gebruikt, worden aanvragen met tokens waarvan de verval tijd meer dan één uur duurt, verwijderd met een niet-geautoriseerde reactie.
* Tokens worden uniek geïdentificeerd door hun hand tekening. Elke wijziging in de payload (bijvoorbeeld bijwerken naar de verloop tijd of de claim) wijzigt de hand tekening van het token en telt als een nieuw token dat de sleutel levering niet eerder is aangetroffen.
* Afspelen mislukt als het token de `maxuses` waarde die is ingesteld door de klant heeft overschreden.
* Deze functie kan worden gebruikt voor alle bestaande beveiligde inhoud (alleen het token dat is uitgegeven moet worden gewijzigd).
* Deze functie werkt met zowel de JWT-als de SWT.

## <a name="custom-key-and-license-acquisition-url"></a>Aangepaste sleutel en licentie voor aanschaf-URL

Gebruik de volgende sjablonen als u een andere sleutel en een service voor licentie levering wilt opgeven (niet Media Services). De twee Vervang bare velden in de sjablonen zijn daar, zodat u uw streaming-beleid kunt delen in veel assets in plaats van een streaming-beleid per activum te maken. 

* EnvelopeEncryption. CustomKeyAcquisitionUrlTemplate: de sjabloon voor de URL van de aangepaste service die sleutels levert aan spelers van eind gebruikers. Niet vereist wanneer Azure Media Services wordt gebruikt voor het uitgeven van sleutels. De sjabloon ondersteunt vervangbaar tokens die door de service worden bijgewerkt tijdens runtime met de waarde die specifiek is voor de aanvraag.  De momenteel ondersteunde token waarden zijn {AlternativeMediaId}. deze worden vervangen door de waarde van StreamingLocatorId. AlternativeMediaId en {ContentKeyId}, die wordt vervangen door de waarde van de id van de sleutel die wordt aangevraagd.
* StreamingPolicyPlayReadyConfiguration. CustomLicenseAcquisitionUrlTemplate: de sjabloon voor de URL van de aangepaste service voor het leveren van licenties aan eind gebruikers. Niet vereist wanneer Azure Media Services wordt gebruikt voor het uitgeven van licenties. De sjabloon ondersteunt vervangbaar tokens die door de service worden bijgewerkt tijdens runtime met de waarde die specifiek is voor de aanvraag. De momenteel ondersteunde token waarden zijn {AlternativeMediaId}. deze worden vervangen door de waarde van StreamingLocatorId. AlternativeMediaId en {ContentKeyId}, die wordt vervangen door de waarde van de id van de sleutel die wordt aangevraagd. 
* StreamingPolicyWidevineConfiguration. CustomLicenseAcquisitionUrlTemplate-zelfde als hierboven, alleen voor Widevine. 
* StreamingPolicyFairPlayConfiguration. CustomLicenseAcquisitionUrlTemplate-zelfde als hierboven, alleen voor FairPlay.  

Bijvoorbeeld:

```csharp
streamingPolicy.EnvelopEncryption.customKeyAcquisitionUrlTemplate = "https://mykeyserver.hostname.com/envelopekey/{AlternativeMediaId}/{ContentKeyId}";
```

De `ContentKeyId` heeft een waarde van de sleutel die wordt aangevraagd en `AlternativeMediaId` de kan worden gebruikt als u de aanvraag aan een entiteit aan uw zijde wilt toewijzen. De kan bijvoorbeeld worden `AlternativeMediaId` gebruikt om u te helpen bij het opzoeken van machtigingen.

Zie [streaming-beleid-maken](https://docs.microsoft.com/rest/api/media/streamingpolicies/create) voor rest-voor beelden die gebruikmaken van url's voor aangepaste sleutel-en licentie-aanschaf

## <a name="troubleshoot"></a>Problemen oplossen

Als u de `MPE_ENC_ENCRYPTION_NOT_SET_IN_DELIVERY_POLICY` fout melding krijgt, moet u het juiste streaming-beleid opgeven.

Als er fouten optreden die eindigen `_NOT_SPECIFIED_IN_URL`op, zorg er dan voor dat u de versleutelings indeling opgeeft in de URL. Bijvoorbeeld `…/manifest(format=m3u8-cmaf,encryption=cbcs-aapl)`. Zie [streaming protocollen en versleutelings typen](#streaming-protocols-and-encryption-types).

## <a name="ask-questions-give-feedback-get-updates"></a>Vragen stellen, feedback geven, updates ophalen

Bekijk het [Azure Media Services Community](media-services-community.md) -artikel voor verschillende manieren om vragen te stellen, feedback te geven en updates te ontvangen over Media Services.

## <a name="next-steps"></a>Volgende stappen

* [Beveiligen met AES-versleuteling](protect-with-aes128.md)
* [Beveiligen met DRM](protect-with-drm.md)
* [Multi-DRM-inhouds beschermings systeem ontwerpen met toegangs beheer](design-multi-drm-system-with-access-control.md)
* [Versleuteling van opslag side](storage-account-concept.md#storage-side-encryption)
* [Veelgestelde vragen](frequently-asked-questions.md)

