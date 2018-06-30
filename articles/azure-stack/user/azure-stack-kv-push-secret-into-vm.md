---
title: Een virtuele machine met een veilig opgeslagen certificaat op de Stack Azure implementeren | Microsoft Docs
description: Informatie over het implementeren van een virtuele machine en een certificaat op deze met behulp van een sleutelkluis in Azure-Stack push
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 46590eb1-1746-4ecf-a9e5-41609fde8e89
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2018
ms.author: mabrigg
ms.openlocfilehash: 05278ee4b0dc1f2c22f40bfcff4f9d7342017c0f
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37108753"
---
# <a name="create-a-virtual-machine-and-install-a-certificate-retrieved-from-an-azure-stack-key-vault"></a>Een virtuele machine maken en een certificaat dat is opgehaald uit een Azure-Stack-sleutelkluis installeren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Informatie over het maken van een Azure-Stack virtuele machine (VM) met een sleutelkluis certificaat geïnstalleerd.

## <a name="overview"></a>Overzicht

Certificaten worden gebruikt in veel scenario's, zoals verificatie bij Active Directory of webverkeer versleutelen. U kunt certificaten veilig opslaan als geheimen in een Azure-Stack-sleutelkluis. De voordelen van het gebruik van Azure Sleutelkluis voor Stack zijn:

* Certificaten worden niet weergegeven in een script, opdrachtregel geschiedenis of sjabloon.
* Het certificaat beheerproces wordt gestroomlijnd.
* Hebt u controle van de sleutels die toegang hebben tot certificaten.

### <a name="process-description"></a>Procesbeschrijving van het

De volgende stappen beschrijven het proces dat is vereist voor de push-een certificaat op de virtuele machine:

1. Een Sleutelkluis geheim maken.
2. Het bestand azuredeploy.parameters.json bijwerken.
3. De sjabloon implementeren

> [!NOTE]
> U kunt deze stappen van de Azure-Stack Development Kit of van een externe client gebruiken als u via VPN-verbinding verbonden bent.

## <a name="prerequisites"></a>Vereisten

* U moet zich abonneren op een aanbieding met de Sleutelkluis-service.
* [Installeer PowerShell voor Azure-Stack.](azure-stack-powershell-install.md)
* [Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](azure-stack-powershell-configure-user.md)

## <a name="create-a-key-vault-secret"></a>Een Sleutelkluis geheim maken

Het volgende script maakt een certificaat in PFX-indeling, maakt u een sleutelkluis en het certificaat wordt opgeslagen in de sleutelkluis als een geheim.

> [!IMPORTANT]
> Moet u de `-EnabledForDeployment` parameter bij het maken van de sleutelkluis. Deze parameter zorgt ervoor dat de sleutelkluis kan worden verwezen vanuit de Azure Resource Manager-sjablonen.

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

De sjabloon implementeren met behulp van de volgende PowerShell-script:

```powershell
# Deploy a Resource Manager template to create a VM and push the secret onto it
New-AzureRmResourceGroupDeployment `
  -Name KVDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Wanneer de sjabloon wordt geïmplementeerd, resulteert dit in de volgende uitvoer:

![Sjabloonresultaten voor implementatie](media/azure-stack-kv-push-secret-into-vm/deployment-output.png)

Het certificaat op de virtuele machine duwt Azure Stack tijdens de implementatie. Locatie van het certificaat is afhankelijk van het besturingssysteem van de VM:

* Het certificaat is toegevoegd in Windows naar de locatie van het certificaat LocalMachine, met het certificaatarchief van de gebruiker opgegeven.
* Het certificaat is in Linux, geplaatst in de map /var/lib/waagent met de bestandsnaam &lt;UppercaseThumbprint&gt;.crt voor de X509 certificaatbestand en &lt;UppercaseThumbprint&gt;.prv voor de persoonlijke sleutel .

## <a name="retire-certificates"></a>Certificaten buiten gebruik stellen

Buiten gebruik stellen van certificaten is onderdeel van het beheerproces van het certificaat. U kunt de oudere versie van een certificaat niet verwijderen, maar u kunt dit onderdeel uitschakelen met behulp van de `Set-AzureKeyVaultSecretAttribute` cmdlet.

Het volgende voorbeeld ziet het uitschakelen van een certificaat. Gebruik uw eigen waarden voor de **VaultName**, **naam**, en **versie** parameters.

```powershell
Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0
```

## <a name="next-steps"></a>Volgende stappen

* [Een virtuele machine implementeren met een Key Vault-wachtwoord](azure-stack-kv-deploy-vm-with-secret.md)
* [Toestaan dat een toepassing voor toegang tot de Sleutelkluis](azure-stack-kv-sample-app.md)
