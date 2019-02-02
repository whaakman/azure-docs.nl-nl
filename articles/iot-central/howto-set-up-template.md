---
title: Een sjabloon van het apparaat in een Azure IoT Central-toepassing instellen | Microsoft Docs
description: Informatie over het instellen van de sjabloon voor een apparaat met metingen, instellingen, eigenschappen, regels en een dashboard.
author: viv-liu
ms.author: viviali
ms.date: 10/26/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: fc18262326a8474cac417b67a37df35d91d75439
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55657736"
---
# <a name="set-up-a-device-template"></a>Een sjabloon van het apparaat instellen

Een apparaat-sjabloon is een blauwdruk die definieert de kenmerken en het gedrag van een type apparaat dat is verbonden met een Azure IoT Central-toepassing.

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

[!INCLUDE [iot-central-experimental-note](../../includes/iot-central-experimental-note.md)]

## <a name="create-a-device-template"></a>Een apparaat-sjabloon maken

1. Ga naar de **toepassing Builder** pagina.

2. Voor het maken van een lege sjabloon selecteert **apparaat-sjabloon maken**, en selecteer vervolgens **aangepaste**.

3. Voer een naam (bijvoorbeeld koelkast-1) voor uw nieuwe apparaat-sjabloon en selecteer **maken**.

   ![Pagina met details met 'Koelkast' als de naam van het apparaat](./media/howto-set-up-template/devicedetailspage.png)

4. Nu bent u op de **Apparaatdetails** pagina van een nieuw gesimuleerd apparaat. Een gesimuleerd apparaat wordt automatisch voor u gemaakt wanneer u een apparaat-sjabloon maken. Deze gegevens rapporten en net als bij een echt apparaat kan worden beheerd.

Nu gaan we kijken elk van de tabbladen op de **Apparaatdetails** pagina.

## <a name="measurements"></a>Metingen

Metingen zijn de gegevens die afkomstig zijn van uw apparaat. U kunt meerdere metingen toevoegen aan uw sjabloon apparaat zodat deze overeenkomen met de mogelijkheden van uw apparaat.

- **Telemetrie** metingen zijn de numerieke gegevenspunten die uw apparaat na verloop van tijd verzamelt. Ze worden weergegeven als een continue stroom. Een voorbeeld is de temperatuur.
- **Gebeurtenis** metingen zijn point-in-time-gegevens die staat voor iets significante op het apparaat. Een ernstniveau vertegenwoordigt het belang van een gebeurtenis. Een voorbeeld is een motor fan-fout.
- **Status** metingen vertegenwoordigt de status van het apparaat of de bijbehorende onderdelen gedurende een bepaalde periode. Bijvoorbeeld, een ventilator-modus kan worden gedefinieerd als **besturingssysteem** en **gestopt** als de twee mogelijke statussen.

### <a name="create-a-telemetry-measurement"></a>Maken van een meting telemetrie
Selecteren om toe te voegen een nieuwe meting met telemetrie, **sjabloon bewerken**, en klik vervolgens op de **+ nieuwe meting** knop. Selecteer **telemetrie** als de meting, te typen en de details op de **maken telemetrie** formulier.

> [!NOTE]
> De veldnamen in de sjabloon van het apparaat moeten overeenkomen met de namen van eigenschappen in de bijbehorende apparaatcode in volgorde voor de meting telemetrie moet worden weergegeven in de toepassing wanneer een echt apparaat is verbonden. Doe hetzelfde bij het configureren van instellingen voor apparaateigenschappen opdrachten, en als u doorgaat met het definiëren van de sjabloon van het apparaat in de volgende secties.

U kunt bijvoorbeeld een nieuwe meting van de temperatuur-telemetrie toevoegen:
| Weergavenaam        | Veldnaam    |  Eenheden    | Min.   |Max.|
| --------------------| ------------- |-----------|-------|---|
| Temperatuur         | temp          |  degC     |  0    |100|

