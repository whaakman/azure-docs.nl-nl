---
title: Een sjabloon van het apparaat in een Azure IoT Central-toepassing instellen | Microsoft Docs
description: Informatie over het instellen van de sjabloon voor een apparaat met metingen, instellingen, eigenschappen, regels en een dashboard.
author: viv-liu
ms.author: viviali
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: a076ee5275a4d053613902a1980542590263385c
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034266"
---
# <a name="set-up-a-device-template"></a>Een sjabloon van het apparaat instellen

Een apparaat-sjabloon is een blauwdruk die definieert de kenmerken en het gedrag van een type apparaat dat is verbonden met een Microsoft Azure IoT Central-toepassing.

Een opbouwfunctie kunt bijvoorbeeld een apparaat-sjabloon voor een IoT-verbonden ventilator waarvoor maken:

- Temperatuur telemetrie meting

- Ventilator motor fout gebeurtenis meting

- Operationele status meting ventilator

- Ventilator snelheid instellen

- Locatie-eigenschap

- Regels voor waarschuwingen verzenden

- Dashboard waarmee u een algemeen overzicht van het apparaat

Met deze sjabloon apparaat een operator kunt maken en verbinding maken met echte fan-apparaten met de namen, zoals **fan-1** en **fan-2**. Alle deze fans zijn metingen, instellingen, eigenschappen, regels en een dashboard dat gebruikers van uw toepassing kunnen controleren en beheren.

> [!NOTE]
> Alleen builders en beheerders kunnen maken, bewerken en verwijderen van apparaatsjablonen. Elke gebruiker apparaten kunt maken op de **Device Explorer** pagina vanuit bestaande apparaatsjablonen.

## <a name="create-a-device-template"></a>Een apparaat-sjabloon maken

1. Ga naar de **toepassing Builder** pagina.

2. Voor het maken van een lege sjabloon selecteert **apparaat-sjabloon maken**, en selecteer vervolgens **aangepaste**.

3. Voer een naam voor uw nieuwe apparaat-sjabloon en selecteer **maken**.

   ![Pagina met details met 'Koelkast' als de naam van het apparaat](./media/howto-set-up-template/devicedetailspage.png)

4. Nu bent u op de **Apparaatdetails** pagina van een nieuw gesimuleerd apparaat. Een gesimuleerd apparaat wordt automatisch voor u gemaakt wanneer u een apparaat-sjabloon maken. Deze gegevens rapporten en net als bij een echt apparaat kan worden beheerd.

Bekijk nu de tabbladen op de **Apparaatdetails** pagina.

## <a name="measurements"></a>Metingen

Metingen zijn de gegevens die afkomstig zijn van uw apparaat. U kunt meerdere metingen toevoegen aan uw sjabloon apparaat zodat deze overeenkomen met de mogelijkheden van uw apparaat.

- **Telemetrie** metingen zijn de numerieke gegevenspunten die uw apparaat na verloop van tijd verzamelt. Ze worden weergegeven als een continue stroom. Een voorbeeld is de temperatuur.
- **Gebeurtenis** metingen zijn point-in-time-gegevens die staat voor iets significante op het apparaat. Een ernstniveau vertegenwoordigt het belang van een gebeurtenis. Een voorbeeld is een motor fan-fout.
- **Status** metingen vertegenwoordigt de status van het apparaat of de bijbehorende onderdelen gedurende een bepaalde periode. Bijvoorbeeld, een ventilator-modus kan worden gedefinieerd als **besturingssysteem** en **gestopt** als de twee mogelijke statussen.

### <a name="create-a-telemetry-measurement"></a>Maken van een meting telemetrie
Selecteren om toe te voegen een nieuwe meting met telemetrie, **sjabloon bewerken**, en klik vervolgens op de **+ nieuwe meting** knop. Selecteer **telemetrie** als de meting, te typen en de details op de **maken telemetrie** formulier.

> [!NOTE]
> Wanneer een echt apparaat is verbonden, Let op de naam van de meting die het apparaat rapporteert. De naam moet exact overeenkomen met de **veldnaam** vermelding voor een meting.

