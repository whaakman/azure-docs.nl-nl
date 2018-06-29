---
title: Een sjabloon van het apparaat in een Azure IoT centrale toepassing instellen | Microsoft Docs
description: Informatie over het instellen van een sjabloon van het apparaat met metingen, instellingen, eigenschappen, regels en een dashboard.
author: viv-liu
ms.author: viviali
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: f71d4c7dc94fedfd598ab87c51366ba9fb1f184a
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063032"
---
# <a name="set-up-a-device-template"></a>Een sjabloon van het apparaat instellen

Een apparaat-sjabloon is een blauwdruk die de kenmerken en het gedrag van een type apparaat dat verbinding met een Microsoft Azure IoT centrale toepassing maakt definieert.

Een opbouwfunctie kunt bijvoorbeeld een sjabloon van het apparaat voor een IoT verbonden ventilator met maken:

- Temperatuur telemetrie meting

- Ventilator motor fout gebeurtenis meting

- Operationele status meting ventilator

- Ventilator snelheidsinstelling

- Locatie-eigenschap

- Regels die waarschuwingen verzenden

- Dashboard waarmee u een algemeen overzicht van het apparaat

Met deze sjabloon apparaat een operator kunt maken en verbinding maken met echte fan apparaten met namen, zoals **ventilator 1** en **ventilator 2**. Alle deze ventilatoren hebben metingen, instellingen, eigenschappen, regels en een dashboard dat gebruikers van uw toepassing kunnen bewaken en beheren.

> [!NOTE]
> Alleen opbouwfuncties en beheerders kunnen maken, bewerken en verwijderen van apparaat-sjablonen. Elke gebruiker apparaten kunt maken op de **apparaat Explorer** pagina via bestaande sjablonen van het apparaat.

## <a name="create-a-device-template"></a>Een apparaat-sjabloon maken

1. Ga naar de **toepassing Builder** pagina.

2. U maakt een lege sjabloon selecteren **apparaat sjabloon maken**, en selecteer vervolgens **aangepaste**.

3. Voer een naam voor de nieuwe sjabloon voor het apparaat en selecteer **maken**.

   ![De detailpagina apparaat met 'Koelkast' als naam van de sjabloon](./media/howto-set-up-template/devicedetailspage.png)

4. Nu u gaat de **Apparaatdetails** pagina van een nieuw gesimuleerd apparaat. Een gesimuleerd apparaat wordt automatisch voor u gemaakt wanneer u een apparaat-sjabloon maken. Deze gegevens rapporten en net als bij een echte apparaat kan worden beheerd.

Bekijk de tabbladen worden weergegeven op de **Apparaatdetails** pagina.

## <a name="measurements"></a>Metingen

Metingen zijn de gegevens die afkomstig zijn van uw apparaat. U kunt meerdere metingen toevoegen aan uw sjabloon apparaat overeenkomen met de mogelijkheden van uw apparaat.

- **Telemetrie** metingen zijn de numerieke gegevenspunten die het apparaat gedurende een periode worden verzameld. Ze wordt weergegeven als een continue stroom. Een voorbeeld is temperatuur.
- **Gebeurtenis** metingen zijn de punt in tijd-gegevens die verwijst naar iets anders significante op het apparaat. Een urgentieniveau vertegenwoordigt het belang van een gebeurtenis. Een voorbeeld is een motor ventilator-fout.
- **Status** metingen vertegenwoordigen de status van het apparaat of de bijbehorende onderdelen gedurende een periode. Bijvoorbeeld, een ventilator-modus kan worden gedefinieerd als bestanden met **besturingssysteem** en **gestopt** als de twee mogelijke statussen.

### <a name="create-a-telemetry-measurement"></a>Maak een meting telemetrie
Selecteer om een nieuwe telemetrie meting toe de **+ nieuwe meting** knop. Selecteer **telemetrie** als de meting, te typen en de details op de **telemetrie maken** formulier.

