---
title: Een sjabloon validatie-hulpprogramma gebruiken om te controleren van sjablonen voor Azure-Stack | Microsoft Docs
description: Sjablonen voor de implementatie van Azure-Stack controleren
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 88fac41ce2c9fa0c5569beae02ab90a507c89a34
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="check-your-templates-for-azure-stack-with-the-template-validation-tool"></a>Controleer uw sjablonen voor Azure-Stack met het hulpprogramma voor validatie van sjabloon

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt het validatieprogramma sjabloon gebruiken om te controleren of uw Azure Resource Manager [sjablonen](azure-stack-arm-templates.md) gereed zijn voor het implementeren van Azure-Stack. Het validatieprogramma sjabloon is beschikbaar als onderdeel van de Azure-Stack-hulpprogramma's. De Azure-Stack-hulpprogramma's te downloaden met behulp van de stappen in de [hulpprogramma's downloaden vanuit GitHub](azure-stack-powershell-download.md) artikel.

## <a name="overview"></a>Overzicht

Voor het valideren van een sjabloon die u hebt voor het bouwen van een cloud mogelijkheden eerste bestand en voer het validatiehulpprogramma. U gebruikt de volgende PowerShell-modules uit Azure Stack-hulpprogramma's:

- In de **TemplateValidator** map:<br>         AzureRM.CloudCapabilities.psm1 maakt een cloud mogelijkheden JSON-bestand die de services en -versies in de cloud van een Azure-Stack.
- In de **CloudCapabilities** map:<br>
AzureRM.TemplateValidator.psm1 maakt gebruik van een JSON-bestand van de cloud-mogelijkheden voor het testen van sjablonen voor implementatie in Azure-Stack.

## <a name="build-the-cloud-capabilities-file"></a>Het cloud-mogelijkheden-bestand maken

Voordat u de validatiefunctie sjabloon gebruikt, voert u de AzureRM.CloudCapabilities PowerShell-module voor het bouwen van een JSON-bestand.

>[!NOTE]
>Als u uw geïntegreerde systeem bijwerken of nieuwe services of virtuele-extensies toevoegen, moet u deze module opnieuw uitvoeren.

1. Zorg ervoor dat u verbinding hebt met Azure-Stack. Deze stappen kunnen worden uitgevoerd vanaf de Azure-Stack development kit host, of kunt u een [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) vanaf uw werkstation verbinding maken.
2. Importeer de module AzureRM.CloudCapabilities PowerShell:

    ```PowerShell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ```

3. Gebruik de cmdlet Get-CloudCapabilities op te halen-versies en maak een JSON-bestand van de cloud-mogelijkheden. Als u geen opgeeft **- OutputPath**, het bestand AzureCloudCapabilities.Json in de huidige map is gemaakt. Gebruik uw werkelijke locatie:

    ```PowerShell
    Get-AzureRMCloudCapability -Location <your location> -Verbose
    ```

## <a name="validate-templates"></a>Valideren van sjablonen

Volg deze stappen voor het valideren van sjablonen met behulp van de AzureRM.TemplateValidator PowerShell-module. U kunt uw eigen sjablonen gebruiken of valideren van de [Azure Stack-snelstartsjablonen](https://github.com/Azure/AzureStack-QuickStart-Templates).

1. Importeer de module AzureRM.TemplateValidator.psm1 PowerShell:

    ```PowerShell
    cd "c:\AzureStack-Tools-master\TemplateValidator"
    Import-Module .\AzureRM.TemplateValidator.psm1
    ```

2. Voer de validatie van de sjabloon:

    ```PowerShell
    Test-AzureRMTemplate -TemplatePath <path to template.json or template folder> `
    -CapabilitiesPath <path to cloudcapabilities.json> `
    -Verbose
    ```

Sjabloon Validatiewaarschuwingen of fouten worden geregistreerd in de PowerShell-console en een HTML-bestand in de bronmap. De volgende Schermafbeelding toont een voorbeeld van een validatierapport:

![Validatierapport van sjabloon](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parameters

De validatiefunctie sjabloon ondersteunt de volgende parameters.

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

In dit voorbeeld alle valideert de [Azure Stack-snelstartsjablonen](https://github.com/Azure/AzureStack-QuickStart-Templates) gedownload naar de lokale opslag. In het voorbeeld valideert ook grootten van virtuele machines en -extensies op basis van de mogelijkheden van Azure Stack Development Kit.

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
