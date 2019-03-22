---
title: Een sjabloon van het apparaat in een Azure IoT Central-toepassing instellen | Microsoft Docs
description: Informatie over het instellen van de sjabloon voor een apparaat met metingen, instellingen, eigenschappen, regels en een dashboard.
author: viv-liu
ms.author: viviali
ms.date: 01/30/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 1306b97ce7fcf2cd429be31bc99d022de53805d6
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58259853"
---
# <a name="set-up-a-device-template-new-ui-design"></a>Instellen van een apparaat-sjabloon (nieuwe gebruikersinterface ontwerp)

Een apparaat-sjabloon is een blauwdruk die definieert de kenmerken en het gedrag van een type apparaat dat is verbonden met een Azure IoT Central-toepassing.

Bijvoorbeeld, kunt een opbouwfunctie voor een apparaat-sjabloon voor een IoT-verbonden ventilator waarvoor a: maken

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

1. Navigeer naar de **Apparaatsjablonen** pagina.

2. Eerst selecteert voor het maken van een sjabloon, **+ nieuw**.

3. Als u wilt snel aan de slag, kiezen uit de bestaande vooraf gemaakte sjablonen. Selecteer anders **aangepaste**, voer een naam in en klikt u op **maken** aan het bouwen van uw eigen sjabloon maken.

   ![Bibliotheek voor sjablonen van apparaat](./media/howto-set-up-template/newtemplate.png)

4. Wanneer u een aangepaste sjabloon maken, ziet u de **Apparaatdetails** pagina voor uw nieuwe apparaat-sjabloon. Een gesimuleerd apparaat IoT Central automatisch gemaakt wanneer u een apparaat-sjabloon maken. Een gesimuleerd apparaat kunt u het gedrag van uw toepassing testen voordat u verbinding maakt met een echt apparaat.

De volgende secties beschrijven elk van de tabbladen op de **apparaat sjabloon** pagina.

## <a name="measurements"></a>Metingen

Metingen zijn de gegevens die afkomstig zijn van uw apparaat. U kunt meerdere metingen toevoegen aan uw sjabloon apparaat zodat deze overeenkomen met de mogelijkheden van uw apparaat.

- **Telemetrie** metingen zijn de numerieke gegevenspunten die uw apparaat na verloop van tijd verzamelt. Ze worden weergegeven als een continue stroom. Een voorbeeld is de temperatuur.
- **Gebeurtenis** metingen zijn point-in-time-gegevens die staat voor iets significante op het apparaat. Een ernstniveau vertegenwoordigt het belang van een gebeurtenis. Een voorbeeld is een motor fan-fout.
- **Status** metingen vertegenwoordigt de status van het apparaat of de bijbehorende onderdelen gedurende een bepaalde periode. Bijvoorbeeld, een ventilator-modus kan worden gedefinieerd als **besturingssysteem** en **gestopt** als de twee mogelijke statussen.

### <a name="create-a-telemetry-measurement"></a>Maken van een meting telemetrie

Selecteren om toe te voegen een nieuwe meting met telemetrie, **+ nieuwe meting**, kiest u **telemetrie** als de meting, te typen en de details in het formulier.

> [!NOTE]
> De veldnamen in de sjabloon van het apparaat moeten overeenkomen met de namen van eigenschappen in de bijbehorende apparaatcode in volgorde voor de meting telemetrie moet worden weergegeven in de toepassing wanneer een echt apparaat is verbonden. Doe hetzelfde bij het configureren van instellingen voor apparaateigenschappen opdrachten, en als u doorgaat met het definiëren van de sjabloon van het apparaat in de volgende secties.

U kunt bijvoorbeeld een nieuwe meting van de temperatuur-telemetrie toevoegen:

| Weergavenaam        | Veldnaam    |  Eenheden    | Min.   |Max.|
| --------------------| ------------- |-----------|-------|---|
| Temperatuur         | temp          |  degC     |  0    |100|

![Formulier met details voor de temperatuurmeting 'Telemetrie maken'](./media/howto-set-up-template/measurementsform.png)

