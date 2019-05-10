---
title: Azure Media Services-pakketten en de oorsprong fouten | Microsoft Docs
description: Dit onderwerp beschrijft de fouten die u van de service Azure Media Services Streaming-eindpunt (waarop ontvangen kan).
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2019
ms.author: juliako
ms.openlocfilehash: ebcda6026f79bc88df91471d8be88316ba57bfc6
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65411373"
---
# <a name="streaming-endpoint-origin-errors"></a>Streaming-eindpunt (oorsprong)-fouten 

In dit onderwerp beschrijft de fouten die u van de Azure Media Services ontvangt mogelijk [Streaming-eindpunt service](streaming-endpoint-concept.md).

## <a name="400-bad-request"></a>400-Ongeldige aanvraag

De aanvraag bevat ongeldige gegevens en is geweigerd met deze foutcodes en een van de volgende oorzaken hebben:

|Foutcode|Hexadecimale waarde |Foutbeschrijving|
|---|---|---|
|MPE_BAD_URL_SYNTAX |0x80890201|Een URL-fout voor syntaxis of de indeling. Voorbeelden zijn aanvragen voor een ongeldig type of een ongeldige fragment een ongeldige bijhouden. |
|MPE_ENC_ENCRYPTION_NOT_SPECIFIED_IN_URL |0x8088024C|De aanvraag heeft geen versleuteling-code in de URL. CMAF-aanvragen vereisen een tag versleuteling in de URL. Andere protocollen die zijn geconfigureerd met meer dan één versleutelingstype wordt ook de tag versleuteling voor ondubbelzinnigheid vereist. |
|MPE_STORAGE_BAD_URL_SYNTAX |0x808900E9|De aanvraag voor opslag om te voldoen aan de aanvraag is mislukt met een ongeldige aanvraag-fout. |

## <a name="403-forbidden"></a>403-verboden

De aanvraag is niet toegestaan vanwege een van de volgende redenen:

|Foutcode|Hexadecimale waarde |Foutbeschrijving|
|---|---|---|
|MPE_STORAGE_AUTHENTICATION_FAILED |0x808900EA|De aanvraag voor opslag om te voldoen aan de aanvraag is mislukt met een verificatiefout opgetreden. Dit kan gebeuren als de storage-sleutels zijn gedraaid en de service kan niet synchroniseren van de storage-sleutels is. <br/><br/>Neem contact op met Azure ondersteuning door te gaan naar [Help en ondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) in Azure portal.|
|MPE_STORAGE_INSUFFICIENT_ACCOUNT_PERMISSIONS |0x808900EB |Opslag-bewerkingsfout toegang is mislukt vanwege onvoldoende machtigingen. |
|MPE_STORAGE_ACCOUNT_IS_DISABLED |0x808900EC |De aanvraag naar de opslag om te voldoen aan de aanvraag is mislukt omdat het opslagaccount Is uitgeschakeld. |
|MPE_STORAGE_AUTHENTICATION_FAILURE |0x808900F3 |Opslag-bewerkingsfout toegang is mislukt vanwege een algemene fouten. |
|MPE_OUTPUT_FORMAT_BLOCKED |0x80890207 |Indeling van de uitvoer is geblokkeerd vanwege de configuratie in de StreamingPolicy. |
|MPE_ENC_ENCRYPTION_REQUIRED |0x8088021E |Versleuteling is vereist voor de inhoud, beleid voor de levering is vereist voor de indeling van de uitvoer. |
|MPE_ENC_ENCRYPTION_NOT_SET_IN_DELIVERY_POLICY |0x8088024D |Versleuteling is niet ingesteld in de instellingen voor delivery. |

## <a name="404-not-found"></a>404 Niet gevonden

De bewerking wordt geprobeerd om te reageren op een resource die niet meer bestaat. Bijvoorbeeld: de resource mogelijk al verwijderd.

