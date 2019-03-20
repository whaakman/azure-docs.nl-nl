---
title: API-versieprofielen gebruiken met Python in Azure Stack | Microsoft Docs
description: Meer informatie over het gebruik van API-versieprofielen met Python in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 01/05/2019
<!-- dev: viananth -->
ms.openlocfilehash: b3bfc3072f819a92bdceb1721bb7737a3dc04cf8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58078853"
---
# <a name="use-api-version-profiles-with-python-in-azure-stack"></a>API-versieprofielen gebruiken met Python in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

## <a name="python-and-api-version-profiles"></a>Python en API-versieprofielen

De Python-SDK biedt ondersteuning voor API-versieprofielen om u te richten op verschillende platforms, zoals Azure Stack en globale Azure. U kunt de API-profielen in het maken van oplossingen voor een hybride cloud. De Python-SDK biedt ondersteuning voor de volgende API-profielen:

- **latest**  
    Dit profiel is bedoeld voor de meest recente API-versies voor alle providers in het Azure-Platform.
- **2018-03-01-hybrid**     
    Dit profiel is bedoeld voor de meest recente API-versies voor alle resourceproviders in Azure Stack-platform.
- **2017-03-09-profile**  
    Dit profiel is bedoeld voor de meest compatibele API-versies van de resourceproviders die wordt ondersteund door Azure Stack.

   Zie voor meer informatie over API-profielen en Azure Stack [beheren API-versieprofielen in Azure Stack](azure-stack-version-profiles.md).

## <a name="install-the-azure-python-sdk"></a>De Azure Python-SDK installeren

