---
title: Lezen en schrijven van grote gegevensbestanden | Microsoft Docs
description: Lezen en schrijven van grote bestanden in Azure Machine Learning-experimenten.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/10/2017
ms.openlocfilehash: 5a772f8792c02139e45977e207b5be4bebc63a9c
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37908236"
---
# <a name="persisting-changes-and-working-with-large-files"></a>Opslaan van wijzigingen en werken met grote bestanden
Met de Azure Machine Learning experimenten-service, kunt u een verscheidenheid aan uitvoeringsdoelen configureren. Sommige doelen zijn lokale computer, zoals een lokale computer of een Docker-container op een lokale computer. Andere resources zijn externe, zoals een Docker-container op een externe computer of een HDInsight-cluster. Zie voor meer informatie, [overzicht van Azure Machine Learning experimenten-service kan worden uitgevoerd](experimentation-service-configuration.md). 

Voordat u op een doel uitvoeren kunt, moet u de projectmap kopiëren naar de compute-doel. U moet doen dus zelfs met een lokale uitvoering die gebruikmaakt van een lokale tijdelijke map voor dit doel. 

## <a name="execution-isolation-portability-and-reproducibility"></a>Uitvoering van isolatie, draagbaarheid en reproduceerbaarheid
Het doel van dit ontwerp is om te controleren of de isolatie, reproduceerbaarheid en draagbaarheid van de uitvoering. Als u hetzelfde script twee keer worden uitgevoerd, op dezelfde of een andere compute-doel, ontvangt u hetzelfde resultaat. De wijzigingen tijdens de eerste uitvoering mag niet van invloed zijn op die in de tweede uitvoering. Met dit ontwerp kunt u de compute-doelen behandelen als stateless rekenbronnen die, elke hoeven geen relatie voor de taken die worden uitgevoerd nadat ze voltooid zijn.

## <a name="challenges"></a>Uitdagingen
Hoewel dit ontwerp de voordelen van draagbaarheid en herhaalbaarheid biedt, brengt dit ook enkele unieke uitdagingen.

### <a name="persisting-state-changes"></a>Permanente statuswijzigingen
Als uw script de status van de compute-context wijzigt, de wijzigingen zijn niet permanent voor uw volgende uitvoering en ze doorgegeven worden niet automatisch terug naar de clientcomputer. 

Meer specifiek, als uw script maakt u een submap of een bestand wordt geschreven, wordt dat bestand of map niet aanwezig zijn in uw projectmap na de uitvoering. De bestanden worden opgeslagen in een tijdelijke map in de doel-rekenomgeving. U kunt ze gebruiken voor foutopsporing, maar u niet vertrouwen op hun permanente bestaan.

### <a name="working-with-large-files-in-the-project-folder"></a>Werken met grote bestanden in de projectmap

Als de projectmap geen grote bestanden bevat, in rekening worden gebracht latentie wanneer de map wordt gekopieerd naar de doel-compute-omgeving aan het begin van de uitvoering. Zelfs als de uitvoering wordt lokaal uitgevoerd, is er nog steeds onnodige schijfverkeer om te voorkomen. Om deze reden gegevenslimiet we momenteel de maximale projectgrootte op 25 MB.

## <a name="option-1-use-the-outputs-folder"></a>Optie 1: Gebruik de *levert* map
Deze optie is het beter als alle volgende voorwaarden van toepassing zijn:
* Het script wordt geproduceerd voor bestanden.
* U verwacht dat de bestanden te wijzigen met elke experiment.
* U wilt behouden een geschiedenis van deze bestanden. 

De algemene scenario's zijn:
* Een model trainen
* Het maken van een gegevensset
* Een grafiek getekend als een installatiekopiebestand als onderdeel van de uitvoering van uw model-training 

Bovendien wilt u de uitvoer van wordt uitgevoerd, selecteer een uitvoerbestand (zoals een model) die is gemaakt door een eerdere uitvoeren, en vervolgens worden gebruikt voor een latere taak (zoals scoren) met elkaar vergelijken.

U kunt bestanden schrijven naar een map met de naam *levert* is ten opzichte van de hoofdmap. De map ontvangt speciale behandeling door de experimenten-service. Alles wat uw script maakt u in de map tijdens het uitvoeren, zoals een modelbestand, bestand of uitgezette installatiekopiebestand (gezamenlijk bekend als _artefacten_), wordt gekopieerd naar de Azure Blob storage-account dat gekoppeld aan uw experimenten-account nadat de uitvoering is voltooid. De bestanden worden onderdeel van de record van de uitvoeringsgeschiedenis.