|Foutcode|Hexadecimale waarde |Foutbeschrijving|
|---|---|---|
|MPE_EGRESS_TRACK_NOT_FOUND |0x80890209 |De aangevraagde track is niet gevonden. |
|MPE_RESOURCE_NOT_FOUND |0x808901F9 |De aangevraagde resource is niet gevonden. |
|MPE_UNAUTHORIZED |0x80890244 |De toegang is niet gemachtigd. |
|MPE_EGRESS_TIMESTAMP_NOT_FOUND |0x8089020A |De aangevraagde tijdstempel is niet gevonden. |
|MPE_EGRESS_FILTER_NOT_FOUND |0x8089020C |De aangevraagde dynamisch-manifestfilter is niet gevonden. |
|MPE_FRAGMENT_BY_INDEX_NOT_FOUND |0x80890252 |De aangevraagde fragment index valt buiten het geldige bereik. |
|MPE_LIVE_MEDIA_ENTRIES_NOT_FOUND |0x80890254 |Live-media-items om op te halen moov buffer kunnen niet worden gevonden. |
|MPE_FRAGMENT_TIMESTAMP_NOT_FOUND |0x80890255 |Kan het fragment vinden op de aangevraagde tijd voor een bepaalde bijhouden.<br/><br/>Kan zijn dat het fragment niet in de opslag. Probeer een andere laag van de presentatie die mogelijk een fragment. |
|MPE_MANIFEST_MEDIA_ENTRY_NOT_FOUND |0x80890256 |Kan de media-vermelding vinden voor de aangevraagde bitrate in het manifest. <br/><br/>Kan zijn dat de speler wordt gevraagd om een video bijhouden van een bepaalde bitrate is niet in het manifest opgenomen.|
|MPE_METADATA_NOT_FOUND |0x80890257 |Kan bepaalde metagegevens vinden in het manifest of kan niet worden gevonden rebase uit de opslag. |
|MPE_STORAGE_RESOURCE_NOT_FOUND |0x808900ED |Fout in de opslag-bewerking, resource is niet gevonden. |

## <a name="409-conflict"></a>409 conflict

De ID die is opgegeven voor een bron op een `PUT` of `POST` bewerking heeft een bestaande resource genomen. Gebruik een andere ID voor de resource om dit probleem te verhelpen.

|Foutcode|Hexadecimale waarde |Foutbeschrijving|
|---|---|---|
|MPE_STORAGE_CONFLICT  |0x808900EE  |Fout in de opslag-bewerking, foutbericht.  |

## <a name="410"></a>410

|Foutcode|Hexadecimale waarde |Foutbeschrijving|
|---|---|---|
|MPE_FILTER_FORCE_END_LEFT_EDGE_CROSSED_DVR_WINDOW|0x80890263|Voor live streamen, wanneer het filter dat is is ingesteld op true, het begin of einde timestamp forceEndTimestamp buiten het huidige DVR-venster.|

## <a name="412-precondition-failure"></a>412-voorwaarde-fout

De bewerking opgegeven een eTag die verschilt van de versie die beschikbaar zijn op de server, dat wil zeggen, een optimistische gelijktijdigheid-fout. De aanvraag opnieuw proberen na de meest recente versie van de resource lezen en bijwerken van de eTag voor de aanvraag.

|Foutcode|Hexadecimale waarde |Foutbeschrijving|
|---|---|---|
|MPE_FRAGMENT_NOT_READY |0x80890200 |Het aangevraagde fragment is niet gereed.|
|MPE_STORAGE_PRECONDITION_FAILED| 0x808900EF|Fout in de opslag-bewerking, een voorwaarde-fout.|

## <a name="415-unsupported-media-type"></a>415 mediatype niet-ondersteund

De indeling van de nettolading die is verzonden door de client heeft een niet-ondersteunde indeling.

