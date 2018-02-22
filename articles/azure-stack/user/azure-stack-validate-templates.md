---
title: Validatie van de sjabloon gebruiken om te controleren van sjablonen voor Azure-Stack | Microsoft Docs
description: Sjablonen voor de implementatie van Azure-Stack controleren
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 6a77efb3ef4236048ff08b14346175b592493982
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="check-your-templates-for-azure-stack-with-template-validator"></a>Controleer uw sjablonen voor Azure-Stack met sjabloon Validator

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt het validatieprogramma sjabloon gebruiken om te controleren of uw Azure Resource Manager [sjablonen](azure-stack-arm-templates.md) gereed zijn voor Azure-Stack. Het validatieprogramma sjabloon is beschikbaar als onderdeel van de Azure-Stack-hulpprogramma's. De Azure-Stack-hulpprogramma's te downloaden met behulp van de stappen in de [hulpprogramma's downloaden vanuit GitHub](azure-stack-powershell-download.md) artikel. 

Voor het valideren van sjablonen die u gebruikt de volgende PowerShell-modules in **TemplateValidator** en **CloudCapabilities** mappen: 

 - AzureRM.CloudCapabilities.psm1 maakt een cloud mogelijkheden JSON-bestand dat de services en -versies in een cloud zoals Azure Stack vertegenwoordigt.
 - AzureRM.TemplateValidator.psm1 maakt gebruik van een JSON-bestand van de cloud-mogelijkheden voor het testen van sjablonen voor implementatie in Azure-Stack.
 
In dit artikel wordt een bestand met capabilities cloud bouwen en voer het hulpprogramma validator.

## <a name="build-cloud-capabilities-file"></a>Bestand met capabilities cloud bouwen
Voordat u de validatiefunctie sjabloon gebruikt, voert u de AzureRM.CloudCapabilities PowerShell-module voor het bouwen van een JSON-bestand. Als u uw geïntegreerde systeem bijwerken, of Voeg nieuwe services of VM-extensies moet u die module ook opnieuw uitvoeren.

1.  Zorg ervoor dat u verbinding hebt met Azure-Stack. Deze stappen kunnen worden uitgevoerd vanaf de Azure-Stack development kit host, of kunt u een [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) vanaf uw werkstation verbinding maken. 
2.  Importeer de module AzureRM.CloudCapabilities PowerShell:

    ```PowerShell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ``` 

3.  Gebruik de cmdlet Get-CloudCapabilities op te halen-versies en maak een JSON-bestand van de cloud-mogelijkheden. Als u geen - OutputPath opgeeft, het bestand AzureCloudCapabilities.Json is gemaakt in de huidige map. Gebruik uw werkelijke locatie:

    ```PowerShell
    Get-AzureRMCloudCapability -Location <your location> -Verbose
    ```             

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

Sjabloon Validatiewaarschuwingen of fouten worden geregistreerd in de PowerShell-console en een HTML-bestand in de bronmap. Hier volgt een voorbeeld van het validatierapport:

![validatie van voorbeeldrapport](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parameters

| Parameter | Beschrijving | Vereist |
| ----- | -----| ----- |
| Sjabloonpad | Hiermee geeft u het pad naar recursief zoeken naar Azure Resource Manager-sjablonen | Ja | 
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


## <a name="next-steps"></a>Volgende stappen
 - [Sjablonen implementeren naar Azure-Stack](azure-stack-arm-templates.md)
 - [Sjablonen voor Azure-Stack ontwikkelen](azure-stack-develop-templates.md)

