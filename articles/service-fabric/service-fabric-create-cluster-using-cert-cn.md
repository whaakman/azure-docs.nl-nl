---
title: Maken van een Azure Service Fabric-cluster met behulp van de algemene certificaatnaam | Microsoft Docs
description: Informatie over het maken van een Service Fabric-cluster met behulp van de algemene naam van certificaat van een sjabloon.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2018
ms.author: ryanwi
ms.openlocfilehash: 8725dd1931b120b0369d0810fa49108a00c71e8e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34211062"
---
# <a name="deploy-a-service-fabric-cluster-that-uses-certificate-common-name-instead-of-thumbprint"></a>Een Service Fabric-cluster dat gebruik maakt van de algemene certificaatnaam in plaats van de vingerafdruk implementeren
Er zijn geen twee certificaten kunnen dezelfde vingerafdruk, waardoor de overschakeling van de cluster-certificaat of management moeilijk hebben. Meerdere certificaten kunnen echter hebben de dezelfde algemene naam of het onderwerp.  Een cluster met de algemene naam van het certificaat kunt u veel eenvoudiger beheer van certificaten. In dit artikel wordt beschreven hoe een Service Fabric-cluster voor het gebruik van de algemene naam van het certificaat in plaats van de vingerafdruk van het certificaat te implementeren.
 