![Formulier met details voor de temperatuurmeting 'Telemetrie maken'](./media/howto-set-up-template/measurementsform.png)

Nadat u hebt geselecteerd **gedaan**, wordt de **temperatuur** meting wordt weergegeven in de lijst met metingen. In enkele ogenblikken ziet u de visualisatie van de temperatuurgegevens die het gesimuleerde apparaat gegenereerd. Wanneer u een apparaat-sjabloon maakt, wordt een gesimuleerde apparaat gegenereerd uit de sjabloon waarmee u het gedrag van uw toepassing testen voordat een fysieke/real-apparaat is verbonden.


> [!NOTE]
  Het gegevenstype van de meting telemetrie is een drijvende-kommagetal zijn.

### <a name="create-an-event-measurement"></a>Een gebeurtenis meting maken
Selecteer om een nieuwe meting van de gebeurtenis toe **sjabloon bewerken**, en klik vervolgens op de **+ nieuwe meting** knop. Selecteer **gebeurtenis** als de meting, te typen en de details op de **gebeurtenis maken** formulier.

Geef de **weergavenaam**, **veldnaam**, en **ernst** details voor de gebeurtenis. U kunt kiezen uit de drie beschikbare niveaus van ernst: **Fout**, **waarschuwing**, en **informatie**.  

Bijvoorbeeld, u kunt toevoegen van een nieuwe **ventilator Motor fout** gebeurtenis.

| Weergavenaam        | Veldnaam    |  Standaardurgentie | 
| --------------------| ------------- |-----------|
| Storing ventilatormotor     | fanmotorerror |  Fout    | 

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


| Weergavenaam | Veldnaam    |  Waarde 1   | Weergavenaam | Waarde 2    |Weergavenaam  | 
| -------------| ------------- |----------- | -------------| -----------| -------------|
| Ventilatormodus     | ventilatormodus       |  1         | Actief    |     0      | Gestopt      |

!['Bewerkstatus' formulier met details voor de ventilator modus](./media/howto-set-up-template/statemeasurementsform.png)

Nadat u hebt geselecteerd **gedaan**, wordt de **ventilator modus** meting van de status wordt weergegeven in de lijst met metingen. De operator ziet de visualisatie van de van statusgegevens die het apparaat verzendt.

![Status meting grafiek](./media/howto-set-up-template/statemeasurementschart.png)

Als het apparaat te veel gegevenspunten in een kleine duur verzendt, is de meting van de status wordt weergegeven met een ander visueel element, zoals wordt weergegeven in de volgende schermafbeelding. Als u op de grafiek klikt, worden alle gegevenspunten binnen deze periode worden in chronologische volgorde weergegeven. U kunt ook het tijdsbereik voor het weergeven van de meting in de grafiek getekend verfijnen.

> [!NOTE]
  Het gegevenstype van de meting van de status is een tekenreeks.

## <a name="settings"></a>Instellingen

Instellingen voor beheren een apparaat. Deze operators van uw toepassing voor invoer naar het apparaat in staat stellen. U kunt meerdere instellingen toevoegen aan de sjabloon voor apparaten die worden weergegeven als tegels op de **instellingen** tabblad voor operators te gebruiken. U kunt veel soorten instellingen toevoegen: getal, tekst, datum, in-/ uitschakelen, selectielijst en sectielabel. 

Instellingen kunnen zich in een van drie statussen. Het apparaat meldt deze statussen.

- **Gesynchroniseerd**: Het apparaat is gewijzigd om de waarde van de instelling weer te geven.

- **In behandeling**: Het apparaat wordt momenteel gewijzigd naar de waarde van de instelling.

- **Fout**: Het apparaat heeft een fout geretourneerd.

U kunt bijvoorbeeld een nieuwe ventilator snelheid-instelling toevoegen door te selecteren **sjabloon bewerken** en in te voeren in de nieuwe **getal** instelling:

| Weergavenaam  | Veldnaam    |  Eenheden  | Aantal decimalen |Oorspronkelijk|
| --------------| ------------- |---------| ---------|---- |
| Snelheid van ventilator     | fanSpeed      | RPM     | 2        | 0   |

!['Aantal configureren' formulier met details voor instellingen](./media/howto-set-up-template/settingsform.png)

Nadat u hebt geselecteerd **opslaan**, wordt de **ventilatorsnelheid** instelling wordt weergegeven als een tegel en is gereed om te worden gebruikt voor het wijzigen van de snelheid van de ventilator van het apparaat.

Nadat u een tegel maakt, selecteert u **gedaan** in de rechterbovenhoek van het scherm. Zodra het echt apparaat is verbonden met de toepassing, wordt de waarde van de instelling gewijzigd in gesynchroniseerd.

![Tabblad 'Instellingen' met de "Ontwerpmodus" switch voor de tegel](./media/howto-set-up-template/settingstile.png)

## <a name="properties"></a>Properties

Eigenschappen zijn de metagegevens van apparaten die is gekoppeld aan het apparaat, zoals de locatie van het apparaat en het serienummer. U kunt meerdere eigenschappen toevoegen aan de sjabloon voor apparaten die worden weergegeven als tegels op de **eigenschappen** tabblad. U kunt veel soorten eigenschappen toevoegen: getal, tekst, datum, in-/ uitschakelen, apparaateigenschappen, label en locatie. Een operator kunt de waarden van eigenschappen opgeven wanneer ze een apparaat maken en ze kunnen deze waarden op elk gewenst moment bewerken. Echter apparaateigenschappen zijn alleen-lezen van het apparaat worden verzonden naar de toepassing en kunnen niet worden gewijzigd door de operator. Wanneer het echt apparaat is verbonden, wordt de eigenschap tegel in de toepassing worden bijgewerkt. 

Er zijn twee categorieën eigenschappen:

- **Apparaat** eigenschappen die het apparaat aan de IoT Central-toepassing rapporteert. Deze zijn alleen-lezen die zijn gerapporteerd door het apparaat en in de toepassing wordt bijgewerkt wanneer de echte apparaat is verbonden. 
- **Toepassing** eigenschappen die alleen in de toepassing worden opgeslagen en kunnen worden bewerkt door de operator. Het apparaat niet wordt herkend door de toepassingseigenschappen van de.

U kunt bijvoorbeeld locatie van het apparaat toevoegen als een nieuwe **tekst** eigenschap (een eigenschap van de toepassing) door te selecteren **sjabloon bewerken** en in te voeren in de nieuwe eigenschap:

| Weergavenaam  | Veldnaam | Voorloopspaties verwijderen  | Volgspaties verwijderen  | Hoofdlettergevoeligheid| Minimumlengte | Maximumlengte |
| --------------| -----------|---------| ---------|---- |----|----|
| Locatie      | loc        | Uit     |  Uit     | Gemengde modus  | 0 | 100|

![Het formulier 'Tekst configureren' op het tabblad 'Eigenschappen'](./media/howto-set-up-template/propertiesform.png)

Nadat u hebt geselecteerd **opslaan**, locatie van het apparaat wordt weergegeven als een tegel:

![Locatie-tegel](./media/howto-set-up-template/propertiestile.png)

Nadat u de tegel hebt gemaakt, kunt u de waarde van de toepassing-eigenschap kunt wijzigen. Selecteer eerst **gedaan** in de rechterbovenhoek van het scherm.

### <a name="create-a-location-property-through-azure-maps"></a>Een locatie-eigenschap via Azure-kaarten maken
U kunt de geografische context geven tot uw locatiegegevens in Azure IoT Central en eventuele coördinaten breedtegraad en lengtegraad van een straatnaam. Of u kunt gewoon breedtegraad en lengtegraad kaartcoördinaten. Azure kaarten kunt deze mogelijkheid in IoT Central.

