# [Overzicht](media-services-overview.md)
## [Concepten](media-services-concepts.md)


# Aan de slag
## [Account maken en beheren](media-services-portal-create-account.md)
## [Uw ontwikkelaarsomgeving instellen](media-services-set-up-computer.md)
## Video op aanvraag leveren
### [Portal](media-services-portal-vod-get-started.md)
### [.NET SDK](media-services-dotnet-get-started.md)
### [Java](media-services-java-how-to-use.md)
### [REST](media-services-rest-get-started.md)
## Live streamen
### [Portal](media-services-portal-live-passthrough-get-started.md)
### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)

# Procedures
## Beheren
### [Streaming-eindpunten in de portal beheren](media-services-portal-manage-streaming-endpoints.md)
### Entiteiten beheren
#### [.NET](media-services-dotnet-manage-entities.md)
#### [REST](media-services-rest-manage-entities.md)
### [Accounts beheren met PowerShell](media-services-manage-with-powershell.md)
### [Video’s bijsnijden met Media Encoder Standard](media-services-crop-video.md)
### [Procedure: Media Services bijwerken na gebruik toegangstoetsen voor opslag](media-services-roll-storage-access-keys.md)
### [Quota en beperkingen](media-services-quotas-and-limitations.md)
### Filters
#### [Filters maken met Azure Media Services .NET SDK](media-services-dotnet-dynamic-manifest.md)
#### [Een asset coderen met Media Encoder Standard](media-services-rest-encode-asset.md)
### Verbinding maken via een programma
#### [.NET](media-services-dotnet-connect-programmatically.md)
#### [REST](media-services-rest-connect-programmatically.md)

## Inhoud uploaden
### Bestanden uploaden in een account
#### [Portal ](media-services-portal-upload-files.md)
#### [.NET](media-services-dotnet-upload-files.md)
#### [REST](media-services-rest-upload-files.md)
### [Bestaande blobs kopiëren](media-services-copying-existing-blob.md)

## Coderen
### [Inhoud](media-services-encode-asset.md)
#### Een asset coderen met Media Encoder Standard
##### [Portal](media-services-portal-encode.md)
##### [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
#### [Miniatuurweergaven genereren met Media Encoder Standard met .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
#### [Geavanceerde codering](media-services-advanced-encoding-with-mes.md)
##### [Media Encoder Premium Workflow](media-services-encode-with-premium-workflow.md)
##### [Zelfstudies voor Media Encoder Premium Workflow](media-services-media-encoder-premium-workflow-tutorials.md)
##### [Geavanceerde coderingswerkstromen maken met Workflow Designer](media-services-workflow-designer.md)
##### [Premium Workflow met verschillende invoer](media-services-media-encoder-premium-workflow-multiplefilesinput.md)

#### Schema 's 
#####[Media Encoder Standard](media-services-mes-schema.md)
#####[Invoermetagegevens](media-services-input-metadata-schema.md)
#####[Uitvoermetagegevens](media-services-output-metadata-schema.md)

#### Verouderde coderingsprogramma’s
##### [Azure Media Packager gebruiken](media-services-static-packaging.md)

### [Live streams](media-services-manage-channels-overview.md)
#### [On-premises coderingsprogramma’s](media-services-live-streaming-with-onprem-encoders.md)
#### Zelfstudies voor on-premises coderingsprogramma’s
##### [Portal](media-services-portal-live-passthrough-get-started.md)
##### [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)
#### [Live streamen met cloudcoderingsprogramma’s](media-services-manage-live-encoder-enabled-channels.md)
#### Zelfstudies voor cloudcoderingsprogramma’s
##### [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
##### [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
#### [On-premises coderingsprogramma’s configureren voor gebruik met cloudcoderingsprogramma’s](media-services-live-encoders-overview.md)
#### [Langdurige bewerkingen afhandelen](media-services-dotnet-long-operations.md)
#### [Specificatie gefragmenteerde MP4-liveopname](media-services-fmp4-live-ingest-overview.md)
#### [Dynamische pakketten](media-services-dynamic-packaging-overview.md)

### Mediaverwerking
#### [.NET](media-services-get-media-processor.md)
#### [REST](media-services-rest-get-media-processor.md)

### Coderingsprogramma’s voor een single-bitrate livestream configureren
#### [Elemental Live-coderingsprogramma](media-services-configure-elemental-live-encoder.md)
#### [FMLE-coderingsprogramma](media-services-configure-fmle-live-encoder.md)
#### [NewTek TriCaster-coderingsprogramma](media-services-configure-tricaster-live-encoder.md)
#### [Wirecast-coderingsprogramma](media-services-configure-wirecast-live-encoder.md)

