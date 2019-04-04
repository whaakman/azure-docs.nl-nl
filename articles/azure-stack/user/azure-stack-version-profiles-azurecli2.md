---
title: Verbinding maken met Azure Stack met CLI | Microsoft Docs
description: Meer informatie over het gebruik van de platformoverschrijdende opdrachtregelinterface (CLI) om te beheren en implementeren van resources in Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 02/28/2019
ms.openlocfilehash: 21167366ff3af2bb360c33eaae9d591020bf11a5
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58487580"
---
# <a name="use-api-version-profiles-with-azure-cli-in-azure-stack"></a>API-versieprofielen gebruiken met Azure CLI in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt de stappen in dit artikel om in te stellen van de Azure-opdrachtregelinterface (CLI) voor het beheren van resources voor Azure Stack Development Kit (ASDK) van Linux, Mac en Windows-clientplatforms.

## <a name="prepare-for-azure-cli"></a>Voorbereiden voor Azure CLI

U moet de CA-basiscertificaat voor Azure Stack voor gebruik van Azure CLI op uw ontwikkelcomputer. U kunt het certificaat gebruiken voor het beheren van resources via de CLI.

 - **De Azure Stack-CA-basiscertificaat** is vereist als u de CLI op een werkstation buiten de ASDK.  

 - **Het eindpunt van de virtuele machine-aliassen** biedt een alias, zoals "UbuntuLTS" of "Win2012Datacenter,' die verwijst naar een installatiekopie-uitgever, aanbieding, SKU en versie als één parameter bij het implementeren van virtuele machines.  

De volgende secties wordt beschreven hoe u deze waarden ophalen.

### <a name="export-the-azure-stack-ca-root-certificate"></a>Het Azure Stack-CA-basiscertificaat exporteren

Als u van een geïntegreerd systeem gebruikmaakt, moet u niet de CA-basiscertificaat te exporteren. U moet de CA-basiscertificaat op een ASDK te exporteren.

Voor het exporteren van het basiscertificaat ASDK in PEM-indeling:

1. [Een Windows VM maken op Azure Stack](azure-stack-quick-windows-portal.md).

2. Meld u aan bij de computer, open een verhoogde PowerShell-prompt en voer het volgende script:

    ```powershell  
      $label = "AzureStackSelfSignedRootCert"
      Write-Host "Getting certificate from the current user trusted store with subject CN=$label"
      $root = Get-ChildItem Cert:\CurrentUser\Root | Where-Object Subject -eq "CN=$label" | select -First 1
      if (-not $root)
      {
          Write-Error "Certificate with subject CN=$label not found"
          return
      }

    Write-Host "Exporting certificate"
    Export-Certificate -Type CERT -FilePath root.cer -Cert $root

    Write-Host "Converting certificate to PEM format"
    certutil -encode root.cer root.pem
    ```

3. Kopieer het certificaat naar uw lokale computer.


### <a name="set-up-the-virtual-machine-aliases-endpoint"></a>Instellen van het eindpunt van de virtuele machine-aliassen

U kunt een openbaar toegankelijk eindpunt dat als host fungeert voor een virtuele machine alias-bestand instellen. De virtuele machine alias-bestand is een JSON-bestand dat een algemene naam voor een afbeelding biedt. U gebruikt de naam bij het implementeren van een virtuele machine als een parameter van de Azure CLI.

1. Als u een aangepaste installatiekopie publiceert, noteer de uitgever, aanbieding, SKU en versie-informatie die u hebt opgegeven tijdens het publiceren. Als een installatiekopie van de marketplace is, kunt u de informatie weergeven met behulp van de ```Get-AzureVMImage``` cmdlet.  

2. Download de [voorbeeldbestand](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) vanuit GitHub.

3. Maak een opslagaccount in Azure Stack. Wanneer u dat hebt gedaan, maakt u een blob-container. Instellen van het toegangsbeleid voor 'openbare'.  

4. Upload het JSON-bestand naar de nieuwe container. Wanneer u dat hebt gedaan, kunt u de URL van de blob weergeven. Selecteer de blobnaam van de en vervolgens de URL in de blob-eigenschappen te selecteren.

### <a name="install-or-upgrade-cli"></a>Installeren of upgraden van CLI

Aanmelden bij uw ontwikkelwerkstation en CLI installeren. Azure Stack is versie 2.0 of hoger van Azure CLI vereist. De nieuwste versie van de API-profielen vereist een actuele versie van de CLI.  U kunt de CLI installeren met behulp van de stappen in de [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) artikel. Om te controleren of de installatie geslaagd is, open een terminal of opdrachtpromptvenster en voer de volgende opdracht uit:

```shell
az --version
```

