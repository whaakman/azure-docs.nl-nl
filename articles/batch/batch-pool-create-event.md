---
title: Azure Batch-pool gebeurtenis maken | Microsoft Docs
description: Naslaginformatie voor Batch-pool gebeurtenis maken.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: danlep
ms.openlocfilehash: f8c0adf96d027f58a35dbe570f1b19c311cd84b9
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51246511"
---
# <a name="pool-create-event"></a>Gebeurtenis pool maken

 Deze gebeurtenis wordt verzonden zodra een groep is gemaakt. De inhoud van het logboek wordt algemene informatie over de groep weergegeven. Houd er rekening mee dat als de doelgrootte van de groep groter is dan 0 rekenknooppunten, een gebeurtenis formaat pool wijzigen starten direct na deze gebeurtenis wordt volgen.

 Het volgende voorbeeld ziet de instantie van een groep met de gebeurtenis voor een groep gemaakt met behulp van de eigenschap CloudServiceConfiguration maken.

```
{
    "id": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "3",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoScale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

|Element|Type|Opmerkingen|
|-------------|----------|-----------|
|id|Reeks|De id van de groep.|
|displayName|Reeks|De weergavenaam van de pool.|
|vmSize|Reeks|De grootte van de virtuele machines in de groep. Alle virtuele machines in een pool zijn even groot is. <br/><br/> Zie voor meer informatie over de beschikbare grootten van virtuele machines voor Cloud Services pools (pools die zijn gemaakt met cloudServiceConfiguration), [groottes voor Cloud Services](https://azure.microsoft.com/documentation/articles/cloud-services-sizes-specs/). Batch ondersteunt alle Cloud Services VM-grootten met uitzondering van `ExtraSmall`.<br/><br/> Zie voor meer informatie over beschikbare VM-grootten voor groepen met installatiekopieën uit de Marketplace voor virtuele Machines (pools de zijn gemaakt met virtualMachineConfiguration) [grootten voor virtuele Machines](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-sizes/) (Linux) of [grootten voor virtuele Machines](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) (Windows). Batch ondersteunt alle Azure VM-groottes met uitzondering van `STANDARD_A0` en die met Premium Storage (de serie `STANDARD_GS`, `STANDARD_DS` en `STANDARD_DSV2`).|
|[cloudServiceConfiguration](#bk_csconf)|Complex Type|De cloud-serviceconfiguratie voor de pool.|
|[virtualMachineConfiguration](#bk_vmconf)|Complex Type|De configuratie van de virtuele machine voor de pool.|
|[networkConfiguration](#bk_netconf)|Complex Type|De netwerkconfiguratie voor de pool.|
|resizeTimeout|Time|De time-out voor de toewijzing van compute-knooppunten aan de groep die is opgegeven voor de laatste bewerking formaat wijzigen in de pool.  (De initiële grootte wanneer de groep wordt gemaakt telt als een formaat wijzigen.)|
|targetDedicated|Int32|Het aantal rekenknooppunten die zijn aangevraagd voor de pool.|
|enableAutoScale|BOOL|Hiermee geeft u op of de groepsgrootte die automatisch wordt aangepast na verloop van tijd.|
|enableInterNodeCommunication|BOOL|Hiermee geeft u op of de groep is ingesteld voor rechtstreekse communicatie tussen knooppunten.|
|isAutoPool|BOOL|Speficies of de groep is gemaakt via een job AutoPool mechanisme.|
|maxTasksPerNode|Int32|Het maximale aantal taken die mogen worden uitgevoerd op een enkel knooppunt in de pool.|
|vmFillType|Reeks|Hiermee wordt gedefinieerd hoe taken met de Batch-service tussen rekenknooppunten in de groep worden gedistribueerd. Geldige waarden worden verdeeld, of Pack.|

###  <a name="bk_csconf"></a> cloudServiceConfiguration

|De naam van element|Type|Opmerkingen|
|------------------|----------|-----------|
|besturingssysteemtype|Reeks|De Azure Guest OS family moet worden geïnstalleerd op de virtuele machines in de groep.<br /><br /> Mogelijke waarden zijn:<br /><br /> **2** – OS-familie 2, gelijk is aan Windows Server 2008 R2 SP1.<br /><br /> **3** – OS-familie 3, gelijk is aan Windows Server 2012.<br /><br /> **4** – OS Family 4, gelijk is aan Windows Server 2012 R2.<br /><br /> Zie voor meer informatie, [Azure Guest OS Releases](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|
|targetOSVersion|Reeks|De Azure Guest OS-versie moet worden geïnstalleerd op de virtuele machines in de groep.<br /><br /> De standaardwaarde is **\*** dat aangeeft dat de nieuwste versie van het besturingssysteem voor de opgegeven-familie.<br /><br /> Zie voor andere toegestane waarden [Azure Guest OS Releases](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|

###  <a name="bk_vmconf"></a> virtualMachineConfiguration

|De naam van element|Type|Opmerkingen|
|------------------|----------|-----------|
|[imageReference](#bk_imgref)|Complex Type|Hiermee geeft u informatie over het platform of Marketplace-installatiekopie moet worden gebruikt.|
|nodeAgentSKUId|Reeks|De SKU van de Batch-knooppuntagent ingericht op het rekenknooppunt.|
|[windowsConfiguration](#bk_winconf)|Complex Type|Hiermee geeft u instellingen voor Windows-besturingssysteem op de virtuele machine. Deze eigenschap niet moet worden opgegeven als de imageReference verwijst naar een installatiekopie van het Linux-besturingssysteem.|

###  <a name="bk_imgref"></a> imageReference

|De naam van element|Type|Opmerkingen|
|------------------|----------|-----------|
|Uitgever|Reeks|De uitgever van de installatiekopie.|
|aanbieding|Reeks|De aanbieding van de installatiekopie.|
|sku|Reeks|De SKU van de installatiekopie.|
|versie|Reeks|De versie van de installatiekopie.|

###  <a name="bk_winconf"></a> windowsConfiguration

|De naam van element|Type|Opmerkingen|
|------------------|----------|-----------|
|enableAutomaticUpdates|Booleaans|Geeft aan of de virtuele machine is ingeschakeld voor automatische updates. Als deze eigenschap niet opgegeven is, is de standaardwaarde is true.|

###  <a name="bk_netconf"></a> Primary

|De naam van element|Type|Opmerkingen|
|------------------|--------------|----------|
|subnetId|Reeks|Hiermee geeft u de resource-id van het subnet waarin de rekenknooppunten van de pool worden gemaakt.|