## [Beschermen](media-services-content-protection-overview.md)
### [Beveiliging van inhoud configureren in de portal](media-services-portal-protect-content.md)
### [AES-128 Clear Key voor uw stream configureren](media-services-protect-with-aes128.md)
### [Uw inhoud versleutelen met opslagversleuteling op basis van AMS REST API](media-services-rest-storage-encryption.md)
### [Overzicht van sjablonen voor Media Services PlayReady-licentie](media-services-playready-license-template-overview.md)
### [Levering van DRM-licentie](media-services-deliver-keys-and-licenses.md)
### [Partners gebruiken om Widevine-licenties te leveren aan Azure Media Services](media-services-licenses-partner-integration.md)
### [PlayReady en/of Widevine Dynamic Common Encryption gebruiken](media-services-protect-with-drm.md)
### [Azure Media Services gebruiken om uw met Apple FairPlay beveiligde HLS-inhoud te streamen](media-services-protect-hls-with-fairplay.md)
### [CENC met Multi-DRM en Access Control: een referentieontwerp en -implementatie in Azure en Azure Media Services](media-services-cenc-with-multidrm-access-control.md)

### Levering van assets
#### Leveringsbeleid voor assets configureren
##### [.NET](media-services-dotnet-configure-asset-delivery-policy.md)
##### [REST](media-services-rest-configure-asset-delivery-policy.md)
### Inhoudssleutels maken
#### [.NET](media-services-dotnet-create-contentkey.md)
#### [REST](media-services-rest-create-contentkey.md)
### Autorisatiebeleid voor inhoudssleutels configureren
#### [Portal](media-services-portal-configure-content-key-auth-policy.md)
#### [.NET](media-services-dotnet-configure-content-key-auth-policy.md)
#### [REST](media-services-rest-configure-content-key-auth-policy.md)

## [Analyseren](media-services-analytics-overview.md)
### [Verwerken met Indexer 2](media-services-process-content-with-indexer2.md)
### [Verwerken met Indexer](media-services-index-content.md)
### [Verwerken met Hyperlapse](media-services-hyperlapse-content.md)
### [Verwerken met Face Detector](media-services-face-and-emotion-detection.md)
### [Verwerken met Motion Detector](media-services-motion-detection.md)
### [Verwerken met Gezichten onherkenbaar maken](media-services-face-redaction.md)
### [Verwerken met videominiaturen](media-services-video-summarization.md)
### [Verwerken met OCR](media-services-video-optical-character-recognition.md)

## Schalen
### [Mediaverwerking](media-services-scale-media-processing-overview.md)
#### [Portal](media-services-portal-scale-media-processing.md)
#### [.NET](media-services-dotnet-encoding-units.md)
#### [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
### Streaming-eindpunten
#### [Portal](media-services-portal-scale-streaming-endpoints.md)

## [Inhoud leveren](media-services-deliver-content-overview.md)
### [Overzicht filters en dynamische manifesten](media-services-dynamic-manifest-overview.md)
### Filters maken
#### [.NET](media-services-dotnet-dynamic-manifest.md)
#### [REST](media-services-rest-dynamic-manifest.md)
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

## Integreren
### [CDN-beleid voor caching in Media Services-extensie](../cdn/cdn-caching-policy.md?toc=%2fazure%2fmedia-services%2ftoc.json)
### [Licentieverlening Microsoft†" Smooth Streaming Client Porting Kit](media-services-sspk.md)
### [Assets beheren voor meerdere opslagaccounts](meda-services-managing-multiple-storage-accounts.md)
### [Axinom gebruiken om Widevine-licenties te leveren aan Azure Media Services ](media-services-axinom-integration.md)
### [castLabs gebruiken om Widevine-licenties te leveren aan Azure Media Services](media-services-castlabs-integration.md)
### [Overzicht van Widevine-licentiesjablonen](media-services-widevine-license-template-overview.md)

## Controleren
### Taakvoortgang controleren
#### [REST](media-services-rest-check-job-progress.md)
#### [Portal](media-services-portal-check-job-progress.md)
#### [.NET](media-services-check-job-progress.md)
### [Queue Storage voor het bewaken van taakmeldingen](media-services-dotnet-check-job-progress-with-queues.md)

## Problemen oplossen
### [Veelgestelde vragen](media-services-frequently-asked-questions.md)
### [Handleiding voor het oplossen van problemen met live streamen](media-services-troubleshooting-live-streaming.md)
###[Foutcodes](media-services-error-codes.md)
###[Logica voor opnieuw proberen](media-services-retry-logic-in-dotnet-sdk.md)

# Naslaginformatie
## [Releaseopmerkingen](media-services-release-notes.md)
## [.NET](media-services-dotnet-how-to-use.md)
## [REST](media-services-rest-how-to-use.md)
## [Indelingen en codecs voor Media Encoder Premium Workflow](media-services-premium-workflow-encoder-formats.md)
## [Indelingen en codecs voor Media Encoder Standard](media-services-media-encoder-standard-formats.md)

# Resources
## [Prijzen](https://azure.microsoft.com/pricing/details/media-services/)
## [Azure Media Services-community](media-services-community.md)











<!--HONumber=Nov16_HO2-->


