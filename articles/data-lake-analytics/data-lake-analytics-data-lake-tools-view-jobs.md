---
title: Job Browser en Job View gebruiken voor Azure Data Lake Analytics-taken
description: In dit artikel wordt beschreven hoe u Job Browser en Job View gebruiken voor Azure Data Lake Analytics-taken.
services: data-lake-analytics
ms.service: data-lake-analytics
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonwhowell
ms.assetid: bdf27b4d-6f58-4093-ab83-4fa3a99b5650
ms.topic: conceptual
ms.date: 08/02/2017
ms.openlocfilehash: 474478c8049dd97558b49b1df4b00655268fc0b3
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43044095"
---
# <a name="use-job-browser-and-job-view-for-azure-data-lake-analytics"></a>Gebruik Job Browser en Job View voor Azure Data Lake Analytics
De service Azure Data Lake Analytics-archieven verzonden taken in een [query store](#query-store). In dit artikel leert u hoe u Job Browser en Job View gebruiken in Azure Data Lake Tools voor Visual Studio om de historische taakinformatie te vinden. 

Standaard archiveert de Data Lake Analytics-service de taken voor 30 dagen. De verloopperiode kan worden geconfigureerd via de Azure-portal door het configureren van het aangepaste beleid. U pas weer toegang tot de informatie over de taak na de verloopdatum. 

## <a name="prerequisites"></a>Vereisten
Zie [Data Lake Tools voor Visual Studio-vereisten](data-lake-analytics-data-lake-tools-get-started.md#prerequisites).

## <a name="open-the-job-browser"></a>Open de Browser van de taak
Toegang tot de Browser taak via **Server Explorer > Azure > Data Lake Analytics > taken** in Visual Studio.  Met behulp van de Job Browser, kunt u toegang tot het queryarchief van een Data Lake Analytics-account. Job Browser wordt Query Store aan de linkerkant weergegeven met informatie over algemene taak en taak bekijken op de juiste weergeven gedetailleerde informatie over taak.

## <a name="job-view"></a>Taak weergeven
Project-weergave bevat de gedetailleerde gegevens van een taak. U kunt als u wilt een taak openen, dubbelklikt u op een taak in de Browser van de taak of in het Data Lake-menu openen door te klikken op taak weergeven. U ziet een dialoogvenster met de taak-URL zijn ingevuld.

![Data Lake Tools in Visual Studio Job Browser](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view.png)

Project-weergave bevat:

* Taakoverzicht
  
    Vernieuw de weergave van de taak om te zien van de meest recente gegevens van het uitvoeren van taken.
  
  * Taakstatus (graph):
    
      De Status van taak geeft een overzicht van de fasen van de taak:
    
      ![Status van Azure Data Lake Analytics-taak fasen](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-phases.png)
    
    * Voorbereiding: Uw script uploaden naar de cloud, compileren en optimaliseren van het script met behulp van de compile-service.
    * In de wachtrij: Taken in de wachtrij geplaatst wanneer ze op onvoldoende bronnen wachten of de taken langer zijn dan de maximale gelijktijdige taken per accountbeperking. De prioriteitsinstelling van de bepaalt de volgorde van de taken in de wachtrij - hoe lager het nummer, hoe hoger de prioriteit.
    * Uitvoeren: De taak daadwerkelijk wordt uitgevoerd in uw Data Lake Analytics-account.
    * Wordt voltooid: De taak wordt voltooid (bijvoorbeeld, wordt het bestand voltooid).
      
      De taak kan mislukken in elke fase. Bijvoorbeeld, compilatiefouten in de fase voorbereiden, de time-outfouten optreden in de fase in de wachtrij geplaatst en de uitvoeringsfouten in de fase wordt uitgevoerd, enzovoort.
  * Algemene informatie
    
      De eenvoudige taakinformatie wordt weergegeven in het onderste gedeelte van het paneel taaksamenvatting.
    
      ![Status van Azure Data Lake Analytics-taak fasen](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-info.png)
    
    * Taak resultaat: Geslaagd of mislukt. De taak kan mislukken in elke fase.
    * Totale duur: Wand clock-tijd (duur) tussen de tijd verzenden- en eindtijd.
    * Totale tijd: De som van de uitvoeringstijd voor elk hoekpunt, u kunt overwegen deze als de tijd die de taak wordt uitgevoerd in slechts één hoekpunt. Raadpleeg de totale hoekpunten voor meer informatie over het hoekpunt.
    * Tijd van verzenden/begin/einde: De tijd wanneer de Data Lake Analytics-service taak indienen/begint ontvangt met het uitvoeren van de taak/eindigt de taak is of niet.
    * In de wachtrij geplaatst-compilatie/uitvoeren: Kloktijd besteed aan het tijdens de fase voorbereiden/in de wachtrij geplaatst/uitvoeren.
    * Account: De Data Lake Analytics-account gebruikt voor het uitvoeren van de taak.
    * Auteur: De gebruiker die eigenaar van de taak kan zijn van een echte persoon account of een systeemaccount.
    * Prioriteit: De prioriteit van de taak. Des te lager het nummer, hoe hoger de prioriteit. Dit geldt alleen voor de volgorde van de taken in de wachtrij. Instellen van een hogere prioriteit komt niet hebben voorrang op taken die worden uitgevoerd.
    * Parallelle uitvoering: De aangevraagde maximum aantal gelijktijdige Azure Data Lake Analytics Units (ADLAUs), ook wel hoekpunten. Op dit moment één hoekpunt is gelijk aan één virtuele machine met twee virtuele kern en zes GB RAM-geheugen, maar dit kan worden bijgewerkt in de toekomst Data Lake Analytics-updates.
    * Bytes links: Bytes die worden verwerkt moeten totdat de taak is voltooid.
    * Bytes lezen/geschreven: Bytes die zijn gelezen of weggeschreven sinds de taak is gestart.
    * Totaal aantal hoekpunten: de taak wordt opgedeeld in veel werk delen, elke stuk werk een hoekpunt wordt genoemd. Deze waarde wordt beschreven hoeveel werk met onderdelen van de taak bestaat uit. U kunt een hoekpunt beschouwen als een eenheid basisproces, ook wel Azure Data Lake Analytics Unit (ADLAU), en hoekpunten in de parallelle uitvoering kunnen worden uitgevoerd. 
    * Voltooid/actief/is mislukt: Het aantal hoekpunten voltooid/actief/is mislukt. Hoekpunten kunnen mislukken als beide gebruikersfouten code en het systeem, maar het systeem nieuwe pogingen niet hoekpunten automatisch een paar keer. Als het hoekpunt is nog steeds is mislukt na het opnieuw proberen, wordt de hele taak mislukt.
* Taakgrafiek
  
    Een U-SQL-script vertegenwoordigt de logica van invoergegevens om uitvoergegevens te transformeren. Het script is gecompileerd en geoptimaliseerd voor een fysieke uitvoeringsplan in de fase voorbereiden. De Taakgrafiek is om de fysieke uitvoeringsplan weer te geven.  Het volgende diagram illustreert het proces:
  
    ![Status van Azure Data Lake Analytics-taak fasen](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-logical-to-physical-plan.png)
  
    Een taak wordt opgedeeld in veel werk delen. Elke stuk werk heet een hoekpunt. De hoekpunten zijn gegroepeerd als Super hoekpunt (ook wel fasering), en als Taakgrafiek gevisualiseerd. De fasen van weergeven de opschriften groen fase in de taakgrafiek
  
    Elke hoekpunt in een fase gaat met de dezelfde soort werken met verschillende onderdelen van dezelfde gegevens. Als u een bestand met gegevens van one TB hebt en er zijn honderden hoekpunten lezen uit het, wordt elk van deze bijvoorbeeld een segment gelezen. Deze hoekpunten worden gegroepeerd in dezelfde fase en als dit hetzelfde op verschillende onderdelen van dezelfde invoerbestand werkt.
  
  * <a name="state-information"></a>Informatie voor fase
    
      In een bepaalde fase worden getallen die weergegeven in het plakkaat met.
    
      ![Azure Data Lake Analytics-taak graph fase](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage.png)
    
    * SV1 Extraheren: de naam van een fase, met de naam van een getal en de bewerkingsmethode.
    * 84 hoekpunten: het totale aantal hoekpunten in deze fase. De afbeelding geeft aan hoeveel onderdelen werk is onderverdeeld in deze fase.
    * 12.90 s/hoekpunt: de gemiddelde hoekpunt uitvoeringstijd voor deze fase. In deze afbeelding wordt berekend door SUM (elke hoekpunt uitvoeringstijd) / (totaal aantal hoekpunt). Dit betekent dat als u de hoekpunten die zijn uitgevoerd in de parallelle uitvoering toewijst kan, de hele fase is voltooid in 12,90 s. Het betekent ook dat als al het werk in deze fase van de serie wordt uitgevoerd, de kosten zijn #vertices * Gem. tijd.
    * 850,895 geschreven rijen: totaal aantal rijen in deze fase is geschreven.
    * R/W: hoeveelheid gegevens lezen/schriftelijke in deze fase in bytes.
    * Kleuren: De kleuren worden gebruikt in de fase voor verschillende hoekpunt status aan te geven.
      
      * Groen geeft aan dat het hoekpunt is voltooid.
      * Oranje geeft aan dat het hoekpunt wordt opnieuw uitgevoerd. Het opnieuw geprobeerd hoekpunt is mislukt, maar wordt opnieuw geprobeerd automatisch en is door het systeem en de algehele fase is voltooid. Als het hoekpunt opnieuw geprobeerd maar nog steeds mislukt, wordt de kleur rood en de hele taak is mislukt.
      * Rood geeft aan dat is mislukt, wat betekent dat een bepaalde hoekpunt was zijn een paar keer geprobeerd door het systeem, maar nog steeds is mislukt. In dit scenario zorgt ervoor dat de hele taak is mislukt.
      * Blauwe betekent dat een bepaalde hoekpunt wordt uitgevoerd.
      * Technische geeft aan dat het hoekpunt is in afwachting van. Het hoekpunt kan moeten worden gepland zodra een ADLAU beschikbaar, of deze kan worden wacht op invoer omdat de invoergegevens mogelijk niet gereed.
      
      U vindt meer informatie voor de fase door de muisaanwijzer op een status wordt bewogen over:
      
      ![Azure Data Lake Analytics-taak graph fase details](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage-details.png)
  * Hoekpunten: Beschrijving van de details van hoekpunten, bijvoorbeeld het aantal hoekpunten in totaal, het aantal hoekpunten zijn voltooid, zijn ze is mislukt of nog actief/wachten, enzovoort.
  * Het lezen van gegevens tussen/intra-schil: bestanden en gegevens worden opgeslagen in meerdere schillen in een gedistribueerd bestandssysteem. De waarde hier wordt beschreven hoeveel gegevens in de dezelfde schil of cross-schil is gelezen.
  * Totale tijd: de som van de uitvoeringstijd van elke hoekpunt in de fase, kunt u overwegen deze als de lang het duren zou als alle in de fase werkt wordt uitgevoerd in slechts één hoekpunt.
  * Gegevens en rijen geschreven/gelezen: Hiermee wordt aangegeven hoeveel gegevens of rijen zijn gelezen of weggeschreven of moeten worden gelezen.
  * Hoekpunt lezen fouten: Beschrijving van het aantal hoekpunten zijn mislukt tijdens het lezen van gegevens.
  * Dubbele hoekpunt wordt genegeerd: als een hoekpunt te langzaam wordt uitgevoerd, het systeem meerdere hoekpunten om uit te voeren van de dezelfde stuk werk kan plannen. Reductant hoekpunten gaan verloren zodra een van de hoekpunten met succes voltooid. Dubbele verwijderingen hoekpunt legt het aantal hoekpunten die worden verwijderd als dubbele in de fase.
  * Hoekpunt intrekkingen: het hoekpunt is geslaagd, maar enkele redenen later ophalen opnieuw. Bijvoorbeeld, als downstream hoekpunt tussenliggende invoergegevens verliest, wordt u gevraagd het upstream hoekpunt opnieuw uit te voeren.
  * Hoekpunt planning uitvoeringen: de totale tijd die de hoekpunten zijn gepland.
  * Gemiddelde-min/Max hoekpunt gegevens gelezen: het minimum/gemiddelde/maximum van elk hoekpunt lezen van gegevens.
  * Duur: De kloktijd een fase nodig, moet u laden profiel als deze waarde wilt weergeven.
  * Taak afspelen
    
      Data Lake Analytics taken wordt uitgevoerd en archiveert de hoekpunten met informatie van de taken, zoals wanneer de hoekpunten die zijn gestart, gestopt, is mislukt en hoe ze opnieuw kunnen worden uitgevoerd, enzovoort. Alle informatie wordt automatisch geregistreerd in de query store en opgeslagen in het profiel van de taak. U kunt het profiel van de taak via 'Belastingsprofiel' in de taakweergave downloaden en vindt u de taak afspelen nadat u hebt gedownload van het profiel van de taak.
    
      Taak afspelen is een visualisatie epitome van wat is er gebeurd in het cluster. Hiermee kunt u de voortgang van de taak bekijken en visueel vaststellen afwijkingen en knelpunten in zeer korte tijd (minder dan 30 s meestal).
  * Taak Heat Map weergeven 
    
      Heatmap van de taak kan worden geselecteerd via de vervolgkeuzelijst weergegeven in de Taakgrafiek. 
    
      ![Azure Data Lake Analytics-taak graph heap kaart weergeven](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-display.png)
    
      U ziet de heatmap i/o, tijd en doorvoer van een taak, waarmee u kunt vinden waar de taak deel van de tijd besteedt, of of de taak is een i/o-grens-taak, enzovoort.
    
      ![Voorbeeld van Azure Data Lake Analytics-taak graph heap kaart](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-example.png)
    
    * Voortgang: De taakuitvoering voortgang, Zie de informatie in [stadium informatie](#stage-information).
    * Gelezen of weggeschreven gegevens: de heatmap van de totale gegevens lezen/geschreven in elke fase.
    * COMPUTE tijd: de heatmap van SUM (elke hoekpunt uitvoeringstijd), kunt u overwegen deze hoe lang het duren zou als er al het werk in de fase wordt uitgevoerd met slechts 1 hoekpunt.
    * Gemiddelde uitvoeringstijd per knooppunt: de heatmap van SUM (elke hoekpunt uitvoeringstijd) / (hoekpunt getal). Dit betekent dat als u de hoekpunten die zijn uitgevoerd in de parallelle uitvoering toewijst kan, kunt u de hele fase wordt uitgevoerd in dit tijdsbestek.
    * I/o-doorvoer: de heatmap van i/o-doorvoer van elke fase, kunt u controleren als de taak een i/o-gebonden taak via deze is.
* Bewerkingen voor metagegevens
  
    U kunt enkele bewerkingen van metagegevens in uw U-SQL-script uitvoeren, zoals een database te maken, verwijderen van een tabel, enzovoort. Deze bewerkingen worden weergegeven in de metagegevens is na de compilatie. U kunt vinden van asserties, entiteiten maken, entiteiten hier neerzetten.
  
    ![Azure Data Lake Analytics-taak weergeven voor de bewerkingen van metagegevens](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-metadata-operations.png)
* Geschiedenis van status
  
    De geschiedenis van Knooppuntstatus tevens worden gevisualiseerd in taaksamenvatting, maar krijgt u hier meer informatie. U kunt de gedetailleerde informatie kunt vinden, zoals wanneer de taak is voorbereid, in de wachtrij de slag te gaan die wordt uitgevoerd, is beëindigd. U kunt ook het aantal keren dat de taak is gecompileerd vinden (de CcsAttempts: 1), wanneer de taak verzonden naar het cluster feitelijk (details: verzonden met codepad-taak voor het cluster), enzovoort.
  
    ![Statusgeschiedenis van Azure Data Lake Analytics-taak weergeven](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-state-history.png)
* Diagnostiek
  
    Het hulpprogramma diagnoses automatisch uitvoeren van taak. U ontvangt waarschuwingen wanneer er fouten of prestatieproblemen in uw taken. Houd er rekening mee dat u het profiel moet voor volledige informatie hier downloaden. 
  
    ![Diagnostische gegevens van Azure Data Lake Analytics-taak weergeven](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-diagnostics.png)
  
  * Waarschuwingen: Een waarschuwing wordt weergegeven hier met waarschuwing van compiler. U kunt klikken op 'x oorzaak van deze' koppeling om meer informatie wanneer de waarschuwing wordt weergegeven.
  * Hoekpunt uitvoeren te lang: als een hoekpunt onvoldoende tijd (bijvoorbeeld 5 uur), problemen hier worden gevonden.
  * Gebruik: als u meer of onvoldoende parallelle uitvoering toegewezen dan nodig hebt, problemen hier worden gevonden. Ook kunt u gebruik van bronnen voor meer informatie en what-if-scenario's voor een betere resourcetoewijzing uitvoert (Zie voor meer informatie in deze handleiding).
  * Geheugencontrole: als een hoekpunt maakt gebruik van meer dan 5 GB aan geheugen, problemen hier worden gevonden. Uitvoeren van taak kan ophalen beëindigd door een systeem als deze meer geheugen dan systeembeperking gebruikt.

## <a name="job-detail"></a>Taakdetails
Job details ziet u de gedetailleerde informatie van de taak, waaronder Script, Resources en Vertex Execution View.

![Azure Data Lake Analytics-job details](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-details.png)

* Script
  
    De U-SQL-script van de taak wordt opgeslagen in de query store. U kunt het oorspronkelijke U-SQL-script weergeven en opnieuw te verzenden als nodig is.
* Resources
  
    U kunt de taak compilatie uitvoer die zijn opgeslagen in de query store resources vinden. U kunt bijvoorbeeld "algebra.xml' die wordt gebruikt om de Taakgrafiek weer te geven, assembly's die u hebt geregistreerd, enz. hier vinden.
* Hoekpunt uitvoering weergeven
  
    Deze hoekpunten worden uitvoeringsdetails weergegeven. Het profiel van de taak archieven elke uitvoeringslogboek hoekpunt zoals totale gegevens lezen/geschreven, runtime, status, enzovoort. Via deze weergave krijgt u meer informatie over hoe een taak is uitgevoerd. Zie voor meer informatie, [de Vertex Execution View gebruiken in Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).

## <a name="next-steps"></a>Volgende stappen
* Zie [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md) (Diagnostische logboeken openen voor Azure Data Lake Analytics) voor logboekregistratie van diagnostische informatie.
* Zie [Websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md) voor een complexere query.
* Als u wilt vertex execution view gebruiken, Zie [de Vertex Execution View in Data Lake Tools voor Visual Studio gebruiken](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)

