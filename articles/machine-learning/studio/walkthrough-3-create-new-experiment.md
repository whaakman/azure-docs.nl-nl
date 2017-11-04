---
title: 'Stap 3: Maak een nieuwe Machine Learning-experiment | Microsoft Docs'
description: 'Stap 3 van de prognose een voorspellende oplossing overzicht: een nieuw trainingsexperiment maken in Azure Machine Learning Studio.'
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 660e3c27-55ef-4c33-a4e9-dff4d1224630
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: a8f1764204740a8f5ef757e5e2ad63cfd43af150
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>Kennismaken, stap 3: Een nieuw Azure Machine Learning-experiment maken
Dit is de derde stap van de procedure [predictive analytics-oplossing in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md)

1. [Een Machine Learning-werkruimte maken](walkthrough-1-create-ml-workspace.md)
2. [Bestaande gegevens uploaden](walkthrough-2-upload-data.md)
3. **Een nieuw experiment maken**
4. [De modellen trainen en evalueren](walkthrough-4-train-and-evaluate-models.md)
5. [De webservice implementeren](walkthrough-5-publish-web-service.md)
6. [Toegang tot de webservice](walkthrough-6-access-web-service.md)

- - -
De volgende stap in dit scenario is het maken van een experiment in Machine Learning Studio die gebruikmaakt van de gegevensset die wordt geüpload.  

1. Klik in Studio **+ nieuw** aan de onderkant van het venster.
2. Selecteer **EXPERIMENT**, en selecteer vervolgens 'Leeg Experiment'. 

    ![Een nieuw experiment maken][0]

2. Selecteer de naam van de standaard-experiment aan de bovenkant van het canvas en herkenbare naam.

    ![Wijzig de naam van experiment][5]
   
   > [!TIP]
   > Het is raadzaam om in te vullen **samenvatting** en **beschrijving** voor het experiment in de **eigenschappen** deelvenster. Deze eigenschappen kunnen u de kans op het experiment documenteren, zodat iedereen die deze later wordt uw doelen en methodologie begrijpen.
   > 
   > ![Experiment eigenschappen][6]
   > 
3. Vouw in het modulepalet links van het experimentcanvas **gegevenssets opgeslagen**.
4. Zoek de gegevensset die u hebt gemaakt onder **mijn gegevenssets** en sleep deze naar het canvas. U kunt de gegevensset ook vinden door de naam in te voeren de **Search** vak boven het palet.  

    ![De gegevensset naar het experimentcanvas toevoegen][7]

## <a name="prepare-the-data"></a>De gegevens voorbereiden
U kunt de eerste 100 rijen van de gegevens en enkele statische gegevens voor de volledige gegevensset bekijken: klik op de uitvoerpoort van de gegevensset (kleine cirkel onderaan) en selecteer **Visualize**.  

Omdat het bestand niet is meegeleverd met kolomkoppen, Studio algemene koppen is opgegeven (Col1, Col2, *enzovoort*). Goede koppen niet essentieel is voor het maken van een model, maar ze gemakkelijker te werken met de gegevens in het experiment. Ook als we uiteindelijk dit model in een webservice publiceren, de koppen helpen bij het identificeren van de kolommen voor de gebruiker van de service.  

We kunt toevoegen met behulp van kolomkoppen de [metagegevens bewerken] [ edit-metadata] module.
U gebruikt de [metagegevens bewerken] [ edit-metadata] module die u wilt wijzigen van de metagegevens gekoppeld aan een dataset. In dit geval gebruiken we het meer beschrijvende namen voor kolomkoppen op te geven. 

Gebruik [metagegevens bewerken][edit-metadata], geeft u eerst welke kolommen te wijzigen (in dit geval ze allemaal.) Geef vervolgens de actie die moet worden uitgevoerd op deze kolommen (in dit geval kolomkoppen te wijzigen.)

1. Typ in het modulepalet 'metagegevens' in de **Search** vak. De [metagegevens bewerken] [ edit-metadata] wordt weergegeven in de modulelijst.

2. Klik en sleep de [metagegevens bewerken] [ edit-metadata] module naar het canvas en zet deze neer onder de gegevensset die we eerder hebt toegevoegd.

