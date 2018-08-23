---
title: Knooppunt-eenheidacties schalen in Azure Stack | Microsoft Docs
description: Informatie over het knooppuntstatus bekijken en gebruik de kracht van uitschakelen, drain en hervatten knooppunt acties op een geïntegreerde Azure Stack-systeem.
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
ms.date: 08/14/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 1f59f2ce6e3bf8d34ce225aa93da76ad523775e0
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2018
ms.locfileid: "42054044"
---
# <a name="scale-unit-node-actions-in-azure-stack"></a>Knooppunt-eenheidacties schalen in Azure Stack

*Is van toepassing op: Azure Stack-geïntegreerde systemen*

Dit artikel wordt beschreven hoe u om de status van een schaaleenheid en de bijbehorende knooppunten weer te geven en het gebruik van de acties beschikbaar knooppunt. Knooppunt acties opnemen power op power uitschakelen, leegmaken, hervatten en herstellen. Meestal gebruikt u deze acties knooppunt tijdens veld vervanging van delen, of voor knooppunt herstelscenario's.

> [!Important]  
> Alle knooppunt-acties die worden beschreven in dit artikel moeten alleen doel één knooppunt tegelijk.


## <a name="view-the-status-of-a-scale-unit-and-its-nodes"></a>De status van een schaaleenheid en de knooppunten ervan weergeven

In de beheerdersportal voor de, kunt u eenvoudig de status van een schaaleenheid en de bijbehorende knooppunten weergeven.

De status van een schaaleenheid weergeven:

1. Op de **regiobeheer** tegel, selecteert u de regio.
2. Aan de linkerkant, onder **beschikken over infrastructurele resources**, selecteer **schaaleenheden**.
3. Selecteer in de resultaten de schaaleenheid.
 
Hier vindt u de volgende informatie:

- naam van de regio. De regionaam van de waarnaar wordt verwezen met **-locatie** in de PowerShell-module.
- type van systeem
- Totaal aantal logische kerngeheugens
- Totaal geheugen
- de lijst met afzonderlijke knooppunten en hun status; een van beide **met** of **gestopt**.

![Schaal eenheid tegel weer met de status actief heeft voor elk knooppunt](media/azure-stack-node-actions/ScaleUnitStatus.PNG)

## <a name="view-information-about-a-scale-unit-node"></a>Informatie weergeven over een scale unit-knooppunt

Als u een afzonderlijke knooppunt selecteert, kunt u de volgende informatie bekijken:

- regionaam
- server-model
- IP-adres van de BMC (baseboard management controller)
- operationele status
- Totale aantal kernen
- totale hoeveelheid geheugen
 
![Schaal eenheid tegel weer met de status actief heeft voor elk knooppunt](media/azure-stack-node-actions/NodeActions.PNG)

U kunt ook de schaal-eenheidacties dat knooppunt uitvoeren vanaf hier.

## <a name="scale-unit-node-actions"></a>Schaalacties eenheid knooppunt

Als u informatie over een scale unit-knooppunt bekijken, kunt u ook knooppunt acties uitvoeren zoals:

- Clusterbesturingssysteem en hervatten
- Herstellen

De operationele status van het knooppunt bepaalt welke opties beschikbaar zijn.

### <a name="power-off"></a>Uitschakelen

De **uitschakelen** actie schakelt het knooppunt. Het is hetzelfde als wanneer u op de knop drukt. Dit gebeurt **niet** een afsluitingssignaal verzenden naar het besturingssysteem. Voor geplande uitschakelen operations, moet dat u eerst een schaal eenheid knooppunt leegmaken.

Deze actie wordt doorgaans gebruikt wanneer een knooppunt vastgelopen is en niet meer op aanvragen reageert.

> [!Important] 
> Deze functionaliteit is alleen beschikbaar via PowerShell. Dit is beschikbaar in de portal van de beheerder van Azure Stack op een later tijdstip opnieuw uit.


De kracht uitschakelen via PowerShell uitvoeren:

````PowerShell
  Stop-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```` 

In het onwaarschijnlijke geval dat het uitschakelen van de actie niet werkt, gebruikt u de webinterface voor de BMC.

### <a name="power-on"></a>Inschakelen

De **inschakelen** actie Hiermee schakelt u het knooppunt. Het is hetzelfde als wanneer u op de knop drukt. 

> [!Important] 
> Deze functionaliteit is alleen beschikbaar via PowerShell. Dit is beschikbaar in de portal van de beheerder van Azure Stack op een later tijdstip opnieuw uit.

De kracht van actie via PowerShell uitvoeren:

````PowerShell
  Start-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
````

In het onwaarschijnlijke geval die de kracht van de actie niet werkt, gebruikt u de webinterface voor de BMC.

### <a name="drain"></a>Clusterbesturingssysteem

De **leegmaken** actie evacuates alle actieve werkbelastingen door ze te verdelen tussen de resterende knooppunten in die specifieke schaaleenheid.

Deze actie wordt doorgaans gebruikt tijdens het veld vervanging van onderdelen, zoals het vervangen van een heel knooppunt.

> [!IMPORTANT]  
> Zorg ervoor dat u een knooppunt alleen tijdens een geplande onderhoudsvenster leegmaken, waar gebruikers zijn aangemeld. Onder bepaalde omstandigheden kunnen actieve werklasten onderbrekingen optreden.

De actie drain via PowerShell uitvoeren:

  ````PowerShell
  Disable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
  ````

### <a name="resume"></a>Hervatten

De **hervatten** actie hervat een uitgedropen knooppunt en wordt er aangegeven actief is voor plaatsing van werkbelastingen. Eerdere workloads die op het knooppunt werden uitgevoerd mislukken niet terug. (Als u een knooppunt en uitschakelen, wanneer u het knooppunt weer rekenkracht leegmaken, deze niet is gemarkeerd als actief is voor plaatsing van werkbelastingen. Wanneer u klaar bent, moet u de actie doorgaan naar het knooppunt als actief gemarkeerd.)

Om uit te voeren met de actie hervatten via PowerShell:

  ````PowerShell
  Enable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
  ````

### <a name="repair"></a>Herstellen

De **herstellen** actie herstelt een knooppunt. Deze alleen gebruikt voor een van de volgende scenario's:

- Volledige knooppunt vervanging (met of zonder nieuwe gegevensschijven)
- Na onderdeelfouten hardware- en vervangingsdimensie (als op de hoogte in de documentatie van het veld (FRU) replaceable unit).

> [!IMPORTANT]  
> Raadpleeg uw OEM-hardwareleverancier FRU-documentatie voor de exacte stappen wanneer u nodig hebt om een knooppunt of afzonderlijke hardware-onderdelen te vervangen. De documentatie FRU wordt aangegeven of u de herstelactie moet na het vervangen van een hardware-onderdeel uitvoeren.  

Wanneer u de herstelactie uitvoert, moet u het BMC IP-adres opgeven. 

De herstelactie via PowerShell uitvoeren:

  ````PowerShell
  Repair-AzsScaleUnitNode -Location <RegionName> -Name <NodeName> -BMCIPAddress <BMCIPAddress>
  ````

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure Stack-infrastructuur beheerder-module, [Azs.Fabric.Admin](https://docs.microsoft.com/powershell/module/azs.fabric.admin/?view=azurestackps-1.4.0).