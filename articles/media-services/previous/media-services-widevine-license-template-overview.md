---
title: Overzicht van Widevine-licentiesjablonen | Microsoft Docs
description: In dit onderwerp biedt een overzicht van een Widevine-licentiesjabloon die wordt gebruikt om Widevine-licenties te configureren.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 0e6f1f05-7ed6-4ed6-82a0-0cc2182b075a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/05/2018
ms.author: juliako
ms.openlocfilehash: 065845bb2abd02f02fe2050780bc0613cbd2f12c
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51035999"
---
# <a name="widevine-license-template-overview"></a>Overzicht van Widevine-licentiesjablonen
U kunt Azure Media Services gebruiken om te configureren en Google Widevine-licenties aanvragen. Wanneer de speler probeert uw Widevine beschermde inhoud af te spelen, wordt een aanvraag verzonden naar de service voor het leveren van licenties om een licentie te verkrijgen. Als de licentieservice de aanvraag goedkeurt, wordt de licentie serviceproblemen. Het wordt verzonden naar de client en wordt gebruikt om te ontsleutelen en de opgegeven inhoud af te spelen.

Een aanvraag van Widevine-licentie is opgemaakt als een JSON-bericht.  

>[!NOTE]
> U kunt een leeg bericht alleen met geen waarden maken '{}. " Een licentiesjabloon is gemaakt met standaardwaarden. De standaardwaarde is geschikt voor de meeste gevallen. Scenario's op basis van Microsoft licentielevering moeten altijd de standaardwaarden gebruiken. Als u de "provider" en "content_id" waarden instellen wilt, een provider, moet overeenkomen met Widevine-referenties.

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
| content_id |Base64-gecodeerde tekenreeks |ID die wordt gebruikt voor het afleiden van de sleutel-ID en de inhoud voor elke content_key_specs.track_type. |
| Provider |tekenreeks |Gebruikt om te controleren of inhoud sleutels en beleidsregels. Als Microsoft sleutellevering wordt gebruikt voor de levering van Widevine-licentie, is deze parameter wordt genegeerd. |
| Naam_van_beleid |tekenreeks |De naam van een eerder geregistreerde beleid. Optioneel. |
| allowed_track_types |Enum |SD_ONLY of SD_HD. Bepaalt welke inhoud van de sleutels zijn opgenomen in een licentie. |
| content_key_specs |Matrix met JSON structuren, Zie de sectie "Inhoud sleutel specificaties."  |Een nauwkeurigere besturing op welke inhoud sleutels om terug te keren. Zie voor meer informatie de sectie "Inhoud sleutel specificaties." Er kan slechts één van de waarden voor allowed_track_types en content_key_specs worden opgegeven. |
| use_policy_overrides_exclusively |Booleaanse waarde, true of false |Beleid voor kenmerken die zijn opgegeven door policy_overrides gebruiken, en laat alle eerder opgeslagen beleid. |
| policy_overrides |JSON structureren, Zie de sectie "Beleid voor onderdrukkingen." |Instellingen voor deze licentie.  In het geval dat deze asset heeft een vooraf gedefinieerd beleid, wordt deze opgegeven waarden worden gebruikt. |
| session_init |JSON structureren, Zie de sectie "Initialisatie van de sessie." |Optionele gegevens wordt doorgegeven aan de licentie. |
| parse_only |Booleaanse waarde, true of false |De licentieaanvraag wordt geparseerd, maar er is geen licentie wordt uitgegeven. Waarden van de licentieaanvraag worden echter in het antwoord geretourneerd. |

## <a name="content-key-specs"></a>Inhoud specificaties van de sleutel
Als een bestaand beleid bestaat, is er niet nodig om op te geven van de waarden in de inhoud sleutelspecificatie. Het bestaande beleid dat is gekoppeld aan deze inhoud wordt gebruikt om te bepalen van de uitvoerbeveiliging, zoals hoge bandbreedte digitale inhoud Protection (HDCP) en de kopie algemene Management System (CGMS). Als een bestaand beleid niet is geregistreerd met de licentieserver Widevine, kan de provider de waarden invoeren in de licentieaanvraag.   

Elke waarde content_key_specs moet worden opgegeven voor alle sporen te wissen, ongeacht de optie use_policy_overrides_exclusively. 

