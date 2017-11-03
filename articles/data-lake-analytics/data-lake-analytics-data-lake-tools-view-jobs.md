---
title: Taak Browser en weergave van de taak voor Azure Data Lake Analytics-taken gebruiken | Microsoft Docs
description: 'Informatie over het gebruik van de Browser van de taak en taak weergeven voor Azure Data Lake Analytics-taken. '
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: bdf27b4d-6f58-4093-ab83-4fa3a99b5650
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/02/2017
ms.author: jgao
ms.openlocfilehash: 8f1729f84a4fde2a56427a41b356d6263818519e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-job-browser-and-job-view-for-azure-data-lake-analytics-jobs"></a>Taak Browser en weergave van de taak voor Azure Data lake Analytics-taken gebruiken
De service Azure Data Lake Analytics archiveert ingediende taken in een [queryarchief](#query-store). In dit artikel leert u het gebruik van taak Browser en taak weergeven in Azure Data Lake Tools voor Visual Studio om de historische taakinformatie te vinden. 

Standaard wordt in de Data Lake Analytics-service de taken voor 30 dagen gearchiveerd. De verloopperiode kan worden geconfigureerd via de Azure-portal door het aangepaste beleid configureren. Niet mogelijk voor toegang tot de informatie over de taak na de verloopdatum. 

## <a name="prerequisites"></a>Vereisten
Zie [Data Lake Tools voor Visual Studio vereisten](data-lake-analytics-data-lake-tools-get-started.md#prerequisites).

## <a name="open-the-job-browser"></a>Open de Browser van de taak
Toegang tot de taak Browser via **Server Explorer > Azure > Data Lake Analytics > taken** in Visual Studio.  De taak-Browser gebruikt, kunt u toegang tot het queryarchief van een Data Lake Analytics-account. Taak Browser wordt Query Store aan de linkerkant weergegeven met informatie over basic taak en gedetailleerde informatie over de taak van taak weergeven op de juiste weergeeft.

## <a name="job-view"></a>Taak weergeven
Project-weergave bevat de gedetailleerde gegevens van een taak. U kunt voor een taak openen, dubbelklikt u op een taak in de Browser van de taak of door te klikken op de weergave van de taak openen vanuit het menu Data Lake. U ziet een dialoogvenster gevuld met de URL van de taak.

![Data Lake Tools Browser voor Visual Studio-project](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view.png)

Weergave van de taak bevat:

* Taakoverzicht
  
    Vernieuw de weergave van de taak de meest recente gegevens van actieve taken wilt bekijken.
  
  * Status van de taak (grafiek):
    
      De Status van taak geeft een overzicht van de taak fasen:
    
      ![Status fasen Azure Data Lake Analytics-taak](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-phases.png)
    
    * Voorbereiden: Uw script uploaden naar de cloud, compileren en optimaliseren het script met behulp van de compile-service.
    * In de wachtrij: Taken in de wachtrij wei ze voldoende bronnen wachten zijn of de taken overschrijdt het maximumaantal gelijktijdige taken per accountbeperking. De prioriteitsinstelling bepaalt de volgorde van de taken in de wachtrij - hoe lager het getal, hoe hoger de prioriteit.
    * Uitvoeren: De taak daadwerkelijk wordt uitgevoerd in uw Data Lake Analytics-account.
    * Afronden: De taak is voltooid (bijvoorbeeld het bestand afronden).
      
      De taak kan mislukken in elke fase. Bijvoorbeeld, compilatiefouten optreden in de fase voorbereiden, de time-outfouten in de fase in de wachtrij geplaatst en de uitvoeringsfouten in de fase die wordt uitgevoerd, enzovoort.
  * Algemene informatie
    
      De informatie over basic taak ziet u in het onderste gedeelte van het paneel taakoverzicht.
    
      ![Status fasen Azure Data Lake Analytics-taak](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-info.png)
    
    * Resultaat van taak: Is geslaagd of mislukt. De taak kan mislukken in elke fase.
    * Totale duur: Wandstructuur kloktijd (duur) tussen het verzenden van tijd- en eindtijd.
    * Totale tijd Compute: De som van de uitvoeringstijd van elke hoekpunt, u kunt overwegen deze als de tijd die de taak wordt uitgevoerd in slechts één hoekpunt. Raadpleeg de totale hoekpunten voor meer informatie over het hoekpunt.
    * Verzenden/beginnen of eindigen tijd: De tijd wanneer de Data Lake Analytics-service taak verzending/begint ontvangt met de taak/ends taak uitvoeren is of niet.
    * In de wachtrij geplaatst-compilatie/uitvoeren: Kloktijd besteed tijdens de fase in de wachtrij geplaatst-voorbereiden/uitgevoerd.
    * Account: De Data Lake Analytics-account gebruikt voor het uitvoeren van de taak.
    * Auteur: De gebruiker die eigenaar van de taak kan bestaan uit een echte persoon account of een systeem-account.
    * Prioriteit: De prioriteit van de taak. Hoe lager het getal, hoe hoger de prioriteit. Dit geldt alleen voor de volgorde van de taken in de wachtrij. Instellen van een hogere prioriteit heeft geen hebben voorrang op taken die worden uitgevoerd.
    * Parallelle uitvoering: De aangevraagde maximum aantal gelijktijdige Azure Data Lake Analytics-eenheden (ADLAUs), ook wel hoekpunten. Op dit moment één hoekpunt is gelijk aan één virtuele machine met twee virtuele core en 6 GB RAM-geheugen, hoewel dit kan worden bijgewerkt in de toekomst Data Lake Analytics-updates.
    * Links bytes: Bytes die moeten worden verwerkt totdat de taak is voltooid.
    * Bytes gelezen of weggeschreven: Bytes die zijn gelezen of weggeschreven sinds de taak is gestart.
    * Totaal aantal hoekpunten: de taak wordt opgedeeld in veel werk delen, elke stuk werk een hoekpunt wordt aangeroepen. Deze waarde wordt beschreven hoe veel werk delen de taak bestaat uit. U kunt een hoekpunt beschouwen als een eenheid basisproces, ook wel Azure Data Lake Analytics Unit (ADLAU), en hoekpunten kunnen worden uitgevoerd in de parallelle uitvoering. 
    * Voltooid/actief/is mislukt: Het aantal hoekpunten voltooid/actief/is mislukt. Hoekpunten kunnen mislukken door beide gebruiker code en system-fouten, maar de pogingen systeem niet hoekpunten automatisch een paar keer. Als het hoekpunt nog steeds na het opnieuw proberen mislukt is, wordt de hele taak zal mislukken.
* Taakgrafiek
  
    Een U-SQL-script vertegenwoordigt de logica van het transformeren van invoergegevens naar uitvoergegevens. Het script is gecompileerd en geoptimaliseerd voor een fysieke uitvoeringsplan in de fase voorbereiden. Grafiek van de taak is het fysieke uitvoeringsplan weergeven.  Het volgende diagram illustreert het proces:
  
    ![Status fasen Azure Data Lake Analytics-taak](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-logical-to-physical-plan.png)
  
    Een taak wordt opgedeeld in veel werk delen. Elke stuk werk wordt een hoekpunt genoemd. De hoekpunten zijn gegroepeerd als Super hoekpunt (aka stap) en weergegeven als Taakgrafiek. De fasen van weergeven de opschriften groen fase in de taakgrafiek
  
    Elke hoekpunt in een stadium doet hetzelfde type werken met verschillende onderdelen van dezelfde gegevens. Bijvoorbeeld, als u een bestand met één TB gegevens hebt en er zijn honderden hoekpunten lezen van het, lezen elk van deze van een chunk gevonden. Deze hoekpunten worden gegroepeerd in dezelfde fase en hierdoor dezelfde op verschillende onderdelen van hetzelfde bestand voor invoer werkt.
  
  * <a name="state-information"></a>Fase-informatie
    
      In een bepaalde fase worden getallen die weergegeven in het plakkaat met.
    
      ![Azure Data Lake Analytics-taak grafiek fase](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage.png)
    
    * SV1 Uitpakken: de naam van een fase, door een getal en de bewerkingsmethode genoemd.
    * 84 hoekpunten: het totale aantal hoekpunten in deze fase. De afbeelding geeft aan hoeveel stukken werk is onderverdeeld in deze fase.
    * 12.90 s/hoekpunt: de gemiddelde hoekpunt uitvoeringstijd voor deze fase. In deze afbeelding wordt berekend door SUM (elke hoekpunt uitvoeringstijd) / (totaal aantal hoekpunt). Wat betekent dat als u de hoekpunten die zijn uitgevoerd in de parallelle uitvoering toewijst kan, de hele fase is voltooid in 12,90 s. Het betekent ook dat als al het werk in deze fase wordt opeenvolgend uitgevoerd, zijn de kosten #vertices * gemiddelde tijd.
    * 850,895 geschreven rijen: het totale aantal rijen in deze fase wordt geschreven.
    * R/b: hoeveelheid gegevens lezen/schriftelijke in dit stadium in bytes.
    * Kleuren: Kleuren worden gebruikt in de fase voor het andere hoekpunt status aan te geven.
      
      * Groen geeft aan dat het hoekpunt is geslaagd.
      * Oranje geeft aan dat het hoekpunt opnieuw wordt gestart. Het geprobeerd hoekpunt is mislukt, maar wordt opnieuw geprobeerd automatisch en goed door het systeem en de algehele fase is voltooid. Als het hoekpunt opnieuw uitgevoerd, maar nog steeds mislukt, wordt de kleur rood en de hele taak is mislukt.
      * Rood geeft een mislukte, wat betekent dat een bepaalde hoekpunt had al enkele keren opnieuw door het systeem maar nog steeds mislukt. Dit scenario zorgt ervoor dat de hele taak is mislukt.
      * Blauw betekent dat een bepaalde hoekpunt wordt uitgevoerd.
      * Wit geeft aan dat het hoekpunt wacht op. Het hoekpunt kan moeten worden gepland zodra een ADLAU beschikbaar of het kan worden wacht op invoer omdat de invoergegevens mogelijk niet gereed.
      
      U vindt meer informatie voor de fase door de muiswijzer de muisaanwijzer op één status:
      
      ![Azure Data Lake Analytics grafiek fase taakdetails](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-stage-details.png)
  * Hoekpunten: Beschrijving van de gegevens van de hoekpunten, bijvoorbeeld hoeveel hoekpunten in totaal, hoeveel hoekpunten zijn voltooid, zijn ze is mislukt of nog steeds actief/wachten, enzovoort.
  * Het lezen van gegevens tussen verschillende/intra schil: bestanden en gegevens worden opgeslagen in meerdere gehele product in gedistribueerd bestandssysteem. De hier gebruikte waarde beschrijft hoeveel gegevens in dezelfde schil of kruislings schil is gelezen.
  * Totale tijd compute: de som van de uitvoeringstijd van elke vertex in de fase, kunt u overwegen deze als de tijd die nodig hebben zou als alle in de fase werkt in slechts één hoekpunt wordt uitgevoerd.
  * Gegevens en rijen geschreven leestijd: geeft aan hoeveel gegevens of rijen zijn gelezen of weggeschreven of moeten worden gelezen.
  * Hoekpunt lezen fouten: hierin wordt beschreven hoe veel hoekpunten mislukt tijdens het lezen van gegevens.
  * Verwijdert dubbele hoekpunt: als een hoekpunt te langzaam wordt uitgevoerd, het systeem meerdere hoekpunten om uit te voeren van de dezelfde stuk werk kan plannen. Wanneer een van de hoekpunten voltooid, wordt u reductant hoekpunten genegeerd. Dubbele verwijderingen hoekpunt legt het aantal hoekpunten dat in de fase als dubbele genegeerd.
  * Hoekpunt intrekkingen: het hoekpunt is geslaagd, maar u later opnieuw uit om enkele redenen. Bijvoorbeeld, als downstream hoekpunt tussenliggende invoergegevens verliest, deze wordt u gevraagd het upstream hoekpunt opnieuw uit te voeren.
  * Hoekpunt planning uitvoeringen: de totale tijd die de hoekpunten zijn gepland.
  * Gemiddelde-min/Max hoekpunt gegevens gelezen: de minimum/gemiddelde/maximum voor van elke hoekpunt gegevens lezen.
  * Duur: De kloktijd een fase duurt, moet u het laden van profiel als deze waarde wilt weergeven.
  * Taak afspelen
    
      Data Lake Analytics taken wordt uitgevoerd en archiveert de hoekpunten met informatie van de taken, zoals wanneer de hoekpunten worden gestart, gestopt, is mislukt en hoe ze zijn opnieuw gestart, enzovoort. Alle informatie wordt automatisch geregistreerd in het queryarchief en opgeslagen in het profiel van de taak. U kunt downloaden via het profiel van de taak via 'Load profiel' in de weergave van de taak en kunt u de taak afspelen na het downloaden van het profiel van de taak weergeven.
    
      Taak afspelen is een epitome visualisatie van wat er gebeurd in het cluster is. Kunt u de voortgang van de taak bekijken en visueel vaststellen afwijkingen en knelpunten in zeer korte tijd (minder dan meestal 30s).
  * Taak Heat Map weergeven 
    
      Taak Heat Map kan worden geselecteerd in de vervolgkeuzelijst weergegeven in de grafiek taak. 
    
      ![Azure Data Lake Analytics-taak grafiek heap kaartweergave](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-display.png)
    
      Laat de heatmap i/o-, tijd en doorvoer van een taak, waarmee u kunt vinden waar de meeste gevallen nodig heeft voor de taak, of of de taak is een i/o-grens-taak, enzovoort.
    
      ![Azure Data Lake Analytics-taak grafiek heap kaart voorbeeld](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-graph-heat-map-example.png)
    
    * Voortgang: De taak wordt uitgevoerd de voortgang, Zie de informatie in [fase-informatie](#stage-information).
    * Gegevens lezen/geschreven: de heatmap van de totale gegevens lezen/geschreven in elke fase.
    * COMPUTE tijd: de heatmap van SUM (elke hoekpunt uitvoeringstijd), kunt u overwegen dit hoe lang duren zou als alle werkitems in de fase met slechts 1 hoekpunt wordt uitgevoerd.
    * Gemiddelde uitvoeringstijd per knooppunt: de heatmap van SUM (elke hoekpunt uitvoeringstijd) / (hoekpunt Number). Wat betekent dat als u de hoekpunten die zijn uitgevoerd in de parallelle uitvoering toewijst kan, de hele fase wordt uitgevoerd in deze periode.
    * I/o-doorvoer: de heatmap van i/o-doorvoer van elke fase, kunt u controleren of de taak een i/o-afhankelijke taak via deze is.
* Bewerkingen voor metagegevens
  
    U kunt sommige metagegevensbewerkingen uitvoeren in uw U-SQL-script, zoals een database te maken, verwijderen van een tabel, enzovoort. Deze bewerkingen worden weergegeven in de metagegevens na de compilatie. U mag asserties vinden, entiteiten maken, entiteiten hier neerzetten.
  
    ![Bewerkingen van de metagegevens van Azure Data Lake Analytics-taak weergeven](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-metadata-operations.png)
* Geschiedenis van status
  
    De geschiedenis van de status wordt ook weergegeven in Taakoverzicht, maar u kunt hier meer informatie krijgen. U vindt meer informatie, zoals wanneer de taak is voorbereid, in de wachtrij, gestarte uitvoering is beëindigd. U kunt ook hoe vaak de taak is gecompileerd vinden (de CcsAttempts: 1), wanneer de taak verzonden naar het cluster daadwerkelijk (het detailniveau: tijdens het verzenden van taak voor het cluster), enzovoort.
  
    ![Geschiedenis van de Azure Data Lake Analytics-taak weergeven](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-state-history.png)
* Diagnostiek
  
    Het hulpprogramma diagnoses automatisch uitvoeren van taak. U ontvangt waarschuwingen wanneer er fouten of prestatieproblemen in uw taken. Houd er rekening mee dat u het profiel moet voor volledige informatie hier downloaden. 
  
    ![Diagnostische gegevens van Azure Data Lake Analytics-taak weergeven](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-view-diagnostics.png)
  
  * Waarschuwingen: Een waarschuwing weergegeven hier met waarschuwing van compiler. U kunt klikken op 'x sporen' koppeling om meer informatie wanneer de waarschuwing wordt weergegeven.
  * Hoekpunt uitvoeren te lang: als een hoekpunt wordt uitgevoerd buiten de tijd (spreek 5 uur), problemen hier worden gevonden.
  * Resourcegebruik: als u meer of is niet voldoende parallelle uitvoering toegewezen dan nodig hebt, problemen hier worden gevonden. Ook kunt u het gebruik van bronnen voor meer informatie en wat-als-scenario's te zoeken naar een betere brontoewijzing uitvoeren (Zie voor meer informatie in deze handleiding).
  * Geheugencontrole: als een hoekpunt gebruikmaakt van meer dan 5 GB geheugen, problemen hier worden gevonden. Uitvoeren van taak kan ophalen beëindigd door een systeem als gebruikmaakt van meer geheugen dan de systeembeperking van het.

## <a name="job-detail"></a>Taakdetails
Details van de taak wordt de gedetailleerde informatie van de taak, waaronder Script, bronnen en Vertex Execution View.

![Azure Data Lake Analytics-job details](./media/data-lake-analytics-data-lake-tools-view-jobs/data-lake-tools-job-details.png)

* Script
  
    De U-SQL-script van de taak wordt opgeslagen in het queryarchief. U kunt het oorspronkelijke U-SQL-script weergeven en opnieuw te verzenden indien nodig.
* Resources
  
    U kunt de taak compilatie uitvoer opgeslagen in het queryarchief via bronnen vinden. U kunt bijvoorbeeld 'algebra.xml' die wordt gebruikt voor het weergeven van de Taakgrafiek, de assembly's die u hebt geregistreerd, enz. hier vinden.
* Hoekpunt uitvoering weergeven
  
    Deze hoekpunten uitvoering worden details weergegeven. Het profiel van de taak archiveert elke uitvoeringslogboek hoekpunt zoals totale gegevens lezen/geschreven, runtime, status, enzovoort. Via deze weergave kunt u meer informatie over hoe een taak duurde krijgen. Zie voor meer informatie [gebruik van de Vertex Execution View in Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).

## <a name="next-steps"></a>Volgende stappen
* Zie [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md) (Diagnostische logboeken openen voor Azure Data Lake Analytics) voor logboekregistratie van diagnostische informatie.
* Zie [Websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md) voor een complexere query.
* Hoekpunt uitvoering als weergave wilt gebruiken, Zie [de Vertex Execution View in Data Lake Tools voor Visual Studio gebruiken](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)

