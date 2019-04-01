---
title: Toegang tot gegevens met behulp van Azure Security Center voor IoT-Preview | Microsoft Docs
description: Meer informatie over hoe u toegang tot uw beveiligingsgegevens voor waarschuwing en de aanbeveling bij het gebruik van Azure Security Center voor IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: fbd96ddd-cd9f-48ae-836a-42aa86ca222d
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 510ee9bdefe87c1fab40e58bb715f2a8cce936b7
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758529"
---
# <a name="access-your-security-data"></a>Toegang tot uw beveiligingsgegevens 

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Azure Security Center (ASC) voor IoT slaat beveiligingswaarschuwingen, aanbevelingen en onbewerkte beveiligingsgegevens (als u ervoor kiest het op te slaan) in uw Log Analytics-werkruimte.

## <a name="log-analytics"></a>Log Analytics

Configureren welke Log Analytics-werkruimte wordt gebruikt:

1. Open uw IoT-hub.
1. Klik op **beveiliging**
2. Klik op **instellingen**, en de configuratie van uw Log Analytics-werkruimte wijzigen.

Toegang krijgen tot uw Log Analytics-werkruimte na de configuratie:

1. Kies een waarschuwing of een aanbeveling in ASC voor IoT. 
2. Klik op **nader onderzoek**, klikt u vervolgens op **om te zien welke apparaten deze waarschuwing klikt u hier en weergeven van de apparaat-id-kolom**.

