---
title: Power BI-Dashboard vehicle status en die zorg draagt gewoonten - Azure | Microsoft Docs
description: De mogelijkheden van Cortana Intelligence gebruiken om inzicht in real-time en voorspeld op vehicle status en verkeer te krijgen gewoonten.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: bradsev
ms.openlocfilehash: 39be936520d62cb1c1c28de9bd72f8f489166082
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a>Vehicle telemetrie analytics-oplossing sjabloon Power BI-Dashboard installatie-instructies
Dit **menu** koppelingen naar de hoofdstukken in deze playbook. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

De drager telemetrie Analytics-oplossing gepresenteerd hoe auto dealerbedrijven, auto fabrikanten en ondernemingen van de mogelijkheden van Cortana Intelligence gebruikmaken kunnen krijgen realtime en gewoonten voorspellende insights op de drager gezondheid en verkeer naar station verbeteringen aangebracht in het gebied van de klant ervaren R & D- en marketingcampagnes. Dit document bevat stapsgewijze instructies over hoe u de Power BI-rapporten en het dashboard configureren kunt zodra de oplossing is geïmplementeerd in uw abonnement. 

## <a name="prerequisites"></a>Vereisten
1. Implementeer de [telemetrie Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) oplossing  
2. [Installeren van Microsoft Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331)
3. Een [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/). Als u geen Azure-abonnement hebt, aan de slag met de gratis Azure-abonnement
4. Microsoft Power BI-account

## <a name="cortana-intelligence-suite-components"></a>Cortana Intelligence Suite-onderdelen
Als onderdeel van de sjabloon Vehicle telemetrie Analytics-oplossing, worden de volgende Cortana Intelligence-services geïmplementeerd in uw abonnement.

* **Event Hub** voor het opnemen van miljoenen vehicle telemetrische gebeurtenissen in Azure.
* **Stream Analytics** voor realtime-inzichten op vehicle gezondheid en dat de gegevens zich blijft voordoen in langdurige opslag voor uitgebreidere batch analyses.
* **Machine Learning** voor afwijkingsdetectie in realtime en batchverwerking om voorspellende inzicht te krijgen.
* **HDInsight** wordt gebruikt voor het transformeren van gegevens op grote schaal
* **Data Factory** orchestration, planning, bronbeheer en controle van de pijplijn batchverwerking verwerkt.

**Power BI** biedt deze oplossing een uitgebreide dashboard voor realtime-gegevens en visualisaties predictive analytics. 

De oplossing maakt gebruik van twee verschillende gegevensbronnen: **gesimuleerde vehicle signalen en diagnostische gegevensset** en **vehicle catalogus**.

Er is een vehicle telematica simulator opgenomen als onderdeel van deze oplossing. Deze diagnostische gegevens verzendt en signalen overeenkomt met de status van de drager als patroon groot op een bepaald tijdstip. 

De catalogus drager is een verwijzing gegevensset met Chassisnummer model toewijzen aan

## <a name="power-bi-dashboard-preparation"></a>Voorbereiding van Power BI-Dashboard
### <a name="setup-power-bi-real-time-dashboard"></a>Realtime Power BI-Dashboard instellen

**Start de dashboardtoepassing realtime** zodra de implementatie is voltooid, moet u Volg de instructies van de bewerking handmatig

