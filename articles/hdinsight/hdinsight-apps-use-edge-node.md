---
title: Lege edge-knooppunten op Hadoop-clusters in HDInsight - Azure gebruiken | Microsoft Docs
description: Hoe een leeg edge-knooppunt toevoegen aan een HDInsight-cluster die kan worden gebruikt als een client en vervolgens test/host uw HDInsight-toepassingen.
services: hdinsight
editor: cgronlun
manager: jhubbard
author: mumian
tags: azure-portal
documentationcenter: 
ms.assetid: cdc7d1b4-15d7-4d4d-a13f-c7d3a694b4fb
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jgao
ms.openlocfilehash: e21dabcc6999b1f1047d334e782f723d0c03c2cb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-empty-edge-nodes-on-hadoop-clusters-in-hdinsight"></a>Lege edge-knooppunten op Hadoop-clusters in HDInsight gebruiken

Informatie over het toevoegen van een leeg edge-knooppunt aan een HDInsight-cluster. Een leeg edge-knooppunt is een virtuele Linux-machine met de dezelfde clienthulpprogramma's geïnstalleerd en geconfigureerd zoals in de headnodes, maar met geen Hadoop-services die worden uitgevoerd. U kunt het edge-knooppunt gebruiken voor toegang tot het cluster, testen van uw clienttoepassingen en die als host fungeert voor uw clienttoepassingen. 

U kunt een lege edge-knooppunt toevoegen aan een bestaand HDInsight-cluster naar een nieuw cluster bij het maken van het cluster. Toevoegen van een leeg edge-knooppunt wordt uitgevoerd met behulp van Azure Resource Manager-sjabloon.  Het volgende voorbeeld laat zien hoe deze wordt uitgevoerd met behulp van een sjabloon:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [ "[concat('Microsoft.HDInsight/clusters/',parameters('clusterName'))]" ],
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "Standard_D3"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "[parameters('installScriptAction')]",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Zoals u in het voorbeeld, kunt u eventueel aanroepen een [script actie](hdinsight-hadoop-customize-cluster-linux.md) om uit te voeren aanvullende configuratiestappen, zoals het installeren van [Apache Hue](hdinsight-hadoop-hue-linux.md) in het edge-knooppunt. Het script actiescript moet openbaar toegankelijk is op het web.  Bijvoorbeeld, als het script is opgeslagen in Azure-opslag, gebruiken openbare containers of openbare blobs.

De virtuele machinegrootte van het edge-knooppunt moet voldoen aan de vereiste grootte van HDInsight-cluster worker knooppunt vm. Zie voor de aanbevolen worker knooppunt vm-grootten [maken Hadoop-clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).

Nadat u een edge-knooppunt hebt gemaakt, kunt u verbinding maken met het edge-knooppunt met behulp van SSH en clienthulpprogramma's voor toegang tot het Hadoop-cluster in HDInsight worden uitgevoerd.

