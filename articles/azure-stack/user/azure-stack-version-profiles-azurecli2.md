---
title: Verbinding maken met Azure Stack met CLI | Microsoft Docs
description: Meer informatie over het gebruik van de platformoverschrijdende opdrachtregelinterface (CLI) om te beheren en implementeren van resources in Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 59b637e6887a645430d902cd846cacda13b14cfe
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46972807"
---
# <a name="use-api-version-profiles-with-azure-cli-in-azure-stack"></a>API-versieprofielen gebruiken met Azure CLI in Azure Stack

U kunt de stappen in dit artikel om in te stellen van de Azure-opdrachtregelinterface (CLI) voor het beheren van Azure Stack Development Kit resources vanuit Linux, Mac en Windows-clientplatforms.

## <a name="install-cli"></a>CLI installeren

Aanmelden bij uw ontwikkelwerkstation en CLI installeren. Azure Stack is versie 2.0 of hoger van Azure CLI vereist. U kunt die installeren met behulp van de stappen in de [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) artikel. Om te controleren of de installatie geslaagd is, open een terminal of opdrachtprompt en voer de volgende opdracht uit:

```azurecli
az --version
```

Hier ziet u de versie van Azure CLI en andere afhankelijke bibliotheken die zijn geïnstalleerd op uw computer.

## <a name="trust-the-azure-stack-ca-root-certificate"></a>Vertrouwen van de Azure Stack-CA-basiscertificaat

