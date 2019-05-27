---
title: Implementeren van geneste Resource Manager-sjabloon-omgevingen in Azure DevTest Labs | Microsoft Docs
description: Informatie over het implementeren van geneste Azure Resource Manager-sjablonen voor omgevingen met Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2019
ms.author: spelluru
ms.openlocfilehash: eec0cde4a36449f85998bfb04d16f1d52c68bb19
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65835283"
---
# <a name="deploy-nested-azure-resource-manager-templates-for-testing-environments"></a>Geneste Azure Resource Manager-sjablonen voor het testen van omgevingen implementeren
Een geneste implementatie kunt u voor het uitvoeren van andere Azure Resource Manager-sjablonen uit binnen een belangrijke Resource Manager-sjabloon. Hiermee kunt u uw implementatie in een set van gerichte en doel-specifieke sjablonen ontleden. Dit biedt voordelen wat betreft testen, opnieuw gebruiken en leesbaarheid. Het artikel [gekoppelde sjablonen gebruiken bij het implementeren van Azure-resources](../azure-resource-manager/resource-group-linked-templates.md) biedt een goed overzicht van deze oplossing met enkele voorbeelden van code. Dit artikel bevat een voorbeeld waarin is specifiek voor Azure DevTest Labs. 

## <a name="key-parameters"></a>Belangrijke parameters
U kunt uw eigen Resource Manager-sjabloon maken helemaal, raden wij aan dat u de [Azure-resourcegroepproject](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) in Visual Studio, waardoor het eenvoudig ontwikkelen en fouten opsporen in sjablonen. Wanneer u een geneste implementatie-resource aan azuredeploy.json toevoegen, wordt in Visual Studio verschillende items om te zorgen dat de sjabloon flexibelere toegevoegd. Deze items bevatten de submap met de sjabloon en parameters secundair, namen van variabelen in de belangrijkste sjabloonbestand en twee parameters voor de opslaglocatie voor de nieuwe bestanden. De **_artifactsLocation** en **_artifactsLocationSasToken** zijn de belangrijke parameters die gebruikmaakt van de DevTest Labs. 

Als u niet bekend bent met de werking van de DevTest Labs met omgevingen, Zie [multi-VM-omgevingen en PaaS-resources maken met Azure Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md). De sjablonen worden opgeslagen in de opslagplaats die is gekoppeld aan het lab in DevTest Labs. Wanneer u een nieuwe omgeving met deze sjablonen maken, worden de bestanden worden verplaatst naar een Azure Storage-container in het lab. Als u om te bepalen en de geneste bestanden kopiëren, DevTest Labs identificeert de parameters _artifactsLocation en _artifactsLocationSasToken en de submappen tot de storage-container worden gekopieerd. Vervolgens voegt automatisch de locatie en het Shared Access Signature (SaS)-token in parameters. 

## <a name="nested-deployment-example"></a>Voorbeeld van de geneste implementatie
Hier is een eenvoudig voorbeeld van een geneste implementatie:

```json

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": {
    "_artifactsLocation": {
        "type": "string"
    },
    "_artifactsLocationSasToken": {
        "type": "securestring"
    }},
"variables": {
    "NestOneTemplateFolder": "nestedtemplates",
    "NestOneTemplateFileName": "NestOne.json",
    "NestOneTemplateParametersFileName": "NestOne.parameters.json"},
    "resources": [
    {
        "name": "NestOne",
        "type": "Microsoft.Resources/deployments",
        "apiVersion": "2016-09-01",
        "dependsOn": [ ],
        "properties": {
            "mode": "Incremental",
            "templateLink": {
                "uri": "[concat(parameters('_artifactsLocation'), '/', variables('NestOneTemplateFolder'), '/', variables('NestOneTemplateFileName'), parameters('_artifactsLocationSasToken'))]",
                "contentVersion": "1.0.0.0"
            },
            "parametersLink": {
                "uri": "[concat(parameters('_artifactsLocation'), '/', variables('NestOneTemplateFolder'), '/', variables('NestOneTemplateParametersFileName'), parameters('_artifactsLocationSasToken'))]",
                "contentVersion": "1.0.0.0"
            }
        }    
    }],
"outputs": {}
```

De map in de opslagplaats met deze sjabloon is een submap `nestedtemplates` met de bestanden **NestOne.json** en **NestOne.parameters.json**. In de **azuredeploy.json**, URI voor de sjabloon die is gebouwd met behulp van de locatie van de artefacten, de map met geneste sjablonen, genest bestandsnaam van de sjabloon. URI voor de parameters is op deze manier worden gebouwd met behulp van de locatie van de artefacten, geneste sjabloonmap en parameterbestand voor de geneste sjabloon. 

Dit is de installatiekopie van de structuur van de dezelfde project in Visual Studio: 

![Structuur van het project in Visual Studio](./media/deploy-nested-template-environments/visual-studio-project-structure.png)

In de hoofdmap, maar niet elk meer dan één niveau, kunt u extra mappen toevoegen. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over omgevingen: 

- [Multi-VM-omgevingen en PaaS-resources maken met Azure Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md)
- [Configuratie en het gebruik van openbare omgevingen in Azure DevTest Labs](devtest-lab-configure-use-public-environments.md)
- [Verbinding maken met een omgeving met virtuele netwerk van uw lab in Azure DevTest Labs](connect-environment-lab-virtual-network.md)