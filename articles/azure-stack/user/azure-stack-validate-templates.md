---
title: Hulpprogramma voor het valideren van een sjabloon gebruiken om te controleren van sjablonen voor Azure Stack | Microsoft Docs
description: Sjablonen voor implementatie naar Azure Stack controleren
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/27/2018
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: 02ceb6cbcbf824f8bf830c66bc9899c20f6ed822
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484038"
---
# <a name="check-your-templates-for-azure-stack-with-the-template-validation-tool"></a>Neem contact op uw sjablonen voor Azure Stack met het hulpprogramma voor het valideren van sjabloon

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt een hulpprogramma voor het valideren van de sjabloon gebruiken om te controleren of uw Azure Resource Manager [sjablonen](azure-stack-arm-templates.md) gereed zijn voor het implementeren op Azure Stack. Hulpprogramma voor het valideren van de sjabloon is beschikbaar als onderdeel van de hulpprogramma's voor Azure Stack. De hulpprogramma's voor Azure Stack downloaden met behulp van de stappen in de [hulpprogramma's downloaden vanuit GitHub](azure-stack-powershell-download.md) artikel.

## <a name="overview"></a>Overzicht

Voor het valideren van een sjabloon die u hebt voor het bouwen van een cloud mogelijkheden eerst het bestand en voer vervolgens het hulpprogramma voor het valideren. U gebruikt de volgende PowerShell-modules van hulpprogramma's voor Azure Stack:

- In de **CloudCapabilities** map:<br>         `AzureRM.CloudCapabilities.psm1` Hiermee maakt u een cloud mogelijkheden JSON-bestand voor de services en -versies in een Azure Stack-cloud.
- In de **TemplateValidator** map:<br>
`AzureRM.TemplateValidator.psm1` maakt gebruik van een JSON-bestand van de cloud-mogelijkheden voor het testen van sjablonen voor implementatie in Azure Stack.

## <a name="build-the-cloud-capabilities-file"></a>Het bestand van de mogelijkheden voor cloud maken

Voordat u de validatie van de sjabloon gebruikt, voert u de **AzureRM.CloudCapabilities** PowerShell-module voor het bouwen van een JSON-bestand.

>[!NOTE]
>Als u uw geïntegreerde systeem bijwerken of nieuwe services of virtuele-extensies toevoegen, moet u deze module opnieuw uitvoeren.

1. Zorg ervoor dat u verbinding hebt met Azure Stack. Deze stappen kunnen worden uitgevoerd vanuit de Azure Stack development kit host, of kunt u een [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) verbinding maken vanaf uw werkstation.
2. Importeren van de **AzureRM.CloudCapabilities** PowerShell-module:

    ```powershell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ```

3. Gebruik de `Get-CloudCapabilities` cmdlet Serviceversies ophalen en maken van een JSON-bestand van de cloud-mogelijkheden. Als u geen opgeeft **- OutputPath**, het bestand AzureCloudCapabilities.Json in de huidige map wordt gemaakt. Gebruik uw werkelijke locatie:

    ```powershell
    Get-AzureRMCloudCapability -Location <your location> -Verbose
    ```

## <a name="validate-templates"></a>Valideren van sjablonen

Volg deze stappen voor het valideren van sjablonen met behulp van de **AzureRM.TemplateValidator** PowerShell-module. U kunt uw eigen sjablonen gebruiken of valideren de [snelstartsjablonen van Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates).

1. Importeren van de **AzureRM.TemplateValidator.psm1** PowerShell-module:

    ```powershell
    cd "c:\AzureStack-Tools-master\TemplateValidator"
    Import-Module .\AzureRM.TemplateValidator.psm1
    ```

2. Voer de validatie van de sjabloon:

    ```powershell
    Test-AzureRMTemplate -TemplatePath <path to template.json or template folder> `
    -CapabilitiesPath <path to cloudcapabilities.json> `
    -Verbose
    ```

Sjabloon voor validatie van waarschuwingen of fouten worden geregistreerd in de PowerShell-console en een HTML-bestand in de bronmap. De volgende Schermafbeelding toont een voorbeeld van een validatierapport:

![Validatierapport voor sjabloon](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parameters

De validatie van de sjabloon ondersteunt de volgende parameters.

| Parameter | Beschrijving | Vereist |
| ----- | -----| ----- |
| TemplatePath | Hiermee geeft u het pad naar recursief zoeken Azure Resource Manager-sjablonen | Ja | 
| TemplatePattern | Hiermee geeft u de naam van de sjabloonbestanden die overeenkomen met. | Nee |
| CapabilitiesPath | Hiermee geeft u het pad naar de cloud mogelijkheden JSON-bestand | Ja | 
| IncludeComputeCapabilities | Evaluatie van IaaS-resources zoals VM-grootten en VM-extensies bevat | Nee |
| IncludeStorageCapabilities | Evaluatie van de storage-resources, zoals SKU typen bevat | Nee |
| Rapport | Hiermee geeft u de naam van het gegenereerde HTML-rapport | Nee |
| Uitgebreid | Registreert fouten en waarschuwingen in de console | Nee|

### <a name="examples"></a>Voorbeelden

In dit voorbeeld alle valideert de [snelstartsjablonen van Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates) gedownload naar de lokale opslag. Het voorbeeld valideert ook grootten van virtuele machines en -extensies op basis van Azure Stack Development Kit mogelijkheden:

```powershell
test-AzureRMTemplate -TemplatePath C:\AzureStack-Quickstart-Templates `
-CapabilitiesPath .\TemplateValidator\AzureStackCloudCapabilities_with_AddOns_20170627.json `
-TemplatePattern MyStandardTemplateName.json `
-IncludeComputeCapabilities `
-Report TemplateReport.html
```

## <a name="next-steps"></a>Volgende stappen

- [Sjablonen implementeren met Azure Stack](azure-stack-arm-templates.md)
- [Sjablonen ontwikkelen voor Azure Stack](azure-stack-develop-templates.md)
