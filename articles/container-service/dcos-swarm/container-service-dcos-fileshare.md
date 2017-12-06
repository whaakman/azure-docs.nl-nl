---
title: Bestandsshare voor Azure DC/OS-cluster
description: Maken en koppelen van een bestandsshare bij een DC/OS-cluster in Azure Container Service
services: container-service
author: julienstroheker
manager: dcaro
ms.service: container-service
ms.topic: tutorial
ms.date: 06/07/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: c1c318f4204efd24a2d9d3d83bb1cb71f5775bdb
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="create-and-mount-a-file-share-to-a-dcos-cluster"></a>Maken en koppelen van een bestandsshare bij een DC/OS-cluster

Deze zelfstudie details over het maken van een bestandsshare in Azure en op elke agent en de hoofdserver van de DC/OS-cluster te koppelen. Instellen van een bestandsshare, kunt gemakkelijker delen van bestanden in uw cluster, zoals configuratie, toegang en Logboeken. De volgende taken worden uitgevoerd in deze zelfstudie:

> [!div class="checklist"]
> * Een Azure-opslagaccount maken
> * Een bestandsshare maken
> * De share in de DC/OS-cluster koppelen

U moet een ACS-DC/OS-cluster de stappen in deze zelfstudie. Indien nodig, [dit voorbeeldscript](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) kunt maken voor u.

Voor deze zelfstudie is versie 2.0.4 of hoger van de Azure CLI vereist. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-file-share-on-microsoft-azure"></a>Een bestandsshare maken in Microsoft Azure

Voordat u een Azure-bestandsshare met een ACS-DC/OS-cluster, moet de storage-account en een bestandsshare worden gemaakt. Voer het volgende script voor het maken van de opslag en bestandsshare. Werk de parameters met thoes uit uw omgeving.

```azurecli-interactive
# Change these four parameters
DCOS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
DCOS_PERS_RESOURCE_GROUP=myResourceGroup
DCOS_PERS_LOCATION=eastus
DCOS_PERS_SHARE_NAME=dcosshare

# Create the storage account with the parameters
az storage account create -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -l $DCOS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $DCOS_PERS_STORAGE_ACCOUNT_NAME -g $DCOS_PERS_RESOURCE_GROUP -o tsv`

# Create the share
az storage share create -n $DCOS_PERS_SHARE_NAME
```

## <a name="mount-the-share-in-your-cluster"></a>De share in het cluster koppelen

De bestandsshare moet vervolgens worden gekoppeld aan elke virtuele machine in uw cluster. Deze taak is voltooid met het hulpprogramma cifs/protocol. De koppelingsbewerking kan handmatig worden voltooid op elk knooppunt van het cluster of door een script uitgevoerd op elk knooppunt in het cluster.

In dit voorbeeld worden twee scripts uitgevoerd, een Azure-bestandsshare en een tweede dit script uitvoeren op elk knooppunt van de DC/OS-cluster koppelen.

Eerst zijn de Azure-opslag-accountnaam en toegangssleutel nodig. Voer de volgende opdrachten om op te halen van deze informatie. Let op elk, deze waarden worden gebruikt in een later stadium.

Opslagaccountnaam:

```azurecli-interactive
STORAGE_ACCT=$(az storage account list --resource-group $DCOS_PERS_RESOURCE_GROUP --query "[?contains(name, '$DCOS_PERS_STORAGE_ACCOUNT_NAME')].[name]" -o tsv)
echo $STORAGE_ACCT
```

Toegangssleutel voor opslagaccount:

```azurecli-interactive
az storage account keys list --resource-group $DCOS_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCT --query "[0].value" -o tsv
```

Vervolgens de FQDN-naam van de DC/OS-master ophalen en opslaan in een variabele.

```azurecli-interactive
FQDN=$(az acs list --resource-group $DCOS_PERS_RESOURCE_GROUP --query "[0].masterProfile.fqdn" --output tsv)
```

Kopieer uw persoonlijke sleutel naar het hoofdknooppunt. Deze sleutel is vereist voor het maken van een ssh-verbinding met alle knooppunten in het cluster. Werk de gebruikersnaam als een niet-standaard-waarde is gebruikt bij het maken van het cluster. 

```azurecli-interactive
scp ~/.ssh/id_rsa azureuser@$FQDN:~/.ssh
```

Een SSH-verbinding maken met het model (of het eerste model) van uw DC/OS gebaseerde-cluster. Werk de gebruikersnaam als een niet-standaard-waarde is gebruikt bij het maken van het cluster.

```azurecli-interactive
ssh azureuser@$FQDN
```

Maak een bestand met de naam **cifsMount.sh**, en kopieer de volgende inhoud in de App. 

Dit script wordt gebruikt voor het koppelen van de Azure-bestandsshare. Update de `STORAGE_ACCT_NAME` en `ACCESS_KEY` variabelen met de informatie die eerder zijn verzameld.

```azurecli-interactive
#!/bin/bash

# Azure storage account name and access key
SHARE_NAME=dcosshare
STORAGE_ACCT_NAME=mystorageaccount
ACCESS_KEY=mystorageaccountKey

# Install the cifs utils, should be already installed
sudo apt-get update && sudo apt-get -y install cifs-utils

# Create the local folder that will contain our share
if [ ! -d "/mnt/share/$SHARE_NAME" ]; then sudo mkdir -p "/mnt/share/$SHARE_NAME" ; fi

# Mount the share under the previous local folder created
sudo mount -t cifs //$STORAGE_ACCT_NAME.file.core.windows.net/$SHARE_NAME /mnt/share/$SHARE_NAME -o vers=3.0,username=$STORAGE_ACCT_NAME,password=$ACCESS_KEY,dir_mode=0777,file_mode=0777
```
Maak een tweede bestand met de naam **getNodesRunScript.sh** en kopieer de volgende inhoud in het bestand. 

Dit script detecteert alle clusterknooppunten en voert de **cifsMount.sh** script om de bestandsshare op elk koppelen.

```azurecli-interactive
#!/bin/bash

# Install jq used for the next command
sudo apt-get install jq -y

# Get the IP address of each node using the mesos API and store it inside a file called nodes
curl http://leader.mesos:1050/system/health/v1/nodes | jq '.nodes[].host_ip' | sed 's/\"//g' | sed '/172/d' > nodes

# From the previous file created, run our script to mount our share on each node
cat nodes | while read line
do
  ssh `whoami`@$line -o StrictHostKeyChecking=no < ./cifsMount.sh
  done
```

Voer het script voor het koppelen van de Azure-bestandsshare op alle knooppunten van het cluster.

```azurecli-interactive
sh ./getNodesRunScript.sh
```  

De bestandsshare is nu openen op `/mnt/share/dcosshare` op elk knooppunt van het cluster.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie een Azure is-bestandsshare beschikbaar gesteld aan een DC/OS-cluster met behulp van de stappen uit:

> [!div class="checklist"]
> * Een Azure-opslagaccount maken
> * Een bestandsshare maken
> * De share in de DC/OS-cluster koppelen

Ga naar de volgende zelfstudie voor meer informatie over het integreren van een Azure Container register met DC/OS in Azure.  

> [!div class="nextstepaction"]
> [Load balancing gebruiken voor toepassingen](container-service-dcos-acr.md)