|Foutcode|Hexadecimale waarde |Foutbeschrijving|
|---|---|---|
|MPE_ENC_ALREADY_ENCRYPTED| 0x8088021F| Versleuteling moet niet van toepassing op al versleutelde inhoud.|
|MPE_ENC_INVALID_INPUT_ENCRYPTION_FORMAT|0x8088021D |De versleuteling is ongeldig voor de invoerindeling.|
|MPE_INVALID_ASSET_DELIVERY_POLICY_TYPE|0x8088021C| Bezorgingstype beleid is ongeldig.|
|MPE_ENC_MULTIPLE_SAME_DELIVERY_TYPE|0x8088024E |De oorspronkelijke instellingen kunnen worden gedeeld door meerdere uitvoerindeling op te geven.|
|MPE_FORMAT_NOT_SUPPORTED|0x80890205|De media-indeling of het type wordt niet ondersteund. Media Services biedt bijvoorbeeld geen ondersteuning voor kwaliteit niveau aantal dat is meer dan 64. In de videotag FLV-, biedt Media Services geen ondersteuning voor een video kader met meerdere SP's en meerdere PPS.|
|MPE_INPUT_FORMAT_NOT_SUPPORTED|0x80890218| De invoerindeling van asset aangevraagd wordt niet ondersteund. Media Services ondersteunt (live), Smooth MP4 (VoD) en progressief downloaden indelingen.|
|MPE_OUTPUT_FORMAT_NOT_SUPPORTED|0x8089020D|De indeling van de uitvoer aangevraagd wordt niet ondersteund. Media Services ondersteunt Smooth, DASH (CSF, CMAF), HLS (v3, v4, CMAF), en progressief downloaden indelingen.|
|MPE_ENCRYPTION_NOT_SUPPORTED|0x80890208|Niet-ondersteunde versleutelingstype aangetroffen.|
|MPE_MEDIA_TYPE_NOT_SUPPORTED|0x8089020E|Het gevraagde mediatype wordt niet ondersteund door de uitvoerindeling. De ondersteunde typen zijn video, audio of "SUBT" subtitel.|
|MPE_MEDIA_ENCODING_NOT_SUPPORTED|0x8089020F|De bronmedia asset is gecodeerd met een media-indeling die is niet compatibel met de indeling van de uitvoer.|
|MPE_VIDEO_ENCODING_NOT_SUPPORTED|0x80890210|De bron-asset is gecodeerd met een video-indeling die is niet compatibel met de indeling van de uitvoer. H.264, AVC, H.265 (HEVC, hev1 of hvc1) worden ondersteund.|
|MPE_AUDIO_ENCODING_NOT_SUPPORTED|0x80890211|De bron-asset is gecodeerd met een audio-indeling die is niet compatibel met de indeling van de uitvoer. Ondersteunde audio-indelingen zijn AAC, E-AC3 (DD +), Dolby DTS.|
|MPE_SOURCE_PROTECTION_CONVERSION_NOT_SUPPORTED|0x80890212|De bron beveiligde asset kan niet worden geconverteerd naar de indeling van de uitvoer.|
|MPE_OUTPUT_PROTECTION_FORMAT_NOT_SUPPORTED|0x80890213|De indeling van de beveiliging wordt niet ondersteund door de uitvoerindeling.|
|MPE_INPUT_PROTECTION_FORMAT_NOT_SUPPORTED|0x80890219|De indeling van de beveiliging wordt niet ondersteund door de invoer-indeling.|
|MPE_INVALID_VIDEO_NAL_UNIT|0x80890231|Ongeldige video NAL-eenheid, bijvoorbeeld alleen de eerste NAL in het voorbeeld is een AUD.|
|MPE_INVALID_NALU_SIZE|0x80890260|Ongeldige grootte van NAL.|
|MPE_INVALID_NALU_LENGTH_FIELD|0x80890261|Ongeldige waarde NAL-eenheid voor lengte.|
|MPE_FILTER_INVALID|0x80890236|Ongeldige dynamisch-manifestfilters.|
|MPE_FILTER_VERSION_INVALID|0x80890237|Ongeldige of niet-ondersteund filter versies.|
|MPE_FILTER_TYPE_INVALID|0x80890238|Ongeldig filtertype.|
|MPE_FILTER_RANGE_ATTRIBUTE_INVALID|0x80890239|Ongeldig bereik is opgegeven door het filter.|
|MPE_FILTER_TRACK_ATTRIBUTE_INVALID|0x8089023A|Ongeldige bijhouden kenmerk is opgegeven door het filter.|
|MPE_FILTER_PRESENTATION_WINDOW_INVALID|0x8089023B|Ongeldige toewijzingssjabloon venster lengte is opgegeven door het filter.|
|MPE_FILTER_LIVE_BACKOFF_INVALID|0x8089023C|Ongeldig live uitstel is opgegeven door het filter.|
|MPE_FILTER_MULTIPLE_SAME_TYPE_FILTERS|0x8089023D|Slechts één absTimeInHNS-element wordt in de filters voor verouderde ondersteund.|
|MPE_FILTER_REMOVED_ALL_STREAMS|0x8089023E|Er is helemaal geen stromen meer nadat de filters zijn toegepast.|
|MPE_FILTER_LIVE_BACKOFF_OVER_DVRWINDOW|0x8089023F|De live back-off valt buiten het DVR-venster.|
|MPE_FILTER_LIVE_BACKOFF_OVER_PRESENTATION_WINDOW|0x80890262|De live back-off is groter dan de presentatievenster.|
|MPE_FILTER_COMPOSITION_FILTER_COUNT_OVER_LIMIT|0x80890246|Maximale toegestane standaardfilters tien (10) is overschreden.|
|MPE_FILTER_COMPOSITION_MULTIPLE_FIRST_QUALITY_OPERATOR_NOT_ALLOWED|0x80890248|Meerdere eerste videokwaliteit-operator is niet toegestaan in de filters voor gecombineerde aanvraag.|
|MPE_FILTER_FIRST_QUALITY_ATTRIBUTE_INVALID|0x80890249|Het aantal eerste bitrate kwaliteitskenmerken moet een (1).|
|MPE_HLS_SEGMENT_TOO_LARGE|0x80890243|Duur van HLS-segment moet kleiner zijn dan één derde van de DVR-venster en HLS terug.|
|MPE_KEY_FRAME_INTERVAL_TOO_LARGE|0x808901FE|Fragment duur moet kleiner zijn dan of gelijk is aan ongeveer 20 seconden of de invoer kwaliteitsniveaus zijn niet uitgelijnd tijd.|
|MPE_DTS_RESERVEDBOX_EXPECTED|0x80890105|DTS-specifieke fout, kan de ReservedBox vinden wanneer deze moet aanwezig zijn in de DTSSpecficBox DTS tijdens het parseren.|
|MPE_DTS_INVALID_CHANNEL_COUNT|0x80890106|DTS-specifieke fout: Er zijn geen kanalen gevonden in de DTSSpecficBox tijdens het parseren van DTS-vak.|
|MPE_DTS_SAMPLETYPE_MISMATCH|0x80890107|Voorbeeld van niet-overeenkomend gegevenstype in de DTSSpecficBox DTS-specifieke fout.|
|MPE_DTS_MULTIASSET_DTSH_MISMATCH|0x80890108|DTS-specifieke fout, meerdere asset is ingesteld, maar DTSH voorbeeld type komt niet overeen.|
|MPE_DTS_INVALID_CORESTREAM_SIZE|0x80890109|DTS-specifieke fout, core stroomgrootte is ongeldig.|
|MPE_DTS_INVALID_SAMPLE_RESOLUTION|0x8089010A|DTS-specifieke fout, voorbeeld resolutie is ongeldig.|
|MPE_DTS_INVALID_SUBSTREAM_INDEX|0x8089010B|DTS-specifieke fout, subregio stream extensie index is ongeldig.|
|MPE_DTS_INVALID_BLOCK_NUM|0x8089010C|DTS-specifieke fout, subregio stream blokkeren-nummer is ongeldig.|
|MPE_DTS_INVALID_SAMPLING_FREQUENCE|0x8089010D|DTS-specifieke fout, samplefrequentie is ongeldig.|
|MPE_DTS_INVALID_REFCLOCKCODE|0x8089010E|DTS-specifieke fout, de code van de klok verwijzing in onderliggende stream-uitbreiding is ongeldig.|
|MPE_DTS_INVALID_SPEAKERS_REMAP|0x8089010F|DTS-specifieke fout, het aantal sprekers nieuwe toewijzing set is ongeldig.|

