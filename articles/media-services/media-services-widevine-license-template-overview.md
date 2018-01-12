---
title: Overzicht van de sjabloon voor Widevine-licentie | Microsoft Docs
description: In dit onderwerp geeft een overzicht van een Widevine-licentie-sjabloon die wordt gebruikt om Widevine-licenties te configureren.
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
ms.openlocfilehash: 85de5765975b0c55fafe9bb4c14a1c1f435a6d5c
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="widevine-license-template-overview"></a>Overzicht van de sjabloon voor Widevine-licentie
U kunt Azure Media Services gebruiken om te configureren en Google Widevine-licenties aanvragen. Wanneer dit wordt geprobeerd uw Widevine beveiligde inhoud af te spelen, wordt een aanvraag verzonden naar de service voor het leveren van licenties voor het verkrijgen van een licentie. Als de licentieservice de aanvraag goedkeurt, wordt in de service de licentie verstrekt. Het wordt verzonden naar de client en wordt gebruikt om te ontsleutelen en de inhoud van de opgegeven spelen.

Een aanvraag voor Widevine-licentie is opgemaakt als een JSON-bericht.  

>[!NOTE]
> U kunt een leeg bericht maken zonder waarden, alleen '{}." Een licentiesjabloon wordt gemaakt met standaardwaarden. De standaardwaarde gebruiken voor de meeste gevallen. Gebruik altijd de standaardinstellingen op basis van een Microsoft-licentie-levering scenario's. Als u de 'provider' en 'content_id' waarden instellen wilt, moet een provider Widevine referenties overeenkomen.

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
| Provider |tekenreeks |Gebruikt voor het opzoeken van inhoud sleutels en het beleid. Als Microsoft sleutellevering wordt gebruikt voor de levering van Widevine-licentie, wordt deze parameter wordt genegeerd. |
| Naam_van_beleid |tekenreeks |Naam van een eerder geregistreerde beleid. Optioneel. |
| allowed_track_types |Enum |SD_ONLY of SD_HD. Besturingselementen die inhoud van de sleutels zijn opgenomen in een licentie. |
| content_key_specs |Matrix van JSON structuren, Zie de sectie 'Inhoud belangrijke kenmerken.'  |Een besturingselement nauwkeurigere op welke inhoud toetsen om terug te keren. Zie voor meer informatie de sectie 'Inhoud belangrijke kenmerken.' Er kan slechts één van de waarden voor allowed_track_types en content_key_specs opgegeven. |
| use_policy_overrides_exclusively |Booleaanse waarde, true of false |Gebruik beleid kenmerken die zijn opgegeven door policy_overrides en alle eerder opgeslagen beleid weglaten. |
| policy_overrides |JSON structuur, Zie de sectie 'Beleid negeren'. |Beleidsinstellingen voor deze licentie.  In het geval dit activum een vooraf gedefinieerd beleid heeft, worden deze opgegeven waarden gebruikt. |
| session_init |JSON structuur, Zie de sectie "Initialisatie-sessie." |Optionele gegevens worden doorgegeven aan de licentie. |
| parse_only |Booleaanse waarde, true of false |De licentieaanvraag wordt geparseerd, maar er is geen licentie wordt verleend. Waarden van de licentieaanvraag worden echter in het antwoord geretourneerd. |

## <a name="content-key-specs"></a>Inhoud sleutel specificaties
Als een bestaande beleid bestaat, hoeft niet op te geven van de waarden in de inhoud sleutelspecificatie. Het bestaande beleid die zijn gekoppeld aan deze inhoud wordt gebruikt om te bepalen van de beveiliging van de uitvoer, zoals hoge bandbreedte digitale inhoud Protection (HDCP) en de kopie algemene Management System (CGMS). Als een bestaande beleid is niet geregistreerd bij de server Widevine-licentie, kan de inhoudsprovider de waarden invoeren in de licentieaanvraag.   

Elke content_key_specs-waarde moet worden opgegeven voor alle nummers, ongeacht de optie use_policy_overrides_exclusively. 

| Naam | Waarde | Beschrijving |
| --- | --- | --- |
| content_key_specs. track_type |tekenreeks |Een typenaam bijhouden. Als content_key_specs is opgegeven in de licentieaanvraag, zorg ervoor dat u dat alle typen expliciet bijhouden. Niet doet, resulteert in is mislukt om af te spelen na 10 seconden. |
| content_key_specs  <br/> security_level |UInt32 |Hiermee definieert u robuustheid clientvereisten voor afspelen. <br/> -Wit in cryptografie op basis van software is vereist. <br/> -Cryptografie software en een verborgen decoder zijn vereist. <br/> -De materiaal- en cryptografie sleutelbewerkingen moeten worden uitgevoerd binnen een hardwarematige vertrouwde uitvoeringsomgeving. <br/> -De cryptografie en decoderen van inhoud moeten worden uitgevoerd binnen een hardwarematige vertrouwde uitvoeringsomgeving.  <br/> -De cryptografie, decoderen en alle verwerking van de media (gecomprimeerde en ongecomprimeerde) moeten worden verwerkt binnen een hardwarematige vertrouwde uitvoeringsomgeving. |
| content_key_specs <br/> required_output_protection.hDC |tekenreeks een van de HDCP_NONE, HDCP_V1, HDCP_V2 |Hiermee wordt aangegeven of HDCP vereist is. |
| content_key_specs <br/>sleutel |Base64-<br/>gecodeerde tekenreeks |De inhoudssleutel moet worden gebruikt voor dit nummer. Indien opgegeven, is de track_type of key_id vereist. Provider van de inhoud kunt u deze optie gebruiken om te injecteren van de inhoudssleutel voor dit nummer in plaats van de licentieserver Widevine genereren of het opzoeken van een sleutel te laten. |
| content_key_specs.key_id |Binary base64-gecodeerde tekenreeks, 16 bytes |De unieke id voor de sleutel. |

