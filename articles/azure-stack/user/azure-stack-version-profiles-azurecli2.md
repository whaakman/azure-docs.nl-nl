---
title: Verbinding maken met Azure Stack met CLI | Microsoft Docs
description: Informatie over het gebruik van de platformoverschrijdende opdrachtregelinterface (CLI) kunt beheren en implementeren van resources op Azure-Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: eb01d31d00177560aca3aa71750cd2d1ec096f8f
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938393"
---
# <a name="use-api-version-profiles-with-azure-cli-20-in-azure-stack"></a>API-versie profielen gebruiken met Azure CLI 2.0 in Azure-Stack

U kunt de stappen in dit artikel voor het instellen van de de Azure-opdrachtregelinterface (CLI) Azure Stack Development Kit om resources te beheren van Linux, Mac en Windows client-platformen.

## <a name="install-cli"></a>CLI installeren

Aanmelden bij uw ontwikkelwerkstation en CLI installeren. Azure Stack vereist versie 2.0 van Azure CLI. U kunt die installeren met behulp van de stappen in de [2.0 voor Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) artikel. Om te controleren of de installatie geslaagd is, een terminal of een opdrachtpromptvenster open en voer de volgende opdracht:

```azurecli
az --version
```

Hier ziet u de versie van Azure CLI en andere afhankelijke bibliotheken die op uw computer zijn geïnstalleerd.

## <a name="trust-the-azure-stack-ca-root-certificate"></a>Vertrouwen van de Azure-Stack-CA-basiscertificaat

1. Ophalen van de Azure-Stack-CA-basiscertificaat van [uw Azure-Stack-operator](..\azure-stack-cli-admin.md#export-the-azure-stack-ca-root-certificate) en deze vertrouwt. Als u het basiscertificaat van de Azure-Stack CA vertrouwt, voegt u deze toe aan het bestaande certificaat voor Python.

2. De certificaatlocatie op uw computer vinden. De locatie kan variëren, afhankelijk van waar u Python hebt geïnstalleerd. U moet hebben [pip](https://pip.pypa.io) en de [gecertificeerde](https://pypi.org/project/certifi/) -module geïnstalleerd. U kunt de volgende opdracht Python vanaf de opdrachtprompt bash gebruiken:

  ```bash  
    python -c "import certifi; print(certifi.where())"
  ```

  Noteer de certificaatlocatie. Bijvoorbeeld `~/lib/python3.5/site-packages/certifi/cacert.pem`. Het pad voor de specifieke, is afhankelijk van uw besturingssysteem en de versie van Python die u hebt geïnstalleerd.

### <a name="set-the-path-for-a-development-machine-inside-the-cloud"></a>Het pad voor een ontwikkelcomputer worden ingesteld in de cloud

Als u CLI op een Linux-machine die in de Azure-Stack-omgeving uitvoert, voer de volgende bash-opdracht met het pad naar uw certificaat is gemaakt.

```bash
sudo cat /var/lib/waagent/Certificates.pem >> ~/<yourpath>/cacert.pem
```

### <a name="set-the-path-for-a-development-machine-outside-the-cloud"></a>Het pad voor een ontwikkelcomputer ingesteld buiten de cloud

Als u CLI op een machine uitvoert **buiten** de Stack van Azure-omgeving:  

1. U moet instellen [VPN-verbinding met Azure-Stack](azure-stack-connect-azure-stack.md).

2. Kopieer het PEM-certificaat dat u Azure-Stack operator hebt gekregen, en noteer de locatie van het bestand (PATH_TO_PEM_FILE).

3. Voer de volgende opdrachten, afhankelijk van afloopt op uw ontwikkelwerkstation OS.

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
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```

   b. Registreren de *gebruiker* omgeving, gebruiken:

      ```azurecli
      az cloud register \ 
        -n AzureStackUser \ 
        --endpoint-resource-manager "https://management.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
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

 - De interactieve modus CLI eenledige de `az interactive` opdracht is nog niet ondersteund in Azure-Stack.
 - Als u de lijst met installatiekopieën van virtuele machines beschikbaar zijn in Azure-Stack, gebruikt de `az vm images list --all` opdracht in plaats van de `az vm image list` opdracht. Geven de `--all` optie zorgt ervoor dat antwoord retourneert alleen de afbeeldingen die beschikbaar in uw Azure-Stack-omgeving zijn.
 - Virtuele machine installatiekopie aliassen die beschikbaar in Azure zijn is mogelijk niet van toepassing op Azure-Stack. Als u installatiekopieën van virtuele machines gebruikt, moet u de volledige URN-parameter (Canonical: UbuntuServer:14.04.3-LTS:1.0.0) in plaats van de alias van de installatiekopie. Deze URN moet overeenkomen met de installatiekopie-specificaties die is afgeleid van de `az vm images list` opdracht.

## <a name="next-steps"></a>Volgende stappen

[Sjablonen met Azure CLI implementeren](azure-stack-deploy-template-command-line.md)

[Azure CLI inschakelen voor gebruikers van de Azure-Stack (Operator)](..\azure-stack-cli-admin.md)

[Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md)