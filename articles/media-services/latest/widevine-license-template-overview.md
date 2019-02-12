---
title: Overzicht van sjablonen van Azure Media Services met Widevine-licentie | Microsoft Docs
description: In dit onderwerp biedt een overzicht van een Widevine-licentiesjabloon die wordt gebruikt om Widevine-licenties te configureren.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 3615bd88cfadf2f59942fab7678d36d4d20d8c9f
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55992735"
---
# <a name="widevine-license-template-overview"></a>Overzicht van Widevine-licentiesjablonen 

Azure Media Services kunt u voor het versleutelen van uw inhoud met **Google Widevine**. Media Services biedt ook een service voor het leveren van Widevine-licenties. U kunt Azure Media Services-API's gebruiken om Widevine-licenties te configureren. Wanneer een speler probeert uw Widevine beschermde inhoud af te spelen, wordt een aanvraag verzonden naar de service voor het leveren van licenties om de licentie te verkrijgen. Als de licentieservice de aanvraag goedkeurt, wordt de licentie serviceproblemen. Het wordt verzonden naar de client en wordt gebruikt om te ontsleutelen en de opgegeven inhoud af te spelen.

Een aanvraag van Widevine-licentie is opgemaakt als een JSON-bericht.  

>[!NOTE]
> U kunt een leeg bericht alleen met geen waarden maken '{}. " Een licentiesjabloon is gemaakt met standaardwaarden. De standaardwaarde is geschikt voor de meeste gevallen. Scenario's op basis van Microsoft licentielevering moeten altijd de standaardwaarden gebruiken. Als u de "provider" en "content_id" waarden instellen wilt, een provider, moet overeenkomen met Widevine-referenties.

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

| Name | Value | Description |
| --- | --- | --- |
| nettolading |Base64-gecodeerde tekenreeks |De licentieaanvraag is verzonden door een client. |
| content_id |Base64-gecodeerde tekenreeks |ID die wordt gebruikt voor het afleiden van de sleutel-ID en de inhoud voor elke content_key_specs.track_type. |
| provider |string |Gebruikt om te controleren of inhoud sleutels en beleidsregels. Als Microsoft sleutellevering wordt gebruikt voor de levering van Widevine-licentie, is deze parameter wordt genegeerd. |
| Naam_van_beleid |string |De naam van een eerder geregistreerde beleid. Optioneel. |
| allowed_track_types |enum |SD_ONLY of SD_HD. Bepaalt welke inhoud van de sleutels zijn opgenomen in een licentie. |
| content_key_specs |Matrix met JSON structuren, Zie de sectie "Inhoud sleutel specificaties."  |Een nauwkeurigere besturing op welke inhoud sleutels om terug te keren. Zie voor meer informatie de sectie "Inhoud sleutel specificaties." Er kan slechts één van de waarden voor allowed_track_types en content_key_specs worden opgegeven. |
| use_policy_overrides_exclusively |Booleaanse waarde, true of false |Beleid voor kenmerken die zijn opgegeven door policy_overrides gebruiken, en laat alle eerder opgeslagen beleid. |
| policy_overrides |JSON structureren, Zie de sectie "Beleid voor onderdrukkingen." |Instellingen voor deze licentie.  In het geval dat deze asset heeft een vooraf gedefinieerd beleid, wordt deze opgegeven waarden worden gebruikt. |
| session_init |JSON structureren, Zie de sectie "Initialisatie van de sessie." |Optionele gegevens wordt doorgegeven aan de licentie. |
| parse_only |Booleaanse waarde, true of false |De licentieaanvraag wordt geparseerd, maar er is geen licentie wordt uitgegeven. Waarden van de licentieaanvraag worden echter in het antwoord geretourneerd. |

## <a name="content-key-specs"></a>Inhoud specificaties van de sleutel
Als een bestaand beleid bestaat, is er niet nodig om op te geven van de waarden in de inhoud sleutelspecificatie. Het bestaande beleid dat is gekoppeld aan deze inhoud wordt gebruikt om te bepalen van de uitvoerbeveiliging, zoals hoge bandbreedte digitale inhoud Protection (HDCP) en de kopie algemene Management System (CGMS). Als een bestaand beleid niet is geregistreerd met de licentieserver Widevine, kan de provider de waarden invoeren in de licentieaanvraag.   

Elke waarde content_key_specs moet worden opgegeven voor alle sporen te wissen, ongeacht de optie use_policy_overrides_exclusively. 

