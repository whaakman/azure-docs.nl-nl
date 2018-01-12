---
title: Knooppunt-eenheidacties schalen in Azure-Stack | Microsoft Docs
description: "Informatie over het knooppunt status te bekijken en gebruik van de kracht op uitschakelen leegmaken en knooppunt acties op een Azure-Stack geïntegreerd systeem hervat."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: dbb68b10-c721-4188-aa07-584d0cd63138
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: mabrigg
ms.openlocfilehash: 55cc0eb3cc187d87e0d2ae96e2433cb9682ab370
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="scale-unit-node-actions-in-azure-stack"></a>Knooppunt-eenheidacties schalen in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerd systemen*

Dit artikel wordt beschreven hoe u kunt de status van een schaaleenheid en de bijbehorende knooppunten weergeven en het gebruik van de acties beschikbaar knooppunt. Knooppunt acties power op power zijn uitgeschakeld, leegmaakt, hervatten en herstellen. Meestal gebruikt u deze acties knooppunt tijdens veld vervanging van onderdelen of voor knooppunt herstelscenario's.

> [!Important]  
> Alle knooppunt acties beschreven in dit artikel moeten alleen doel één knooppunt tegelijk.


## <a name="view-the-status-of-a-scale-unit-and-its-nodes"></a>De status van een schaaleenheid en de knooppunten ervan bekijken

In de beheerdersportal kunt u eenvoudig de status van een schaaleenheid en de bijbehorende knooppunten weergeven.

De status van een schaaleenheid weergeven:

1. Op de **regio management** tegel, selecteer de regio.
2. Aan de linkerkant onder **infrastructuurresources**, selecteer **schaaleenheden**.
3. Selecteer in de resultaten van de schaaleenheid.
 
Hier kunt u de volgende informatie bekijken:

- Regionaam
- type systeem
- Totaal aantal logische kernen
- totale hoeveelheid geheugen
- de lijst met afzonderlijke knooppunten en hun status; een uitgevoerd of gestopt.

![Scale unit tegel met de status actief heeft voor elk knooppunt](media/azure-stack-node-actions/ScaleUnitStatus.PNG)

## <a name="view-information-about-a-scale-unit-node"></a>Informatie weergeven over een scale unit-knooppunt

Als u een afzonderlijk knooppunt selecteert, kunt u de volgende informatie bekijken:

- Regionaam
- server-model
- IP-adres van de BMC (baseboard management controller)
- operationele status
- Totaal aantal kernen
- totale hoeveelheid geheugen
 
![Scale unit tegel met de status actief heeft voor elk knooppunt](media/azure-stack-node-actions/NodeActions.PNG)

U kunt ook scale unit knooppunt acties uitvoeren vanaf hier.

## <a name="scale-unit-node-actions"></a>Scale-eenheidacties dat knooppunt

Wanneer u informatie over een scale unit-knooppunt bekijken, kunt u ook knooppunt bewerkingen uitvoeren, zoals:

- inschakelen en uitschakelen
- leegmaken en hervatten
- Herstellen

De operationele status van het knooppunt bepaalt welke opties beschikbaar zijn.

### <a name="power-off"></a>Uitschakelen

De **uitschakelen** actie schakelt het knooppunt. Dit is hetzelfde als wanneer u op de knop drukt. Dit gebeurt **niet** afsluiten signaal verzenden naar het besturingssysteem. Voor geplande uitschakelen bewerkingen, moet dat u eerst een scale unit knooppunt leegmaakt.

Deze actie wordt doorgaans gebruikt wanneer een knooppunt in een toepassing is vastgelopen en niet meer op aanvragen reageert.

> [!Important] 
> Deze functionaliteit is alleen beschikbaar via PowerShell. Deze zijn beschikbaar in de Azure-Stack-beheerdersportal opnieuw op een later tijdstip.


Het uitschakelen van de actie via PowerShell uitvoeren:

````PowerShell
  Stop-AzsScaleUnitNode -Region <RegionName> -Name <NodeName>
```` 

In het onwaarschijnlijke geval dat het uitschakelen van de actie niet werkt, gebruikt u de BMC webinterface.

### <a name="power-on"></a>Inschakelen

De **inschakelen** actie Hiermee schakelt u het knooppunt. Dit is hetzelfde als wanneer u op de knop drukt. 

> [!Important] 
> Deze functionaliteit is alleen beschikbaar via PowerShell. Deze zijn beschikbaar in de Azure-Stack-beheerdersportal opnieuw op een later tijdstip.

De macht op actie via PowerShell uitvoeren:

````PowerShell
  Start-AzsScaleUnitNode -Region <RegionName> -Name <NodeName>
````

In het onwaarschijnlijke geval dat de actie inschakelen niet werkt, gebruikt u de BMC webinterface.

### <a name="drain"></a>Leegmaken

De **leegmaakt** actie evacuates alle actieve werkbelastingen doordat ze tussen de resterende knooppunten in die bepaalde schaaleenheid.

Deze actie wordt meestal gebruikt tijdens de vervanging van veld van onderdelen, zoals de vervanging van een knooppunt als geheel.

> [!IMPORTANT]
> Zorg ervoor dat u een knooppunt leegmaken alleen tijdens een geplande onderhoudsvenster, waar gebruikers zijn aangemeld. Onder bepaalde omstandigheden kunnen actieve werkbelastingen onderbrekingen optreden.

De actie leegmaken via PowerShell uitvoeren:

  ````PowerShell
  Disable-AzsScaleUnitNode -Region <RegionName> -Name <NodeName>
  ````

### <a name="resume"></a>Hervatten

De **hervatten** actie een uitgedropen knooppunt hervat en markeert actief is voor plaatsing. Eerdere werkbelastingen die werden uitgevoerd op het knooppunt mislukken niet terug. (Als u een knooppunt en uitschakelen, wanneer u het knooppunt terug op rekenkracht leegmaakt, deze niet is gemarkeerd als actief is voor plaatsing. Wanneer u klaar bent, moet u de actie hervatten het knooppunt als actief gemarkeerd.)

De actie hervatten via PowerShell uitvoeren:

  ````PowerShell
  Enable-AzsScaleUnitNode -Region <RegionName> -Name <NodeName>
  ````

### <a name="repair"></a>Herstellen

De **reparatie** actie een knooppunt hersteld. Gebruik dit alleen voor een van de volgende scenario's:

- Volledige knooppunt vervanging (met of zonder nieuwe gegevensschijven)
- Na onderdeelfout hardware- en vervangingsdimensie (als op de hoogte in de documentatie van veld (FRU) replaceable unit).

> [!IMPORTANT]
> Zie de OEM-hardwareleverancier FRU-documentatie voor de exacte stappen wanneer u moet een knooppunt of afzonderlijke hardwareonderdelen vervangen. De documentatie FRU wordt aangegeven of u de herstelactie moet na het vervangen van een hardware-onderdeel uitvoeren.  

Wanneer u de herstelactie uitvoert, moet u het BMC IP-adres opgeven. 

De herstelactie via PowerShell uitvoeren:

  ````PowerShell
  Repair-AzsScaleUnitNode -Region <RegionName> -Name <NodeName> -BMCIPAddress <BMCIPAddress>
  ````


