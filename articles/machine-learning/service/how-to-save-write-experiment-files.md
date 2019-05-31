---
title: Beperkingen van storage voorkomen en experimenteren latentie met invoer- en -mappen
description: In dit artikel leert u waar om op te slaan, uw experiment-invoerbestanden en waar de uitvoerbestanden om te voorkomen dat opslag beperking fouten en experimenteren latentie schrijven.
services: machine-learning
author: rastala
ms.author: roastala
manager: danielsc
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 05/28/2019
ms.openlocfilehash: 28d8c47db8ea9c8a51bc8e9deb0a689eb0b20177
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66392910"
---
# <a name="where-to-save-and-write-files-for-azure-machine-learning-experiments"></a>Wanneer u bestanden wilt opslaan en schrijven voor Azure Machine Learning-experimenten

In dit artikel leert u waar om op te slaan invoerbestanden en waar de uitvoerbestanden van de experimenten nodig hebben om te voorkomen dat opslag fouten beperken en experimenteren latentie schrijven.

Wanneer de training starten wordt uitgevoerd op een [compute-doel](how-to-set-up-training-targets.md), ze zijn geïsoleerd van buiten-omgevingen. Het doel van dit ontwerp is om ervoor te zorgen reproduceerbaarheid en draagbaarheid van het experiment. Als u twee keer hetzelfde script uitvoert, op dezelfde of een andere compute-doel, ontvangt u hetzelfde resultaat. Met dit ontwerp kunt u de compute-doelen behandelen als stateless rekenbronnen die, elke hoeven geen affiniteit met de taken die worden uitgevoerd nadat ze voltooid zijn.

## <a name="where-to-save-input-files"></a>Waar om op te slaan invoerbestanden

Voordat u een experiment op een compute-doel of uw lokale computer starten kunt, moet u ervoor zorgen dat de benodigde bestanden beschikbaar voor deze compute-doel, zoals de afhankelijkheid en gegevensbestanden die uw code moet zijn worden uitgevoerd.

Azure Machine Learning trainingsscripts uitgevoerd door de gehele script-map kopiëren naar de doel-compute-context en een momentopname. De limiet voor opslag voor momentopnamen van het experiment is 300 MB en/of 2000-bestanden.

Daarom raden we het volgende aan:

* **Uw bestanden opslaan in een Azure Machine Learning [gegevensopslag](https://docs.microsoft.com/python/api/azureml-core/azureml.data?view=azure-ml-py).** Dit voorkomt dat experiment latentieproblemen en heeft de voordelen van toegang tot gegevens op een externe compute-doel, wat betekent dat de verificatie en koppelen worden beheerd door Azure Machine Learning-service. Meer informatie over het opgeven van een gegevensarchief als de bronmap en bestanden uploaden naar uw gegevensopslag in de [toegang tot gegevens van uw gegevensopslag](how-to-access-data.md) artikel.

* **Als u alleen een aantal gegevensbestanden hoeft en afhankelijkheid scripts en een gegevensarchief kan niet worden gebruikt** plaatst u de bestanden in dezelfde mapdirectory als uw trainingsscript. Specificeert u deze map als uw `source_directory` rechtstreeks in uw trainingsscript, of in de code die uw trainingsscript aanroept.

<a name="limits"></a>

### <a name="storage-limits-of-experiment-snapshots"></a>Opslagbeperkingen van momentopnamen van experiment

Voor experimenten maakt Azure Machine Learning automatisch een momentopname van een experiment van uw code op basis van de map die u bij het configureren van de uitvoering voorstellen. Dit heeft een limiet van 300 MB en/of 2000-bestanden. Als u deze limiet overschrijdt, ziet u de volgende fout:

```Python
While attempting to take snapshot of .
Your total snapshot size exceeds the limit of 300.0 MB
```

U kunt deze fout oplossen door uw experiment bestanden opslaat op een gegevensarchief. Als u niet een gegevensopslag, gebruiken de onderstaande tabel biedt mogelijke alternatieve oplossingen.

Experiment&nbsp;beschrijving|Limiet opslagoplossing
---|---
Minder dan 2000-bestanden en een gegevensarchief kan niet worden gebruikt| Grootte van de momentopname met overschrijven <br> `azureml._restclient.snapshots_client.SNAPSHOT_MAX_SIZE_BYTES = 'insert_desired_size'`<br> Dit kan enige tijd duren, afhankelijk van het aantal en de grootte van bestanden.
Moet specifiek script directory gebruiken| Controleer een `.amlignore` bestand wilt uitsluiten van bestanden vanuit de momentopname van uw experiment die geen deel uitmaken van de broncode. Toevoegen van de namen van bestanden naar de `.amlignore` -bestand en plaats deze in dezelfde map als het trainingsscript. De `.amlignore` bestand maakt gebruik van dezelfde [syntaxis en patronen](https://git-scm.com/docs/gitignore) als een `.gitignore` bestand.
Pijplijn|Gebruik een andere submap voor elke stap
Jupyter-notebooks| Maak een `.amlignore` bestand of uw laptop verplaatsen naar een nieuwe, lege submap en voer uw code opnieuw uit.

## <a name="where-to-write-files"></a>Waar u bestanden te schrijven

Vanwege de isolatie van training experimenten, de wijzigingen in bestanden die ontstaan tijdens uitvoeringen zijn niet noodzakelijkerwijs permanent opgeslagen buiten uw omgeving. Als uw script de lokale compute-bestanden wijzigt, de wijzigingen zijn niet permanent voor uw volgende experiment uitvoert en ze doorgegeven zijn niet automatisch terug naar de clientcomputer. Daarom worden de wijzigingen tijdens de eerste experiment uitvoeren niet en mag niet van invloed zijn op die in de tweede.

Bij het schrijven van wijzigingen, wordt u aangeraden bestanden schrijven naar een Azure Machine Learning-gegevensopslag. Zie [toegang tot gegevens van uw gegevensopslag](how-to-access-data.md).

Als u een gegevensarchief niet vereist, schrijft u bestanden naar de `./outputs` en/of `./logs` map.

>[!Important]
> Twee mappen *levert* en *logboeken*, speciale behandeling door Azure Machine Learning. Tijdens de training, bij het schrijven van bestanden naar`./outputs` en`./logs` mappen, worden de bestanden worden automatisch uploaden naar uw uitvoeringsgeschiedenis, zodat u de toegang tot hebben als uw uitvoering is voltooid.

* **Voor uitvoer zoals statusberichten of scoring resultaten** schrijven van bestanden naar de `./outputs` map, zodat ze worden doorgevoerd als artefacten in de uitvoeringsgeschiedenis. Worden Houd ook rekening met het aantal en de grootte van bestanden die worden weggeschreven naar deze map, zoals latentie optreden kan wanneer de inhoud naar de uitvoeringsgeschiedenis worden geüpload. Als de latentie is een probleem, wordt het schrijven van bestanden naar een gegevensarchief aanbevolen.

* **Geschreven opslaan als Logboeken in de uitvoeringsgeschiedenis,** bestanden schrijven `./logs` map. De logboeken zijn geüpload in realtime, zodat deze methode geschikt is voor het streamen van live-updates vanaf een externe uitvoeren.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [toegang krijgen tot gegevens uit uw gegevensopslag](how-to-access-data.md).

* Meer informatie over [Training doelen instellen hoe](how-to-set-up-training-targets.md).