## <a name="policy-overrides"></a>Beleid negeren
| Naam | Waarde | Beschrijving |
| --- | --- | --- |
| policy_overrides. can_play |Booleaanse waarde, true of false |Hiermee wordt aangegeven dat het afspelen van de inhoud is toegestaan. Standaard is ingesteld op false. |
| policy_overrides. can_persist |Booleaanse waarde, true of false |Hiermee wordt aangegeven dat de licentie naar een niet-vluchtige opslag voor offlinegebruik kan worden gehandhaafd. Standaard is ingesteld op false. |
| policy_overrides. can_renew |Booleaanse waarde, true of false |Hiermee wordt aangegeven dat de verlenging van deze licentie is toegestaan. Indien waar, kan de duur van de licentie worden uitgebreid door heartbeat. Standaard is ingesteld op false. |
| policy_overrides. license_duration_seconds |Int64 |Hiermee geeft u het tijdvenster voor deze specifieke licentiegroep. Een waarde van 0 geeft aan dat er geen limiet voor de duur. De standaardwaarde is 0. |
| policy_overrides. rental_duration_seconds |Int64 |Hiermee geeft u het tijdvenster tijdens het afspelen is toegestaan. Een waarde van 0 geeft aan dat er geen limiet voor de duur. De standaardwaarde is 0. |
| policy_overrides. playback_duration_seconds |Int64 |Het weergavevenster na het afspelen is gestart in de duur van de licentie. Een waarde van 0 geeft aan dat er geen limiet voor de duur. De standaardwaarde is 0. |
| policy_overrides. renewal_server_url |tekenreeks |Alle heartbeat (vernieuwing) aanvragen voor deze licentie worden omgeleid naar de opgegeven URL. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |
| policy_overrides. renewal_delay_seconds |Int64 |Hoeveel seconden na license_start_time voordat vernieuwing wordt eerst geprobeerd. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. De standaardwaarde is 0. |
| policy_overrides. renewal_retry_interval_seconds |Int64 |Hiermee wordt de vertraging in seconden tussen opeenvolgende licentie-verlengingsaanvragen in geval van storing. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |
| policy_overrides. renewal_recovery_duration_seconds |Int64 |Het venster van de tijd in welke afspelen kan doorgaan terwijl vernieuwing wordt uitgevoerd, maar mislukt vanwege problemen met de licentieserver back-end. Een waarde van 0 geeft aan dat er geen limiet voor de duur. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |
| policy_overrides. renew_with_usage |Booleaanse waarde, true of false |Hiermee wordt aangegeven dat de licentie voor vernieuwing wordt verzonden wanneer gebruik wordt gestart. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |

## <a name="session-initialization"></a>De initialisatie-sessie
| Naam | Waarde | Beschrijving |
| --- | --- | --- |
| provider_session_token |Base64-gecodeerde tekenreeks |Deze sessietoken terug in de licentie wordt doorgegeven en voorkomt in de daaropvolgende vernieuwingen. Het sessietoken persistent niet afgezien van sessies. |
| provider_client_token |Base64-gecodeerde tekenreeks |Clienttoken te verzenden terug in het antwoord van de licentie. Als de licentieaanvraag een clienttoken bevat, wordt deze waarde wordt genegeerd. Het clienttoken persistente buiten licentie-sessies. |
| override_provider_client_token |Booleaanse waarde, true of false |Als onwaar heeft en de licentieaanvraag bevat een clienttoken, gebruikt u het token van de aanvraag, zelfs als een clienttoken is opgegeven in deze structuur. Indien waar, gebruik altijd het token dat is opgegeven in deze-structuur. |

## <a name="configure-your-widevine-licenses-by-using-net-types"></a>Widevine-licenties configureren met behulp van .NET-typen
Media Services biedt .NET API's die u gebruiken kunt om uw Widevine-licenties te configureren. 

### <a name="classes-as-defined-in-the-media-services-net-sdk"></a>Klassen, zoals gedefinieerd in de Media Services .NET SDK
De volgende klassen zijn de definities van de volgende typen:

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
Het volgende voorbeeld ziet u hoe .NET API's gebruiken om te configureren van een eenvoudige Widevine-licentie:

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
[PlayReady en/of Widevine dynamic common encryption gebruiken](media-services-protect-with-playready-widevine.md)

