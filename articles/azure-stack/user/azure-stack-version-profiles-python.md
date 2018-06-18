---
title: Met behulp van API-versie profielen met behulp van Python in Azure-Stack | Microsoft Docs
description: Meer informatie over het gebruik van API-versie profielen met behulp van Python in Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: mabrigg
ms.reviewer: sijuman
<!-- dev: viananth -->
ms.openlocfilehash: d17ba9ed4548a986d6846d934aee197609ec80ca
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34806833"
---
# <a name="use-api-version-profiles-with-python-in-azure-stack"></a>API-versie profielen met behulp van Python in Azure-Stack gebruiken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

## <a name="python-and-api-version-profiles"></a>Profielen van Python en API-versie

De Python SDK biedt ondersteuning voor API-versie profielen voor verschillende cloudplatforms zoals Azure Stack en globale Azure als doel. U kunt profielen API bij het maken van oplossingen voor een hybride cloud. De SDK voor Python ondersteunt de volgende API-profielen:

1. **meest recente**  
    Het profiel is bedoeld voor de meest recente API-versies voor alle serviceproviders in de Azure-Platform.
2.  **2017-03-09-profiel**  
    **2017-03-09-profiel**  
    Het profiel is bedoeld voor de API-versies van de resourceproviders ondersteund door Azure-Stack.

    Zie voor meer informatie over API-profielen en Azure Stack [versie-profielen API beheren in Azure-Stack](azure-stack-version-profiles.md).

## <a name="install-azure-python-sdk"></a>Azure Python-SDK installeren