> [!NOTE]
> Wanneer een echte apparaat is verbonden, moet u aandacht schenken aan de naam van de meting die het apparaat rapporteert. De naam moet exact overeenkomen met de **veldnaam** vermelding voor een meting.

U kunt bijvoorbeeld een nieuwe temperatuur telemetrie meting toevoegen:

![Formulier met details voor de temperatuurmeting 'Telemetrie maken'](./media/howto-set-up-template/measurementsform.png)

Nadat u hebt geselecteerd **opslaan**, wordt de **temperatuur** meting wordt weergegeven in de lijst met metingen. Een operator ziet de visualisatie van de gegevens temperatuur die het apparaat wordt verzameld.

![Grafiek van de meting](./media/howto-set-up-template/measurementsgraph.png)

### <a name="create-an-event-measurement"></a>Maak een meting gebeurtenis
Selecteer om een nieuwe gebeurtenis meting toe de **+ nieuwe meting** knop. Selecteer **gebeurtenis** als de meting, te typen en de details op de **maken gebeurtenis** formulier.

Geef de **weergavenaam**, **veldnaam**, en **ernst** details voor de gebeurtenis. U kunt kiezen uit de drie beschikbare niveaus van ernst: **fout**, **waarschuwing**, en **informatie**.  

Bijvoorbeeld, u een nieuwe kunt toevoegen **ventilator Motor fout** gebeurtenis.

![Formulier met details voor een ventilator motor gebeurtenis 'Gebeurtenis maken'](./media/howto-set-up-template/eventmeasurementsform.png)

Nadat u hebt geselecteerd **opslaan**, wordt de **ventilator Motor fout** meting wordt weergegeven in de lijst met metingen. Een operator ziet de visualisatie van de gegevens van de gebeurtenis voor het verzenden van het apparaat.

![Gebeurtenis meting grafiek](./media/howto-set-up-template/eventmeasurementschart.png)

Als u meer details over de gebeurtenis, selecteer het pictogram gebeurtenis in het diagram:

![Details voor de gebeurtenis "Ventilator Motor Error"](./media/howto-set-up-template/eventmeasurementsdetail.png)


### <a name="create-a-state-measurement"></a>Maak een meting status
Selecteer om een nieuwe status meting toe de **+ nieuwe meting** knop. Selecteer **status** als de meting, te typen en de details op de **maken status** formulier.

Geef de details voor **weergavenaam**, **veldnaam**, en **waarden** van de status. Elke waarde kan ook een weergavenaam die wordt gebruikt wanneer de waarde wordt weergegeven in de grafieken en tabellen hebben.

Bijvoorbeeld, u een nieuwe kunt toevoegen **ventilator modus** status die twee mogelijke waarden die het apparaat verzenden heeft kunt, **besturingssysteem** en **gestopt**.

!['Bewerkstatus' formulier met details voor de modus ventilator](./media/howto-set-up-template/statemeasurementsform.png)

Nadat u hebt geselecteerd **opslaan**, wordt de **ventilator modus** status meting wordt weergegeven in de lijst met metingen. De operator ziet de visualisatie van de van statusgegevens die het verzenden van het apparaat.

![Status meting grafiek](./media/howto-set-up-template/statemeasurementschart.png)

Als het apparaat te veel gegevenspunten in een kleine duur verzendt, is de meting status wordt weergegeven met een andere visuele zoals weergegeven in de volgende schermafbeelding. Als u op de grafiek klikt, worden alle gegevenspunten binnen deze periode in chronologische volgorde weergegeven. U kunt ook het tijdsbereik voor een overzicht van de meting in de grafiek getekend afbakenen.

![Details voor de meting van de status 'Statische modus' voor de ventilator](./media/howto-set-up-template/statemeasurementsdetail.png)


## <a name="settings"></a>Instellingen

Instellingen beheren voor een apparaat. Ze inschakelen operators van uw toepassing informatie invoeren voor het apparaat. U kunt meerdere instellingen toevoegen aan de sjabloon voor apparaten die worden weergegeven als tegels op de **instellingen** tabblad voor operators te gebruiken. U kunt de zes soorten instellingen toevoegen: getal, tekst, datum, in-of uitschakelen, selectielijst en sectielabel.

