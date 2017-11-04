---
title: Grote hoeveelheden gegevensbestanden lezen en schrijven | Microsoft Docs
description: Lezen en schrijven van grote bestanden in Azure Machine Learning-experimenten.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/10/2017
ms.openlocfilehash: f5c75b95d9019c15bb402313ce7407fa9abb81d4
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="persisting-changes-and-working-with-large-files"></a>Het behouden blijven van wijzigingen en werken met grote bestanden
U kunt met de service Azure Machine Learning-experimenten tal van uitvoering van doelen configureren. Sommige doelen zijn lokale computer, zoals een lokale computer of een Docker-container op een lokale computer. Andere zijn externe, zoals een Docker-container op een externe computer of een HDInsight-cluster. Zie voor meer informatie [overzicht van Azure Machine Learning-experiment uitvoering service](experimentation-service-configuration.md). 

Voordat u op een doel uitvoeren kunt, moet u de projectmap kopiëren naar de compute-doel. U moet doen zelfs met een lokale uitvoering die gebruikmaakt van een lokale tijdelijke map voor dit doel. 

## <a name="execution-isolation-portability-and-reproducibility"></a>Uitvoering van isolatie en draagbaarheid reproduceerbaarheid
Het doel van dit ontwerp is om te controleren of de isolatie, reproduceerbaarheid en draagbaarheid van de uitvoering. Als u hetzelfde script tweemaal worden uitgevoerd, op dezelfde of een andere compute-doel, wordt dezelfde resultaten. De wijzigingen zijn aangebracht tijdens de eerste uitvoering mag niet van invloed zijn op die in de tweede uitvoering. Bij dit ontwerp kunt u de compute-doelen behandelen als staatloze berekening resources, elke hoeven geen relatie aan de taken die worden uitgevoerd nadat ze klaar zijn.

## <a name="challenges"></a>Uitdagingen
Hoewel dit ontwerp de voordelen van draagbaarheid en herhaalbaarheid biedt, brengt dit ook enkele unieke uitdagingen.

### <a name="persisting-state-changes"></a>Statuswijzigingen persistent maken
Als uw script de status van de compute-context wijzigt, de wijzigingen zijn niet permanent opgeslagen voor uw volgende uitvoering en ze doorgegeven wordt niet automatisch terug naar de clientcomputer. 

Meer specifiek, als uw script een submap maakt of schrijft u een bestand, zich map of het bestand niet aanwezig zijn in uw projectmap na de uitvoering. De bestanden worden opgeslagen in een tijdelijke map in de doelomgeving compute. U kunt ze gebruiken voor foutopsporing, maar u niet vertrouwen op hun permanente bestaan.

### <a name="working-with-large-files-in-the-project-folder"></a>Werken met grote bestanden in de projectmap

Als de projectmap geen grote bestanden bevat, tot latentie wanneer de map wordt gekopieerd naar de compute doelomgeving aan het begin van de uitvoering van een. Zelfs als de uitvoering lokaal gebeurt, is er nog steeds onnodige schijf verkeer om te voorkomen. Om deze reden cap we momenteel de projectgrootte van de maximale van 25 MB.

## <a name="option-1-use-the-outputs-folder"></a>Optie 1: Gebruik de *levert* map
Deze optie is beter als alle volgende voorwaarden van toepassing:
* Het script wordt geproduceerd, bestanden.
* Verwacht u dat de bestanden te wijzigen met elke experiment.
* Wilt u deze bestanden bijhouden. 

Algemene gebruiksvoorbeelden zijn:
* Een model trainen
* Maakt een gegevensset
* Een grafiek uitzetten als afbeelding als onderdeel van de uitvoering van uw model trainen 

Bovendien wilt u de uitvoer van wordt uitgevoerd, selecteer een bestand voor uitvoer (zoals een model) die is geproduceerd door een eerdere uitvoeren en vervolgens worden gebruikt voor een volgende taak (zoals score berekenen) met elkaar vergelijken.

U kunt bestanden schrijven naar een map met de naam *levert* die is gebaseerd op de hoofdmap. De map ontvangt speciale behandeling door de service experimenteren. Alles wat uw script maakt in de map tijdens het uitvoeren, zoals een modelbestand, gegevensbestand of uitgezette installatiekopiebestand (gezamenlijk bekend als _artefacten_), wordt gekopieerd naar de Azure Blob storage-account dat gekoppeld aan uw experimenteren account nadat de uitvoering is voltooid. De bestanden uitmaken deel van uw uitvoeringsgeschiedenis-record.

