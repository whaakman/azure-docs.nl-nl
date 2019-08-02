---
title: Een Azure Service Fabric-cluster certificaat overkantelen | Microsoft Docs
description: Meer informatie over het overschakelen van een Service Fabric cluster certificaat dat wordt geïdentificeerd door de algemene naam van het certificaat.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2018
ms.author: atsenthi
ms.openlocfilehash: 5d11054ca8eb684f1f25a25ddeac1b53e82b3775
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68599917"
---
# <a name="manually-roll-over-a-service-fabric-cluster-certificate"></a>Hand matig overschakelen op een Service Fabric cluster certificaat
Wanneer een Service Fabric cluster certificaat bijna verloopt, moet u het certificaat bijwerken.  Certificaat overschakeling is eenvoudig als het cluster is ingesteld op het [gebruik van certificaten op basis van algemene naam](service-fabric-cluster-change-cert-thumbprint-to-cn.md) (in plaats van een vinger afdruk).  Vraag een nieuw certificaat aan bij een certificerings instantie met een nieuwe verval datum.  Zelfondertekende certificaten bieden geen ondersteuning voor productie Service Fabric clusters, om certificaten op te nemen die zijn gegenereerd tijdens de werk stroom voor het maken van een Azure Portal cluster. Het nieuwe certificaat moet dezelfde algemene naam hebben als het oudere certificaat. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Service Fabric cluster gebruikt het gedeclareerde certificaat automatisch met een verdere verval datum. Wanneer er meer dan één certificaat voor validatie op de host is geïnstalleerd. Een best practice is het gebruik van een resource manager-sjabloon voor het inrichten van Azure-resources. Voor een niet-productie omgeving kunt u het volgende script gebruiken om een nieuw certificaat te uploaden naar een sleutel kluis en vervolgens het certificaat installeren op de schaalset voor virtuele machines: 

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  <subscription ID>

# Sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "keyvaultgroup"
$VaultName = "cntestvault2"
$certFilename = "C:\users\sfuser\sftutorialcluster20180419110824.pfx"
$certname = "cntestcert"
$Password  = "!P@ssw0rd321"
$VmssResourceGroupName     = "sfclustertutorialgroup"
$VmssName                  = "prnninnxj"

# Create new Resource Group 
New-AzResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Get the key vault.  The key vault must be enabled for deployment.
$keyVault = Get-AzKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName 
$resourceId = $keyVault.ResourceId  

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

$certConfig = New-AzVmssVaultCertificateConfig -CertificateUrl $CertificateURL -CertificateStore "My"

# Get current VM scale set 
$vmss = Get-AzVmss -ResourceGroupName $VmssResourceGroupName -VMScaleSetName $VmssName

# Add new secret to the VM scale set.
$vmss = Add-AzVmssSecret -VirtualMachineScaleSet $vmss -SourceVaultId $SourceVault -VaultCertificate $certConfig

# Update the VM scale set 
Update-AzVmss -ResourceGroupName $VmssResourceGroupName -Name $VmssName -VirtualMachineScaleSet $vmss  -Verbose
```

>[!NOTE]
> Reken bewerkingen voor virtuele-machine schaal sets ondersteunen niet dezelfde resource-id voor twee afzonderlijke geheimen, omdat elk geheim een unieke versie van een bron is. 

Lees voor meer informatie het volgende:
* Meer informatie over [cluster beveiliging](service-fabric-cluster-security.md).
* [Cluster certificaten bijwerken en beheren](service-fabric-cluster-security-update-certs-azure.md)