> [!NOTE]
> Wanneer een echte apparaat is verbonden, Let op de naam van de instelling die het apparaat rapporteert. De naam moet exact overeenkomen met de **veldnaam** vermelding voor een instelling.

Instellingen kunnen worden gebruikt in een van drie statussen. Het apparaat meldt deze statussen.

- **Gesynchroniseerd**: het apparaat is gewijzigd om de waarde van de instelling weer te geven.

- **In behandeling**: het apparaat is momenteel wijzigen in de waarde van de instelling.

- **Fout**: het apparaat heeft een fout geretourneerd.

U kunt bijvoorbeeld een nieuwe ventilator snelheidsinstelling toevoegen:

!['Het aantal configureren' formulier met details voor snelheid](./media/howto-set-up-template/settingsform.png)

Nadat u hebt geselecteerd **opslaan**, wordt de **ventilatorsnelheid** instelling wordt weergegeven als een tegel en is gereed om te worden gebruikt voor het wijzigen van de snelheid van de ventilator van het apparaat.

Nadat u een tegel hebt gemaakt, kunt u uw nieuwe instelling uitproberen. Eerst uitschakelen ontwerpmodus op de rechterbovenhoek van het scherm.

![Tabblad 'Instellingen' met de schakeloptie 'ontwerpmodus' voor de tegel](./media/howto-set-up-template/settingstile.png)

## <a name="properties"></a>Eigenschappen

Eigenschappen zijn de metagegevens van apparaten die is gekoppeld aan het apparaat, zoals de locatie van het apparaat en het serienummer. U kunt meerdere eigenschappen toevoegen aan de sjabloon voor apparaten die worden weergegeven als tegels op de **eigenschappen** tabblad. Een operator kunt de waarden voor eigenschappen opgeven wanneer ze een apparaat maken en deze waarden op elk gewenst moment kunnen bewerken. U kunt de zes typen eigenschappen toevoegen: getal, tekst, datum, in-of uitschakelen, apparaateigenschap en label.

Er zijn twee soorten eigenschappen:

- **Apparaat** eigenschappen die het apparaat rapporteert.
- **Toepassing** eigenschappen die alleen in de toepassing worden opgeslagen. Het apparaat niet wordt herkend door de toepassingseigenschappen van de.

> [!NOTE]
> Voor apparaateigenschappen, wanneer een echte apparaat is verbonden, Let op de naam van de eigenschap die het apparaat rapporteert. De naam moet exact overeenkomen met de **veldnaam** vermelding voor de eigenschap. Voor toepassingseigenschappen mag de veldnaam alles wat die u wilt, zolang de naam uniek zijn in de sjabloon van het apparaat is.

U kunt bijvoorbeeld de locatie van het apparaat toevoegen als een nieuwe eigenschap:

![Formulier 'Configureren tekst' op het tabblad 'Eigenschappen'](./media/howto-set-up-template/propertiesform.png)

Nadat u hebt geselecteerd **opslaan**, locatie van het apparaat wordt weergegeven als een tegel:

![Locatie-tegel](./media/howto-set-up-template/propertiestile.png)

Nadat u een tegel hebt gemaakt, kunt u de waarde van eigenschap kunt wijzigen. Eerst uitschakelen ontwerpmodus in de rechterbovenhoek van het scherm.

### <a name="create-a-location-property-through-azure-maps"></a>Maak een locatie-eigenschap via Azure Maps
U kunt geografische context geven tot uw locatiegegevens in Azure IoT centrale en eventuele coördinaten breedtegraad en lengtegraad van een adres worden toegewezen. Of u kunt gewoon breedtegraad en lengtegraad kaartcoördinaten. Azure-kaarten kunnen deze mogelijkheid in IoT centraal.