U kunt bijvoorbeeld een nieuwe meting van de temperatuur-telemetrie toevoegen:

![Formulier met details voor de temperatuurmeting 'Telemetrie maken'](./media/howto-set-up-template/measurementsform.png)

Nadat u hebt geselecteerd **gedaan**, wordt de **temperatuur** meting wordt weergegeven in de lijst met metingen. Een operator ziet de visualisatie van de temperatuurgegevens die het verzamelen van het apparaat.

> [!NOTE]
  Het gegevenstype van de meting telemetrie is dubbel.

### <a name="create-an-event-measurement"></a>Een gebeurtenis meting maken
Selecteer om een nieuwe meting van de gebeurtenis toe **sjabloon bewerken**, en klik vervolgens op de **+ nieuwe meting** knop. Selecteer **gebeurtenis** als de meting, te typen en de details op de **gebeurtenis maken** formulier.

Geef de **weergavenaam**, **veldnaam**, en **ernst** details voor de gebeurtenis. U kunt kiezen uit de drie beschikbare niveaus van ernst: **fout**, **waarschuwing**, en **informatie**.  

Bijvoorbeeld, u kunt toevoegen van een nieuwe **ventilator Motor fout** gebeurtenis.

!['Gebeurtenis maken' formulier met details voor een motor gebeurtenis ventilator](./media/howto-set-up-template/eventmeasurementsform.png)

Nadat u hebt geselecteerd **gedaan**, wordt de **ventilator Motor fout** meting wordt weergegeven in de lijst met metingen. Een operator ziet de visualisatie van de gegevens van de gebeurtenis die het apparaat verzendt.

![Gebeurtenis meting grafiek](./media/howto-set-up-template/eventmeasurementschart.png)

Als u wilt meer informatie over de gebeurtenis weergeven, selecteert u het pictogram van de gebeurtenis in de grafiek.

![Details voor de gebeurtenis "Ventilator Motor Error"](./media/howto-set-up-template/eventmeasurementsdetail.png)

> [!NOTE]
  Het gegevenstype van de meting van de gebeurtenis is een tekenreeks.

### <a name="create-a-state-measurement"></a>Maken van een meting staat
Selecteren om toe te voegen een nieuwe meting van de status, **sjabloon bewerken**, en klik vervolgens op de **+ nieuwe meting** knop. Selecteer **status** als de meting, te typen en de details op de **maken status** formulier.

Geef de details voor **weergavenaam**, **veldnaam**, en **waarden** van de status. Elke waarde kan ook een weergavenaam die wordt gebruikt als de waarde wordt weergegeven in de grafieken en tabellen hebben.

Bijvoorbeeld, u kunt toevoegen van een nieuwe **ventilator modus** status waaraan twee mogelijke waarden die het apparaat verzenden kan, **besturingssysteem** en **gestopt**.

!['Bewerkstatus' formulier met details voor de ventilator modus](./media/howto-set-up-template/statemeasurementsform.png)

Nadat u hebt geselecteerd **gedaan**, wordt de **ventilator modus** meting van de status wordt weergegeven in de lijst met metingen. De operator ziet de visualisatie van de van statusgegevens die het apparaat verzendt.

![Status meting grafiek](./media/howto-set-up-template/statemeasurementschart.png)

Als het apparaat te veel gegevenspunten in een kleine duur verzendt, is de meting van de status wordt weergegeven met een ander visueel element, zoals wordt weergegeven in de volgende schermafbeelding. Als u op de grafiek klikt, worden alle gegevenspunten binnen deze periode worden in chronologische volgorde weergegeven. U kunt ook het tijdsbereik voor het weergeven van de meting in de grafiek getekend verfijnen.

> [!NOTE]
  Het gegevenstype van de meting van de status is een tekenreeks.

## <a name="settings"></a>Instellingen

