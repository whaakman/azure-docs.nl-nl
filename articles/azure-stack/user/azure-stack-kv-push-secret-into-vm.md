---
title: Een virtuele machine met een veilig opgeslagen certificaat op de Stack Azure implementeren | Microsoft Docs
description: Informatie over het implementeren van een virtuele machine en een certificaat op deze met behulp van een sleutelkluis in Azure-Stack push
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 46590eb1-1746-4ecf-a9e5-41609fde8e89
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/03/2017
ms.author: mabrigg
ms.openlocfilehash: e319f5c6d27d3a223764b0a5593480f02864ddbe
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="create-a-virtual-machine-and-include-certificate-retrieved-from-a-key-vault"></a>Maken van een virtuele machine en certificaat opgehaald uit een sleutelkluis opnemen

In dit artikel helpt u bij het maken van een virtuele machine in Azure-Stack en push certificaten op deze. 

## <a name="prerequisites"></a>Vereisten

* U moet zich abonneren op een aanbieding met de Sleutelkluis-service. 
* [Installeer PowerShell voor Azure-Stack.](azure-stack-powershell-install.md)  
* [Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](azure-stack-powershell-configure-user.md)

Een sleutelkluis in Azure-Stack wordt gebruikt voor het opslaan van certificaten. Certificaten zijn handig in veel verschillende scenario's. Neem bijvoorbeeld een scenario waarin het hebben van een virtuele machine in Azure-Stack waarop een toepassing die u een certificaat moet wordt uitgevoerd. Dit certificaat kan worden gebruikt voor het coderen voor verificatie bij de Active Directory of voor SSL op een website. Als het certificaat van een sleutelkluis kunnen Zorg ervoor dat deze is beveiligd.

In dit artikel doorlopen we de stappen die nodig zijn om een certificaat op een virtuele Windows-machine in Azure-Stack. U kunt deze stappen van de Azure-Stack Development Kit of vanaf een externe Windows-client gebruiken als u via VPN-verbinding verbonden bent.

De volgende stappen beschrijven het proces dat is vereist voor de push-een certificaat op de virtuele machine:

1. Een Sleutelkluis geheim maken.
2. Het bestand azuredeploy.parameters.json bijwerken.
3. De sjabloon implementeren

## <a name="create-a-key-vault-secret"></a>Een Sleutelkluis geheim maken

Het volgende script maakt een certificaat in PFX-indeling, maakt u een sleutelkluis en het certificaat wordt opgeslagen in de sleutelkluis als een geheim. Moet u de `-EnabledForDeployment` parameter als u de sleutelkluis. Deze parameter zorgt ervoor dat de sleutelkluis kan worden verwezen vanuit de Azure Resource Manager-sjablonen.

```powershell

# Create a certificate in the .pfx format
New-SelfSignedCertificate `
  -certstorelocation cert:\LocalMachine\My `
  -dnsname contoso.microsoft.com

$pwd = ConvertTo-SecureString `
  -String "<Password used to export the certificate>" `
  -Force `
  -AsPlainText

Export-PfxCertificate `
  -cert "cert:\localMachine\my\<Certificate Thumbprint that was created in the previous step>" `
  -FilePath "<Fully qualified path where the exported certificate can be stored>" `
  -Password $pwd

# Create a key vault and upload the certificate into the key vault as a secret
$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "servicecert"
$fileName = "<Fully qualified path where the exported certificate can be stored>"
$certPassword = "<Password used to export the certificate>"

$fileContentBytes = get-content $fileName `
  -Encoding Byte

$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
$jsonObject = @"
{
"data": "$filecontentencoded",
"dataType" :"pfx",
"password": "$certPassword"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -sku standard `
  -EnabledForDeployment

$secret = ConvertTo-SecureString `
  -String $jsonEncoded `
  -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
   -SecretValue $secret

```

Wanneer u het vorige script uitvoert, wordt de uitvoer de geheime URI bevat. Noteer deze URI. U moet verwijzen naar deze in de [Push-certificaat naar Windows Resource Manager-sjabloon](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate). Download de [vm-push-certificaat-windows-sjabloon](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate) map op uw ontwikkelcomputer. Deze map bevat de `azuredeploy.json` en `azuredeploy.parameters.json` bestanden, wat u in de volgende stappen moet.

Wijzig de `azuredeploy.parameters.json` bestand volgens de Omgevingswaarden in de. De parameters van speciaal belang zijn de kluisnaam van de, de resourcegroep voor de kluis en het geheim URI (zoals gegenereerd door het vorige script). Het volgende bestand is een voorbeeld van een parameterbestand:

## <a name="update-the-azuredeployparametersjson-file"></a>Het bestand azuredeploy.parameters.json bijwerken

Het bestand azuredeploy.parameters.json bijwerken met de vaultName, geheime URI, VmName en andere waarden aan de hand van uw omgeving. Het volgende JSON-bestand ziet u een voorbeeld van de sjabloon parameters: 

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "newStorageAccountName": {
      "value": "kvstorage01"
    },
    "vmName": {
      "value": "VM1"
    },
    "vmSize": {
      "value": "Standard_D1_v2"
    },
    "adminUserName": {
      "value": "demouser"
    },
    "adminPassword": {
      "value": "demouser@123"
    },
    "vaultName": {
      "value": "contosovault"
    },
    "vaultResourceGroup": {
      "value": "contosovaultrg"
    },
    "secretUrlWithVersion": {
      "value": "https://testkv001.vault.local.azurestack.external/secrets/testcert002/82afeeb84f4442329ce06593502e7840"
    }
  }
}
```

## <a name="deploy-the-template"></a>De sjabloon implementeren

Nu u de sjabloon implementeren met behulp van de volgende PowerShell-script:

```powershell
# Deploy a Resource Manager template to create a VM and push the secret onto it
New-AzureRmResourceGroupDeployment `
  -Name KVDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Wanneer de sjabloon wordt geïmplementeerd, resulteert dit in de volgende uitvoer:

![Implementatie-uitvoer](media/azure-stack-kv-push-secret-into-vm/deployment-output.png)

Wanneer deze virtuele machine wordt geïmplementeerd, stuurt Azure Stack het certificaat op de virtuele machine. Het certificaat is toegevoegd in Windows naar de locatie van het certificaat LocalMachine, met het certificaatarchief van de gebruiker opgegeven. Het certificaat is in Linux, geplaatst in de map /var/lib/waagent met de bestandsnaam &lt;UppercaseThumbprint&gt;.crt voor de X509 certificaatbestand en &lt;UppercaseThumbprint&gt;.prv voor de persoonlijke sleutel .

## <a name="retire-certificates"></a>Certificaten buiten gebruik stellen

In de vorige sectie we u laten zien hoe u een nieuw certificaat op een virtuele machine push. Uw oude certificaat is nog steeds op de virtuele machine en kan niet worden verwijderd. U kunt echter de oudere versie van het geheim uitschakelen met behulp van de `Set-AzureKeyVaultSecretAttribute` cmdlet. Hier volgt een voorbeeld van de informatie over het gebruik van deze cmdlet. Zorg ervoor dat u de kluisnaam, geheime naam en Versiewaarden volgens uw omgeving:

```powershell
Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0
```

## <a name="next-steps"></a>Volgende stappen

* [Een virtuele machine implementeren met een Key Vault-wachtwoord](azure-stack-kv-deploy-vm-with-secret.md)
* [Toestaan dat een toepassing voor toegang tot de Sleutelkluis](azure-stack-kv-sample-app.md)


