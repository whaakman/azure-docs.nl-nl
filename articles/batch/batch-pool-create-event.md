---
title: Azure Batch-pool maken gebeurtenis | Microsoft Docs
description: Naslaginformatie voor Batch-pool gebeurtenis maken.
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 67edaa55d7ccd00d4aebb309f11bcf95486e87fa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="pool-create-event"></a>Groep gebeurtenis maken

 Deze gebeurtenis wordt verzonden wanneer een groep is gemaakt. De inhoud van het logboek voor algemene informatie over de groep zal worden blootgesteld. Houd er rekening mee dat als de doelgrootte van de pool groter is dan 0 rekenknooppunten, een startgebeurtenis van toepassingen onmiddellijk na deze gebeurtenis wordt volgen.

 Het volgende voorbeeld ziet de hoofdtekst van een groep gebeurtenis gemaakt voor een groep gemaakt met behulp van de eigenschap CloudServiceConfiguration.

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
|id|Tekenreeks|De id van de groep.|
|Weergavenaam|Tekenreeks|De weergavenaam van de groep.|
|vmSize|Tekenreeks|De grootte van de virtuele machines in de groep. Alle virtuele machines in een pool zijn dezelfde grootte hebben. <br/><br/> Zie voor informatie over beschikbare grootten van virtuele machines voor Cloud Services pools (groepen gemaakt met cloudServiceConfiguration), [grootten voor Cloudservices](http://azure.microsoft.com/documentation/articles/cloud-services-sizes-specs/). Batch ondersteunt alle Cloud-Services VM-grootten, behalve `ExtraSmall`.<br/><br/> Zie voor informatie over beschikbare virtuele machine grootten voor groepen met behulp van installatiekopieën van virtuele Machines Marketplace (groepen die zijn gemaakt met virtualMachineConfiguration) [grootten voor virtuele Machines](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-sizes/) (Linux) of [grootten voor virtuele Machines](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) (Windows). Batch ondersteunt alle Azure VM-groottes met uitzondering van `STANDARD_A0` en die met Premium Storage (de serie `STANDARD_GS`, `STANDARD_DS` en `STANDARD_DSV2`).|
|[cloudServiceConfiguration](#bk_csconf)|Complex Type|De cloud-serviceconfiguratie voor de pool.|
|[virtualMachineConfiguration](#bk_vmconf)|Complex Type|De configuratie van de virtuele machine voor de groep.|
|[networkConfiguration](#bk_netconf)|Complex Type|De netwerkconfiguratie voor de pool.|
|resizeTimeout|Time|De time-out voor de toewijzing van rekenknooppunten aan de groep die is opgegeven voor de laatste bewerking formaat wijzigen op de groep.  (De eerste sizing tijdens het maken van de groep telt als een formaat te wijzigen.)|
|targetDedicated|Int32|Het aantal rekenknooppunten die zijn aangevraagd voor de groep.|
|enableAutoScale|BOOL|Hiermee geeft u op of de poolgrootte automatisch wordt aangepast gedurende een bepaalde periode.|
|enableInterNodeCommunication|BOOL|Hiermee geeft u op of de groep is ingesteld voor directe communicatie tussen knooppunten.|
|isAutoPool|BOOL|Speficies of de groep is gemaakt via een taak AutoPool mechanisme.|
|maxTasksPerNode|Int32|Het maximale aantal taken dat gelijktijdig kan worden uitgevoerd op één rekenknooppunt in de groep.|
|vmFillType|Tekenreeks|Hiermee wordt gedefinieerd hoe taken met de Batch-service tussen rekenknooppunten in de groep worden gedistribueerd. Geldige waarden worden verdeeld of Pack.|

###  <a name="bk_csconf"></a>cloudServiceConfiguration

|Elementnaam|Type|Opmerkingen|
|------------------|----------|-----------|
|besturingssysteemtype|Tekenreeks|De Azure Gast OS-familie worden geïnstalleerd op de virtuele machines in de groep.<br /><br /> Mogelijke waarden zijn:<br /><br /> **2** – OS-familie 2, die gelijkwaardig zijn aan Windows Server 2008 R2 SP1.<br /><br /> **3** – OS-familie 3, die gelijkwaardig zijn aan Windows Server 2012.<br /><br /> **4** – OS-familie 4, gelijk is aan Windows Server 2012 R2.<br /><br /> Zie voor meer informatie [Azure Gast OS Releases](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|
|targetOSVersion|Tekenreeks|De Azure Gast OS-versie moet worden geïnstalleerd op de virtuele machines in de groep.<br /><br /> De standaardwaarde is  **\***  die aangeeft dat de nieuwste versie van het besturingssysteem voor de opgegeven familie.<br /><br /> Zie voor andere toegestane waarden [Azure Gast OS Releases](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|

###  <a name="bk_vmconf"></a>virtualMachineConfiguration

|Elementnaam|Type|Opmerkingen|
|------------------|----------|-----------|
|[imageReference](#bk_imgref)|Complex Type|Hiermee geeft u informatie over het platform of de Marketplace-installatiekopie te gebruiken.|
|nodeAgentSKUId|Tekenreeks|De SKU van de Batch-knooppunt-agent is ingericht op het rekenknooppunt.|
|[windowsConfiguration](#bk_winconf)|Complex Type|Hiermee geeft u de instellingen voor Windows-besturingssysteem op de virtuele machine. Deze eigenschap mag niet zijn opgegeven als de imageReference verwijst naar een installatiekopie van het Linux-besturingssysteem.|

###  <a name="bk_imgref"></a>imageReference

|Elementnaam|Type|Opmerkingen|
|------------------|----------|-----------|
|Uitgever|Tekenreeks|De uitgever van de installatiekopie.|
|Aanbieding|Tekenreeks|De aanbieding van de afbeelding.|
|SKU|Tekenreeks|De SKU van de afbeelding.|
|Versie|Tekenreeks|De versie van de installatiekopie.|

###  <a name="bk_winconf"></a>windowsConfiguration

|Elementnaam|Type|Opmerkingen|
|------------------|----------|-----------|
|enableAutomaticUpdates|Booleaanse waarde|Hiermee wordt aangegeven of de virtuele machine is ingeschakeld voor automatische updates. Als deze eigenschap niet is opgegeven, is de standaardwaarde is true.|

###  <a name="bk_netconf"></a>networkConfiguration

|Elementnaam|Type|Opmerkingen|
|------------------|--------------|----------|
|subnetId|Tekenreeks|Hiermee geeft u de bron-id van het subnet waarin de rekenknooppunten van de pool worden gemaakt.|
