---
title: Beveiligen van HLS-inhoud met Microsoft PlayReady- of Apple FairPlay - Azure | Microsoft Docs
description: In dit onderwerp biedt een overzicht en laat zien hoe Azure Media Services gebruiken op uw HTTP Live Streaming (HLS) inhoud met Apple FairPlay dynamisch wordt versleuteld. U ziet ook het gebruik van de Media Services-service voor het leveren van licenties FairPlay-licenties te leveren aan clients.
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
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 8d5683cb060b63aebad7c68672c78f5b350a25d3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67073586"
---
# <a name="protect-your-hls-content-with-apple-fairplay-or-microsoft-playready"></a>Beveiligen van uw inhoud met Apple FairPlay of Microsoft PlayReady HLS

> [!NOTE]
> U hebt een Azure-account nodig om deze zelfstudie te voltooien. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.   > Er zijn geen nieuwe functies of functionaliteit worden toegevoegd aan Media Services v2. <br/>Maak kennis met de nieuwste versie, [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/). Zie ook [hulp bij de migratie van v2 naar v3](../latest/migrate-from-v2-to-v3.md)
>

Azure Media Services kunt u de inhoud van uw HTTP Live Streaming (HLS) dynamisch wordt versleuteld met behulp van de volgende indelingen:  

* **De lege sleutel AES-128-envelop**

    Het hele segment is versleuteld met behulp van de **AES-128 CBC** modus. De ontsleuteling van de stroom wordt ondersteund door iOS en OS X de ingesloten speler van de systeemeigen. Zie voor meer informatie, [dynamische versleuteling met behulp van AES-128 en sleutelleveringsservice](media-services-protect-with-aes128.md).
* **Apple FairPlay**

    De afzonderlijke video en audio-voorbeelden zijn versleuteld met behulp van de **AES-128 CBC** modus. **FairPlay Streaming** (FPS) is geïntegreerd in de besturingssystemen van apparaten, met systeemeigen ondersteuning op iOS- en Apple tv-programma's. Safari op OS X kan FPS met behulp van de ondersteuning van de Encrypted Media Extensions (EME)-interface.
* **Microsoft PlayReady**

De volgende afbeelding toont de **HLS + FairPlay of PlayReady dynamische versleuteling** werkstroom.

![Diagram van de werkstroom voor dynamische versleuteling](./media/media-services-content-protection-overview/media-services-content-protection-with-FairPlay.png)

In dit artikel laat zien hoe u uw HLS-inhoud met Apple FairPlay dynamisch wordt versleuteld met Media Services. U ziet ook het gebruik van de Media Services-service voor het leveren van licenties FairPlay-licenties te leveren aan clients.

> [!NOTE]
> Als u ook wilt voor het versleutelen van uw HLS-inhoud met PlayReady, moet u een algemene inhoudssleutel maken en deze koppelen aan uw asset. U moet ook configureren voor de inhoudssleutel autorisatie, zoals beschreven in [dynamische algemene versleuteling met PlayReady met behulp van](media-services-protect-with-playready-widevine.md).
>
>

## <a name="requirements-and-considerations"></a>Vereisten en overwegingen

