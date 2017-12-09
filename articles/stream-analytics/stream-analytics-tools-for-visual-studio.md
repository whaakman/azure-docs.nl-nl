---
title: Gebruik Azure Stream Analytics-hulpprogramma's voor Visual Studio | Microsoft Docs
description: Zelfstudie voor de Azure Stream Analytics-Tools voor Visual Studio aan de slag
keywords: Visual studio
documentationcenter: 
services: stream-analytics
author: su-jie
manager: jhubbard
editor: cgronlun
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: sujie
ms.openlocfilehash: b06eae6b85f2ca41390955cde2499af2531e2e12
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="use-azure-stream-analytics-tools-for-visual-studio"></a>Azure Stream Analytics-hulpprogramma's voor Visual Studio gebruiken
Azure Stream Analytics-hulpprogramma's voor Visual Studio zijn nu algemeen beschikbaar. Deze hulpprogramma's inschakelen van een rijkere ervaring voor gebruikers van de Stream Analytics oplossen, evenals een complexe query's schrijven en zelfs schrijven lokaal query's. U kunt ook een Stream Analytics-taak exporteren naar een Visual Studio-project.

## <a name="introduction"></a>Inleiding
In deze zelfstudie leert u hoe u met Stream Analytics-tools voor Visual Studio maken, ontwerpen, lokaal testen, beheren en fouten opsporen in uw Stream Analytics-taken. 

Na het voltooien van deze zelfstudie, kunt u zich kunt:

* Lees de Stream Analytics-hulpprogramma's voor Visual Studio.
* Configureren en implementeren van een Stream Analytics-taak.
* Test uw taak lokaal met lokale voorbeeldgegevens.
* Gebruik de bewaking problemen kunt oplossen.
* Bestaande taken exporteren naar projecten.

## <a name="prerequisites"></a>Vereisten
U moet de volgende vereisten om deze zelfstudie te voltooien:

* Voltooi de stappen maximaal 'Een Stream Analytics-taak maken' in de zelfstudie [een IoT-oplossing bouwen met behulp van de Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics). 
* Visual Studio 2017, Visual Studio 2015 of Visual Studio 2013 Update 4 installeren. Enterprise (Ultimate/Premium), Professional en Community-edities worden ondersteund. Express edition wordt niet ondersteund. 
* Ga als volgt de [installatie-instructies](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-install) Stream Analytics-hulpprogramma's voor Visual Studio installeren.

## <a name="create-a-stream-analytics-project"></a>Een Stream Analytics-project maken
Selecteer in Visual Studio **bestand** > **nieuw Project**. Selecteer in de lijst met sjablonen aan de linkerkant **Stream Analytics**, en selecteer vervolgens **Azure Stream Analytics toepassing**.
Voer het project aan de onderkant van de pagina **naam**, **locatie**, en **oplossingsnaam** als voor andere projecten.

![Nieuw project maken](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-01.png)

Het project **Tolstation** wordt gegenereerd **Solution Explorer**.

![Tolstation-project in Solution Explorer](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-02.png)

## <a name="choose-the-correct-subscription"></a>Kies het juiste abonnement
1. Op de **weergave** selecteert u **Server Explorer** in Visual Studio.

2. Aanmelden met uw Azure-account. 

## <a name="define-input-sources"></a>Invoerbronnen definiëren
1. In **Solution Explorer**, vouw de **invoer** knooppunt en wijzig de naam **Input.json** naar **EntryStream.json**. Dubbelklik op **EntryStream.json**.

2. Voor **invoer Alias**, voer **EntryStream**. Houd er rekening mee dat de invoeralias wordt gebruikt in query-script.

3. Voor **brontype**, selecteer **gegevensstroom**.

4. Voor **bron**, selecteer **Event Hub**.

5. Voor **Service Bus Namespace**, selecteer de **TollData** optie in de vervolgkeuzelijst.

6. Voor **Event Hub-naam**, selecteer **vermelding**.

7. Voor **Event Hub-beleidsnaam**, selecteer **RootManageSharedAccessKey** (de standaardwaarde).

8. Voor **gebeurtenis serialisatie-indeling**, selecteer **Json**, en voor **codering**, selecteer **UTF8**.
   
   Uw instellingen moeten uitzien:
   
   ![Instellingen voor invoer](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-01.png)
   
9. Selecteer aan de onderkant van de pagina **opslaan** om de wizard te voltooien. U kunt nu een andere invoer bron voor het maken van de stroom afsluiten toevoegen. Met de rechtermuisknop op de **invoer** uit en selecteer **Nieuw Item**.
   
   ![Nieuw Item](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-02.png)
   
10. Kies in het pop-upvenster **Stream Analytics invoer**, en wijzig de **naam** naar **ExitStream.json**. Selecteer **Toevoegen**.
   
    ![Nieuw Item toevoegen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-03.png)
   
