---
title: Ondersteuning van Azure Media Services en Apple FairPlay licentie | Microsoft Docs
description: In dit onderwerp biedt een overzicht van een licentie voor Apple FairPlay vereisten en de configuratie.
author: juliako
manager: cfowler
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2018
ms.author: juliako
ms.openlocfilehash: a68896d061040843990318cbc39eaf1aaa3c8b27
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39115111"
---
# <a name="apple-fairplay-license-requirements-and-configuration"></a>Vereisten voor Apple FairPlay-licenties en configuratie 

Azure Media Services kunt u voor het versleutelen van uw HLS-inhoud met **Apple FairPlay** (AES-128 CBC). Media Services biedt ook een service voor het leveren van FairPlay-licenties. Wanneer een speler probeert uw FairPlay beveiligde inhoud af te spelen, wordt een aanvraag verzonden naar de service voor het leveren van licenties om een licentie te verkrijgen. Als de licentieservice de aanvraag goedkeurt, geeft deze de licentie die is verzonden naar de client en wordt gebruikt om te ontsleutelen en de opgegeven inhoud af te spelen.

Media Services biedt ook API's die u gebruiken kunt om uw FairPlay-licenties te configureren. In dit onderwerp gaat over vereisten voor FairPlay-licenties en laat zien hoe u kunt configureren een **FairPlay** met behulp van Media Sercies APIs van licentie. 

## <a name="requirements"></a>Vereisten

Het volgende is vereist als u Media Services voor het versleutelen van uw HLS-inhoud met **Apple FairPlay** en Media Services gebruiken om FairPlay-licenties te leveren:

* Aanmelden met [Program van Apple Development](https://developer.apple.com/).
* Apple vereist dat de eigenaar van de inhoud verkrijgen van de [implementatiepakket](https://developer.apple.com/contact/fps/). Status die u al sleutel Security Module (KSM) met Media Services hebt geïmplementeerd en dat u het laatste FPS pakket aanvraagt. Er zijn instructies in het definitieve pakket FPS certificering genereren en downloaden van de toepassing geheime sleutel (ASK). VRAAG kunt u FairPlay configureren.
* De volgende dingen moeten worden ingesteld op aan clientzijde voor levering van Media Services-sleutels/licenties:

    * **App-certificaat (AC)**: dit is een pfx-bestand dat de persoonlijke sleutel bevat. U kunt dit bestand maken en versleutelen met een wachtwoord. Het pfx-bestand shoul worden in Base 64-indeling.

        De volgende stappen wordt beschreven hoe u een PFX-certificaatbestand voor FairPlay genereren:

        1. Installeer de OpenSSL van https://slproweb.com/products/Win32OpenSSL.html.

            Ga naar de map waar het certificaat FairPlay en andere bestanden die worden geleverd door Apple zijn.
        2. Voer de volgende opdracht uit via de opdrachtregel. Hiermee zet u het cer-bestand naar een PEM-bestand.

            "C:\OpenSSL-Win32\bin\openssl.exe" x509-informeren der-in FairPlay.cer-out FairPlay-out.pem
        3. Voer de volgende opdracht uit via de opdrachtregel. Hiermee zet u het .pem-bestand naar een pfx-bestand met de persoonlijke sleutel. Het wachtwoord voor het pfx-bestand wordt vervolgens gevraagd door OpenSSL.

            "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-Exporteer - FairPlay-out.pfx-inkey privatekey.pem-in FairPlay-out.pem - passin file:privatekey-pem-pass.txt
            
    * **Certificaat voor App-wachtwoord**: het wachtwoord voor het maken van het pfx-bestand.
    * **VRAAG**: deze sleutel wordt weergegeven wanneer u het certificaat genereert met behulp van de Apple Developer-portal. Elke ontwikkelingsteam ontvangt een unieke vraag. Sla een kopie van de vraag, en sla deze op een veilige plaats. U moet configureren vragen als FairPlayAsk met Media Services.
    
* De volgende dingen moeten zijn ingesteld door de client FPS:

  * **App-certificaat (AC)**: dit is een.cer/.der-bestand met de openbare sleutel, die het besturingssysteem wordt gebruikt voor het versleutelen van sommige nettolading. Media Services moet weten over het, omdat deze is vereist voor de speler. De sleutelleveringsservice ontsleutelt deze met behulp van de bijbehorende persoonlijke sleutel.

* Als u wilt afspelen van een versleutelde FairPlay-stream een echte vraag eerste ophalen en genereer vervolgens een echt certificaat zijn. Dit proces wordt gemaakt van alle drie onderdelen:

  * der-bestand
  * PFX-bestand
  * wachtwoord voor het .pfx-bestand

## <a name="fairplay-and-player-apps"></a>FairPlay en de ingesloten speler apps

Als uw inhoud is versleuteld met **Apple FairPlay**, de afzonderlijke video en audio-voorbeelden zijn versleuteld met behulp van de **AES-128 CBC** modus. **FairPlay Streaming** (FPS) is geïntegreerd in de besturingssystemen van apparaten, met systeemeigen ondersteuning op iOS- en Apple tv-programma's. Safari op OS X kan FPS met behulp van de ondersteuning van de Encrypted Media Extensions (EME)-interface.

Azure Media Player biedt ook ondersteuning voor FairPlay afspelen. Zie voor meer informatie, [documentatie voor Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html).

U kunt uw eigen player-apps ontwikkelen met behulp van de iOS SDK. Als u FairPlay inhoud af te spelen, moet u de licentie voor exchange-protocol implementeren. Dit protocol is niet opgegeven door Apple. Het is aan elke app sleutellevering aanvragen verzenden. De Media Services FairPlay sleutelleveringsservice wordt verwacht dat de SPC te komen als een bericht van het gecodeerde bericht www-form-url, in de volgende notatie:

```
spc=<Base64 encoded SPC>
```

## <a name="fairplay-configuration-net-example"></a>FairPlay-configuratie .NET-voorbeeld

U kunt Media Services-API gebruiken om FairPlay-licenties te configureren. Wanneer de speler probeert uw FairPlay beveiligde inhoud af te spelen, wordt een aanvraag verzonden naar de service voor het leveren van licenties om de licentie te verkrijgen. Als de licentieservice de aanvraag goedkeurt, wordt de licentie serviceproblemen. Het wordt verzonden naar de client en wordt gebruikt om te ontsleutelen en de opgegeven inhoud af te spelen.

> [!NOTE]
> Normaal gesproken zou u wilt configureren beleidsopties voor FairPlay slechts één keer, omdat er slechts één set van een certificeringsinstantie en een vraag.

Het volgende voorbeeld wordt [Media Services .NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models?view=azure-dotnet) het configureren van de licentie.

```csharp
private static ContentKeyPolicyFairPlayConfiguration ConfigureFairPlayPolicyOptions()
{

    string askHex = "";
    string FairPlayPfxPassword = "";

    var appCert = new X509Certificate2("FairPlayPfxPath", FairPlayPfxPassword, X509KeyStorageFlags.Exportable);

    byte[] askBytes = Enumerable
        .Range(0, askHex.Length)
        .Where(x => x % 2 == 0)
        .Select(x => Convert.ToByte(askHex.Substring(x, 2), 16))
        .ToArray();

    ContentKeyPolicyFairPlayConfiguration fairPlayConfiguration =
    new ContentKeyPolicyFairPlayConfiguration
    {
        Ask = askBytes,
        FairPlayPfx =
                Convert.ToBase64String(appCert.Export(X509ContentType.Pfx, FairPlayPfxPassword)),
        FairPlayPfxPassword = FairPlayPfxPassword,
        RentalAndLeaseKeyType =
                ContentKeyPolicyFairPlayRentalAndLeaseKeyType
                .PersistentUnlimited,
        RentalDuration = 2249
    };

    return fairPlayConfiguration;
}
```

## <a name="next-steps"></a>Volgende stappen

Bekijk hoe u [beveiligen met DRM](protect-with-drm.md)