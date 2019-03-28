---
title: Stapsgewijze instructies voor het wijzigen van een ASC voor IoT-moduledubbel in ASC voor IoT-Preview | Microsoft Docs
description: Informatie over het wijzigen van een ASC voor IoT security moduledubbel voor ASC voor IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 1bc5dc86-0f33-4625-b3d3-f9b6c1a54e14
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 4bb25db7feddea66e75d9dd069aaec7785faf738
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541945"
---
# <a name="modify-an-asc-for-iot-module-twin"></a>Wijzigen van een ASC voor IoT-moduledubbel

> [!IMPORTANT]
> ASC voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

In dit artikel wordt uitgelegd hoe u de configuratie van een bestaande wijzigen **AzureIoTSecurity moduledubbel** voor een bestaand apparaat. 

Zie [maken van een ASC voor IoT-module](quickstart-create-security-twin.md) voor informatie over het maken van een nieuwe security-module voor een nieuw apparaat.  

## <a name="modification-considerations"></a>Overwegingen voor aanpassing

> [!IMPORTANT]
> Elke configuratie in de configuratie van de dubbele overschrijft de standaardconfiguratie. Als een specifieke configuratie niet meer aanwezig zijn in de configuratie van dubbel is, wordt de standaardconfiguratie gebruikt. 

## <a name="configuration-schema-and-validation"></a>Het schema en validatie 