Hier ziet u de versie van Azure CLI en andere afhankelijke bibliotheken die zijn geïnstalleerd op uw computer.

### <a name="install-python-on-windows"></a>Python installeren op Windows

1. Installeer [Python 3 op uw systeem](https://www.python.org/downloads/).

2. PIP upgraden. PIP is een pakketbeheerprogramma voor Python. Open een opdrachtprompt of een PowerShell-prompt met verhoogde bevoegdheid en typ de volgende opdracht:

    ```powershell  
    python -m pip install --upgrade pip
    ```

3. Installeer de **gecertificeerde** module. [Gecertificeerde](https://pypi.org/project/certifi/) een module en een verzameling van basiscertificaten voor het valideren van de betrouwbaarheid van SSL-certificaten tijdens het verifiëren van de identiteit van de TLS-hosts. Open een opdrachtprompt of een PowerShell-prompt met verhoogde bevoegdheid en typ de volgende opdracht:

    ```powershell
    pip install certifi
    ```

### <a name="install-python-on-linux"></a>Python installeren in Linux

1. De Ubuntu 16.04-installatiekopie wordt geleverd met Python 2.7 en Python 3.5 standaard geïnstalleerd. U kunt uw versie van Python 3 controleren door het uitvoeren van de volgende opdracht uit:

    ```bash  
    python3 --version
    ```

2. PIP upgraden. PIP is een pakketbeheerprogramma voor Python. Open een opdrachtprompt of een PowerShell-prompt met verhoogde bevoegdheid en typ de volgende opdracht:

    ```bash  
    sudo -H pip3 install --upgrade pip
    ```

3. Installeer de **gecertificeerde** module. [Gecertificeerde](https://pypi.org/project/certifi/) is een verzameling van basiscertificaten voor het valideren van de betrouwbaarheid van SSL-certificaten tijdens het verifiëren van de identiteit van de TLS-hosts. Open een opdrachtprompt of een PowerShell-prompt met verhoogde bevoegdheid en typ de volgende opdracht:

    ```bash
    pip3 install certifi
    ```

### <a name="install-python-on-macos"></a>Python installeren in Mac OS

1. Installeer [Python 3 op uw systeem](https://www.python.org/downloads/). Voor versies van Python 3.7 biedt Python.org twee opties voor het binaire installatieprogramma downloaden. De standaard-variant is 64-bits-only en werkt in macOS 10.9 (Mavericks) en latere besturingssystemen. Controleer uw python-versie van de terminal te openen en de volgende opdracht te typen:

    ```bash  
    python3 --version
    ```

2. PIP upgraden. PIP is een pakketbeheerprogramma voor Python. Open een opdrachtprompt of een PowerShell-prompt met verhoogde bevoegdheid en typ de volgende opdracht:

    ```bash  
    sudo -H pip3 install --upgrade pip
    ```

3. Installeer de **gecertificeerde** module. [Gecertificeerde](https://pypi.org/project/certifi/) een module en een verzameling van basiscertificaten voor het valideren van de betrouwbaarheid van SSL-certificaten tijdens het verifiëren van de identiteit van de TLS-hosts. Open een opdrachtprompt of een PowerShell-prompt met verhoogde bevoegdheid en typ de volgende opdracht:

    ```bash
    pip3 install certifi
    ```

## <a name="windows-azure-ad"></a>Windows (Azure AD)

In deze sectie helpen u bij het instellen van de CLI als u van Azure AD als uw identity management-service gebruikmaakt en worden met behulp van CLI op een Windows-machine.

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Vertrouwen van de Azure Stack-CA-basiscertificaat

Als u de ASDK gebruikt, moet u het CA-basiscertificaat op de externe computer vertrouwt. U moet niet om dit te doen met de geïntegreerde systemen.

Als u het basiscertificaat van de Azure Stack-CA vertrouwt, voegt u deze toe aan het bestaande certificaat voor Python.

1. De certificaatlocatie op uw computer vinden. De locatie kan variëren, afhankelijk van waar u Python hebt geïnstalleerd. Open een opdrachtprompt of een PowerShell-prompt met verhoogde bevoegdheid en typ de volgende opdracht:

    ```powershell  
      python -c "import certifi; print(certifi.where())"
    ```

    Noteer de certificaatlocatie. Bijvoorbeeld `~/lib/python3.5/site-packages/certifi/cacert.pem`. Het pad voor de desbetreffende, is afhankelijk van uw besturingssysteem en de versie van Python gebruikt die u hebt geïnstalleerd.

2. De Azure Stack-CA-basiscertificaat door deze toe te voegen aan het bestaande Python-certificaat vertrouwen.

    ```powershell
    $pemFile = "<Fully qualified path to the PEM certificate Ex: C:\Users\user1\Downloads\root.pem>"

    $root = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
    $root.Import($pemFile)

    Write-Host "Extracting required information from the cert file"
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

    Write-Host "Python Cert store was updated to allow the Azure Stack CA root certificate"
    ```

### <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack

1. Registreren van uw Azure Stack-omgeving door het uitvoeren van de `az cloud register` opdracht.

    In sommige scenario's worden direct uitgaande verbinding met internet wordt doorgestuurd via een proxy of firewall, die wordt afgedwongen SSL worden onderschept. In dergelijke gevallen de `az cloud register` opdracht kan mislukken met een fout betreft, zoals "Kan geen eindpunten ophalen uit de cloud." U kunt deze fout omzeilen, kunt u de volgende omgevingsvariabelen instellen:

    ```shell  
    set AZURE_CLI_DISABLE_CONNECTION_VERIFICATION=1 
    set ADAL_PYTHON_SSL_NO_VERIFY=1
    ```

2. Registreer uw omgeving. De volgende parameters gebruiken bij het uitvoeren van `az cloud register`.

    | Value | Voorbeeld | Description |
    | --- | --- | --- |
    | Naam van de omgeving | AzureStackUser | Gebruik `AzureStackUser` voor de gebruikersomgeving. Als u de operator bent, geeft u `AzureStackAdmin`. |
    | Resource Manager-eindpunt | https://management.local.azurestack.external | De **ResourceManagerUrl** is in de Azure Stack Development Kit (ASDK): `https://management.local.azurestack.external/` De **ResourceManagerUrl** in geïntegreerde systemen is: `https://management.<region>.<fqdn>/` Om op te halen de metagegevens die vereist zijn: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0` Als u een vraag hebt over het eindpunt geïntegreerd systeem, moet u contact op met uw cloud-operator. |
    | Opslageindpunt | local.azurestack.external | `local.azurestack.external` is voor de ASDK. Voor een geïntegreerd systeem, wordt u wilt gebruiken van een eindpunt voor uw systeem.  |
    | Keyvalut achtervoegsel | .vault.local.azurestack.external | `.vault.local.azurestack.external` is voor de ASDK. Voor een geïntegreerd systeem, wordt u wilt gebruiken van een eindpunt voor uw systeem.  |
    | VM-installatiekopie alias doc-eindpunt | https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json | De URI van het document waarin de virtuele machine-installatiekopie aliassen. Zie voor meer informatie, [### instellen van het eindpunt van de virtuele machine-aliassen](#set-up-the-virtual-machine-aliases-endpoint). |

    ```azurecli  
    az cloud register -n <environmentname> --endpoint-resource-manager "https://management.local.azurestack.external" --suffix-storage-endpoint "local.azurestack.external" --suffix-keyvault-dns ".vault.local.azurestack.external" --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
    ```

1. De actieve omgeving instellen met behulp van de volgende opdrachten.

      ```azurecli
      az cloud set -n <environmentname>
      ```

1. Werk de configuratie van uw omgeving voor het gebruik van de Azure Stack specifieke API-versie-profiel. Voor het bijwerken van de configuratie, moet u de volgende opdracht uitvoeren:

    ```azurecli
    az cloud update --profile 2018-03-01-hybrid
   ```

    >[!NOTE]  
    >Als u een versie van Azure Stack voordat de 1808-build uitvoert, moet u het profiel van API-versie **2017-03-09-profiel** in plaats van de API-versie profiel **2018-03-01-hybride**. U moet een recente versie van de Azure CLI.
 
1. Aanmelden bij uw Azure Stack-omgeving met behulp van de `az login` opdracht. U kunt aanmelden bij de Azure Stack-omgeving als een gebruiker of als een [service-principal](../../active-directory/develop/app-objects-and-service-principals.md). 

   - Meld u aan als een *gebruiker*: 

     U kunt de gebruikersnaam en het wachtwoord rechtstreeks binnen opgeven de `az login` opdracht of verifiëren met behulp van een browser. U moet de laatste doen als uw account multi-factor authentication ingeschakeld heeft:

     ```azurecli
     az login -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
     ```

     > [!NOTE]
     > Als uw gebruikersaccount multi-factor authentication ingeschakeld heeft, kunt u de `az login` opdracht zonder op te geven de `-u` parameter. Met deze opdracht geeft u een URL en een code die u gebruiken moet om te verifiëren.

   - Meld u aan als een *service-principal*: 
    
     Voordat u zich aanmeldt, [maken van een service-principal via de Azure-portal](azure-stack-create-service-principals.md) of CLI en een rol toewijzen. Nu aanmelden met behulp van de volgende opdracht uit:

     ```azurecli  
     az login --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> --service-principal -u <Application Id of the Service Principal> -p <Key generated for the Service Principal>
     ```

### <a name="test-the-connectivity"></a>De connectiviteit testen

Met Alles instellen, CLI gebruiken voor het maken van resources in Azure Stack. U kunt bijvoorbeeld een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Gebruik de volgende opdracht om een resourcegroep met de naam 'MyResourceGroup' te maken:

```azurecli
az group create -n MyResourceGroup -l local
```

Als de resourcegroep is gemaakt, voert de vorige opdracht de volgende eigenschappen van de zojuist gemaakte resource:

![Resourcegroep uitvoer maken](media/azure-stack-connect-cli/image1.png)

## <a name="windows-ad-fs"></a>Windows (AD FS)

In deze sectie helpen u bij het instellen van de CLI als u van Active Directory Federated Services (AD FS) als uw identity management-service gebruikmaakt en worden met behulp van CLI op een Windows-machine.

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Vertrouwen van de Azure Stack-CA-basiscertificaat

Als u de ASDK gebruikt, moet u het CA-basiscertificaat op de externe computer vertrouwt. U moet niet om dit te doen met de geïntegreerde systemen.

1. De certificaatlocatie op uw computer vinden. De locatie kan variëren, afhankelijk van waar u Python hebt geïnstalleerd. Open een opdrachtprompt of een PowerShell-prompt met verhoogde bevoegdheid en typ de volgende opdracht:

    ```powershell  
      python -c "import certifi; print(certifi.where())"
    ```

    Noteer de certificaatlocatie. Bijvoorbeeld `~/lib/python3.5/site-packages/certifi/cacert.pem`. Het pad voor de desbetreffende, is afhankelijk van uw besturingssysteem en de versie van Python gebruikt die u hebt geïnstalleerd.

2. De Azure Stack-CA-basiscertificaat door deze toe te voegen aan het bestaande Python-certificaat vertrouwen.

    ```powershell
    $pemFile = "<Fully qualified path to the PEM certificate Ex: C:\Users\user1\Downloads\root.pem>"

    $root = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
    $root.Import($pemFile)

    Write-Host "Extracting required information from the cert file"
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

    Write-Host "Python Cert store was updated to allow the Azure Stack CA root certificate"
    ```

### <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack

1. Registreren van uw Azure Stack-omgeving door het uitvoeren van de `az cloud register` opdracht.

    In sommige scenario's worden direct uitgaande verbinding met internet wordt doorgestuurd via een proxy of firewall, die wordt afgedwongen SSL worden onderschept. In dergelijke gevallen de `az cloud register` opdracht kan mislukken met een fout betreft, zoals "Kan geen eindpunten ophalen uit de cloud." U kunt deze fout omzeilen, kunt u de volgende omgevingsvariabelen instellen:

    ```shell  
    set AZURE_CLI_DISABLE_CONNECTION_VERIFICATION=1 
    set ADAL_PYTHON_SSL_NO_VERIFY=1
    ```

2. Registreer uw omgeving. De volgende parameters gebruiken bij het uitvoeren van `az cloud register`.

    | Value | Voorbeeld | Beschrijving |
    | --- | --- | --- |
    | Naam van de omgeving | AzureStackUser | Gebruik `AzureStackUser` voor de gebruikersomgeving. Als u de operator bent, geeft u `AzureStackAdmin`. |
    | Resource Manager-eindpunt | https://management.local.azurestack.external | De **ResourceManagerUrl** is in de Azure Stack Development Kit (ASDK): `https://management.local.azurestack.external/` De **ResourceManagerUrl** in geïntegreerde systemen is: `https://management.<region>.<fqdn>/` Om op te halen de metagegevens die vereist zijn: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0` Als u een vraag hebt over het eindpunt geïntegreerd systeem, moet u contact op met uw cloud-operator. |
    | Opslageindpunt | local.azurestack.external | `local.azurestack.external` is voor de ASDK. Voor een geïntegreerd systeem, wordt u wilt gebruiken van een eindpunt voor uw systeem.  |
    | Keyvalut achtervoegsel | .vault.local.azurestack.external | `.vault.local.azurestack.external` is voor de ASDK. Voor een geïntegreerd systeem, wordt u wilt gebruiken van een eindpunt voor uw systeem.  |
    | VM-installatiekopie alias doc-eindpunt | https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json | De URI van het document waarin de virtuele machine-installatiekopie aliassen. Zie voor meer informatie, [### instellen van het eindpunt van de virtuele machine-aliassen](#set-up-the-virtual-machine-aliases-endpoint). |

    ```azurecli  
    az cloud register -n <environmentname> --endpoint-resource-manager "https://management.local.azurestack.external" --suffix-storage-endpoint "local.azurestack.external" --suffix-keyvault-dns ".vault.local.azurestack.external" --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
    ```

1. De actieve omgeving instellen met behulp van de volgende opdrachten.

      ```azurecli
      az cloud set -n <environmentname>
      ```

1. Werk de configuratie van uw omgeving voor het gebruik van de Azure Stack specifieke API-versie-profiel. Voor het bijwerken van de configuratie, moet u de volgende opdracht uitvoeren:

    ```azurecli
    az cloud update --profile 2018-03-01-hybrid
   ```

    >[!NOTE]  
    >Als u een versie van Azure Stack voordat de 1808-build uitvoert, moet u het profiel van API-versie **2017-03-09-profiel** in plaats van de API-versie profiel **2018-03-01-hybride**. U moet een recente versie van de Azure CLI.

1. Aanmelden bij uw Azure Stack-omgeving met behulp van de `az login` opdracht. U kunt aanmelden bij de Azure Stack-omgeving als een gebruiker of als een [service-principal](../../active-directory/develop/app-objects-and-service-principals.md). 

   - Meld u aan als een *gebruiker*:

     U kunt de gebruikersnaam en het wachtwoord rechtstreeks binnen opgeven de `az login` opdracht of verifiëren met behulp van een browser. U moet de laatste doen als uw account multi-factor authentication ingeschakeld heeft:

     ```azurecli
     az cloud register  -n <environmentname>   --endpoint-resource-manager "https://management.local.azurestack.external"  --suffix-storage-endpoint "local.azurestack.external" --suffix-keyvault-dns ".vault.local.azurestack.external" --endpoint-active-directory-resource-id "https://management.adfs.azurestack.local/<tenantID>" --endpoint-active-directory-graph-resource-id "https://graph.local.azurestack.external/" --endpoint-active-directory "https://adfs.local.azurestack.external/adfs/" --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>   --profile "2018-03-01-hybrid"
     ```

     > [!NOTE]
     > Als uw gebruikersaccount multi-factor authentication ingeschakeld heeft, kunt u de `az login` opdracht zonder op te geven de `-u` parameter. Met deze opdracht geeft u een URL en een code die u gebruiken moet om te verifiëren.

   - Meld u aan als een *service-principal*: 
    
     Bereid het .pem-bestand moet worden gebruikt voor service-principal-aanmelding.

     Op de clientcomputer waar de principal is gemaakt, de service-principal-certificaat exporteren als een pfx met de persoonlijke sleutel te vinden op `cert:\CurrentUser\My`; de naam heeft dezelfde naam als de principal certificaat.

     Converteer de pfx naar pem (Gebruik het hulpprogramma OpenSSL).

     Meld u aan bij de CLI:
  
     ```azurecli  
     az login --service-principal \
      -u <Client ID from the Service Principal details> \
      -p <Certificate's fully qualified name, such as, C:\certs\spn.pem>
      --tenant <Tenant ID> \
      --debug 
     ```

### <a name="test-the-connectivity"></a>De connectiviteit testen

Met Alles instellen, CLI gebruiken voor het maken van resources in Azure Stack. U kunt bijvoorbeeld een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Gebruik de volgende opdracht om een resourcegroep met de naam 'MyResourceGroup' te maken:

```azurecli
az group create -n MyResourceGroup -l local
```

Als de resourcegroep is gemaakt, voert de vorige opdracht de volgende eigenschappen van de zojuist gemaakte resource:

![Resourcegroep uitvoer maken](media/azure-stack-connect-cli/image1.png)


## <a name="linux-azure-ad"></a>Linux (Azure AD)

In deze sectie helpen u bij het instellen van de CLI als u van Azure AD als uw identity management-service gebruikmaakt en worden met behulp van CLI op een Linux-machine.

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Vertrouwen van de Azure Stack-CA-basiscertificaat

Als u de ASDK gebruikt, moet u het CA-basiscertificaat op de externe computer vertrouwt. U moet niet om dit te doen met de geïntegreerde systemen.

De Azure Stack-CA-basiscertificaat door deze toe te voegen aan het bestaande Python-certificaat vertrouwen.

1. De certificaatlocatie op uw computer vinden. De locatie kan variëren, afhankelijk van waar u Python hebt geïnstalleerd. U moet beschikken over pip en de gecertificeerde [-module geïnstalleerd](#install-python-on-linux). U kunt de volgende Python-opdracht uit vanuit de bash-prompt:

    ```bash  
    python3 -c "import certifi; print(certifi.where())"
    ```

    Noteer de certificaatlocatie: bijvoorbeeld, `~/lib/python3.5/site-packages/certifi/cacert.pem`. Het specifieke pad, is afhankelijk van uw besturingssysteem en de versie van Python gebruikt die u hebt geïnstalleerd.

2. Voer de volgende bash-opdracht door het pad naar het certificaat.

   - Voor een externe Linux-machine:

     ```bash  
     sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
     ```

   - Voor een Linux-machine in de Azure Stack-omgeving:

     ```bash  
     sudo cat /var/lib/waagent/Certificates.pem >> ~/<yourpath>/cacert.pem
     ```

### <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack

Gebruik de volgende stappen uit om te verbinden met Azure Stack:

1. Registreren van uw Azure Stack-omgeving door het uitvoeren van de `az cloud register` opdracht. In sommige scenario's worden direct uitgaande verbinding met internet wordt doorgestuurd via een proxy of firewall, die wordt afgedwongen SSL worden onderschept. In dergelijke gevallen de `az cloud register` opdracht kan mislukken met een fout betreft, zoals "Kan geen eindpunten ophalen uit de cloud." U kunt deze fout omzeilen, kunt u de volgende omgevingsvariabelen instellen:

   ```shell
   set AZURE_CLI_DISABLE_CONNECTION_VERIFICATION=1 
   set ADAL_PYTHON_SSL_NO_VERIFY=1
   ```

2. Registreer uw omgeving. De volgende parameters gebruiken bij het uitvoeren van `az cloud register`.

    | Value | Voorbeeld | Beschrijving |
    | --- | --- | --- |
    | Naam van de omgeving | AzureStackUser | Gebruik `AzureStackUser` voor de gebruikersomgeving. Als u de operator bent, geeft u `AzureStackAdmin`. |
    | Resource Manager-eindpunt | https://management.local.azurestack.external | De **ResourceManagerUrl** is in de Azure Stack Development Kit (ASDK): `https://management.local.azurestack.external/` De **ResourceManagerUrl** in geïntegreerde systemen is: `https://management.<region>.<fqdn>/` Om op te halen de metagegevens die vereist zijn: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0` Als u een vraag hebt over het eindpunt geïntegreerd systeem, moet u contact op met uw cloud-operator. |
    | Opslageindpunt | local.azurestack.external | `local.azurestack.external` is voor de ASDK. Voor een geïntegreerd systeem, wordt u wilt gebruiken van een eindpunt voor uw systeem.  |
    | Keyvalut achtervoegsel | .vault.local.azurestack.external | `.vault.local.azurestack.external` is voor de ASDK. Voor een geïntegreerd systeem, wordt u wilt gebruiken van een eindpunt voor uw systeem.  |
    | VM-installatiekopie alias doc-eindpunt | https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json | De URI van het document waarin de virtuele machine-installatiekopie aliassen. Zie voor meer informatie, [### instellen van het eindpunt van de virtuele machine-aliassen](#set-up-the-virtual-machine-aliases-endpoint). |

    ```azurecli  
    az cloud register -n <environmentname> --endpoint-resource-manager "https://management.local.azurestack.external" --suffix-storage-endpoint "local.azurestack.external" --suffix-keyvault-dns ".vault.local.azurestack.external" --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
    ```

3. Stel de actieve omgeving. 

      ```azurecli
        az cloud set -n <environmentname>
      ```

4. Werk de configuratie van uw omgeving voor het gebruik van de Azure Stack specifieke API-versie-profiel. Voor het bijwerken van de configuratie, moet u de volgende opdracht uitvoeren:

    ```azurecli
      az cloud update --profile 2018-03-01-hybrid
   ```

    >[!NOTE]  
    >Als u een versie van Azure Stack voordat de 1808-build uitvoert, moet u het profiel van API-versie **2017-03-09-profiel** in plaats van de API-versie profiel **2018-03-01-hybride**. U moet een recente versie van de Azure CLI.

5. Aanmelden bij uw Azure Stack-omgeving met behulp van de `az login` opdracht. U kunt aanmelden bij de Azure Stack-omgeving als een gebruiker of als een [service-principal](../../active-directory/develop/app-objects-and-service-principals.md). 

   * Meld u aan als een *gebruiker*:

     U kunt de gebruikersnaam en het wachtwoord rechtstreeks binnen opgeven de `az login` opdracht of verifiëren met behulp van een browser. U moet de laatste doen als uw account multi-factor authentication ingeschakeld heeft:

     ```azurecli
     az login \
       -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> \
       --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
     ```

     > [!NOTE]
     > Als uw gebruikersaccount multi-factor authentication ingeschakeld heeft, kunt u de `az login` opdracht zonder op te geven de `-u` parameter. Met deze opdracht geeft u een URL en een code die u gebruiken moet om te verifiëren.
   
   * Meld u aan als een *service-principal*
    
     Voordat u zich aanmeldt, [maken van een service-principal via de Azure-portal](azure-stack-create-service-principals.md) of CLI en een rol toewijzen. Nu aanmelden met behulp van de volgende opdracht uit:

     ```azurecli  
     az login \
       --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
       --service-principal \
       -u <Application Id of the Service Principal> \
       -p <Key generated for the Service Principal>
     ```

### <a name="test-the-connectivity"></a>De connectiviteit testen

Met Alles instellen, CLI gebruiken voor het maken van resources in Azure Stack. U kunt bijvoorbeeld een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Gebruik de volgende opdracht om een resourcegroep met de naam 'MyResourceGroup' te maken:

```azurecli
    az group create -n MyResourceGroup -l local
```

Als de resourcegroep is gemaakt, voert de vorige opdracht de volgende eigenschappen van de zojuist gemaakte resource:

![Resourcegroep uitvoer maken](media/azure-stack-connect-cli/image1.png)

## <a name="linux-ad-fs"></a>Linux (AD FS)

In deze sectie helpen u bij het instellen van de CLI als u van Active Directory Federated Services (AD FS) als uw management-service gebruikmaakt en worden met behulp van CLI op een Linux-machine.

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Vertrouwen van de Azure Stack-CA-basiscertificaat

Als u de ASDK gebruikt, moet u het CA-basiscertificaat op de externe computer vertrouwt. U moet niet om dit te doen met de geïntegreerde systemen.

De Azure Stack-CA-basiscertificaat door deze toe te voegen aan het bestaande Python-certificaat vertrouwen.

1. De certificaatlocatie op uw computer vinden. De locatie kan variëren, afhankelijk van waar u Python hebt geïnstalleerd. U moet beschikken over pip en de gecertificeerde [-module geïnstalleerd](#install-python-on-linux). U kunt de volgende Python-opdracht uit vanuit de bash-prompt:

    ```bash  
    python3 -c "import certifi; print(certifi.where())"
    ```

    Noteer de certificaatlocatie: bijvoorbeeld, `~/lib/python3.5/site-packages/certifi/cacert.pem`. Het specifieke pad, is afhankelijk van uw besturingssysteem en de versie van Python gebruikt die u hebt geïnstalleerd.

2. Voer de volgende bash-opdracht door het pad naar het certificaat.

   - Voor een externe Linux-machine:

     ```bash  
     sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
     ```

   - Voor een Linux-machine in de Azure Stack-omgeving:

     ```bash  
     sudo cat /var/lib/waagent/Certificates.pem >> ~/<yourpath>/cacert.pem
     ```

### <a name="connect-to-azure-stack"></a>Verbinding maken met Azure Stack

Gebruik de volgende stappen uit om te verbinden met Azure Stack:

1. Registreren van uw Azure Stack-omgeving door het uitvoeren van de `az cloud register` opdracht. In sommige scenario's worden direct uitgaande verbinding met internet wordt doorgestuurd via een proxy of firewall, die wordt afgedwongen SSL worden onderschept. In dergelijke gevallen de `az cloud register` opdracht kan mislukken met een fout betreft, zoals "Kan geen eindpunten ophalen uit de cloud." U kunt deze fout omzeilen, kunt u de volgende omgevingsvariabelen instellen:

   ```shell
   set AZURE_CLI_DISABLE_CONNECTION_VERIFICATION=1 
   set ADAL_PYTHON_SSL_NO_VERIFY=1
   ```

2. Registreer uw omgeving. De volgende parameters gebruiken bij het uitvoeren van `az cloud register`.

    | Value | Voorbeeld | Description |
    | --- | --- | --- |
    | Naam van de omgeving | AzureStackUser | Gebruik `AzureStackUser` voor de gebruikersomgeving. Als u de operator bent, geeft u `AzureStackAdmin`. |
    | Resource Manager-eindpunt | https://management.local.azurestack.external | De **ResourceManagerUrl** is in de Azure Stack Development Kit (ASDK): `https://management.local.azurestack.external/` De **ResourceManagerUrl** in geïntegreerde systemen is: `https://management.<region>.<fqdn>/` Om op te halen de metagegevens die vereist zijn: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0` Als u een vraag hebt over het eindpunt geïntegreerd systeem, moet u contact op met uw cloud-operator. |
    | Opslageindpunt | local.azurestack.external | `local.azurestack.external` is voor de ASDK. Voor een geïntegreerd systeem, wordt u wilt gebruiken van een eindpunt voor uw systeem.  |
    | Keyvalut achtervoegsel | .vault.local.azurestack.external | `.vault.local.azurestack.external` is voor de ASDK. Voor een geïntegreerd systeem, wordt u wilt gebruiken van een eindpunt voor uw systeem.  |
    | VM-installatiekopie alias doc-eindpunt | https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json | De URI van het document waarin de virtuele machine-installatiekopie aliassen. Zie voor meer informatie, [### instellen van het eindpunt van de virtuele machine-aliassen](#set-up-the-virtual-machine-aliases-endpoint). |

    ```azurecli  
    az cloud register -n <environmentname> --endpoint-resource-manager "https://management.local.azurestack.external" --suffix-storage-endpoint "local.azurestack.external" --suffix-keyvault-dns ".vault.local.azurestack.external" --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
    ```

3. Stel de actieve omgeving. 

      ```azurecli
        az cloud set -n <environmentname>
      ```

4. Werk de configuratie van uw omgeving voor het gebruik van de Azure Stack specifieke API-versie-profiel. Voor het bijwerken van de configuratie, moet u de volgende opdracht uitvoeren:

    ```azurecli
      az cloud update --profile 2018-03-01-hybrid
   ```

    >[!NOTE]  
    >Als u een versie van Azure Stack voordat de 1808-build uitvoert, moet u het profiel van API-versie **2017-03-09-profiel** in plaats van de API-versie profiel **2018-03-01-hybride**. U moet een recente versie van de Azure CLI.

5. Aanmelden bij uw Azure Stack-omgeving met behulp van de `az login` opdracht. U kunt aanmelden bij de Azure Stack-omgeving als een gebruiker of als een [service-principal](../../active-directory/develop/app-objects-and-service-principals.md). 

6. Aanmelden: 

   *  Als een **gebruiker** via een webbrowser met de apparaatcode van een:  

   ```azurecli  
    az login --use-device-code
   ```

   > [!NOTE]  
   >Met de opdracht geeft u een URL en een code die u gebruiken moet om te verifiëren.

   * Als een service-principal:
        
     Bereid het .pem-bestand moet worden gebruikt voor service-principal-aanmelding.

      * Op de clientcomputer waar de principal is gemaakt, de service-principal-certificaat exporteren als een pfx met de persoonlijke sleutel te vinden op `cert:\CurrentUser\My`; de naam heeft dezelfde naam als de principal certificaat.
  
      * Converteer de pfx naar pem (Gebruik het hulpprogramma OpenSSL).

     Meld u aan bij de CLI:

      ```azurecli  
      az login --service-principal \
        -u <Client ID from the Service Principal details> \
        -p <Certificate's fully qualified name, such as, C:\certs\spn.pem>
        --tenant <Tenant ID> \
        --debug 
      ```

### <a name="test-the-connectivity"></a>De connectiviteit testen

Met Alles instellen, CLI gebruiken voor het maken van resources in Azure Stack. U kunt bijvoorbeeld een resourcegroep voor een toepassing maken en toevoegen van een virtuele machine. Gebruik de volgende opdracht om een resourcegroep met de naam 'MyResourceGroup' te maken:

```azurecli
  az group create -n MyResourceGroup -l local
```

Als de resourcegroep is gemaakt, voert de vorige opdracht de volgende eigenschappen van de zojuist gemaakte resource:

![Resourcegroep uitvoer maken](media/azure-stack-connect-cli/image1.png)

## <a name="known-issues"></a>Bekende problemen

Er zijn bekende problemen bij het gebruik van de CLI in Azure Stack:

 - De interactieve modus van de CLI; bijvoorbeeld, de `az interactive` opdracht, wordt nog niet ondersteund in Azure Stack.
 - Als u de lijst met installatiekopieën van virtuele machines beschikbaar in Azure Stack, gebruikt de `az vm image list --all` opdracht in plaats van de `az vm image list` opdracht. Op te geven de `--all` optie zorgt ervoor dat het antwoord retourneert alleen de installatiekopieën die beschikbaar in uw Azure Stack-omgeving zijn.
 - Virtuele machine-installatiekopie aliassen die beschikbaar in Azure zijn is mogelijk niet van toepassing op Azure Stack. Wanneer u installatiekopieën van virtuele machines, moet u de volledige URN-parameter (Canonical: UbuntuServer:14.04.3-LTS:1.0.0) in plaats van de alias van de installatiekopie. Deze URN moet overeenkomen met de specificaties van de installatiekopie die is afgeleid van de `az vm images list` opdracht.

## <a name="next-steps"></a>Volgende stappen

- [Sjablonen implementeren met Azure CLI](azure-stack-deploy-template-command-line.md)
- [Azure CLI voor Azure Stack-gebruikers (Operator) inschakelen](../azure-stack-cli-admin.md)
- [Gebruikersmachtigingen beheren](azure-stack-manage-permissions.md) 
