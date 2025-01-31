---
title: Azure Service Fabric-infrastructuur als Code Best Practices | Microsoft Docs
description: Aanbevolen procedures voor het beheren van Service Fabric als infrastructuur als code.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: 2dfe1493c6611fb69a417895aaa1028ad5881b9c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66237421"
---
# <a name="infrastructure-as-code"></a>Infrastructure als code

Maak in een productiescenario voor, Azure Service Fabric-clusters met behulp van Resource Manager-sjablonen. Resource Manager-sjablonen bieden meer controle over de resource-eigenschappen en zorg ervoor dat u een consistent model hebt.

Voorbeeld van Resource Manager-sjablonen zijn beschikbaar voor Windows en Linux binnen de [Azure-voorbeelden op GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates). Deze sjablonen kunnen worden gebruikt als uitgangspunt voor de clustersjabloon voor het. Download `azuredeploy.json` en `azuredeploy.parameters.json` en bewerk ze zodat deze voldoen aan uw aangepaste vereisten.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

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

New-AzResourceGroup -Name $ResourceGroupName -Location $Location
New-AzResourceGroupDeployment -Name $ResourceGroupName -TemplateFile $Template -TemplateParameterFile $Parameters
```

## <a name="azure-service-fabric-resources"></a>Azure Service Fabric-resources

U kunt toepassingen en services implementeren in uw Service Fabric-cluster via Azure Resource Manager. Zie [toepassingen en services als Azure Resource Manager-resources beheren](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-arm-resource) voor meer informatie. Hieronder vindt u een best practice Service Fabric-toepassing specifieke resources om op te nemen in uw sjabloonresources Resource Manager.

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

## <a name="azure-virtual-machine-operating-system-automatic-upgrade-configuration"></a>Virtuele Azure-Machine automatische Upgrade configuratie van besturingssysteem 
Upgraden van uw virtuele machines is een bewerking door de gebruiker gestart en wordt aanbevolen dat u [Virtual Machine Scale Stel automatische upgrade van besturingssysteem](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) voor Azure Service Fabric-clusters die host patchbeheer; Patch Orchestration-toepassing is een alternatieve oplossing die is bedoeld voor wanneer deze wordt gehost buiten Azure, hoewel POA kan worden gebruikt in Azure, met extra overhead van die als host fungeert POA in Azure wordt een algemene redenen voor de keuze van de virtuele Machine voor het automatische Upgrade van besturingssysteem via POA. Hier volgen de sjablooneigenschappen Virtual Machine Scale ingesteld Resource-Manager Compute bijwerken van het besturingssysteem automatisch inschakelen:

```json
"upgradePolicy": {
   "mode": "Automatic",
   "automaticOSUpgradePolicy": {
        "enableAutomaticOSUpgrade": true,
        "disableAutomaticRollback": false
    }
},
```
Als u automatische Besturingssysteemupgrades voor Service Fabric, wordt de nieuwe installatiekopie van het besturingssysteem geïmplementeerd één Updatedomein tegelijk voor hoge beschikbaarheid van de services die worden uitgevoerd in Service Fabric. Uw cluster moet voor het gebruik van automatische Besturingssysteemupgrades in Service Fabric worden geconfigureerd om te gebruiken de Duurzaamheidslaag Silver- of hoger.

Zorg ervoor dat de volgende registersleutel is ingesteld op false om te voorkomen dat uw windows-hostmachines ongecoördineerde updates wordt gestart: HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU.

Hier volgen de Virtual Machine Scale ingesteld Resource-Manager Compute Sjablooneigenschappen de Windows Update-registersleutel instellen op false:
```json
"osProfile": {
        "computerNamePrefix": "{vmss-name}",
        "adminUsername": "{your-username}",
        "secrets": [],
        "windowsConfiguration": {
          "provisionVMAgent": true,
          "enableAutomaticUpdates": false
        }
      },
```

## <a name="azure-service-fabric-cluster-upgrade-configuration"></a>Configuratie van Azure Service Fabric-Cluster upgraden
Hier volgt de Service Fabric-cluster Resource Manager-sjablooneigenschap Automatische upgrade inschakelen:
```json
"upgradeMode": "Automatic",
```
Uw cluster handmatig bijwerken, de distributie van het CAB-bestand/deb downloaden naar een cluster virtuele machine en vervolgens aanroepen van de volgende PowerShell:
```powershell
Copy-ServiceFabricClusterPackage -Code -CodePackagePath <"local_VM_path_to_msi"> -CodePackagePathInImageStore ServiceFabric.msi -ImageStoreConnectionString "fabric:ImageStore"
Register-ServiceFabricClusterPackage -Code -CodePackagePath "ServiceFabric.msi"
Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <"msi_code_version">
```

## <a name="next-steps"></a>Volgende stappen

* Een cluster maken op virtuele machines of op computers met Windows Server: [Service Fabric-cluster maken voor Windows Server](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* Een cluster maken op VM's of computers waarop Linux wordt uitgevoerd: [Een Linux-cluster maken](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md)