Hier volgt een kort voorbeeld van code voor het opslaan van een model in de *levert* map:
```python
import os
import pickle

# m is a scikit-learn model. 
# we serialize it into a mode.plk file under the ./outputs folder.
with open(os.path.join('.', 'outputs', 'model.pkl'), 'wb') as f:    
    pickle.dump(m, f)
```
U kunt een artefact downloaden door te bladeren naar de **uitvoerbestanden** sectie van de detailpagina van het uitvoeren van een bepaalde uitvoeren in Azure Machine Learning Workbench. Selecteer het uitvoeren en selecteer vervolgens de **downloaden** knop. U kunt ook opgeven de `az ml asset download` opdracht in het venster van de opdrachtregelinterface (CLI).

Zie voor een volledig voorbeeld de `iris_sklearn.py` Python-script in de _Iris classificeren_ voorbeeldproject.

## <a name="option-2-use-the-shared-folder"></a>Optie 2: De gedeelde map gebruiken
Als u niet hoeft te houden van een geschiedenis van elke uitvoering van bestanden, zijn met behulp van een gedeelde map die toegankelijk zijn voor onafhankelijke runs een goede optie voor de volgende scenario's: 
- U moet uw script laden van gegevens uit de lokale bestanden, zoals CSV-bestanden of een map met bestanden van een tekst- of afbeeldingsbestand als training of test gegevens. 
- Het script onbewerkte gegevens worden verwerkt en schrijft tussenliggende resultaten genereren, zoals de trainingsgegevens boommodel van text- of image-bestanden, die worden gebruikt in een latere training uitvoeren. 
- Het script wordt een model en uw volgende scoring-script moet het model ophalen en deze gebruiken voor evaluatie. 

Het is belangrijk te weten dat de gedeelde map zich lokaal op uw gekozen compute-doel. Daarom wordt deze optie werkt alleen als alle uw script uitgevoerd die verwijzen naar deze gedeelde map worden uitgevoerd op dezelfde-doel COMPUTE, en de compute-doel is niet gerecycled wordt uitgevoerd.

Door te profiteren van de gedeelde map-functie, kunt u lezen van of schrijven naar een speciale map die wordt geïdentificeerd door een omgevingsvariabele `AZUREML_NATIVE_SHARE_DIRECTORY`. 

### <a name="example"></a>Voorbeeld
Hier volgt een voorbeeld van Python code voor het gebruik van deze map delen te lezen en schrijven naar een tekstbestand:
```python
import os

# write to the shared folder
with open(os.environ['AZUREML_NATIVE_SHARE_DIRECTORY'] + 'test.txt', "w") as f1:
    f1.write(“Hello World”)

# read from the shared folder
with open(os.environ['AZUREML_NATIVE_SHARE_DIRECTORY'] + 'test.txt', "r") as f2:
    text = f2.read()
```

Zie voor een volledig voorbeeld de *iris_sklearn_shared_folder.py* -bestand in de _Iris classificeren_ voorbeeldproject.

Voordat u deze functie gebruiken kunt, u hebt ingesteld de *.compute* enkele eenvoudige configuraties die staan voor de betreffende uitvoeringscontext in het bestand de *aml_config* map. Het werkelijke pad naar de map kan variëren afhankelijk van de compute-doel die u kiest en de waarde die u configureert.

### <a name="configure-local-compute-context"></a>Configureren van de lokale compute-context

U schakelt deze functie in een lokale compute-context, Voeg uw *.compute* bestand de volgende regel, vertegenwoordigt de _lokale_ omgeving (meestal *local.compute*).
```
# local.runconfig
...
nativeSharedDirectory: ~/.azureml/share
...
```

Het pad ~/.azureml/share is het standaardpad naar de basismap. U kunt dit wijzigen op een lokale absolute pad dat toegankelijk is via het script uitgevoerd. Naam van de experimenten-account, Werkruimtenaam en naam van het project worden automatisch toegevoegd aan de basismap-naam, die het volledige pad van de gedeelde map wordt. Bijvoorbeeld, de bestanden kunnen worden geschreven naar (en opgehaald van) het volgende pad als u de voorgaande standaardwaarde:

```
# on Windows
C:\users\<username>\.azureml\share\<exp_acct_name>\<workspace_name>\<proj_name>\

# on macOS
/Users/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/
```

### <a name="configure-the-docker-compute-context-local-or-remote"></a>Configureren van de Docker-compute-context (lokaal of extern)
U schakelt deze functie op een Docker-compute-context, moet u de volgende twee regels toevoegen aan uw lokale of externe Docker *.compute* bestand.

```
# docker.compute
...
sharedVolumes: true
nativeSharedDirectory: ~/.azureml/share
...
```
>[!IMPORTANT]
>De **sharedVolumes** eigenschap moet worden ingesteld op *waar* wanneer u gebruikt de `AZUREML_NATIVE_SHARE_DIRECTORY` omgevingsvariabele voor toegang tot de gedeelde map. Anders mislukt de uitvoering.

