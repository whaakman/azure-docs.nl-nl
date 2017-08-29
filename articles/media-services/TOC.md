# [Overzicht](media-services-overview.md)
## [Scenario's en beschikbaarheid](scenarios-and-availability.md)
## [Concepten](media-services-concepts.md)

# Aan de slag
## [Account maken en beheren](media-services-portal-create-account.md)
## [Uw ontwikkelaarsomgeving instellen](media-services-set-up-computer.md)
### [.NET](media-services-dotnet-how-to-use.md)
### [REST](media-services-rest-how-to-use.md)  
## [AAD-verificatie gebruiken voor toegang tot API](media-services-use-aad-auth-to-access-ams-api.md)
### [Portal gebruiken voor het beheren van AAD-verificatie](media-services-portal-get-started-with-aad.md)
### [Toegang tot API met .NET](media-services-dotnet-get-started-with-aad.md)
### [Toegang tot API met REST](media-services-rest-connect-with-aad.md)
### [De CLI gebruiken om een AAD-app te maken en configureren](media-services-cli-create-and-configure-aad-app.md)
### [PowerShell gebruiken om een AAD-app te maken en configureren](media-services-powershell-create-and-configure-aad-app.md)

## Video op aanvraag leveren
### [Portal](media-services-portal-vod-get-started.md)
### [.NET-SDK](media-services-dotnet-get-started.md)
### [Java](media-services-java-how-to-use.md)
### [REST](media-services-rest-get-started.md)
## Live streamen
### [Portal](media-services-portal-live-passthrough-get-started.md)
### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)

# Procedures
## Beheren
### Entiteiten
#### [.NET](media-services-dotnet-manage-entities.md)
#### [REST](media-services-rest-manage-entities.md)
### [Streaming-eindpunten](media-services-streaming-endpoints-overview.md)
#### [Portal](media-services-portal-manage-streaming-endpoints.md)
#### [.NET](media-services-dotnet-manage-streaming-endpoints.md)
### Storage
#### [Media Services bijwerken na gebruik toegangssleutels voor opslag](media-services-roll-storage-access-keys.md)
#### [Assets beheren voor meerdere opslagaccounts](meda-services-managing-multiple-storage-accounts.md)
### [Quota en beperkingen](media-services-quotas-and-limitations.md)

## Inhoud uploaden
### Bestanden uploaden in een account
#### [Portal](media-services-portal-upload-files.md)
#### [.NET](media-services-dotnet-upload-files.md)
#### [REST](media-services-rest-upload-files.md)
### [Grote bestanden uploaden met Aspera](media-services-upload-files-with-aspera.md)
### [Bestanden uploaden met StorSimple](media-services-upload-files-from-storsimple.md)
### [Bestaande blobs kopiëren](media-services-copying-existing-blob.md)

