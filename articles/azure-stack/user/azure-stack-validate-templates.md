---
title: Validatie van de sjabloon gebruiken om te controleren van sjablonen voor Azure-Stack | Microsoft Docs
description: Sjablonen voor de implementatie van Azure-Stack controleren
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: c30b0a78cf3421554cf8f7c887c7973c7b9f4b9c
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="check-your-templates-for-azure-stack-with-template-validator"></a>Controleer uw sjablonen voor Azure-Stack met sjabloon Validator

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt het validatieprogramma sjabloon gebruiken om te controleren of uw Azure Resource Manager [sjablonen](azure-stack-arm-templates.md) gereed zijn voor Azure-Stack. Het validatieprogramma sjabloon is beschikbaar als onderdeel van de Azure-Stack-hulpprogramma's. De Azure-Stack-hulpprogramma's te downloaden met behulp van de stappen in de [hulpprogramma's downloaden vanuit GitHub](azure-stack-powershell-download.md) artikel. 

Voor het valideren van sjablonen die u gebruikt de volgende PowerShell-modules en het JSON-bestand vinden in **TemplateValidator** en **CloudCapabilities** mappen: 

 - AzureRM.CloudCapabilities.psm1 maakt een cloud mogelijkheden JSON-bestand dat de services en -versies in een cloud zoals Azure Stack vertegenwoordigt.
 - AzureRM.TemplateValidator.psm1 maakt gebruik van een JSON-bestand van de cloud-mogelijkheden voor het testen van sjablonen voor implementatie in Azure-Stack.
 - AzureStackCloudCapabilities_with_AddOns_20170627.json is een standaardbestand voor de cloud-mogelijkheden.  U kunt uw eigen maken of dit bestand gebruiken om te beginnen. 

Validatie uitvoeren op uw sjablonen in dit onderwerp en eventueel een bestand met capabilities cloud te bouwen.

## <a name="validate-templates"></a>Valideren van sjablonen
In deze stappen kunt u sjablonen valideren met behulp van de AzureRM.TemplateValidator PowerShell-module. U kunt uw eigen sjablonen gebruiken of valideren van de [Azure Stack-snelstartsjablonen](https://github.com/Azure/AzureStack-QuickStart-Templates).

1.  Importeer de module AzureRM.TemplateValidator.psm1 PowerShell:
    
    ```PowerShell
    cd "c:\AzureStack-Tools-master\TemplateValidator"
    Import-Module .\AzureRM.TemplateValidator.psm1
    ```

2.  Voer de validatie van de sjabloon:

    ```PowerShell
    Test-AzureRMTemplate -TemplatePath <path to template.json or template folder> `
    -CapabilitiesPath <path to cloudcapabilities.json> `
    -Verbose
    ```

Sjabloon Validatiewaarschuwingen of fouten worden geregistreerd in de PowerShell-console en wordt ook vastgelegd in een HTML-bestand in de bronmap. Een voorbeeld van het rapport validatie uitvoer ziet er als volgt:

![validatie van voorbeeldrapport](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parameters

| Parameter | Beschrijving | Vereist |
| ----- | -----| ----- |
| Sjabloonpad | Hiermee geeft u het pad naar recursief zoeken naar Resource Manager-sjablonen | Ja | 
| TemplatePattern | Hiermee geeft u de naam van de sjabloonbestanden moeten voldoen. | Nee |
| CapabilitiesPath | Hiermee geeft u het pad naar de cloud mogelijkheden JSON-bestand | Ja | 
| IncludeComputeCapabilities | Bevat de evaluatie van IaaS-bronnen zoals VM-grootten en VM-extensies | Nee |
| IncludeStorageCapabilities | Evaluatie van opslagbronnen zoals SKU typen bevat | Nee |
| Rapport | Hiermee geeft u de naam van de gegenereerde HTML-rapport | Nee |
| Uitgebreid | Registreert fouten en waarschuwingen in de console | Nee|


### <a name="examples"></a>Voorbeelden
In dit voorbeeld alle valideert de [Azure Stack-snelstartsjablonen](https://github.com/Azure/AzureStack-QuickStart-Templates) lokaal gedownload, en ook valideert de VM-grootten en -extensies op basis van de mogelijkheden van Azure Stack Development Kit.

```PowerShell
test-AzureRMTemplate -TemplatePath C:\AzureStack-Quickstart-Templates `
-CapabilitiesPath .\TemplateValidator\AzureStackCloudCapabilities_with_AddOns_20170627.json.json `
-TemplatePattern MyStandardTemplateName.json`
-IncludeComputeCapabilities`
-Report TemplateReport.html
```

## <a name="build-cloud-capabilities-file"></a>Bestand met capabilities cloud bouwen
De gedownloade bestanden bevatten standaard *AzureStackCloudCapabilities_with_AddOns_20170627.json* bestand, dat de Serviceversies die beschikbaar zijn in Azure Stack Development Kit PaaS-Services is geïnstalleerd beschrijft.  Als u extra Resource Providers installeert, kunt u de AzureRM.CloudCapabilities PowerShell-module voor het bouwen van een JSON-bestand met inbegrip van de nieuwe services.  

1.  Zorg ervoor dat u verbinding hebt met Azure-Stack.  Deze stappen kunnen worden uitgevoerd vanaf de Azure-Stack development kit host, of kunt u [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) vanaf uw werkstation verbinding maken. 
2.  Importeer de module AzureRM.CloudCapabilities PowerShell:

    ```PowerShell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ``` 

3.  Gebruik de cmdlet Get-CloudCapabilities op te halen-versies en maak een JSON-bestand van cloud mogelijkheden:

    ```PowerShell
    Get-AzureRMCloudCapability -Location 'local' -Verbose
    ```             


## <a name="next-steps"></a>Volgende stappen
 - [Sjablonen implementeren naar Azure-Stack](azure-stack-arm-templates.md)
 - [Sjablonen voor Azure-Stack ontwikkelen](azure-stack-develop-templates.md)