1.  Installeer Git van [de officiële site](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
2.  Zie voor instructies voor het installeren van de SDK voor Python [Azure voor Python-ontwikkelaars](https://docs.microsoft.com/python/azure/python-sdk-azure-install?view=azure-python).
3.  Als niet beschikbaar is, een abonnement maken en opslaan van de abonnements-ID moet later worden gebruikt. Zie voor instructies voor het maken van een abonnement, [abonnementen op aanbiedingen maken in Azure-Stack](../azure-stack-subscribe-plan-provision-vm.md). 
4.  Maak een service-principal en sla de ID en het geheim. Zie voor instructies voor het maken van een service-principal voor Azure-Stack [toepassingen toegang bieden tot Azure-Stack](../azure-stack-create-service-principals.md). 
5.  Zorg ervoor dat uw service-principal heeft de rol van Inzender/eigenaar op uw abonnement. Zie voor instructies over het toewijzen van rollen aan de service-principal [toepassingen toegang bieden tot Azure-Stack](../azure-stack-create-service-principals.md).

## <a name="prerequisites"></a>Vereisten

Om te kunnen Python Azure SDK met de Azure-Stack gebruiken, moet u de volgende waarden opgeven en vervolgens waarden met omgevingsvariabelen instellen. Zie de instructies na de tabel voor uw besturingssysteem voor het instellen van de omgevingsvariabelen. 

| Waarde | Omgevingsvariabelen | Beschrijving |
|---------------------------|-----------------------|-------------------------------------------------------------------------------------------------------------------------|
| Tenant-id | AZURE_TENANT_ID | De waarde van uw Azure-Stack [tenant-ID](../azure-stack-identity-overview.md). |
| Client-id | AZURE_CLIENT_ID | De service principal toepassings-ID opgeslagen wanneer de service-principal is gemaakt op de vorige sectie van dit document. |
| Abonnements-id | AZURE_SUBSCRIPTION_ID | De [abonnements-ID](../azure-stack-plan-offer-quota-overview.md#subscriptions) is hoe u toegang hebben tot aanbiedingen in Azure-Stack. |
| Clientgeheim | AZURE_CLIENT_SECRET | De principal servicetoepassing geheim opgeslagen bij het service-principal is gemaakt. |
| Resource Manager-eindpunt | ARM_ENDPOINT | Zie [de Stack van Azure resource manager-eindpunt](azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint). |


## <a name="python-samples-for-azure-stack"></a>Python-voorbeelden voor Azure-Stack 

De volgende codevoorbeelden kunt u algemene beheertaken uitvoeren voor virtuele machines in uw Azure-Stack.

De codevoorbeelden tonen u:

- Virtuele machines maken:
    - Een virtuele Linux-machine maken
    - Maak een virtuele Windows-machine
- Een virtuele machine bijwerken:
    - Vouw een station
    - Labels van een virtuele machine
    - Gegevensschijven koppelen
    - Gegevensschijven ontkoppelen
- Een virtuele machine worden uitgevoerd:
    - Een virtuele machine starten
    - Een virtuele machine stoppen
    - Een virtuele machine opnieuw opstarten
- Lijst met virtuele machines
- Een virtuele machine verwijderen

Als u wilt bekijken de code voor deze bewerkingen uitvoeren, bekijk de **run_example()** functie in het script Python **Hybrid/unmanaged-disks/example.py** in de GitHub-Repo [ virtuele machines-python-beheren](https://github.com/viananth/virtual-machines-python-manage/tree/8643ed4bec62aae6fdb81518f68d835452872f88).

Elke bewerking is duidelijk gelabeld met een opmerking en een functie print.
De voorbeelden zijn niet noodzakelijkerwijs in de volgorde van de bovenstaande lijst.


## <a name="run-the-python-sample"></a>De Python-voorbeeld uitvoeren

1.  Als u dit nog niet hebt [Python installeren](https://www.python.org/downloads/).

    Dit voorbeeld (en de SDK) is compatibel met Python 2.7 3.4, 3.5 en 3.6.

2.  Algemene aanbevelingen voor de ontwikkeling van een Python is het gebruik van een virtuele omgeving. 
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

4.  De afhankelijkheden die met behulp van pip installeren.

    ````bash
    cd virtual-machines-python-manage\Hybrid
    pip install -r requirements.txt
    ````

5.  Maak een [service-principal](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals) werken met Azure-Stack. Zorg ervoor dat uw service-principal [rol van Inzender/eigenaar](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal) op uw abonnement.

6.  De volgende variabelen instellen en deze omgevingsvariabelen exporteren naar uw huidige shell. 

    ````bash
    export AZURE_TENANT_ID={your tenant id}
    export AZURE_CLIENT_ID={your client id}
    export AZURE_CLIENT_SECRET={your client secret}
    export AZURE_SUBSCRIPTION_ID={your subscription id}
    export ARM_ENDPOINT={your AzureStack Resource Manager Endpoint}
    ```

7.  In order to run this sample, Ubuntu 16.04-LTS and WindowsServer 2012-R2-Datacenter images must be present in Azure Stack market place. These can be either [downloaded from Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-download-azure-marketplace-item) or [added to Platform Image Repository](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-vm-image).

8. Run the sample.

    ```
    python unmanaged-disks\example.py
    ```

## Notes

You may be tempted to try to retrieve a VM's OS disk by using
`virtual_machine.storage_profile.os_disk`.
In some cases, this may do what you want,
but be aware that it gives you an `OSDisk` object.
In order to update the OS Disk's size, as `example.py` does,
you need not an `OSDisk` object but a `Disk` object.
`example.py` gets the `Disk` object with the following:

```python
os_disk_name = virtual_machine.storage_profile.os_disk.name
os_disk = compute_client.disks.get(GROUP_NAME, os_disk_name)
```

## Next steps

- [Azure Python Development Center](https://azure.microsoft.com/develop/python/)
- [Azure Virtual Machines documentation](https://azure.microsoft.com/services/virtual-machines/)
- [Learning Path for Virtual Machines](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/)
- If you don't have a Microsoft Azure subscription, you can get a FREE trial account [here](http://go.microsoft.com/fwlink/?LinkId=330212).