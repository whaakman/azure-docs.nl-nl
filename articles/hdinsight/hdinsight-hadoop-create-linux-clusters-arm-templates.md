---
title: Apache Hadoop-clusters maken met behulp van sjablonen - Azure HDInsight
description: Informatie over het maken van clusters voor HDInsight met behulp van Resource Manager-sjablonen
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: 028d00f579e266c92b4a6d00d5ebb5b0ef7b1b60
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53787537"
---
# <a name="create-apache-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a>Apache Hadoop-clusters in HDInsight maken met behulp van Resource Manager-sjablonen
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

In dit artikel leert u verschillende manieren om Azure HDInsight-clusters met behulp van Azure Resource Manager-sjablonen te maken. Zie voor meer informatie, [een toepassing implementeren met Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-template-deploy.md). Voor meer informatie over andere cluster maken-hulpprogramma's en functies, klikt u op de tabselector boven aan deze pagina of Zie [methoden voor het maken van Cluster](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Als u wilt de instructies in dit artikel hebt u het volgende nodig:

* Een [Azure-abonnement](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Azure PowerShell en/of Azure klassieke CLI.

### <a name="resource-manager-templates"></a>Resource Manager-sjablonen
Resource Manager-sjabloon kunt gemakkelijk maken van de volgende resoruces voor uw toepassing in een enkele, gecoördineerde bewerking:
* HDInsight-clusters en de bijbehorende afhankelijke bronnen (zoals het standaardopslagaccount).
* Andere bronnen (zoals Azure SQL Database gebruiken [Apache Sqoop](https://sqoop.apache.org/)).

In de sjabloon definieert u de resources die nodig zijn voor de toepassing. U geeft ook implementatieparameters voor het invoeren van waarden voor verschillende omgevingen. De sjabloon bestaat uit JSON en uitdrukkingen die u gebruikt om waarden voor uw implementatie samen te stellen.

U vindt de voorbeeldsjablonen HDInsight op [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/?term=hdinsight). Cross-platform gebruiken [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) met de [Resource Manager-extensie](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) of een teksteditor voor het opslaan van de sjabloon in een bestand op uw werkstation. 

Zie de volgende artikelen voor meer informatie over het Resource Manager-sjablonen:

* [Azure Resource Manager-sjablonen maken](../azure-resource-manager/resource-group-authoring-templates.md)
* [Een toepassing implementeren met Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-template-deploy.md)

## <a name="generate-templates"></a>Genereren van sjablonen

Resource Manager kunt u een Resource Manager-sjabloon exporteren uit bestaande resources in uw abonnement met behulp van verschillende hulpprogramma's. Deze gegenereerde sjabloon kunt u vervolgens gebruiken om meer te weten te komen over de sjabloonsyntaxis of om desgewenst het hergebruik van uw oplossing te automatiseren.

- Azure Portal: Zie [een Azure Resource Manager-sjabloon exporteren uit bestaande resources](../azure-resource-manager/resource-manager-export-template.md).
- Azure PowerShell: Zie [exporteren van Azure Resource Manager-sjablonen met PowerShell](../azure-resource-manager/resource-manager-export-template-powershell.md).
- Klassieke Azure-CLI: Zie [exporteren van Azure Resource Manager-sjablonen met klassieke Azure-CLI](../azure-resource-manager/resource-manager-export-template-cli.md).


## <a name="deploy-using-the-portal"></a>Implementeren met behulp van de portal

U kunt een Resource Manager-sjabloon met behulp van de Azure portal implementeren. Zie voor meer informatie, [resources implementeren vanuit een aangepaste sjabloon](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

## <a name="deploy-using-powershell"></a>Implementeren met behulp van PowerShell

U kunt implementeren met behulp van Azure PowerShell Resource Manager-sjabloon. Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md) en [persoonlijke Resource Manager-sjabloon implementeren met SAS-token en Azure PowerShell](../azure-resource-manager/resource-manager-powershell-sas-token.md).

## <a name="deploy-using-azure-cli"></a>Implementeren met behulp van Azure CLI

U kunt een Resource Manager-sjabloon met behulp van klassieke CLI implementeren. Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) en [persoonlijke Resource Manager-sjabloon implementeren met SAS-token en Azure CLI](../azure-resource-manager/resource-manager-cli-sas-token.md).

## <a name="deploy-using-the-rest-api"></a>Implementeren met behulp van de REST-API
U kunt een Resource Manager-sjabloon met behulp van REST-API implementeren. Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Resource Manager REST API](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="deploy-with-visual-studio"></a>Implementeren met Visual Studio
 Visual Studio gebruiken voor het maken van een resource-group-project en deze implementeren naar Azure via de gebruikersinterface. U selecteert het type van de resources wilt opnemen in uw project. Deze resources worden automatisch toegevoegd aan de Resource Manager-sjabloon. Het project bevat ook een PowerShell-script om de sjabloon te implementeren.

Zie voor een inleiding tot het gebruik van Visual Studio met resourcegroepen, [maken en implementeren van Azure-resourcegroepen met Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](hdinsight-hadoop-create-linux-clusters-portal.md) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd verschillende manieren om een HDInsight-cluster te maken. Zie de volgende artikelen voor meer informatie:

* Zie voor meer HDInsight verwante sjablonen [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/?term=hdinsight).
* Zie voor een voorbeeld van de implementatie van resources via de .NET-clientbibliotheek [resources implementeren met behulp van .NET-bibliotheken en een sjabloon](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Zie voor een uitgebreid voorbeeld van het implementeren van een toepassing, [inrichten en implementeren van microservices zoals verwacht in Azure](../app-service/deploy-complex-application-predictably.md).
* Zie voor instructies over het implementeren van uw oplossing in verschillende omgevingen [Ontwikkeling en testomgevingen in Microsoft Azure](../solution-dev-test-environments.md).
* Zie voor meer informatie over de secties van de Azure Resource Manager-sjabloon, [-sjablonen maken](../azure-resource-manager/resource-group-authoring-templates.md).
* Zie voor een lijst van de functies die u in een Azure Resource Manager-sjabloon gebruiken kunt, [sjabloonfuncties](../azure-resource-manager/resource-group-template-functions.md).
