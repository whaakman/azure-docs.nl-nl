---
title: Maken van een Azure Service Fabric-cluster met behulp van de algemene certificaatnaam | Microsoft Docs
description: Informatie over het maken van een Service Fabric-cluster met behulp van de algemene certificaatnaam van een sjabloon.
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
ms.openlocfilehash: 78812f7bcce82090802672e3e232e713f0d047d1
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214110"
---
# <a name="deploy-a-service-fabric-cluster-that-uses-certificate-common-name-instead-of-thumbprint"></a>Implementeren van een Service Fabric-cluster dat gebruik maakt van de algemene certificaatnaam in plaats van vingerafdruk
Er zijn geen twee certificaten kunnen hebben dezelfde vingerafdruk, waardoor certificaatrollover cluster of de beheer-moeilijk. Meerdere certificaten kunnen echter hebben de dezelfde algemene naam of het onderwerp.  Een cluster met behulp van algemene naam van het certificaat maakt het veel eenvoudiger Certificaatbeheer. In dit artikel wordt beschreven hoe u een Service Fabric-cluster voor het gebruik van de algemene naam van het certificaat in plaats van de vingerafdruk van het certificaat te implementeren.
 
## <a name="get-a-certificate"></a>Een certificaat ophalen
Haal eerst een certificaat van een [certificeringsinstantie (CA)](https://wikipedia.org/wiki/Certificate_authority).  De algemene naam van het certificaat moet zijn voor het aangepaste domein dat u zelf, en hebben gekocht van een domeinregistrar. Bijvoorbeeld, "azureservicefabricbestpractices.com"; personen die niet Microsoft-medewerkers zijn kunnen certificaten voor MS domeinen, niet inrichten, zodat u de DNS-namen van uw LB of Traffic Manager niet als algemene namen voor uw certificaat gebruiken kunt, en moet u voor het inrichten van een [Azure DNS-Zone](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns) als uw aangepaste domein te kunnen worden omgezet in Azure. Ook wilt declareren van uw aangepaste domein dat u de eigenaar als uw cluster 'managementEndpoint' bent als u wilt dat de portal in overeenstemming met het aangepaste domein alias voor uw cluster.

Voor testdoeleinden kan u een door CA ondertekend certificaat van een gratis of open certificeringsinstantie ophalen.

> [!NOTE]
> Zelfondertekende certificaten, met inbegrip van die gegenereerd bij het implementeren van een Service Fabric-cluster in Azure portal, worden niet ondersteund. 

## <a name="upload-the-certificate-to-a-key-vault"></a>Upload het certificaat naar een key vault
Op een virtuele-machineschaalset is een Service Fabric-cluster in Azure geïmplementeerd.  Upload het certificaat naar een sleutelkluis.  Wanneer het cluster is geïmplementeerd, wordt het certificaat geïnstalleerd op de virtuele-machineschaalset die het cluster wordt uitgevoerd op.

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
In dit artikel wordt de [5-knooppunten beveiligd cluster voorbeeld](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure) sjabloon en sjabloonparameters. Download de *azuredeploy.json* en *azuredeploy.parameters.json* bestanden op uw computer.

### <a name="update-parameters-file"></a>Parameterbestand bijwerken
Open eerst de *azuredeploy.parameters.json* bestand in een teksteditor en voeg de waarde van de volgende parameter toe:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
```

Vervolgens stelt de *certificateCommonName*, *sourceVaultValue*, en *certificateUrlValue* parameterwaarden die geretourneerd door het vorige script:
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

2. Stel de waarde van de *sfrpApiVersion* variabele '2018-02-01':
    ```json
    "sfrpApiVersion": "2018-02-01",
    ```

3. In de **Microsoft.Compute/virtualMachineScaleSets** resource, de extensie van de virtuele machine voor het gebruik van de algemene naam in de instellingen van het certificaat in plaats van de vingerafdruk van het bijwerken.  In **virtualMachineProfile**->**extensionProfile**->**extensies**->**eigenschappen** -> **instellingen**->**certificaat**, toevoegen 
    ```json
       "commonNames": [
        "[parameters('certificateCommonName')]"
       ],
    ```

    en verwijder `"thumbprint": "[parameters('certificateThumbprint')]",`.

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

4.  In de **Microsoft.ServiceFabric/clusters** resource, update de API-versie naar '2018-02-01'.  Ook toevoegen een **certificateCommonNames** instellen met een **commonNames** eigenschap en verwijder de **certificaat** (met de eigenschap vingerafdruk) zoals in het volgende instellen Voorbeeld:
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
* Meer informatie over [cluster security](service-fabric-cluster-security.md).
* Meer informatie over het [rollover een clustercertificaat](service-fabric-cluster-rollover-cert-cn.md)
* [Bijwerken en clustercertificaten beheren](service-fabric-cluster-security-update-certs-azure.md)
* Vereenvoudig het beheer van het certificaat door [wijzigen-cluster op basis van de vingerafdruk van certificaat in een algemene naam](service-fabric-cluster-change-cert-thumbprint-to-cn.md)

[image1]: .\media\service-fabric-cluster-change-cert-thumbprint-to-cn\PortalViewTemplates.png
