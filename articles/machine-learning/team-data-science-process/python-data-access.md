---
title: Toegang tot gegevenssets met Python-clientbibliotheek - Team Data Science Process
description: Installeer en gebruik van de Python-clientbibliotheek voor toegang tot en beheer van Azure Machine Learning-gegevens veilig vanuit een lokale Python-omgeving.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: bf0e679ab46752d71ba4f5ef2b014e0cb2b4c6ad
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60593998"
---
# <a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a>Toegang tot gegevenssets met Python met behulp van de clientbibliotheek van Azure Machine Learning Python
De Preview-versie van Microsoft Azure Machine Learning Python-clientbibliotheek kunt inschakelen van beveiligde toegang tot uw Azure Machine Learning-gegevenssets van een lokale Python-omgeving en kunt het maken en beheren van gegevenssets in een werkruimte.

In dit onderwerp bevat instructies over het:

* installeren van de Machine Learning Python-clientbibliotheek
* toegang tot en het uploaden van gegevenssets, zoals instructies voor het ophalen van de machtiging voor toegang tot Azure Machine Learning-gegevenssets van uw lokale omgeving voor Python
* toegang tot de tussenliggende gegevenssets van experimenten
* de Python-clientbibliotheek gebruiken om te sommen, gegevenssets, toegang tot metagegevens, de inhoud van een gegevensset te lezen, nieuwe gegevenssets maken en bijwerken van bestaande gegevenssets

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="prerequisites"></a>Vereisten
De Python-clientbibliotheek is getest onder de volgende omgevingen:

* Windows, Mac en Linux
* Python 2.7, 3.3 en 3.4

Dit is afhankelijk van de volgende pakketten:

* aanvragen
* Python-dateutil
* pandas

