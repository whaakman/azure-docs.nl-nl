---
title: Power BI-dashboard van de status van de drager en groot gewoonten - Azure | Microsoft Docs
description: De mogelijkheden van Cortana Intelligence gebruiken om inzicht in real-time en voorspeld op vehicle status en verkeer te krijgen gewoonten.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: bradsev
ms.openlocfilehash: 626987ec0648f9e770499b4a48bc4ca2d175d2b4
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a>Vehicle telemetrie Analytics-oplossing sjabloon Power BI-dashboard installatie-instructies
Dit menu is gekoppeld aan de hoofdstukken in deze playbook: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

De drager telemetrie Analytics-oplossing gepresenteerd hoe auto dealerbedrijven, auto fabrikanten en ondernemingen de mogelijkheden van Cortana Intelligence kunnen gebruiken. Ze krijgen real-time en voorspellende insights vehicle gezondheid en gewoonten klantervaring, onderzoek en ontwikkeling te verbeteren, en marketingcampagnes. Deze stapsgewijze instructies laten zien hoe u de Power BI-rapporten en het dashboard configureren kunt nadat u de oplossing in uw abonnement implementeert. 

## <a name="prerequisites"></a>Vereisten
* Implementeer de [Vehicle telemetrie Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) oplossing. 
* [Installeer Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331).
* Verkrijgen van een [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/). Als u geen Azure-abonnement hebt, aan de slag met de gratis Azure-abonnement.
* Open een Power BI-account.

## <a name="cortana-intelligence-suite-components"></a>Cortana Intelligence suite-onderdelen
Als onderdeel van de sjabloon Vehicle telemetrie Analytics-oplossing, worden de volgende Cortana Intelligence-services geïmplementeerd in uw abonnement:

* **Azure Event Hubs** miljoenen vehicle telemetrische gebeurtenissen opgenomen in Azure.
* **Azure Stream Analytics** biedt realtime-inzichten op vehicle health en dat de gegevens zich blijft voordoen in langdurige opslag voor uitgebreidere batch analyses.
* **Azure Machine Learning** afwijkingen in realtime detecteert en batchverwerking wordt gebruikt om voorspellende insights.
* **Azure HDInsight** gegevens op schaal worden omgezet.
* **Azure Data Factory** verwerkt orchestration, planning, bronbeheer en bewaking van de pipeline batch verwerkt.

**Power BI** biedt deze oplossing een uitgebreide dashboard voor realtime-gegevens en visualisaties predictive analytics. 

De oplossing maakt gebruik van twee verschillende gegevensbronnen:

* Gesimuleerde vehicle signalen en diagnostische gegevens
* Vehicle catalogus

Er is een vehicle telematica simulator opgenomen als onderdeel van deze oplossing. Deze verzendt diagnostische gegevens en signalen die met de status van de drager en groot patronen op een bepaald tijdstip overeenkomen. 

De catalogus drager is een verwijzing van een gegevensset die VINs wordt toegewezen aan modellen.

## <a name="power-bi-dashboard-preparation"></a>Voorbereiding van Power BI-dashboard
### <a name="set-up-the-power-bi-real-time-dashboard"></a>Instellen van de realtime Power BI-dashboard

#### <a name="start-the-real-time-dashboard-application"></a>Start de dashboardtoepassing realtime
Nadat de implementatie is voltooid, wordt de handmatige bewerking instructies.

1. De dashboardtoepassing realtime RealtimeDashboardApp.zip downloaden en uitpakken van het.

2.  Open de app-configuratiebestand RealtimeDashboardApp.exe.config in de uitgepakte map. Vervang appSettings voor Event Hubs, Azure Blob storage en Azure Machine Learning-service-verbindingen met de waarden in de handmatige bewerking-instructies. Sla uw wijzigingen op.