U kunt twee typen locatie-eigenschappen toevoegen:
- **Locatie als een eigenschap van de toepassing**, dat alleen in de toepassing wordt opgeslagen. Het apparaat niet wordt herkend door de toepassingseigenschappen van de.
- **Locatie als een apparaateigenschap**, dat het apparaat rapporteert aan de toepassing.

#### <a name="add-location-as-an-application-property"></a>Locatie als een eigenschap van de toepassing toevoegen 
U kunt een locatie-eigenschap maken als een eigenschap van de toepassing met behulp van Azure-kaarten in uw IoT Central-toepassing. U kunt bijvoorbeeld de installatie-adres van het apparaat toevoegen. 

1. Op de **eigenschappen** tabblad **sjabloon bewerken**.

   ![Tabblad 'Eigenschappen' met de ontwerpmodus ingeschakeld](./media/howto-set-up-template/locationcloudproperty1.png)

2. Selecteer in de bibliotheek **locatie**.
3. Configureer **weergavenaam**, **veldnaam**, en (optioneel) **beginwaarde** voor de locatie. 

    | Weergavenaam  | Veldnaam | Initiële waarde |
    | --------------| -----------|---------| 
    | Installatie-adres | insta_address | Microsoft, 1 Microsoft Way, Redmond, WA 98052   |

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

    | Weergavenaam  | Veldnaam | Gegevenstype |
    | --------------| -----------|-----------| 
    | Locatie apparaat | deviceLoc| location  |

   > [!NOTE]
   > De veldnamen moeten overeenkomen met de namen van eigenschappen in de bijbehorende apparaatcode

   ![Formulier met details voor de locatie 'Eigenschappen van een apparaat configureren'](./media/howto-set-up-template/locationdeviceproperty2.png)