Het volgende is vereist als u Media Services leveren HLS versleuteld met FairPlay en om FairPlay-licenties te leveren:

  * Een Azure-account. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) voor meer informatie.
  * Een Media Services-account. Zie voor het maken van een [een Azure Media Services-account maken met de Azure-portal](media-services-portal-create-account.md).
  * Aanmelden met [Program van Apple Development](https://developer.apple.com/).
  * Apple vereist dat de eigenaar van de inhoud verkrijgen van de [implementatiepakket](https://developer.apple.com/contact/fps/). Status die u al sleutel Security Module (KSM) met Media Services hebt geïmplementeerd en dat u het laatste FPS pakket aanvraagt. Er zijn instructies in het definitieve pakket FPS certificering genereren en downloaden van de toepassing geheime sleutel (ASK). VRAAG kunt u FairPlay configureren.
  * Azure Media Services .NET SDK-versie **3.6.0** of hoger.

De volgende dingen moeten worden ingesteld op Media Services sleutellevering aan clientzijde:

  * **App-certificaat (AC)** : Dit is een pfx-bestand dat de persoonlijke sleutel bevat. U kunt dit bestand maken en versleutelen met een wachtwoord.

       Wanneer u een sleutel leveringsbeleid configureert, moet u opgeven dat wachtwoord en het pfx-bestand in Base 64-indeling.

      De volgende stappen wordt beschreven hoe u een PFX-certificaatbestand voor FairPlay genereren:

    1. Installeer de OpenSSL van https://slproweb.com/products/Win32OpenSSL.html.

        Ga naar de map waar het certificaat FairPlay en andere bestanden die worden geleverd door Apple zijn.
    2. Voer de volgende opdracht uit via de opdrachtregel. Hiermee zet u het cer-bestand naar een PEM-bestand.

        "C:\OpenSSL-Win32\bin\openssl.exe" x509-informeren der-in FairPlay.cer-out FairPlay-out.pem
    3. Voer de volgende opdracht uit via de opdrachtregel. Hiermee zet u het .pem-bestand naar een pfx-bestand met de persoonlijke sleutel. Het wachtwoord voor het pfx-bestand wordt vervolgens gevraagd door OpenSSL.

        "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-Exporteer - FairPlay-out.pfx-inkey privatekey.pem-in FairPlay-out.pem - passin file:privatekey-pem-pass.txt
  * **Certificaat voor App-wachtwoord**: Het wachtwoord voor het maken van het pfx-bestand.
  * **App-certificaat wachtwoord ID**: U moet het wachtwoord, vergelijkbaar met hoe ze andere sleutels Media Services uploaden uploaden. Gebruik de **ContentKeyType.FairPlayPfxPassword** enum-waarde om de Media Services-ID. Dit is wat ze nodig hebben om te gebruiken binnen de beleidsoptie sleutellevering.
  * **iv**: Dit is een willekeurige waarde van 16 bytes. Dit moet overeenkomen met de iv in het leveringsbeleid voor Assets. U genereert de iv en plaatsen op beide plaatsen: het leveringsbeleid voor Assets en de optie voor sleutellevering.
  * **VRAAG**: Deze sleutel wordt ontvangen wanneer u het certificaat genereert met behulp van de Apple Developer-portal. Elke ontwikkelingsteam ontvangt een unieke vraag. Sla een kopie van de vraag, en sla deze op een veilige plaats. U moet vragen als FairPlayAsk met Media Services later configureren.
  * **VRAAG ID**: Deze ID wordt verkregen tijdens het uploaden van vragen in Media Services. U moet bevestigen uploaden met behulp van de **ContentKeyType.FairPlayAsk** enum-waarde. Als het resultaat wordt geretourneerd met de ID van de Media Services en dit is wat moet worden gebruikt bij het instellen van de belangrijkste bezorgingsoptie beleid.

De volgende dingen moeten zijn ingesteld door de client FPS:

  * **App-certificaat (AC)** : Dit is een.cer/.der-bestand met de openbare sleutel, die het besturingssysteem wordt gebruikt voor het versleutelen van sommige nettolading. Media Services moet weten over het, omdat deze is vereist voor de speler. De sleutelleveringsservice ontsleutelt deze met behulp van de bijbehorende persoonlijke sleutel.

Als u wilt afspelen van een versleutelde FairPlay-stream een echte vraag eerste ophalen en genereer vervolgens een echt certificaat zijn. Dit proces wordt gemaakt van alle drie onderdelen:

  * der-bestand
  * PFX-bestand
  * wachtwoord voor het .pfx-bestand

De volgende clients ondersteunen HLS met **AES-128 CBC** versleuteling: Safari op OS X-, Apple TV, iOS.

## <a name="configure-fairplay-dynamic-encryption-and-license-delivery-services"></a>FairPlay dynamische versleuteling en licentie levering van services configureren
Hier volgen de algemene stappen voor het beveiligen van uw activa met FairPlay met behulp van de Media Services-service voor het leveren van licenties en dynamische versleuteling.

1. Maak een asset en upload bestanden in de asset.
2. Codeer de asset die het bestand bevat naar de Adaptive Bitrate MP4-set.
3. Maak een inhoudssleutel en koppel deze aan de gecodeerde asset.  
4. Configureer het autorisatiebeleid voor de inhoudssleutel. Het volgende opgeven:

   * De leveringsmethode (in dit geval FairPlay).
   * Configuratie van opties voor FairPlay. Zie voor meer informatie over het configureren van FairPlay de **ConfigureFairPlayPolicyOptions()** methode in het onderstaande voorbeeld.

     > [!NOTE]
     > Normaal gesproken zou u wilt configureren beleidsopties voor FairPlay slechts één keer, omdat er slechts één set van een certificeringsinstantie en een vraag.
     >
     >
   * Beperkingen (openen of token).
   * Informatie die specifiek zijn voor het sleutelleveringstype waarmee wordt gedefinieerd hoe de sleutel aan de client wordt geleverd.
5. Configureer het leveringsbeleid voor Assets. De configuratie van een leveringsbeleid omvat:

   * Het leveringsprotocol (HLS).
   * Het type dynamische versleuteling (algemene CBC versleuteling).
   * De URL voor het verkrijgen licentie.

     > [!NOTE]
     > Als u een stroom die is versleuteld met een andere Digital Rights Management (DRM)-systeem en FairPlay leveren wilt, hebt u beleid voor de afzonderlijke levering configureren:
     >
     > * Een IAssetDeliveryPolicy Dynamic Adaptive Streaming via HTTP (DASH) met algemene versleuteling (CENC) (PlayReady en Widevine) en Smooth met PlayReady configureren
     > * Een andere IAssetDeliveryPolicy FairPlay voor HLS configureren
     >
     >
6. Maak een OnDemand-locator om een streaming-URL te verkrijgen.

## <a name="use-fairplay-key-delivery-by-player-apps"></a>FairPlay sleutellevering met player-apps gebruiken
U kunt player-apps ontwikkelen met behulp van de iOS SDK. Als u FairPlay inhoud af te spelen, moet u de licentie voor exchange-protocol implementeren. Dit protocol is niet opgegeven door Apple. Het is aan elke app sleutellevering aanvragen verzenden. De Media Services FairPlay sleutelleveringsservice wordt verwacht dat de SPC te komen als een bericht van het gecodeerde bericht www-form-url, in de volgende notatie:

    spc=<Base64 encoded SPC>

> [!NOTE]
> Azure Media Player ondersteunt FairPlay afspelen. Zie [documentatie voor Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) voor meer informatie.
>
>

## <a name="streaming-urls"></a>Streaming-URL 's
Als uw asset is versleuteld met meer dan één DRM, moet u een tag versleuteling in de streaming-URL: (format = 'm3u8-aapl' codering = 'xxx').

De volgende overwegingen zijn van toepassing:

* Alleen nul of één versleutelingstype kan worden opgegeven.
* Het versleutelingstype hoeft te worden opgegeven in de URL als er slechts één versleuteling is toegepast op de asset.
* Het versleutelingstype is niet hoofdlettergevoelig.
* De volgende versleutelingstypen kunnen worden opgegeven:  
  * **cenc**:  Algemene versleuteling (PlayReady of Widevine)
  * **cbcs-aapl**: FairPlay
  * **cbc**: AES-codering van envelop

## <a name="create-and-configure-a-visual-studio-project"></a>Maak en configureer een Visual Studio-project.

1. Stel uw ontwikkelomgeving in en vul in het bestand app.config de verbindingsinformatie in, zoals beschreven in [Media Services ontwikkelen met .NET](media-services-dotnet-how-to-use.md). 
2. Voeg de volgende elementen toe aan **appSettings** dat in het bestand app.config is gedefinieerd:

    ```xml
    <add key="Issuer" value="http://testissuer.com"/>
    <add key="Audience" value="urn:test"/>
    ```

## <a name="example"></a>Voorbeeld

Het volgende voorbeeld ziet u de mogelijkheid om met Media Services om uw inhoud die is versleuteld met FairPlay te leveren. Deze functionaliteit is ingevoerd in de Azure Media Services SDK voor .NET versie 3.6.0. 

Overschrijf de code in uw Program.cs-bestand met de code die wordt weergegeven in deze sectie.

>[!NOTE]
>Er geldt een limiet van 1.000.000 beleidsregels voor verschillende AMS-beleidsitems (bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy). U moet dezelfde beleids-id gebruiken als u altijd dezelfde dagen/toegangsmachtigingen gebruikt, bijvoorbeeld beleidsregels voor locators die zijn bedoeld om gedurende een lange periode gehandhaafd te blijven (niet-upload-beleidsregels). Raadpleeg [dit artikel](media-services-dotnet-manage-entities.md#limit-access-policies) voor meer informatie.

Zorg ervoor dat variabelen zo worden bijgewerkt dat ze verwijzen naar de mappen waar uw invoerbestanden zich bevinden.

```csharp
using System;
using System.Collections.Generic;
using System.Configuration;
using System.IO;
using System.Linq;
using System.Threading;
using Microsoft.WindowsAzure.MediaServices.Client;
using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
using Newtonsoft.Json;
using System.Security.Cryptography.X509Certificates;

namespace DynamicEncryptionWithFairPlay
{
    class Program
    {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AMSAADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
        private static readonly string _AMSClientId =
            ConfigurationManager.AppSettings["AMSClientId"];
        private static readonly string _AMSClientSecret =
            ConfigurationManager.AppSettings["AMSClientSecret"];

        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        private static readonly Uri _sampleAudience =
            new Uri(ConfigurationManager.AppSettings["Audience"]);

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static readonly string _mediaFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            AzureAdTokenCredentials tokenCredentials =
                new AzureAdTokenCredentials(_AADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            bool tokenRestriction = false;
            string tokenTemplateString = null;

            IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
            Console.WriteLine("Uploaded asset: {0}", asset.Id);

            IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
            Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);

            IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
            Console.WriteLine();

            if (tokenRestriction)
                tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
            else
                AddOpenAuthorizationPolicy(key);

            Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
            Console.WriteLine();

            CreateAssetDeliveryPolicy(encodedAsset, key);
            Console.WriteLine("Created asset delivery policy. \n");
            Console.WriteLine();

            if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
            {
                // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
                // back into a TokenRestrictionTemplate class instance.
                TokenRestrictionTemplate tokenTemplate =
                    TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

                // Generate a test token based on the data in the given TokenRestrictionTemplate.
                // Note, you need to pass the key id Guid because we specified
                // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
                string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                            DateTime.UtcNow.AddDays(365));
                Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                Console.WriteLine();
            }

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);

            Console.ReadLine();
        }

        static public IAsset UploadFileAndCreateAsset(string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
        {
            var encodingPreset = "Adaptive Streaming";

            IJob job = _context.Jobs.Create(String.Format("Encoding {0}", inputAsset.Name));

            var mediaProcessors =
            _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();

            var latestMediaProcessor =
            mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();

            ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
            encodeTask.InputAssets.Add(inputAsset);
            encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }

        static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
        {
            // Create HLS SAMPLE AES encryption content key
            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryptionCbcs);

            // Associate the key with the asset.
            asset.ContentKeys.Add(key);

            return key;
        }


        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
        {
            // Create ContentKeyAuthorizationPolicy with Open restrictions
            // and create authorization policy          

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                    new ContentKeyAuthorizationPolicyRestriction
                    {
                        Name = "Open",
                        KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                        Requirements = null
                    }
                    };


            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();

            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
            ContentKeyDeliveryType.FairPlay,
            restrictions,
            FairPlayConfiguration);


            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

            // Associate the content key authorization policy with the content key.
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;
        }

        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
        {
            string tokenTemplateString = GenerateTokenRequirements();

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                    new ContentKeyAuthorizationPolicyRestriction
                    {
                        Name = "Token Authorization Policy",
                        KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                        Requirements = tokenTemplateString,
                    }
                    };

            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();


            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                   ContentKeyDeliveryType.FairPlay,
                   restrictions,
                   FairPlayConfiguration);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

            // Associate the content key authorization policy with the content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        private static string ConfigureFairPlayPolicyOptions()
        {
            // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK.
            // However, for production you must use a real ASK from Apple bound to a real prod certificate.
            byte[] askBytes = Guid.NewGuid().ToByteArray();
            var askId = Guid.NewGuid();
            // Key delivery retrieves askKey by askId and uses this key to generate the response.
            IContentKey askKey = _context.ContentKeys.Create(
                        askId,
                        askBytes,
                        "askKey",
                        ContentKeyType.FairPlayASk);

            //Customer password for creating the .pfx file.
            string pfxPassword = "<customer password for creating the .pfx file>";
            // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
            var pfxPasswordId = Guid.NewGuid();
            byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
            IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                        pfxPasswordId,
                        pfxPasswordBytes,
                        "pfxPasswordKey",
                        ContentKeyType.FairPlayPfxPassword);

            // iv - 16 bytes random value, must match the iv in the asset delivery policy.
            byte[] iv = Guid.NewGuid().ToByteArray();

            //Specify the .pfx file created by the customer.
            var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);

            string FairPlayConfiguration =
            Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                appCert,
                pfxPassword,
                pfxPasswordId,
                askId,
                iv);

            return FairPlayConfiguration;
        }

        static private string GenerateTokenRequirements()
        {
            TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

            template.PrimaryVerificationKey = new SymmetricVerificationKey();
            template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
            template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

            return TokenRestrictionTemplateSerializer.Serialize(template);
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();

            var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);

            FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);

            // Get the FairPlay license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);

            // The reason the below code replaces "https://" with "skd://" is because
            // in the IOS player sample code which you obtained in Apple developer account,
            // the player only recognizes a Key URL that starts with skd://.
            // However, if you are using a customized player,
            // you can choose whatever protocol you want.
            // For example, "https".

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                    {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
            };

            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
            assetDeliveryPolicyConfiguration);

            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        }


        /// <summary>
        /// Gets the streaming origin locator.
        /// </summary>
        /// <param name="assets"></param>
        /// <returns></returns>
        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference to the streaming manifest file from the  
            // collection of files in the asset.

            var assetFile = asset.AssetFiles.LoList().Where(f => f.Name.ToLower().
                         EndsWith(".ism")).
                         FirstOrDefault();

            // Create a 30-day readonly access policy.
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

            // Create a locator to the streaming content on an origin.
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

            // Create a URL to the manifest file.
            return originLocator.Path + assetFile.Name;
        }

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
        }

        static private byte[] GetRandomBuffer(int length)
        {
            var returnValue = new byte[length];

            using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
            {
                rng.GetBytes(returnValue);
            }

            return returnValue;
        }
    }
}
```

## <a name="next-steps-media-services-learning-paths"></a>Volgende stappen: Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
