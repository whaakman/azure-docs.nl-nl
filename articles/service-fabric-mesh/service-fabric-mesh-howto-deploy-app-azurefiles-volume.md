---
title: Een volume op basis van Azure-bestanden gebruiken in een Service Fabric-NET-toepassing | Microsoft Docs
description: Informatie over het status opslaan in een Azure Service Fabric NET toepassing door te koppelen van een volume op basis van Azure-bestanden in een service met de Azure CLI.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: chakdan
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: 69866b9235f907b9e70a339d35372a8dca8ec175
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56804515"
---
# <a name="mount-an-azure-files-based-volume-in-a-service-fabric-mesh-application"></a>Een volume op basis van Azure-bestanden in een Service Fabric-NET-toepassing koppelen 

Dit artikel wordt beschreven hoe u een volume op basis van Azure-bestanden in een service van een Service Fabric-NET-toepassing te koppelen.  De Azure Files volume stuurprogramma is een Docker volume gebruikt voor het koppelen van een Azure-bestandsshare naar een container, die u gebruiken om vast te leggen de status van service. Volumes bieden u de opslag voor algemeen gebruik en kunnen u bestanden met behulp van API's van een normale schijf i/o-bestand voor lezen/schrijven.  Lees voor meer informatie over volumes en opties voor het opslaan van toepassingsgegevens [status opslaan](service-fabric-mesh-storing-state.md).