## <a name="get-a-certificate"></a>Een certificaat ophalen
Eerst een certificaat ophalen bij een [certificeringsinstantie (CA)](https://wikipedia.org/wiki/Certificate_authority).  De algemene naam van het certificaat moet de hostnaam van het cluster.  Bijvoorbeeld 'myclustername.southcentralus.cloudapp.azure.com'.  

Voor testdoeleinden kan u een door CA ondertekend certificaat verkrijgen van een certificeringsinstantie gratis of openen.

> [!NOTE]
> Zelfondertekende certificaten, inclusief de gegenereerd bij het implementeren van een Service Fabric-cluster in de Azure portal, worden niet ondersteund.

## <a name="upload-the-certificate-to-a-key-vault"></a>Upload het certificaat naar een sleutelkluis
Een Service Fabric-cluster wordt in Azure worden geïmplementeerd op een virtuele-machineschaalset.  Upload het certificaat naar een sleutelkluis.  Wanneer het cluster implementeert, wordt het certificaat installeert op de virtuele-machineschaalset die op het cluster wordt uitgevoerd.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgroup"
$VaultName = "mykeyvault"
$certFilename = "C:\users\sfuser\myclustercert.pfx"
$certname = "myclustercert"
$Password  = "P@ssw0rd!123"

# Create new Resource Group 
New-AzureRmResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Create the new key vault
$newKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName -Location $region -EnabledForDeployment 
$resourceId = $newKeyVault.ResourceId 

# Add the certificate to the key vault.
$PasswordSec = ConvertTo-SecureString -String $Password -AsPlainText -Force
$KVSecret = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certName  -FilePath $certFilename -Password $PasswordSec

$CertificateThumbprint = $KVSecret.Thumbprint
$CertificateURL = $KVSecret.SecretId
$SourceVault = $resourceId
$CommName    = $KVSecret.Certificate.SubjectName.Name

Write-Host "CertificateThumbprint    :"  $CertificateThumbprint
Write-Host "CertificateURL           :"  $CertificateURL
Write-Host "SourceVault              :"  $SourceVault
Write-Host "Common Name              :"  $CommName    
```

## <a name="download-and-update-a-sample-template"></a>Downloaden en bijwerken van een voorbeeldsjabloon
Dit artikel wordt de [voorbeeld 5-knooppunt beveiligde cluster](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure) sjabloon en sjabloonparameters. Download de *azuredeploy.json* en *azuredeploy.parameters.json* bestanden op uw computer.

### <a name="update-parameters-file"></a>Update-parameterbestand
Open eerst de *azuredeploy.parameters.json* bestand in een teksteditor en voeg de waarde van de volgende parameter:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
```

Vervolgens stelt de *certificateCommonName*, *sourceVaultValue*, en *certificateUrlValue* parameterwaarden die wordt geretourneerd door de voorgaande script:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
"sourceVaultValue": {
  "value": "/subscriptions/<subscription>/resourceGroups/testvaultgroup/providers/Microsoft.KeyVault/vaults/testvault"
},
"certificateUrlValue": {
  "value": "https://testvault.vault.azure.net:443/secrets/testcert/5c882b7192224447bbaecd5a46962655"
},
```

### <a name="update-the-template-file"></a>Het sjabloonbestand bijwerken
Open vervolgens de *azuredeploy.json* -bestand in een teksteditor en drie updates voor de ondersteuning van de algemene certificaatnaam maken.

1. In de **parameters** sectie, het toevoegen van een *certificateCommonName* parameter:
    ```json
    "certificateCommonName": {
      "type": "string",
      "metadata": {
        "description": "Certificate Commonname"
      }
    },
    ```

    Houd ook rekening met verwijderen van de *certificateThumbprint*, mogelijk niet meer nodig.

2. Stel de waarde van de *sfrpApiVersion* variabele '2018-02-01':
    ```json
    "sfrpApiVersion": "2018-02-01",
    ```

3. In de **Microsoft.Compute/virtualMachineScaleSets** resource, werk de extensie van de virtuele machine voor het gebruik van de algemene naam in de certificaatinstellingen in plaats van de vingerafdruk van het.  In **virtualMachineProfile**->**extenstionProfile**->**extensies**->**eigenschappen** -> **instellingen**->**certificaat**, Voeg `"commonNames": ["[parameters('certificateCommonName')]"],` en verwijder `"thumbprint": "[parameters('certificateThumbprint')]",`.
    ```json
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              "type": "ServiceFabricNode",
              "autoUpgradeMinorVersion": true,
              "protectedSettings": {
                "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
              },
              "publisher": "Microsoft.Azure.ServiceFabric",
              "settings": {
                "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                "nodeTypeRef": "[variables('vmNodeType0Name')]",
                "dataPath": "D:\\SvcFab",
                "durabilityLevel": "Bronze",
                "enableParallelJobs": true,
                "nicPrefixOverride": "[variables('subnet0Prefix')]",
                "certificate": {
                  "commonNames": ["[parameters('certificateCommonName')]"],
                  "x509StoreName": "[parameters('certificateStoreValue')]"
                }
              },
              "typeHandlerVersion": "1.0"
            }
          },
    ```

4.  In de **Microsoft.ServiceFabric/clusters** resource, update de API-versie naar '2018-02-01'.  Ook toevoegen een **certificateCommonNames** instellen met een **commonNames** eigenschap en verwijder de **certificaat** (met de eigenschap thumbprint) zoals in het volgende instellen Voorbeeld:
    ```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
        ],
        "properties": {
        "addonFeatures": [
            "DnsService",
            "RepairManager"
        ],        
        "certificateCommonNames": {
            "commonNames": [
            {
                "certificateCommonName": "[parameters('certificateCommonName')]",
                "certificateIssuerThumbprint": ""
            }
            ],
            "x509StoreName": "[parameters('certificateStoreValue')]"
        },
        ...
    ```

## <a name="deploy-the-updated-template"></a>De bijgewerkte sjabloon implementeren
Implementeer de bijgewerkte sjabloon opnieuw nadat de wijzigingen zijn.

```powershell
# Variables.
$groupname = "testclustergroup"
$clusterloc="southcentralus"  
$id="<subscription ID"

# Sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $id 

# Create a new resource group and deploy the cluster.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\AzureDeploy.Parameters.json" -TemplateFile "C:\temp\cluster\AzureDeploy.json" -Verbose
```

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [beveiliging cluster](service-fabric-cluster-security.md).
* Meer informatie over hoe [rollover van een cluster-certificaat](service-fabric-cluster-rollover-cert-cn.md)
* [Bijwerken en beheren van clustercertificaten](service-fabric-cluster-security-update-certs-azure.md)

[image1]: .\media\service-fabric-cluster-change-cert-thumbprint-to-cn\PortalViewTemplates.png