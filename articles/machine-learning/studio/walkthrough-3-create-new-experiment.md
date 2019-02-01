---
title: 'Stap 3: Een nieuw Machine Learning Studio-experiment maken'
titleSuffix: Azure Machine Learning Studio
description: 'Stap 3 van het ontwikkelen van een overzicht van de voorspellende oplossing: Een nieuw trainingsexperiment maken in Azure Machine Learning Studio.'
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: garyericson
ms.author: garye
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/23/2017
ms.openlocfilehash: aee0370b4d1ea1bffe8e6723c14e6248ebbd808a
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55506393"
---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-studio-experiment"></a>Kennismaken, stap 3: Een nieuw Azure Machine Learning Studio-experiment maken
Dit is de derde stap van de procedure [predictive analytics-oplossing in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md)

1. [Een Machine Learning-werkruimte maken](walkthrough-1-create-ml-workspace.md)
2. [Bestaande gegevens uploaden](walkthrough-2-upload-data.md)
3. **Een nieuw experiment maken**
4. [De modellen trainen en evalueren](walkthrough-4-train-and-evaluate-models.md)
5. [De webservice implementeren](walkthrough-5-publish-web-service.md)
6. [De webservice openen](walkthrough-6-access-web-service.md)

- - -
De volgende stap in dit scenario is het maken van een experiment in Machine Learning Studio die gebruikmaakt van de gegevensset die we hebben geüpload.  

1. Klik in de Studio **+ nieuw** aan de onderkant van het venster.
2. Selecteer **EXPERIMENT**, en selecteer vervolgens 'Leeg Experiment'. 

    ![Een nieuw experiment maken][0]

2. Selecteer de naam van het experiment standaard aan de bovenkant van het canvas en wijzig de naam in een herkenbare naam.

    ![Wijzig de naam van experiment][5]
   
   > [!TIP]
   > Het is raadzaam om in te vullen **samenvatting** en **beschrijving** van het experiment in de **eigenschappen** deelvenster. Deze eigenschappen kunnen u de kans om te documenteren van het experiment zodat iedereen die kijkt naar deze later wordt meer informatie over uw doelstellingen methodologie.
   > 
   > ![Eigenschappen van experiment][6]
   > 
3. Vouw in het modulepalet links van het experimentcanvas, **opgeslagen gegevenssets**.
4. Zoek de gegevensset die u hebt gemaakt onder **mijn gegevenssets** en sleep deze naar het canvas. U kunt de gegevensset ook vinden door de naam in te voeren de **zoeken** vak boven het kleurenpalet.  

    ![De gegevensset toevoegen aan het experiment][7]

## <a name="prepare-the-data"></a>De gegevens voorbereiden
U kunt de eerste 100 rijen van de gegevens en enkele statische gegevens voor de hele gegevensset weergeven: Klik op de uitvoerpoort van de gegevensset (de kleine cirkel aan de onderkant) en selecteer **Visualize**.  

Omdat het gegevensbestand is niet geleverd met kolomkoppen, Studio algemene koppen is opgegeven (Col1, Col2, *enzovoort*). Goede koppen niet essentieel is voor het maken van een model, maar ze maken het gemakkelijker om te werken met de gegevens in het experiment. Ook, wanneer we dit model uiteindelijk in een web-service publiceert, de kolomkoppen te identificeren de kolommen voor de gebruiker van de service.  

We kunnen met behulp van kolomkoppen toevoegen de [metagegevens bewerken] [ edit-metadata] module.
U gebruikt de [metagegevens bewerken] [ edit-metadata] module metagegevens die zijn gekoppeld aan een gegevensset wijzigen. In dit geval gebruiken we het meer beschrijvende namen voor de kolomkoppen op te geven. 

Gebruik [metagegevens bewerken][edit-metadata], u eerst welke kolommen u wilt wijzigen (in dit geval ze allemaal.) Geef vervolgens de actie die moet worden uitgevoerd op deze kolommen (in dit geval kolomkoppen wijzigen.)

