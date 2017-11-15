---
title: Toegang tot gegevenssets met Machine Learning Python-clientbibliotheek | Microsoft Docs
description: Installeren en de Python-clientbibliotheek gebruiken voor toegang tot en beheer van Azure Machine Learning gegevens veilig vanuit een lokale Python-omgeving.
services: machine-learning
documentationcenter: python
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 9ab42272-c30c-4b7e-8e66-d64eafef22d0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: huvalo;bradsev
ms.openlocfilehash: 3cffb90baadef570bbb7e4e2d96741cc0420b0a2
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a>Toegang tot gegevenssets met Python met behulp van de clientbibliotheek van Azure Machine Learning Python
De evaluatieversie van Microsoft Azure Machine Learning Python-clientbibliotheek kunt inschakelen van beveiligde toegang naar uw Azure Machine Learning-gegevenssets vanuit een lokale Python-omgeving en kunt het maken en beheren van gegevenssets in een werkruimte.

In dit onderwerp bevat instructies over het:

* installeren van de clientbibliotheek van Machine Learning Python 
* toegang tot en het uploaden van gegevenssets, zoals instructies over het verkrijgen van de autorisatie voor toegang tot Azure Machine Learning gegevenssets uit uw lokale omgeving voor Python
* toegang tot tussenliggende gegevenssets van experimenten
* gebruik van de clientbibliotheek Python opsommen gegevenssets, toegang tot metagegevens, de inhoud van een gegevensset lezen, nieuwe gegevenssets maken en bestaande gegevenssets bijwerken

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="prerequisites"></a>Vereisten
De Python-clientbibliotheek is getest onder de volgende omgevingen:

* Windows, Mac en Linux
* Python 2.7 3.3 en 3.4

Deze is afhankelijk van de volgende pakketten:

* aanvragen
* Python-dateutil
* pandas

