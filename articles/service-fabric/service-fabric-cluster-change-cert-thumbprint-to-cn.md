---
title: Een Azure Service Fabric-cluster voor het gebruik van de algemene certificaatnaam bijwerken | Microsoft Docs
description: Informatie over het wijzigen van een Service Fabric-cluster certificaatvingerafdrukken voor het gebruik van de algemene certificaatnaam te gebruiken.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: aljo
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2018
ms.author: ryanwi
ms.openlocfilehash: 39dc5800edd743cdc950c7a96f7633fb4c0a7c45
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="change-cluster-from-certificate-thumbprint-to-common-name"></a>Cluster niet wijzigen van de vingerafdruk van certificaat in de algemene naam
Er zijn geen twee certificaten kunnen dezelfde vingerafdruk, waardoor de overschakeling van de cluster-certificaat of management moeilijk hebben. Meerdere certificaten kunnen echter hebben de dezelfde algemene naam of het onderwerp.  Schakelen tussen een geïmplementeerde cluster certificaatvingerafdrukken voor het gebruik van de algemene naam van het certificaat te gebruiken, maakt Certificaatbeheer veel eenvoudiger. In dit artikel wordt beschreven hoe een actieve Service Fabric-cluster voor het gebruik van de algemene naam van het certificaat in plaats van de vingerafdruk van het certificaat bijwerken.
 
## <a name="get-a-certificate"></a>Een certificaat ophalen
Eerst een certificaat ophalen bij een [certificeringsinstantie (CA)](https://wikipedia.org/wiki/Certificate_authority).  De algemene naam van het certificaat moet de hostnaam van het cluster.  Bijvoorbeeld 'myclustername.southcentralus.cloudapp.azure.com'.  

Voor testdoeleinden kan u een door CA ondertekend certificaat verkrijgen van een certificeringsinstantie gratis of openen.

> [!NOTE]
> Zelfondertekende certificaten, inclusief de gegenereerd bij het implementeren van een Service Fabric-cluster in de Azure portal, worden niet ondersteund.

## <a name="upload-the-certificate-and-install-it-in-the-scale-set"></a>Upload het certificaat en installeer het in de schaalset
Een Service Fabric-cluster wordt in Azure worden geïmplementeerd op een virtuele-machineschaalset.  Upload het certificaat naar een sleutelkluis en installeer deze vervolgens op de virtuele-machineschaalset die op het cluster wordt uitgevoerd.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgropu"
$VaultName = "mykeyvault"
$certFilename = "C:\users\sfuser\myclustercert.pfx"
$certname = "myclustercert"
$Password  = "P@ssw0rd!123"
$VmssResourceGroupName     = "myclustergroup"
$VmssName                  = "prnninnxj"

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

Set-StrictMode -Version 3
$ErrorActionPreference = "Stop"

$certConfig = New-AzureRmVmssVaultCertificateConfig -CertificateUrl $CertificateURL -CertificateStore "My"

# Get current VM scale set 
$vmss = Get-AzureRmVmss -ResourceGroupName $VmssResourceGroupName -VMScaleSetName $VmssName

# Add new secret to the VM scale set.
$vmss = Add-AzureRmVmssSecret -VirtualMachineScaleSet $vmss -SourceVaultId $SourceVault -VaultCertificate $certConfig

# Update the VM scale set 
Update-AzureRmVmss -ResourceGroupName $VmssResourceGroupName -Name $VmssName -VirtualMachineScaleSet $vmss  -Verbose
```

## <a name="download-and-update-the-template-from-the-portal"></a>Downloaden en de sjabloon van de portal bijwerken
Het certificaat is geïnstalleerd op de onderliggende schaalset, maar u moet ook bijwerken van de Service Fabric-cluster als dat certificaat en de algemene naam wilt gebruiken.  De sjabloon voor de Clusterimplementatie nu downloaden.  Meld u aan bij de [Azure-portal](https://portal.azure.com) en navigeer naar de resourcegroep die als host fungeert voor het cluster.  In **instellingen**, selecteer **implementaties**.  Selecteer de meest recente implementatie en klik op **sjabloon weergeven**.

![Sjablonen weergeven][image1]

Download de sjabloon en parameters JSON-bestanden op uw lokale computer.

Eerst open het parameterbestand in een teksteditor en voeg de waarde van de volgende parameter:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
```

Vervolgens het sjabloonbestand openen in een teksteditor en drie updates voor de ondersteuning van de algemene certificaatnaam maken.

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

2. In de **Microsoft.Compute/virtualMachineScaleSets** resource, werk de extensie van de virtuele machine voor het gebruik van de algemene naam in de certificaatinstellingen in plaats van de vingerafdruk van het.  In **virtualMachineProfile**->**extenstionProfile**->**extensies**->**eigenschappen** -> **instellingen**->**certificaat**, Voeg `"commonNames": ["[parameters('certificateCommonName')]"],` en verwijder `"thumbprint": "[parameters('certificateThumbprint')]",`.
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

3.  In de **Microsoft.ServiceFabric/clusters** resource, update de API-versie naar '2018-02-01'.  Ook toevoegen een **certificateCommonNames** instellen met een **commonNames** eigenschap en verwijder de **certificaat** (met de eigenschap thumbprint) zoals in het volgende instellen Voorbeeld:
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
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"

New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\parameters.json" -TemplateFile "C:\temp\cluster\template.json" -Verbose
```

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [beveiliging cluster](service-fabric-cluster-security.md).
* Meer informatie over hoe [rollover van een cluster-certificaat](service-fabric-cluster-rollover-cert-cn.md)
* [Bijwerken en beheren van clustercertificaten](service-fabric-cluster-security-update-certs-azure.md)

[image1]: .\media\service-fabric-cluster-change-cert-thumbprint-to-cn\PortalViewTemplates.png