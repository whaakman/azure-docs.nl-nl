---
title: Configureren van een ASC voor IoT-agent Preview | Microsoft Docs
description: Informatie over het configureren van agents voor gebruik met ASC voor IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: f95c445a-4f0d-4198-9c6c-d01446473bd0
ms.service: ascforiot
ms.devlang: na
ms.topic: concept
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 3a3806c73ef254abab91bd28cd8761917371235f
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541915"
---
# <a name="configure-security-agents"></a>Beveiliging-agents configureren

> [!IMPORTANT]
> ASC voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

In dit artikel bevat een uitleg over het configureren van een agent voor gebruik met ASC voor IoT.

## <a name="agents"></a>Agents

ASC voor IoT security agents gegevens verzamelen van IoT-apparaten en beveiliging te werk om te beperken van de gedetecteerde kwetsbaarheden. Beveiligingsconfiguratie van de agent worden beheerd met behulp van een set van module dubbeleigenschappen die u kunt aanpassen. Secundaire updates voor deze eigenschappen zijn in het algemeen, incidentele.  

ASC voor dubbele configuratieobject voor de IoT security agent is een JSON-indeling-object. Het configuratieobject is een set te beheren zijn eigenschappen die u definiëren kunt om te bepalen het gedrag van de agent. 

Deze configuraties kunnen u de agent voor elk scenario vereist aanpassen. Automatisch zijn met uitzondering van bepaalde gebeurtenissen of het energieverbruik voor een minimumniveau behouden bijvoorbeeld mogelijk door het configureren van deze eigenschappen.  

De ASC gebruiken voor de configuratie van IoT security agent [schema](https://github.com/azure/asc-for-iot-schemas/security/module/twin) wijzigingen aanbrengen.  

## <a name="configuration-objects"></a>Configuratie-objecten 

Elke ASC voor IoT security-agent die betrekking hebben de eigenschap bevindt zich in de agent configuration-object in de sectie gewenste eigenschappen van de module azureiotsecurity. 

Voor het wijzigen van de configuratie maken en wijzigen van dit object binnen de azureiotsecurity module dubbele identiteit. Als de agent configuration-object niet in de moduledubbel azureiotsecurity bestaat, worden alle eigenschapswaarden van de security-agent zijn ingesteld op standaard. 

```json
"desired": { //azureiotsecurity Module Identity Twin – desired properties section  
  "azureiot*com^securityAgentConfiguration^1*0*0": { //Agent configuration object 
  … 
} 
}
```

Zorg ervoor dat u uw wijzigingen in de configuratie agent tegen deze valideren [schema](https://aka.ms/iot-security-github-module-schema).
De agent wordt niet gestart als het configuratieobject komt niet overeen met het schema.


## <a name="editing-a-property"></a>Een eigenschap bewerken 

Alle aangepaste eigenschappen moeten worden ingesteld in het object voor de configuratie van agent binnen de moduledubbel azureiotsecurity. 

Als u een eigenschap instelt, overschrijft de standaardwaarde. Een eigenschap wilt instellen, toevoegen de sleutel voor het configuratieobject met de gewenste waarde. 

Voor het gebruik van de waarde van een standaard-eigenschap, de eigenschap te verwijderen uit de configuration-object. 

```json
"desired": { //azureiotsecurity Module Identity Twin – desired properties section  
  "azureiot*com^securityAgentConfiguration^1*0*0": { //ASC for IoT Agent 
      // configuration section  
    "lowPriorityMessageFrequency": "PT1H",     
    "highPriorityMessageFrequency": "PT7M",    
    "eventPriorityFirewallConfiguration": "High",     
    "eventPriorityConnectionCreate": "High" 
  } 
}, 
```

## <a name="default-properties"></a>Standaard-eigenschappen 
Set te beheren zijn eigenschappen die regelen van de ASC voor IoT-beveiliging-agents.

Standaardwaarden zijn beschikbaar in het juiste schema in [Github](https://aka.ms/iot-security-module-default).

| Name| Status | Geldige waarden| Standaardwaarden| Description |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|---------------|
|highPriorityMessageFrequency|Vereist: false |Geldige waarden: De duur in ISO 8601-indeling |Standaardwaarde: PT7M |Maximale tijd vóór berichten met hoge prioriteit worden verzonden.|
|lowPriorityMessageFrequency |Vereist: false|Geldige waarden: De duur in ISO 8601-indeling |Standaardwaarde: PT5H |Maximale tijd vóór berichten met een lage prioriteit worden verzonden.| 
|snapshotFrequency |Vereist: false|Geldige waarden: duur in ISO 8601-notatie |Standaardwaarde PT13H |Het tijdsinterval voor het maken van momentopnamen van de status van apparaat.| 
|maxLocalCacheSizeInBytes |Vereist: false |Geldige waarden: |Standaardwaarde: 2560000, groter is dan 8192 liggen | Maximale opslag (in bytes) toegestaan voor het cachegeheugen voor statusberichten van een agent. Maximale hoeveelheid ruimte die zijn toegestaan voor het opslaan van berichten op het apparaat voordat berichten worden verzonden.| 
|maxMessageSizeInBytes |Vereist: false |Geldige waarden: Een positief getal, groter is dan 8192, minder dan 262144 |Standaardwaarde: 204800 |Maximale toegestane grootte van een agent op cloudbericht. Deze instelling bepaalt de hoeveelheid gegevens in elk bericht dat wordt verzonden. |
|eventPriority${EventName} |Vereist: false |Geldige waarden: Zeer laag, hoog uitschakelen |Standaardwaarden: |Prioriteit van elke agent gebeurtenis is gegenereerd | 

### <a name="events"></a>Gebeurtenissen

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
 

## <a name="see-also"></a>Zie ook

- [Meer informatie over ASC voor IoT-aanbevelingen](concept-recommendations.md)
- [Ontdek de ASC voor IoT-waarschuwingen](concept-security-alerts.md)
- [Toegang tot onbewerkte security gegevens](how-to-security-data-access.md)