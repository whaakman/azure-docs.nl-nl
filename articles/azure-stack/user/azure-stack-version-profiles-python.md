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
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: sijuman
<!-- dev: viananth -->
ms.openlocfilehash: c55dcf0736642690f245f680db5cb1620c2175e7
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42058720"
---
# <a name="use-api-version-profiles-with-python-in-azure-stack"></a>API-versieprofielen gebruiken met Python in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

## <a name="python-and-api-version-profiles"></a>Python en API-versieprofielen

De Python-SDK biedt ondersteuning voor API-versieprofielen om u te richten op verschillende platforms, zoals Azure Stack en globale Azure. U kunt de API-profielen in het maken van oplossingen voor een hybride cloud. De Python-SDK biedt ondersteuning voor de volgende API-profielen:

1. **meest recente**  
    Het profiel is bedoeld voor de meest recente API-versies voor alle providers in het Azure-Platform.
2.  **2017-03-09-profiel**  
    **2017-03-09-profiel**  
    Het profiel is bedoeld voor de API-versies van de resourceproviders die wordt ondersteund door Azure Stack.

    Zie voor meer informatie over API-profielen en Azure Stack [beheren API-versieprofielen in Azure Stack](azure-stack-version-profiles.md).

## <a name="install-azure-python-sdk"></a>Azure Python-SDK installeren

