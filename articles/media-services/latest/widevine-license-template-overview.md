---
title: Azure Media Services met Widevine-licentie sjabloon overzicht | Microsoft Docs
description: In dit onderwerp geeft een overzicht van een Widevine-licentie-sjabloon die wordt gebruikt om Widevine-licenties te configureren.
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
ms.date: 06/27/2018
ms.author: juliako
ms.openlocfilehash: e3af5efd253458401c13f6174d9567f932482eb0
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37132883"
---
# <a name="widevine-license-template-overview"></a>Overzicht van de sjabloon voor Widevine-licentie

U kunt Azure Media Services gebruiken om te configureren en Google Widevine-licenties aanvragen. Wanneer dit wordt geprobeerd uw Widevine beveiligde inhoud af te spelen, wordt een aanvraag verzonden naar de service voor het leveren van licenties voor het verkrijgen van een licentie. Als de licentieservice de aanvraag goedkeurt, wordt in de service de licentie verstrekt. Het wordt verzonden naar de client en wordt gebruikt om te ontsleutelen en de inhoud van de opgegeven spelen.

Een aanvraag voor Widevine-licentie is opgemaakt als een JSON-bericht.  

>[!NOTE]
> U kunt alleen een leeg bericht zonder waarden maken '{}. " Een licentiesjabloon wordt gemaakt met standaardwaarden. De standaardwaarde gebruiken voor de meeste gevallen. Gebruik altijd de standaardinstellingen op basis van een Microsoft-licentie-levering scenario's. Als u de 'provider' en 'content_id' waarden instellen wilt, moet een provider Widevine referenties overeenkomen.

    {  
       "payload":"<license challenge>",
       "content_id": "<content id>"
       "provider": "<provider>"
       "allowed_track_types":"<types>",
       "content_key_specs":[  
          {  
             "track_type":"<track type 1>"
          },
          {  
             "track_type":"<track type 2>"
          },
          …
       ],
       "policy_overrides":{  
          "can_play":<can play>,
          "can persist":<can persist>,
          "can_renew":<can renew>,
          "rental_duration_seconds":<rental duration>,
          "playback_duration_seconds":<playback duration>,
          "license_duration_seconds":<license duration>,
          "renewal_recovery_duration_seconds":<renewal recovery duration>,
          "renewal_server_url":"<renewal server url>",
          "renewal_delay_seconds":<renewal delay>,
          "renewal_retry_interval_seconds":<renewal retry interval>,
          "renew_with_usage”:<renew with usage>
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
| policy_overrides&#46;can_play |Booleaanse waarde, true of false |Hiermee wordt aangegeven dat het afspelen van de inhoud is toegestaan. De standaardinstelling is onwaar. |
| policy_overrides&#46;can_persist |Booleaanse waarde, true of false |Hiermee wordt aangegeven dat de licentie naar een niet-vluchtige opslag voor offlinegebruik kan worden gehandhaafd. De standaardinstelling is onwaar. |
| policy_overrides&#46;can_renew |Booleaanse waarde, true of false |Hiermee wordt aangegeven dat de verlenging van deze licentie is toegestaan. Indien waar, kan de duur van de licentie worden uitgebreid door heartbeat. De standaardinstelling is onwaar. |
| policy_overrides&#46;license_duration_seconds |Int64 |Hiermee geeft u het tijdvenster voor deze specifieke licentiegroep. Een waarde van 0 geeft aan dat er geen limiet voor de duur. De standaardwaarde is 0. |
| policy_overrides&#46;rental_duration_seconds |Int64 |Hiermee geeft u het tijdvenster tijdens het afspelen is toegestaan. Een waarde van 0 geeft aan dat er geen limiet voor de duur. De standaardwaarde is 0. |
| policy_overrides&#46;playback_duration_seconds |Int64 |Het weergavevenster na het afspelen is gestart in de duur van de licentie. Een waarde van 0 geeft aan dat er geen limiet voor de duur. De standaardwaarde is 0. |
| policy_overrides&#46;renewal_server_url |tekenreeks |Alle heartbeat (vernieuwing) aanvragen voor deze licentie worden omgeleid naar de opgegeven URL. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |
| policy_overrides&#46;renewal_delay_seconds |Int64 |Hoeveel seconden na license_start_time voordat vernieuwing wordt eerst geprobeerd. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. De standaardwaarde is 0. |
| policy_overrides&#46;renewal_retry_interval_seconds |Int64 |Hiermee wordt de vertraging in seconden tussen opeenvolgende licentie-verlengingsaanvragen in geval van storing. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |
| policy_overrides&#46;renewal_recovery_duration_seconds |Int64 |Het venster van de tijd in welke afspelen kan doorgaan terwijl vernieuwing wordt uitgevoerd, maar mislukt vanwege problemen met de licentieserver back-end. Een waarde van 0 geeft aan dat er geen limiet voor de duur. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |
| policy_overrides&#46;renew_with_usage |Booleaanse waarde, true of false |Hiermee wordt aangegeven dat de licentie voor vernieuwing wordt verzonden wanneer gebruik wordt gestart. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |

## <a name="session-initialization"></a>De initialisatie-sessie
| Naam | Waarde | Beschrijving |
| --- | --- | --- |
| provider_session_token |Base64-gecodeerde tekenreeks |Deze sessietoken terug in de licentie wordt doorgegeven en voorkomt in de daaropvolgende vernieuwingen. Het sessietoken persistent niet afgezien van sessies. |
| provider_client_token |Base64-gecodeerde tekenreeks |Clienttoken te verzenden terug in het antwoord van de licentie. Als de licentieaanvraag een clienttoken bevat, wordt deze waarde wordt genegeerd. Het clienttoken persistente buiten licentie-sessies. |
| override_provider_client_token |Booleaanse waarde, true of false |Als onwaar heeft en de licentieaanvraag bevat een clienttoken, gebruikt u het token van de aanvraag, zelfs als een clienttoken is opgegeven in deze structuur. Indien waar, gebruik altijd het token dat is opgegeven in deze-structuur. |

## <a name="configure-your-widevine-license-with-net"></a>Widevine-licentie configureren met .NET 

Media Services biedt een klasse die u kunt een Widevine-licentie configureren. Kan de licentie, doorgeven JSON naar [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate).

De sjabloon die u kunt configureren:

1.  Rechtstreeks constructie/hardcode een JSON-tekenreeks (die mogelijk foutgevoelige);

    ```csharp
        ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
    {
        WidevineTemplate = @"{""allowed_track_types"":""SD_HD"",""content_key_specs"":[{""track_type"":""SD"",""security_level"":1,""required_output_protection"":{""hdcp"":""HDCP_V2""}}],""policy_overrides"":{""can_play"":true,""can_persist"":true,""can_renew"":false}}"
    };
    ```

2.  Constructie klassen nodig met de eigenschappen toewijzen aan deze JSON-kenmerken en exemplaar te maken voordat ze serialiseren naar JSON-tekenreeks. Hieronder volgt een voorbeeld van dergelijke klassen en hoe ze geïnstantieerd en geserialiseerd.

    ```csharp
    public class policy_overrides
    {
        public bool can_play { get; set; }
        public bool can_persist { get; set; }
        public bool can_renew { get; set; }
        public int rental_duration_seconds { get; set; }    //Indicates the time window while playback is permitted. A value of 0 indicates that there is no limit to the duration. Default is 0.
        public int playback_duration_seconds { get; set; }  //The viewing window of time after playback starts within the license duration. A value of 0 indicates that there is no limit to the duration. Default is 0.
        public int license_duration_seconds { get; set; }   //Indicates the time window for this specific license. A value of 0 indicates that there is no limit to the duration. Default is 0.
    }

    public class content_key_spec
    {
        public string track_type { get; set; }
        public int security_level { get; set; }
        public output_protection required_output_protection { get; set; }
    }

    public class output_protection
    {
        public string hdcp { get; set; }
    }

    public class widevine_template
    {
        public string allowed_track_types { get; set; }
        public content_key_spec[] content_key_specs { get; set; }
        public policy_overrides policy_overrides { get; set; }
    }
    ```

### <a name="configure-the-license"></a>De licentie configureren

Klassen die zijn gedefinieerd in de vorige sectie gebruiken om te maken van JSON die wordt gebruikt voor het configureren van [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate):

```csharp
void ConfigureLicense()
{
    widevine_template objwidevine_template = new widevine_template()
    {
        allowed_track_types = "SD_HD",
        content_key_specs = new content_key_spec[]
        {
            new content_key_spec()
            {
                track_type = "SD",
                security_level = 1,
                required_output_protection = new output_protection()
                {
                hdcp = "HDCP_V2"
                }
            }
        },
        policy_overrides = new policy_overrides()
        {
            can_play = true,
            can_persist = true,
            can_renew = false,
            license_duration_seconds = 2592000,
            playback_duration_seconds = 10800,
            rental_duration_seconds = 604800,
        }
    };

    ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
    {
        WidevineTemplate = Newtonsoft.Json.JsonConvert.SerializeObject(objwidevine_template)
    };
}
```

## <a name="next-steps"></a>Volgende stappen

[Overzicht](content-protection-overview.md)