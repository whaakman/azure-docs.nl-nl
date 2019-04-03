---
title: Azure Security Center te configureren voor de Preview-versie van de IoT-agent | Microsoft Docs
description: Informatie over het configureren van agents voor gebruik met Azure Security Center voor IoT.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: f95c445a-4f0d-4198-9c6c-d01446473bd0
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 1e212e752309fd8347836d462a3394da2fca4a15
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862774"
---
# <a name="tutorial-configure-security-agents"></a>Zelfstudie: Beveiligingsagents configureren

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Dit artikel wordt uitgelegd van Azure Security Center (ASC) voor IoT security-agent, hoe wijzigen ze ASC voor agents van IoT-beveiliging configureren.

> [!div class="checklist"]
> * Beveiligingsagents configureren
> * Agent-gedrag wijzigen door dubbeleigenschappen te bewerken
> * Standaardconfiguratie detecteren

## <a name="agents"></a>Agents

ASC voor IoT security agents gegevens verzamelen van IoT-apparaten en beveiliging te werk om te beperken van de gedetecteerde kwetsbaarheden. Beveiligingsconfiguratie van de agent worden beheerd met behulp van een set van module dubbeleigenschappen die u kunt aanpassen. Secundaire updates voor deze eigenschappen zijn in het algemeen, incidentele.  

ASC voor dubbele configuratieobject voor de IoT security agent is een JSON-indeling-object. Het configuratieobject is een set te beheren zijn eigenschappen die u definiëren kunt om te bepalen het gedrag van de agent. 

Deze configuraties kunnen u de agent voor elk scenario vereist aanpassen. Automatisch zijn met uitzondering van bepaalde gebeurtenissen of het energieverbruik voor een minimumniveau behouden bijvoorbeeld mogelijk door het configureren van deze eigenschappen.  