We raden aan met behulp van een Python-distributie, zoals [Anaconda](http://continuum.io/downloads#all) of [bladerdak](https://store.enthought.com/downloads/), die worden geleverd met Python, IPython en de hierboven genoemde drie-pakketten geïnstalleerd. Hoewel IPython niet strikt vereist is, is een geweldige omgeving voor het werken met en gegevens interactief te visualiseren.

### <a name="installation"></a>Het installeren van de Azure Machine Learning Python-clientbibliotheek
De Azure Machine Learning Python-clientbibliotheek moet ook worden geïnstalleerd voor het voltooien van de taken die worden beschreven in dit onderwerp. Het is beschikbaar via de [Python Package Index](https://pypi.python.org/pypi/azureml). Om het te installeren in uw Python-omgeving, voer de volgende opdracht uit uw lokale Python-omgeving:

    pip install azureml

U kunt ook u kunt downloaden en installeren van de bronnen op [GitHub](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).

    python setup.py install

Als u git op uw computer geïnstalleerd hebt, kunt u pip gebruiken om rechtstreeks vanuit de git-opslagplaats te installeren:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


## <a name="datasetAccess"></a>Codefragmenten Studio gebruiken voor toegang tot gegevenssets
De Python-clientbibliotheek biedt u programmatische toegang tot uw bestaande gegevenssets van experimenten die zijn uitgevoerd.

In de Studio webinterface, kunt u codefragmenten die bevatten alle benodigde informatie om te downloaden en te deserialiseren van gegevenssets als pandas DataFrame objecten op uw lokale computer genereren.

### <a name="security"></a>Beveiliging voor toegang tot gegevens
De codefragmenten die is geleverd door de Studio voor gebruik met de Python-clientbibliotheek uw werkruimte-id en -autorisatie bevat-token. Deze bieden volledige toegang tot uw werkruimte en moeten worden beveiligd, zoals een wachtwoord.

Uit veiligheidsoverwegingen, de codefragment-functionaliteit is alleen beschikbaar voor gebruikers die beschikken over hun rol instellen als **eigenaar** voor de werkruimte. Uw rol in Azure Machine Learning Studio wordt weergegeven op de **gebruikers** pagina onder **instellingen**.

![Beveiliging][security]

Als uw rol is niet ingesteld als **eigenaar**, kunt u een aanvraag voor het opnieuw worden uitgenodigd als een eigenaar, of vraag de eigenaar van de werkruimte om te voorzien van het codefragment.

Als u wilt het Autorisatietoken, kunt u het volgende doen:

* Vragen om een token van een eigenaar. Eigenaren kunnen toegang krijgen tot hun autorisatietokens vanaf de pagina instellingen van de werkruimte in Studio. Selecteer **instellingen** in het linkerdeelvenster en klik op **AUTORISATIETOKENS** om te zien van de primaire en secundaire tokens. Hoewel de primaire of de secundaire autorisatietokens kunnen worden gebruikt in het codefragment, verdient het aanbeveling dat de eigenaren van de secundaire autorisatietokens alleen delen.

![Autorisatietokens](./media/python-data-access/ml-python-access-settings-tokens.png)

* Vragen om te worden gepromoveerd tot de rol van eigenaar. U doet dit door een huidige eigenaar van de werkruimte, moet eerst worden verwijderd uit de werkruimte vervolgens opnieuw voor het uitnodigen als een eigenaar.

Als ontwikkelaars de werkruimte-id en -autorisatie hebt verkregen token, ze hebben toegang tot de werkruimte met behulp van het codefragment, ongeacht hun rol.

Autorisatietokens worden beheerd op de **AUTORISATIETOKENS** pagina onder **instellingen**. U kunt ze genereren, maar deze procedure trekt u toegang tot het vorige token.

### <a name="accessingDatasets"></a>Toegang tot gegevenssets vanuit een lokale Python-toepassing
1. In Machine Learning Studio, klikt u op **GEGEVENSSETS** in de navigatiebalk aan de linkerkant.
2. Selecteer de gegevensset die u wilt openen. U kunt een van de gegevenssets van selecteren de **mijn GEGEVENSSETS** lijst of vanuit de **voorbeelden** lijst.
3. Klik op de werkbalk onder **Code voor gegevenstoegang genereren**. Als de gegevens in een indeling die niet compatibel met de Python-clientbibliotheek is, wordt deze knop is uitgeschakeld.
   
    ![Gegevenssets][datasets]
4. Selecteer het codefragment in het venster dat wordt weergegeven en deze naar het Klembord te kopiëren.
   
    ![Data access code knop genereren][dataset-access-code]
5. Plak de code in het notitieblok van uw lokale Python-toepassing.
   
    ![Plak de code in de notebook][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Toegang tot de tussenliggende gegevenssets van Machine Learning-experimenten
Nadat een experiment in Machine Learning Studio wordt uitgevoerd, is het mogelijk voor toegang tot de tussenliggende gegevenssets van de uitvoerknooppunten van de modules. Tussenliggende gegevenssets zijn de gegevens die is gemaakt en gebruikt voor de tussenliggende stappen wanneer een model-hulpprogramma is uitgevoerd.

Tussenliggende gegevenssets kunnen worden geopend, zolang de gegevensindeling compatibel met de Python-clientbibliotheek is.

De volgende indelingen worden ondersteund (constanten voor deze zich in de `azureml.DataTypeIds` klasse):

* Als tekst zonder opmaak
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

U kunt de indeling bepalen door de muiswijzer op een knooppunt van de uitvoer module. Deze wordt weergegeven, samen met de naam van het knooppunt in de knopinfo.

Enkele van de modules, zoals de [splitsen] [ split] -module, de uitvoer naar een indeling met de naam `Dataset`, die niet wordt ondersteund door de Python-clientbibliotheek.

![Indeling van de gegevensset][dataset-format]

U moet gebruiken een conversie-module, zoals [converteren naar CSV][convert-to-csv], om op te halen van uitvoer naar een geldige indeling.

![GenericCSV indeling][csv-format]

De volgende stappen ziet een voorbeeld van een experiment maakt, wordt uitgevoerd en heeft toegang tot de tussenliggende gegevensset.

1. Een nieuw experiment maken.
2. Voeg een **volwassenen telling inkomsten binaire classificatie gegevensset** module.
3. Voeg een [splitsen] [ split] -module, en verbinding maken met de invoer voor de uitvoer van de module gegevensset.
4. Plaats een [converteren naar CSV] [ convert-to-csv] module en verbinding maken met de invoer voor een van de [splitsen] [ split] module levert.
5. Opslaan van het experiment, uitvoeren en wachten op worden uitgevoerd.
6. Klik op het knooppunt uitvoer in de [converteren naar CSV] [ convert-to-csv] module.
7. Wanneer het contextmenu wordt weergegeven, selecteert u **Code voor gegevenstoegang genereren**.
   
    ![Contextmenu][experiment]
8. Selecteer het codefragment en naar het Klembord kopiëren vanuit het venster dat wordt weergegeven.
   
    ![Code voor gegevenstoegang genereren in contextmenu][intermediate-dataset-access-code]
9. Plak de code in uw laptop.
   
    ![Plak de code in de notebook][ipython-intermediate-dataset]
10. U kunt de gegevens visualiseren met matplotlib. Dit wordt weergegeven in een histogram van de kolom Leeftijd:
    
    ![Histogram][ipython-histogram]

## <a name="clientApis"></a>De Machine Learning Python-clientbibliotheek gebruiken om te openen, lezen, maken en gegevenssets beheren
### <a name="workspace"></a>Werkruimte
De werkruimte is het toegangspunt voor de Python-clientbibliotheek. Geef de `Workspace` klasse met uw werkruimte-id en -autorisatie token om een exemplaar te maken:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Sommen, gegevenssets
Het inventariseren van alle gegevenssets in een opgegeven werkruimte:

    for ds in ws.datasets:
        print(ds.name)

Alleen de gebruiker gemaakte gegevenssets inventariseren:

    for ds in ws.user_datasets:
        print(ds.name)

Het opsommen van alleen de voorbeeld-gegevenssets:

    for ds in ws.example_datasets:
        print(ds.name)

U kunt toegang tot een gegevensset met de naam (dit is hoofdlettergevoelig):

    ds = ws.datasets['my dataset name']

Of u het kunt openen met de index:

    ds = ws.datasets[0]


### <a name="metadata"></a>Metagegevens
Gegevenssets hebben metagegevens, naast de inhoud. (Tussenliggende gegevenssets vormen een uitzondering op deze regel en hoeft niet alle metagegevens.)

Sommige metagegevenswaarden van de zijn toegewezen door de gebruiker tijdens het maken:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Anderen zijn waarden die door Azure ML is toegewezen:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Zie de `SourceDataset` klasse voor meer informatie over de metagegevens van de beschikbaar.

### <a name="read-contents"></a>Inhoud lezen
De codefragmenten die is geleverd door Machine Learning Studio automatisch downloaden en te deserialiseren van de gegevensset naar een pandas DataFrame-object. Dit wordt gedaan met de `to_dataframe` methode:

    frame = ds.to_dataframe()

Als u liever de onbewerkte gegevens te downloaden en uitvoeren van de deserialisatie zelf, kan die worden gebruikt. Op dit moment is dit de enige optie voor indelingen zoals 'ARFF', de Python-clientbibliotheek kan niet worden gedeserialiseerd.

De inhoud lezen als tekst:

    text_data = ds.read_as_text()

De inhoud lezen als binair bestand:

    binary_data = ds.read_as_binary()

U kunt ook gewoon een stroom om de inhoud te openen:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Maak een nieuwe gegevensset
De Python-clientbibliotheek kunt u het uploaden van gegevenssets van uw Python-programma. Deze gegevenssets zijn vervolgens beschikbaar voor gebruik in uw werkruimte.

Als u uw gegevens in een pandas DataFrame hebt, gebruikt u de volgende code:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Als uw gegevens al is geserialiseerd, kunt u het volgende gebruiken:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

De Python-clientbibliotheek kan het serialiseren van een pandas DataFrame naar de volgende indelingen (constanten voor deze zich in de `azureml.DataTypeIds` klasse):

* Als tekst zonder opmaak
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

### <a name="update-an-existing-dataset"></a>Bijwerken van een bestaande gegevensset
Als u probeert te uploaden van een nieuwe gegevensset met een naam die overeenkomt met een bestaande gegevensset, moet u een conflict opgetreden.

Voor het bijwerken van een bestaande gegevensset, moet u eerst een verwijzing naar de bestaande gegevensset opgehaald:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Gebruik vervolgens `update_from_dataframe` voor het serialiseren en vervang de inhoud van de gegevensset op Azure:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Als u de gegevens naar een andere indeling wilt, Geef een waarde op voor de optionele `data_type_id` parameter.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

U kunt eventueel een nieuwe beschrijving instellen door een waarde opgeeft voor de `description` parameter.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

U kunt eventueel een nieuwe naam instellen door een waarde opgeeft voor de `name` parameter. Van nu af, zult u de gegevensset met behulp van de nieuwe naam alleen ophalen. De volgende code werkt de gegevens, naam en beschrijving.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

De `data_type_id`, `name` en `description` parameters zijn optioneel en wordt standaard ingesteld op hun vorige waarde. De `dataframe` parameter is altijd vereist.

Als uw gegevens al is geserialiseerd, gebruikt u `update_from_raw_data` in plaats van `update_from_dataframe`. Als u alleen doorgeeft `raw_data` in plaats van `dataframe`, het op een soortgelijke manier werkt.

<!-- Images -->
[security]:./media/python-data-access/security.png
[dataset-format]:./media/python-data-access/dataset-format.png
[csv-format]:./media/python-data-access/csv-format.png
[datasets]:./media/python-data-access/datasets.png
[dataset-access-code]:./media/python-data-access/dataset-access-code.png
[ipython-dataset]:./media/python-data-access/ipython-dataset.png
[experiment]:./media/python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

