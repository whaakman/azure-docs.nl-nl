---
title: Overzicht van de sjabloon voor Widevine-licentie | Microsoft Docs
description: In dit onderwerp geeft een overzicht van een Widevine-licentie-sjabloon die wordt gebruikt voor het configureren van Widevine-licenties.
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0e6f1f05-7ed6-4ed6-82a0-0cc2182b075a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 667ff16dc7608dab2a5b8b1fd7df715da4620ca1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="widevine-license-template-overview"></a>Overzicht van de sjabloon voor Widevine-licentie
## <a name="overview"></a>Overzicht
Azure Media Services kunt u nu configureren en Widevine-licenties aanvragen. Wanneer de eindgebruiker speler probeert uw Widevine beveiligde inhoud af te spelen, wordt een aanvraag verzonden naar de service voor het leveren van licenties voor het verkrijgen van een licentie. Als de licentieservice de aanvraag goedkeurt, moet deze de licentie die is verzonden naar de client en kan worden gebruikt om te ontsleutelen en de inhoud van de opgegeven spelen uitgeeft.

Widevine-licentie-aanvraag is opgemaakt als een JSON-bericht.  

>[!NOTE]
> U kunt kiezen voor het maken van een leeg bericht zonder waarden alleen '{}' en een licentiesjabloon wordt gemaakt met alle standaardwaarden. De standaardwaarde gebruiken voor de meeste gevallen. Bijvoorbeeld: voor op basis van MS licentie levering scenario's die standaard moeten zijn. Als u hoeft de 'provider' en 'content_id' waarden instellen, moet een provider van Google Widevine referenties overeenkomen.

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

## <a name="json-message"></a>JSON-bericht
| Naam | Waarde | Beschrijving |
| --- | --- | --- |
| nettolading |Base64-gecodeerde tekenreeks |De licentieaanvraag is verzonden door een client. |
| content_id |Base64-gecodeerde tekenreeks |De id die wordt gebruikt voor het afleiden van KeyId(s) en inhoud sleutel (s) voor elke content_key_specs.track_type. |
| Provider |Tekenreeks |Gebruikt voor het opzoeken van inhoud sleutels en het beleid. Als de sleutellevering MS wordt gebruikt voor de levering van Widevine-licentie, wordt deze parameter wordt genegeerd. |
| Naam_van_beleid |Tekenreeks |Naam van een eerder geregistreerde beleid. Optioneel |
| allowed_track_types |Enum |SD_ONLY of SD_HD. Bepaalt welke inhoud sleutels moeten worden opgenomen in een licentie |
| content_key_specs |matrix van JSON-structuren, Zie **inhoud sleutel specificaties** hieronder |Een fijnere geslaagde besturingselement op welke inhoud sleutels om terug te keren. Zie inhoud sleutel Spec hieronder voor meer informatie.  Er kan slechts één van allowed_track_types en content_key_specs worden opgegeven. |
| use_policy_overrides_exclusively |Booleaanse waarde. waar of ONWAAR |Gebruik beleid kenmerken opgegeven door policy_overrides en laat alle eerder opgeslagen beleid. |
| policy_overrides |JSON-structuur, Zie **overschreven** hieronder |Beleidsinstellingen voor deze licentie.  In het geval dit activum een vooraf gedefinieerd beleid heeft, wordt deze opgegeven waarden worden gebruikt. |
| session_init |JSON-structuur, Zie **initialisatie van de sessie** hieronder |Optionele gegevens doorgegeven aan een licentie. |
| parse_only |Booleaanse waarde. waar of ONWAAR |De licentieaanvraag wordt geparseerd, maar er is geen licentie wordt verleend. De waarden echter formulier de licentieaanvraag zijn in het antwoord geretourneerd. |

## <a name="content-key-specs"></a>Inhoudssleutel specificaties
Als een bestaande beleid bestaat, hoeft niet op te geven van de waarden in de inhoud sleutel-specificaties.  Het bestaande beleid die zijn gekoppeld aan deze inhoud wordt gebruikt om te bepalen van de beveiliging van de uitvoer zoals HDCP en CGMS.  Als u een reeds bestaande beleid is niet geregistreerd bij de Server Widevine-licentie, kan de inhoudsprovider de waarden invoeren in de licentieaanvraag.   

Elke content_key_specs moet worden opgegeven voor alle nummers, ongeacht de optie use_policy_overrides_exclusively. 

| Naam | Waarde | Beschrijving |
| --- | --- | --- |
| content_key_specs. track_type |Tekenreeks |Een typenaam bijhouden. Als content_key_specs is opgegeven in de licentieaanvraag, zorg ervoor dat u dat alle typen expliciet bijhouden. Om dit te doen, treedt tot mislukte pogingen om het afspelen te afgelopen 10 seconden. |
| content_key_specs  <br/> security_level |UInt32 |Hiermee definieert u robuustheid clientvereisten voor afspelen. <br/> 1 - softwarematige whitebox crypto is vereist. <br/> 2 - crypto-software en een verborgen decoder is vereist. <br/> 3 - de sleutelmateriaal opslaat en de cryptografische bewerkingen moeten worden uitgevoerd binnen een hardware back-ups vertrouwde uitvoeringsomgeving. <br/> 4 - de codering en decodering van inhoud moeten worden uitgevoerd binnen een hardware back-ups vertrouwde uitvoeringsomgeving.  <br/> 5 - de crypto, decoderen en alle verwerkingstijd van de media (gecomprimeerde en ongecomprimeerde) moeten worden verwerkt binnen een hardware back-ups vertrouwde uitvoeringsomgeving. |
| content_key_specs <br/> required_output_protection.hDC |String - een van: HDCP_NONE, HDCP_V1, HDCP_V2 |Hiermee wordt aangegeven of HDCP nodig is |
| content_key_specs <br/>sleutel |Base64 <br/>gecodeerde tekenreeks |De inhoudssleutel moet worden gebruikt voor dit nummer. Indien opgegeven, is de track_type of key_id vereist.  Deze optie kunt de provider van de inhoud de inhoudssleutel voor dit nummer in plaats van de licentieserver Widevine genereren of het opzoeken van een sleutel laten invoeren. |
| content_key_specs.key_id |Met base64 gecodeerde tekenreeks binaire, 16 bytes |De unieke id voor de sleutel. |

