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
ms.date: 06/30/2017
ms.author: jgao
ms.openlocfilehash: 82733e2a3025f932961122bad9d70c26896837b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a>Hadoop-clusters maken in HDInsight met behulp van Resource Manager-sjablonen
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

In dit artikel leert u Azure HDInsight-clusters maken met Azure Resource Manager-sjablonen op verschillende manieren. Zie voor meer informatie [Implementeer een toepassing met Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-template-deploy.md). Voor meer informatie over andere hulpmiddelen voor het cluster maken en -onderdelen, klik op de tabselector boven aan deze pagina of Zie [methoden voor het maken van Cluster](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Volg de instructies in dit artikel, hebt u het volgende nodig:

* Een [Azure-abonnement](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Azure PowerShell en/of Azure CLI.

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="resource-manager-templates"></a>Resource Manager-sjablonen
Resource Manager-sjabloon kunt u gemakkelijk maken de volgende onderwerpen voor uw toepassing in een enkele, gecoördineerde bewerking:
* HDInsight-clusters en hun afhankelijke bronnen (zoals het standaardopslagaccount)
* Andere bronnen (zoals Azure SQL Database Apache Sqoop gebruiken)

In de sjabloon definieert u de resources die nodig zijn voor de toepassing. U ook opgeven implementatieparameters voor het invoeren van waarden voor verschillende omgevingen. De sjabloon bestaat uit JSON en uitdrukkingen die u gebruikt om waarden voor uw implementatie samen te stellen.

Vindt u voorbeelden van HDInsight-sjabloon op [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/?term=hdinsight). Cross-platform gebruiken [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) met de [Resource Manager-extensie](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) of een teksteditor aan de sjabloon opslaan in een bestand op uw werkstation. U informatie over het aanroepen van de sjabloon met behulp van verschillende methoden.

Zie de volgende artikelen voor meer informatie over Resource Manager-sjablonen:

* [De auteur van Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md)
* [Implementeer een toepassing met Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-template-deploy.md)

## <a name="generate-templates"></a>Genereren van sjablonen

U kunt de eigenschappen van een cluster configureren en vervolgens de sjabloon opslaan voordat u deze implementeert met behulp van de Azure-portal. U kunt vervolgens de sjabloon opnieuw te gebruiken.

**Genereren van een sjabloon met behulp van de Azure-portal**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **nieuw** Klik op het menu links op **Intelligence en analyse**, en klik vervolgens op **HDInsight**.
3. Volg de instructies voor de eigenschappen in te voeren. U kunt ofwel de **snelle invoer** of de **aangepaste** optie.
4. Op de **samenvatting** tabblad **sjabloon en parameters downloaden**:

    ![Maken van HDInsight Hadoop cluster Resource Manager-sjabloon downloaden](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download.png)

    U ziet een lijst van het sjabloonbestand, parameterbestand en codevoorbeelden die worden gebruikt om de sjabloon te implementeren:

    ![HDInsight Hadoop maken-cluster Downloadinstellingen voor Resource Manager-sjabloon](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download-options.png)

    Hier kunt u downloaden van de sjabloon, sla deze op uw sjabloonbibliotheek of implementeren van de sjabloon.

    Als u een sjabloon in de bibliotheek, klikt u op **meer services** vanuit het menu aan de linkerkant en klik op **sjablonen** (onder de **andere** categorie).

    > [!Note]
    > Het bestand sjabloon en de parameters moet samen worden gebruikt. Anders krijgt u mogelijk onverwachte resultaten. Bijvoorbeeld, de standaard **clusterKind** is altijd de waarde van de eigenschap **hadoop**, ondanks wat u opgeven voordat u de sjabloon hebt gedownload.



## <a name="deploy-with-powershell"></a>Implementeren met PowerShell

Deze procedure maakt u een Hadoop-cluster in HDInsight.

1. Sla het JSON-bestand in de [bijlage](#appx-a-arm-template) naar uw werkstation. In het PowerShell-script de bestandsnaam is `C:\HDITutorials-ARM\hdinsight-arm-template.json`.
2. De parameters en variabelen instellen indien nodig.
3. Voer de sjabloon opnieuw uit met behulp van de volgende PowerShell-script:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>"
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and variables
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage account
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

    Het PowerShell-script configureert u de naam van het cluster. Naam van het opslagaccount is vastgelegd in de sjabloon. U wordt gevraagd het wachtwoord van de gebruiker cluster invoeren. (De standaardgebruikersnaam **admin**.) U wordt ook gevraagd het wachtwoord van de gebruiker SSH invoeren. (De standaard SSH-gebruikersnaam is **sshuser**.)  

Zie voor meer informatie [implementeren met PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).

## <a name="deploy-with-cli"></a>Implementeren met CLI
Het volgende voorbeeld maakt gebruik van Azure-opdrachtregelinterface (CLI). Er wordt een cluster en het afhankelijke opslagaccount en container gemaakt door het aanroepen van Resource Manager-sjabloon:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"

U wordt gevraagd om in te voeren:
* De naam van het cluster.
* Wachtwoord van de cluster-gebruiker. (De standaardgebruikersnaam **admin**.)
* Wachtwoord van de SSH-gebruiker. (De standaard SSH-gebruikersnaam is **sshuser**.)

De volgende code geeft de parameters inline:

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

## <a name="deploy-with-the-rest-api"></a>Met de REST-API implementeren
Zie [implementeren met de REST-API](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="deploy-with-visual-studio"></a>Implementeren met Visual Studio
 Gebruik Visual Studio een resourcegroepproject maken en deze implementeren naar Azure via de gebruikersinterface. U selecteert het type resources in uw project op te nemen. Deze resources worden automatisch toegevoegd aan de Resource Manager-sjabloon. Het project biedt ook een PowerShell-script voor het implementeren van de sjabloon.

Zie voor een inleiding tot het gebruik van Visual Studio aan resourcegroepen, [maken en implementeren van Azure-resourcegroepen met Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](hdinsight-administer-use-portal-linux.md#create-clusters) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd op verschillende manieren om een HDInsight-cluster te maken. Zie voor meer informatie de volgende artikelen:

* Zie voor een voorbeeld van het implementeren van resources via de .NET-clientbibliotheek [resources implementeren met behulp van .NET-bibliotheken en een sjabloon](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Zie voor een gedetailleerd voorbeeld van een toepassing implementeren, [inrichten en implementeren van microservices zoals verwacht in Azure](../app-service/app-service-deploy-complex-application-predictably.md).
* Zie voor instructies over het implementeren van uw oplossing in verschillende omgevingen [Ontwikkeling en testomgevingen in Microsoft Azure](../solution-dev-test-environments.md).
* Zie voor meer informatie over de secties van de Azure Resource Manager-sjabloon, [sjablonen](../azure-resource-manager/resource-group-authoring-templates.md).
* Zie voor een lijst van de functies die u in een Azure Resource Manager-sjabloon gebruiken kunt [sjabloonfuncties](../azure-resource-manager/resource-group-template-functions.md).

## <a name="appendix-resource-manager-template-to-create-a-hadoop-cluster"></a>Bijlage: Resource Manager-sjabloon maken van een Hadoop-cluster
De volgende Azure Resource Manager-sjabloon maakt een Linux gebaseerde Hadoop-cluster met de afhankelijke Azure storage-account.

> [!NOTE]
> Dit voorbeeld bevat configuratie-informatie voor Hive-metastore en Oozie-metastore. De sectie te verwijderen of de sectie configureren voordat u de sjabloon.
>
>

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "sshUserName": {
        "type": "string",
        "defaultValue": "sshuser",
        "metadata": {
            "description": "These credentials can be used to remotely access the cluster."
        }
        },
        "sshPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "location": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Australia East",
            "Australia Southeast"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterType": {
        "type": "string",
        "defaultValue": "hadoop",
        "allowedValues": [
            "hadoop",
            "hbase",
            "storm",
            "spark"
        ],
        "metadata": {
            "description": "The type of the HDInsight cluster to create."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
    "variables": {
        "defaultApiVersion": "2015-05-01-preview",
        "clusterApiVersion": "2015-03-01-preview",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
        {
        "name": "[parameters('clusterName')]",
        "type": "Microsoft.HDInsight/clusters",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('clusterApiVersion')]",
        "dependsOn": [ "[concat('Microsoft.Storage/storageAccounts/',variables('clusterStorageAccountName'))]" ],
        "tags": {

        },
        "properties": {
            "clusterVersion": "3.4",
            "osType": "Linux",
            "tier": "standard",
            "clusterDefinition": {
            "kind": "[parameters('clusterType')]",
            "configurations": {
                "gateway": {
                "restAuthCredential.isEnabled": true,
                "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                },
                "hive-site": {
                    "javax.jdo.option.ConnectionDriverName": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "javax.jdo.option.ConnectionURL": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "javax.jdo.option.ConnectionUserName": "johndole",
                    "javax.jdo.option.ConnectionPassword": "myPassword$"
                },
                "hive-env": {
                    "hive_database": "Existing MSSQL Server database with SQL authentication",
                    "hive_database_name": "myhive20160901",
                    "hive_database_type": "mssql",
                    "hive_existing_mssql_server_database": "myhive20160901",
                    "hive_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "hive_hostname": "myadla0901dbserver.database.windows.net"
                },
                "oozie-site": {
                    "oozie.service.JPAService.jdbc.driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "oozie.service.JPAService.jdbc.url": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "oozie.service.JPAService.jdbc.username": "johndole",
                    "oozie.service.JPAService.jdbc.password": "myPassword$",
                    "oozie.db.schema.name": "oozie"
                },
                "oozie-env": {
                    "oozie_database": "Existing MSSQL Server database with SQL authentication",
                    "oozie_database_name": "myhive20160901",
                    "oozie_database_type": "mssql",
                    "oozie_existing_mssql_server_database": "myhive20160901",
                    "oozie_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "oozie_hostname": "myadla0901dbserver.database.windows.net"
                }            
            }
            },
            "storageProfile": {
            "storageaccounts": [
                {
                "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                "isDefault": true,
                "container": "[parameters('clusterName')]",
                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                }
            ]
            },
            "computeProfile": {
            "roles": [
                {
                "name": "headnode",
                "targetInstanceCount": "2",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                },
                {
                "name": "workernode",
                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                }
            ]
            }
        }
        }
    ],
    "outputs": {
        "cluster": {
        "type": "object",
        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
        }
    }
    }

## <a name="appendix-resource-manager-template-to-create-a-spark-cluster"></a>Bijlage: Resource Manager-sjabloon maken van een Spark-cluster

Deze sectie bevat een Resource Manager-sjabloon die u gebruiken kunt om een HDInsight Spark-cluster te maken. Deze sjabloon bevat configuraties voor `spark-defaults` en `spark-thrift-sparkconf` (voor Spark 1.6-clusters) en `spark2-defaults` en `spark2-thrift-sparkconf` (voor 2 Spark-clusters). Hiernaast HDInsight berekend en ingesteld configuraties, zoals `spark.executor.instances`, `spark.executor.memory`, en `spark.executor.cores` op basis van de clustergrootte. 

Als u een één parameter ingesteld in een sectie als onderdeel van de sjabloon zelf, HDInsight niet berekenen en stel de andere parameters van de dezelfde sectie. Bijvoorbeeld: parameter `spark.executor.instances` bevindt zich in de `spark-defaults` configuratie. Als u een andere parameter ingesteld (bijvoorbeeld `spark.yarn.exector.memoryOverhead`) in de `spark-defaults` configuratie HDInsight niet berekenen en stel de `spark.executor.instances` ook de parameter.

    {
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "parameters": {
        "clusterName": {
            "type": "string",
            "metadata": {
                "description": "The name of the HDInsight cluster to create."
            }
        },
        "clusterLoginUserName": {
            "type": "string",
            "defaultValue": "admin",
            "metadata": {
                "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
            }
        },
        "clusterLoginPassword": {
            "type": "securestring",
            "metadata": {
                "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus",
            "metadata": {
                "description": "The location where all azure resources will be deployed."
            }
        },
        "clusterVersion": {
            "type": "string",
            "defaultValue": "3.5",
            "metadata": {
                "description": "HDInsight cluster version."
            }
        },
        "clusterWorkerNodeCount": {
            "type": "int",
            "defaultValue": 4,
            "metadata": {
                "description": "The number of nodes in the HDInsight cluster."
            }
        },
        "clusterKind": {
            "type": "string",
            "defaultValue": "SPARK",
            "metadata": {
                "description": "The type of the HDInsight cluster to create."
            }
        },
        "sshUserName": {
            "type": "string",
            "defaultValue": "sshuser",
            "metadata": {
                "description": "These credentials can be used to remotely access the cluster."
            }
        },
        "sshPassword": {
            "type": "securestring",
            "metadata": {
                "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        }
    },
    "variables": {
        "defaultApiVersion": "2017-06-01",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
    {
            "apiVersion": "2015-03-01-preview",
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "dependsOn": [],
            "properties": {
                "clusterVersion": "[parameters('clusterVersion')]",
                "osType": "Linux",
                "tier": "standard",
                "clusterDefinition": {
                    "kind": "[parameters('clusterKind')]",
                    "configurations": {
                        "gateway": {
                            "restAuthCredential.isEnabled": true,
                            "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                            "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                        },
                        "spark-defaults": {
                            "spark.executor.cores": "2"
                        },
                        "spark-thrift-sparkconf": {
                            "spark.yarn.executor.memoryOverhead": "896"
                        }
                    }
                },
                "storageProfile": {
                    "storageaccounts": [
                        {
                            "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                            "isDefault": true,
                            "container": "[parameters('clusterName')]",
                            "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                        }
                    ]
                },
                "computeProfile": {
                    "roles": [
                        {
                            "name": "headnode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 2,
                            "hardwareProfile": {
                                "vmSize": "Standard_D12"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                }
                            },
                            "virtualNetworkProfile": null,
                            "scriptActions": []
                        },
                        {
                            "name": "workernode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 4,
                            "hardwareProfile": {
                                "vmSize": "Standard_D4"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                    }
                                },
                                "virtualNetworkProfile": null,
                                "scriptActions": []
                            }
                        ]
                    }
                }
            }
        ]
    }