* Realtime dashboardtoepassing RealtimeDashboardApp.zip downloaden en uitpakken van het.
*  Open de app-configuratiebestand RealtimeDashboardApp.exe.config, vervangen appSettings voor Eventhub, Blob Storage en ML service-verbindingen met de waarden in de instructies van de bewerking handmatig en sla de wijzigingen in de uitgepakte map.
* Toepassing RealtimeDashboardApp.exe uitvoeren. Een aanmeldingsvenster wordt pop-, geldige referenties van uw Power BI en klik op de **accepteren** knop. De app wordt vervolgens gestart om uit te voeren.

   ![Aanmelden bij Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
   ![Machtigingen voor Power BI-Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

* Meld u aan bij Power BI-website en realtime dashboard maken.

Nu bent u klaar voor het configureren van de Power BI-dashboard met uitgebreide visualisaties krijgen realtime en gewoonten voorspellende insights op de drager gezondheid en groot. Het duurt ongeveer 45 minuten tot een uur alle rapporten maken en configureren van het dashboard. 

### <a name="configure-power-bi-reports"></a>Power BI-rapporten configureren
De realtime-rapporten en het dashboard rekening houden met ongeveer 30 tot 45 minuten om te voltooien. Blader naar [http://powerbi.com](http://powerbi.com) en meld u aan.

![Aanmelden bij Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

Een nieuwe gegevensset wordt gegenereerd in Power BI. Klik op de **ConnectedCarsRealtime** gegevensset.

![Geselecteerd verbonden realtime gegevensset voor auto 's](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Sla de leeg rapport via **Ctrl + s**.

![Leeg rapport opslaan](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Geef de naam van rapport *Vehicle telemetrie Analytics realtime - rapporten*.

![Geef de naam van rapport](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Realtime-rapporten
Er zijn drie realtime rapporten in deze oplossing:

1. Voertuigen in bewerking
2. Voertuigen onderhoud vereisen
3. Voertuigen Health statistieken

U kunt de drie realtime rapporten configureren of stoppen nadat elke fase en gaat u verder met de volgende sectie van de configuratie van de batch-rapporten. We raden u aan te maken van de drie rapporten voor het visualiseren van de volledige inzichten van het real-time pad van de oplossing.  

### <a name="1-vehicles-in-operation"></a>1. Voertuigen in bewerking
Dubbelklik op **pagina 1** en wijzig de naam 'Voertuigen in bewerking'  
    ![Auto - verbonden voertuigen in bewerking](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Selecteer **vin** veld **velden** en visualisatie kiezen als **'Kaart'**.  

Kaart visualisatie wordt gemaakt, zoals weergegeven in afbeelding.  
    ![Verbonden auto - Selecteer vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Klik op het lege gebied om toe te voegen nieuwe visualisatie.  

Selecteer **stad** en **vin** van velden. Visualisatie te wijzigen **'Kaart'**. Sleep **vin** in waardengebied. Sleep **stad** van velden in om **legenda** gebied.   
    ![Auto - kaart visualisatie verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)

Selecteer **indeling** rubriek van **visualisaties**, klikt u op **titel** en wijzig de **tekst** naar **'voertuigen in bewerking per plaats'**.  
    ![Auto - verbonden voertuigen in bewerking per plaats](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Laatste visualisatie lijkt zoals weergegeven in afbeelding.    
    ![Verbonden auto - laatste visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Klik op het lege gebied om toe te voegen nieuwe visualisatie.  

Selecteer **stad** en **vin**, wijzigt in visualisatie **kolomdiagram geclusterde**. Zorg ervoor dat **stad** veld **as gebied** en **vin** in **waarde gebied**  

De grafiek sorteren door **'Aantal van vin'**  
    ![Verbonden auto - telling van vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

Diagram wijzigen **titel** naar **'Voertuigen in bewerking per plaats'**  

Klik op de **indeling** sectie en selecteer vervolgens **gegevens kleuren**, klikt u op de **'Op'** naar **Alles weergeven**  
    ![Verbonden auto - weergeven alle gegevens kleuren](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

De kleur van afzonderlijke plaats door te klikken op het Kleurenpictogram wijzigen.  
    ![Verbonden auto - kleuren wijzigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Klik op het lege gebied om toe te voegen nieuwe visualisatie.  

Selecteer **kolomdiagram geclusterde** visualisatie van visualisaties, Sleep **stad** veld **as** gebied **Model** in **Legenda** gebied en **vin** in **waarde** gebied.  
    ![Verbonden auto - gegroepeerd kolomdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Verbonden auto - Rendering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)

Alle visualisaties op deze pagina rangschikken zoals weergegeven in afbeelding.  
    ![Verbonden auto - visualisaties](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

U hebt het real-time rapport 'Voertuigen in bewerking' geconfigureerd. U kunt doorgaan met het volgende realtime rapport gemaakt of hier stoppen en het configureren van het dashboard. 

### <a name="2-vehicles-requiring-maintenance"></a>2. Voertuigen onderhoud vereisen
Klik op ![toevoegen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) om een nieuw rapport toevoegt, wijzigt u de naam **'Voertuigen vereisen onderhoud'**

![Verbonden auto - voertuigen onderhoud vereisen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Selecteer **vin** veld en wijzigt in visualisatie **kaart**.  
    ![Verbonden auto - visualisatie Vin-kaart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

We hebben een veld met de naam 'MaintenanceLabel' In de gegevensset. Dit veld kan een waarde van '0' of '1' hebben." Deze waarde is ingesteld door het Azure Machine Learning-model ingericht als onderdeel van de oplossing en geïntegreerd met het real-time pad. De waarde '1' geeft dat een medium vereist onderhoud. 

Om toe te voegen een **paginaniveau** filter voor het weergeven van voertuigen gegevens, die onderhoud vereisen: 

1. Sleep de **'MaintenanceLabel'** veld in **niveau paginafilters**.  
   ![Verbonden auto - Filters paginaniveau](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  
2. Klik op **Basic filteren** menu aan de onderkant van het niveau van MaintenanceLabel paginafilter aanwezig.  
   ![Verbonden auto - Basic filteren](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  
3. Stel de filterwaarde op **'1'**    
   ![Verbonden auto - filterwaarde](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  

Klik op het lege gebied om toe te voegen nieuwe visualisatie.  

Selecteer **kolomdiagram geclusterde** van visualisaties  
![Verbonden auto - visualisatie Vind-kaart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Verbonden auto - gegroepeerd kolomdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Sleep veld **Model** in **as** gebied **Vin** naar **waarde** gebied. Sorteer visualisatie door **aantal vin**.  Diagram wijzigen **titel** naar **"Voertuigen onderhoud vereisen door model"**  

Sleep **vin** velden in **kleurverzadiging** aanwezig zijn bij **velden** ![velden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) sectie van **visualisatie**tabblad  
![Verbonden auto - kleurverzadiging](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

Wijziging **gegevens kleuren** in visualisaties van **indeling** sectie  
Minimale kleur te wijzigen: **F2C812**  
Maximale kleur te wijzigen: **FF6300**  
![Auto - wijzigingen in de kleuren verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Verbonden auto - nieuwe visualisatie kleuren](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Klik op het lege gebied om toe te voegen nieuwe visualisatie.  

Selecteer **gegroepeerd kolomdiagram** visualisaties, Sleep **vin** veld in **waarde** gebied slepen **stad** veld in **As** gebied. De grafiek sorteren door **'Aantal van vin'**. Diagram wijzigen **titel** naar **'Voertuigen vereisen onderhoud per plaats'**   
![Verbonden auto - voertuigen onderhoud per plaats vereisen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Klik op het lege gebied om toe te voegen nieuwe visualisatie.  

Selecteer **met meerdere rijen kaart** visualisatie van visualisaties, Sleep **Model** en **vin** in de **velden** gebied.  
![Verbonden auto - meerdere rij-kaart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Herschikken alle van de visualisatie, het laatste rapport ziet er als volgt:  
![Verbonden auto - meerdere rij-kaart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

U hebt het real-time rapport 'Voertuigen vereisen onderhoud' geconfigureerd. U kunt doorgaan met het volgende realtime rapport gemaakt of hier stoppen en het configureren van het dashboard. 

### <a name="3-vehicles-health-statistics"></a>3. Voertuigen Health statistieken
Klik op ![toevoegen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) voor het nieuwe rapport toevoegt, wijzigt u de naam **'Voertuigen Health statistieken'**  

Selecteer **meter** visualisatie van visualisaties, Sleep de **snelheid** veld in **waarde, minimumwaarde, maximumwaarde** gebieden.  
![Verbonden auto - meerdere rij-kaart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

Wijzigen van de standaard-aggregatie van **snelheid** in **waarde gebied** naar **gemiddelde** 

Wijzigen van de standaard-aggregatie van **snelheid** in **Minimum gebied** naar **Minimum**

Wijzigen van de standaard-aggregatie van **snelheid** in **Maximum gebied** naar **maximale**

![Verbonden auto - meerdere rij-kaart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Wijzig de naam van de **meter titel** naar **'Gemiddelde snelheid'** 

![Verbonden auto - meter](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Klik op het lege gebied om toe te voegen nieuwe visualisatie.  

Op dezelfde manier toevoegen een **meter** voor **engine olie gemiddelde**, **gemiddelde brandstof**, en **gemiddelde temperatuur aangeven engine**.  

Wijzigen van de standaard-aggregatie van velden in elke meter conform bovenstaande stappen in **'Gemiddelde snelheid'** meter.

![Verbonden auto - meters](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Klik op het lege gebied om toe te voegen nieuwe visualisatie.

Selecteer **lijndiagram en gegroepeerd kolomdiagram** van visualisaties, sleept u **stad** veld in **gedeeld as**, sleept u **snelheid**, **velden tirepressure en engineoil** in **kolomwaarden** gebied wijzigt in hun aggregatie **gemiddelde**. 

Sleep de **engineTemperature** veld in **regelwaarden** gebied wijzigt in de aggregatie **gemiddelde**. 

![Verbonden auto - visualisaties velden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

Wijzigen van de grafiek **titel** naar **'Gemiddelde snelheid, band druk, engine olie en temperatuur motor'**.  

![Verbonden auto - visualisaties velden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Klik op het lege gebied om toe te voegen nieuwe visualisatie.

Selecteer **Treemap** visualisatie van visualisaties, sleept u de **Model** veld in de **groep** gebied en sleep het veld **MaintenanceProbability** in de **waarden** gebied.

Wijzigen van de grafiek **titel** naar **'Vehicle modellen vereisen onderhoud'**.

![Verbonden auto - grafiektitel wijzigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Klik op het lege gebied om toe te voegen nieuwe visualisatie.

Selecteer **100% gestapeld staafdiagram** visualisatie, Sleep de **stad** veld in de **as** gebied en sleep de **MaintenanceProbability**, **RecallProbability** velden in de **waarde** gebied.

![Verbonden auto - toevoegen nieuwe visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Klik op **indeling**, selecteer **gegevens kleuren**, en stel de **MaintenanceProbability** kleur aan de waarde **'F2C80F'**.

Wijzig de **titel** van de grafiek **'Waarschijnlijkheid van Vehicle onderhoud & intrekken per plaats'**.

![Verbonden auto - toevoegen nieuwe visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Klik op het lege gebied om toe te voegen nieuwe visualisatie.

Selecteer **vlakdiagram** visualisatie van visualisaties Sleep de **Model** veld in de **as** gebied en sleep de **engineOil, tirepressure, de snelheid en MaintenanceProbability** velden in de **waarden** gebied. Wijzigen van hun samenvoegingstype naar **'Gemiddeld'**. 

![Verbonden auto - aggregatie wijzigingstype](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

Wijzig de titel van de grafiek **"Gemiddelde engine olie, druk, snelheid en het onderhoud kans genoeg door model"**.

![Verbonden auto - grafiektitel wijzigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Klik op het lege gebied om toe te voegen nieuwe visualisatie:

1. Selecteer **grafiek spreidingsdiagrammen** visualisatie van visualisaties.
2. Sleep de **Model** veld in de **Details** en **legenda** gebied.
3. Sleep de **brandstof** veld in de **x-as** gebied wijzigen van de aggregatie op **gemiddelde**.
4. Sleep **engineTemparature** in **y-as gebied**, wijzig de aggregatie op **gemiddelde**
5. Sleep de **vin** veld in de **grootte** gebied.

![Verbonden auto - toevoegen nieuwe visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Wijzigen van de grafiek **titel** naar **"Gemiddelden van brandstof, Engine temperatuur door Model"**.

![Verbonden auto - grafiektitel wijzigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

Het laatste rapport eruit als zoals hieronder wordt weergegeven.

![Auto-Final rapport verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>Pincode visualisaties van de rapporten naar het real-time dashboard
Een lege dashboard door te klikken op het plusteken naast Dashboards maken. U kunt andere naam 'Vehicle telemetrie Analytics Dashboard'

![Verbonden auto-Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

De visualisatie uit de bovenstaande rapporten naar het dashboard vastmaken. 

![Verbonden auto-Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

Het dashboard ziet er als volgt wanneer de drie rapporten zijn gemaakt en de bijbehorende visualisaties worden vastgemaakt aan het dashboard. Als u niet alle rapporten hebt gemaakt, kan uw dashboard anders zijn. 

![Verbonden auto-Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Gefeliciteerd. U hebt het real-time dashboard gemaakt. Als u doorgaat CarEventGenerator.exe en RealtimeDashboardApp.exe uit te voeren, ziet u live updates op het dashboard. Het moet ongeveer 10 tot 15 minuten duren voordat de volgende stappen uitvoeren.

## <a name="setup-power-bi-batch-processing-dashboard"></a>Power BI batch-verwerking dashboard instellen
> [!NOTE]
> Het duurt ongeveer twee uur (van de voltooiing van de implementatie) voor de pijplijn complete batchverwerking uitvoering is voltooid en een jaar waard van gegenereerde gegevens verwerkt. Dus wacht u totdat de verwerking te voltooien voordat u doorgaat met de volgende stappen. 
> 
> 

**Download het Power BI designer**

* Een vooraf geconfigureerde Power BI designer-bestand is opgenomen als onderdeel van de implementatie handmatig bewerking instructies
* Zoek naar 2. Het installatieprogramma Power BI batch verwerking dashboard kunt u de Power BI-sjabloon voor batchverwerking dashboard hier aangeroepen downloaden **ConnectedCarsPbiReport.pbix**.
* Lokaal opslaan

**Power BI-rapporten configureren**

* Open het bestand designer '**ConnectedCarsPbiReport.pbix**' met behulp van Power BI Desktop. Als u nog geen, installeert u Power BI Desktop van [Power BI Desktop installeren](http://www.microsoft.com/download/details.aspx?id=45331). 
* Klik op de **query's bewerken**.

![Power BI query bewerken](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

* Dubbelklik op de **bron**.

![Set Power BI-bron](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

* Server-verbindingsreeks bijwerken met de Azure SQL-server die als onderdeel van de implementatie is ingericht.  Zoeken in de handmatige bewerking instructies onder 

    4. Azure SQL Database
    
    * Server: somethingsrv.database.windows.net
    * Database: connectedcar
    * Gebruikersnaam: gebruikersnaam
    * Wachtwoord: U kunt het wachtwoord van uw SQL-server beheren vanuit Azure-portal

* Laat **Database** als *connectedcar*.

![Stel de database in Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

* Klik op **OK**.
* U ziet **Windows-referentie** tabblad standaard geselecteerd, wijzigen in **Database referenties** door te klikken op **Database** tabblad rechts.
* Geef de **gebruikersnaam** en **wachtwoord** van uw Azure SQL Database die is opgegeven tijdens de installatie van de implementatie.

![Geef databasereferenties op](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

* Klik op **verbinding maken**
* Herhaal de bovenstaande stappen voor elk van de drie resterende query's aanwezig zijn bij het rechter deelvenster en werk vervolgens de details van gegevensbron verbinding.
* Klik op **sluiten en te laden**. Power BI Desktop-bestand gegevenssets zijn verbonden met SQL Azure Database-tabellen.
* **Sluit** Power BI Desktop-bestand.

![Sluit Power BI desktop](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

* Klik op **opslaan** knop de wijzigingen wilt opslaan. 

U hebt nu alle rapporten die overeenkomt met het pad voor het verwerken van batch in de oplossing hebt geconfigureerd. 

## <a name="upload-to-powerbicom"></a>Uploaden naar *powerbi.com*
1. Navigeer naar de Power BI-webportal op http://powerbi.com en meld u aan.
2. Klik op **gegevens ophalen**  
3. De Power BI Desktop-bestand uploaden.  
4. Als u wilt uploaden, klikt u op **gegevens ophalen -> bestanden ophalen -> lokaal bestand**  
5. Navigeer naar de **'**ConnectedCarsPbiReport.pbix**'**  
6. Zodra het bestand is geüpload, wordt u gaat terug naar uw Power BI-werkruimte.  

Een gegevensset, rapport en een lege dashboard wordt voor u gemaakt.  

Pincode-grafieken vast aan een nieuw dashboard aangeroepen **Dashboard met analytische telemetrie Vehicle** in **Power BI**. Klik op de lege dashboard die eerder is gemaakt en navigeer vervolgens naar de **rapporten** sectie op het rapport dat recent geüploade.  

![Vehicle telemetrie Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

**Houd rekening met dat het rapport heeft zes pagina's:**  
Pagina 1: Vehicle dichtheid  
Pagina 2: Realtime vehicle health  
Pagina 3: Aangestuurd agressief voertuigen   
Pagina 4: Teruggehaald voertuigen  
Pagina 5: Brandstof efficiënt aangedreven voertuigen  
Pagina 6: Contoso-Logo  

![Verbonden auto's Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

**Vanaf 3 pagina**, pincode van de volgende:  

1. Telling van VIN  
   ![Verbonden auto's Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 
2. Agressief voertuigen aangedreven door model – waterval grafiek  
   ![Vehicle telemetrie - pincode grafieken 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Van pagina 5**, pincode van de volgende: 

1. Telling van vin    
   ![Vehicle telemetrie - grafieken van de pincode 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2. Efficiënte voertuigen brandstof door model: gegroepeerd kolomdiagram  
   ![Vehicle telemetrie - pincode grafieken 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Van pagina 4**, pincode van de volgende:  

1. Telling van vin  
   ![Vehicle telemetrie - pincode grafieken 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 
2. Ingetrokken voertuigen per plaats, model: Treemap  
   ![Vehicle telemetrie - pincode grafieken 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Pagina 6**, pincode van de volgende:  

1. Contoso motoren-logo  
   ![Vehicle telemetrie - pincode grafieken 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Het dashboard te delen**  

1. Ga naar het dashboard
2. Beweeg de muisaanwijzer over elke grafiek en wijzig de naam van dit op basis van de naamgeving opgegeven in de onderstaande afbeelding van volledige dashboard. De grafieken rond ook verplaatsen eruitzien zoals bij het onderstaande dashboard.  
   ![Vehicle telemetrie - Dashboard 2 ordenen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
   ![Vehicle telemetrie Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3. Als u kunt de rapporten hebt gemaakt, zoals vermeld in dit document, is de laatste voltooide dashboard moet eruitzien als in de volgende afbeelding. 

![Vehicle telemetrie - Dashboard 2 ordenen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Gefeliciteerd. U hebt gemaakt met de rapporten en het dashboard te krijgen van realtime, voorspellende batch-inzichten op vehicle status en groot gewoonten.  
