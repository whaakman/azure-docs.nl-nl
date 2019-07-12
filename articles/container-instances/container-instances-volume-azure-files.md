---
title: Koppelen van een Azure Files-volume in Azure Container Instances
description: Informatie over het koppelen van een Azure Files-volume als u wilt persisteren van statuswaarden met Azure Container Instances
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 07/08/2019
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: bc09aa500743d608c0a3a7a379fe9584c9c55e9b
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657622"
---
# <a name="mount-an-azure-file-share-in-azure-container-instances"></a>Een Azure-bestandsshare in Azure Container Instances koppelen

Azure Container Instances zijn standaard staatloos. Als de container vastloopt of stopt, wordt alle van de staat van verloren gaan. Als u wilt behouden status, buiten de levensduur van de container, moet u een volume koppelen vanuit een externe opslag. Dit artikel wordt beschreven hoe u een Azure-bestandsshare gemaakt met koppelen [Azure Files](../storage/files/storage-files-introduction.md) voor gebruik met Azure Container Instances. Met Azure Files beschikt u over volledig beheerde bestandsshares in de cloud die toegankelijk zijn via het industriestandaard SMB-protocol (Server Message Block). Met behulp van een Azure-bestandsshare met Azure Container Instances biedt functies delen van bestanden die vergelijkbaar is met behulp van een Azure-bestandsshare met Azure virtual machines.