3. Verbinding maken met de gegevensset naar de [metagegevens bewerken][edit-metadata]: klik op de uitvoerpoort van de gegevensset (kleine cirkel aan de onderkant van de gegevensset), sleept u naar de invoer-poort van [metagegevens bewerken] [ edit-metadata] (de kleine cirkel aan de bovenkant van de module), klikt u vervolgens de muisknop loslaat. De module en gegevensset blijven verbonden, zelfs als u ofwel op het canvas verplaatsen.
   
   Het experiment moet nu als volgt uitzien:  
   
   ![Metagegevens bewerken][1]
   
   Rood uitroepteken geeft aan dat we de eigenschappen voor deze module nog niet hebt ingesteld. We doen om het dialoogvenster.
   
   > [!TIP]
   > U kunt een opmerking aan een module toevoegen door te dubbelklikken op de module en tekst in te voeren. Zodoende kunt u in één oogopslag zien wat de module in uw experiment doet. In dit geval, dubbelklikt u op de [metagegevens bewerken] [ edit-metadata] module en typ de opmerking 'Add kolomkoppen'. Klik ergens anders op het canvas te sluiten van het tekstvak. De opmerking, klikt u op de pijl-omlaag in de module.
   > 
   > ![Metagegevens module bewerken met opmerkingen toegevoegd][8]
   > 
4. Selecteer [metagegevens bewerken][edit-metadata], en in de **eigenschappen** deelvenster rechts van het canvas te klikken op **Launch column selector**.

5. In de **kolommen selecteren** dialoogvenster, selecteer de rijen in **beschikbare kolommen** en klik op > te verplaatsen naar **geselecteerde kolommen**.
   Het dialoogvenster ziet er als volgt:

   ![Kolomkiezer met alle kolommen geselecteerd][2]

6. Klik op de **OK** selectievakje is ingeschakeld.

7. Terug in de **eigenschappen** deelvenster, zoek de **nieuwe kolomnamen** parameter. Geef een lijst met namen van de 21 kolommen in de gegevensset, gescheiden door komma's en in de volgorde van kolommen in dit veld. U kunt de namen van kolommen uit de dataset-documentatie op de website UCI verkrijgen of voor uw gemak bedoeld; u kunt kopiëren en plakken van de volgende lijst:  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   Het deelvenster Eigenschappen ziet er als volgt:
   
   ![Eigenschappen voor metagegevens bewerken][3]

> [!TIP]
> Als u controleren of de kolomkoppen wilt, voer het experiment (Klik op **uitvoeren** onder het experimentcanvas). Wanneer deze is voltooid (Er verschijnt een groen vinkje op [metagegevens bewerken][edit-metadata]), klikt u op de uitvoerpoort van de [metagegevens bewerken] [ edit-metadata] module en selecteer **Visualize**. U kunt de uitvoer van elke module weergeven op dezelfde manier om de voortgang van de gegevens via het experiment weer te geven.
> 
> 

## <a name="create-training-and-test-datasets"></a>Training maken en testen van gegevenssets
We moeten sommige gegevens aan het model trainen en sommige te testen.
Dus in de volgende stap van het experiment we de gegevensset in twee afzonderlijke gegevenssets splitsen: één voor het trainen van het model en één voor het testen van het.

Om dit te doen, gebruiken we de [Split Data] [ split] module.  

1. Zoek de [Split Data] [ split] module, sleep deze naar het canvas en verbindt deze met de [metagegevens bewerken] [ edit-metadata] module.

2. Standaard is de verhouding tussen het gesplitste 0,5 en de **Randomized gesplitste** parameter is ingesteld. Dit betekent dat een willekeurige helft van de gegevens uitgevoerd via één poort van wordt de [Split Data] [ split] -module en de helft via de andere. U kunt deze parameters aanpassen, evenals de **willekeurige seed** parameter wijzigen van de scheiding tussen trainings- en testdoeleinden gegevens. In dit voorbeeld laat we ze-is.
   
   > [!TIP]
   > De eigenschap **fractie van rijen in de eerste uitvoergegevensset** bepaalt hoeveel van de gegevens is de uitvoer via de *links* uitvoerpoort. Bijvoorbeeld als u de verhouding tussen 0,7 instelt, is 70% van de gegevens uitvoer via de poort van de linker- en 30% tot en met de juiste poort.  
   > 
   > 