11. Dubbelklik op **ExitStream.json** in het project en volg dezelfde stappen als de stroom van de vermelding in de velden te vullen. Voor **naam Event Hub**, voer **sluiten**, zoals wordt weergegeven in de volgende schermafbeelding:
   
    ![ExitStream instellingen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-04.png)
   
   U hebt nu twee invoer streams gedefinieerd.
   
   ![Twee invoerstromen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-05.png)
   
   Vervolgens voegt u de gegevensinvoer verwijzing voor het blob-bestand dat de registratiegegevens auto bevat.
   
12. Met de rechtermuisknop op de **invoer** knooppunt in het project en volg voor de invoer stroom hetzelfde proces. Voor **brontype**, selecteer **referentiegegevens**, en voor **invoer Alias**, voer **registratie**.
   
    ![Registratie-instellingen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-06.png)
   
13. Selecteer de **opslag** rekening met de optie met **TollData**. De containernaam van de is **TollData**, en de **pad patroon** is **registration.json**. Deze naam is hoofdlettergevoelig en moet in kleine letters.

14. Selecteer **opslaan** om de wizard te voltooien.

Nu zijn de invoer gedefinieerd.

## <a name="define-output"></a>Uitvoer definiëren
1. In **Solution Explorer**, vouw de **invoer** knooppunt en dubbelklik op **Output.json**.

2. Voor **Uitvoeraliassen**, voer **uitvoer**. Voor **Sink**, selecteer **SQL-Database**.

3. Voor de **Database** naam, voer **TollDataDB**.

4. Voor **gebruikersnaam**, voer **tolladmin**. Voor **wachtwoord**, voer **123toll!**. Voor **tabel**, voer **TollDataRefJoin**.

5. Selecteer **Opslaan**.

   ![Voor de uitvoercache](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-output-01.png)
 
## <a name="stream-analytics-query"></a>Stream Analytics query
Deze zelfstudie probeert verschillende bedrijven vragen die zijn gerelateerd aan gegevens tolbruggen te beantwoorden. We samengesteld query's die kunnen worden gebruikt in de Stream Analytics relevante beantwoorden. Voordat u uw eerste Stream Analytics-taak, gaan we een eenvoudig scenario en de querysyntaxis verkennen.

### <a name="introduction-to-stream-analytics-query-language"></a>Inleiding tot Stream Analytics query language
Stel dat u moet het aantal voertuigen die een tolstation stand invoeren. Omdat deze stroom gebeurtenissen continue is, hebt u voor het definiëren van een bepaalde periode. Laten we wijzigen de vraag om 'hoeveel voertuigen Geef een tolstation stand elke drie minuten?' Deze meting wordt vaak aangeduid als het aantal daling.

Bekijk de Stream Analytics query waarmee deze vraag worden beantwoord:

        SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count 
        FROM EntryStream TIMESTAMP BY EntryTime 
        GROUP BY TUMBLINGWINDOW(minute, 3), TollId 

Zoals u ziet, Stream Analytics maakt gebruik van een querytaal die is vergelijkbaar met SQL. Enkele uitbreidingen om op te geven tijd gerelateerde aspecten van de query wordt toegevoegd.