Wordt u aangeraden een Python-distributie, zoals [Anaconda](http://continuum.io/downloads#all) of [bladerdak](https://store.enthought.com/downloads/), die worden geleverd met Python, IPython en de drie pakketten die hierboven worden geïnstalleerd. Hoewel IPython niet strikt vereist is, is een geweldige omgeving voor het werken en gegevens interactief te visualiseren.

### <a name="installation"></a>Het installeren van de clientbibliotheek van Azure Machine Learning Python
De clientbibliotheek van Azure Machine Learning Python moet ook worden geïnstalleerd voor het voltooien van de taken die worden beschreven in dit onderwerp. Deze beschikbaar is via de [Python Package Index](https://pypi.python.org/pypi/azureml). Om het te installeren in uw omgeving Python, voer de volgende opdracht uit uw lokale Python-omgeving:

    pip install azureml

U kunt ook downloaden en installeren vanuit de bronnen op [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).

    python setup.py install

Als u git op deze computer geïnstalleerd hebt, kunt u pip rechtstreeks te installeren vanaf de git-opslagplaats:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


## <a name="datasetAccess"></a>Studio-codefragmenten gebruiken voor toegang tot gegevenssets
De Python-clientbibliotheek hebt u programmatische toegang tot uw bestaande gegevenssets van experimenten die zijn uitgevoerd.

Vanuit de webinterface Studio kunt u codefragmenten die bevatten de benodigde informatie om te downloaden en gegevenssets te deserialiseren als Pandas DataFrame objecten op uw machine locatie genereren.

### <a name="security"></a>Beveiliging voor toegang tot gegevens
De codefragmenten die is opgegeven door Studio voor gebruik met de Python-clientbibliotheek uw werkruimte-id en autorisatie bevat-token. Deze volledige toegang tot uw werkruimte en moeten worden beveiligd, zoals een wachtwoord.

Om veiligheidsredenen de code codefragment-functionaliteit is alleen beschikbaar voor gebruikers die hun rol die is ingesteld als **eigenaar** voor de werkruimte. Uw rol in Azure Machine Learning Studio wordt weergegeven op de **gebruikers** pagina onder **instellingen**.

![Beveiliging][security]

Als uw rol niet is ingesteld als **eigenaar**, kunt u ofwel een verzoek voor opnieuw worden uitgenodigd als een eigenaar of vraag de eigenaar van de werkruimte om te voorzien van het codefragment.

Als u het verificatietoken, kunt u een van de volgende doen:

* Vragen om een token van een eigenaar. Eigenaars kunnen toegang krijgen tot hun tokens autorisatie vanaf de pagina instellingen van de werkruimte in Studio. Selecteer **instellingen** in het linkerdeelvenster en klik op **AUTORISATIE TOKENS** om te zien van de primaire en secundaire tokens.  Hoewel de primaire of secundaire autorisatie tokens kunnen in het codefragment worden gebruikt, wordt u aangeraden eigenaars delen alleen de secundaire autorisatie-tokens.

![Autorisatie-tokens](./media/python-data-access/ml-python-access-settings-tokens.png)

* Vraag om de rol van eigenaar worden gepromoveerd.  U doet dit door moet een huidige eigenaar van de werkruimte eerst worden verwijderd uit de werkruimte vervolgens opnieuw uit te nodigen u aan als eigenaar.

Als ontwikkelaars de werkruimte-id en -autorisatie hebt verkregen token, ze hebben toegang tot de werkruimte met behulp van het codefragment ongeacht hun rol.

Autorisatie-tokens worden beheerd op de **AUTORISATIE TOKENS** pagina onder **instellingen**. U kunt ze genereren, maar deze procedure trekt u toegang tot het vorige token.

### <a name="accessingDatasets"></a>Gegevenssets toegang vanuit een lokale Python-toepassing
1. Klik in Machine Learning Studio **GEGEVENSSETS** in de navigatiebalk aan de linkerkant.
2. Selecteer de gegevensset die u wilt openen. U kunt kiezen uit de gegevenssets van de **mijn GEGEVENSSETS** lijst of vanuit de **voorbeelden** lijst.
3. Klik in de onderste werkbalk op **toegangscode gegevens genereren**. Als de gegevens in een indeling die incompatibel is met de Python-clientbibliotheek is, wordt deze knop is uitgeschakeld.
   
    ![Gegevenssets][datasets]
4. Selecteer het codefragment in het venster dat wordt weergegeven en naar het Klembord kopiëren.
   
    ![Toegangscode][dataset-access-code]
5. Plak de code in de notebook van uw lokale Python-toepassing.
   
    ![Notebook][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Tussenliggende gegevenssets van de toegang van Machine Learning-experimenten
Nadat u een experiment wordt uitgevoerd in de Machine Learning Studio, is het mogelijk voor toegang tot de tussenliggende gegevenssets van de uitvoerknooppunten van modules. Tussenliggende gegevenssets zijn de gegevens die zijn gemaakt en gebruikt voor de tussenliggende stappen wanneer een model-hulpprogramma is uitgevoerd.

Tussenliggende gegevenssets toegankelijk zijn, zolang de gegevensindeling compatibel met de clientbibliotheek van Python is.

De volgende indelingen worden ondersteund (constanten voor deze zich in de `azureml.DataTypeIds` klasse):

* Tekst zonder opmaak
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

U kunt de indeling bepalen door de muiswijzer op een knooppunt van de uitvoer module. Deze wordt samen met de knooppuntnaam in de knopinfo weergegeven.

Sommige van de modules, zoals de [gesplitste] [ split] module uitvoer naar een indeling met de naam `Dataset`, die niet wordt ondersteund door de clientbibliotheek van Python.

![Indeling van de gegevensset][dataset-format]

U wilt gebruiken, zoals een module conversie [converteren naar CSV][convert-to-csv], om uitvoer naar een ondersteunde indeling.

![GenericCSV-indeling][csv-format]

De volgende stappen ziet een voorbeeld van een experiment maakt, worden uitgevoerd en heeft toegang tot de tussenliggende gegevensset.

1. Een nieuw experiment maken.
2. Voeg een **volwassenen inventarisering Income binaire classificatie gegevensset** module.
3. Plaats een [gesplitste] [ split] module en verbinding maken met de invoer voor de uitvoer van de module gegevensset.
4. Plaats een [converteren naar CSV] [ convert-to-csv] module en verbinding maken met de invoer op een van de [gesplitste] [ split] module levert.
5. Sla het experiment, uitvoeren en wacht totdat deze is uitgevoerd.
6. Klik op het uitvoer-knooppunt in de [converteren naar CSV] [ convert-to-csv] module.
7. Wanneer het contextmenu wordt weergegeven, selecteert u **toegangscode gegevens genereren**.
   
    ![Contextmenu][experiment]
8. Selecteer het codefragment en naar het Klembord kopiëren vanuit het venster dat wordt weergegeven.
   
    ![Toegangscode][intermediate-dataset-access-code]
9. Plak de code in uw laptop.
   
    ![Notebook][ipython-intermediate-dataset]
10. U kunt de gegevens visualiseren met matplotlib. Hiermee wordt in een histogram voor de kolom Leeftijd:
    
    ![Histogram][ipython-histogram]

## <a name="clientApis"></a>De Machine Learning Python-clientbibliotheek gebruiken om te openen, lezen, maken en beheren van gegevenssets
### <a name="workspace"></a>Werkruimte
De werkruimte is het toegangspunt voor de clientbibliotheek van Python. Geef de `Workspace` klasse met uw werkruimte-id en -autorisatie token een exemplaar te maken:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Gegevenssets opsommen
Het inventariseren van alle gegevenssets in een bepaalde werkruimte:

    for ds in ws.datasets:
        print(ds.name)

Alleen de gebruiker gemaakte gegevenssets inventariseren:

    for ds in ws.user_datasets:
        print(ds.name)

Alleen de voorbeeld-gegevenssets inventariseren:

    for ds in ws.example_datasets:
        print(ds.name)

U kunt toegang krijgen tot een gegevensset met de naam (dit is hoofdlettergevoelig):

    ds = ws.datasets['my dataset name']

Of u toegang hebt tot het met de index:

    ds = ws.datasets[0]


### <a name="metadata"></a>Metagegevens
Gegevenssets hebben metagegevens, naast inhoud. (Tussenliggende gegevenssets vormen een uitzondering op deze regel en hoeft niet alle metagegevens.)

Sommige metagegevenswaarden worden toegewezen door de gebruiker tijdens het maken:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Sommige zijn toegewezen door Azure ML waarden:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Zie de `SourceDataset` klasse voor meer informatie over de metagegevens beschikbaar.

### <a name="read-contents"></a>Inhoud lezen
De codefragmenten die is geleverd door Machine Learning Studio automatisch downloaden en te deserialiseren van de gegevensset naar een object Pandas DataFrame. Dit wordt gedaan met de `to_dataframe` methode:

    frame = ds.to_dataframe()

Als u liever de onbewerkte gegevens downloaden en uitvoeren van de deserialisatie, is dat een optie. Op dit moment is dit de enige optie voor indelingen zoals 'ARFF', de clientbibliotheek Python kan niet worden gedeserialiseerd.

De inhoud lezen als tekst:

    text_data = ds.read_as_text()

De inhoud lezen als binair:

    binary_data = ds.read_as_binary()

U kunt ook gewoon een stroom om de inhoud te openen:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Maak een nieuwe gegevensset
De Python-clientbibliotheek kunt u voor het uploaden van gegevenssets van het programma Python. Deze gegevenssets zijn vervolgens beschikbaar voor gebruik in uw werkruimte.

Als u uw gegevens in een DataFrame Pandas hebt, gebruikt u de volgende code:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Als u al uw gegevens geserialiseerd, kunt u het volgende gebruiken:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Kan de Python-clientbibliotheek voor het serialiseren van een DataFrame Pandas naar de volgende indelingen (constanten voor deze zich in de `azureml.DataTypeIds` klasse):

* Tekst zonder opmaak
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

### <a name="update-an-existing-dataset"></a>Bijwerken van een bestaande gegevensset
Als u probeert te uploaden van een nieuwe gegevensset met een naam die overeenkomt met een bestaande gegevensset, krijgt u een conflict opgetreden.

Voor het bijwerken van een bestaande gegevensset, moet u eerst een verwijzing naar de bestaande gegevensset ophalen:

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

Als u wilt voor het serialiseren van de gegevens naar een andere indeling, Geef een waarde op voor de optionele `data_type_id` parameter.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

U kunt desgewenst een nieuwe beschrijving instellen door een waarde opgeeft voor de `description` parameter.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

U kunt desgewenst een nieuwe naam instellen door een waarde opgeeft voor de `name` parameter. U zult van nu aan de gegevensset met alleen de nieuwe naam ophalen. De volgende code werkt de gegevens, naam en beschrijving.

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

De `data_type_id`, `name` en `description` parameters zijn optioneel en wordt standaard ingesteld op hun vorige waarden. De `dataframe` parameter is altijd vereist.

Als u al uw gegevens geserialiseerd, gebruikt u `update_from_raw_data` in plaats van `update_from_dataframe`. Als u alleen doorgeeft in `raw_data` in plaats van `dataframe`, het op een vergelijkbare manier werkt.

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