U kunt twee soorten locatie eigenschappen toevoegen:
- **Locatie als de eigenschap van een toepassing**, dat alleen in de toepassing wordt opgeslagen. Het apparaat niet wordt herkend door de toepassingseigenschappen van de.
- **Locatie als een apparaateigenschap**, dat het apparaat rapporteert.

#### <a name="add-location-as-an-application-property"></a>Locatie als de eigenschap van een toepassing toevoegen 
U kunt locatie-eigenschap als een eigenschap van een toepassing maken met behulp van Azure Maps in uw toepassing IoT centraal. U kunt bijvoorbeeld de installatie-adres van het apparaat toevoegen. 

1. Op de **eigenschappen** tabblad, zorg ervoor dat **ontwerpmodus** is **op**.

   ![Tabblad 'Eigenschappen' met ontwerpmodus ingeschakeld](./media/howto-set-up-template/locationcloudproperty1.png)

2. Selecteer in de bibliotheek **locatie**.
3. Configureer **weergavenaam**, **veldnaam**, en (optioneel) **beginwaarde** voor de locatie. 

   ![Formulier met details voor de locatie van "Locatie configureren"](./media/howto-set-up-template/locationcloudproperty2.png)

   Er zijn twee ondersteunde indelingen voor het toevoegen van een locatie:
   - **Locatie als een adres**
   - **Locatie als coördinaten** 

4. Selecteer **Opslaan**. 

   ![De eigenschap Location met installatie-adres toegevoegd](./media/howto-set-up-template/locationcloudproperty3.png)

Een operator kan nu de locatiewaarde in het veld locatie formulier bijwerken. 

#### <a name="add-location-as-a-device-property"></a>Locatie als de apparaateigenschap van een toevoegen 

U kunt een locatie-eigenschap maken als een apparaateigenschap die het apparaat rapporteert. Bijvoorbeeld, als u wilt bijhouden van de locatie van het apparaat:

1. Op de **eigenschappen** tabblad, zorg ervoor dat **ontwerpmodus** is **op**.

   ![Tabblad 'Eigenschappen' met ontwerpmodus ingeschakeld](./media/howto-set-up-template/locationdeviceproperty1.png)

2. Selecteer **Apparaateigenschap** uit de bibliotheek.
3. Configureer de weergavenaam en de veldnaam en selecteer **locatie** als gegevenstype. 

   > [!NOTE]
   > De veldnaam moet exact overeenkomen met de naam van de eigenschap die het apparaat rapporteert. 

   ![Formulier met details voor de locatie 'Apparaateigenschappen configureren'](./media/howto-set-up-template/locationdeviceproperty2.png)