1.  Installeer Git van [de officiële site](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
2.  Zie voor instructies voor het installeren van de Python-SDK, [Azure voor Python-ontwikkelaars](https://docs.microsoft.com/python/azure/python-sdk-azure-install?view=azure-python).
3.  Als niet beschikbaar is, maakt u een abonnement en opslaan van de abonnements-ID moet later worden gebruikt. Zie voor instructies voor het maken van een abonnement, [abonnementen voor aanbiedingen maken in Azure Stack](../azure-stack-subscribe-plan-provision-vm.md). 
4.  Een service-principal maken en sla de ID en -geheim op. Zie voor instructies voor het maken van een service-principal voor Azure Stack, [toepassingen toegang verlenen tot Azure Stack](../azure-stack-create-service-principals.md). 
5.  Zorg ervoor dat uw service-principal de rol van inzender of eigenaar heeft op uw abonnement. Zie voor instructies over het toewijzen van rollen aan service-principal [toepassingen toegang verlenen tot Azure Stack](../azure-stack-create-service-principals.md).

## <a name="prerequisites"></a>Vereisten

Als u wilt gebruiken Python-SDK van Azure met Azure Stack, moet u de volgende waarden opgeven en vervolgens waarden met omgevingsvariabelen instellen. Zie de instructies onder de tabel voor uw besturingssysteem voor het instellen van de omgevingsvariabelen. 

| Waarde | Omgevingsvariabelen | Beschrijving |
|---------------------------|-----------------------|-------------------------------------------------------------------------------------------------------------------------|
| Tenant-id | AZURE_TENANT_ID | De waarde van uw Azure Stack [tenant-ID](../azure-stack-identity-overview.md). |
| Client-id | AZURE_CLIENT_ID | De service principal toepassings-ID opgeslagen wanneer service-principal is gemaakt op de vorige sectie van dit document. |
| Abonnements-id | AZURE_SUBSCRIPTION_ID | De [abonnements-ID](../azure-stack-plan-offer-quota-overview.md#subscriptions) is hoe u toegang hebben tot aanbiedingen in Azure Stack. |
| Clientgeheim | AZURE_CLIENT_SECRET | De service-principal toepassing geheim opgeslagen bij het service-principal is gemaakt. |
| Resource Manager-eindpunt | ARM_ENDPOINT | Zie [de Azure Stack resource manager-eindpunt](azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint). |


## <a name="python-samples-for-azure-stack"></a>Python-voorbeelden voor Azure Stack 

De volgende voorbeelden van code kunt u algemene beheertaken uitvoeren voor virtuele machines in uw Azure Stack.

De codevoorbeelden die zien u om te:

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

Als u wilt controleren van de code voor het uitvoeren van deze bewerkingen, bekijk de **run_example()** functie in het Python-script **Hybrid/unmanaged-disks/example.py** in de GitHub-opslagplaats [ virtuele machines-python-beheren](https://github.com/viananth/virtual-machines-python-manage/tree/8643ed4bec62aae6fdb81518f68d835452872f88).

Elke bewerking wordt duidelijk aangegeven met een opmerking en een functie print.
De voorbeelden zijn niet noodzakelijkerwijs in de volgorde die wordt weergegeven in de bovenstaande lijst.


## <a name="run-the-python-sample"></a>Het Python-voorbeeld uitvoeren

1.  Als u dit nog niet, [installeren Python](https://www.python.org/downloads/).

    In dit voorbeeld (en de SDK) is compatibel met Python 2.7, 3.4, 3.5 en 3.6.

2.  Algemene aanbevelingen voor het ontwikkelen van Python is het gebruik van een virtuele omgeving. 
    Zie voor meer informatie https://docs.python.org/3/tutorial/venv.html
    
    Installeren en het initialiseren van de virtuele omgeving met de module 'venv' op Python 3 (u moet installeren [virtualenv](https://pypi.python.org/pypi/virtualenv) voor Python 2.7):

    ````bash
    python -m venv mytestenv # Might be "python3" or "py -3.6" depending on your Python installation
    cd mytestenv
    source bin/activate      # Linux shell (Bash, ZSH, etc.) only
    ./scripts/activate       # PowerShell only
    ./scripts/activate.bat   # Windows CMD only
    ````

3.  Kloon de opslagplaats.

    ````bash
    git clone https://github.com/Azure-Samples/virtual-machines-python-manage.git
    ````

4.  Installeer de afhankelijkheden via pip.

    ````bash
    cd virtual-machines-python-manage\Hybrid
    pip install -r requirements.txt
    ````

5.  Maak een [service-principal](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals) om te werken met Azure Stack. Zorg ervoor dat uw service-principal heeft [rol van inzender of eigenaar](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal) op uw abonnement.

6.  De volgende variabelen instellen en deze omgevingsvariabelen worden geëxporteerd in uw huidige shell. 

    ```bash
    export AZURE_TENANT_ID={your tenant id}
    export AZURE_CLIENT_ID={your client id}
    export AZURE_CLIENT_SECRET={your client secret}
    export AZURE_SUBSCRIPTION_ID={your subscription id}
    export ARM_ENDPOINT={your AzureStack Resource Manager Endpoint}
    ```

7.  Om uit te voeren in dit voorbeeld, moet Ubuntu 16.04-LTS en WindowsServer 2012 R2 Datacenter afbeeldingen aanwezig zijn in Azure Stack-marktplaats. Dit kunnen zijn [gedownload van Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-download-azure-marketplace-item) of [toegevoegd aan Platform Image Repository](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-vm-image).

8. Voet het voorbeeld uit.

    ```
    python unmanaged-disks\example.py
    ```

## <a name="notes"></a>Opmerkingen

U kunt mogelijk geneigd om te proberen om op te halen van de besturingssysteemschijf van een virtuele machine met behulp van `virtual_machine.storage_profile.os_disk`.
In sommige gevallen kan dit doen wat u wilt hebt, maar er rekening mee dat dit u biedt een `OSDisk` object.
Als u wilt bijwerken van de grootte van de OS-schijf, als `example.py` , u hoeft niet een `OSDisk` object, maar een `Disk` object.
`example.py` haalt de `Disk` object met het volgende:

```python
os_disk_name = virtual_machine.storage_profile.os_disk.name
os_disk = compute_client.disks.get(GROUP_NAME, os_disk_name)
```

## <a name="next-steps"></a>Volgende stappen

- [Azure Python-ontwikkeling Center](https://azure.microsoft.com/develop/python/)
- [Documentatie voor Azure-Machines](https://azure.microsoft.com/services/virtual-machines/)
- [Leertraject voor virtuele Machines](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/)
- Als u geen Microsoft Azure-abonnement hebt, krijgt u een gratis proefaccount [hier](http://go.microsoft.com/fwlink/?LinkId=330212).