| Name | Value | Description |
| --- | --- | --- |
| content_key_specs. track_type |string |Een typenaam bijhouden. Als content_key_specs is opgegeven in de licentieaanvraag, zorg ervoor dat alle typen expliciet bijhouden opgeven. Dit niet doet, resulteert in de afgelopen 10 seconden worden afgespeeld is mislukt. |
| content_key_specs  <br/> security_level |uint32 |Hiermee definieert u robuustheid clientvereisten om te worden afgespeeld. <br/> -Software-wit-box cryptografie is vereist. <br/> -Cryptografie software en een verborgen decoder zijn vereist. <br/> -De belangrijkste materiaal en cryptografie-bewerkingen moeten worden uitgevoerd binnen een van vertrouwde hardware ondersteunde uitvoeringsomgeving. <br/> -De codering en decodering van inhoud moet worden uitgevoerd binnen een van vertrouwde hardware ondersteunde uitvoeringsomgeving.  <br/> -De cryptografie, decoderen en alle verwerking van de media (gecomprimeerde en ongecomprimeerde) moeten worden afgehandeld in een vertrouwde hardware ondersteunde uitvoeringsomgeving. |
| content_key_specs <br/> required_output_protection.hdc |tekenreeks, een van de HDCP_NONE, HDCP_V1, HDCP_V2 |Geeft aan of HDCP vereist is. |
| content_key_specs <br/>sleutel |Base64-<br/>gecodeerde tekenreeks |De inhoudssleutel moet worden gebruikt voor dit nummer. Als u opgeeft, is de track_type of key_id vereist. Provider van de inhoud kunt u deze optie gebruiken om te injecteren de inhoudssleutel voor dit nummer in plaats van dat de Widevine-licentie-server of opzoeken van een sleutel genereren. |
| content_key_specs.key_id |Binary base64-gecodeerde tekenreeks, 16 bytes |De unieke id voor de sleutel. |

## <a name="policy-overrides"></a>Beleid negeren
| Name | Value | Description |
| --- | --- | --- |
| policy_overrides&#46;can_play |Booleaanse waarde, true of false |Geeft aan dat het afspelen van de inhoud is toegestaan. De standaardinstelling is onwaar. |
| policy_overrides&#46;can_persist |Booleaanse waarde, true of false |Geeft aan dat de licentie kan worden vastgehouden naar permanente opslag voor offlinegebruik. De standaardinstelling is onwaar. |
| policy_overrides&#46;can_renew |Booleaanse waarde, true of false |Geeft aan dat de verlenging van deze licentie is toegestaan. Indien waar, kan de duur van de licentie kan worden uitgebreid door heartbeat. De standaardinstelling is onwaar. |
| policy_overrides&#46;license_duration_seconds |int64 |Geeft aan dat het tijdvenster voor deze specifieke licentie. Een waarde van 0 geeft aan dat er geen limiet voor de duur. Standaard is 0. |
| policy_overrides&#46;rental_duration_seconds |int64 |Geeft aan het tijdvenster tijdens het afspelen is toegestaan. Een waarde van 0 geeft aan dat er geen limiet voor de duur. Standaard is 0. |
| policy_overrides&#46;playback_duration_seconds |int64 |Het venster weergeven na het afspelen is gestart binnen de duur van de licentie. Een waarde van 0 geeft aan dat er geen limiet voor de duur. Standaard is 0. |
| policy_overrides&#46;renewal_server_url |string |Alle aanvragen van heartbeat (vernieuwen) voor deze licentie wordt omgeleid naar de opgegeven URL. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |
| policy_overrides&#46;renewal_delay_seconds |int64 |Het aantal seconden na license_start_time voordat vernieuwing wordt eerst geprobeerd. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. Standaard is 0. |
| policy_overrides&#46;renewal_retry_interval_seconds |int64 |Hiermee geeft u de vertraging in seconden tussen vernieuwingsaanvragen van de volgende licentie, in geval van storing. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |
| policy_overrides&#46;renewal_recovery_duration_seconds |int64 |Het venster van de tijd in welke afspelen kunt blijven tijdens het vernieuwen wordt uitgevoerd, maar mislukt vanwege problemen met de back-end met de licentieserver. Een waarde van 0 geeft aan dat er geen limiet voor de duur. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |
| policy_overrides&#46;renew_with_usage |Booleaanse waarde, true of false |Geeft aan dat de licentie voor vernieuwing wordt verzonden wanneer gebruik wordt gestart. Dit veld wordt alleen gebruikt als can_renew ingesteld op true is. |