## [Inhoud coderen](media-services-encode-asset.md)
### [Encoders vergelijken](media-services-compare-encoders.md)
### [De snelheid en gelijktijdigheid van uw codering beheren](media-services-manage-encoding-speed.md)
### Media Encoder Standard (MES)
#### [Indelingen en codecs voor Media Encoder Standard](media-services-media-encoder-standard-formats.md)
#### [MES gebruiken om automatisch een bitrate ladder te genereren](media-services-autogen-bitrate-ladder-with-mes.md)
#### Coderen met Media Encoder Standard
##### [Portal](media-services-portal-encode.md)
##### [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
##### [REST](media-services-rest-encode-asset.md)
#### [Geavanceerde codering met MES](media-services-advanced-encoding-with-mes.md)
##### [Voorinstellingen van Media Encoder Standard aanpassen](media-services-custom-mes-presets-with-dotnet.md)
##### [Miniatuurweergaven genereren met Media Encoder Standard met .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
##### [Video's bijsnijden met Media Encoder Standard](media-services-crop-video.md)
#### MES-schema's
##### [Media Encoder Standard-schema](media-services-mes-schema.md)
##### [Invoermetagegevens](media-services-input-metadata-schema.md)
##### [Uitvoermetagegevens](media-services-output-metadata-schema.md)
#### [MES-standaardinstellingen](media-services-mes-presets-overview.md) 
##### [H264 Multiple Bitrate 1080p Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-1080p-Audio-5.1.md)
##### [H264 Multiple Bitrate 1080p](media-services-mes-preset-H264-Multiple-Bitrate-1080p.md)
##### [H264 Multiple Bitrate 16x9 SD Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD-Audio-5.1.md)
##### [H264 Multiple Bitrate 16x9 SD](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD.md)
##### [H264 Multiple Bitrate 16x9 for iOS](media-services-mes-preset-H264-Multiple-Bitrate-16x9-for-iOS.md)
##### [H264 Multiple Bitrate 4K Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4K-Audio-5.1.md)
##### [H264 Multiple Bitrate 4K](media-services-mes-preset-H264-Multiple-Bitrate-4K.md)
##### [H264 Multiple Bitrate 4x3 SD Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD-Audio-5.1.md)
##### [H264 Multiple Bitrate 4x3 SD](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD.md)
##### [H264 Multiple Bitrate 4x3 for iOS](media-services-mes-preset-H264-Multiple-Bitrate-4x3-for-iOS.md)
##### [H264 Multiple Bitrate 720p Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-720p-Audio-5.1.md)
##### [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md)
##### [H264 Single Bitrate 1080p Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-1080p-Audio-5.1.md)
##### [H264 Single Bitrate 1080p](media-services-mes-preset-H264-Single-Bitrate-1080p.md)
##### [H264 Single Bitrate 16x9 SD Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-16x9-SD-Audio-5.1.md)
##### [H264 Single Bitrate 16x9 SD](media-services-mes-preset-H264-Single-Bitrate-16x9-SD.md)
##### [H264 Single Bitrate 4K Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-4K-Audio-5.1.md)
##### [H264 Single Bitrate 4K](media-services-mes-preset-H264-Single-Bitrate-4K.md)
##### [H264 Single Bitrate 4x3 SD Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-4x3-SD-Audio-5.1.md)
##### [H264 Single Bitrate 4x3 SD](media-services-mes-preset-H264-Single-Bitrate-4x3-SD.md)
##### [H264 Single Bitrate 720p Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-720p-Audio-5.1.md)
##### [H264 Single Bitrate 720p](media-services-mes-preset-H264-Single-Bitrate-720p.md)
##### [H264 Single Bitrate 720p for Android](media-services-mes-preset-H264-Single-Bitrate-720p-for-Android.md)
##### [H264 Single Bitrate High Quality SD for Android](media-services-mes-preset-H264-Single-Bitrate-High-Quality-SD-for-Android.md)
##### [H264 Single Bitrate Low Quality SD for Android](media-services-mes-preset-H264-Single-Bitrate-Low-Quality-SD-for-Android.md)
### Media Encoder Premium Workflow
#### [Indelingen en codecs voor Media Encoder Premium Workflow](media-services-premium-workflow-encoder-formats.md)
#### Coderen met Media Encoder Premium Workflow
##### [Media Encoder Premium Workflow](media-services-encode-with-premium-workflow.md)
##### [Zelfstudies voor Media Encoder Premium Workflow](media-services-media-encoder-premium-workflow-tutorials.md)
##### [Geavanceerde coderingswerkstromen maken met Workflow Designer](media-services-workflow-designer.md)
##### [Premium Workflow met verschillende invoer](media-services-media-encoder-premium-workflow-multiplefilesinput.md)
### [Een taak maken waarmee fMP4-chunks worden gegenereerd](media-services-generate-fmp4-chunks.md)
### Media-processors
#### [.NET](media-services-get-media-processor.md)
#### [REST](media-services-rest-get-media-processor.md)
### [Foutcodes](media-services-encoding-error-codes.md)
### Afgeschaft
#### [Statische pakketten en versleuteling](media-services-static-packaging.md)