## <a name="policy-overrides"></a>Beleid negeren
| Naam | Waarde | Beschrijving |
| --- | --- | --- |
| policy_overrides. can_play |Booleaanse waarde. waar of ONWAAR |Hiermee wordt aangegeven dat het afspelen van de inhoud is toegestaan. Standaard is ingesteld op false. |
| policy_overrides. can_persist |Booleaanse waarde. waar of ONWAAR |Hiermee wordt aangegeven dat de licentie kan worden opgeslagen in niet-vluchtige opslag voor offlinegebruik. Standaard is ingesteld op false. |
| policy_overrides. can_renew |Booleaanse waarde waar of ONWAAR |Hiermee wordt aangegeven dat de verlenging van deze licentie is toegestaan. Indien waar, kan de duur van de licentie worden uitgebreid door heartbeat. Standaard is ingesteld op false. |
| policy_overrides. license_duration_seconds |Int64 |Hiermee geeft u het tijdvenster voor deze specifieke licentiegroep. Een waarde van 0 geeft aan dat er geen limiet voor de duur. De standaardwaarde is 0. |
| policy_overrides. rental_duration_seconds |Int64 |Hiermee geeft u het tijdvenster tijdens het afspelen is toegestaan. Een waarde van 0 geeft aan dat er geen limiet voor de duur. De standaardwaarde is 0. |
| policy_overrides. playback_duration_seconds |Int64 |Het weergavevenster tijd wanneer het afspelen is gestart in de duur van de licentie. Een waarde van 0 geeft aan dat er geen limiet voor de duur. De standaardwaarde is 0. |
| policy_overrides. renewal_server_url |Tekenreeks |Alle heartbeat (vernieuwing) aanvragen voor deze licentie wordt omgeleid naar de opgegeven URL. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |
| policy_overrides. renewal_delay_seconds |Int64 |Hoeveel seconden na license_start_time, voordat de vernieuwing wordt eerst geprobeerd. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. Standaardwaarde is 0 |
| policy_overrides. renewal_retry_interval_seconds |Int64 |Hiermee wordt de vertraging in seconden tussen opeenvolgende licentie-verlengingsaanvragen in geval van storing. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |
| policy_overrides. renewal_recovery_duration_seconds |Int64 |Het venster van de tijd waarin afspelen is toegestaan om door te gaan tijdens het vernieuwen is geprobeerde, nog mislukt vanwege problemen met de licentieserver back-end. Een waarde van 0 geeft aan dat er geen limiet voor de duur. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |
| policy_overrides. renew_with_usage |Booleaanse waarde waar of ONWAAR |Hiermee wordt aangegeven dat de licentie voor verlenging moet worden verzonden wanneer gebruik wordt gestart. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |

## <a name="session-initialization"></a>De initialisatie-sessie
| Naam | Waarde | Beschrijving |
| --- | --- | --- |
| provider_session_token |Base64-gecodeerde tekenreeks |Deze sessietoken terug in de licentie wordt doorgegeven en in daaropvolgende vernieuwingen aanwezig.  Het sessietoken bewaard niet afgezien van sessies. |
| provider_client_token |Base64-gecodeerde tekenreeks |Clienttoken te verzenden terug in het antwoord van de licentie.  Als de licentieaanvraag een clienttoken bevat, wordt deze waarde wordt genegeerd. Het clienttoken bewaard buiten licentie-sessies. |
| override_provider_client_token |Booleaanse waarde. waar of ONWAAR |Als onwaar heeft en de licentieaanvraag bevat een clienttoken, gebruikt u het token van de aanvraag, zelfs als een clienttoken is opgegeven in deze structuur.  Indien waar, gebruik altijd het token dat is opgegeven in deze-structuur. |

## <a name="configure-your-widevine-licenses-using-net-types"></a>Configureren van uw .NET-typen met Widevine-licenties
Media Services biedt .NET API's waarmee u uw Widevine-licenties te configureren. 

### <a name="classes-as-defined-in-the-media-services-net-sdk"></a>Klassen, zoals gedefinieerd in de Media Services .NET SDK
Hieronder vindt u de definities van de volgende typen.

    public class WidevineMessage
    {
        public WidevineMessage();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

### <a name="example"></a>Voorbeeld
Het volgende voorbeeld laat zien hoe .NET API's gebruiken om te configureren van een eenvoudige Widevine-licentie.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[PlayReady en/of Widevine Dynamic Common Encryption gebruiken](media-services-protect-with-drm.md)