1. Installeer Git van [de officiële site](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
2. Zie voor instructies over hoe u de Python-SDK installeert, [Azure voor Python-ontwikkelaars](/python/azure/python-sdk-azure-install?view=azure-python).
3. Als niet beschikbaar is, maakt u een abonnement en de abonnements-ID voor later gebruik opslaan. Zie voor instructies over het maken van een abonnement [abonnementen voor aanbiedingen maken in Azure Stack](../azure-stack-subscribe-plan-provision-vm.md).
4. Een service-principal maken en sla de ID en -geheim op. Zie voor instructies over het maken van een service-principal voor Azure Stack [toepassingen toegang verlenen tot Azure Stack](../azure-stack-create-service-principals.md).
5. Zorg ervoor dat uw service-principal de rol van inzender of eigenaar heeft op uw abonnement. Zie voor instructies over het toewijzen van rollen aan service-principal [toepassingen toegang verlenen tot Azure Stack](../azure-stack-create-service-principals.md).

## <a name="prerequisites"></a>Vereisten

Als u wilt de Azure-SDK voor Python gebruiken met Azure Stack, moet u de volgende waarden opgeven en vervolgens waarden met omgevingsvariabelen instellen. Zie de instructies onder de tabel voor uw besturingssysteem voor het instellen van de omgevingsvariabelen.

| Value | Omgevingsvariabelen | Description |
|---------------------------|-----------------------|-------------------------------------------------------------------------------------------------------------------------|
| Tenant-id | AZURE_TENANT_ID | De waarde van uw Azure Stack [tenant-ID](../azure-stack-identity-overview.md). |
| Client-id | AZURE_CLIENT_ID | De service principal toepassings-ID opgeslagen bij het service-principal is gemaakt in de vorige sectie van dit artikel. |
| Abonnements-id | AZURE_SUBSCRIPTION_ID | De [abonnements-ID](../azure-stack-plan-offer-quota-overview.md#subscriptions) is hoe u toegang hebben tot aanbiedingen in Azure Stack. |
| Clientgeheim | AZURE_CLIENT_SECRET | De service principal toepassingsgeheim opgeslagen wanneer de service-principal is gemaakt. |
| Resource Manager-eindpunt | ARM_ENDPOINT | Zie de [Azure Stack Resource Manager-eindpunt](azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint). |
| Resourcelocatie | AZURE_RESOURCE_LOCATION | De Resourcelocatie van uw Azure Stack-omgeving.

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Vertrouwen van de Azure Stack-CA-basiscertificaat

Als u de ASDK gebruikt, moet u het CA-basiscertificaat op de externe computer vertrouwt. U moet niet om dit te doen met de geïntegreerde systemen.

#### <a name="windows"></a>Windows

1. Zoek de python certificaatarchieflocatie op uw computer. De locatie kan variëren, afhankelijk van waar u Python hebt geïnstalleerd. Open een opdrachtprompt of een PowerShell-prompt met verhoogde bevoegdheid en typ de volgende opdracht:

    ```PowerShell  
      python -c "import certifi; print(certifi.where())"
    ```

    Maak een notitie van het certificaat locatie opslaan. Bijvoorbeeld, *~/lib/python3.5/site-packages/certifi/cacert.pem*. Het pad voor de desbetreffende, is afhankelijk van uw besturingssysteem en de versie van Python gebruikt die u hebt geïnstalleerd.

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
    Add-Content "${env:ProgramFiles(x86)}\Python35\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

    Write-Host "Python Cert store was updated to allow the Azure Stack CA root certificate"

    ```

> [!NOTE]  
> Als u van virtualenv gebruikmaakt voor het ontwikkelen met Python-SDK, zoals hieronder wordt beschreven, moet u de bovenstaande certificaat toevoegen aan de virtuele omgeving certificatenstore ook. Het pad kan er ongeveer als volgt: ".. \mytestenv\Lib\site-packages\certifi\cacert.PEM"



## <a name="python-samples-for-azure-stack"></a>Python-voorbeelden voor Azure Stack

Enkele van de codevoorbeelden die beschikbaar zijn voor Azure Stack met behulp van Python SDK zijn:

- [Beheren van Resources en resourcegroepen](https://azure.microsoft.com/resources/samples/hybrid-resourcemanager-python-manage-resources/).
- [Storage-Account beheren](https://azure.microsoft.com/resources/samples/hybrid-storage-python-manage-storage-account/).
- [Virtuele Machines beheren](https://azure.microsoft.com/resources/samples/hybrid-compute-python-manage-vm/).

## <a name="python-manage-virtual-machine-sample"></a>Python beheren voorbeeld van de virtuele machine

Het volgende voorbeeld kunt u algemene beheertaken uitvoeren voor virtuele machines in uw Azure Stack. In het voorbeeld ziet u:

- Virtuele machines maken:
  - Een virtuele Linux-machine maken
  - Maak een virtuele Windows-machine
- Update voor een virtuele machine:
  - Een station uitbreiden
  - Een virtuele machine taggen
  - Gegevensschijven koppelen
  - Gegevensschijven ontkoppelen
- Een virtuele machine worden uitgevoerd:
  - Een virtuele machine starten
  - Een virtuele machine stoppen
  - Een virtuele machine opnieuw starten
- Lijst met virtuele machines
- Een virtuele machine verwijderen

De code die deze bewerkingen uitvoert, Zie de **run_example()** functie in het Python-script **example.py** in de GitHub-opslagplaats [hybride-Compute-Python-beheer-VM](https://github.com/Azure-Samples/Hybrid-Compute-Python-Manage-VM).

Elke bewerking wordt duidelijk aangegeven met een opmerking en een functie print. De voorbeelden zijn niet noodzakelijkerwijs in de volgorde die in deze lijst wordt weergegeven.

## <a name="run-the-python-sample"></a>Het Python-voorbeeld uitvoeren

1. Als u nog niet gedaan hebt, [installeren Python](https://www.python.org/downloads/). In dit voorbeeld (en de SDK) is compatibel met Python 2.7, 3.4, 3.5 en 3.6.

2. Algemene aanbevelingen voor het ontwikkelen van Python is het gebruik van een virtuele omgeving. Zie voor meer informatie de [dokumentace Pro Python](https://docs.python.org/3/tutorial/venv.html).

3. Installeren en het initialiseren van de virtuele omgeving met de module 'venv' op Python 3 (u moet installeren [virtualenv](https://pypi.python.org/pypi/virtualenv) voor Python 2.7):

    ```bash
    python -m venv mytestenv # Might be "python3" or "py -3.6" depending on your Python installation
    cd mytestenv
    source bin/activate      # Linux shell (Bash, ZSH, etc.) only
    ./scripts/activate       # PowerShell only
    ./scripts/activate.bat   # Windows CMD only
    ```

4. Kloon de opslagplaats:

    ```bash
    git clone https://github.com/Azure-Samples/Hybrid-Compute-Python-Manage-VM.git
    ```

5. Installeer de afhankelijkheden met behulp van pip:

    ```bash
    cd Hybrid-Compute-Python-Manage-VM
    pip install -r requirements.txt
    ```

6. Maak een [service-principal](../azure-stack-create-service-principals.md) om te werken met Azure Stack. Zorg ervoor dat uw service-principal heeft [rol van inzender of eigenaar](../azure-stack-create-service-principals.md#assign-a-role) op uw abonnement.

7. De volgende variabelen instellen en deze omgevingsvariabelen worden geëxporteerd in uw huidige shell:

    ```bash
    export AZURE_TENANT_ID={your tenant id}
    export AZURE_CLIENT_ID={your client id}
    export AZURE_CLIENT_SECRET={your client secret}
    export AZURE_SUBSCRIPTION_ID={your subscription id}
    export ARM_ENDPOINT={your AzureStack Resource Manager Endpoint}
    export AZURE_RESOURCE_LOCATION={your AzureStack Resource location}
    ```

8. Om uit te voeren in dit voorbeeld, moet Ubuntu 16.04-LTS en WindowsServer 2012 R2 DataCenter afbeeldingen aanwezig zijn in de Azure Stack marketplace. Dit kunnen zijn [gedownload van Azure](../azure-stack-download-azure-marketplace-item.md), of toegevoegd aan de [Platform Image Repository](../azure-stack-add-vm-image.md).

9. Voer het voorbeeld uit:

    ```python
    python example.py
    ```


## <a name="next-steps"></a>Volgende stappen

- [Azure Python Development Center](https://azure.microsoft.com/develop/python/)
- [Documentatie voor Azure-Machines](https://azure.microsoft.com/services/virtual-machines/)
- [Leertraject voor virtuele Machines](/learn/paths/deploy-a-website-with-azure-virtual-machines/)
- Als u geen Microsoft Azure-abonnement hebt, krijgt u een gratis proefaccount [hier](https://go.microsoft.com/fwlink/?LinkId=330212).
