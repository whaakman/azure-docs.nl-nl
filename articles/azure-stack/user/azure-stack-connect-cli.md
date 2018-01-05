---
title: Verbinding maken met Azure Stack met CLI | Microsoft Docs
description: Informatie over het gebruik van de platformoverschrijdende opdrachtregelinterface (CLI) kunt beheren en implementeren van resources op Azure-Stack
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: f576079c-5384-4c23-b5a4-9ae165d1e3c3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: mabrigg
ms.openlocfilehash: 3ca71abb1b84f619cfafbf4c25b0c0cb95430858
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="install-and-configure-cli-for-use-with-azure-stack"></a>CLI installeren en configureren voor gebruik met Azure-Stack

In dit artikel helpen we u aan de hand van het gebruik van de Azure-opdrachtregelinterface (CLI) Azure Stack Development Kit om resources te beheren van Linux en Mac-clientplatforms. 

## <a name="install-cli"></a>CLI installeren

Vervolgens aanmelden bij uw ontwikkelwerkstation en CLI installeren. Azure Stack vereist versie 2.0 van Azure CLI. U kunt die installeren met behulp van de stappen in de [2.0 voor Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) artikel. Om te controleren of de installatie geslaagd is, een terminal of een opdrachtpromptvenster open en voer de volgende opdracht:

```azurecli
az --version
```

Hier ziet u de versie van Azure CLI en andere afhankelijke bibliotheken die op uw computer zijn geïnstalleerd.

## <a name="trust-the-azure-stack-ca-root-certificate"></a>Vertrouwen van de Azure-Stack-CA-basiscertificaat

Ophalen van de Azure-Stack-CA-basiscertificaat van de Azure-Stack-operator en deze vertrouwt. Als u het basiscertificaat van de Azure-Stack CA vertrouwt, voegt u deze toe aan het bestaande certificaat voor Python. Als u CLI vanaf een Linux-machine die wordt gemaakt in de Azure-Stack-omgeving uitvoert, voert u de volgende bash-opdracht:

```bash
sudo cat /var/lib/waagent/Certificates.pem >> ~/lib/azure-cli/lib/python2.7/site-packages/certifi/cacert.pem
```

Als u van een virtuele machine buiten de omgeving verpakking Azure CLI uitvoert, moet u eerst instellen [VPN-verbinding met Azure-Stack](azure-stack-connect-azure-stack.md). Nu het PEM-certificaat dat u eerder hebt geëxporteerd naar uw ontwikkelwerkstation kopiëren en voer de volgende opdrachten, afhankelijk van uw ontwikkelwerkstation OS.

### <a name="linux"></a>Linux

```bash
sudo cat PATH_TO_PEM_FILE >> ~/lib/azure-cli/lib/python2.7/site-packages/certifi/cacert.pem
```

### <a name="macos"></a>macOS

```bash
sudo cat PATH_TO_PEM_FILE >> ~/lib/azure-cli/lib/python2.7/site-packages/certifi/cacert.pem
```

### <a name="windows"></a>Windows

```powershell
$pemFile = "<Fully qualified path to the PEM certificate Ex: C:\Users\user1\Downloads\root.pem>"

$root = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$root.Import($pemFile)

Write-Host "Extracting needed information from the cert file"
$md5Hash    = (Get-FileHash -Path $pemFile -Algorithm MD5).Hash.ToLower()
$sha1Hash   = (Get-FileHash -Path $pemFile -Algorithm SHA1).Hash.ToLower()
$sha256Hash = (Get-FileHash -Path $pemFile -Algorithm SHA256).Hash.ToLower()

$issuerEntry  = [string]::Format("# Issuer: {0}", $root.Issuer)
$subjectEntry = [string]::Format("# Subject: {0}", $root.Subject)
$labelEntry   = [string]::Format("# Label: {0}", $root.Subject.Split('=')[-1])
$serialEntry  = [string]::Format("# Serial: {0}", $root.GetSerialNumberString().ToLower())
$md5Entry     = [string]::Format("# MD5 Fingerprint: {0}", $md5Hash)
$sha1Entry    = [string]::Format("# SHA1 Finterprint: {0}", $sha1Hash)
$sha256Entry  = [string]::Format("# SHA256 Fingerprint: {0}", $sha256Hash)
$certText = (Get-Content -Path $pemFile -Raw).ToString().Replace("`r`n","`n")

$rootCertEntry = "`n" + $issuerEntry + "`n" + $subjectEntry + "`n" + $labelEntry + "`n" + `
$serialEntry + "`n" + $md5Entry + "`n" + $sha1Entry + "`n" + $sha256Entry + "`n" + $certText

Write-Host "Adding the certificate content to Python Cert store"
Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

Write-Host "Python Cert store was updated for allowing the azure stack CA root certificate"
```

## <a name="get-the-virtual-machine-aliases-endpoint"></a>Het eindpunt van de aliassen virtuele machine ophalen

Voordat gebruikers u virtuele machines maken kunnen met CLI, moeten deze contact op met de Azure-Stack-operator en ophalen van het eindpunt van de virtuele machine aliassen URI. Azure gebruikt bijvoorbeeld de volgende URI: https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json. De cloudbeheerder moet instellen een vergelijkbare eindpunt voor Azure-Stack met de afbeeldingen die beschikbaar in de Stack van Azure marketplace zijn. Gebruikers de URI van het eindpunt moeten doorgeven aan de `endpoint-vm-image-alias-doc` -parameter voor de `az cloud register` opdracht zoals weergegeven in de volgende sectie. 
   