1. Typ in het modulepalet 'metagegevens' in de **zoeken** vak. De [metagegevens bewerken] [ edit-metadata] wordt weergegeven in de modulelijst.

2. Klik en sleep de [metagegevens bewerken] [ edit-metadata] module naar het canvas en zet het neer hieronder de gegevensset die we eerder hebt toegevoegd.

3. Verbinding maken met de gegevensset in de [metagegevens bewerken][edit-metadata]: klik op de uitvoerpoort van de gegevensset (de kleine cirkel aan de onderkant van de gegevensset), sleept u aan de invoerpoort van [metagegevens bewerken] [ edit-metadata] (de kleine cirkel aan de bovenkant van de module), laat de muisknop los. De gegevensset en de module blijven verbonden, zelfs als u een op het canvas verplaatsen.
   
   Het experiment ziet er ongeveer als volgt:  
   
   ![Metagegevens bewerken][1]
   
   Het rode uitroepteken geeft aan dat we de eigenschappen voor deze module nog niet hebt ingesteld. We doen dat gaan.
   
   > [!TIP]
   > U kunt een opmerking aan een module toevoegen door te dubbelklikken op de module en tekst in te voeren. Zodoende kunt u in één oogopslag zien wat de module in uw experiment doet. In dit geval, dubbelklikt u op de [metagegevens bewerken] [ edit-metadata] -module en typ de opmerking 'Add kolomkoppen'. Klik ergens anders zijn op het canvas om te sluiten in het tekstvak. Als de opmerking weergeven, klikt u op de pijl-omlaag in de module.
   > 
   > ![De metagegevens van module bewerken met opmerking wordt toegevoegd][8]
   > 
4. Selecteer [metagegevens bewerken][edit-metadata], en klik in de **eigenschappen** deelvenster aan de rechterkant van het canvas, klikt u op **kolomkiezer lancering**.

5. In de **kolommen selecteren** dialoogvenster, selecteer de rijen in **beschikbare kolommen** en klikt u op > te verplaatsen naar **geselecteerde kolommen**.
   Het dialoogvenster ziet er als volgt:

   ![Kolomkiezer met alle geselecteerde kolommen][2]

6. Klik op de **OK** selectievakje is ingeschakeld.

7. Klik in de **eigenschappen** deelvenster, zoek de **nieuwe kolomnamen** parameter. Voer een lijst met namen van de 21 kolommen in de gegevensset, gescheiden door komma's en in de volgorde van kolommen in dit veld. U kunt de namen van kolommen in de gegevensset-documentatie op de website van UCI verkrijgen of voor het gemak kunt u kopiëren en plakken van de volgende lijst:  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   Het deelvenster met eigenschappen ziet er als volgt:
   
   ![Eigenschappen voor metagegevens bewerken][3]

> [!TIP]
> Als u controleren of de kolomkoppen wilt, voer het experiment uit (Klik op **uitvoeren** onder het experimentcanvas). Wanneer deze is voltooid, wordt uitgevoerd (een groen vinkje wordt weergegeven op [metagegevens bewerken][edit-metadata]), klikt u op de uitvoerpoort van de [metagegevens bewerken] [ edit-metadata] module en selecteer **Visualize**. U kunt de uitvoer van elke module weergeven op dezelfde manier om de voortgang van de gegevens via het experiment weer te geven.
> 
> 

## <a name="create-training-and-test-datasets"></a>Training maken en testen van gegevenssets
Moeten we enkele gegevens aan het model te trainen en sommige om dit te testen.
Dus in de volgende stap van het experiment we de gegevensset in twee afzonderlijke gegevenssets splitsen: één voor het trainen van het model en één voor het testen.

Om dit te doen, gebruiken we de [Split Data] [ split] module.  

1. Zoek de [Split Data] [ split] -module, sleep deze naar het canvas en verbindt deze met de [metagegevens bewerken] [ edit-metadata] module.