De ASC gebruiken voor de configuratie van IoT security agent [schema](https://aka.ms/iot-security-github-module-schema) wijzigingen aanbrengen.  

## <a name="configuration-objects"></a>Configuratie-objecten 

Elke ASC voor IoT security-agent die betrekking hebben de eigenschap bevindt zich in de agent configuration-object in de sectie gewenste eigenschappen van de **azureiotsecurity** module. 

Voor het wijzigen van de configuratie maken en wijzigen van dit object binnen de **azureiotsecurity** module dubbele identiteit. 

Als de agent configuration-object bestaat niet in de **azureiotsecurity** moduledubbel, alle eigenschapswaarden van de security-agent zijn ingesteld op standaard. 

```json
"desired": {
  "azureiot*com^securityAgentConfiguration^1*0*0": {
  } 
}
```

Zorg ervoor dat u uw wijzigingen in de configuratie agent tegen deze valideren [schema](https://aka.ms/iot-security-github-module-schema).
De agent wordt niet gestart als het configuratieobject komt niet overeen met het schema.

## <a name="configuration-schema-and-validation"></a>Het schema en validatie 

Zorg ervoor dat u de configuratie van de agent op basis van dit [schema](https://aka.ms/iot-security-github-module-schema). Een agent wordt niet gestart als het configuratieobject komt niet overeen met het schema.

 
Als, terwijl de agent wordt uitgevoerd, wordt het configuratieobject gewijzigd naar een ongeldige configuratie (de configuratie komt niet overeen met het schema), wordt de agent de ongeldige configuratie worden genegeerd en wordt voortgezet met behulp van de huidige configuratie. 

## <a name="editing-a-property"></a>Een eigenschap bewerken 

Alle aangepaste eigenschappen moeten worden ingesteld in het object voor de configuratie van agent in de **azureiotsecurity** moduledubbel.
Voor het gebruik van de waarde van een standaard-eigenschap, de eigenschap te verwijderen uit de configuration-object.

### <a name="setting-a-property"></a>Het instellen van een eigenschap

1. Zoek in uw IoT-Hub, en selecteer het apparaat dat u wilt wijzigen.

1. Klik op het apparaat en klik vervolgens op **azureiotsecurity** module.

1. Klik op **identiteit Moduledubbel**.

1. De gewenste eigenschappen van de beveiligingsmodule bewerken.
   
   Bijvoorbeeld, voor connection-gebeurtenissen configureren als hoge prioriteit en verzamelen van gebeurtenissen met hoge prioriteit om 7 minuten, gebruik de volgende configuratie.
   
   ```json
    "desired": {
      "azureiot*com^securityAgentConfiguration^1*0*0": {
        "highPriorityMessageFrequency": "PT7M",    
        "eventPriorityConnectionCreate": "High" 
      } 
    }, 
    ```

1. Klik op **Opslaan**.

### <a name="using-a-default-value"></a>Met behulp van een standaardwaarde

Voor het gebruik van de waarde van een standaard-eigenschap, de eigenschap te verwijderen uit de configuration-object.

## <a name="default-properties"></a>Standaard-eigenschappen 

De volgende tabel bevat de te beheren zijn eigenschappen van ASC voor IoT-beveiliging-agents.

Standaardwaarden zijn beschikbaar in het juiste schema in [Github](https://aka.ms/iot-security-module-default).

| Name| Status | Geldige waarden| Standaardwaarden| Description |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|highPriorityMessageFrequency|Vereist: false |Geldige waarden: De duur in ISO 8601-indeling |Standaardwaarde: PT7M |Maximale tijd vóór berichten met hoge prioriteit worden verzonden.|
|lowPriorityMessageFrequency |Vereist: false|Geldige waarden: De duur in ISO 8601-indeling |Standaardwaarde: PT5H |Maximale tijd vóór berichten met een lage prioriteit worden verzonden.| 
|snapshotFrequency |Vereist: false|Geldige waarden: duur in ISO 8601-notatie |Standaardwaarde PT13H |Het tijdsinterval voor het maken van momentopnamen van de status van apparaat.| 
|maxLocalCacheSizeInBytes |Vereist: false |Geldige waarden: |Standaardwaarde: 2560000, groter is dan 8192 liggen | Maximale opslag (in bytes) toegestaan voor het cachegeheugen voor statusberichten van een agent. Maximale hoeveelheid ruimte die zijn toegestaan voor het opslaan van berichten op het apparaat voordat berichten worden verzonden.| 
|maxMessageSizeInBytes |Vereist: false |Geldige waarden: Een positief getal, groter is dan 8192, minder dan 262144 |Standaardwaarde: 204800 |Maximale toegestane grootte van een agent op cloudbericht. Deze instelling bepaalt de hoeveelheid gegevens in elk bericht dat wordt verzonden. |
|eventPriority${EventName} |Vereist: false |Geldige waarden: Zeer laag, hoog uitschakelen |Standaardwaarden: |Prioriteit van elke agent gebeurtenis is gegenereerd | 

### <a name="supported-security-events"></a>Ondersteunde beveiligingsgebeurtenissen

|Gebeurtenisnaam| propertyName | Standaardwaarde| Momentopname van gebeurtenis| Details van Status  |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|Diagnostische gebeurtenissen|eventPriorityDiagnostic| Uit| False| Agent gerelateerde diagnostische gebeurtenissen. Gebruik deze gebeurtenis voor uitgebreide logboekregistratie.| 
|Configuratiefout |eventPriorityConfigurationError |Laag |False |Parseren van de configuratie van de agent is mislukt. Controleer de configuratie op basis van het schema.| 
|Verwijderde gebeurtenissen statistieken |eventPriorityDroppedEventsStatistics |Laag |True|Agent de bijbehorende gebeurtenis statistische gegevens. |
|Statistieken van berichten|eventPriorityMessageStatistics |Laag |True |Agent gerelateerde statistieken van berichten. |
|Verbonden hardware|eventPriorityConnectedHardware |Laag |True |Momentopname van alle hardware die is verbonden met het apparaat.|
|Luisterende poorten|eventPriorityListeningPorts |Hoog |True |Momentopname van alle geopende luisterende poorten op het apparaat.|
|Proces maken |eventPriorityProcessCreate |Laag |False |Controles verwerken maken op het apparaat.|
|Proces beëindigen|eventPriorityProcessTerminate |Laag |False |Controles verwerken beëindiging op het apparaat.| 
|Systeemgegevens |eventPrioritySystemInformation |Laag |True |Een momentopname van systeemgegevens (bijvoorbeeld: Besturingssysteem of CPU).| 
|Lokale gebruikers| eventPriorityLocalUsers |Hoog |True|Een momentopname van de geregistreerde lokale gebruikers binnen het systeem. |
|Aanmelden|  eventPriorityLogin |Hoog|False|De aanmelding controlegebeurtenissen op het apparaat (lokale en externe aanmeldingen).|
|Verbinding maken |eventPriorityConnectionCreate|Laag|False|Voert een controle uit TCP-verbindingen die zijn gemaakt en naar het apparaat. |
|Firewall-configuratie| eventPriorityFirewallConfiguration|Laag|True|Momentopname van de firewall apparaatconfiguratie (firewall-regels). |
|Basislijn van het besturingssysteem| eventPriorityOSBaseline| Laag|True|Momentopname van de basislijn van het besturingssysteem apparaat controleren.|
 

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over ASC voor IoT-aanbevelingen](concept-recommendations.md)
- [Ontdek de ASC voor IoT-waarschuwingen](concept-security-alerts.md)
- [Toegang tot onbewerkte beveiligingsgegevens](how-to-security-data-access.md)