## [Live streamen](media-services-manage-channels-overview.md)
### [on-premises coderingsprogramma's](media-services-live-streaming-with-onprem-encoders.md)
#### [Portal](media-services-portal-live-passthrough-get-started.md)
#### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)
### [Live streamen met cloudcoderingsprogramma's](media-services-manage-live-encoder-enabled-channels.md)
#### [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
#### [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
### [On-premises coderingsprogramma's configureren voor gebruik met cloudcoderingsprogramma's](media-services-live-encoders-overview.md)
#### [Elemental Live-coderingsprogramma](media-services-configure-elemental-live-encoder.md)
#### [FMLE-coderingsprogramma](media-services-configure-fmle-live-encoder.md)
#### [NewTek TriCaster-coderingsprogramma](media-services-configure-tricaster-live-encoder.md)
#### [Wirecast-coderingsprogramma](media-services-configure-wirecast-live-encoder.md)
### [Langdurige bewerkingen afhandelen](media-services-dotnet-long-operations.md)
### [Specificatie gefragmenteerde MP4-liveopname](media-services-fmp4-live-ingest-overview.md)

## [Beschermen](media-services-content-protection-overview.md)
### [Beveiliging van inhoud configureren in de portal](media-services-portal-protect-content.md)
### [AES-128 Clear Key voor uw stream configureren](media-services-protect-with-aes128.md)
### [REST gebruiken voor het versleutelen van uw inhoud met opslagversleuteling](media-services-rest-storage-encryption.md)
### [Overzicht van sjablonen voor Media Services PlayReady-licentie](media-services-playready-license-template-overview.md)
### [Overzicht van Widevine-licentiesjablonen](media-services-widevine-license-template-overview.md)
### [Levering van DRM-licentie](media-services-deliver-keys-and-licenses.md)
### [Partners gebruiken om Widevine-licenties te leveren aan Media Services](media-services-licenses-partner-integration.md)
#### [Axinom gebruiken om Widevine-licenties te leveren aan Media Services ](media-services-axinom-integration.md)
#### [castLabs gebruiken om Widevine-licenties te leveren aan Media Services ](media-services-castlabs-integration.md)
### [PlayReady en/of Widevine Dynamic Common Encryption gebruiken](media-services-protect-with-drm.md)
### [Uw met Apple FairPlay beveiligde HLS-inhoud streamen](media-services-protect-hls-with-fairplay.md)
### [Hybride ontwerp van DRM-subsysteem](hybrid-design-drm-sybsystem.md)
### [CENC met Multi-DRM en Toegangsbeheer](media-services-cenc-with-multidrm-access-control.md)
### Leveringsbeleid voor assets configureren
#### [.NET](media-services-dotnet-configure-asset-delivery-policy.md)
#### [REST](media-services-rest-configure-asset-delivery-policy.md)
### Inhoudssleutels maken
#### [.NET](media-services-dotnet-create-contentkey.md)
#### [REST](media-services-rest-create-contentkey.md)
### Autorisatiebeleid voor inhoudssleutels configureren
#### [Portal](media-services-portal-configure-content-key-auth-policy.md)
#### [.NET](media-services-dotnet-configure-content-key-auth-policy.md)
#### [REST](media-services-rest-configure-content-key-auth-policy.md)

## [Analyseren](media-services-analytics-overview.md)
### [Media analyseren met behulp van de portal](media-services-portal-analyze.md)
### [Verwerken met Indexer 2](media-services-process-content-with-indexer2.md)
### [Verwerken met Indexer](media-services-index-content.md)
#### [Taken vooraf instellen](indexer-task-preset.md)
### [Verwerken met Hyperlapse](media-services-hyperlapse-content.md)
### [Verwerken met Face Detector](media-services-face-and-emotion-detection.md)
### [Verwerken met Motion Detector](media-services-motion-detection.md)
### [Verwerken met Face Redactor](media-services-face-redaction.md)
#### [Walkthrough Face Redactor](media-services-redactor-walkthrough.md)
### [Verwerken met videominiaturen](media-services-video-summarization.md)
### [Verwerken met OCR](media-services-video-optical-character-recognition.md)

## [Telemetrie configureren](media-services-telemetry-overview.md)
###[.NET](media-services-dotnet-telemetry.md)
###[REST](media-services-rest-telemetry.md)