## <a name="session-initialization"></a>De initialisatie-sessie
| Name | Value | Description |
| --- | --- | --- |
| provider_session_token |Base64-gecodeerde tekenreeks |Dit sessietoken wordt doorgegeven in de licentie en bestaat in de volgende vernieuwing. Het sessietoken persistent niet dan sessies. |
| provider_client_token |Base64-gecodeerde tekenreeks |Het clienttoken verzenden terug in het antwoord van de licentie. Als de licentieaanvraag een clienttoken bevat, wordt deze waarde wordt genegeerd. Het clienttoken zich blijft voordoen na licentie-sessies. |
| override_provider_client_token |Booleaanse waarde, true of false |Als false en de licentieaanvraag bevat een clienttoken, gebruikt u de token van de aanvraag, zelfs als een clienttoken is opgegeven in deze structuur. Indien waar, moet u altijd het token dat is opgegeven in deze structuur gebruiken. |

## <a name="configure-your-widevine-license-with-net"></a>Configureren van uw Widevine-licentie met .NET 

Media Services biedt een klasse die u kunt een Widevine-licentie configureren. Kan de licentie, doorgeven JSON naar [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate).

Voor het configureren van de sjabloon, kunt u het volgende doen:

### <a name="directly-construct-a-json-string"></a>Een JSON-tekenreeks rechtstreeks samenstellen

Deze methode kan zijn foutgevoelig zijn. Het verdient aanbeveling gebruik van andere methode, die wordt beschreven [definiëren die nodig zijn de klassen en serialiseren naar JSON](#classes).

    ```csharp
    ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
    {
        WidevineTemplate = @"{""allowed_track_types"":""SD_HD"",""content_key_specs"":[{""track_type"":""SD"",""security_level"":1,""required_output_protection"":{""hdcp"":""HDCP_V2""}}],""policy_overrides"":{""can_play"":true,""can_persist"":true,""can_renew"":false}}"
    };
    ```

### <a id="classes"></a> Benodigde klassen definiëren en te serialiseren naar JSON

#### <a name="define-classes"></a>Klassen definiëren

Het volgende voorbeeld toont een voorbeeld van definities van klassen die zijn toegewezen aan Widevine JSON-schema. U kunt de klassen instantiëren voordat ze serialiseren naar JSON-tekenreeks.  

    ```csharp
    public class PolicyOverrides
    {
        public bool CanPlay { get; set; }
        public bool CanPersist { get; set; }
        public bool CanRenew { get; set; }
        public int RentalDurationSeconds { get; set; }    //Indicates the time window while playback is permitted. A value of 0 indicates that there is no limit to the duration. Default is 0.
        public int PlaybackDurationSeconds { get; set; }  //The viewing window of time after playback starts within the license duration. A value of 0 indicates that there is no limit to the duration. Default is 0.
        public int LicenseDurationSeconds { get; set; }   //Indicates the time window for this specific license. A value of 0 indicates that there is no limit to the duration. Default is 0.
    }

    public class ContentKeySpec
    {
        public string TrackType { get; set; }
        public int SecurityLevel { get; set; }
        public OutputProtection RequiredOutputProtection { get; set; }
    }

    public class OutputProtection
    {
        public string HDCP { get; set; }
    }

    public class WidevineTemplate
    {
        public string AllowedTrackTypes { get; set; }
        public ContentKeySpec[] ContentKeySpecs { get; set; }
        public PolicyOverrides PolicyOverrides { get; set; }
    }
    ```

#### <a name="configure-the-license"></a>De licentie configureren

Klassen die zijn gedefinieerd in de vorige sectie gebruiken om te maken van JSON die wordt gebruikt voor het configureren van [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate):

```csharp
private static ContentKeyPolicyWidevineConfiguration ConfigureWidevineLicenseTempate()
{
    WidevineTemplate template = new WidevineTemplate()
    {
        AllowedTrackTypes = "SD_HD",
        ContentKeySpecs = new ContentKeySpec[]
        {
            new ContentKeySpec()
            {
                TrackType = "SD",
                SecurityLevel = 1,
                RequiredOutputProtection = new OutputProtection()
                {
                    HDCP = "HDCP_V2"
                }
            }
        },
        PolicyOverrides = new PolicyOverrides()
        {
            CanPlay = true,
            CanPersist = true,
            CanRenew = false,
            RentalDurationSeconds = 2592000,
            PlaybackDurationSeconds = 10800,
            LicenseDurationSeconds = 604800,
        }
    };

    ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
    {
        WidevineTemplate = Newtonsoft.Json.JsonConvert.SerializeObject(template)
    };
    return objContentKeyPolicyWidevineConfiguration;
}
```

## <a name="next-steps"></a>Volgende stappen

Bekijk hoe u [beveiligen met DRM](protect-with-drm.md)
