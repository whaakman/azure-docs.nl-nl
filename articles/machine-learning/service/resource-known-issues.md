---
title: Bekende problemen en probleemoplossing
titleSuffix: Azure Machine Learning service
description: Een lijst van de bekende problemen, oplossingen, en probleemoplossing voor Azure Machine Learning-service.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 5634a1aae32b3e9895bf5c5b72837f29223bca27
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54381825"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Bekende problemen en oplossen van problemen met Azure Machine Learning-service
 
Dit artikel helpt u bij het zoeken en corrigeer de fouten of fouten opgetreden bij het gebruik van de Azure Machine Learning-service. 

## <a name="sdk-installation-issues"></a>Problemen met de SDK-installatie

**Foutbericht: Cannot uninstall 'PyYAML'** 

Azure Machine Learning-SDK voor Python: PyYAML is een geïnstalleerde distutils-project. We kan geen nauwkeurig bepalen welke bestanden deel uitmaken van deze in het geval van een gedeeltelijke verwijderen. Gebruik het volgende om door te gaan met het installeren van de SDK tijdens deze fout negeren:
```Python 
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

## <a name="trouble-creating-azure-machine-learning-compute"></a>Problemen bij het maken van Azure Machine Learning-Computing

Er wordt een zeldzaam kans dat sommige gebruikers die hun Azure Machine Learning-werkruimte hebt gemaakt vanuit de Azure-portal voordat u de GA-versie niet mogelijk te maken van Azure Machine Learning-Computing in de werkruimte. U kunt een ondersteuningsaanvraag indient voor de service te verhogen of een nieuwe werkruimte via de Portal of de SDK om de blokkering zelf onmiddellijk te maken. 

## <a name="image-building-failure"></a>Installatiekopie samenstellen fout

De installatiekopie van het bouwen van fout bij het implementeren van web-service. Tijdelijke oplossing is om toe te voegen ' pynacl 1.2.1 == "als een afhankelijkheid pip Conda-bestand voor de configuratie van installatiekopie.  

## <a name="deployment-failure"></a>Fout bij de implementatie

Als u ziet ' DaskOnBatch:context_managers. DaskOnBatch', 'Setup.PY gebruikt']' is verbroken met < Signals.SIGKILL: 9 >-wijzig de SKU voor virtuele machines in uw implementatie te hoeven meer geheugen gebruikt.

## <a name="fpgas"></a>FPGAs
Niet mogelijk om te implementeren van modellen op FPGA's totdat u hebt aangevraagd en goedgekeurd voor FPGA quotum. Vul het aanvraagformulier voor de quota voor het aanvragen van toegang: https://aka.ms/aml-real-time-ai

## <a name="databricks"></a>Databricks

Problemen met Databricks en Azure Machine Learning.
 
1. AML SDK installeren is mislukt op Databricks wanneer meer pakketten zijn geïnstalleerd.

   Sommige pakketten, zoals `psutil`, kan leiden tot conflicten. Om installatiefouten te voorkomen,-pakketten door blokkering lib versie te installeren. Dit probleem is met betrekking tot Databricks en niet met betrekking tot Azure ML-SDK - die zich kunnen voordoen deze met andere bibliotheken te. Voorbeeld:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
   ```
   U kunt ook init scripts gebruiken als u problemen met Python-bibliotheken installeren houden aangesloten. Deze benadering is niet een aanpak die officieel ondersteund. U kunt verwijzen naar [dit document](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

2. Wanneer u automatische Machine Learning op Databricks, als u wilt een uitvoering annuleren en start een nieuw experiment uitvoeren, start u uw Azure Databricks-cluster.

3. In de automatische ml-instellingen, wanneer u > 10 iteraties Stel show_output als onwaar bij het indienen van uw uitvoering.


## <a name="azure-portal"></a>Azure Portal
Als u rechtstreeks naar het weergeven van uw werkruimte van een koppeling voor het delen van de SDK of de portal gaat, wordt het niet mogelijk om de normale overzichtspagina met abonnementsgegevens in de extensie weer te geven. U wordt ook niet mogelijk om over te schakelen naar een andere werkruimte. Als u nodig hebt om een andere werkruimte weer te geven, de tijdelijke oplossing is het gaat u rechtstreeks naar de [Azure-portal](https://portal.azure.com) en zoek de naam van de werkruimte.

## <a name="diagnostic-logs"></a>Diagnostische logboeken
Soms kan het handig zijn als u diagnostische gegevens opgeven kunt wanneer u hulp vragen. Dit is waar de logboekbestanden bevinden:

## <a name="resource-quotas"></a>Resourcequota

Meer informatie over de [resourcequota](how-to-manage-quotas.md) optreden tijdens het werken met Azure Machine Learning.

## <a name="get-more-support"></a>Meer ondersteuning krijgen

U kunt aanvragen verzenden voor ondersteuning en hulp krijgen van de technische ondersteuning, forums en meer. [Meer informatie...](support-for-aml-services.md)
