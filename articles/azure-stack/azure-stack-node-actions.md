---
title: Knooppunt-eenheidacties schalen in Azure-Stack | Microsoft Docs
description: Informatie over het knooppunt status te bekijken en gebruik van de kracht op uitschakelen leegmaken en knooppunt acties op een Azure-Stack geïntegreerd systeem hervat.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 3ecc8885a30a11472fe93bbda60c39131c6b3bd7
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801412"
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

- Regionaam. De regionaam van de waarnaar wordt verwezen met **-locatie** in de PowerShell-module.
- type systeem
- Totaal aantal logische kernen
- totale hoeveelheid geheugen
- de lijst met afzonderlijke knooppunten en hun status; beide **met** of **gestopt**.

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
  Stop-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```` 

In het onwaarschijnlijke geval dat het uitschakelen van de actie niet werkt, gebruikt u de BMC webinterface.

### <a name="power-on"></a>Inschakelen

De **inschakelen** actie Hiermee schakelt u het knooppunt. Dit is hetzelfde als wanneer u op de knop drukt. 

> [!Important] 
> Deze functionaliteit is alleen beschikbaar via PowerShell. Deze zijn beschikbaar in de Azure-Stack-beheerdersportal opnieuw op een later tijdstip.

De macht op actie via PowerShell uitvoeren:

````PowerShell
  Start-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
````

In het onwaarschijnlijke geval dat de actie inschakelen niet werkt, gebruikt u de BMC webinterface.

### <a name="drain"></a>Leegmaken

De **leegmaakt** actie evacuates alle actieve werkbelastingen doordat ze tussen de resterende knooppunten in die bepaalde schaaleenheid.

Deze actie wordt meestal gebruikt tijdens de vervanging van veld van onderdelen, zoals de vervanging van een knooppunt als geheel.

> [!IMPORTANT]  
> Zorg ervoor dat u een knooppunt leegmaken alleen tijdens een geplande onderhoudsvenster, waar gebruikers zijn aangemeld. Onder bepaalde omstandigheden kunnen actieve werkbelastingen onderbrekingen optreden.

De actie leegmaken via PowerShell uitvoeren:

  ````PowerShell
  Disable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
  ````

### <a name="resume"></a>Hervatten

De **hervatten** actie een uitgedropen knooppunt hervat en markeert actief is voor plaatsing. Eerdere werkbelastingen die werden uitgevoerd op het knooppunt mislukken niet terug. (Als u een knooppunt en uitschakelen, wanneer u het knooppunt terug op rekenkracht leegmaakt, deze niet is gemarkeerd als actief is voor plaatsing. Wanneer u klaar bent, moet u de actie hervatten het knooppunt als actief gemarkeerd.)

De actie hervatten via PowerShell uitvoeren:

  ````PowerShell
  Enable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
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
  Repair-AzsScaleUnitNode -Location <RegionName> -Name <NodeName> -BMCIPAddress <BMCIPAddress>
  ````

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure-Stack Fabric-beheermodule, [Azs.Fabric.Admin](https://docs.microsoft.com/powershell/module/azs.fabric.admin/?view=azurestackps-1.3.0).