Nadat u hebt geselecteerd **opslaan**, wordt de **temperatuur** meting wordt weergegeven in de lijst met metingen. In enkele ogenblikken ziet u de visualisatie van de temperatuurgegevens van het gesimuleerde apparaat.

Bij het weergeven van telemetrie, kunt u kiezen uit de volgende opties voor aggregatie: De gemiddelde, Minimum, Maximum, som en aantal. **Gemiddelde** is geselecteerd als de standaardaggregatie op de grafiek. 

> [!NOTE]
> Het gegevenstype van de meting telemetrie is een drijvende-kommagetal zijn.

### <a name="create-an-event-measurement"></a>Een gebeurtenis meting maken

Selecteer om een nieuwe meting van de gebeurtenis toe **+ nieuwe meting** en selecteer **gebeurtenis** als het type meting. Voer de details voor de **gebeurtenis maken** formulier.

Geef de **weergavenaam**, **veldnaam**, en **ernst** details voor de gebeurtenis. U kunt kiezen uit de drie beschikbare niveaus van ernst: **Fout**, **waarschuwing**, en **informatie**.

Bijvoorbeeld, u kunt toevoegen van een nieuwe **ventilator Motor fout** gebeurtenis.

| Weergavenaam        | Veldnaam    |  Standaardurgentie |
| --------------------| ------------- |-----------|
| Storing ventilatormotor     | fanmotorerror |  Fout    |

!['Gebeurtenis maken' formulier met details voor een motor gebeurtenis ventilator](./media/howto-set-up-template/eventmeasurementsform.png)

Nadat u hebt geselecteerd **opslaan**, wordt de **ventilator Motor fout** meting wordt weergegeven in de lijst met metingen. In enkele ogenblikken ziet u de visualisatie van de gegevens van de gebeurtenis van het gesimuleerde apparaat.

Als u wilt meer informatie over een gebeurtenis bekijken, selecteert u het pictogram van de gebeurtenis in de grafiek:

![Details voor de gebeurtenis "Ventilator Motor Error"](./media/howto-set-up-template/eventmeasurementsdetail.png)

> [!NOTE]
> Het gegevenstype van de meting van de gebeurtenis is een tekenreeks.

### <a name="create-a-state-measurement"></a>Maken van een meting staat

Selecteren om toe te voegen een nieuwe meting van de status, de **+ nieuwe meting** en selecteer **status** als het type meting. Voer de details voor de **maken status** formulier.

Geef de details voor **weergavenaam**, **veldnaam**, en **waarden** van de status. Elke waarde kan ook een weergavenaam die wordt gebruikt als de waarde wordt weergegeven in de grafieken en tabellen hebben.

Bijvoorbeeld, u kunt toevoegen van een nieuwe **ventilator modus** status waaraan twee mogelijke waarden die het apparaat verzenden kan, **besturingssysteem** en **gestopt**.

| Weergavenaam | Veldnaam    |  Waarde 1   | Weergavenaam | Waarde 2    |Weergavenaam  | 
| -------------| ------------- |----------- | -------------| -----------| -------------|
| Ventilatormodus     | ventilatormodus       |  1         | Actief    |     0      | Gestopt      |

!['Bewerkstatus' formulier met details voor de ventilator modus](./media/howto-set-up-template/statemeasurementsform.png)

Nadat u hebt geselecteerd **opslaan**, wordt de **ventilator modus** meting van de status wordt weergegeven in de lijst met metingen. In een korte tijd wordt u door de visualisatie van de statusgegevens van het gesimuleerde apparaat te bekijken.

Als het apparaat te veel gegevenspunten in een kleine duur verzendt, wordt de meting van de status weergegeven met een ander visueel element. Selecteer de grafiek om weer te geven van alle gegevenspunten in die periode in chronologische volgorde. U kunt ook het tijdsbereik voor het weergeven van de meting in de grafiek getekend verfijnen.

> [!NOTE]
> Het gegevenstype van de meting van de status is een tekenreeks.

## <a name="settings"></a>Instellingen

