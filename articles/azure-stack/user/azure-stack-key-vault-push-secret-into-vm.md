---
title: Een virtuele machine met een certificaat veilig opgeslagen in Azure Stack implementeren | Microsoft Docs
description: Meer informatie over het implementeren van een virtuele machine en een certificaat op het pushen met behulp van een key vault in Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 46590eb1-1746-4ecf-a9e5-41609fde8e89
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/27/2018
ms.author: sethm
ms.openlocfilehash: d0807d625cd26b891b9ecb51a8cd40979934583f
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54192456"
---
# <a name="create-a-virtual-machine-and-install-a-certificate-retrieved-from-an-azure-stack-key-vault"></a>Maak een virtuele machine en installeert u een certificaat dat is opgehaald uit een Azure Stack-sleutelkluis

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Informatie over het maken van een Azure Stack-virtuele machine (VM) met een key vault-certificaat dat is geïnstalleerd.

## <a name="overview"></a>Overzicht

Certificaten worden gebruikt in veel scenario's, zoals verificatie bij Active Directory of versleutelen webverkeer te genereren. U kunt certificaten veilig opslaan als geheimen in een key vault met Azure Stack. De voordelen van het gebruik van Azure Stack Key Vault zijn:

* Certificaten worden niet weergegeven in een script, opdrachtregelgeschiedenis of sjabloon.
* Het proces voor het beheren van certificaten wordt gestroomlijnd.
* Hebt u controle van de sleutels die toegang hebben tot certificaten.

### <a name="process-description"></a>Beschrijving van proces

De volgende stappen beschrijven het proces dat is vereist om een certificaat aan de virtuele machine:

1. Maak een Key Vault geheim.
2. Het bestand azuredeploy.parameters.json bijwerken.
3. Implementeer de sjabloon.

> [!NOTE]
> U kunt deze stappen van de Azure Stack Development Kit, of van een externe client gebruiken als u via VPN-verbinding verbonden bent.

## <a name="prerequisites"></a>Vereisten

* U moet zich abonneren op een aanbieding met de service Key Vault.
* [Installeren van PowerShell voor Azure Stack](azure-stack-powershell-install.md).
* [PowerShell-omgeving van de Azure Stack-gebruiker configureren](azure-stack-powershell-configure-user.md).

## <a name="create-a-key-vault-secret"></a>Maak een Key Vault secret

Het volgende script maakt een certificaat in het PFX-indeling, maakt u een key vault en het certificaat wordt opgeslagen in de key vault als een geheim.

> [!IMPORTANT]
> Moet u de `-EnabledForDeployment` parameter bij het maken van de key vault. Deze parameter zorgt ervoor dat de key vault kan worden verwezen vanuit Azure Resource Manager-sjablonen.

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

Wanneer u het vorige script uitvoert, wordt in de uitvoer de geheime URI bevat. Maak een notitie van deze URI. U moet verwijzen naar deze in de [Push-certificaat naar Windows Resource Manager-sjabloon](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate). Download de [vm-push-certificaat-windows](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate) sjabloonmap op uw computer. Deze map bevat de `azuredeploy.json` en `azuredeploy.parameters.json` bestanden, die u in de volgende stappen.

Wijzig de `azuredeploy.parameters.json` bestand op basis van de omgevingswaarden van uw. De parameters van speciaal belang zijn de naam van de kluis, de resourcegroep van de kluis en de geheime URI (zoals die worden gegenereerd door het vorige script). De volgende sectie ziet u een voorbeeld van een parameterbestand.

## <a name="update-the-azuredeployparametersjson-file"></a>Het bestand azuredeploy.parameters.json bijwerken

Update de `azuredeploy.parameters.json` bestand met de `vaultName`, geheime URI `VmName`, en andere waarden aan de hand van uw omgeving. De volgende JSON-bestand toont een voorbeeld van het bestand met sjabloonparameters:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
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

Wanneer de sjabloon is geïmplementeerd, resulteert dit in de volgende uitvoer:

![Implementatieresultaten van sjabloon](media/azure-stack-key-vault-push-secret-into-vm/deployment-output.png)

Azure Stack pushes het certificaat aan de virtuele machine tijdens de implementatie. De certificaatlocatie is afhankelijk van het besturingssysteem van de virtuele machine:

* In Windows, het certificaat is toegevoegd aan de **LocalMachine /** locatie van het certificaatarchief op die de gebruiker opgegeven certificaat.
* Het certificaat is in Linux, geplaatst onder de `/var/lib/waagent directory`, met de naam van het &lt;UppercaseThumbprint&gt;.crt voor de X509 certificaatbestand en &lt;UppercaseThumbprint&gt;.prv voor de persoonlijke sleutel.

## <a name="retire-certificates"></a>Certificaten buiten gebruik stellen

Buiten gebruik stellen van certificaten is onderdeel van het beheerproces voor certificaten. U kunt de oudere versie van een certificaat niet verwijderen, maar u kunt deze uitschakelen met behulp van de `Set-AzureKeyVaultSecretAttribute` cmdlet.

Het volgende voorbeeld ziet hoe u een certificaat uitschakelen. Gebruik uw eigen waarden voor de **VaultName**, **naam**, en **versie** parameters.

```powershell
Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0
```

## <a name="next-steps"></a>Volgende stappen

* [Een virtuele machine implementeren met een Key Vault-wachtwoord](azure-stack-key-vault-deploy-vm-with-secret.md)
* [Toestaan dat een toepassing toegang tot Key Vault](azure-stack-key-vault-sample-app.md)