> [!NOTE]
> Een Azure-bestandsshare koppelen is momenteel beperkt tot Linux-containers. Terwijl we werken om alle functies op Windows-containers, vindt u de huidige platform verschillen in de [overzicht](container-instances-overview.md#linux-and-windows-containers).

## <a name="create-an-azure-file-share"></a>Een Azure-bestandsshare maken

Voordat u een Azure-bestandsshare gebruikt met Azure Container Instances, moet u deze maken. Voer het volgende script om een opslagaccount voor het hosten van de bestandsshare en de share zelf te maken. Naam van het opslagaccount moet globaal uniek zijn, zodat het script wordt een willekeurige waarde toegevoegd aan de tekenreeks met base.

```azurecli-interactive
# Change these four parameters as needed
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --location $ACI_PERS_LOCATION \
    --sku Standard_LRS

# Create the file share
az storage share create --name $ACI_PERS_SHARE_NAME --account-name $ACI_PERS_STORAGE_ACCOUNT_NAME
```

## <a name="get-storage-credentials"></a>Storage-referenties ophalen

Voor het koppelen van een Azure-bestandsshare als een volume in Azure Container Instances, moet u drie waarden: naam van het opslagaccount, de sharenaam en de toegangssleutel voor opslag.

Als u het bovenstaande script gebruikt, is de naam van opslagaccount in de $ACI_PERS_STORAGE_ACCOUNT_NAME variabele opgeslagen. Overzicht van de accountnaam, typt u:

```console
echo $ACI_PERS_STORAGE_ACCOUNT_NAME
```

De sharenaam is al bekend (gedefinieerd als *acishare* in het bovenstaande script), zodat alle dat blijft is de opslagaccountsleutel die kan worden gevonden met de volgende opdracht:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $ACI_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume---cli"></a>Container implementeren en te koppelen van de volume - CLI

Voor het koppelen van een Azure-bestandsshare als een volume in een container met behulp van de Azure CLI, geven de share en het volume koppelpunt bij het maken van de container met [az container maken][az-container-create]. Als u de vorige stappen hebt gevolgd, kunt u de share die u eerder hebt gemaakt met behulp van de volgende opdracht uit om een container te maken kunt koppelen:

```azurecli-interactive
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image mcr.microsoft.com/azuredocs/aci-hellofiles \
    --dns-name-label aci-demo \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

De `--dns-name-label` waarde moet uniek zijn binnen Azure-regio waarin u de containerinstantie maken. Werk de waarde in de voorgaande opdracht als u ontvangt een **DNS-naamlabel** foutbericht weergegeven wanneer u de opdracht uitvoert.

## <a name="manage-files-in-mounted-volume"></a>Beheren van bestanden in een gekoppeld volume

Zodra de container wordt gestart, kunt u de eenvoudige web-app geïmplementeerd via de Microsoft [aci-hellofiles][aci-hellofiles] image to create small text files in the Azure file share at the mount path you specified. Obtain the web app's fully qualified domain name (FQDN) with the [az container show][az-container-show] opdracht:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles --query ipAddress.fqdn --output tsv
```

Na het opslaan van tekst met behulp van de app, kunt u de [Azure-portal][portal] or a tool like the [Microsoft Azure Storage Explorer][storage-explorer] ophalen en inspecteren van het bestand geschreven naar de bestandsshare.

## <a name="deploy-container-and-mount-volume---yaml"></a>Container implementeren en volume - YAML koppelen

U kunt ook een containergroep implementeren en een volume in een container met de Azure CLI te koppelen en een [YAML-sjabloon](container-instances-multi-container-yaml.md). Implementeren met YAML-sjabloon is de aanbevolen methode bij het implementeren van groepen met containers die bestaat uit meerdere containers.

De volgende YAML-sjabloon definieert een containergroep met een container gemaakt met de `aci-hellofiles` installatiekopie. De container de Azure-bestandsshare koppelt *acishare* eerder hebt gemaakt als een volume. Indien vermeld, voert u de opslag en de sleutel voor het opslagaccount dat als host fungeert voor de bestandsshare. 

Zoals in het CLI-voorbeeld de `dnsNameLabel` waarde moet uniek zijn binnen Azure-regio waarin u de containerinstantie maken. Werk de waarde in het YAML-bestand, indien nodig.

```yaml
apiVersion: '2018-10-01'
location: eastus
name: file-share-demo
properties:
  containers:
  - name: hellofiles
    properties:
      environmentVariables: []
      image: mcr.microsoft.com/azuredocs/aci-hellofiles
      ports:
      - port: 80
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      volumeMounts:
      - mountPath: /aci/logs/
        name: filesharevolume
  osType: Linux
  restartPolicy: Always
  ipAddress:
    type: Public
    ports:
      - port: 80
    dnsNameLabel: aci-demo
  volumes:
  - name: filesharevolume
    azureFile:
      sharename: acishare
      storageAccountName: <Storage account name>
      storageAccountKey: <Storage account key>
tags: {}
type: Microsoft.ContainerInstance/containerGroups
```

Opslaan als wilt implementeren met behulp van de sjabloon YAML, de voorgaande YAML naar een bestand met de naam `deploy-aci.yaml`, voer de [az container maken][az-container-create] opdracht met de `--file` parameter:

```azurecli
# Deploy with YAML template
az container create --resource-group myResourceGroup --file deploy-aci.yaml
```
## <a name="deploy-container-and-mount-volume---resource-manager"></a>Container en koppelpunten volume - Resource Manager implementeren

Naast de CLI en YAML-implementatie, kunt u een containergroep implementeren en koppelen van een volume in een container met behulp van een Azure [Resource Manager-sjabloon](/azure/templates/microsoft.containerinstance/containergroups).

Eerst, vullen het `volumes` -matrix in de containergroep `properties` gedeelte van de sjabloon. 

Klik voor elke container waarin u wilt koppelen van het volume, vullen het `volumeMounts` matrix in de `properties` sectie van de containerdefinitie van de.

De volgende Resource Manager-sjabloon definieert een containergroep met een container gemaakt met de `aci-hellofiles` installatiekopie. De container de Azure-bestandsshare koppelt *acishare* eerder hebt gemaakt als een volume. Indien vermeld, voert u de opslag en de sleutel voor het opslagaccount dat als host fungeert voor de bestandsshare. 

Zoals in de voorgaande voorbeelden is de `dnsNameLabel` waarde moet uniek zijn binnen Azure-regio waarin u de containerinstantie maken. De waarde in de sjabloon bijwerken indien nodig.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "variables": {
    "container1name": "hellofiles",
    "container1image": "mcr.microsoft.com/azuredocs/aci-hellofiles"
  },
  "resources": [
    {
      "name": "file-share-demo",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-10-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                }
              ],
              "volumeMounts": [
                {
                  "name": "filesharevolume",
                  "mountPath": "/aci/logs"
                }
              ]
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            }
          ],
          "dnsNameLabel": "aci-demo"
        },
        "volumes": [
          {
            "name": "filesharevolume",
            "azureFile": {
                "shareName": "acishare",
                "storageAccountName": "<Storage account name>",
                "storageAccountKey": "<Storage account key>"
            }
          }
        ]
      }
    }
  ]
}
```

Opslaan als wilt implementeren met behulp van de Resource Manager-sjabloon, de eerder vermelde JSON naar een bestand met de naam `deploy-aci.json`, voer de [az group deployment maken][az-group-deployment-create] opdracht met de `--template-file` parameter:

```azurecli
# Deploy with Resource Manager template
az group deployment create --resource-group myResourceGroup --template-file deploy-aci.json
```


## <a name="mount-multiple-volumes"></a>Meerdere volumes koppelen

Voor het koppelen van meerdere volumes in een containerexemplaar, moet u implementeren met behulp van een [Azure Resource Manager-sjabloon](/azure/templates/microsoft.containerinstance/containergroups) of een YAML-bestand. Voor het gebruik van een sjabloon of een YAML-bestand, geef de sharedetails en de volumes definiëren door in te vullen de `volumes` matrix in de `properties` gedeelte van de sjabloon. 

Bijvoorbeeld, als u twee Azure-bestandsshares met de naam gemaakt *share1* en *share2* in storage-account *myStorageAccount*, wordt de `volumes` matrix in een Resource Manager sjabloon wordt weergegeven die vergelijkbaar is met het volgende:

```JSON
"volumes": [{
  "name": "myvolume1",
  "azureFile": {
    "shareName": "share1",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
},
{
  "name": "myvolume2",
  "azureFile": {
    "shareName": "share2",
    "storageAccountName": "myStorageAccount",
    "storageAccountKey": "<storage-account-key>"
  }
}]
```

Vervolgens voor elke container in containergroep waarin u wilt de volumes koppelen, vullen het `volumeMounts` matrix in de `properties` sectie van de containerdefinitie van de. Bijvoorbeeld, dit koppelt u de twee volumes *myvolume1* en *myvolume2*, die is eerder gedefinieerd:

```JSON
"volumeMounts": [{
  "name": "myvolume1",
  "mountPath": "/mnt/share1/"
},
{
  "name": "myvolume2",
  "mountPath": "/mnt/share2/"
}]
```

## <a name="next-steps"></a>Volgende stappen

Informatie over het koppelen van andere typen in Azure Container Instances:

* [Een volume emptyDir in Azure Containerexemplaren koppelen](container-instances-volume-emptydir.md)
* [Koppelen van een volume gitRepo in Azure Containerexemplaren](container-instances-volume-gitrepo.md)
* [Koppelen van een geheime volume in Azure Container Instances](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-hellofiles]: https://hub.docker.com/_/microsoft-azuredocs-aci-hellofiles 
[portal]: https://portal.azure.com
[storage-explorer]: https://storageexplorer.com

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create