2. Standaard is de splitsingsverhouding 0,5 en de **Randomized splitsen** parameter is ingesteld. Dit betekent dat er een willekeurige helft van de gegevens uitgevoerd via één poort van wordt de [Split Data] [ split] -module, en de andere helft via de andere. U kunt deze parameters, evenals de **Random seed** parameter, voor het wijzigen van de scheiding tussen trainings- en testdoeleinden gegevens. In dit voorbeeld laten we deze as-is.
   
   > [!TIP]
   > De eigenschap **fractie van rijen in de eerste uitvoergegevensset** bepaalt hoeveel van de gegevens wordt uitvoer via de *links* uitvoerpoort. Bijvoorbeeld, als u de hoogte-breedteverhouding ingesteld op 0,7, wordt 70% van de gegevens uitvoer via de poort naar links en 30% tot en met de juiste poort.  
   > 
   > 

3. Dubbelklik op de [Split Data] [ split] module en de opmerking invoeren, ' Training/testen gegevens gesplitst 50% '. 

Kunnen worden gebruikt in de uitvoer van de [Split Data] [ split] module echter we graag, maar kiezen we voor de linker uitvoer als trainingsgegevens en het recht uitvoer als testgegevens gebruikt.  

Zoals vermeld in de [vorige stap](walkthrough-2-upload-data.md), de kosten voor een hoge kredietrisico's als laag onjuiste vijf keer hoger is dan de kosten van een laag kredietrisico's zo hoog onjuiste. Als u wilt voor dit account, moet er een nieuwe gegevensset die overeenkomt met deze functie kosten gegenereerd. In de nieuwe gegevensset wordt elk voorbeeld met een hoog risico vijf keer gerepliceerd terwijl elk voorbeeld met een laag risico niet is gerepliceerd.   

We kunnen deze replicatie doen met behulp van R-code:  

1. Zoek en sleep de [R-Script uitvoeren] [ execute-r-script] module naar het experimentcanvas. 

2. Koppel de linkeruitvoerpoort van de [Split Data] [ split] module aan de eerste invoerpoort ('Dataset1') van de [R-Script uitvoeren] [ execute-r-script] module.

3. Dubbelklik op de [R-Script uitvoeren] [ execute-r-script] module en typ de opmerking 'Set kosten aanpassing'.

4. In de **eigenschappen** deelvenster, verwijdert u de standaardtekst in de **R-Script** parameter en voer dit script:
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![R-script in de module R-Script uitvoeren][9]

We moeten doen deze dezelfde replicatiebewerking voor elke uitvoer van de [Split Data] [ split] module zodat de training en tests gegevens de dezelfde kosten aanpassing hebben. De eenvoudigste manier om dit te doen is door het dupliceren van de [R-Script uitvoeren] [ execute-r-script] module die we zojuist hebt gemaakt en het verbinden met de andere uitvoerpoort van de [Split Data] [ split] module.

1. Met de rechtermuisknop op de [R-Script uitvoeren] [ execute-r-script] -module en selecteer **kopie**.

2. Met de rechtermuisknop op het experimentcanvas en selecteer **plakken**.

3. Sleep de nieuwe module naar de gewenste positie en sluit vervolgens de juiste uitvoerpoort van de [Split Data] [ split] module aan de eerste invoerpoort van deze nieuwe [R-Script uitvoeren] [ execute-r-script] module. 

4. Aan de onderkant van het canvas, klikt u op **uitvoeren**. 

> [!TIP]
> De kopie van het uitvoeren van R-Script-module bevat hetzelfde script als de oorspronkelijke module. Wanneer u kopieert en plakt een module op het canvas, behoudt het kopiëren van de eigenschappen van het origineel.  
> 
> 

Onze experiment nu ziet er ongeveer uit als volgt:

![De module Split en R-scripts toe te voegen][4]

Zie voor meer informatie over het gebruik van R-scripts in uw experimenten [uw experiment uitbreiden met R](extend-your-experiment-with-r.md).

**Volgende: [De modellen trainen en evalueren](walkthrough-4-train-and-evaluate-models.md)**

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