Instellingen voor beheren een apparaat. Deze operators voor invoer naar het apparaat in staat stellen. U kunt meerdere instellingen toevoegen aan de sjabloon voor apparaten die worden weergegeven als tegels op de **instellingen** tabblad voor operators te gebruiken. U kunt veel soorten instellingen toevoegen: getal, tekst, datum, in-/ uitschakelen, selectielijst en sectielabel.

Instellingen kunnen zich in een van drie statussen. Het apparaat meldt deze statussen.

- **Gesynchroniseerd**: Het apparaat is gewijzigd om de waarde van de instelling weer te geven.

- **In behandeling**: Het apparaat wordt momenteel gewijzigd naar de waarde van de instelling.

- **Fout**: Het apparaat heeft een fout geretourneerd.

U kunt bijvoorbeeld een nieuwe ventilator snelheid-instelling toevoegen door te selecteren **instellingen** en in te voeren in de nieuwe **getal** instelling:

| Weergavenaam  | Veldnaam    |  Eenheden  | Aantal decimalen |Oorspronkelijk|
| --------------| ------------- |---------| ---------|---- |
| Snelheid van ventilator     | fanSpeed      | RPM     | 2        | 0   |

!['Aantal configureren' formulier met details voor instellingen](./media/howto-set-up-template/settingsform.png)

Nadat u hebt geselecteerd **opslaan**, wordt de **ventilatorsnelheid** instelling wordt weergegeven als een tegel. Een operator de instelling te gebruiken in de **Device Explorer** pagina om te wijzigen van de snelheid van de ventilator van het apparaat.

## <a name="properties"></a>Properties

Eigenschappen zijn metagegevens die is gekoppeld aan het apparaat, zoals de locatie van het apparaat en het serienummer. Meerdere eigenschappen toevoegen aan uw apparaat-sjabloon die worden weergegeven als tegels op de **eigenschappen** tabblad. Een eigenschap kan een type zoals getal, tekst, datum, in-/ uitschakelen, apparaateigenschappen, label of locatie hebben. Een operator kunt de waarden van eigenschappen opgeven wanneer ze een apparaat maken en ze kunnen deze waarden op elk gewenst moment bewerken. Apparaateigenschappen zijn alleen-lezen en naar de toepassing van het apparaat worden verzonden. Een operator kan eigenschappen van een apparaat niet wijzigen. Wanneer een echt apparaat verbinding maakt, is de eigenschap tegel-updates in de toepassing.

Er zijn twee categorieën eigenschappen:

- _Apparaateigenschappen_ die het apparaat rapporteert aan de IoT Central-toepassing. Apparaateigenschappen zijn alleen-lezen die zijn gerapporteerd door het apparaat en in de toepassing worden bijgewerkt wanneer een echt apparaat is verbonden.
- _Toepassingseigenschappen_ die worden opgeslagen in de toepassing en kan worden bewerkt door de operator. Het apparaat niet wordt herkend door de toepassingseigenschappen van de.

U kunt bijvoorbeeld de laatste onderhouden datum voor het apparaat toevoegen als een nieuwe **datum** eigenschap (een eigenschap van de toepassing) op de **eigenschappen** tabblad:

| Weergavenaam  | Veldnaam | Initiële waarde   |
| --------------| -----------|-----------------|
| Laatste servicebeurt      | lastServiced        | 01/29/2019     |

![Het formulier 'Laatste Services configureren' op het tabblad 'Eigenschappen'](./media/howto-set-up-template/propertiesform.png)

Nadat u hebt geselecteerd **opslaan**, de laatste datum onderhouden voor het apparaat wordt weergegeven als een tegel.

Nadat u de tegel hebt gemaakt, kunt u de waarde voor de toepassing van een eigenschap in de **Device Explorer**.

### <a name="create-a-location-property-through-azure-maps"></a>Een locatie-eigenschap via Azure-kaarten maken

U kunt de geografische context geven tot uw locatiegegevens in Azure IoT Central en eventuele coördinaten breedtegraad en lengtegraad van een straatnaam. Of u kunt toewijzen, breedtegraad en lengtegraad coördinaten. Azure kaarten kunt deze mogelijkheid in IoT Central.