Voor het koppelen van een volume in een service, een volumeresource in uw Service Fabric-NET-toepassing maken en deze vervolgens dat volume in uw service.  De volumeresource declareren en ernaar wordt verwezen in de bron van de service kunnen worden gedaan de [op basis van een YAML-resourcebestanden](#declare-a-volume-resource-and-update-the-service-resource-yaml) of de [sjabloon voor de implementatie op basis van JSON](#declare-a-volume-resource-and-update-the-service-resource-json). Voordat u het volume koppelen, moet u eerst een Azure storage-account maken en een [bestandsshare in Azure Files](/azure/storage/files/storage-how-to-create-file-share).

## <a name="prerequisites"></a>Vereisten

U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken om te voltooien in dit artikel. 

Zorg ervoor dat u de Azure CLI lokaal gebruikt met dit artikel, `az --version` ten minste retourneert `azure-cli (2.0.43)`.  De module Azure Service Fabric-NET CLI-extensie Volg hiervoor de volgende installeren (of bijwerken) [instructies](service-fabric-mesh-howto-setup-cli.md).

Aanmelden bij Azure en het instellen van uw abonnement:

```azurecli
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-a-storage-account-and-file-share-optional"></a>Maak een storage-account en een bestandsshare (optioneel)
Koppelen van een Azure Files-volume vereist een storage-account en de bestandsshare.  U kunt een bestaand Azure storage-account en bestandsshare gebruiken of maken van resources:

```azurecli-interactive
az group create --name myResourceGroup --location eastus

az storage account create --name myStorageAccount --resource-group myResourceGroup --location eastus --sku Standard_LRS --kind StorageV2

$current_env_conn_string=$(az storage account show-connection-string -n myStorageAccount -g myResourceGroup --query 'connectionString' -o tsv)

az storage share create --name myshare --quota 2048 --connection-string $current_env_conn_string
```

## <a name="get-the-storage-account-name-and-key-and-the-file-share-name"></a>Naam van het opslagaccount en sleutel en de naam van bestandsshare ophalen
Naam van het opslagaccount, sleutel van het opslagaccount en de naam van bestandsshare wordt verwezen als `<storageAccountName>`, `<storageAccountKey>`, en `<fileShareName>` in de volgende secties. 

Lijst met uw storage-accounts maken en de naam van het opslagaccount met de bestandsshare die u wilt gebruiken:
```azurecli-interactive
az storage account list
```

Haal de naam van de bestandsshare:
```azurecli-interactive
az storage share list --account-name <storageAccountName>
```

Haal de opslagaccountsleutel ("key1"):
```azurecli-interactive
az storage account keys list --account-name <storageAccountName> --query "[?keyName=='key1'].value"
```

U vindt hier ook deze waarden in de [Azure-portal](https://portal.azure.com):
* `<storageAccountName>` -Onder **Opslagaccounts**, de naam van het opslagaccount dat wordt gebruikt om de bestandsshare te maken.
* `<storageAccountKey>` -Selecteer uw opslagaccount onder **Opslagaccounts** en selecteer vervolgens **toegangssleutels** en gebruikt u de waarde onder **key1**.
* `<fileShareName>` -Selecteer uw opslagaccount onder **Opslagaccounts** en selecteer vervolgens **bestanden**. De naam moet worden gebruikt, is de naam van de bestandsshare die u hebt gemaakt.

## <a name="declare-a-volume-resource-and-update-the-service-resource-json"></a>Declareer een volumeresource en bijwerken van de Serviceresource (JSON)

Voeg parameters voor de `<fileShareName>`, `<storageAccountName>`, en `<storageAccountKey>` waarden die u in de vorige stap hebt gevonden. 

Een volumeresource maken als een peer van de bron van de toepassing. Geef een naam en de provider ('SFAzureFile' te gebruiken van het volume op basis van Azure-bestanden). In `azureFileParameters`, geef de parameters op voor de `<fileShareName>`, `<storageAccountName>`, en `<storageAccountKey>` waarden die u in de vorige stap hebt gevonden.

Voor het koppelen van het volume in uw service, Voeg een `volumeRefs` naar de `codePackages` element van de service.  `name` de resource-ID voor het volume (of een sjabloonparameter implementatie voor de volumeresource) is en de naam van het volume in het bestand van de resource volume.yaml gedeclareerd.  `destinationPath` is de lokale map waarin het volume wordt gekoppeld aan.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "defaultValue": "EastUS",
      "type": "String",
      "metadata": {
        "description": "Location of the resources."
      }
    },
    "fileShareName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Azure Files file share that provides the volume for the container."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Azure storage account that contains the file share."
      }
    },
    "storageAccountKey": {
      "type": "securestring",
      "metadata": {
        "description": "Access key for the Azure storage account that contains the file share."
      }
    },
    "stateFolderName": {
      "type": "string",
      "defaultValue": "TestVolumeData",
      "metadata": {
        "description": "Folder in which to store the state. Provide a empty value to create a unique folder for each container to store the state. A non-empty value will retain the state across deployments, however if more than one applications are using the same folder, the counter may update more frequently."
      }
    }
  },
  "resources": [
    {
      "apiVersion": "2018-09-01-preview",
      "name": "VolumeTest",
      "type": "Microsoft.ServiceFabricMesh/applications",
      "location": "[parameters('location')]",
      "dependsOn": [
        "Microsoft.ServiceFabricMesh/networks/VolumeTestNetwork",
        "Microsoft.ServiceFabricMesh/volumes/testVolume"
      ],
      "properties": {
        "services": [
          {
            "name": "VolumeTestService",
            "properties": {
              "description": "VolumeTestService description.",
              "osType": "Windows",
              "codePackages": [
                {
                  "name": "VolumeTestService",
                  "image": "volumetestservice:dev",
                  "volumeRefs": [
                    {
                      "name": "[resourceId('Microsoft.ServiceFabricMesh/volumes', 'testVolume')]",
                      "destinationPath": "C:\\app\\data"
                    }
                  ],
                  "environmentVariables": [
                    {
                      "name": "ASPNETCORE_URLS",
                      "value": "http://+:20003"
                    },
                    {
                      "name": "STATE_FOLDER_NAME",
                      "value": "[parameters('stateFolderName')]"
                    }
                  ],
                  ...
                }
              ],
              ...
            }
          }
        ],
        "description": "VolumeTest description."
      }
    },
    {
      "apiVersion": "2018-09-01-preview",
      "name": "testVolume",
      "type": "Microsoft.ServiceFabricMesh/volumes",
      "location": "[parameters('location')]",
      "dependsOn": [],
      "properties": {
        "description": "Azure Files storage volume for the test application.",
        "provider": "SFAzureFile",
        "azureFileParameters": {
          "shareName": "[parameters('fileShareName')]",
          "accountName": "[parameters('storageAccountName')]",
          "accountKey": "[parameters('storageAccountKey')]"
        }
      }
    }
    ...
  ]
}
```

## <a name="declare-a-volume-resource-and-update-the-service-resource-yaml"></a>Declareer een volumeresource en bijwerken van de Serviceresource (YAML)

Toevoegen van een nieuwe *volume.yaml* -bestand in de *App-resources* Active directory voor uw toepassing.  Geef een naam en de provider ('SFAzureFile' te gebruiken van het volume op basis van Azure-bestanden). `<fileShareName>`, `<storageAccountName>`, en `<storageAccountKey>` zijn de waarden die u in de vorige stap hebt gevonden.

```yaml
volume:
  schemaVersion: 1.0.0-preview2
  name: testVolume
  properties:
    description: Azure Files storage volume for counter App.
    provider: SFAzureFile
    azureFileParameters: 
        shareName: <fileShareName>
        accountName: <storageAccountName>
        accountKey: <storageAccountKey>
```

Update de *service.yaml* -bestand in de *serviceresources* directory voor het koppelen van het volume in uw service.  Voeg de `volumeRefs` element op de `codePackages` element.  `name` de resource-ID voor het volume (of een sjabloonparameter implementatie voor de volumeresource) is en de naam van het volume in het bestand van de resource volume.yaml gedeclareerd.  `destinationPath` is de lokale map waarin het volume wordt gekoppeld aan.

```yaml
## Service definition ##
application:
  schemaVersion: 1.0.0-preview2
  name: VolumeTest
  properties:
    services:
      - name: VolumeTestService
        properties:
          description: VolumeTestService description.
          osType: Windows
          codePackages:
            - name: VolumeTestService
              image: volumetestservice:dev
              volumeRefs:
                - name: "[resourceId('Microsoft.ServiceFabricMesh/volumes', 'testVolume')]"
                  destinationPath: C:\app\data
              endpoints:
                - name: VolumeTestServiceListener
                  port: 20003
              environmentVariables:
                - name: ASPNETCORE_URLS
                  value: http://+:20003
                - name: STATE_FOLDER_NAME
                  value: TestVolumeData
              resources:
                requests:
                  cpu: 0.5
                  memoryInGB: 1
          replicaCount: 1
          networkRefs:
            - name: VolumeTestNetwork
```

## <a name="next-steps"></a>Volgende stappen

- De voorbeeldtoepassing voor Azure Files-volume bekijken op [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter).
- Zie [Inleiding tot Service Fabric Resource-model](service-fabric-mesh-service-fabric-resources.md) voor meer informatie over het Service Fabric Resource-model.
- Lees [Wat is Service Fabric Mesh?](service-fabric-mesh-overview.md) voor meer informatie over Service Fabric Mesh.