Hier volgt een korte voorbeeld van code voor het opslaan van een model in de *levert* map:
```python
import os
import pickle

# m is a scikit-learn model. 
# we serialize it into a mode.plk file under the ./outputs folder.
with open(os.path.join('.', 'outputs', 'model.pkl'), 'wb') as f:    
    pickle.dump(m, f)
```
U kunt een artefact downloaden door te bladeren naar de **uitvoerbestanden** sectie van de detailpagina voor het uitvoeren van een bepaalde uitvoeren in Azure Machine Learning-Workbench. Schakelt de uitvoeren en selecteer vervolgens de **downloaden** knop. U kunt ook kunt u de `az ml asset download` opdracht in het venster opdrachtregelinterface (CLI).

Zie voor een uitgebreider voorbeeld de `iris_sklearn.py` Python script in de _Iris classificeren_ voorbeeldproject.

## <a name="option-2-use-the-shared-folder"></a>Optie 2: De gedeelde map gebruiken
Als u niet hoeft te houden van een overzicht van elke uitvoering bestanden, zijn met behulp van een gedeelde map die toegankelijk zijn voor onafhankelijke uitvoert een goede optie voor de volgende scenario's: 
- Het script moet gegevens van lokale bestanden, zoals CSV-bestanden of een map met bestanden van een tekst- of afbeeldingsbestand als training of test gegevens te laden. 
- Uw script onbewerkte gegevens worden verwerkt en schrijft tussenliggende resultaten, zoals de trainingsgegevens featurized uit tekst of afbeelding bestanden die worden gebruikt in een latere training uitvoeren. 
- Het script wordt een model en het volgende script scoreprofiel moet het model kunnen worden opgepikt en deze gebruiken voor evaluatie. 

Het is belangrijk te weten dat de gedeelde map zich lokaal op uw gekozen compute-doel. Daarom deze optie werkt alleen als alle uw script wordt uitgevoerd die verwijzen naar deze gedeelde map worden uitgevoerd op hetzelfde doel berekenen en de compute-doel is niet gerecycled tussen wordt uitgevoerd.

Door gebruik te maken van de functie voor de gedeelde map, kunt u lezen uit of schrijven naar een speciale map die wordt geïdentificeerd door een omgevingsvariabele `AZUREML_NATIVE_SHARE_DIRECTORY`. 

### <a name="example"></a>Voorbeeld
Hier volgt een voorbeeld Python code voor het gebruik van deze map delen te lezen en schrijven naar een tekstbestand:
```python
import os

# write to the shared folder
with open(os.environ['AZUREML_NATIVE_SHARE_DIRECTORY'] + 'test.txt', 'wb') as f:
    f.write(“Hello World”)

# read from the shared folder
with open(os.environ['AZUREML_NATIVE_SHARE_DIRECTORY'] + 'test.txt', 'r') as f:
    text = file.read()
```

Zie voor een uitgebreider voorbeeld de *iris_sklearn_shared_folder.py* bestand de _Iris classificeren_ voorbeeldproject.

Voordat u deze functie gebruiken kunt, u hebt ingesteld de *.compute* bestand enkele eenvoudige configuraties die de betreffende uitvoeringscontext in vertegenwoordigen de *aml_config* map. Het werkelijke pad naar de map kan variëren afhankelijk van de compute-doel die u kiest en de waarde die u configureert.

### <a name="configure-local-compute-context"></a>Configureren van lokale compute-context

U schakelt deze functie in een lokale compute-context, toe te voegen aan uw *.compute* bestand de volgende regel vertegenwoordigt de _lokale_ omgeving (meestal met de naam *local.compute*).
```
# local.runconfig
...
nativeSharedDirectory: ~/.azureml/share
...
```

Het pad ~/.azureml/share is het standaardpad basismap. U kunt dit wijzigen op een lokale absolute pad die toegankelijk is door het script uitvoeren. De accountnaam experimenteren, Werkruimtenaam en projectnaam worden automatisch toegevoegd aan de basismap van de naam, die het volledige pad van de gedeelde map wordt. Bijvoorbeeld, de bestanden kunnen worden geschreven naar (en opgehaald van) het volgende pad als u de voorgaande standaardwaarde:

```
# on Windows
C:\users\<username>\.azureml\share\<exp_acct_name>\<workspace_name>\<proj_name>\

# on macOS
/Users/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/
```

### <a name="configure-the-docker-compute-context-local-or-remote"></a>Configureer de Docker compute-context (lokaal of extern)
U schakelt deze functie in een Docker compute-context, moet u de volgende twee regels toevoegen aan uw lokale of externe Docker *.compute* bestand.

```
# docker.compute
...
sharedVolumes: true
nativeSharedDirectory: ~/.azureml/share
...
```
>[!IMPORTANT]
>De **sharedVolumes** eigenschap moet worden ingesteld op *true* wanneer u gebruikt de `AZUREML_NATIVE_SHARE_DIRECTORY` omgevingsvariabele voor toegang tot de gedeelde map. Anders wordt de uitvoering mislukt.

