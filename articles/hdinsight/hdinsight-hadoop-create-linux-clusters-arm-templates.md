---
title: Hadoop-clusters maken met behulp van sjablonen - Azure HDInsight | Microsoft Docs
description: Informatie over het maken van clusters voor HDInsight met behulp van Resource Manager-sjablonen
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00a80dea-011f-44f0-92a4-25d09db9d996
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/06/2017
ms.author: jgao
ms.openlocfilehash: 38a60a64c8b2207a4795a63cfeb3527dedc8aa91
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="create-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a>Hadoop-clusters maken in HDInsight met behulp van Resource Manager-sjablonen
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

In dit artikel leert u op verschillende manieren om Azure HDInsight-clusters met behulp van Azure Resource Manager-sjablonen te maken. Zie voor meer informatie [Implementeer een toepassing met Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-template-deploy.md). Voor meer informatie over andere hulpmiddelen voor het cluster maken en -onderdelen, klik op de tabselector boven aan deze pagina of Zie [methoden voor het maken van Cluster](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Volg de instructies in dit artikel, hebt u het volgende nodig:

* Een [Azure-abonnement](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Azure PowerShell en/of Azure CLI.

### <a name="resource-manager-templates"></a>Resource Manager-sjablonen
Resource Manager-sjabloon kunt u gemakkelijk maken de volgende resoruces voor uw toepassing in een enkele, gecoördineerde bewerking:
* HDInsight-clusters en hun afhankelijke bronnen (zoals het standaardopslagaccount)
* Andere bronnen (zoals Azure SQL Database Apache Sqoop gebruiken)

In de sjabloon definieert u de resources die nodig zijn voor de toepassing. U ook opgeven implementatieparameters voor het invoeren van waarden voor verschillende omgevingen. De sjabloon bestaat uit JSON en uitdrukkingen die u gebruikt om waarden voor uw implementatie samen te stellen.

Vindt u voorbeelden van HDInsight-sjabloon op [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/?term=hdinsight). Cross-platform gebruiken [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) met de [Resource Manager-extensie](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) of een teksteditor aan de sjabloon opslaan in een bestand op uw werkstation. 

Zie de volgende artikelen voor meer informatie over Resource Manager-sjablonen:

* [De auteur van Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md)
* [Implementeer een toepassing met Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-template-deploy.md)

## <a name="generate-templates"></a>Genereren van sjablonen

Resource Manager kunt u een Resource Manager-sjabloon exporteren uit bestaande resources in uw abonnement met verschillende hulpprogramma's. Deze gegenereerde sjabloon kunt u vervolgens gebruiken om meer te weten te komen over de sjabloonsyntaxis of om desgewenst het hergebruik van uw oplossing te automatiseren.

- Azure-portal: Zie [een Azure Resource Manager-sjabloon uit bestaande resources exporteren](../azure-resource-manager/resource-manager-export-template.md).
- Azure PowerShell: Zie [exporteren Azure Resource Manager-sjablonen met PowerShell](../azure-resource-manager/resource-manager-export-template-powershell.md).
- Azure CLI: Zie [exporteren Azure Resource Manager-sjablonen met Azure CLI](../azure-resource-manager/resource-manager-export-template-cli.md).


## <a name="deploy-using-the-portal"></a>Implementeren met behulp van de portal

U kunt een Resource Manager-sjabloon met de Azure portal kunt implementeren. Zie voor meer informatie [resources met aangepaste sjabloon implementeren](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

## <a name="deploy-using-powershell"></a>Implementeren met behulp van PowerShell

U kunt implementeren met behulp van PowerShell voor Azure Resource Manager-sjabloon. Zie voor meer informatie [implementeren van resources met Resource Manager-sjablonen en Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md) en [persoonlijke Resource Manager-sjabloon implementeren met SAS-token en Azure PowerShell](../azure-resource-manager/resource-manager-powershell-sas-token.md).

## <a name="deploy-using-cli"></a>Implementeren met CLI

U kunt een Resource Manager-sjabloon die met Azure CLI kunt implementeren. Zie voor meer informatie [implementeren van resources met Resource Manager-sjablonen en Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) en [persoonlijke Resource Manager-sjabloon implementeren met SAS-token en Azure CLI](../azure-resource-manager/resource-manager-cli-sas-token.md).

## <a name="deploy-using-the-rest-api"></a>Implementeren met behulp van de REST-API
U kunt een Resource Manager-sjabloon met REST-API implementeren. Zie voor meer informatie [resources met Resource Manager-sjablonen en REST-API van Resource Manager implementeren](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="deploy-with-visual-studio"></a>Implementeren met Visual Studio
 Gebruik Visual Studio een resourcegroepproject maken en deze implementeren naar Azure via de gebruikersinterface. U selecteert het type resources in uw project op te nemen. Deze resources worden automatisch toegevoegd aan de Resource Manager-sjabloon. Het project biedt ook een PowerShell-script voor het implementeren van de sjabloon.

Zie voor een inleiding tot het gebruik van Visual Studio aan resourcegroepen, [maken en implementeren van Azure-resourcegroepen met Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](hdinsight-administer-use-portal-linux.md#create-clusters) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd op verschillende manieren om een HDInsight-cluster te maken. Zie voor meer informatie de volgende artikelen:

* Zie voor meer HDInsight gerelateerde sjablonen [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/?term=hdinsight).
* Zie voor een voorbeeld van het implementeren van resources via de .NET-clientbibliotheek [resources implementeren met behulp van .NET-bibliotheken en een sjabloon](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Zie voor een gedetailleerd voorbeeld van een toepassing implementeren, [inrichten en implementeren van microservices zoals verwacht in Azure](../app-service/app-service-deploy-complex-application-predictably.md).
* Zie voor instructies over het implementeren van uw oplossing in verschillende omgevingen [Ontwikkeling en testomgevingen in Microsoft Azure](../solution-dev-test-environments.md).
* Zie voor meer informatie over de secties van de Azure Resource Manager-sjabloon, [sjablonen](../azure-resource-manager/resource-group-authoring-templates.md).
* Zie voor een lijst van de functies die u in een Azure Resource Manager-sjabloon gebruiken kunt [sjabloonfuncties](../azure-resource-manager/resource-group-template-functions.md).