Zie voor meer informatie over het opvragen van gegevens van Log Analytics [aan de slag met query's in Log Analytics](https://docs.microsoft.com//azure/log-analytics/query-language/get-started-queries).

## <a name="security-alerts"></a>Beveiligingswaarschuwingen

Beveiligingswaarschuwingen zijn opgeslagen in _AzureSecurityOfThings.SecurityAlert_ tabel in de Log Analytics-werkruimte die is geconfigureerd voor de ASC voor IoT-oplossing.

We hebt opgegeven een aantal handige query's om u te helpen aan de slag beveiligingswaarschuwingen verkennen.

### <a name="sample-records"></a>Voorbeeldrecords

Selecteer een paar willekeurige records

```
// Select a few random records
//
SecurityAlert
| project 
    TimeGenerated, 
    IoTHubId=ResourceId, 
    DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"]),
    AlertSeverity, 
    DisplayName,
    Description,
    ExtendedProperties
| take 3
```

| TimeGenerated           | IoTHubId                                                                                                       | DeviceId      | AlertSeverity | DisplayName                           | Description                                             | ExtendedProperties                                                                                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2018-11-18T18:10:29.000 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | < apparaatnaam > | Hoog          | Brute force-aanval is geslaagd           | Een Brute force-aanval op het apparaat is geslaagd        |    {"Volledige bronadres": "[\"10.165.12.18:\"]", "Naam": "[\"\"] ', 'DeviceId': "IoT-Device-Linux" }                                                                       |
| 2018-11-19T12:40:31.000 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | < apparaatnaam > | Hoog          | Lokale aanmelding die is voltooid op apparaat      | Een geslaagde lokale aanmelding op het apparaat is gedetecteerd     | { "Remote Address": "?", "Remote Port": "", "Local Port": "", "Login Shell": "/bin/su", "Login Process Id": '28207', 'gebruikersnaam': 'aanvaller', 'DeviceId': "IoT-Device-Linux" } |
| 2018-11-19T12:40:31.000 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | < apparaatnaam > | Hoog          | Poging van de lokale aanmelding die op het apparaat is mislukt  | Een mislukte lokale aanmeldingspoging met het apparaat is gedetecteerd |  { "Remote Address": "?", "Remote Port": "", "Local Port": "", "Login Shell": "/bin/su", "Login Process Id": '22644', 'gebruikersnaam': 'aanvaller', 'DeviceId': "IoT-Device-Linux" } |

### <a name="device-summary"></a>Apparaatsamenvatting

Selecteer het aantal afzonderlijke beveiligingswaarschuwingen afgelopen week door IoT Hub, apparaat, de ernst van waarschuwing, type waarschuwing gedetecteerd.

```
// Select number of distinct security alerts detected last week by 
//   IoT hub, device, alert severity, alert type
//
SecurityAlert
| where TimeGenerated > ago(7d)
| summarize Cnt=dcount(SystemAlertId) by
    IoTHubId=ResourceId, 
    DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"]),
    AlertSeverity,
    DisplayName
```

| IoTHubId                                                                                                       | DeviceId      | AlertSeverity | DisplayName                           | Count |
|----------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------|-----|
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | < apparaatnaam > | Hoog          | Brute force-aanval is geslaagd           | 9   |   
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | < apparaatnaam > | Middelgroot        | Poging van de lokale aanmelding die op het apparaat is mislukt  | 242 |    
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | < apparaatnaam > | Hoog          | Lokale aanmelding die is voltooid op apparaat      | 31  |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | < apparaatnaam > | Middelgroot        | Crypto Coin Miner                     | 4   |

### <a name="iot-hub-summary"></a>IoT hub-overzicht

Selecteer een aantal unieke apparaten met waarschuwingen in de afgelopen week, via de IoT Hub, de ernst van waarschuwing Waarschuwingstype

```
// Select number of distinct devices which had alerts in the last week, by 
//   IoT hub, alert severity, alert type
//
SecurityAlert
| where TimeGenerated > ago(7d)
| extend DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"])
| summarize CntDevices=dcount(DeviceId) by
    IoTHubId=ResourceId, 
    AlertSeverity,
    DisplayName
```

| IoTHubId                                                                                                       | AlertSeverity | DisplayName                           | CntDevices |
|----------------------------------------------------------------------------------------------------------------|---------------|---------------------------------------|------------|
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Hoog          | Brute force-aanval is geslaagd           | 1          |    
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Middelgroot        | Poging van de lokale aanmelding die op het apparaat is mislukt  | 1          | 
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Hoog          | Lokale aanmelding die is voltooid op apparaat      | 1          |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Middelgroot        | Crypto Coin Miner                     | 1          |

## <a name="security-recommendations"></a>Aanbevelingen voor beveiliging

Aanbevelingen voor beveiliging worden opgeslagen in _AzureSecurityOfThings.SecurityRecommendation_ tabel in de Log Analytics-werkruimte die is geconfigureerd voor de ASC voor IoT-oplossing.

We hebt een aantal handige query's kunt u start het verkennen van de aanbevelingen voor beveiliging opgegeven.

### <a name="sample-records"></a>Voorbeeldrecords

Selecteer een paar willekeurige records

```
// Select a few random records
//
SecurityRecommendation
| project 
    TimeGenerated, 
    IoTHubId=AssessedResourceId, 
    DeviceId,
    RecommendationSeverity,
    RecommendationState,
    RecommendationDisplayName,
    Description,
    RecommendationAdditionalData
| take 2
```
    
| TimeGenerated | IoTHubId | DeviceId | RecommendationSeverity | RecommendationState | RecommendationDisplayName | Description | RecommendationAdditionalData |
|---------------|----------|----------|------------------------|---------------------|---------------------------|-------------|------------------------------|
| 2019-03-22T10:21:06.060 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | < apparaatnaam > | Middelgroot | Actief | Strikte firewallregel in de invoer keten is gevonden | Er is een regel in de firewall aangetroffen die een patroon met veel machtigingen voor een groot aantal IP-adressen of poorten bevat. | {'Regels': ' [{\"bronadres\":\"\",\"bronpoort\":\"\",\"DestinationAddress\":\" \" \"DestinationPort\":\"1337\"}] "} |
| 2019-03-22T10:50:27.237 | /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | < apparaatnaam > | Middelgroot | Actief | Strikte firewallregel in de invoer keten is gevonden | Er is een regel in de firewall aangetroffen die een patroon met veel machtigingen voor een groot aantal IP-adressen of poorten bevat. | {'Regels': ' [{\"bronadres\":\"\",\"bronpoort\":\"\",\"DestinationAddress\":\" \" \"DestinationPort\":\"1337\"}] "} |

### <a name="device-summary"></a>Apparaatsamenvatting

Selecteer het aantal afzonderlijke active beveiligingsaanbevelingen door IoT Hub, apparaat, aanbeveling ernst en type.

```
// Select number of distinct active security recommendations by 
//   IoT hub, device, recommendation severity and type
//
SecurityRecommendation
| extend IoTHubId=AssessedResourceId
| summarize CurrentState=arg_max(RecommendationState, DiscoveredTimeUTC) by IoTHubId, DeviceId, RecommendationSeverity, RecommendationDisplayName
| where CurrentState == "Active"
| summarize Cnt=count() by IoTHubId, DeviceId, RecommendationSeverity
```

| IoTHubId                                                                                                       | DeviceId      | RecommendationSeverity | Count |
|----------------------------------------------------------------------------------------------------------------|---------------|------------------------|-----|
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | < apparaatnaam > | Hoog          | 2   |    
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | < apparaatnaam > | Middelgroot        | 1 |  
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | < apparaatnaam > | Hoog          | 1  |
| /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | < apparaatnaam > | Middelgroot        | 4   |


## <a name="next-steps"></a>Volgende stappen

- Lees de ASC voor IoT [overzicht](overview.md)
- Meer informatie over ASC voor IoT [architectuur](architecture.md)
- Begrijpen en verkennen [ASC voor IoT-waarschuwingen](concept-security-alerts.md)
- Begrijpen en verkennen [ASC voor IoT-aanbeveling](concept-recommendations.md)