Instellingen voor beheren een apparaat. Deze operators van uw toepassing voor invoer naar het apparaat in staat stellen. U kunt meerdere instellingen toevoegen aan de sjabloon voor apparaten die worden weergegeven als tegels op de **instellingen** tabblad voor operators te gebruiken. U kunt momenteel zes typen instellingen toevoegen: getal, tekst, datum, in-/ uitschakelen, selectielijst en sectielabel.

> [!NOTE]
> Wanneer een echt apparaat is verbonden, Let op de naam van de instelling die het apparaat rapporteert. De naam moet exact overeenkomen met de **veldnaam** vermelding voor een instelling.

Instellingen kunnen zich in een van drie statussen. Het apparaat meldt deze statussen.

- **Gesynchroniseerd**: het apparaat is gewijzigd om de waarde van de instelling weer te geven.

- **In behandeling**: het apparaat wordt momenteel gewijzigd in de waarde van de instelling.

- **Fout**: het apparaat heeft een fout geretourneerd.

U kunt bijvoorbeeld een nieuwe ventilator snelheid-instelling toevoegen door te selecteren **sjabloon bewerken** en in de nieuwe instelling in te voeren:

!['Aantal configureren' formulier met details voor instellingen](./media/howto-set-up-template/settingsform.png)

Nadat u hebt geselecteerd **opslaan**, wordt de **ventilatorsnelheid** instelling wordt weergegeven als een tegel en is gereed om te worden gebruikt voor het wijzigen van de snelheid van de ventilator van het apparaat.

Nadat u een tegel hebt gemaakt, kunt u uw nieuwe instelling uitproberen. Selecteer eerst **gedaan** in de rechterbovenhoek van het scherm.

![Tabblad 'Instellingen' met de "Ontwerpmodus" switch voor de tegel](./media/howto-set-up-template/settingstile.png)

## <a name="properties"></a>Eigenschappen

Eigenschappen zijn de metagegevens van apparaten die is gekoppeld aan het apparaat, zoals de locatie van het apparaat en het serienummer. U kunt meerdere eigenschappen toevoegen aan de sjabloon voor apparaten die worden weergegeven als tegels op de **eigenschappen** tabblad. Een operator kunt de waarden van eigenschappen opgeven wanneer ze een apparaat maken en ze kunnen deze waarden op elk gewenst moment bewerken. U kunt momenteel zes typen eigenschappen toevoegen: getal, tekst, datum, in-/ uitschakelen, apparaateigenschap en label.

Er zijn twee soorten eigenschappen:

- **Apparaat** eigenschappen die het apparaat rapporteert.
- **Toepassing** eigenschappen die alleen in de toepassing worden opgeslagen. Het apparaat niet wordt herkend door de toepassingseigenschappen van de.

> [!NOTE]
> Voor apparaateigenschappen, wanneer een echt apparaat is verbonden, Let op de naam van de eigenschap die het apparaat rapporteert. De naam moet exact overeenkomen met de **veldnaam** vermelding voor de eigenschap. Voor eigenschappen van de toepassing de veldnaam kan van alles zijn die u wilt dat, als de naam uniek in de sjabloon van het apparaat is.

Bijvoorbeeld, u locatie van het apparaat kunt toevoegen als een nieuwe eigenschap door te selecteren **sjabloon bewerken** en in te voeren in de nieuwe eigenschap:

![Het formulier 'Tekst configureren' op het tabblad 'Eigenschappen'](./media/howto-set-up-template/propertiesform.png)

Nadat u hebt geselecteerd **opslaan**, locatie van het apparaat wordt weergegeven als een tegel:

![Locatie-tegel](./media/howto-set-up-template/propertiestile.png)

Nadat u een tegel hebt gemaakt, kunt u de waarde van eigenschap kunt wijzigen. Selecteer eerst **gedaan** in de rechterbovenhoek van het scherm.

### <a name="create-a-location-property-through-azure-maps"></a>Een locatie-eigenschap via Azure-kaarten maken
U kunt de geografische context geven tot uw locatiegegevens in Azure IoT Central en eventuele coördinaten breedtegraad en lengtegraad van een straatnaam. Of u kunt gewoon breedtegraad en lengtegraad kaartcoördinaten. Azure kaarten kunt deze mogelijkheid in IoT Central.