## Schalen
### [Mediaverwerking](media-services-scale-media-processing-overview.md)
#### [Portal](media-services-portal-scale-media-processing.md)
#### [.NET](media-services-dotnet-encoding-units.md)
### Streaming-eindpunten
#### [Portal](media-services-portal-scale-streaming-endpoints.md)

## [Inhoud leveren](media-services-deliver-content-overview.md)
### [Dynamische pakketten](media-services-dynamic-packaging-overview.md)
### [Overzicht filters en dynamische manifesten](media-services-dynamic-manifest-overview.md)
#### [Filters maken met .NET](media-services-dotnet-dynamic-manifest.md)
#### [Filters maken met REST](media-services-rest-dynamic-manifest.md)
### [CDN-beleid voor caching in Media Services-extensie](../cdn/cdn-caching-policy.md?toc=%2fazure%2fmedia-services%2ftoc.json)
### Inhoud publiceren
#### [Portal](media-services-portal-publish.md)
#### [.NET](media-services-deliver-streaming-content.md)
#### [REST](media-services-rest-deliver-streaming-content.md)
### [Leveren via download](media-services-deliver-asset-download.md)
### [Failoverscenario voor streamen](media-services-implement-failover.md)

## Gebruiken
### [Media afspelen met bestaande spelers](media-services-playback-content-with-existing-players.md)
### [Media afspelen met Media Player](media-services-develop-video-players.md)
### Overige afspeelopties
#### [Windows Store-toepassing met smooth streaming](media-services-build-smooth-streaming-apps.md)
#### [HTML5-toepassing met DASH.js](media-services-embed-mpeg-dash-in-html5.md)
#### [Adobe Open Source Media Framework-spelers](media-services-use-osmf-smooth-streaming-client-plugin.md)
### [Advertenties invoeren op de client](media-services-inserting-ads-on-client-side.md)
### [Licentieverlening Microsoft Smooth Streaming Client Porting Kit](media-services-sspk.md)
### [Met AES versleutelde HLS afspelen in Safari](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/)

## Integreren
### [Azure Functions gebruiken met Media Services](media-services-dotnet-how-to-use-azure-functions.md)
### [Voorbeelden van Azure Functions met Media Services](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)

## Bewaken
### Taakvoortgang controleren
#### [REST](media-services-rest-check-job-progress.md)
#### [Portal](media-services-portal-check-job-progress.md)
#### [.NET](media-services-check-job-progress.md)
### [Taakmeldingen bewaken met behulp van Queue Storage](media-services-dotnet-check-job-progress-with-queues.md)
### [Taakmeldingen bewaken met behulp van webhooks](media-services-dotnet-check-job-progress-with-webhooks.md)

## Problemen oplossen
### [Veelgestelde vragen](media-services-frequently-asked-questions.md)
### [Handleiding voor het oplossen van problemen met live streamen](media-services-troubleshooting-live-streaming.md)
### [Foutcodes](media-services-error-codes.md)
### [Logica voor opnieuw proberen](media-services-retry-logic-in-dotnet-sdk.md)

# Naslaginformatie
## [Codevoorbeelden](https://azure.microsoft.com/en-us/resources/samples/?service=media-services)
## [PowerShell (Resource Manager)](/powershell/module/azurerm.media)
## [PowerShell (Service Management)](/powershell/module/azure/?view=azuresmps-3.7.0)
## [.NET](/dotnet/api/microsoft.windowsazure.mediaservices.client)
## [REST](/rest/api/media/mediaservice)  

# Resources
## [Azure Media Services-community](media-services-community.md)
## [Azure-roadmap](https://azure.microsoft.com/roadmap/?category=web-mobile)
## [Prijzen](https://azure.microsoft.com/pricing/details/media-services/)
## [Prijscalculator](https://azure.microsoft.com/pricing/calculator/)
## [Releaseopmerkingen](media-services-release-notes.md)
## [Video's](https://azure.microsoft.com/resources/videos/index/?services=media-services)