1. Ophalen van de Azure Stack-CA-basiscertificaat van [uw Azure Stack-operators](..\azure-stack-cli-admin.md#export-the-azure-stack-ca-root-certificate) en vertrouwt. Als u het basiscertificaat van de Azure Stack-CA vertrouwt, voegt u deze toe aan het bestaande certificaat voor Python.

1. De certificaatlocatie op uw computer vinden. De locatie kan variëren, afhankelijk van waar u Python hebt geïnstalleerd. U moet hebben [pip](https://pip.pypa.io) en de [gecertificeerde](https://pypi.org/project/certifi/) -module geïnstalleerd. U kunt de volgende Python-opdracht uit vanuit de bash-prompt:

  ```bash  
    python -c "import certifi; print(certifi.where())"
  ```

  Noteer de certificaatlocatie. Bijvoorbeeld `~/lib/python3.5/site-packages/certifi/cacert.pem`. Het pad voor de desbetreffende, is afhankelijk van uw besturingssysteem en de versie van Python gebruikt die u hebt geïnstalleerd.

### <a name="set-the-path-for-a-development-machine-inside-the-cloud"></a>Het pad voor een ontwikkelcomputer instellen in de cloud

Als u CLI vanaf een Linux-machine die wordt gemaakt binnen de Azure Stack-omgeving uitvoert, voer de volgende bash-opdracht door het pad naar het certificaat.

```bash
sudo cat /var/lib/waagent/Certificates.pem >> ~/<yourpath>/cacert.pem
```

### <a name="set-the-path-for-a-development-machine-outside-the-cloud"></a>Het pad voor een ontwikkelcomputer instellen buiten de cloud

Als u CLI van een virtuele machine uitvoert **buiten** de Azure Stack-omgeving:  

1. U moet instellen [VPN-verbinding met Azure Stack](azure-stack-connect-azure-stack.md).

1. Kopieer het PEM-certificaat dat u hebt verkregen via de Azure Stack-operators en noteer de locatie van het bestand (PATH_TO_PEM_FILE).

1. Voer de volgende opdrachten, afhankelijk afloopt op uw ontwikkelwerkstation OS.

#### <a name="linux"></a>Linux

```bash
sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
```

#### <a name="macos"></a>macOS

```bash
sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
```

#### <a name="windows"></a>Windows

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
$sha1Entry    = [string]::Format("# SHA1 Fingerprint: {0}", $sha1Hash)
$sha256Entry  = [string]::Format("# SHA256 Fingerprint: {0}", $sha256Hash)
$certText = (Get-Content -Path $pemFile -Raw).ToString().Replace("`r`n","`n")

$rootCertEntry = "`n" + $issuerEntry + "`n" + $subjectEntry + "`n" + $labelEntry + "`n" + `
$serialEntry + "`n" + $md5Entry + "`n" + $sha1Entry + "`n" + $sha256Entry + "`n" + $certText

Write-Host "Adding the certificate content to Python Cert store"
Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

Write-Host "Python Cert store was updated for allowing the azure stack CA root certificate"
```

## <a name="get-the-virtual-machine-aliases-endpoint"></a>Het eindpunt van de virtuele machine-aliassen ophalen

Voordat gebruikers virtuele machines maken kunnen met behulp van CLI, moeten deze contact op met de Azure Stack-operators en ophalen van het eindpunt van de virtuele machine-aliassen URI. Azure gebruikt bijvoorbeeld de volgende URI: https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json. Beheerder van de cloud moet instellen van een soortgelijke eindpunt voor Azure Stack met de afbeeldingen die beschikbaar in de Azure Stack marketplace zijn. Gebruikers de URI van het eindpunt moeten doorgeven aan de `endpoint-vm-image-alias-doc` parameter voor de `az cloud register` opdracht zoals wordt weergegeven in de volgende sectie. 
   

## <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack

Gebruik de volgende stappen uit om te verbinden met Azure Stack:

1. Registreren van uw Azure Stack-omgeving door het uitvoeren van de `az cloud register` opdracht.
   
   a. Om u te registreren de *cloud administratieve* omgeving gebruiken:

      ```azurecli
      az cloud register \ 
        -n AzureStackAdmin \ 
        --endpoint-resource-manager "https://adminmanagement.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".adminvault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```

   b. Om u te registreren de *gebruiker* omgeving gebruiken:

      ```azurecli
      az cloud register \ 
        -n AzureStackUser \ 
        --endpoint-resource-manager "https://management.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```

1. De actieve omgeving instellen met behulp van de volgende opdrachten.

   a. Voor de *cloud administratieve* omgeving gebruiken:

      ```azurecli
      az cloud set \
        -n AzureStackAdmin
      ```

   b. Voor de *gebruiker* omgeving gebruiken:

      ```azurecli
      az cloud set \
        -n AzureStackUser
      ```

1. Werk de configuratie van uw omgeving voor het gebruik van de Azure Stack specifieke API-versie-profiel. Voor het bijwerken van de configuratie, moet u de volgende opdracht uitvoeren:

   ```azurecli
   az cloud update \
     --profile 2018-03-01-hybrid
   ```

    >[!NOTE]  
    >Als u een versie van de Azure Stack voordat de 1808-build uitvoert, wordt u moet de API-versie-profiel gebruiken **2017-03-09-profiel** in plaats van de API-versie profiel **2018-03-01-hybride**.

1. Aanmelden bij uw Azure Stack-omgeving met behulp van de `az login` opdracht. U kunt aanmelden bij de Azure Stack-omgeving als een gebruiker of als een [service-principal](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects). 

   * Meld u aan als een *gebruiker*: U kunt de gebruikersnaam en het wachtwoord rechtstreeks binnen opgeven de `az login` opdracht of verifiëren met behulp van een browser. U moet de laatste doen als uw account multi-factor authentication ingeschakeld heeft.

      ```azurecli
      az login \
        -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
      ```

      > [!NOTE]
      > Als uw gebruikersaccount multi-factor authentication ingeschakeld heeft, kunt u de `az login command` zonder op te geven de `-u` parameter. Met de opdracht geeft u een URL en een code die u gebruiken moet om te verifiëren.
   
   * Meld u aan als een *service-principal*: voordat u zich aanmeldt, [maken van een service-principal via de Azure-portal](azure-stack-create-service-principals.md) of CLI en een rol toewijzen. Nu aanmelden met behulp van de volgende opdracht uit:

      ```azurecli
      az login \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
        --service-principal \
        -u <Application Id of the Service Principal> \
        -p <Key generated for the Service Principal>
      ```

## <a name="test-the-connectivity"></a>De connectiviteit testen

Nu dat alles instellen, gaan we CLI gebruiken voor het maken van resources in Azure Stack. U kunt bijvoorbeeld een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Gebruik de volgende opdracht om een resourcegroep met de naam 'MyResourceGroup' te maken:

```azurecli
az group create \
  -n MyResourceGroup -l local
```

Als de resourcegroep is gemaakt, voert de vorige opdracht de volgende eigenschappen van de zojuist gemaakte resource:

![Resourcegroep uitvoer maken](media/azure-stack-connect-cli/image1.png)

## <a name="known-issues"></a>Bekende problemen
Er zijn enkele bekende problemen waarmee u rekening houden moet bij het gebruik van de CLI in Azure Stack:

 - De CLI interactieve modus Internet Explorer de `az interactive` opdracht wordt nog niet ondersteund in Azure Stack.
 - Als u de lijst met installatiekopieën van virtuele machines beschikbaar in Azure Stack, gebruikt de `az vm images list --all` opdracht in plaats van de `az vm image list` opdracht. Op te geven de `--all` optie zorgt ervoor dat antwoord retourneert alleen de installatiekopieën die beschikbaar in uw Azure Stack-omgeving zijn.
 - Virtuele machine-installatiekopie aliassen die beschikbaar in Azure zijn is mogelijk niet van toepassing op Azure Stack. Wanneer u installatiekopieën van virtuele machines, moet u de volledige URN-parameter (Canonical: UbuntuServer:14.04.3-LTS:1.0.0) in plaats van de alias van de installatiekopie. Deze URN moet overeenkomen met de specificaties van de installatiekopie die is afgeleid van de `az vm images list` opdracht.

## <a name="next-steps"></a>Volgende stappen

[Sjablonen implementeren met Azure CLI](azure-stack-deploy-template-command-line.md)

[Azure CLI voor Azure Stack-gebruikers (Operator) inschakelen](..\azure-stack-cli-admin.md)

[Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md)
