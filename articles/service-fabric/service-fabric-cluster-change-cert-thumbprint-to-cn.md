---
title: Een Azure Service Fabric-cluster voor het gebruik van de algemene certificaatnaam bijwerken | Microsoft Docs
description: Meer informatie over het overschakelen op een Service Fabric-cluster van het gebruik van vingerafdrukken van het certificaat voor het gebruik van de algemene certificaatnaam.
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
ms.date: 01/01/2019
ms.author: ryanwi
ms.openlocfilehash: 9c1f8507cfa1f21214428e852e6ffed4d7703254
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55564320"
---
# <a name="change-cluster-from-certificate-thumbprint-to-common-name"></a>Cluster niet wijzigen van de vingerafdruk van certificaat in de algemene naam
Er zijn geen twee certificaten kunnen hebben dezelfde vingerafdruk, waardoor certificaatrollover cluster of de beheer-moeilijk. Meerdere certificaten kunnen echter hebben de dezelfde algemene naam of het onderwerp.  Schakelen tussen een geïmplementeerd cluster vanuit vingerafdrukken van het certificaat voor het gebruik van de algemene naam van het certificaat met maakt het beheren van certificaten het veel eenvoudiger. In dit artikel wordt beschreven hoe u een actief Service Fabric-cluster voor het gebruik van de algemene naam van het certificaat in plaats van de vingerafdruk van het certificaat bijwerken.

>[!NOTE]
> Als u twee vingerafdruk gedeclareerd in de sjabloon hebt, moet u twee implementaties uitvoeren.  De eerste implementatie wordt uitgevoerd voordat u de stappen in dit artikel te volgen.  Hiermee stelt u de eerste implementatie uw **vingerafdruk** eigenschap in de sjabloon voor het certificaat dat wordt gebruikt en verwijdert de **thumbprintSecondary** eigenschap.  Volg de stappen in dit artikel voor de tweede implementatie.
 
## <a name="get-a-certificate"></a>Een certificaat ophalen
Haal eerst een certificaat van een [certificeringsinstantie (CA)](https://wikipedia.org/wiki/Certificate_authority).  De algemene naam van het certificaat moet de naam van de host van het cluster.  Bijvoorbeeld, "myclustername.southcentralus.cloudapp.azure.com'.  

Voor testdoeleinden kan u een door CA ondertekend certificaat van een gratis of open certificeringsinstantie ophalen.

> [!NOTE]
> Zelfondertekende certificaten, met inbegrip van die gegenereerd bij het implementeren van een Service Fabric-cluster in Azure portal, worden niet ondersteund.

## <a name="upload-the-certificate-and-install-it-in-the-scale-set"></a>Upload het certificaat en installeer het in de schaalset
Op een virtuele-machineschaalset is een Service Fabric-cluster in Azure geïmplementeerd.  Upload het certificaat naar een key vault en installeer dit op de virtuele-machineschaalset die op het cluster wordt uitgevoerd.

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
$VmssResourceGroupName     = "myclustergroup"
$VmssName                  = "prnninnxj"

# Create new Resource Group 
New-AzureRmResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Create the new key vault
$newKeyVault = New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName `
    -Location $region -EnabledForDeployment 
$resourceId = $newKeyVault.ResourceId 

# Add the certificate to the key vault.
$PasswordSec = ConvertTo-SecureString -String $Password -AsPlainText -Force
$KVSecret = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certName `
    -FilePath $certFilename -Password $PasswordSec

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
$vmss = Add-AzureRmVmssSecret -VirtualMachineScaleSet $vmss -SourceVaultId $SourceVault `
    -VaultCertificate $certConfig

# Update the VM scale set 
Update-AzureRmVmss -ResourceGroupName $VmssResourceGroupName -Verbose `
    -Name $VmssName -VirtualMachineScaleSet $vmss 
```

>[!NOTE]
> Virtual Machine Scale ingesteld geheimen bieden geen ondersteuning voor dezelfde resource-id voor twee afzonderlijke geheimen, berekent zoals elke geheim een samengestelde unieke resource is. 

## <a name="download-and-update-the-template-from-the-portal"></a>Downloaden en de sjabloon van de portal bijwerken
Het certificaat is geïnstalleerd op de onderliggende schaalset, maar u moet ook de Service Fabric-cluster voor het gebruik van dat certificaat en de algemene naam bijwerken.  Download nu de sjabloon voor de implementatie van uw cluster.  Meld u aan bij de [Azure-portal](https://portal.azure.com) en navigeer naar de resourcegroep die als host fungeert voor het cluster.  In **instellingen**, selecteer **implementaties**.  Selecteer de meest recente implementatie en klikt u op **sjabloon weergeven**.

![Sjablonen weergeven][image1]

De sjabloon en parameters JSON-bestanden downloaden naar uw lokale computer.

Eerst, open het parameterbestand in een teksteditor en voeg de waarde van de volgende parameter toe:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
```

Open het sjabloonbestand in een teksteditor en drie updates voor de ondersteuning van de algemene certificaatnaam maken.

1. In de **parameters** sectie, voegt een *certificateCommonName* parameter:
    ```json
    "certificateCommonName": {
        "type": "string",
        "metadata": {
            "description": "Certificate Commonname"
        }
    },
    ```

    Ook kunt u verwijderen de *certificateThumbprint*, het is mogelijk niet meer vereist.

2. In de **Microsoft.Compute/virtualMachineScaleSets** resource, de extensie van de virtuele machine voor het gebruik van de algemene naam in de instellingen van het certificaat in plaats van de vingerafdruk van het bijwerken.  In **virtualMachineProfile**->**extensionProfile**->**extensies**->**eigenschappen** -> **instellingen**->**certificaat**, toevoegen `"commonNames": ["[parameters('certificateCommonName')]"],` en verwijder `"thumbprint": "[parameters('certificateThumbprint')]",`.
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
                                "commonNames": [
                                    "[parameters('certificateCommonName')]"
                                ],
                                "x509StoreName": "[parameters('certificateStoreValue')]"
                            }
                        },
                        "typeHandlerVersion": "1.0"
                    }
                },
    ```

3.  In de **Microsoft.ServiceFabric/clusters** resource, update de API-versie naar '2018-02-01'.  Ook toevoegen een **certificateCommonNames** instellen met een **commonNames** eigenschap en verwijder de **certificaat** (met de eigenschap vingerafdruk) zoals in het volgende instellen Voorbeeld:
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
Implementeer de bijgewerkte sjabloon opnieuw nadat de wijzigingen hebt aangebracht.

```powershell
$groupname = "sfclustertutorialgroup"

New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -Verbose `
    -TemplateParameterFile "C:\temp\cluster\parameters.json" -TemplateFile "C:\temp\cluster\template.json" 
```

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [cluster security](service-fabric-cluster-security.md).
* Meer informatie over het [rollover een clustercertificaat](service-fabric-cluster-rollover-cert-cn.md)
* [Bijwerken en clustercertificaten beheren](service-fabric-cluster-security-update-certs-azure.md)

[image1]: ./media/service-fabric-cluster-change-cert-thumbprint-to-cn/PortalViewTemplates.png