Lees voor meer informatie over [time management](https://msdn.microsoft.com/library/azure/mt582045.aspx) en [windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) constructies die worden gebruikt in de query van MSDN.

Nu dat u uw eerste Stream Analytics query geschreven hebt, test u deze met behulp van bestanden met voorbeeldgegevens zich in de map TollApp in het volgende pad:

**..\TollApp\TollApp\Data**

Deze map bevat de volgende bestanden:

* Entry.JSON
* Exit.JSON
* Registration.JSON

## <a name="question-number-of-vehicles-entering-a-toll-booth"></a>Vraag: Aantal voertuigen een tolstation stand
Dubbelklik in het project op **Script.asaql** het script in de editor te openen. Het script in de vorige sectie in de editor plakken. De query-editor biedt ondersteuning voor IntelliSense syntaxiskleuren en de markering van een fout.

![query-editor](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="test-stream-analytics-queries-locally"></a>Stream Analytics query's lokaal testen
U kunt de query om te zien of er een syntaxisfout eerst compileren.

1. Voor het valideren van deze query op de voorbeeldgegevens, kunt u lokale voorbeeldgegevens gebruikt met de rechtermuisknop op de invoer selecteren **lokale invoer toevoegen**.
   
   ![Lokale invoer toevoegen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-01.png)
   
2. Selecteer de voorbeeldgegevens van uw lokale pad in het pop-upvenster. Selecteer **Opslaan**.
   
   ![Lokale invoer toevoegen](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-02.png)
   
   Een bestand met de naam **local_EntryStream.json** wordt automatisch toegevoegd aan de map van uw invoer.
   
   ![Lijst met lokale invoermap bestanden](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-03.png)
   
3. Selecteer **lokaal uitvoeren** in de query-editor. Of u kunt op F5 drukken.
   
   ![Lokaal uitvoeren](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-01.png)
   
   U vindt het uitvoerpad van de console-uitvoer. Druk op een willekeurige toets om de resultaat-map te openen.
   
   ![Lokaal uitvoeren](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-02.png)
   
4. Controleer de resultaten in de lokale map.
   
   ![Resultaat van de lokale map](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-03.png)
   
   
### <a name="sample-input"></a>Voorbeeldinvoer
U kunt ook invoergegevens uit invoerbronnen naar het lokale bestand sample. Met de rechtermuisknop op het configuratiebestand van de invoer en selecteer **voorbeeldgegevens**. 

![Voorbeeldgegevens](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-01.png)

Houd er rekening mee dat u een steekproef alleen event hubs of IoT hubs nu nemen kunt. Andere bronnen worden niet ondersteund. Vul het lokale pad naar het opslaan van de voorbeeldgegevens in het pop-updialoogvenster. Selecteer **voorbeeld**.

![Voorbeeldconfiguratie voor gegevens](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-02.png)
 
U ziet de voortgang in de **uitvoer** venster. 

![Voorbeelduitvoer van gegevens](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-03.png)
 
### <a name="submit-a-stream-analytics-query-to-azure"></a>Verzenden van een Stream Analytics-query naar Azure
1. In de **Query-Editor**, selecteer **verzenden naar Azure** in de scripteditor.

   ![Verzenden naar Azure](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-01.png)
 
2. Selecteer **maken van een nieuwe Azure Stream Analytics-taak**. Voor **taaknaam**, voer **TollApp**. Kies de juiste **abonnement** in de vervolgkeuzelijst. Selecteer **indienen**.

   ![Verzenden van taak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-02.png)

 
### <a name="start-the-job"></a>Start de taak
Nu uw taak wordt gemaakt en de taakweergave wordt automatisch geopend. 
1. Selecteer de groene pijl om de taak te starten.

   ![Taak knop Start](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-01.png)
 
2. Kies de standaardinstelling en selecteer **Start**.
 
   ![Taak starten](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-02.png)

   U ziet de status van de taak is gewijzigd in **met**, en er zijn i/o-gebeurtenissen.

   ![Taakoverzicht en metrische gegevens](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-03.png)

## <a name="check-results-in-visual-studio"></a>Controleer de resultaten in Visual Studio
1. Open Visual Studio Server Explorer en met de rechtermuisknop op de **TollDataRefJoin** tabel.

2. Selecteer **tabelgegevens weergeven** om te zien van de uitvoer van de taak.
   
   ![Gegevens van de tabel weergeven](media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-check-results.jpg)
   

### <a name="view-job-metrics"></a>Metrische gegevens weergeven taak
Een aantal statistieken basic taak worden weergegeven in **taak metrische gegevens**. 

![Taak metrische gegevens](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-metrics-01.png)

 
## <a name="list-the-job-in-server-explorer"></a>Lijst van de taak in Server Explorer
In **Server Explorer**, selecteer **Stream Analytics-taken** en selecteer vervolgens **vernieuwen**. De taak wordt weergegeven onder **Stream Analytics-taken**.

![Takenlijst met](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-list-jobs-01.png)


## <a name="open-the-job-view"></a>De taakweergave openen
Vouw het knooppunt van uw project en dubbelklik op de **taakweergave** knooppunt om de taakweergave van een te openen.

![Taak weergeven](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-view-01.png)


## <a name="export-an-existing-job-to-a-project"></a>Exporteren van een bestaande taak aan een project
Er zijn twee manieren kunt u een bestaande taak exporteren naar een project.
* In **Server Explorer**onder de **Stream Analytics-taken** knooppunt met de rechtermuisknop op het taakknooppunt. Selecteer **exporteren naar nieuwe Stream Analytics-Project**.
   
   ![Exporteren naar nieuwe Stream Analytics-Project](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-01.png)
   
   Het gegenereerde project wordt weergegeven **Solution Explorer**.
   
    ![Solution Explorer taak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-02.png)
   
* Selecteer in de taakweergave **genereren Project**.
   
   ![Genereren van Project](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-03.png)
   
## <a name="known-issues-and-limitations"></a>Bekende problemen en beperkingen
 
* Lokale testen werkt niet als de query geo-ruimtelijke functies heeft.
* Ondersteuning van editor is niet beschikbaar voor het toevoegen of JavaScript UDF wijzigen.
* Biedt geen ondersteuning voor het testen van lokale opslaan van uitvoer in JSON-indeling. 
* Ondersteuning is niet beschikbaar voor Power BI-uitvoer en ADLS-uitvoer.



## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met behulp van Azure Stream Analytics](stream-analytics-get-started.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics query language reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics management REST API-referentiemateriaal](https://msdn.microsoft.com/library/azure/dn835031.aspx)