De code die wordt uitgevoerd in de Docker-container altijd ziet deze gedeelde map als */azureml-share /*. Het mappad, kan zoals weergegeven door de Docker-container niet worden geconfigureerd. Gebruik niet de naam van deze map in uw code. Gebruik in plaats daarvan de naam van omgevingsvariabele altijd `AZUREML_NATIVE_SHARE_DIRECTORY` om te verwijzen naar deze map. Is toegewezen aan een lokale map op de host Docker machine of compute-context. De basismap van deze lokale map is de configureerbare waarde van de `nativeSharedDirectory` instellen in de *.compute* bestand. Het lokale pad van de gedeelde map op de hostcomputer is als u de standaardwaarde het volgende:
```
# Windows
C:\users\<username>\.azureml\share\<exp_acct_name>\<workspace_name>\<proj_name>\

# macOS
/Users/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/

# Ubuntu Linux
/home/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/
```

>[!NOTE]
>Het pad van de gedeelde map op de lokale schijf is hetzelfde, ongeacht of dit de context van een lokale compute of een lokale Docker compute-context. Dit betekent dat u kunt zelfs bestanden delen tussen een lokale uitvoeren en een lokale Docker uitvoeren.

U kunt de invoergegevens rechtstreeks in deze mappen te plaatsen en verwachten dat uw lokale of Docker wordt uitgevoerd op de machine kunnen worden opgepikt deze. U kunt ook bestanden schrijven naar deze map op uw lokale of Docker wordt uitgevoerd en bestanden in die map ophalen persistent verwachten functionerende van de levenscyclus van de uitvoering.

Zie voor meer informatie [Azure Machine Learning Workbench uitvoering configuratiebestanden](experimentation-service-configuration-reference.md).

>[!NOTE]
>De `AZUREML_NATIVE_SHARE_DIRECTORY` omgevingsvariabele wordt niet ondersteund in de context van een HDInsight-berekeningen. Het is echter eenvoudig hetzelfde resultaat bereiken met behulp van een absoluut pad van de Azure Blob-opslag expliciet te lezen van en schrijven naar de gekoppelde blob-opslag.

## <a name="option-3-use-external-durable-storage"></a>Optie 3: Externe duurzame opslag gebruiken

Externe duurzame opslag kunt u de status behouden tijdens de uitvoering. Deze optie is handig in de volgende scenario's:
- De ingevoerde gegevens is al opgeslagen in duurzame opslag die toegankelijk is vanaf de doelomgeving compute.
- De bestanden moeten niet deel uitmaken van de uitvoeringsgeschiedenis records.
- De bestanden moeten worden gedeeld door uitvoeringen tussen de verschillende compute-omgevingen.
- De bestanden moeten kunnen blijven bewaard als de compute-context zelf.

Een dergelijke aanpak is het gebruik van Azure Blob storage vanuit uw code Python of PySpark. Hier volgt een voorbeeld van een korte:

```python
from azure.storage.blob import BlockBlobService
import glob
import os

ACCOUNT_NAME = "<your blob storage account name>"
ACCOUNT_KEY = "<account key>"
CONTAINER_NAME = "<container name>"

blob_service = BlockBlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

## Create a new container if necessary, or use an existing one
my_service.create_container(CONTAINER_NAME, fail_on_exist=False, public_access=PublicAccess.Container)

# df is a pandas DataFrame
df.to_csv('mydata.csv', sep='\t', index=False)

# Export the mydata.csv file to Blob storage.
for name in glob.iglob('mydata.csv'):
    blob_service.create_blob_from_path(CONTAINER_NAME, 'single_file.csv', name)
```

Hier volgt een voorbeeld van een korte voor elke willekeurige Azure-blobopslag koppelen aan een HDI Spark-runtime:
```python
def attach_storage_container(spark, account, key):
    config = spark._sc._jsc.hadoopConfiguration()
    setting = "fs.azure.account.key." + account + ".blob.core.windows.net"
    if not config.get(setting):
        config.set(setting, key)
 
attach_storage_container(spark, "<storage account name>", "<storage key>”)
```

## <a name="conclusion"></a>Conclusie
Omdat Azure Machine Learning wordt scripts uitgevoerd door de hele projectmap kopiëren naar de doel-compute-context, letten met grote invoer, uitvoer en tussenliggende bestanden. Voor grote Bestandstransacties, kunt u de map speciale uitvoer, de gedeelde map die toegankelijk is via de `AZUREML_NATIVE_SHARE_DIRECTORY` omgevingsvariabele of externe duurzame opslag. 

## <a name="next-steps"></a>Volgende stappen
- Controleer de [Azure Machine Learning Workbench uitvoering configuratiebestanden](experimentation-service-configuration-reference.md) artikel.
- Zie hoe de [Iris classificeren](tutorial-classifying-iris-part-1.md) zelfstudie project maakt gebruik van de map uitvoer voor het persistent maken van een getraind model.