> [!WARNING] 
> Gebruik een lege edge-knooppunt met HDInsight is momenteel in preview. Aangepaste onderdelen die zijn geïnstalleerd op de edge-knooppunt ontvangt binnen commercieel redelijke ondersteuning van Microsoft. Dit kan leiden bij het oplossen van problemen die kunnen optreden. Of u kan worden verwezen naar community-bronnen voor verdere ondersteuning. Hier volgen enkele van de meest actieve sites voor het ophalen van de help van de community:
>
> * [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight)
> * [http://stackoverflow.com](http://stackoverflow.com).
>
> Als u een Apache-technologie gebruikt, u kunt mogelijk hulp via de Apache projectsites zoeken op [http://apache.org](http://apache.org), zoals de [Hadoop](http://hadoop.apache.org/) site.

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Een edge-knooppunt toevoegen aan een bestaand cluster
In deze sectie kunt u een Resource Manager-sjabloon gebruiken een edge-knooppunt toevoegen aan een bestaand HDInsight-cluster.  De Resource Manager-sjabloon kunt u vinden in [GitHub](https://azure.microsoft.com/en-us/resources/templates/101-hdinsight-linux-add-edge-node/). De Resource Manager-sjabloon roept een scriptactie zich bevindt op https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh. Het script heeft geen acties uitvoeren.  Het is voor het demonstreren van aanroepen scriptactie van Resource Manager-sjabloon.

**Een leeg edge-knooppunt toevoegen aan een bestaand cluster**

1. Een HDInsight-cluster maken als u nog niet hebt.  Zie [Hadoop-zelfstudie: aan de slag met Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klik op de volgende afbeelding om te melden bij Azure en open de Azure Resource Manager-sjabloon in de Azure-portal. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-add-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy to Azure"></a>
3. Configureer de volgende eigenschappen:
   
   * **Abonnement**: Selecteer een Azure-abonnement gebruikt voor het maken van het cluster.
   * **Resourcegroep**: Selecteer de resourcegroep die wordt gebruikt voor het bestaande HDInsight-cluster.
   * **Locatie**: Selecteer de locatie van het bestaande HDInsight-cluster.
   * **Clusternaam**: Voer de naam van een bestaand HDInsight-cluster.
   * **Edge-Knooppuntgrootte**: Selecteer een van de VM-grootte. De vm-grootte moet voldoen aan de vereiste worker knooppunt vm-grootte. Zie voor de aanbevolen worker knooppunt vm-grootten [maken Hadoop-clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).
   * **Edge-knooppunt voorvoegsel**: de standaardwaarde is **nieuwe**.  De naam van de edge-knooppunt is de standaardwaarde wordt gebruikt, **nieuwe edgenode**.  U kunt het voorvoegsel van de portal kunt aanpassen. U kunt ook de volledige naam van de sjabloon aanpassen.

4. Controleer **ik ga akkoord met de voorwaarden en bepalingen hierboven**, en klik vervolgens op **aankoop** voor het maken van het edge-knooppunt.

>[!IMPORTANT]
> Zorg ervoor dat u selecteert de Azure-resourcegroep voor de bestaande HDInsight-cluster.  Anders wordt de ophalen van het foutbericht 'kan aangevraagde bewerking op geneste resource niet uitvoeren. Bovenliggende resource '&lt;ClusterName >' niet gevonden. "

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Een edge-knooppunt toevoegen bij het maken van een cluster
In deze sectie kunt u een Resource Manager-sjabloon maken van HDInsight-cluster met een edge-knooppunt.  De Resource Manager-sjabloon kunt u vinden in de [galerie van Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/). De Resource Manager-sjabloon roept een scriptactie zich bevindt op https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh. Het script heeft geen acties uitvoeren.  Het is voor het demonstreren van aanroepen scriptactie van Resource Manager-sjabloon.

**Een leeg edge-knooppunt toevoegen aan een bestaand cluster**

1. Een HDInsight-cluster maken als u nog niet hebt.  Zie [aan de slag met Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klik op de volgende afbeelding om te melden bij Azure en open de Azure Resource Manager-sjabloon in de Azure-portal. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy to Azure"></a>
3. Configureer de volgende eigenschappen:
   
   * **Abonnement**: Selecteer een Azure-abonnement gebruikt voor het maken van het cluster.
   * **Resourcegroep**: Maak een nieuwe resourcegroep gebruikt voor het cluster.
   * **Locatie**: selecteer een locatie voor de resourcegroep.
   * **Clusternaam**: Voer een naam voor het nieuwe cluster te maken.
   * **Aanmeldingsnaam van gebruiker cluster**: Geef de gebruikersnaam van Hadoop HTTP.  De standaardnaam is **admin**.
   * **Aanmeldingswachtwoord cluster**: Geef het wachtwoord van de Hadoop-HTTP-gebruiker.
   * **SSH gebruikersnaam**: Voer de SSH-gebruikersnaam. De standaardnaam is **sshuser**.
   * **SSH wachtwoord**: Geef het wachtwoord van de SSH-gebruiker.
   * **Installeren van de scriptactie**: de standaardwaarde voor het doorlopen van deze zelfstudie houden.
     
     Sommige eigenschappen zijn vastgelegd in de sjabloon: clustertype, aantal Cluster worker-knooppunten, de grootte van de Edge-knooppunt en de naam van de Edge-knooppunt.
4. Controleer **ik ga akkoord met de voorwaarden en bepalingen hierboven**, en klik vervolgens op **aankoop** het cluster maken met het edge-knooppunt.

## <a name="access-an-edge-node"></a>Toegang tot een edge-knooppunt
Het edge-knooppunt ssh-eindpunt is &lt;EdgeNodeName >.&lt; ClusterName >-ssh.azurehdinsight.net:22.  Bijvoorbeeld, nieuwe-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Het edge-knooppunt wordt weergegeven als een toepassing op de Azure-portal.  De portal biedt u de informatie voor toegang tot het gebruik van SSH edge-knooppunt.

**Om te controleren of het edge-knooppunt SSH-eindpunt**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open het HDInsight-cluster met een edge-knooppunt.
3. Klik op **toepassingen** uit de cluster-blade. U ziet het edge-knooppunt.  De standaardnaam is **nieuwe edgenode**.
4. Klik op de edge-knooppunt. U ziet het SSH-eindpunt.

**Hive gebruiken op de edge-knooppunt**

1. SSH gebruiken voor verbinding met het edge-knooppunt. Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

2. Nadat u hebt verbonden met het edge-knooppunt via SSH, gebruikt u de volgende opdracht om de Hive-console te openen:
   
        hive
3. Voer de volgende opdracht om Hive-tabellen in het cluster weer te geven:
   
        show tables;

## <a name="delete-an-edge-node"></a>Verwijderen van een edge-knooppunt
U kunt een edge-knooppunt verwijderen uit de Azure-portal.

**Voor toegang tot een edge-knooppunt**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open het HDInsight-cluster met een edge-knooppunt.
3. Klik op **toepassingen** uit de cluster-blade. U ziet een lijst met knooppunten van de rand.  
4. Met de rechtermuisknop op de edge-knooppunt dat u wilt verwijderen en klik vervolgens op **verwijderen**.
5. Klik op **Ja** om te bevestigen.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe een edge-knooppunt toevoegen en over het openen van het edge-knooppunt. Zie voor meer informatie de volgende artikelen:

* [HDInsight-toepassingen installeren](hdinsight-apps-install-applications.md): informatie over het installeren van een HDInsight-toepassing op uw clusters.
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): informatie over het publiceren van aangepaste HDInsight-toepassingen in Azure Marketplace.
* [MSDN: een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): informatie over het definiëren van HDInsight-toepassingen.
* [Op Linux gebaseerde HDInsight-clusters aanpassen met behulp van een scriptactie](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik van een scriptactie om extra toepassingen te installeren.
* [Op Linux gebaseerde Hadoop-clusters maken in HDInsight met behulp van Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md): informatie over het aanroepen van Resource Manager-sjablonen om HDInsight-clusters te maken.