Voor de versleuteling artikelen en voorbeelden, Zie:

- [Concept: beveiliging van inhoud](content-protection-overview.md)
- [Concept: Beleid voor inhoud sleutels](content-key-policy-concept.md)
- [Concept: Beleid voor streaming](streaming-policy-concept.md)
- [Voorbeeld: beveiligen met AES-versleuteling](protect-with-aes128.md)
- [Voorbeeld: beveiligen met DRM](protect-with-drm.md)

Zie voor meer informatie filteren:

- [Concept: dynamische manifesten](filters-dynamic-manifest-overview.md)
- [Concept: filters](filters-concept.md)
- [Voorbeeld: filters maken met REST-API 's](filters-dynamic-manifest-rest-howto.md)
- [Voorbeeld: filters maken met .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Voorbeeld: filters maken met CLI](filters-dynamic-manifest-cli-howto.md)

Voor live-artikelen en voorbeelden, Zie:

- [Concept: overzicht van live streaming](live-streaming-overview.md)
- [Concept: Live-evenementen en Live-uitvoer](live-events-outputs-concept.md)
- [Voorbeeld: live streaming-zelfstudie](stream-live-tutorial-with-api.md)

## <a name="416-range-not-satisfiable"></a>416 bereik niet geldig

|Foutcode|Hexadecimale waarde |Foutbeschrijving|
|---|---|---|
|MPE_STORAGE_INVALID_RANGE|0x808900F1|Opslag-bewerkingsfout, heeft een ongeldig bereik http 416 fout geretourneerd.|

## <a name="500-internal-server-error"></a>500 Interne serverfout

Media Services aangetroffen tijdens de verwerking van de aanvraag een fout waarmee wordt voorkomen de verwerking van u doorgaat dat.  

|Foutcode|Hexadecimale waarde |Foutbeschrijving|
|---|---|---|
|MPE_STORAGE_SOCKET_TIMEOUT|0x808900F4|Ontvangen en vertaalde vanaf Winhttp foutcode ERROR_WINHTTP_TIMEOUT (0x00002ee2).|
|MPE_STORAGE_SOCKET_CONNECTION_ERROR|0x808900F5|Ontvangen en vertaalde vanaf Winhttp foutcode ERROR_WINHTTP_CONNECTION_ERROR (0x00002efe).|
|MPE_STORAGE_SOCKET_NAME_NOT_RESOLVED|0x808900F6|Ontvangen en vertaalde vanaf Winhttp foutcode ERROR_WINHTTP_NAME_NOT_RESOLVED (0x00002ee7).|
|MPE_STORAGE_INTERNAL_ERROR|0x808900E6|Fout in de opslag-bewerking, algemene activiteits-id van een HTTP 500-fouten.|
|MPE_STORAGE_OPERATION_TIMED_OUT|0x808900E7|Fout in de opslag-bewerking, algemene OperationTimedOut van een HTTP 500-fouten.|
|MPE_STORAGE_FAILURE|0x808900F2|Fout in de opslag-bewerking, andere HTTP 500-fouten dan activiteits-id of OperationTimedOut.|

## <a name="503-service-unavailable"></a>503 Service niet beschikbaar

De server is momenteel niet om aanvragen te ontvangen. Deze fout kan worden veroorzaakt door overmatige aanvragen voor de service. Media Services beperking mechanisme Hiermee beperkt u het Resourcegebruik voor toepassingen die overmatige bij de service indienen.

> [!NOTE]
> Controleer het foutbericht en de tekenreeks voor de foutcode voor meer gedetailleerde informatie over de reden je de 503-fout. Deze fout betekent niet altijd beperking.
> 

|Foutcode|Hexadecimale waarde |Foutbeschrijving|
|---|---|---|
|MPE_STORAGE_SERVER_BUSY|0x808900E8|Opslag-bewerkingsfout, ontvangen HTTP-server bezet fout 503.|

## <a name="ask-questions-give-feedback-get-updates"></a>Stel vragen, feedback geven, updates ophalen

Bekijk de [Azure Media Services-community](media-services-community.md) artikel om te zien van verschillende manieren kunt u vragen stellen, feedback te geven en updates over Media Services ophalen.

## <a name="see-also"></a>Zie ook

- [Foutcodes voor codering](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)
- [Azure Media Services-concepten](concepts-overview.md)
- [Quota en beperkingen](limits-quotas-constraints.md)

## <a name="next-steps"></a>Volgende stappen

[Voorbeeld: toegang tot ErrorCode en bericht via ApiException met .NET](configure-connect-dotnet-howto.md#connect-to-the-net-client)