U kunt twee typen locatie-eigenschappen toevoegen:
- **Locatie als een eigenschap van de toepassing**, dat alleen in de toepassing wordt opgeslagen. Het apparaat niet wordt herkend door de toepassingseigenschappen van de.
- **Locatie als een apparaateigenschap**, dat het apparaat rapporteert.

#### <a name="add-location-as-an-application-property"></a>Locatie als een eigenschap van de toepassing toevoegen 
U kunt een locatie-eigenschap maken als een eigenschap van de toepassing met behulp van Azure-kaarten in uw IoT Central-toepassing. U kunt bijvoorbeeld de installatie-adres van het apparaat toevoegen. 

1. Op de **eigenschappen** tabblad **sjabloon bewerken**.

   ![Tabblad 'Eigenschappen' met de ontwerpmodus ingeschakeld](./media/howto-set-up-template/locationcloudproperty1.png)

2. Selecteer in de bibliotheek **locatie**.
3. Configureer **weergavenaam**, **veldnaam**, en (optioneel) **beginwaarde** voor de locatie. 

   ![Formulier met details voor de locatie van "Locatie configureren"](./media/howto-set-up-template/locationcloudproperty2.png)

   Er zijn twee ondersteunde indelingen voor het toevoegen van een locatie:
   - **Locatie als een adres**
   - **Locatie als coördinaten** 

4. Selecteer **opslaan** en **gedaan**. Een operator kan nu de locatiewaarde in het veld locatie formulier bijwerken. 

#### <a name="add-location-as-a-device-property"></a>Locatie als de apparaateigenschap van een toevoegen 

U kunt een locatie-eigenschap maken als een apparaateigenschap die het apparaat rapporteert. Bijvoorbeeld, als u wilt bijhouden van de locatie van het apparaat:

1. Op de **eigenschappen** tabblad **sjabloon bewerken**.

   ![Tabblad 'Eigenschappen' met de ontwerpmodus ingeschakeld](./media/howto-set-up-template/locationdeviceproperty1.png)

2. Selecteer **Apparaateigenschap** uit de bibliotheek.
3. Configureer de weergavenaam en de veldnaam van het en selecteer **locatie** als het gegevenstype. 

   > [!NOTE]
   > De veldnaam moet exact overeenkomen met de naam van de eigenschap die het apparaat rapporteert. 

   ![Formulier met details voor de locatie 'Eigenschappen van een apparaat configureren'](./media/howto-set-up-template/locationdeviceproperty2.png)