3. Dubbelklik op de [Split Data] [ split] module en de opmerking invoeren, ' Training/testen gegevens splitsen 50% '. 

Kunnen we gebruiken de uitvoer van de [Split Data] [ split] module echter we graag, maar we Kies moet de uitvoer van de linker trainingsgegevens en het recht uitvoergegevens als testen.  

Zoals vermeld in de [vorige stap](walkthrough-2-upload-data.md), de kosten van een hoge kredietrisico als laag onjuiste vijf keer hoger dan de kosten van het onjuiste een lage kredietrisico zo hoog is. Als u wilt voor dit account, moet er een nieuwe gegevensset die overeenkomt met deze functie kosten gegenereerd. In de nieuwe gegevensset, elk met een hoog risico voorbeeld vijf keer gerepliceerd terwijl elke laag risico voorbeeld niet is gerepliceerd.   

We kunnen deze replicatie doen met behulp van R-code:  

1. Zoek en sleep de [R-Script uitvoeren] [ execute-r-script] module naar het experimentcanvas. 

2. Verbinding maken met de uitvoerpoort links van de [Split Data] [ split] module aan de eerste invoer poort ('Dataset1') van de [R-Script uitvoeren] [ execute-r-script] module.

3. Dubbelklik op de [R-Script uitvoeren] [ execute-r-script] module en voer de opmerking 'Instellen kosten aanpassing'.

4. In de **eigenschappen** deelvenster, verwijdert u de standaardtekst in de **R-Script** parameter en voer dit script:
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![R-script in de module R-Script uitvoeren][9]

We moeten doen deze dezelfde replicatiebewerking voor elke uitvoer van de [Split Data] [ split] module zodat de training en testen gegevens de dezelfde aanpassing van de kosten hebben. De eenvoudigste manier om dit te doen is door het dupliceren van de [R-Script uitvoeren] [ execute-r-script] module die we zojuist hebt gemaakt en verbinding maken met de andere uitvoerpoort van de [Split Data] [ split] module.

1. Met de rechtermuisknop op de [R-Script uitvoeren] [ execute-r-script] module en selecteer **kopie**.

2. Met de rechtermuisknop op het experimentcanvas en selecteer **plakken**.

3. De nieuwe module naar positie slepen en sluit vervolgens de juiste uitvoerpoort van de [Split Data] [ split] module die u wilt de eerste invoerpoort van deze nieuwe [R-Script uitvoeren] [ execute-r-script] module. 

4. Klik aan de onderkant van het canvas op **uitvoeren**. 

> [!TIP]
> De kopie van de module R-Script uitvoeren bevat hetzelfde script als de oorspronkelijke module. Wanneer u kopieert en plakt een module op het canvas, behoudt de kopie de eigenschappen van het origineel.  
> 
> 

Onze experiment ziet nu er ongeveer als volgt:

![Split-module en R scripts toe te voegen][4]

Zie voor meer informatie over het gebruik van R-scripts in uw experimenten [uw experiment uitbreiden met R](extend-your-experiment-with-r.md).

**Volgende stap: [trainen en evalueren van de modellen](walkthrough-4-train-and-evaluate-models.md)**

[0]: ./media/walkthrough-3-create-new-experiment/create-new-experiment.png
[5]: ./media/walkthrough-3-create-new-experiment/rename-experiment.png
[6]: ./media/walkthrough-3-create-new-experiment/experiment-properties.png
[7]: ./media/walkthrough-3-create-new-experiment/add-dataset-to-experiment.png
[8]: ./media/walkthrough-3-create-new-experiment/edit-metadata-with-comment.png
[9]: ./media/walkthrough-3-create-new-experiment/execute-r-script.png
[1]: ./media/walkthrough-3-create-new-experiment/experiment-with-edit-metadata-module.png
[2]: ./media/walkthrough-3-create-new-experiment/select-columns.png
[3]: ./media/walkthrough-3-create-new-experiment/edit-metadata-properties.png
[4]: ./media/walkthrough-3-create-new-experiment/experiment.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