Zorg ervoor dat u de configuratie van de agent op basis van dit [schema](https://github.com/Azure/asc-for-iot/schema/security_module_twin). Een agent wordt niet gestart als het configuratieobject komt niet overeen met het schema.

 
Als, terwijl de agent wordt uitgevoerd, wordt het configuratieobject gewijzigd naar een ongeldige configuratie (de configuratie komt niet overeen met het schema), wordt de agent de ongeldige configuratie worden genegeerd en wordt voortgezet met behulp van de huidige configuratie. 

## <a name="edit-a-property"></a>Een eigenschap bewerken  

Alle aangepaste eigenschappen in het object voor de configuratie van agent binnen de moduledubbel AzureIoTSecurity instellen. 

Een eigenschap wilt instellen, toevoegen de sleutel voor het configuratieobject met de gewenste waarde. Verwijder de eigenschap van de configuration-object voor het gebruik van de waarde van een standaard-eigenschap:

```json
"desired": { //AzureIoTSecurity Module Identity Twin – desired properties section  
  "azureiot*com^securityAgentConfiguration^1*0*0": { //ASC for IoT Agent 
      // configuration section  
    "hubResourceId": "/subscriptions/82392767-31d3-4bd2-883d-9b60596f5f42/resourceGroups/myResourceGroup/providers/Microsoft.Devices/IotHubs/myIotHub",     
    "lowPriorityMessageFrequency": "PT1H",     
    "highPriorityMessageFrequency": "PT7M",    
    "eventPriorityFirewallConfiguration": "High",     
    "eventPriorityConnectionCreate": "Off" 
  } 
}, 
```

## <a name="properties"></a>Properties 
De volgende tabel bevat alle van de configureerbare eigenschappen die ASC voor IoT-agents regelen. 
          

| Name| Status | Geldige waarden| Standaardwaarden| Description |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|highPriorityMessageFrequency|Vereist: false |Geldige waarden: De duur in ISO 8601-indeling |Standaardwaarde: PT7M |Maximale tijd vóór berichten met hoge prioriteit worden verzonden.|
|lowPriorityMessageFrequency |Vereist: false|Geldige waarden: De duur in ISO 8601-indeling |Standaardwaarde: PT5H |Maximale tijd vóór berichten met een lage prioriteit worden verzonden.| 
|snapshotFrequency |Vereist: false|Geldige waarden: duur in ISO 8601-notatie |Standaardwaarde PT13H |Het tijdsinterval voor het maken van momentopnamen van de status van apparaat.| 
|maxLocalCacheSizeInBytes |Vereist: false |Geldige waarden: |Standaardwaarde: 2560000, groter is dan 8192 liggen | Maximale opslag (in bytes) toegestaan voor het cachegeheugen voor statusberichten van een agent. Maximale hoeveelheid ruimte die zijn toegestaan voor het opslaan van berichten op het apparaat voordat berichten worden verzonden.| 
|maxMessageSizeInBytes |Vereist: false |Geldige waarden: Een positief getal, groter is dan 8192, minder dan 262144 |Standaardwaarde: 204800 |Maximale toegestane grootte van een agent op cloudbericht. Deze instelling bepaalt de hoeveelheid gegevens in elk bericht dat wordt verzonden. |
|eventPriority${EventName} |Vereist: false |Geldige waarden: Zeer laag, hoog uitschakelen |Standaardwaarden: |Prioriteit van elke agent gebeurtenis is gegenereerd. | 
|

### <a name="events"></a>Gebeurtenissen

De volgende lijst met gebeurtenissen worden alle gebeurtenissen de ASC voor IoT-agent kan verzamelen van uw apparaten. Gebruik de moduledubbel AzureIotSecurity om te configureren welke van deze gebeurtenissen worden verzameld en hun prioriteit in uw oplossing bepalen. 
 
|Gebeurtenisnaam| propertyName | Standaardwaarde| Momentopname van gebeurtenis| Gedetailleerde status  |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|Diagnostische gebeurtenissen|eventPriorityDiagnostic| Uit| False| Agent gerelateerde diagnostische gebeurtenissen. Gebruik deze gebeurtenis voor uitgebreide logboekregistratie.| 
Configuratiefout |eventPriorityConfigurationError |Laag |False |Parseren van de configuratie van de agent is mislukt. Controleer de configuratie op basis van het schema.| 
|Verwijderde gebeurtenissen statistieken |eventPriorityDroppedEventsStatistics |Laag |True|Agent de bijbehorende gebeurtenis statistische gegevens. |
|Statistieken van berichten|eventPriorityMessageStatistics |Laag |True |Agent gerelateerde statistieken van berichten. |
|Verbonden hardware|eventPriorityConnectedHardware |Laag |True |Momentopname van alle hardware die is verbonden met het apparaat.|
|Luisterende poorten|eventPriorityListeningPorts |Hoog |True |Momentopname van alle geopende luisterende poorten op het apparaat.|
| Proces maken |eventPriorityProcessCreate |Laag |False |Controles verwerken maken op het apparaat.|
| Proces beëindigen|eventPriorityProcessTerminate |Laag |False |Controles verwerken beëindiging op het apparaat.| 
 Systeemgegevens |eventPrioritySystemInformation |Laag |True |Een momentopname van de systeeminformatie, zoals de OS- of CPU.|
|Lokale gebruikers| eventPriorityLocalUsers |Hoog |True|Een momentopname van de geregistreerde lokale gebruikers binnen het systeem. |
|Aanmelden|  eventPriorityLogin |Hoog|False|Voert een controle uit de aanmeldgebeurtenissen op het apparaat (lokale en externe aanmeldingen).|
|Verbinding maken |eventPriorityConnectionCreate|Laag|False|Voert een controle uit TCP-verbindingen die zijn gemaakt en naar het apparaat. |
|Firewall-configuratie| eventPriorityFirewallConfiguration|Laag|True|Momentopname van de firewall apparaatconfiguratie (firewall-regels). |
|Basislijn van het besturingssysteem| eventPriorityOSBaseline| Laag|True|Momentopname van de basislijn van het besturingssysteem apparaat controleren.| 
|


## <a name="next-steps"></a>Volgende stappen

- Lees de ASC voor IoT [overzicht](overview.md)
- Meer informatie over ASC voor IoT [architectuur](architecture.md)
- Begrijpen en verkennen [ASC voor IoT-waarschuwingen](concept-security-alerts.md)
- Ontdek hoe u toegang tot uw [beveiligingsgegevens](how-to-security-data-access.md)