Nu dat u de eigenschap location hebt geconfigureerd, kunt u [toevoegen van een kaart om de locatie in het dashboard apparaat](#add-an-azure-maps-location-in-the-dashboard).

## <a name="commands"></a>Opdrachten

Opdrachten worden gebruikt om een apparaat extern kunt beheren. Ze inschakelen operators van uw toepassing direct uitvoeren van opdrachten op het apparaat. U kunt meerdere opdrachten toevoegen aan de sjabloon voor apparaten die worden weergegeven als tegels op de **opdrachten** tabblad voor operators te gebruiken. Als de opbouwfunctie voor van het apparaat hebt u de flexibiliteit voor het definiëren van opdrachten volgens uw vereisten.

Hoe wordt een opdracht van een instelling? 

* **Instelling**: een is een configuratie die u wilt toepassen op een apparaat, en u wilt dat de configuratie bewaard totdat u het wijzigen van het apparaat. Bijvoorbeeld, u wilt de temperatuur van de vriezer instellen en u wilt dat deze instelling zelfs wanneer de vriesvaartuigen opnieuw opstart. 

* **Opdracht**: U opdrachten kunt onmiddellijk een opdracht wordt uitgevoerd op het apparaat op afstand van IoT centraal. Als een apparaat niet is verbonden, wordt de opdracht een time-out optreedt en mislukt. U wilt bijvoorbeeld een apparaat opnieuw opstarten.  

Wanneer u een opdracht uitvoert, kan deze zich in een van drie statussen, afhankelijk van of het apparaat de opdracht ontvangen. 

Bijvoorbeeld, u een nieuwe kunt toevoegen **Echo** opdracht:

![Formulier met details voor de echo '-opdracht configureren'](./media/howto-set-up-template/commandsecho.png)

Nadat u hebt geselecteerd **opslaan**, wordt de **Echo** opdracht wordt weergegeven als een tegel en is gereed om te worden gebruikt voor het apparaat echo.

Nadat u een tegel hebt gemaakt, kunt u proberen uw nieuwe opdracht.

## <a name="rules"></a>Regels

Regels inschakelen operators voor het bewaken van apparaten in bijna realtime. Regels aanroepen automatisch acties zoals een e-mailbericht verzenden wanneer de regel wordt geactiveerd. Een type regel is vandaag beschikbaar:

- **Telemetrie-regel**, die wordt geactiveerd wanneer de telemetrie van het geselecteerde apparaat een opgegeven drempelwaarde overschrijdt. [Meer informatie over de regels voor telemetrie](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Dashboard

Het dashboard is waar een operator voor informatie over een apparaat kunt gaan. U kunt de tegels op deze pagina om te begrijpen hoe het apparaat is gedragen operators toevoegen als een opbouwfunctie. U kunt meerdere dashboard tegels toevoegen aan de sjabloon voor het apparaat. U kunt de zes soorten dashboard tegels toevoegen: image en label grafiek, staafdiagram, KPI, instellingen en eigenschappen.

U kunt bijvoorbeeld toevoegen een **instellingen en eigenschappen** tegel om een selectie van de huidige waarden van instellingen en eigenschappen weer te geven:

![Formulier met details voor de instellingen en eigenschappen 'Apparaatdetails configureren'](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Als een operator bekijkt het dashboard, kunnen ze nu deze tegel waarin de eigenschappen en instellingen van het apparaat zien:

![Tabblad met de weergegeven instellingen en eigenschappen voor de tegel 'Dashboard'](./media/howto-set-up-template/dashboardtile.png)

### <a name="add-an-azure-maps-location-in-the-dashboard"></a>Een Azure-Maps-locatie toevoegen in het dashboard

Als u een locatie-eigenschap eerder in geconfigureerd [maakt u een locatie-eigenschap via Azure Maps](#create-a-location-property-through-azure-maps), kunt u de locatie visualiseren met behulp van een kaart in het dashboard van uw apparaat.

1. Op de **Dashboard** tabblad, zorg ervoor dat **ontwerpmodus** is **op**.

   ![Tabblad met ontwerpmodus 'Dashboard' ingeschakeld](./media/howto-set-up-template/locationcloudproperty4map.png)

2. Selecteer op het dashboard apparaat **kaart** uit de bibliotheek. 
3. Geef een titel en kies de locatie-eigenschap die u eerder hebt geconfigureerd als onderdeel van de apparaateigenschappen van uw.

   ![Formulier met details voor de titel en de eigenschappen 'Kaart configureren'](./media/howto-set-up-template/locationcloudproperty5map.png)

4. Selecteer **Opslaan**. De tegel kaart toont nu de locatie die u hebt geselecteerd. 

   ![Tegel met de geselecteerde locatie worden toegewezen](./media/howto-set-up-template/locationcloudproperty6map.png) 

U kunt de grootte van de kaart van het gewenste formaat.

Als een operator bekijkt het dashboard, kunnen ze nu alle dashboard tegels die u hebt geconfigureerd, inclusief een locatiekaart zien.

![Tegels op het dashboard](./media/howto-set-up-template/locationcloudproperty7map.png) 

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u een sjabloon van het apparaat in uw Azure IoT centrale toepassing instelt, kunt u het volgende doen:

> [!div class="nextstepaction"]
> [Maak een nieuwe versie van de apparaat-sjabloon](howto-version-devicetemplate.md)
