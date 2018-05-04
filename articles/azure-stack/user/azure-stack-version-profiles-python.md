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
ms.date: 04/30/2018
ms.author: mabrigg
ms.reviewer: sijuman
<!-- dev: viananth -->
ms.openlocfilehash: a4fe62ba8c0732745326831b977e8975e1210436
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2018
---
# <a name="use-api-version-profiles-with-python-in-azure-stack"></a>API-versie profielen met behulp van Python in Azure-Stack gebruiken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

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

5.  Maak een [service-principal](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-service-principals) werken met Azure-Stack. Zorg ervoor dat uw service-principal [rol van Inzender/eigenaar](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal) op uw abonnement.

6.  De volgende variabelen instellen en deze omgevingsvariabelen exporteren naar uw huidige shell. 

`Note: provide an explanation of where these variables come from?`

    ````bash
    export AZURE_TENANT_ID={your tenant id}
    export AZURE_CLIENT_ID={your client id}
    export AZURE_CLIENT_SECRET={your client secret}
    export AZURE_SUBSCRIPTION_ID={your subscription id}
    export ARM_ENDPOINT={your AzureStack Resource Manager Endpoint}
    ```

7.  Houd er rekening mee dat om uit te voeren in dit voorbeeld, Ubuntu 16.04-LTS en installatiekopieën van Windows Server 2012-R2-Datacenter aanwezig in de Stack van het Azure-marktplaats zijn moeten. Dit kunnen zijn [gedownload van Azure](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-download-azure-marketplace-item) of [toegevoegd aan de opslagplaats voor installatiekopieën Platform](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-add-vm-image).


8. Voet het voorbeeld uit.

    ```
    python unmanaged-disks\example.py
    ```

## <a name="notes"></a>Opmerkingen

Hebt u mogelijk geneigd om te proberen op te halen van de besturingssysteemschijf van een virtuele machine met behulp van `virtual_machine.storage_profile.os_disk`.
In sommige gevallen kan dit doen wat u wilt dat, maar let erop dat dit u biedt een `OSDisk` object.
Om bij te werken van de grootte van de Besturingssysteemschijf als `example.py` , u hoeft niet een `OSDisk` object maar er is een `Disk` object.
`example.py` haalt de `Disk` object met het volgende:

```python
os_disk_name = virtual_machine.storage_profile.os_disk.name
os_disk = compute_client.disks.get(GROUP_NAME, os_disk_name)
```

## <a name="next-steps"></a>Volgende stappen

- [Azure Python Development Center](https://azure.microsoft.com/develop/python/)
- [Documentatie bij Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/)
- [Leertraject voor virtuele Machines](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/)
- Als u geen Microsoft Azure-abonnement hebt kunt u een gratis proefaccount [hier](http://go.microsoft.com/fwlink/?LinkId=330212).
