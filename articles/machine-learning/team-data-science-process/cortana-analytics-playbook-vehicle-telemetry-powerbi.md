---
title: Power BI-dashboard voor en rijgewoonten - Azure | Microsoft Docs
description: De mogelijkheden van Cortana Intelligence gebruiken om te krijgen van realtime en voorspellende inzichten op en winkelgedrag.
services: machine-learning
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.author: deguhath
ms.openlocfilehash: e6601093577eb9e3dfba4ed27e1e0510cad17de7
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421046"
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a>Vehicle Telemetry Analytics-oplossing sjabloon Power BI-dashboard installatie-instructies
Dit bevat menukoppelingen naar de hoofdstukken in deze playbook: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

De Vehicle Telemetry Analytics-oplossing demonstreert hoe autodealers, autofabrikanten en verzekeringsmaatschappijen kunnen worden gebruikt u de mogelijkheden van Cortana Intelligence. Kan de aanvaller realtime en voorspellende inzichten in voertuigstatus en gewoonten voor het verbeteren van klantervaring, onderzoek en ontwikkeling te stimuleren en marketingcampagnes. Deze stapsgewijze instructies laten zien hoe het configureren van de Power BI-rapporten en dashboard nadat u de oplossing in uw abonnement implementeert. 

## <a name="prerequisites"></a>Vereisten
* Implementeer de [Voertuigtelemetrieanalyse](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) oplossing. 
* [Power BI Desktop installeren](http://www.microsoft.com/download/details.aspx?id=45331).
* Verkrijgen van een [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/). Als u geen Azure-abonnement hebt, aan de slag met het gratis Azure-abonnement.
* Open Power BI-account.

## <a name="cortana-intelligence-suite-components"></a>Cortana Intelligence suite-onderdelen
Als onderdeel van de oplossingssjabloon Vehicle Telemetry Analytics, zijn de volgende Cortana Intelligence-services worden geïmplementeerd in uw abonnement:

* **Azure Event Hubs** neemt miljoenen voertuigtelemetriegebeurtenissen in Azure.
* **Azure Stream Analytics** biedt in realtime inzichten in voertuigstatus en bewaart die gegevens in langdurige opslag voor rijkere batchanalyse.
* **Azure Machine Learning** afwijkingen in realtime gedetecteerd en gebruikt voor batchverwerking voorspellende statistieken aanleveren.
* **Azure HDInsight** gegevens op schaal worden getransformeerd.
* **Azure Data Factory** indeling, planning, resourcebeheer en controle van de batchverwerkingspijplijn worden verwerkt.

**Power BI** biedt deze oplossing een uitgebreid dashboard toegevoegd voor gegevens en voorspellende analyses visualisaties. 

De oplossing maakt gebruik van twee verschillende gegevensbronnen:

* Gesimuleerde vehicle signalen en diagnostische gegevens
* Vehicle catalogus

Er is een vehicle telematicssimulator opgenomen als onderdeel van deze oplossing. Het verzendt de diagnostische gegevens en signalen die met de status van het voertuig sparen patronen op een bepaald tijdstip overeenkomen. 

De catalogus vehicle is een referentiegegevensset die is toegewezen VINs aan modellen.

## <a name="power-bi-dashboard-preparation"></a>Voorbereiding van Power BI-dashboard
### <a name="set-up-the-power-bi-real-time-dashboard"></a>Instellen van de Power BI realtime-dashboard

#### <a name="start-the-real-time-dashboard-application"></a>Start de dashboardtoepassing realtime
Nadat de implementatie is voltooid, wordt de handmatige bewerking instructies.

1. De dashboardtoepassing voor realtime RealtimeDashboardApp.zip gedownload en pak het uit.

1.  Open de app-configuratiebestand RealtimeDashboardApp.exe.config in de uitgepakte map. Vervang appSettings voor Event Hubs, Azure-blobopslag en Azure Machine Learning-service-verbindingen met de waarden in de instructies voor handmatige bewerking. Sla uw wijzigingen op.

1. De toepassing RealtimeDashboardApp.exe uitvoeren. Voer uw geldige Power BI-referenties in het venster aanmelden. 

   ![Power BI-aanmelden-venster](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
1. Selecteer **accepteren**. De app wordt gestart om uit te voeren.

   ![Machtigingen voor Power BI-dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

1. Aanmelden bij de website van Power BI en maak een realtime dashboard.

U bent nu klaar om te configureren van de Power BI-dashboard.  

### <a name="configure-power-bi-reports"></a>Power BI-rapporten configureren
De realtime rapporten en het dashboard moet u ongeveer 30 tot 45 minuten duren om te voltooien. 

1. Blader naar de [Power BI](http://powerbi.com) webpagina en meld u aan.

    ![Power BI-aanmeldingspagina](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

1. Een nieuwe gegevensset wordt in Power BI gegenereerd. Selecteer de **ConnectedCarsRealtime** gegevensset.

    ![ConnectedCarsRealtime-gegevensset](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

1. Druk op Ctrl + S om de leeg rapport hebt opgeslagen.

    ![Leeg rapport](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

1. Voer de naam van het rapport **Vehicle Telemetry Analytics Real-time - rapporten**.

    ![Rapportnaam](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Realtime rapporten
Er zijn drie realtime rapporten in deze oplossing:

* Voertuigen in bewerking
* Voertuigen onderhoud vereisen
* Vehicle Health statistieken

U kunt configureren dat alle drie de rapporten, of kunt u stoppen na elke fase. Vervolgens kunt u doorgaan met de volgende sectie over het configureren van de batch-rapporten. U wordt aangeraden dat u alle drie rapporten voor het visualiseren van de volledige inzichten van de realtime pad van de oplossing maken.  

### <a name="vehicles-in-operation-report"></a>Voertuigen in bewerking rapport
1. Dubbelklik op **pagina 1**, en wijzig de naam **voertuigen in bewerking**.

    ![Voertuigen in bewerking](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

1. Op de **velden** tabblad **vin**. Op de **visualisaties** tabblad de **kaart** visualisatie.  

    De **kaart** visualisatie is gemaakt, zoals wordt weergegeven in de volgende afbeelding:

    ![Selecteer vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

1. Selecteer de lege ruimte toe te voegen een nieuwe visualisatie.  

1. Op de **velden** tabblad **plaats** en **vin**. Op de **visualisaties** tabblad de **kaart** visualisatie. Sleep **vin** naar de **waarden** gebied. Sleep **plaats** naar de **legenda** gebied. 

    ![Kaartvisualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)

1. Op de **visualisaties** tabblad de **indeling** sectie. Selecteer **titel**, en wijzig **tekst** naar **voertuigen in bewerking per stad**.

    ![Voertuigen in bewerking per stad](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

    De laatste visualisatie ziet eruit als in het volgende voorbeeld:

    ![Laatste visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

1. Selecteer de lege ruimte toe te voegen een nieuwe visualisatie.  

1. Op de **velden** tabblad **plaats** en **vin**. Op de **visualisaties** tabblad de **gegroepeerd kolomdiagram** visualisatie. Sleep **plaats** naar de **as** gebied. Sleep **vin** naar de **waarde** gebied.

1. Sorteer de kaart op **aantal vin**.

    ![Aantal vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

1. De grafiek wijzigen **titel** naar **voertuigen in bewerking per stad**. 

1. Selecteer de **indeling** uit en selecteer vervolgens **Gegevenskleuren**. Wijziging **Show All** naar **op**.

    ![Gegevenskleuren](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

1. De kleur van een afzonderlijke plaats wijzigen door het symbool kleur te selecteren.

    ![Kleur wijzigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

1. Selecteer de lege ruimte toe te voegen een nieuwe visualisatie.  

1. Op de **visualisaties** tabblad de **gegroepeerd kolomdiagram** visualisatie. Op de **velden** tabblad, sleept u **plaats** naar de **as** gebied. Sleep **Model** naar de **legenda** gebied. Sleep **vin** naar de **waarde** gebied.

    ![Gegroepeerd kolomdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)

    De grafiek ziet eruit zoals in de volgende afbeelding:

    ![Rendering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)

1. Alle visualisaties herschikken zodat de pagina ziet er als volgt uit:

    ![Dashboard met visualisaties](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

U het rapport 'Voertuigen in bewerking' geconfigureerd. U kunt de volgende realtime rapport maken, of u kunt hier stoppen en configureren van het dashboard. 

### <a name="vehicles-requiring-maintenance-report"></a>Rapport van voertuigen onderhoud vereisen

1. Selecteer ![toevoegen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) om toe te voegen een nieuw rapport. Wijzig de naam **voertuigen waarvoor onderhoud**.

    ![Voertuigen onderhoud vereisen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

1. Op de **velden** tabblad **vin**. Op de **visualisaties** tabblad de **kaart** visualisatie.

    ![Vin kaartvisualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

    De gegevensset een veld met de naam bevat **MaintenanceLabel**. Dit veld kan hebben een waarde van "0" of "1". De waarde is ingesteld door het machine learning-model dat ingericht als onderdeel van de oplossing. Het geïntegreerd met de realtime-pad. De waarde '1' geeft aan dat een vehicle onderhoud vereist. 

1. Om toe te voegen een **niveau paginafilter** om gegevens voor de voertuigen waarvoor onderhoud weer te geven: 

   a. Sleep de **MaintenanceLabel** veld **Filters op niveau van pagina**.
  
      ![Filters op paginaniveau](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)

    b. Aan de onderkant van **pagina niveau Filters MaintenanceLabel**, selecteer **standaardfilters toepassen**.

      ![Standaardfilters toepassen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png) 

    c. Stel de filterwaarde op **1**.

      ![Filterwaarde](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  

1. Selecteer de lege ruimte toe te voegen een nieuwe visualisatie.  

1. Op de **visualisaties** tabblad de **gegroepeerd kolomdiagram** visualisatie. 

    ![Vin-kaart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)

    ![Gegroepeerd kolomdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

1. Op de **velden** tabblad, sleept u **Model** naar de **as** gebied. Sleep **vin** naar de **waarde** gebied. Sorteer de visualisatie door **aantal vin**. De grafiek wijzigen **titel** naar **voertuigen waarvoor onderhoud model**. 

1. Op de **velden** ![velden-image](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) sectie van de **visualisaties** tabblad, sleept u **vin** naar **kleurverzadiging**.

    ![Kleurverzadiging](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

1. Op de **indeling** sectie, wijzigt u **Gegevenskleuren** in de visualisatie: 

    a. Wijzig de **Minimum** kleur naar **F2C812**.

    b. Wijzig de **maximale** kleur naar **FF6300**.

    ![Nieuwe gegevenskleuren](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)

    De nieuwe visualisatie kleuren er uitzien als in het volgende voorbeeld:

    ![Nieuwe visualisatie kleuren](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

1. Selecteer de lege ruimte toe te voegen een nieuwe visualisatie.  

1. Op de **visualisaties** tabblad **gegroepeerd kolomdiagram**. Sleep **vin** naar de **waarde** gebied. Sleep **plaats** naar de **as** gebied. Sorteer de kaart op **aantal vin**. De grafiek wijzigen **titel** naar **voertuigen waarvoor onderhoud per stad**.

    ![Voertuigen waarvoor onderhoud per stad](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

1. Selecteer de lege ruimte toe te voegen een nieuwe visualisatie.  

1. Op de **visualisaties** tabblad de **kaart met meerdere rijen** visualisatie. Sleep **Model** en **vin** naar de **velden** gebied.

    ![Kaart met meerdere rijen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

1. Alle visualisaties herschikken zodat het uiteindelijke rapport ziet er als volgt uit: 

    ![Andere volgorde uiteindelijke rapport](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

U het real-time "Voertuigen waarvoor onderhoud"-rapport geconfigureerd. U kunt de volgende realtime rapport maken, of u kunt hier stoppen en configureren van het dashboard. 

### <a name="vehicle-health-statistics-report"></a>Rapport van de gezondheid van statistieken voor voertuigen

1. Selecteer ![toevoegen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) om toe te voegen een nieuw rapport. Wijzig de naam **voertuigen Health statistieken**. 

1. Op de **visualisaties** tabblad de **meter** visualisatie. Sleep **snelheid** naar de **waarde**, **minimumwaarde**, en **maximumwaarde** gebieden.

   ![Voertuigen Health statistieken](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

1. In de **waarde** gebied wijzigen van het standaard aggregatietype van **snelheid** naar **gemiddelde**.

1. In de **minimumwaarde** gebied wijzigen van het standaard aggregatietype van **snelheid** naar **Minimum**.

1. In de **maximumwaarde** gebied wijzigen van het standaard aggregatietype van **snelheid** naar **maximale**.

   ![Snelheid van waarden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

1. Wijzig de naam van de **meter titel** naar **gemiddelde snelheid**.

   ![Meter](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

1. Selecteer de lege ruimte toe te voegen een nieuwe visualisatie.  

    Op deze manier toevoegen een **meter** voor **gemiddelde engine olie**, **gemiddelde brandstof**, en **gemiddelde temperatuur engine**.  

1. Wijzigen van het standaard aggregatietype van velden in elke meter zoals u dit hebt gedaan in de vorige stappen in de **gemiddelde snelheid** te meten.

    ![Aanvullende meters](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

1. Selecteer de lege ruimte toe te voegen een nieuwe visualisatie.

1. Op de **visualisaties** tabblad de **lijndiagram en gegroepeerd kolomdiagram** visualisatie. Sleep **plaats** naar **gedeelde as**. Sleep **tirepressure**, **engineoil**, en **snelheid** naar de **kolomwaarden** gebied. Wijzigen van hun aggregatietype op **gemiddelde**. 

1. Sleep **engineTemperature** naar de **Lijnwaarden** gebied. Het aggregatietype wijzigen **gemiddelde**. 

    ![Kolom en regelwaarden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

1. De grafiek wijzigen **titel** naar **gemiddelde snelheid, band druk te verlichten, engine olie en engine-temperatuur**.  

    ![Lijndiagram en gegroepeerd kolomdiagram titel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

1. Selecteer de lege ruimte toe te voegen een nieuwe visualisatie.

1. Op de **visualisaties** tabblad de **Treemap** visualisatie. Sleep **Model** naar de **groep** gebied. Sleep **MaintenanceProbability** naar de **waarden** gebied.

1. De grafiek wijzigen **titel** naar **Vehicle modellen waarvoor onderhoud**.

    ![Titel van de Treemap](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

1. Selecteer de lege ruimte toe te voegen een nieuwe visualisatie.

1. Op de **visualisaties** tabblad de **100% gestapeld staafdiagram** visualisatie. Sleep **plaats** naar de **as** gebied. Sleep **MaintenanceProbability** en **RecallProbability** naar de **waarde** gebied.

    ![Gebieden van as en de waarde](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

1. Op de **indeling** sectie, selecteer **Gegevenskleuren**. Stel de **MaintenanceProbability** kleur aan de waarde **F2C80F**.

1. De grafiek wijzigen **titel** naar **kans van Vehicle onderhoud & intrekken per stad**.

    ![Titel van 100% gestapeld staafdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

1. Selecteer de lege ruimte toe te voegen een nieuwe visualisatie.

1. Op de **visualisaties** tabblad de **vlakdiagram** visualisatie. Sleep **Model** naar de **as** gebied. Sleep **engineOil**, **tirepressure**, **snelheid**, en **MaintenanceProbability** naar de **waarden** gebied. Wijzigen van hun aggregatietype op **gemiddelde**. 

    ![Aggregatietype](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

1. De grafiek wijzigen **titel** naar **gemiddelde engine olie, band druk te verlichten, snelheid en onderhoud kans door model**.

    ![Grafiektitel gebied](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

1. Selecteer de lege ruimte toe te voegen een nieuwe visualisatie.

1. Op de **visualisaties** tabblad de **spreidingsdiagrammen** visualisatie. Sleep **Model** naar de **Details** en **legenda** gebieden. Sleep **brandstof** naar de **x-as** gebied. Wijzig de aggregatie in **gemiddelde**. Sleep **engineTemperature** naar de **y-as** gebied. Wijzig de aggregatie in **gemiddelde**. Sleep **vin** naar de **grootte** gebied.

    ![Details, legenda, as en de grootte gebieden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

1. De grafiek wijzigen **titel** naar **gemiddelde van brandstof, het gemiddelde van engineTemperature en aantal vin door Model**.

    ![Grafiektitel spreidingsdiagrammen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

    Het uiteindelijke rapport ziet er als volgt uit:

    ![Laatste rapport](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>Visualisaties vastmaken van de rapporten naar het realtime dashboard
1. Een leeg dashboard maken door het selecteren van het plusteken naast **Dashboards**. Voer de naam **Vehicle Telemetry Analytics-Dashboard**.

    ![Dashboard plus symbool](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

1. De visualisaties uit de vorige rapporten aan het dashboard vastmaken. 

    ![Symbool van dashboard vastmaken](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

    Wanneer alle drie rapporten zijn vastgemaakt aan het dashboard, ziet deze er als het volgende voorbeeld. Als u alle rapporten hebt gemaakt, kan uw dashboard anders uitzien. 

    ![Dashboard met rapporten](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

U het realtime dashboard gemaakt. Als u doorgaat CarEventGenerator.exe en RealtimeDashboardApp.exe uit te voeren, ziet u live updates op het dashboard. De volgende stappen ongeveer 10 tot 15 minuten duren.

## <a name="set-up-the-power-bi-batch-processing-dashboard"></a>Instellen van de Power BI-dashboard verwerking van batch
> [!NOTE]
> Het duurt ongeveer twee uur (van de voltooiing van de implementatie) voor de pijplijn uitgevoerd en het verwerken van een jaar lang van gegenereerde gegevens voor batchverwerking van end-to-end. Wacht totdat de verwerking worden voltooid voordat u doorgaat met de volgende stappen uit:
> 
> 

### <a name="download-the-power-bi-designer-file"></a>De Power BI designer-bestand downloaden

1. Een vooraf geconfigureerde Power BI designer-bestand is opgenomen als onderdeel van de implementatie-instructies voor handmatige bewerking. Zoek naar "2. Instellen van de Power BI-dashboard voor batch verwerken."

1. Download de Power BI-sjabloon voor batchverwerking dashboard hier met de naam **ConnectedCarsPbiReport.pbix**.

1. Lokaal opslaan.

### <a name="configure-power-bi-reports"></a>Power BI-rapporten configureren

1. Open het bestand designer **ConnectedCarsPbiReport.pbix** met behulp van de Power BI Desktop. Als u er nog geen hebt, installeert u de Power BI Desktop uit de [Power BI Desktop installatie](http://www.microsoft.com/download/details.aspx?id=45331) website.

1. Selecteer **query's bewerken**.

    ![Query's bewerken](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

1. Dubbelklik op **bron**.

    ![Bron](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

1. De server-verbindingsreeks bijwerken met de Azure SQL-server die is ingericht als onderdeel van de implementatie. Zoeken in de instructies voor handmatige bewerking onder Azure SQL-database:

    * Server: somethingsrv.database.windows.net
    * Database: connectedcar
    * Gebruikersnaam: gebruikersnaam
    * Wachtwoord: U kunt het wachtwoord van uw SQL Server beheren vanuit Azure portal.

1. Laat **Database** als **connectedcar**.

    ![Database](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

1. Selecteer **OK**.

1. De **Windows-referentie** tabblad is standaard geselecteerd. Wijzig dit in **referenties van de Database** door het selecteren van de **Database** tabblad aan de rechterkant.

1. Voer de **gebruikersnaam** en **wachtwoord** van uw Azure SQL-database die is opgegeven tijdens de installatie van de implementatie.

    ![Databasereferenties](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

1. Selecteer **Verbinden**.

1. Herhaal de vorige stappen voor elk van de drie resterende query's aanwezig zijn in het rechter deelvenster. Werk vervolgens de details van gegevensbron verbinding.

1. Selecteer **sluiten en laden**. Gegevenssets voor Power BI Desktop-bestand zijn verbonden met SQL database-tabellen.

1. Selecteer **sluiten** te sluiten van de Power BI Desktop-bestand.

    ![Sluiten](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

1. Selecteer **opslaan** de wijzigingen op te slaan. 

U hebt nu alle rapporten die overeenkomen met het pad in de oplossing voor batchverwerking geconfigureerd. 

## <a name="upload-to-powerbicom"></a>Uploaden naar powerbi.com
1. Ga naar de [webportal van Power BI](http://powerbi.com), en meld u aan.

1. Selecteer **Gegevens ophalen**.

1. Het Power BI Desktop-bestand uploaden. Selecteer **gegevens ophalen** > **bestanden ophalen** > **lokaal bestand**.

1. Ga naar **ConnectedCarsPbiReport.pbix**.

1. Nadat het bestand is geüpload, gaat u terug naar uw Power BI-werkruimte. Een gegevensset, een rapport en een leeg dashboard worden voor u gemaakt.  

1. Grafieken vastmaken aan een nieuw dashboard met de naam **Vehicle Telemetry Analytics-Dashboard** in Power BI. Selecteer het leeg dashboard dat eerder is gemaakt, en ga vervolgens naar de **rapporten** sectie. Selecteer het recent geüploade rapport.  

    ![Nieuwe Power BI-dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

    Het rapport bevat zes pagina's:

    Pagina 1: Vehicle dichtheid  
    Pagina 2: Realtime voertuigstatus  
    Pagina 3: Gestuurde doelbewust voertuigen   
    Pagina 4: Ingetrokken voertuigen  
    Pagina 5: Brandstof efficiënt gestuurde voertuigen  
    Pagina 6: Contoso Motors-logo  

    ![Power BI-rapport met zes pagina 's](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

1. Van **pagina 3**, vastmaken van de volgende inhoud:  

    a. **Aantal vin**  

   ![Pagina 3 aantal vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png)

    b. **Doelbewust voertuigen aangestuurd door-model, watervalgrafiek** 

   ![Pagina 3 grafiek 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

1. Van **pagina 5**, vastmaken van de volgende inhoud: 

    a. **Aantal vin**

   ![Pagina 5 grafiek 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)

    b. **Fuel-Efficient voertuigen door model: gegroepeerd kolomdiagram**

   ![Pagina 5 grafiek 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

1. Van **pagina 4**, vastmaken van de volgende inhoud:  

    a. **Aantal vin** 

   ![Pagina 4 Grafiek 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 

    b. **Ingetrokken voertuigen per stad, model: Treemap**

   ![Pagina 4 grafiek 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

1. Van **pagina 6**, vastmaken van de volgende inhoud:  

    * **Contoso Motors-logo**

    ![Contoso Motors-logo](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

### <a name="organize-the-dashboard"></a>Het dashboard delen  

1. Ga naar het dashboard.

1. Beweeg de muisaanwijzer over elke grafiek. Wijzig de naam van elke grafiek op basis van de naamgeving opgegeven in het volgende voltooid dashboard-voorbeeld:

   ![Dashboard-organisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png) 
   
1. Verplaats de grafieken om te zoeken, zoals in het volgende voorbeeld van dashboard:

    ![Andere volgorde dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)

1. Nadat u alle rapporten die worden vermeld in dit document hebt gemaakt, is de laatste voltooide dashboard ziet eruit zoals in het volgende voorbeeld: 

   ![Laatste dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

U hebt gemaakt de rapporten en het dashboard te krijgen realtime, voorspellende en batch inzichten en rijgewoonten.  
