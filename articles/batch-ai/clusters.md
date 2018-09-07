---
title: Werken met Azure Batch AI-clusters | Microsoft Docs
description: Over het kiezen van de configuratie van een Batch AI-cluster, en maken en beheren van een AI-cluster
services: batch-ai
documentationcenter: ''
author: johnwu10
manager: jeconnoc
editor: ''
ms.service: batch-ai
ms.topic: article
ms.date: 08/14/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 61294d8b6b84b03b1e0c8d79b4d2855452c7f0e6
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057116"
---
# <a name="work-with-batch-ai-clusters"></a>Werken met Batch AI-clusters 

In dit artikel wordt uitgelegd hoe u werkt met clusters in Azure Batch AI. Dit introduceert het concept van clusters, het type van de configuraties die mogelijk zijn, en voorbeelden. De meeste van de voorbeelden voor het maken en beheren van een cluster in dit artikel gebruikt u de Azure CLI. U kunt echter andere hulpprogramma's, waaronder de Azure-portal en de Azure Batch AI SDK's gebruiken om te werken met clusters.

Een Batch AI-cluster is een van verschillende resources in de service. Zie de [overzicht van Batch AI-resources](resource-concepts.md) om te begrijpen van het bereik van clusters in de service.

## <a name="introduction-to-clusters"></a>Inleiding tot clusters

Een Batch AI-cluster bevat de rekenresources voor het uitvoeren van machine learning en AI-trainingstaken. Alle knooppunten in een cluster hebben de dezelfde VM-grootte en de installatiekopie van het besturingssysteem. Batch AI biedt veel opties voor het maken van clusters die zijn aangepast aan verschillende behoeften. Normaal gesproken instellen van een ander cluster voor elke categorie van de verwerkingscapaciteit die nodig zijn om een project te voltooien. U kunt het aantal knooppunten in een cluster omhoog en omlaag op basis van vraag en budget schalen. Clusters kunnen worden ingericht en gedeeld door een team of personen kunnen elk een eigen cluster inrichten. 