De code die in de Docker-container altijd ziet deze gedeelde map als */azureml-share /*. Het mappad, kan zoals gezien door de Docker-container niet worden geconfigureerd. Gebruik niet de naam van deze map in uw code. Gebruik in plaats daarvan altijd de naam van omgevingsvariabele `AZUREML_NATIVE_SHARE_DIRECTORY` om te verwijzen naar deze map. Deze is toegewezen aan een lokale map op de Docker-host van de computer of compute-context. De basismap van deze lokale map is de configureerbare waarde van de `nativeSharedDirectory` instellen in de *.compute* bestand. Het lokale pad van de gedeelde map op de hostcomputer is als u de standaardwaarde het volgende:
```
# Windows
C:\users\<username>\.azureml\share\<exp_acct_name>\<workspace_name>\<proj_name>\

# macOS
/Users/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/

# Ubuntu Linux
/home/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/
```

>[!NOTE]
>Het pad van de gedeelde map op de lokale schijf is hetzelfde, ongeacht of het een lokale compute-context of een lokale Docker-compute-context. Dit betekent dat u kunt zelfs bestanden delen tussen een lokaal uitvoeren en een lokale Docker-uitvoering.

U kunt gegevens rechtstreeks in deze mappen plaatsen en verwachten dat uw lokale of Docker wordt uitgevoerd op de machine verder kunnen gaan deze. U kunt ook bestanden schrijven naar deze map vanaf uw lokale of Docker wordt uitgevoerd en bestanden in die map ophalen persistent verwacht functionerende van de levenscyclus van de uitvoering.

Zie voor meer informatie, [Azure Machine Learning Workbench uitvoering-configuratiebestanden](experimentation-service-configuration-reference.md).

>[!NOTE]
>De `AZUREML_NATIVE_SHARE_DIRECTORY` omgevingsvariabele wordt niet ondersteund in een HDInsight-compute-context. Het is echter eenvoudig naar hetzelfde resultaat bereiken met behulp van expliciet een absoluut pad van de Azure Blob-opslag om te lezen uit en schrijven naar de gekoppelde blob-opslag.

## <a name="option-3-use-external-durable-storage"></a>Optie 3: Externe duurzame opslag gebruiken

Externe duurzame opslag kunt u persisteren van statuswaarden tijdens de uitvoering. Deze optie is handig in de volgende scenario's:
- De ingevoerde gegevens is al opgeslagen in een duurzame opslag die toegankelijk is vanaf de doel-compute-omgeving.
- De bestanden moeten niet deel uitmaken van de uitvoeringsgeschiedenis records.
- De bestanden moeten worden gedeeld door uitvoeringen in verschillende omgevingen.
- De bestanden moeten kunnen overbruggen van de compute-context zelf.

Deze benadering is het gebruik van Azure Blob-opslag vanuit uw Python of PySpark-code. Hier volgt een kort voorbeeld:

```python
from azure.storage.blob import BlockBlobService
from azure.storage.blob.models import PublicAccess
import glob
import os

ACCOUNT_NAME = "<your blob storage account name>"
ACCOUNT_KEY = "<account key>"
CONTAINER_NAME = "<container name>"

blob_service = BlockBlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

## Create a new container if necessary, or use an existing one
blob_service.create_container(CONTAINER_NAME, fail_on_exist=False, public_access=PublicAccess.Container)

# df is a pandas DataFrame
df.to_csv('mydata.csv', sep='\t', index=False)

# Export the mydata.csv file to Blob storage.
for name in glob.iglob('mydata.csv'):
    blob_service.create_blob_from_path(CONTAINER_NAME, 'single_file.csv', name)
```

Hier volgt een kort voorbeeld voor het koppelen van elke willekeurige Azure-blobopslag naar een HDI Spark-runtime:
```python
def attach_storage_container(spark, account, key):
    config = spark._sc._jsc.hadoopConfiguration()
    setting = "fs.azure.account.key." + account + ".blob.core.windows.net"
    if not config.get(setting):
        config.set(setting, key)
 
attach_storage_container(spark, "<storage account name>", "<storage key>”)
```

## <a name="conclusion"></a>Conclusie
Omdat Azure Machine Learning wordt de scripts uitgevoerd door de hele projectmap kopiëren naar de doel-compute-context, neemt u speciale aandacht met grote invoer, uitvoer en tussenliggende bestanden. Voor grote Bestandstransacties, kunt u de map speciale uitvoer, de gedeelde map die toegankelijk is via de `AZUREML_NATIVE_SHARE_DIRECTORY` omgevingsvariabele of externe duurzame opslag. 

## <a name="next-steps"></a>Volgende stappen
- Controleer de [Azure Machine Learning Workbench uitvoering-configuratiebestanden](experimentation-service-configuration-reference.md) artikel.
- Zie hoe de [Iris classificeren](tutorial-classifying-iris-part-1.md) in deze les gebruikt u de uitvoer-map om vast te leggen van een getraind model.