Nu u de locatie-eigenschap hebt geconfigureerd, kunt u [toevoegen van een kaart voor het visualiseren van de locatie in het apparaatdashboard](#add-an-azure-maps-location-in-the-dashboard).

## <a name="commands"></a>Opdrachten

Opdrachten worden gebruikt om een apparaat extern kunt beheren. Deze operators van uw toepassing onmiddellijk opdrachten uitvoeren op het apparaat in staat stellen. U kunt meerdere opdrachten toevoegen aan de sjabloon voor apparaten die worden weergegeven als tegels op de **opdrachten** tabblad voor operators te gebruiken. Als de opbouwfunctie voor van het apparaat hebt u de flexibiliteit om opdrachten op basis van uw vereisten te definiëren.

Wat is een opdracht verschil met een instelling? 

* **Instellen van**: een instelling is een configuratie die u wilt toepassen op een apparaat, en u wilt dat het apparaat aan dat de configuratie van blijft actief totdat u deze wijzigt. Bijvoorbeeld, u wilt de temperatuur van de vriezer instellen en u wilt dat deze instelling, zelfs wanneer de vriesvaartuigen opnieuw wordt opgestart. 

* **Opdracht**: U opdrachten gebruiken voor het direct uitvoeren op het apparaat op afstand van IoT Central. Als een apparaat niet is verbonden, wordt de opdracht een time-out optreedt en mislukt. U wilt bijvoorbeeld een apparaat opnieuw start.  

Wanneer u een opdracht uitvoert, kan het zijn in een van drie statussen, afhankelijk van of het apparaat de opdracht ontvangen.

Bijvoorbeeld, u kunt toevoegen van een nieuwe **Echo** opdracht door het selecteren van **sjabloon bewerken**, vervolgens te klikken op **+ nieuwe opdracht**, en in te voeren in de nieuwe opdracht:

!['-Opdracht configureren' formulier met details voor echo](./media/howto-set-up-template/commandsecho.png)

Nadat u hebt geselecteerd **opslaan** en **gedaan**, wordt de **Echo** opdracht wordt weergegeven als een tegel en is gereed om te worden gebruikt voor het echo van het apparaat.

Nadat u een tegel hebt gemaakt, kunt u uw nieuwe opdracht uitproberen.

## <a name="rules"></a>Regels

Regels zorgen ervoor dat operators voor het bewaken van apparaten in bijna realtime. Regels voor aanroepen automatisch van acties zoals het verzenden van een e-mail wanneer de regel wordt geactiveerd. Een type regel is nu beschikbaar:

- **Telemetrie-regel**, die wordt geactiveerd wanneer de telemetrie van de geselecteerde apparaten een opgegeven drempelwaarde overschrijdt. [Meer informatie over telemetrie regels](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Dashboard

Het dashboard is waar een operator voor informatie over een apparaat kunt gaan. U kunt de tegels op deze pagina om operators te begrijpen hoe het gedrag van het apparaat te toevoegen als een builder. U kunt meerdere dashboardtegels toevoegen aan de sjabloon voor het apparaat. U kunt momenteel zes typen dashboardtegels toevoegen: afbeelding, grafiek, staafdiagram, KPI, instellingen en eigenschappen en labelen.

Bijvoorbeeld, u kunt toevoegen een **instellingen en eigenschappen** tegel om weer te geven van een selectie van de huidige waarden van instellingen en eigenschappen van het selecteren van **sjabloon bewerken** en de tegel uit de bibliotheek:

!['Apparaatdetails configureren' formulier met details voor instellingen en eigenschappen](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Als een operator bekijkt het dashboard, kunnen ze nu deze tegel waarin de eigenschappen en instellingen van het apparaat zien:

![Tabblad met de weergegeven instellingen en eigenschappen voor de tegel 'Dashboard'](./media/howto-set-up-template/dashboardtile.png)

### <a name="add-an-azure-maps-location-in-the-dashboard"></a>De locatie van een Azure-kaarten in het dashboard toevoegen

Als u een locatie-eigenschap eerder in geconfigureerd [maakt u een locatie-eigenschap via Azure Maps](#create-a-location-property-through-azure-maps), kunt u de locatie visualiseren met behulp van een kaart in het apparaatdashboard van uw.

1. Op de **Dashboard** tabblad **sjabloon bewerken**.

   ![Tabblad met de ontwerpmodus 'Dashboard' is ingeschakeld](./media/howto-set-up-template/locationcloudproperty4map.png)

2. Selecteer op het apparaatdashboard **kaart** uit de bibliotheek. 
3. Wijs hieraan een titel in en kies de locatie-eigenschap die u eerder hebt geconfigureerd als onderdeel van de apparaateigenschappen van uw.

   !['Configureren kaart' formulier met details voor de titel en de eigenschappen](./media/howto-set-up-template/locationcloudproperty5map.png)

4. Selecteer **Opslaan**. De tegel met de kaart toont nu de locatie die u hebt geselecteerd. 

   ![Tegel met de geselecteerde locatie worden toegewezen](./media/howto-set-up-template/locationcloudproperty6map.png) 

U kunt het formaat van de kaart naar de gewenste grootte. Als een operator bekijkt het dashboard, kunnen ze nu alle dashboardtegels die u hebt geconfigureerd, inclusief een overzicht van de locatie zien.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een sjabloon van het apparaat in uw Azure IoT Central-toepassing instelt, kunt u het volgende doen:

> [!div class="nextstepaction"]
> [Maak een nieuwe versie van de apparaat-sjabloon](howto-version-devicetemplate.md)