Elk cluster bestaat onder een Batch AI-resource met de naam een *werkruimte*. Voordat u een cluster inricht, moet u een Batch AI-werkruimte instellen hebt. Bijvoorbeeld, als u de Azure CLI gebruikt, gebruikt de [az batchai werkruimte maken](/cli/azure/batchai/workspace?view=azure-cli-latest#az-batchai-workspace-create) opdracht voor het instellen van een werkruimte. 

Nadat u een cluster maakt, kunt u taken één bewerking tegelijk verzenden in een wachtrij. Het toewijzingsproces resource voor het uitvoeren van de taken op het cluster vervolgens verwerkt met batch AI. 

## <a name="cluster-configuration-options"></a>Cluster-configuratie-opties

Bij het plannen van een cluster, moet u eerst uw rekenvereisten bepalen. Batch AI biedt flexibele configuratieopties, zodat u een cluster aan uw behoeften aanpassen kunt. Hier volgen de majors-opties om te overwegen bij het inrichten van een cluster:

* **VM-grootte** -kiezen [VM-grootte](../virtual-machines/linux/sizes.md) die beschikbaar is in een [ondersteunde regio](https://azure.microsoft.com/global-infrastructure/services/) voor de clusterknooppunten. Elk cluster mag alleen een grootte van virtuele machine, dus als uw taken meerdere typen virtuele machines is vereist, u moet voor het inrichten van een cluster dat gescheiden is voor elk type compute vereiste. Als u wilt trainen machine learning- of AI-modellen die zijn ontwikkeld door frameworks die van GPU's profiteren, Zie de [GPU VM-grootten geoptimaliseerd](../virtual-machines/linux/sizes-gpu.md) in Azure.
  
* **VM-prioriteit** -Batch AI biedt toegewezen of lage prioriteit VM's voor een cluster. Toegewezen virtuele machines zijn gereserveerd voor gebruik in het cluster. Niet-gebruikte Azure-VM-capaciteit bij aanzienlijke kostenbesparingen ruil de mogelijkheid om taken voorrang wordt genomen als Azure de virtuele machines maakt kan de optie met lage prioriteit worden toegewezen. Taken die langer dan 24 uur worden uitgevoerd op een VM met lage prioriteit zijn ook automatisch bezet. Als het budget is een probleem, kunt u overwegen VM's met lage prioriteit voor korte experimenten-taken. Vervolgens overschakelen naar de toegewezen virtuele machines wanneer is het tijd meer taken uit te voeren.

* **Aantal knooppunten** -Batch AI biedt opties voor het aantal knooppunten in het cluster handmatig en automatisch schalen. Met de handmatige optie beheren u wanneer u een cluster omhoog of omlaag schalen, zodat u uw eigen compute-kosten kunt beheren. De optie voor automatisch schalen zorgt ervoor dat het cluster altijd downscales wanneer u deze niet gebruikt, zodat u uw compute-kosten kunt minimaliseren. 

  Als u kiest voor het cluster handmatig schalen, vervolgens definieert u het eerste doel tijdens het maken van clusters. Het doel is het aantal knooppunten dat Batch AI in eerste instantie toewijst. U kunt het aantal knooppunten later handmatig wijzigen.  

  Als u ervoor de optie voor automatisch schalen kiest, definieert u de maximale en minimale aantal doelknooppunten tijdens het maken van clusters. Het doel kan variëren van 0 tot het maximum aantal knooppunten die worden ondersteund door uw [Batch AI-quotum voor kerngeheugens](quota-limits.md). Een doel van 0 kunt u uw clusterconfiguratie onderhouden zonder voor de compute-kosten in rekening gebracht. Als u een hogere doel dan uw limiet voor quotum ondersteunt aanvraagt, klikt u vervolgens mislukken de inrichting. 

* **VM-installatiekopie** -Batch AI standaard richt de cluster-VM's met een standaardinstallatiekopie voor Ubuntu-Server die ondersteuning biedt voor workloads voor containers. U kunt een andere vooraf geconfigureerde Linux-installatiekopie kiezen uit de Azure Marketplace, zoals een [Data Science Virtual Machine](../machine-learning/data-science-virtual-machine/overview.md). 

* **Opslag** -Batch AI biedt een optie automatisch opslaan, kunt u kiezen wanneer u een cluster met behulp van de Azure CLI maakt. Deze optie wordt automatisch een Azure file Share- en Blob-container in een nieuw opslagaccount gemaakt. Deze storage-resources zijn gekoppeld aan elk van de knooppunten in het cluster tijdens de uitvoeringstijd, zodat de bestanden worden geopend vanaf een lokaal pad. De namen van opslagaccounts, bestandssharenaam, de naam van de Blob-container en koppelpunt paden die alle standaardwaarden hebben, die kunnen ook worden aangepast met behulp van aanvullende parameters tijdens het maken van clusters. 

  Als er geen opties voor opslag zijn gedefinieerd, moet u de storage-resources afzonderlijk maken en ze bij het verzenden van taken te definiëren. Deze optie is handig als uw cluster wordt beheerd op het organisatieniveau van de, maar uw opslag wordt beheerd op het gebruikersniveau van de. Zie voor meer informatie over het uploaden van bestanden naar Azure Storage en toegang tot deze tijdens de uitvoeringstijd [Store Batch AI-taak invoer en uitvoer met Azure Storage](use-azure-storage.md).

* **Gebruikerstoegang** - Batch AI kunt u voor het genereren van openbare en persoonlijke SSH bestanden sleutel bij het maken van een cluster, of geef uw eigen SSH-sleutels, zodat u kunt SSH in afzonderlijke knooppunten. U kunt ook een naam (standaard ingesteld als de huidige gebruiker) en het wachtwoord definiëren. Deze referenties kunnen worden gebruikt voor toegang tot de knooppunten tijdens het uitvoeren als u wilt weergeven van verschillende metrische gegevens of verdere inzicht aan uw taken.

* **Een taak** -Batch AI kunt u definiëren opdrachtregelargumenten moet worden uitgevoerd op elk knooppunt van de toewijzing. U kunt ook het pad waar het uitvoerbestand moet worden geregistreerd om te definiëren. Gebruik deze optie als u extra provisioning stappen na de basisinstallatiekopie.

* **Aanvullende configuratie** -er zijn enkele minder gebruikelijke scenario's, die mogelijk meer gespecialiseerde configuraties. In dit geval een [cluster configuratiebestand](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#using-cluster-configuration-file) kan worden gekoppeld met de Azure CLI-opdracht om een cluster te maken. 

## <a name="create-the-cluster"></a>Het cluster maken

Nadat u hebt besloten op de configuratieopties voor het cluster, gebruikt u de Azure CLI, Azure-portal of Batch AI-API's om het cluster te maken. Bijvoorbeeld, voor het maken van een cluster met behulp van de Azure CLI, kunt u volgen de [az batchai cluster maken](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-create) documentatie voor het maken van de exacte opdracht waarmee u beschikt over de configuraties die u nodig hebt. 

Op het meest eenvoudige configuratie, de volgende opdracht bepalingen een Standard_NC6 van het cluster met één knooppunt en SSH-toegang. Standaard bevat deze cluster toegewezen virtuele machines met de meest recente standaard Ubuntu Server-installatiekopie.

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --vm-size Standard_NC6 \
    --target 1 \
    --generate-ssh-keys
```

Het volgende voorbeeld levert een Standard_NC6-cluster dat automatisch wordt geschaald van 0 met 4 knooppunten en knooppunten met lage prioriteit en een auto-storage-account gebruikt. Deze instelling is een goede configuratie als u een cluster dat is de lage kosten en eenvoudig te beheren. 

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --vm-size Standard_NC6 \
    --vm-priority lowpriority \
    --max 4 \
    --min 0 \
    --use-auto-storage 
```

Een Standard nc6-cluster dat een Data Science Virtual Machine-installatiekopie bevat het volgende voorbeeld wordt ingericht, aangepaste opslag en koppelen-opties, aangepaste SSH-referenties, een setup-taak die u installeert de *unzip* pakket en een cluster het configuratiebestand voor aanvullende instellingen. Deze configuratie is een voorbeeld van een cluster die beter is afgestemd op uw eigen behoeften.

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --vm-size Standard_NC6 \
    --image UbuntuDSVM \ 
    --config-file cluster.json \
    --setup-task 'apt install unzip -y'
    --storage-account-name <STORAGE ACCOUNT NAME> \
    --nfs-name nfsmount \
    --afs-name afsmount \
    --bfs-name bfsmount \
    --user-name adminuser \
    --ssh-key id_rsa.pub \
    --password secretpassword 
```

## <a name="monitor-the-cluster"></a>Monitor voor het cluster

De [az batchai cluster list](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-list), [az batchai cluster show](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-show), en [az batchai cluster knooppuntenlijst](/cli/azure/batchai/cluster/node?view=azure-cli-latest#az-batchai-cluster-node-list) opdrachten kunnen worden gebruikt om verschillende gegevens voor elk van de clusters te controleren.

### <a name="list-all-clusters"></a>Lijst met alle clusters

De volgende opdracht lijst toestaan van de clusters in een werkruimte.

```azurecli-interactive
az batchai cluster list \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> 
```

### <a name="show-information-about-a-cluster"></a>Informatie weergeven over een cluster

De volgende opdracht toont de volledige informatie over een specifiek cluster in een tabelindeling.

```azurecli-interactive
az batchai cluster show \
    --name <CLUSTER NAME> \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --output table
```

Als uw cluster is ingericht met behulp van de opslagoptie automatische, moet u de naam van het opslagaccount te gebruiken tijdens het uploaden van scripts en trainingstaken ophalen. Gebruik de volgende opdracht:

```azurecli-interactive
az batchai cluster show \
    --name <CLUSTER NAME> \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --query "nodeSetup.mountVolumes.azureFileShares[0].{storageAccountName:accountName}"
```

De uitvoer is vergelijkbaar met de volgende.

```JSON
{
  "storageAccountName": "baixxxxxxxxx"
}
```

### <a name="list-cluster-nodes"></a>Lijst met clusterknooppunten

Als u verbinding maken met de clusterknooppunten wilt, haalt de volgende opdracht uit de lijst met knooppunten en de verbindingsinformatie bevatten.  

```azurecli-interactive
az batchai cluster node list \
    --name <CLUSTER NAME> \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> 
 ```

De uitvoer voor elk knooppunt zijn vergelijkbaar met het volgende:

```JSON
[
  {
    "ipAddress": "40.68.xxx.xxx",
    "nodeId": "tvm-xxxxxxxxxx-xxxxxxxx",
    "port": 50000.0
  }
]
```
U kunt deze informatie gebruiken om te maken van een SSH-verbinding naar een knooppunt met een vergelijkbaar met de volgende opdracht.

```bash
ssh myusername@40.68.xxx.xxx -p 50000
``` 

## <a name="submit-jobs-to-the-cluster"></a>Taken naar het cluster verzenden

Na het inrichten van het cluster kunt u vervolgens verzenden van taken om uit te voeren op de knooppunten. Zie de [az batchai job](/cli/azure/batchai/job?view=azure-cli-latest) opdracht voor verschillende manieren voorbereiden, indienen en bewaken met behulp van de Azure CLI-taken. 

## <a name="downscale-cluster-for-later-use"></a>Downscale cluster voor later gebruik

Zodra u klaar bent met uitvoeren van taken, wilt u uw cluster te verkleinen. Het verdient altijd rekenkosten downscale clusters wanneer ze niet worden gebruikt om op te slaan. Een cluster op 0 knooppunten downscaling, kunt u uw kosten stoppen terwijl dat niet hoeft te worden de clusters in de toekomst opnieuw inrichten als u wilt opnieuw zo. Als u automatisch schalen hebt geselecteerd in het maken van clusters, moet automatisch het cluster met de doel-minimale verkleinen. Als handmatig schalen is ingeschakeld, verkleinen het cluster met behulp van de volgende opdracht uit.

```azurecli-interactive
az batchai cluster resize \
    --name <CLUSTER NAME> \
    --resource-group <RESOURCE GROUP> \
    --workspace <WORKSPACE> \
    --target 0
```

## <a name="delete-a-cluster"></a>Een cluster verwijderen

Zodra u klaar bent met behulp van een cluster, gebruikt u de [az batchai cluster delete](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-delete) opdracht om deze te verwijderen.

```azurecli-interactive
az batchai cluster delete \
    --name <CLUSTER NAME> \
    --resource-group <RESOURCE GROUP> \
    --workspace <WORKSPACE>
```

De resourcegroep ook automatisch verwijderen, verwijdert u alle clusters die zijn ingericht in die resourcegroep.

```azurecli-interactive
az group delete --name <RESOURCE GROUP>
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer voorbeelden van het maken van een Batch AI-cluster, de [Portal](quickstart-create-cluster-portal.md) of [Azure CLI](quickstart-create-cluster-cli.md) snelstartgids, of de [Batch AI recepten](https://github.com/Azure/BatchAI/tree/master/recipes) op GitHub.