3. Voer de toepassing RealtimeDashboardApp.exe. Geef uw geldige Power BI-referenties in het venster aanmelden. 

   ![Power BI-in-venster](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
4. Selecteer **accepteren**. De app wordt gestart.

   ![Power BI-dashboard machtigingen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

5. Aanmelden bij de Power BI-website en een realtime dashboard maken.

U kunt nu klaar om te configureren van de Power BI-dashboard.  

### <a name="configure-power-bi-reports"></a>Power BI-rapporten configureren
De realtime-rapporten en het dashboard rekening houden met ongeveer 30 tot 45 minuten te voltooien. 

1. Blader naar de [Power BI](http://powerbi.com) webpagina, en meld u aan.

    ![Power BI-aanmeldingspagina](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

2. Een nieuwe gegevensset wordt gegenereerd in Power BI. Selecteer de **ConnectedCarsRealtime** gegevensset.

    ![ConnectedCarsRealtime-gegevensset](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

3. Druk op Ctrl + S voor het opslaan van het rapport leeg.

    ![Leeg rapport](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

4. Voer de naam van het rapport **Vehicle telemetrie Analytics realtime - rapporten**.

    ![Rapportnaam](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Realtime-rapporten
Er zijn drie realtime rapporten in deze oplossing:

* Voertuigen in bewerking
* Voertuigen onderhoud vereisen
* Vehicle Health statistieken

U kunt alle drie de realtime-rapporten configureren of kunt u na een stadium stoppen. Vervolgens kunt u doorgaan met de volgende sectie over het configureren van de batch-rapporten. U wordt aangeraden dat u alle drie rapporten voor het visualiseren van de volledige inzichten van het real-time pad van de oplossing maakt.  

### <a name="vehicles-in-operation-report"></a>Voertuigen in bewerking rapport
1. Dubbelklik op **pagina 1**, en de naam **voertuigen in bewerking**.

    ![Voertuigen in bewerking](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

2. Op de **velden** tabblad **vin**. Op de **visualisaties** tabblad de **kaart** visualisatie.  

    De **kaart** visualisatie wordt gemaakt, zoals wordt weergegeven in de volgende afbeelding:

    ![Selecteer vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

3. Selecteer een leeg gebied toevoegen van een nieuwe visualisatie.  

4. Op de **velden** tabblad **stad** en **vin**. Op de **visualisaties** tabblad de **kaart** visualisatie. Sleep **vin** naar de **waarden** gebied. Sleep **stad** naar de **legenda** gebied. 

    ![Visualisatie van de kaart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)

5. Op de **visualisaties** tabblad de **indeling** sectie. Selecteer **titel**, en wijzig **tekst** naar **voertuigen in bewerking per plaats**.

    ![Voertuigen in bewerking per plaats](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

    De laatste visualisatie lijkt op het volgende voorbeeld:

    ![Laatste visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

6. Selecteer een leeg gebied toevoegen van een nieuwe visualisatie.  

7. Op de **velden** tabblad **stad** en **vin**. Op de **visualisaties** tabblad de **kolomdiagram geclusterde** visualisatie. Sleep **stad** naar de **as** gebied. Sleep **vin** naar de **waarde** gebied.

8. Sorteren van de grafiek door **aantal vin**.

    ![Telling van vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

9. Wijzigen van de grafiek **titel** naar **voertuigen in bewerking per plaats**. 

10. Selecteer de **indeling** sectie en selecteer vervolgens **gegevens kleuren**. Wijziging **Alles weergeven** naar **op**.

    ![Gegevens kleuren](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

11. De kleur van een afzonderlijke plaats wijzigen door het symbool kleur te selecteren.

    ![Kleur wijzigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

12. Selecteer een leeg gebied toevoegen van een nieuwe visualisatie.  

13. Op de **visualisaties** tabblad de **kolomdiagram geclusterde** visualisatie. Op de **velden** tabblad, sleept u **stad** naar de **as** gebied. Sleep **Model** naar de **legenda** gebied. Sleep **vin** naar de **waarde** gebied.

    ![Gegroepeerd kolomdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)

    De grafiek ziet er in de volgende afbeelding:

    ![Rendering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)

14. Alle visualisaties rangschikken zodat de pagina op het volgende voorbeeld lijkt:

    ![Dashboard met visualisaties](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

Het real-time rapport 'Voertuigen in bewerking' geconfigureerd. U kunt de volgende realtime rapport maken of u kunt hier stoppen en configureren van het dashboard. 

### <a name="vehicles-requiring-maintenance-report"></a>Voertuigen onderhoud vereisen rapport

1. Selecteer ![toevoegen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) een nieuw rapport toe te voegen. Wijzig de naam **voertuigen vereisen onderhoud**.

    ![Voertuigen onderhoud vereisen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

2. Op de **velden** tabblad **vin**. Op de **visualisaties** tabblad de **kaart** visualisatie.

    ![De visualisatie vin-kaart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

    De gegevensset bevat een veld met de naam **MaintenanceLabel**. Dit veld kunt hebben de waarde '0' of '1'. De waarde is ingesteld door het machine learning-model dat als onderdeel van de oplossing ingericht. Het geïntegreerd met het real-time pad. De waarde '1' geeft aan dat een medium onderhoud vereist. 

3. Om toe te voegen een **niveau paginafilter** om weer te geven gegevens voor de voertuigen die onderhoud vereisen: 

   a. Sleep de **MaintenanceLabel** veld **niveau paginafilters**.
  
      ![Filters paginaniveau](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)

    b. Aan de onderkant van **pagina niveau Filters MaintenanceLabel**, selecteer **Basic filteren**.

      ![Basic filteren](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png) 

    c. Stel de filterwaarde op **1**.

      ![Filterwaarde](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  

4. Selecteer een leeg gebied toevoegen van een nieuwe visualisatie.  

5. Op de **visualisaties** tabblad de **kolomdiagram geclusterde** visualisatie. 

    ![Chassisnummer-kaart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)

    ![Gegroepeerd kolomdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

6. Op de **velden** tabblad, sleept u **Model** naar de **as** gebied. Sleep **vin** naar de **waarde** gebied. Sorteer de visualisatie door **aantal vin**. Wijzigen van de grafiek **titel** naar **voertuigen onderhoud vereisen door model**. 

7. Op de **velden** ![velden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) sectie van de **visualisaties** tabblad, sleept u **vin** naar **kleurverzadiging**.

    ![Kleur van de CPU](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

8. Op de **indeling** sectie, wijzigt u **gegevens kleuren** in de visualisatie: 

    a. Wijzig de **Minimum** kleur naar **F2C812**.

    b. Wijzig de **maximale** kleur naar **FF6300**.

    ![Nieuwe gegevens kleuren](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)

    De nieuwe visualisatie kleuren eruitzien als in het volgende voorbeeld:

    ![Nieuwe visualisatie kleuren](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

9. Selecteer een leeg gebied toevoegen van een nieuwe visualisatie.  

10. Op de **visualisaties** tabblad **kolomdiagram geclusterde**. Sleep **vin** naar de **waarde** gebied. Sleep **stad** naar de **as** gebied. Sorteren van de grafiek door **aantal vin**. Wijzigen van de grafiek **titel** naar **voertuigen vereisen onderhoud per plaats**.

    ![Voertuigen onderhoud per plaats vereisen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

11. Selecteer een leeg gebied toevoegen van een nieuwe visualisatie.  

12. Op de **visualisaties** tabblad de **met meerdere rijen kaart** visualisatie. Sleep **Model** en **vin** naar de **velden** gebied.

    ![Meerdere rij-kaart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

13. Alle visualisaties rangschikken zodat het laatste rapport op het volgende voorbeeld lijkt: 

    ![Andere volgorde laatste rapport](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Het real-time rapport 'Voertuigen vereisen onderhoud' geconfigureerd. U kunt de volgende realtime rapport maken of u kunt hier stoppen en configureren van het dashboard. 

### <a name="vehicle-health-statistics-report"></a>Vehicle Health statistieken rapport

1. Selecteer ![toevoegen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) een nieuw rapport toe te voegen. Wijzig de naam **voertuigen Health statistieken**. 

2. Op de **visualisaties** tabblad de **meter** visualisatie. Sleep **snelheid** naar de **waarde**, **minimumwaarde**, en **maximumwaarde** gebieden.

   ![Voertuigen Health statistieken](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

3. In de **waarde** gebied wijzigen van de standaard-aggregatie van **snelheid** naar **gemiddelde**.

4. In de **minimumwaarde** gebied wijzigen van de standaard-aggregatie van **snelheid** naar **Minimum**.

5. In de **maximumwaarde** gebied wijzigen van de standaard-aggregatie van **snelheid** naar **maximale**.

   ![Waarden van de snelheid](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

6. Wijzig de naam van de **meter titel** naar **gemiddelde snelheid**.

   ![Meter](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

7. Selecteer een leeg gebied toevoegen van een nieuwe visualisatie.  

    Op deze manier toevoegen een **meter** voor **engine olie gemiddelde**, **gemiddelde brandstof**, en **gemiddelde temperatuur engine**.  

8. Wijzigen van de standaard-aggregatie van velden in elke meter net zoals in de vorige stappen in de **gemiddelde snelheid** meter.

    ![Aanvullende meters](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

9. Selecteer een leeg gebied toevoegen van een nieuwe visualisatie.

10. Op de **visualisaties** tabblad de **lijndiagram en gegroepeerd kolomdiagram** visualisatie. Sleep **stad** naar **as gedeeld**. Sleep **tirepressure**, **engineoil**, en **snelheid** naar de **kolomwaarden** gebied. Wijzigen van hun samenvoegingstype naar **gemiddelde**. 

11. Sleep **engineTemperature** naar de **regelwaarden** gebied. Wijzigen van het samenvoegingstype naar **gemiddelde**. 

    ![Kolom en regelwaarden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

12. Wijzigen van de grafiek **titel** naar **gemiddelde snelheid, band druk, engine olie en temperatuur motor**.  

    ![Lijndiagram en gegroepeerd kolomdiagram titel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

13. Selecteer een leeg gebied toevoegen van een nieuwe visualisatie.

14. Op de **visualisaties** tabblad de **Treemap** visualisatie. Sleep **Model** naar de **groep** gebied. Sleep **MaintenanceProbability** naar de **waarden** gebied.

15. Wijzigen van de grafiek **titel** naar **Vehicle modellen vereisen onderhoud**.

    ![Titel van de Treemap](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

16. Selecteer een leeg gebied toevoegen van een nieuwe visualisatie.

17. Op de **visualisaties** tabblad de **100% gestapeld staafdiagram** visualisatie. Sleep **stad** naar de **as** gebied. Sleep **MaintenanceProbability** en **RecallProbability** naar de **waarde** gebied.

    ![Gebieden van de as en waarde](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

18. Op de **indeling** sectie **gegevens kleuren**. Stel de **MaintenanceProbability** kleur aan de waarde **F2C80F**.

19. Wijzigen van de grafiek **titel** naar **kans Vehicle onderhoud & intrekken door stad**.

    ![Titel van 100% gestapeld staafdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

20. Selecteer een leeg gebied toevoegen van een nieuwe visualisatie.

21. Op de **visualisaties** tabblad de **vlakdiagram** visualisatie. Sleep **Model** naar de **as** gebied. Sleep **engineOil**, **tirepressure**, **snelheid**, en **MaintenanceProbability** naar de **waarden** gebied. Wijzigen van hun samenvoegingstype naar **gemiddelde**. 

    ![Samenvoegingstype](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

22. Wijzigen van de grafiek **titel** naar **gemiddelde engine olie, druk, snelheid en het onderhoud kans genoeg door model**.

    ![De grafiektitel gebied](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

23. Selecteer een leeg gebied toevoegen van een nieuwe visualisatie.

24. Op de **visualisaties** tabblad de **grafiek spreidingsdiagrammen** visualisatie. Sleep **Model** naar de **Details** en **legenda** gebieden. Sleep **brandstof** naar de **X-as** gebied. Wijzigen van de aggregatie op **gemiddelde**. Sleep **engineTemperature** naar de **Y-as** gebied. Wijzigen van de aggregatie op **gemiddelde**. Sleep **vin** naar de **grootte** gebied.

    ![Details, legenda, as en de grootte van gebieden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

25. Wijzigen van de grafiek **titel** naar **gemiddelde van brandstof, het gemiddelde van engineTemperature en aantal vin door het Model en het Model**.

    ![Spreiding grafiektitel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

    Het laatste rapport lijkt op het volgende voorbeeld:

    ![Laatste rapport](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>Pincode visualisaties van de rapporten naar het real-time dashboard
1. Een lege dashboard maken door het plusteken naast **Dashboards**. Voer de naam **Dashboard met analytische telemetrie Vehicle**.

    ![Dashboard plus symbool](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

2. De visualisaties van de vorige rapporten naar het dashboard vastmaken. 

    ![Dashboard pincode symbool](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

    Wanneer alle drie rapporten worden vastgemaakt aan het dashboard, ziet er in het volgende voorbeeld. Als u alle rapporten hebt gemaakt, kan uw dashboard anders uitzien. 

    ![Dashboard met rapporten](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Het real-time dashboard gemaakt. Als u doorgaat CarEventGenerator.exe en RealtimeDashboardApp.exe uit te voeren, ziet u live updates op het dashboard. De volgende stappen nemen ongeveer 10 tot 15 minuten te voltooien.

## <a name="set-up-the-power-bi-batch-processing-dashboard"></a>De Power BI-dashboard batch verwerking instellen
> [!NOTE]
> Het duurt ongeveer twee uur (van de voltooiing van de implementatie) voor de end-to-end-batch pijplijn voor het voltooien van de uitvoering en verwerken van een jaar lang van gegenereerde gegevens verwerkt. Wacht totdat de verwerking te voltooien voordat u verdergaat met de volgende stappen uit. 
> 
> 

### <a name="download-the-power-bi-designer-file"></a>Download het Power BI designer

1. Een vooraf geconfigureerde Power BI designer-bestand is opgenomen als onderdeel van de implementatie-instructies voor handmatige bewerking. Zoek naar '2. Instellen van het dashboard met Power BI batch verwerkt."

2. Download de Power BI-sjabloon voor batchverwerking dashboard hier aangeroepen **ConnectedCarsPbiReport.pbix**.

3. Lokaal opslaat.

### <a name="configure-power-bi-reports"></a>Power BI-rapporten configureren

1. Open het bestand designer **ConnectedCarsPbiReport.pbix** met behulp van de Power BI Desktop. Als u er nog geen hebt, installeert u de Power BI Desktop van de [installatie van Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331) website.

2. Selecteer **query's bewerken**.

    ![Query's bewerken](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

3. Dubbelklik op **bron**.

    ![Bron](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

4. De server-verbindingsreeks bijwerken met de Azure SQL-server die is ingericht als onderdeel van de implementatie. Zoeken in de handmatige bewerking instructies onder Azure SQL database:

    * Server: somethingsrv.database.windows.net
    * Database: connectedcar
    * Gebruikersnaam: gebruikersnaam
    * Wachtwoord: U kunt het wachtwoord van uw SQL Server beheren vanuit de Azure-portal.

5. Laat **Database** als **connectedcar**.

    ![Database](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

6. Selecteer **OK**.

7. De **Windows-referentie** tabblad is standaard geselecteerd. Wijzig dit in **Database referenties** door het selecteren van de **Database** tabblad aan de rechterkant.

8. Voer de **gebruikersnaam** en **wachtwoord** van uw Azure SQL-database die is opgegeven tijdens de installatie van de implementatie.

    ![Databasereferenties](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

9. Selecteer **Verbinden**.

10. Herhaal de vorige stappen voor elk van de drie resterende query's aanwezig zijn in het rechterdeelvenster. Werk vervolgens de details van gegevensbron verbinding.

11. Selecteer **sluiten en te laden**. Gegevenssets voor Power BI Desktop-bestand zijn verbonden met SQL database-tabellen.

12. Selecteer **sluiten** de Power BI Desktop-bestand te sluiten.

    ![Sluiten](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

13. Selecteer **opslaan** de wijzigingen wilt opslaan. 

U hebt nu alle rapporten die overeenkomen met het pad in de oplossing voor batchverwerking geconfigureerd. 

## <a name="upload-to-powerbicom"></a>Uploaden naar powerbi.com
1. Ga naar de [Power BI-webportal](http://powerbi.com), en meld u aan.

2. Selecteer **gegevens ophalen**.

3. Upload het Power BI Desktop-bestand. Selecteer **gegevens ophalen** > **bestanden ophalen** > **lokaal bestand**.

4. Ga naar **ConnectedCarsPbiReport.pbix**.

5. Nadat het bestand is geüpload, gaat u terug naar uw Power BI-werkruimte. Een gegevensset, een rapport en een lege dashboard voor u gemaakt.  

6. Pincode-grafieken vast aan een nieuw dashboard aangeroepen **Dashboard met analytische telemetrie Vehicle** in Power BI. Selecteer het lege dashboard dat eerder is gemaakt en gaat u naar de **rapporten** sectie. Selecteer het recent geüploade rapport.  

    ![Nieuwe Power BI-dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

    Het rapport heeft zes pagina's:

    Pagina 1: Vehicle dichtheid  
    Pagina 2: Realtime vehicle health  
    Pagina 3: Aangestuurd agressief voertuigen   
    Pagina 4: Teruggehaald voertuigen  
    Pagina 5: Brandstof efficiënt aangedreven voertuigen  
    Pagina 6: Contoso motoren-logo  

    ![Power BI-rapport met zes pagina 's](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

7. Van **pagina 3**, pincode van de volgende inhoud:  

    a. **Telling van vin**  

   ![Aantal van de pagina 3 van vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png)

    b. **Agressief voertuigen aangedreven door model – waterval grafiek** 

   ![Diagram van de pagina 3 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

8. Van **pagina 5**, pincode van de volgende inhoud: 

    a. **Telling van vin**

   ![Pagina 5 grafiek 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)

    b. **Fuel-Efficient voertuigen door model: gegroepeerd kolomdiagram**

   ![Pagina 5 grafiek 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

9. Van **pagina 4**, pincode van de volgende inhoud:  

    a. **Telling van vin** 

   ![Pagina 4 Grafiek 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 

    b. **Ingetrokken voertuigen per plaats, model: Treemap**

   ![Pagina 4 grafiek 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

10. Van **pagina 6**, pincode van de volgende inhoud:  

    * **Contoso motoren-logo**

    ![Contoso motoren-logo](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

### <a name="organize-the-dashboard"></a>Het dashboard te delen  

1. Ga naar het dashboard.

2. Beweeg de muisaanwijzer over elke grafiek. Wijzig de naam van elke grafiek op basis van de opgegeven in het volgende voorbeeld van de voltooide dashboard naamgeving:

   ![Dashboard organisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png) 
   
3. De grafieken om te zoeken op het volgende voorbeeld van dashboard verplaatsen:

    ![Andere volgorde dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)

4. Nadat u de rapporten die zijn vermeld in dit document hebt gemaakt, is de laatste voltooide dashboard ziet eruit als in het volgende voorbeeld: 

   ![Laatste dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

U hebt gemaakt de rapporten en het dashboard krijgen realtime, voorspellende en gewoonten batch insights op de drager gezondheid en groot.  
