---
title: Azure Service Fabric-infrastructuur als Code Best Practices | Microsoft Docs
description: Aanbevolen procedures voor het beheren van Service Fabric als infrastructuur als code.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: jeanpaul.connock
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: 23c851008988af06927d387daaa5a6df980dab96
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54913821"
---
# <a name="infrastructure-as-code"></a>Infrastructuur als code

Maak in een productiescenario voor, Azure Service Fabric-clusters met behulp van Resource Manager-sjablonen. Resource Manager-sjablonen bieden meer controle over de resource-eigenschappen en zorg ervoor dat u een consistent model hebt.

Voorbeeld van Resource Manager-sjablonen zijn beschikbaar voor Windows en Linux binnen de [Azure-voorbeelden op GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates). Deze sjablonen kunnen worden gebruikt als uitgangspunt voor de clustersjabloon voor het. Download `azuredeploy.json` en `azuredeploy.parameters.json` en bewerk ze zodat deze voldoen aan uw aangepaste vereisten.

Implementatie van de `azuredeploy.json` en `azuredeploy.parameters.json` sjablonen die u hebt gedownload, gebruikt u de volgende Azure CLI-opdrachten:

```azurecli
ResourceGroupName="sfclustergroup"
Location="westus"

az group create --name $ResourceGroupName --location $Location 
az group deployment create --name $ResourceGroupName  --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
```

Het maken van een resource met behulp van Powershell

```powershell
$ResourceGroupName="sfclustergroup"
$Location="westus"
$Template="azuredeploy.json"
$Parameters="azuredeploy.parameters.json"

New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
New-AzureRmResourceGroupDeployment -Name $ResourceGroupName -TemplateFile $Template -TemplateParameterFile $Parameters
```

## <a name="azure-service-fabric-resources"></a>Azure Service Fabric-resources

U kunt toepassingen en services implementeren naar uw Service Fabric-cluster via Azure Resource Manager. Zie [toepassingen en services als Azure Resource Manager-resources beheren](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-arm-resource) voor meer informatie. Hieronder vindt u een best practice Service Fabric-toepassing specifieke resources om op te nemen in uw sjabloonresources Resource Manager.

```json
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```

Voor het implementeren van uw toepassing met behulp van Azure Resource Manager, moet u eerst [maken van een sfpkg](https://docs.microsoft.com/azure/service-fabric/service-fabric-package-apps#create-an-sfpkg) Service Fabric-toepassingspakket. Het volgende python-script wordt een voorbeeld van het maken van een sfpkg:

```python
# Create SFPKG that needs to be uploaded to Azure Storage Blob Container
microservices_sfpkg = zipfile.ZipFile(self.microservices_app_package_name, 'w', zipfile.ZIP_DEFLATED)
package_length = len(self.microservices_app_package_path)

for root, dirs, files in os.walk(self.microservices_app_package_path):
    root_folder = root[package_length:]
    for file in files:
        microservices_sfpkg.write(os.path.join(root, file), os.path.join(root_folder, file))

microservices_sfpkg.close()
```

## <a name="next-steps"></a>Volgende stappen

* Een cluster maken op virtuele machines of op computers met Windows Server: [Service Fabric-cluster maken voor Windows Server](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* Een cluster maken op VM's of computers waarop Linux wordt uitgevoerd: [Een Linux-cluster maken](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)