## <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack

Gebruik de volgende stappen verbinding maken met Azure-Stack:

1. Registreren van uw Azure-Stack-omgeving door het uitvoeren van de `az cloud register` opdracht.
   
   a. Registreren de *cloud beheerdersrechten* omgeving, gebruiken:

      ```azurecli
      az cloud register \ 
        -n AzureStackAdmin \ 
        --endpoint-resource-manager "https://adminmanagement.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".adminvault.local.azurestack.external" \ 
        --endpoint-active-directory-graph-resource-id "https://graph.windows.net/" \
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```

   b. Registreren de *gebruiker* omgeving, gebruiken:

      ```azurecli
      az cloud register \ 
        -n AzureStackUser \ 
        --endpoint-resource-manager "https://management.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
        --endpoint-active-directory-graph-resource-id "https://graph.windows.net/" \
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```

2. De actieve omgeving instellen met behulp van de volgende opdrachten.

   a. Voor de *cloud beheerdersrechten* omgeving, gebruiken:

      ```azurecli
      az cloud set \
        -n AzureStackAdmin
      ```

   b. Voor de *gebruiker* omgeving, gebruiken:

      ```azurecli
      az cloud set \
        -n AzureStackUser
      ```

3. Werk de configuratie van uw omgeving voor het gebruik van het profiel Azure Stack specifieke API-versie. Voer de volgende opdracht voor het bijwerken van de configuratie:

   ```azurecli
   az cloud update \
     --profile 2017-03-09-profile
   ```

4. Aanmelden bij uw Azure-Stack-omgeving met behulp van de `az login` opdracht. U kunt aanmelden bij de Azure-Stack-omgeving als een gebruiker of als een [service-principal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects). 

   * Meld u aan als een *gebruiker*: kunt u opgeven de gebruikersnaam en wachtwoord rechtstreeks in de `az login` opdracht of verifiëren via een browser. U moet de laatste doen als uw account multi-factor authentication ingeschakeld heeft.

      ```azurecli
      az login \
        -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
      ```

      > [!NOTE]
      > Als uw gebruikersaccount multi-factor authentication ingeschakeld heeft, kunt u de `az login command` zonder op te geven de `-u` parameter. Met de opdracht geeft u een URL en een code die u gebruiken moet om te verifiëren.
   
   * Meld u aan als een *service-principal*: voordat u zich aanmeldt, [maken van een service-principal via de Azure portal](azure-stack-create-service-principals.md) of CLI en een rol toewijzen. Nu aanmelden met de volgende opdracht:

      ```azurecli
      az login \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
        --service-principal \
        -u <Application Id of the Service Principal> \
        -p <Key generated for the Service Principal>
      ```

## <a name="test-the-connectivity"></a>De connectiviteit testen

Nu dat we hebben een alles instellen, gebruiken we CLI voor het maken van resources in Azure-Stack. U kunt bijvoorbeeld een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Gebruik de volgende opdracht om een resourcegroep met de naam 'MyResourceGroup' te maken:

```azurecli
az group create \
  -n MyResourceGroup -l local
```

Als de resourcegroep wordt gemaakt, levert de vorige opdracht de volgende eigenschappen van de zojuist gemaakte resource:

![Resourcegroep maken-uitvoer](media/azure-stack-connect-cli/image1.png)

## <a name="known-issues"></a>Bekende problemen
Er zijn enkele bekende problemen waarmee u rekening houden moet bij het gebruik van CLI in Azure-Stack:

* De interactieve modus CLI eenledige de `az interactive` opdracht is nog niet ondersteund in Azure-Stack.
* Als u de lijst met installatiekopieën van virtuele machines beschikbaar zijn in Azure-Stack, gebruikt de `az vm images list --all` opdracht in plaats van de `az vm image list` opdracht. Geven de `--all` optie zorgt ervoor dat antwoord retourneert alleen de afbeeldingen die beschikbaar in uw Azure-Stack-omgeving zijn. 
* Virtuele machine installatiekopie aliassen die beschikbaar in Azure zijn is mogelijk niet van toepassing op Azure-Stack. Als u installatiekopieën van virtuele machines gebruikt, moet u de volledige URN-parameter (Canonical: UbuntuServer:14.04.3-LTS:1.0.0) in plaats van de alias van de installatiekopie. Deze URN moet overeenkomen met de installatiekopie-specificaties die is afgeleid van de `az vm images list` opdracht.
* Standaard wordt met CLI 2.0 'Standard_DS1_v2' gebruikt als de standaardgrootte voor de installatiekopie van virtuele machine. Maar deze grootte is nog niet beschikbaar in de Stack van Azure, dus u moet opgeven de `--size` parameter expliciet bij het maken van een virtuele machine. U kunt u de lijst met grootten van virtuele machines die beschikbaar in Azure Stack met behulp van zijn de `az vm list-sizes --location <locationName>` opdracht.


## <a name="next-steps"></a>Volgende stappen

[Sjablonen met Azure CLI implementeren](azure-stack-deploy-template-command-line.md)

[Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md)