U kunt twee typen locatie-eigenschappen toevoegen:

- **Locatie als een eigenschap van de toepassing**, dat wordt opgeslagen in de toepassing. Het apparaat niet wordt herkend door de toepassingseigenschappen van de.
- **Locatie als een apparaateigenschap**, dat het apparaat rapporteert aan de toepassing.

#### <a name="add-location-as-an-application-property"></a>Locatie als een eigenschap van de toepassing toevoegen

U kunt een locatie-eigenschap maken als een eigenschap van de toepassing met behulp van Azure-kaarten in uw IoT Central-toepassing. U kunt bijvoorbeeld de installatie-adres van het apparaat toevoegen:

1. Navigeer naar de **eigenschappen** tabblad.

2. Selecteer in de bibliotheek **locatie**.

3. Configureer **weergavenaam**, **veldnaam**, en (optioneel) **beginwaarde** voor de locatie.

    | Weergavenaam  | Veldnaam | Initiële waarde |
    | --------------| -----------|---------| 
    | Installatie-adres | installAddress | Microsoft, 1 Microsoft Way, Redmond, WA 98052   |

   ![Formulier met details voor de locatie van "Locatie configureren"](./media/howto-set-up-template/locationcloudproperty2.png)

   Er zijn twee ondersteunde indelingen voor het toevoegen van een locatie:
   - **Locatie als een adres**
   - **Locatie als coördinaten**

4. Selecteer **Opslaan**. Een operator kan werk de locatiewaarde in de **Device Explorer**.

#### <a name="add-location-as-a-device-property"></a>Locatie als de apparaateigenschap van een toevoegen

U kunt een locatie-eigenschap maken als een apparaateigenschap die het apparaat rapporteert. Bijvoorbeeld, als u wilt bijhouden van de locatie van het apparaat:

1. Navigeer naar de **eigenschappen** tabblad.

2. Selecteer **Apparaateigenschap** uit de bibliotheek.

3. Configureer de weergavenaam en de veldnaam van het en selecteer **locatie** als het type:

    | Weergavenaam  | Veldnaam | Gegevenstype |
    | --------------| -----------|-----------|
    | Locatie apparaat | deviceLocation | location  |

   > [!NOTE]
   > De veldnamen moeten overeenkomen met de namen van eigenschappen in de bijbehorende apparaatcode

   ![Formulier met details voor de locatie 'Eigenschappen van een apparaat configureren'](./media/howto-set-up-template/locationdeviceproperty2.png)