| Naam | Waarde | Beschrijving |
| --- | --- | --- |
| content_key_specs. track_type |tekenreeks |Een typenaam bijhouden. Als content_key_specs is opgegeven in de licentieaanvraag, zorg ervoor dat alle typen expliciet bijhouden opgeven. Dit niet doet, resulteert in de afgelopen 10 seconden worden afgespeeld is mislukt. |
| content_key_specs  <br/> security_level |UInt32 |Hiermee definieert u robuustheid clientvereisten om te worden afgespeeld. <br/> -Software-wit-box cryptografie is vereist. <br/> -Cryptografie software en een verborgen decoder zijn vereist. <br/> -De belangrijkste materiaal en cryptografie-bewerkingen moeten worden uitgevoerd binnen een van vertrouwde hardware ondersteunde uitvoeringsomgeving. <br/> -De codering en decodering van inhoud moet worden uitgevoerd binnen een van vertrouwde hardware ondersteunde uitvoeringsomgeving.  <br/> -De cryptografie, decoderen en alle verwerking van de media (gecomprimeerde en ongecomprimeerde) moeten worden afgehandeld in een vertrouwde hardware ondersteunde uitvoeringsomgeving. |
| content_key_specs <br/> required_output_protection.hDC |tekenreeks, een van de HDCP_NONE, HDCP_V1, HDCP_V2 |Geeft aan of HDCP vereist is. |
| content_key_specs <br/>sleutel |Base64-<br/>gecodeerde tekenreeks |De inhoudssleutel moet worden gebruikt voor dit nummer. Als u opgeeft, is de track_type of key_id vereist. Provider van de inhoud kunt u deze optie gebruiken om te injecteren de inhoudssleutel voor dit nummer in plaats van dat de Widevine-licentie-server of opzoeken van een sleutel genereren. |
| content_key_specs.key_id |Binary base64-gecodeerde tekenreeks, 16 bytes |De unieke id voor de sleutel. |

## <a name="policy-overrides"></a>Beleid negeren
| Naam | Waarde | Beschrijving |
| --- | --- | --- |
| policy_overrides. can_play |Booleaanse waarde, true of false |Geeft aan dat het afspelen van de inhoud is toegestaan. De standaardinstelling is onwaar. |
| policy_overrides. can_persist |Booleaanse waarde, true of false |Geeft aan dat de licentie kan worden vastgehouden naar permanente opslag voor offlinegebruik. De standaardinstelling is onwaar. |
| policy_overrides. can_renew |Booleaanse waarde, true of false |Geeft aan dat de verlenging van deze licentie is toegestaan. Indien waar, kan de duur van de licentie kan worden uitgebreid door heartbeat. De standaardinstelling is onwaar. |
| policy_overrides. license_duration_seconds |Int64 |Geeft aan dat het tijdvenster voor deze specifieke licentie. Een waarde van 0 geeft aan dat er geen limiet voor de duur. Standaard is 0. |
| policy_overrides. rental_duration_seconds |Int64 |Geeft aan het tijdvenster tijdens het afspelen is toegestaan. Een waarde van 0 geeft aan dat er geen limiet voor de duur. Standaard is 0. |
| policy_overrides. playback_duration_seconds |Int64 |Het venster weergeven na het afspelen is gestart binnen de duur van de licentie. Een waarde van 0 geeft aan dat er geen limiet voor de duur. Standaard is 0. |
| policy_overrides. renewal_server_url |tekenreeks |Alle aanvragen van heartbeat (vernieuwen) voor deze licentie worden doorgestuurd naar de opgegeven URL. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |
| policy_overrides. renewal_delay_seconds |Int64 |Het aantal seconden na license_start_time voordat vernieuwing wordt eerst geprobeerd. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. Standaard is 0. |
| policy_overrides. renewal_retry_interval_seconds |Int64 |Hiermee geeft u de vertraging in seconden tussen vernieuwingsaanvragen van de volgende licentie, in geval van storing. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |
| policy_overrides. renewal_recovery_duration_seconds |Int64 |Het venster van de tijd in welke afspelen kunt blijven tijdens het vernieuwen wordt uitgevoerd, maar mislukt vanwege problemen met de back-end met de licentieserver. Een waarde van 0 geeft aan dat er geen limiet voor de duur. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |
| policy_overrides. renew_with_usage |Booleaanse waarde, true of false |Geeft aan dat de licentie voor vernieuwing wordt verzonden wanneer gebruik wordt gestart. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |

## <a name="session-initialization"></a>De initialisatie-sessie
| Naam | Waarde | Beschrijving |
| --- | --- | --- |
| provider_session_token |Base64-gecodeerde tekenreeks |Dit sessietoken wordt doorgegeven in de licentie en bestaat in de volgende vernieuwing. Het sessietoken persistent niet dan sessies. |
| provider_client_token |Base64-gecodeerde tekenreeks |Het clienttoken verzenden terug in het antwoord van de licentie. Als de licentieaanvraag een clienttoken bevat, wordt deze waarde wordt genegeerd. Het clienttoken zich blijft voordoen na licentie-sessies. |
| override_provider_client_token |Booleaanse waarde, true of false |Als false en de licentieaanvraag bevat een clienttoken, gebruikt u de token van de aanvraag, zelfs als een clienttoken is opgegeven in deze structuur. Indien waar, moet u altijd het token dat is opgegeven in deze structuur gebruiken. |

## <a name="configure-your-widevine-licenses-by-using-net-types"></a>Configureren van uw Widevine-licenties met behulp van .NET-typen
Media Services biedt .NET-API's die u gebruiken kunt om uw Widevine-licenties te configureren. 

### <a name="classes-as-defined-in-the-media-services-net-sdk"></a>Klassen zoals gedefinieerd in de Media Services .NET SDK
De volgende klassen worden de definities van de volgende typen:

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
Het volgende voorbeeld ziet u hoe u .NET-API's gebruiken voor het configureren van een eenvoudige Widevine-licentie:

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
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[PlayReady en/of Widevine dynamic common encryption gebruiken](media-services-protect-with-playready-widevine.md)

