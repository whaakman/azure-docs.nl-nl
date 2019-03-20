---
title: Wat gebeurt er met Azure Batch AI? | Microsoft Docs
description: Meer informatie over wat er met Azure Batch AI en de compute-optie van de Azure Machine Learning-service gebeurt.
ms.service: batch-ai
services: batch-ai
ms.topic: overview
ms.author: jmartens
author: j-martens
ms.date: 2/28/2019
ms.openlocfilehash: edd6a7e5f385d766d51631d77f5889233af2469a
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57194499"
---
# <a name="whats-happening-to-azure-batch-ai"></a>Wat gebeurt er met Azure Batch AI?

**De Azure Batch AI-service wordt in maart buiten gebruik gesteld.** De op-schaal-training en scoremogelijkheden van Batch AI zijn nu beschikbaar in de [Azure Machine Learning-service](../machine-learning/service/overview-what-is-azure-ml.md), die algemeen beschikbaar werd op 4 december 2018.

Samen met veel andere machine learning-mogelijkheden bevat de Azure Machine Learning-service een cloud-gebaseerd, beheerd rekendoel voor het trainen, implementeren en scoren van Machine Learning-modellen. Dit rekendoel heet [Azure Machine Learning Compute](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute). [Begin met migreren en maak er vandaag nog gebruik van](#migrate). U kunt met de Azure Machine Learning-service communiceren via de [Python-SDK's](../machine-learning/service/quickstart-create-workspace-with-python.md), opdrachtregelinterface en de [Azure Portal](../machine-learning/service/quickstart-get-started.md).

## <a name="support-timeline"></a>Ondersteuningstijdlijn

Op dit moment kunt u uw bestaande abonnementen van Azure Batch AI als voordat. Er zijn echter geen **nieuwe abonnementen** mogelijk en er worden geen nieuwe investeringen gedaan.

Vanaf maart&nbsp;31&#x2c;&nbsp;2019, niet-gebruikte Batch AI-abonnementen wordt niet meer werken.

## <a name="compare-to-azure-machine-learning"></a>Vergelijken met Azure Machine Learning
Het is een cloudservice waarmee u Machine Learning-modellen kunt trainen, implementeren, automatiseren en beheren, en dit allemaal op de grote schaal van de cloud. [In dit overzicht](../machine-learning/service/overview-what-is-azure-ml.md) kunt u een dieper inzicht krijgen over Azure Machine Learning Service.
 

Bij een typische ontwikkelingslevenscyclus zijn gegevensvoorbereiding, training en experimenten,en een ontwikkelingsfase betrokken. Deze end-to-end-cyclus kan worden ingedeeld met behulp van Machine Learning-pijplijnen.

![Stroomdiagram](./media/overview-what-happened-batch-ai/lifecycle.png)


[Meer informatie over de werking en de belangrijkste concepten van de service](../machine-learning/service/concept-azure-machine-learning-architecture.md). Veel concepten in de werkstroom van de modeltraining zijn soortgelijk aan die in Batch AI. 

Hieronder is een aantal concepten naast elkaar geplaatst:
 
|Batch AI-service|  Azure Machine Learning-service|
|:--:|:---:|
|Werkruimte|Werkruimte|
|Cluster|   Compute van type `AmlCompute`|
|Bestandsservers|Gegevensopslag|
|Experimenten|Experimenten|
|Taken|Uitvoerbewerkingen (staat geneste uitvoerbewerkingen toe)|
 
Hier ziet u een andere weergave van dezelfde tabel, zodat u de zaken op een andere manier kunt visualiseren:
 
### <a name="batch-ai-hierarchy"></a>Batch AI-hiërarchie
![Stroomdiagram](./media/overview-what-happened-batch-ai/batchai-heirarchy.png) 
 
### <a name="azure-machine-learning-service-hierarchy"></a>Hiërarchie van de service Azure Machine Learning
![Stroomdiagram](./media/overview-what-happened-batch-ai/azure-machine-learning-service-heirarchy.png) 

## <a name="platform-capabilities"></a>Platformfunctionaliteiten
Azure Machine Learning Service bevat een geweldige reeks nieuwe functionaliteiten, waaronder een end-to-end-training -> implementatiestack, waarmee u AI kunt ontwikkelen zonder beheer van Azure-resources. In de onderstaande tabel wordt de ondersteuning van functies voor de training tussen de twee services vergeleken.

|Functie|Batch AI-service|Azure Machine Learning-service|
|-------|:-------:|:-------:|
|Keuze van VM-grootte |CPU/GPU    |CPU/GPU. Ondersteunt tevens FPGA voor inferentie|
|Cluster gereed voor AI (stuurprogramma's, Docker, enz.)|  Ja |Ja|
|Knooppuntvoorbereiding| Ja|    Nee|
|Keuze uit besturingssysteem   |Gedeeltelijk    |Nee|
|Dedicated en LowPriority VM's  |Ja    |Ja|
|Automatisch schalen   |Ja    |Ja (standaard)|
|Wachttijd voor automatisch schalen  |Nee |Ja|
|SSH    |Ja|   Ja|
|Koppeling op clusterniveau |Ja (bestandsshares, blobs, NFS, aangepast)   |Ja (uw gegevensopslag koppelen of downloaden)|
|Gedistribueerde training|  Ja |Ja|
|Modus voor taakuitvoering|    VM of container|    Container|
|Aangepaste containerinstallatiekopie|    Ja |Ja|
|Toolkit    |Ja    |Ja (Python-script uitvoeren)|
|JobPreparation|    Ja |Nog niet|
|Taakniveaukoppeling |Ja (bestandsshares, blobs, NFS, aangepast)   |Ja (bestandsshares, blobs)|
|Taakbewaking     |via GetJob|    via uitvoeringsgeschiedenis (uitgebreidere informatie, aangepaste runtime om meer metrische gegevens te pushen)|
|Taaklogboeken en -bestanden/modellen ophalen |   via ListFiles en opslag-API's  |via Artifact-service|
 |Ondersteuning voor Tensorboard   |Nee|    Ja|
|Quota op niveau van VM-serie |Ja    |Ja (met uw vorige capaciteit getransporteerd)|
 
Naast de functies in de vorige tabel, zijn er functies in Azure Machine Learning Service die voorheen niet in BatchAI werden ondersteund.

|Functie|Batch AI-service|Azure Machine Learning-service|
|-------|:-------:|:-------:|
|Omgevingsvoorbereiding    |Nee |Ja (Conda-voorbereiding en uploaden naar ACR)|
|Hyperparameter-afstemming  |Nee|    Ja|
|Modelbeheer   |Nee |Ja|
|Operationaliseren/implementeren| Nee  |Via AKS en ACI|
|Gegevensvoorbereiding   |Nee |Ja|
|Rekendoelen    |Azure-VM's  |Lokaal, BatchAI (als AmlCompute), DataBricks, HDInsight|
|Geautomatiseerde Machine Learning |Nee|    Ja|
|Pijplijnen  |Nee |Ja|
|Batch scoren  |Ja    |Ja|
|Portal-/CLI-ondersteuning|    Ja |Ja|


## <a name="programming-interfaces"></a>Programmeerinterfaces

In deze tabel worden de verschillende programmeerinterfaces gepresenteerd die voor elke service beschikbaar zijn.
    
|Functie|BatchAI-service|Azure Machine Learning-service|
|-------|:-------:|:-------:|
|SDK    |Java, C#, Python, Node.js   |Python (op configuratie en schatting gebaseerde uitvoering voor algemene frameworks)|
|CLI    |Ja    |Nog niet|
|Azure Portal   |Ja    |Ja (uitgezonderd indienen van taken)|
|REST-API   |Ja    |Ja, maar gedistribueerd over microservices|


Een upgrade uitvoert van de Preview-versie Batch AI met de GA'ed Azure Machine Learning-service biedt u een betere ervaring via concepten die gemakkelijker zijn te gebruiken, zoals loopt en gegevensopslag. Bovendien profiteert u van Azure-SLA's en klantondersteuning op algemeen beschikbaar niveau.

Azure Machine-Learning wordt service ook in de nieuwe functionaliteit, zoals brengt geautomatiseerde machine learning, hyperparameter afstemmen en ML-pijplijnen die nuttig in de meeste grootschalige AI-workloads zijn. De mogelijkheid tot het implementeren van een getraind model over te schakelen op een afzonderlijke service kunt u de data science-lus van voorbereiden van gegevens (met behulp van de SDK voor Data Prep) voltooien helemaal uitoefening en model bewaking.

<a name="migrate"></a>
## <a name="migrate"></a>Migreren

Meer informatie over hoe u migreert en hoe de code die met u wordt toegewezen aan de code in Azure Machine Learning-service in de [migreren naar Azure Machine Learning-service](how-to-migrate.md) artikel.

## <a name="get-support"></a>Ondersteuning krijgen

Batch AI staat gepland buiten gebruik stellen op 31 maart en blokkeert al nieuwe abonnementen op basis van de service wordt geregistreerd, tenzij het wordt in de whitelist opgenomen door een uitzondering door middel van ondersteuning.  Als u vragen of feedback hebt over de migratie naar Azure Machine Learning Service, kunt u contact met ons opnemen via [Azure Batch AI Training Preview](mailto:AzureBatchAITrainingPreview@service.microsoft.com).

Azure Machine Learning Service is een algemeen beschikbare service. Dit betekent dat de service wordt geleverd met een bindende SLA en een keuze uit diverse ondersteuningsplannen.

Prijzen voor het gebruik van Azure-infrastructuur via de Batch AI-service of via de Azure Machine Learning-service moet niet verschillen, omdat we alleen de prijs voor de onderliggende berekeningen in beide gevallen berekenen. Zie de [prijscalculator](https://azure.microsoft.com/pricing/details/machine-learning-service/) voor meer informatie.

Bekijk de regionale beschikbaarheid voor de twee services in de [Azure-portal](https://azure.microsoft.com/global-infrastructure/services/?products=batch-ai,machine-learning-service&regions=all).


## <a name="next-steps"></a>Volgende stappen

+ Informatie over [migreren](how-to-migrate.md) en hoe u nu code wordt toegewezen aan de code in Azure Machine Learning-service.

+ Lees [Overzicht van de Azure Machine Learning-service](../machine-learning/service/overview-what-is-azure-ml.md).

+ [Configureer een rekendoel voor modeltraining](../machine-learning/service/how-to-set-up-training-targets.md) met de Azure Machine Learning-service.

+ Controleer de [Azure-roadmap](https://azure.microsoft.com/updates/) voor meer informatie over andere Azure-service-updates.