Zodra het echt apparaat is verbonden, wordt de locatie die u hebt toegevoegd als een apparaateigenschap wordt bijgewerkt met de waarde die is verzonden door het apparaat. Nu u de locatie-eigenschap hebt geconfigureerd, kunt u [toevoegen van een kaart voor het visualiseren van de locatie in het apparaatdashboard](#add-an-azure-maps-location-in-the-dashboard).

## <a name="commands"></a>Opdrachten

Opdrachten worden gebruikt om een apparaat extern kunt beheren. Deze operators opdrachten uitvoeren op het apparaat in staat stellen. U kunt meerdere opdrachten toevoegen aan de sjabloon voor apparaten die worden weergegeven als tegels op de **opdrachten** tabblad voor operators te gebruiken. Als de opbouwfunctie voor van het apparaat hebt u de flexibiliteit om opdrachten op basis van uw vereisten te definiëren.

Wat is een opdracht verschil met een instelling?

* **Instellen van**: Een instelling is een configuratie die u wilt toepassen op een apparaat. Wilt u het apparaat dat de configuratie van blijft actief totdat u deze wijzigt. Bijvoorbeeld, u wilt de temperatuur van de vriezer instellen en u wilt dat deze instelling, zelfs wanneer de vriesvaartuigen opnieuw wordt opgestart.

* **Opdracht**: Opdrachten kunt u direct een opdracht uitvoeren op het apparaat op afstand van IoT Central. Als een apparaat niet is verbonden, wordt de opdracht een time-out optreedt en mislukt. U wilt bijvoorbeeld een apparaat opnieuw start.

Bijvoorbeeld, u kunt toevoegen van een nieuwe **Echo** opdracht door het selecteren van de **opdrachten** tabblad en selecteer vervolgens **+ nieuwe opdracht**, en de opdrachtdetails van de nieuwe in te voeren:

| Weergavenaam  | Veldnaam | Standaardtime-out | Gegevenstype |
| --------------| -----------|---------------- | --------- |
| Opdracht Echo  | echo       |  30             | tekst      |

!['-Opdracht configureren' formulier met details voor echo](./media/howto-set-up-template/commandsecho.png)

Nadat u hebt geselecteerd **opslaan**, wordt de **Echo** opdracht wordt weergegeven als een tegel en is gereed om te worden gebruikt vanuit de **Device Explorer** wanneer uw echt apparaat verbinding maakt. De veldnamen van de opdracht moeten overeenkomen met de namen van eigenschappen in de bijbehorende apparaatcode om opdrachten uit om te worden uitgevoerd.

## <a name="rules"></a>Regels

Regels zorgen ervoor dat operators voor het bewaken van apparaten in bijna realtime. Regels voor aanroepen automatisch van acties zoals het verzenden van een e-mail wanneer de regel wordt geactiveerd. Een type regel is nu beschikbaar:

- **Telemetrie-regel**, die wordt geactiveerd wanneer de telemetrie van de geselecteerde apparaten een opgegeven drempelwaarde overschrijdt. [Meer informatie over telemetrie regels](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Dashboard

Het dashboard is waar een operator voor informatie over een apparaat kunt gaan. U kunt de tegels op deze pagina om operators te begrijpen hoe het gedrag van het apparaat te toevoegen als een builder. U kunt meerdere dashboardtegels toevoegen aan de sjabloon voor het apparaat. U kunt vele typen dashboardtegels, zoals afbeeldingen, lijndiagram, staafdiagram, prestatie-indicator (KPI), instellingen en eigenschappen toevoegen en een label.

Bijvoorbeeld, u kunt toevoegen een **instellingen en eigenschappen** tegel om weer te geven van een selectie van de huidige waarden van instellingen en eigenschappen van het selecteren van de **Dashboard** tabblad en de tegel uit de bibliotheek:

!['Apparaatdetails configureren' formulier met details voor instellingen en eigenschappen](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Nu wanneer een operator het dashboard in bekijkt de **Device Explorer**, zien ze de tegel.

### <a name="add-an-azure-maps-location-in-the-dashboard"></a>De locatie van een Azure-kaarten in het dashboard toevoegen

Als u een locatie-eigenschap hebt geconfigureerd, kunt u de locatie visualiseren met behulp van een kaart in het apparaatdashboard van uw.

1. Navigeer naar de **Dashboard** tabblad.

1. Selecteer op het apparaatdashboard **kaart** uit de bibliotheek.

1. Geef de kaart een titel. Het volgende voorbeeld heeft de titel **installatielocatie**. Kies de locatie-eigenschap die u eerder hebt geconfigureerd op de **eigenschappen** tabblad. In het volgende voorbeeld **installatie adres** is geselecteerd.

   !['Configureren kaart' formulier met details voor de titel en de eigenschappen](./media/howto-set-up-template/locationcloudproperty5map.png)

4. Selecteer **Opslaan**. De tegel met de kaart toont nu de locatie die u hebt geselecteerd.

U kunt het formaat van de kaart naar de gewenste grootte. Nu wanneer een operator het dashboard in bekijkt de **Device Explorer**, alle dashboard-tegels die u hebt geconfigureerd, met inbegrip van een locatiekaart zichtbaar zijn.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een sjabloon van het apparaat in uw Azure IoT Central-toepassing instelt, kunt u het volgende doen:

> [!div class="nextstepaction"]
> [Maak een nieuwe versie van de apparaat-sjabloon](howto-version-devicetemplate.md)