Zodra het echt apparaat is verbonden, wordt de locatie die u hebt toegevoegd als een apparaateigenschap worden bijgewerkt met de waarde die is verzonden door het apparaat. Het adres van de installatie, de locatie die u hebt toegevoegd als een eigenschap van de toepassing, wordt een bewerkbaar tegel. Nu u de locatie-eigenschap hebt geconfigureerd, kunt u [toevoegen van een kaart voor het visualiseren van de locatie in het apparaatdashboard](#add-an-azure-maps-location-in-the-dashboard).

## <a name="commands"></a>Opdrachten

Opdrachten worden gebruikt om een apparaat extern kunt beheren. Deze operators van uw toepassing onmiddellijk opdrachten uitvoeren op het apparaat in staat stellen. U kunt meerdere opdrachten toevoegen aan de sjabloon voor apparaten die worden weergegeven als tegels op de **opdrachten** tabblad voor operators te gebruiken. Als de opbouwfunctie voor van het apparaat hebt u de flexibiliteit om opdrachten op basis van uw vereisten te definiëren.

Wat is een opdracht verschil met een instelling? 

* **Instellen van**: Een instelling is een configuratie die u wilt toepassen op een apparaat en u wilt dat het apparaat aan dat de configuratie van blijft actief totdat u deze wijzigt. Bijvoorbeeld, u wilt de temperatuur van de vriezer instellen en u wilt dat deze instelling, zelfs wanneer de vriesvaartuigen opnieuw wordt opgestart. 

* **Opdracht**: Opdrachten kunt u direct een opdracht uitvoeren op het apparaat op afstand van IoT Central. Als een apparaat niet is verbonden, wordt de opdracht een time-out optreedt en mislukt. U wilt bijvoorbeeld een apparaat opnieuw start.  


Bijvoorbeeld, u kunt toevoegen van een nieuwe **Echo** opdracht door het selecteren van **sjabloon bewerken**, vervolgens te klikken op **+ nieuwe opdracht**, en in te voeren in de nieuwe opdracht:

| Weergavenaam  | Veldnaam | Standaardtime-out | Gegevenstype |
| --------------| -----------|---------------- | --------- | 
| Opdracht Echo  | echo       |  30             | tekst      |

!['-Opdracht configureren' formulier met details voor echo](./media/howto-set-up-template/commandsecho.png)

Nadat u hebt geselecteerd **opslaan** en **gedaan**, wordt de **Echo** opdracht wordt weergegeven als een tegel en is gereed om te worden gebruikt voor het apparaat echo zodra uw echt apparaat is verbonden. De veldnamen van de opdracht moeten overeenkomen met de namen van eigenschappen in de bijbehorende apparaatcode om opdrachten uit om te worden uitgevoerd.


## <a name="rules"></a>Regels

Regels zorgen ervoor dat operators voor het bewaken van apparaten in bijna realtime. Regels voor aanroepen automatisch van acties zoals het verzenden van een e-mail wanneer de regel wordt geactiveerd. Een type regel is nu beschikbaar:

- **Telemetrie-regel**, die wordt geactiveerd wanneer de telemetrie van de geselecteerde apparaten een opgegeven drempelwaarde overschrijdt. [Meer informatie over telemetrie regels](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Dashboard

Het dashboard is waar een operator voor informatie over een apparaat kunt gaan. U kunt de tegels op deze pagina om operators te begrijpen hoe het gedrag van het apparaat te toevoegen als een builder. U kunt meerdere dashboardtegels toevoegen aan de sjabloon voor het apparaat. U kunt vele typen dashboardtegels, zoals afbeeldingen, lijndiagram, staafdiagram, Key Performance Indicator (KPI), instellingen en eigenschappen toevoegen en een label.

Bijvoorbeeld, u kunt toevoegen een **instellingen en eigenschappen** tegel om weer te geven van een selectie van de huidige waarden van instellingen en eigenschappen van het selecteren van **sjabloon bewerken** en de tegel uit de bibliotheek:

!['Apparaatdetails configureren' formulier met details voor instellingen en eigenschappen](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Als een operator bekijkt het dashboard, kunnen ze nu deze tegel waarin de eigenschappen en instellingen van het apparaat zien:

![Tabblad met de weergegeven instellingen en eigenschappen voor de tegel 'Dashboard'](./media/howto-set-up-template/dashboardtile.png)

### <a name="add-an-azure-maps-location-in-the-dashboard"></a>De locatie van een Azure-kaarten in het dashboard toevoegen

Als u een locatie-eigenschap eerder in geconfigureerd [maakt u een locatie-eigenschap via Azure Maps](#create-a-location-property-through-azure-maps), kunt u de locatie visualiseren met behulp van een kaart in het apparaatdashboard van uw.

1. Op de **Dashboard** tabblad **sjabloon bewerken**.

   ![Tabblad met de ontwerpmodus 'Dashboard' is ingeschakeld](./media/howto-set-up-template/locationcloudproperty4map.png)

2. Selecteer op het apparaatdashboard **kaart** uit de bibliotheek. 
3. Geef een titel. Het onderstaande voorbeeld is de locatie van installatie van de titel, kies de locatie-eigenschap die u eerder hebt geconfigureerd op het tabblad Eigenschappen. In het onderstaande voorbeeld **installatie adres** is geselecteerd.

   !['Configureren kaart' formulier met details voor de titel en de eigenschappen](./media/howto-set-up-template/locationcloudproperty5map.png)

4. Selecteer **Opslaan**. De tegel met de kaart toont nu de locatie die u hebt geselecteerd. 

   ![Tegel met de geselecteerde locatie worden toegewezen](./media/howto-set-up-template/locationcloudproperty6map.png) 

U kunt het formaat van de kaart naar de gewenste grootte. Als een operator bekijkt het dashboard, kunnen ze nu alle dashboardtegels die u hebt geconfigureerd, inclusief een overzicht van de locatie zien.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een sjabloon van het apparaat in uw Azure IoT Central-toepassing instelt, kunt u het volgende doen:

> [!div class="nextstepaction"]
> [Maak een nieuwe versie van de apparaat-sjabloon](howto-version-devicetemplate.md)
