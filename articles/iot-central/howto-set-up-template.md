---
title: Een sjabloon van het apparaat in een Azure IoT centrale toepassing instellen | Microsoft Docs
description: Informatie over het instellen van een sjabloon van het apparaat met metingen, instellingen, eigenschappen, regels en dashboard.
author: viv-liu
ms.author: viviali
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 0bef8722ee51c916652e6964305f324341052341
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36754549"
---
# <a name="set-up-a-device-template"></a>Een sjabloon van het apparaat instellen

Een apparaat-sjabloon is een blauwdruk die de kenmerken en het gedrag van een type apparaat dat verbinding met een Microsoft Azure IoT centrale toepassing maakt definieert.

Een opbouwfunctie kunt bijvoorbeeld een sjabloon van het apparaat voor een verbonden IoT-ventilator met maken:

- Temperatuur telemetrie meting

- Ventilator motor fout gebeurtenis meting

- Operationele status meting ventilator

- Ventilator snelheidsinstelling

- Locatie-eigenschap

- Regels die waarschuwingen verzenden

- Dashboard waarmee u een alles weergave over het apparaat

Met deze sjabloon apparaat een operator kunt maken en verbinding maken met echte fan apparaten met namen, zoals **ventilator 1** en **ventilator 2**. Alle deze ventilatoren hebben metingen, instellingen en eigenschappen van regels en een dashboard dat gebruikers van uw toepassing kunnen bewaken en beheren.

> [!NOTE]
> Alleen opbouwfuncties en beheerders kunnen maken, bewerken en verwijderen van apparaat-sjablonen. Elke gebruiker kan maken van apparaten in de **apparaat Explorer** pagina via bestaande sjablonen van het apparaat.

## <a name="create-a-new-device-template"></a>Een nieuwe apparaatsjabloon maken

1. Navigeer naar de **toepassing Builder** pagina.

2. Voor het maken van een lege sjabloon kiezen **apparaat sjabloon maken**, en kies vervolgens **aangepaste**.

3. Voer een naam voor de nieuwe sjabloon voor het apparaat en kies **maken**.

    ![De pagina details apparaat](./media/howto-set-up-template/devicedetailspage.png)

4. Nu u gaat de **Apparaatdetails** pagina van een nieuw gesimuleerd apparaat. Een gesimuleerd apparaat wordt automatisch voor u gemaakt wanneer u een nieuwe apparaat-sjabloon maakt. Deze gegevens rapporten en net als bij een echte apparaat kan worden beheerd.

Bekijk de tabbladen worden weergegeven op de **Apparaatdetails** pagina.

## <a name="measurements"></a>Metingen

Metingen zijn de gegevens die afkomstig zijn van uw apparaat. U kunt meerdere metingen toevoegen aan uw sjabloon apparaat overeenkomen met de mogelijkheden van uw apparaat. Telemetrie- en gebeurtenis zijn momenteel de soorten ondersteunde metingen.

- **Telemetrie** metingen zijn de numerieke gegevenspunten uw apparaat worden gedurende een periode worden verzameld en worden vertegenwoordigd een continue stroom. Bijvoorbeeld: temperatuur.
- **Gebeurtenis** metingen zijn de punt in tijd-gegevens die iets significante op het apparaat vertegenwoordigen. Gebeurtenissen hebben die zijn gekoppeld aan deze ernst die het belang van de gebeurtenis vertegenwoordigt. Bijvoorbeeld ventilatoren motor fout
- **Status** metingen vertegenwoordigen de status van het apparaat of de bijbehorende onderdelen gedurende een periode. Bijvoorbeeld ventilatoren modus die kan worden gedefinieerd als bestanden met het besturingssysteem en als de twee mogelijke statussen zijn gestopt.

### <a name="create-a-telemetry-measurement"></a>Maak een meting telemetrie
Als u wilt een nieuwe telemetrie meting toevoegen, klikt u op **+ nieuwe meting** knop waarmee een formulier met opties te selecteren van het type meting wordt geopend. Selecteer **telemetrie** en voer de details op de **telemetrie maken** formulier.

> [!NOTE]
> Wanneer een echte apparaat verbonden, betaalde aandacht op de naam van de meting is wordt het apparaat rapporteert. De naam moet exact overeenkomen met de **veldnaam** van een meting.

U kunt bijvoorbeeld een nieuwe temperatuur telemetrie meting toevoegen:

![Metingen formulier](./media/howto-set-up-template/measurementsform.png)

Nadat u hebt gekozen **opslaan**, wordt de **temperatuur** meting wordt weergegeven in de lijst met metingen en een operator de visualisatie van de temperatuur gegevens van het apparaat verzamelen kunt zien.

![Metingen grafiek](./media/howto-set-up-template/measurementsgraph.png)

### <a name="create-an-event-measurement"></a>Maak een meting gebeurtenis
Als u wilt een nieuwe gebeurtenis meting toevoegen, klikt u op **+ nieuwe meting** knop waarmee een formulier met opties te selecteren van het type meting wordt geopend. Selecteer **gebeurtenis** en voer de details op de **maken gebeurtenis** formulier.

Op dit formulier bieden de **weergavenaam**, **veldnaam**, en de **ernst** van de gebeurtenis. U kunt kiezen uit de drie beschikbare niveaus van de ernst: **fout**, **waarschuwing**, en **informatie**.  

U kunt bijvoorbeeld een nieuwe 'Ventilator Motor fout' gebeurtenis toevoegen.

![Gebeurtenis metingen formulier](./media/howto-set-up-template/eventmeasurementsform.png)

Nadat u hebt gekozen **opslaan**, wordt de **ventilator Motor fout** meting wordt weergegeven in de lijst met metingen en een operator de visualisatie van de gegevens van de gebeurtenis die het verzenden van het apparaat kunt zien.

![Omvang van de gebeurtenis](./media/howto-set-up-template/eventmeasurementschart.png)

Voor aanvullende details over de gebeurtenis bekijken, klikt u op het pictogram gebeurtenis in de grafiek:

![Gebeurtenisdetails metingen](./media/howto-set-up-template/eventmeasurementsdetail.png)


### <a name="create-a-state-measurement"></a>Maak een meting status
Als u wilt een nieuwe status meting toevoegen, klikt u op **+ nieuwe meting** knop waarmee een formulier met opties te selecteren van het type meting wordt geopend. Selecteer **status** en voer de details op de **maken status** formulier.

Op dit formulier bieden de **weergavenaam**, **veldnaam**, en de mogelijke **waarden** van de status. Elke **waarde** kan ook een weergavenaam die wordt gebruikt wanneer de waarde weergegeven op de tabellen en grafieken hebben.

U kunt bijvoorbeeld een nieuwe ventilator-modus staat die twee mogelijke waarden die het apparaat verzenden heeft kan, toevoegen **besturingssysteem** en **gestopt**.

![Status metingen formulier](./media/howto-set-up-template/statemeasurementsform.png)

Nadat u hebt gekozen **opslaan**, wordt de **ventilator modus** status meting wordt weergegeven in de lijst met metingen en de operator de weergave van de statusgegevens van het verzenden van het apparaat kunt zien.

![Metingen Statusgrafiek](./media/howto-set-up-template/statemeasurementschart.png)

Als er binnen een korte tijdsduur te veel gegevenspunten door het apparaat worden verzonden, wordt de statusmeting weergegeven met een andere visualisatie, zoals hieronder weergegeven. Als u op het diagram klikt, worden alle gegevenspunten binnen die periode in chronologische volgorde weergegeven. U kunt ook het tijdsbereik voor een overzicht van de meting in de grafiek getekend afbakenen.

![Details van status metingen](./media/howto-set-up-template/statemeasurementsdetail.png)


## <a name="settings"></a>Instellingen

Instellingen beheren voor een apparaat. Ze inschakelen operators van uw toepassing informatie invoeren voor het apparaat. U kunt meerdere instellingen toevoegen aan de sjabloon voor apparaten die worden weergegeven als tegels op de **instellingen** tabblad voor operators te gebruiken. Er zijn zes soorten instellingen die u kunt toevoegen: getal, tekst, datum, in-of uitschakelen, selectielijst en sectielabel.

> [!NOTE]
> Wanneer een echte apparaat is verbonden, moet u aandacht schenken aan de naam van de instelling voor de apparaat-rapporten. De naam moet exact overeenkomen met de **veldnaam** van een instelling.

Instellingen kunnen worden gebruikt in een van drie statussen. Deze statussen worden gemeld door het apparaat.

- **Gesynchroniseerd**: het apparaat is gewijzigd om de waarde van de instelling weer te geven.

- **In behandeling**: het apparaat is momenteel wijzigen in de waarde van de instelling.

- **Fout**: het apparaat heeft een fout geretourneerd.

U kunt bijvoorbeeld een nieuwe ventilator snelheidsinstelling toevoegen:

![Formulier instellingen](./media/howto-set-up-template/settingsform.png)

Nadat u hebt gekozen **opslaan**, wordt de **ventilatoren snelheid** instelling wordt weergegeven als een tegel en is gereed voor om te worden gebruikt voor het wijzigen van de snelheid van de ventilator van het apparaat.

Nadat u een nieuwe tegel hebt gemaakt, kunt u uw nieuwe instelling uitproberen. Eerst uitschakelen ontwerpmodus aan de bovenkant van het scherm.

![Tegel instellingen](./media/howto-set-up-template/settingstile.png)

## <a name="properties"></a>Eigenschappen

Eigenschappen zijn de metagegevens van apparaten die zijn gekoppeld aan het apparaat, zoals de locatie van het apparaat en het serienummer. U kunt meerdere eigenschappen toevoegen aan uw apparaat-sjabloon als tegels worden weergegeven op de **eigenschappen** tabblad. Een operator kunt de waarden voor eigenschappen opgeven wanneer ze een nieuw apparaat maken en deze waarden op elk gewenst moment kunnen bewerken. Er zijn zes typen eigenschappen die u kunt toevoegen: getal, tekst, datum, in-of uitschakelen, apparaateigenschap en label.

Er zijn twee soorten eigenschappen:

- **Apparaateigenschappen** eigenschappen worden gerapporteerd door het apparaat.
- **Toepassingseigenschappen** zijn opgeslagen in de toepassing alleen eigenschappen. Het apparaat heeft geen informatie over de eigenschappen van toepassing.

> [!NOTE]
> Voor apparaateigenschappen, wanneer een echte apparaat verbonden, betaalde aandacht op de naam van de eigenschap is rapporteert het apparaat. De naam moet exact overeenkomen met de **veldnaam** van de eigenschap. Voor toepassingseigenschappen mag de veldnaam alles wat die u wilt, zolang de naam uniek zijn in de sjabloon van het apparaat is.

U kunt bijvoorbeeld de locatie van het apparaat toevoegen als een nieuwe eigenschap:

![Formulier Eigenschappen](./media/howto-set-up-template/propertiesform.png)

Nadat u hebt gekozen **opslaan**, locatie van het apparaat wordt weergegeven als een tegel:

![Tegel eigenschappen](./media/howto-set-up-template/propertiestile.png)

> [!NOTE]
> Na het maken van een nieuwe tegel, kunt u de waarde van eigenschap. Eerst uitschakelen ontwerpmodus aan de bovenkant van het scherm.

### <a name="create-a-location-property-powered-by-azure-maps"></a>Maak een locatie-eigenschap aangedreven door Azure Maps
U kunt geografische context geven tot uw locatiegegevens in Azure IoT centrale en eventuele breedtegraad en lengtegraad coördinaten van een Straatadres, of gewoon breedtegraad en lengtegraad coördinaten worden toegewezen. Deze mogelijkheid In Azure IoT Central wordt aangedreven door Azure Maps.

Er zijn twee soorten locatie-eigenschappen die u kunt toevoegen:
- **Locatie als de eigenschap van een toepassing** die alleen in de toepassing worden opgeslagen. Het apparaat heeft geen informatie over de eigenschappen van toepassing.
- **Locatie als een apparaateigenschap** die worden gerapporteerd door het apparaat.

#### <a name="adding-location-as-an-application-property"></a>Locatie als de eigenschap van een toepassing toe te voegen. 

Kunt u een locatie-eigenschap bevat als de eigenschap van een toepassing met behulp van Azure in uw Azure IoT centrale toepassing wordt toegewezen. U kunt bijvoorbeeld de installatie-adres van het apparaat toevoegen. 

1. Navigeer naar het tabblad Apparaateigenschap. Zorg ervoor dat de dat ontwerpmodus is ingeschakeld.

   ![Locatie-eigenschap](./media/howto-set-up-template/locationcloudproperty1.png)

2. Klik op de locatie op het tabblad eigenschap.
3. Weergavenaam, veldnaam en de oorspronkelijke waarde van de locatie eventueel configureren. 

   ![Formulier voor locatie-eigenschap](./media/howto-set-up-template/locationcloudproperty2.png)

   Er zijn twee ondersteunde indelingen voor het toevoegen van een locatie:
   - **Locatie als een adres**
   - **Locatie als coördinaten** 

4.  Klik op **Opslaan**. 

    ![Het locatieveld-eigenschap](./media/howto-set-up-template/locationcloudproperty3.png)

Een operator kan nu de locatiewaarde in het veld locatie formulier bijwerken. 

#### <a name="adding-location-as-a-device-property"></a>Locatie toe te voegen als een apparaateigenschap 

U kunt een locatie-eigenschap maken als een apparaateigenschap die is gerapporteerd door het apparaat.
Bijvoorbeeld, wilt u bijhouden van de locatie van het apparaat.

1.  Navigeer naar het tabblad Apparaateigenschap. Zorg ervoor dat de dat ontwerpmodus is ingeschakeld.

2.  Klik op de Apparaateigenschap uit de bibliotheek.

    ![Het locatieveld-eigenschap](./media/howto-set-up-template/locationdeviceproperty1.png)

3.  Weergavenaam, veldnaam, configureren, en kies 'locatie' als een gegevenstype. 

    > [!NOTE]
    > De veldnaam moet exact overeenkomen op de naam van de eigenschap van het apparaat rapporten. 

    ![Het locatieveld-eigenschap](./media/howto-set-up-template/locationdeviceproperty2.png)

    ![Locatie-eigenschap operator weergeven](./media/howto-set-up-template/locationdeviceproperty2.png)

Nu dat u de eigenschap location hebt geconfigureerd, kunt u zich kunt toevoegen van een kaart om de locatie in het Dashboard van het apparaat. Zie hoe [locatie toevoegen Azure kaart in Dashboard](howto-set-up-template.md#add-location-azure-map-in-dashboard).

## <a name="commands"></a>Opdrachten

Opdrachten worden gebruikt om een apparaat extern kunt beheren. Ze inschakelen operators van uw toepassing direct uitvoeren van opdrachten op het apparaat. U kunt meerdere opdrachten toevoegen aan de sjabloon voor apparaten die worden weergegeven als tegels op de **opdrachten** tabblad voor operators te gebruiken. Als de opbouwfunctie voor het apparaat dat hebt u de flexibiliteit voor het definiëren van uw opdracht volgens uw vereisten.

Hoe wordt een **opdracht** anders dan een **instelling**? 

* Instelling: Is een configuratie die u wilt toepassen op een apparaat en u wilt dat het apparaat dat de configuratie bewaard totdat u deze te wijzigen. Bijvoorbeeld, u wilt de temperatuur van de vriezer instellen en u die instelling wilt, zelfs wanneer de vriesvaartuigen opnieuw gestart. 

* Opdracht: U kunt opdrachten onmiddellijk een opdracht wordt uitgevoerd op het apparaat op afstand van IoTCentral, als een apparaat niet is verbonden en vervolgens de opdrachttime-out en mislukt. Bijvoorbeeld, u een apparaat opnieuw opstarten/reboot.  

Opdrachten bij uitvoering kunnen zich in een van drie statussen, afhankelijk van of het apparaat de opdracht ontvangen. 

U kunt bijvoorbeeld een nieuwe echo-opdracht toevoegen:

![Opdrachten formulier](./media/howto-set-up-template/commandsecho.png)

Nadat u hebt gekozen **opslaan**, wordt de **Echo** opdracht wordt weergegeven als een tegel en gereed is voor moeten worden gebruikt voor het apparaat Echo.

Nadat u een nieuwe tegel hebt gemaakt, kunt u proberen uw nieuwe opdracht.

## <a name="rules"></a>Regels

Regels inschakelen operators voor het bewaken van apparaten in bijna realtime. Regels automatisch aanroepen **acties** zoals een e-mailbericht verzenden wanneer de regel wordt geactiveerd. Er is een type regel beschikbaar vandaag:

- **Telemetrie-regel:** een regel telemetrie wordt geactiveerd wanneer de telemetrie van het geselecteerde apparaat een opgegeven drempelwaarde overschrijdt. Meer informatie over [telemetrie regels](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Dashboard

Het dashboard is waar een operator voor informatie over een apparaat kunt gaan. U kunt als een opbouwfunctie tegels toevoegen aan deze pagina die u helpen begrijpen hoe het apparaat is gedragen operators. U kunt meerdere dashboard tegels toevoegen aan de sjabloon voor het apparaat. Er zijn zes typen dashboard tegels die u kunt toevoegen: image en label grafiek, staafdiagram, KPI, instellingen en eigenschappen.

U kunt bijvoorbeeld toevoegen een **instellingen en eigenschappen** tegel om een selectie van de huidige waarden van instellingen en eigenschappen weer te geven:

![Dashboard apparaat formulier](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Als een operator bekijkt het dashboard, kunnen ze nu deze tegel waarin de eigenschappen en instellingen van het apparaat zien:

![Dashboardtegel](./media/howto-set-up-template/dashboardtile.png)

### <a name="add-location-azure-map-in-dashboard"></a>Locatie toevoegen Azure kaart in het Dashboard

Als u een locatie-eigenschap, zoals in de stappen hebt geconfigureerd [maakt u een locatie-eigenschap aangedreven door Azure Maps](howto-set-up-template.md), is het mogelijk voor het visualiseren van de locatie met een kaart rechts in het dashboard van uw apparaat.

1.  Navigeer naar het tabblad Dashboard apparaat. Zorg ervoor dat de dat ontwerpmodus is ingeschakeld.

2.  Selecteer de kaart in de bibliotheek op apparaat-Dashboard. 

    ![Dashboard Azure locatiekaart selecteren](./media/howto-set-up-template/locationcloudproperty4map.png)

3.  Geef een titel en kies de locatie-eigenschap die u eerder hebt geconfigureerd als onderdeel van de eigenschap van uw apparaat.

    ![Dashboardlocatie Azure kaart configureren](./media/howto-set-up-template/locationcloudproperty5map.png)

4.  Opslaan en u ziet de kaart tegel weergeven van de locatie die u hebt geselecteerd. 

    ![Dashboardlocatie Azure kaart visualiseren](./media/howto-set-up-template/locationcloudproperty6map.png) 

    U kunt zich vergroten of verkleinen van de kaart aan de gewenste grootte.

    Als een operator bekijkt het dashboard, zien ze nu deze alle Dashboard tegels die u hebt geconfigureerd met inbegrip van een locatie kaart!

    ![Dashboard locatie Azure kaart Dashboard](./media/howto-set-up-template/locationcloudproperty7map.png) 

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u een sjabloon van het apparaat in uw Azure IoT centrale toepassing instelt, wordt hier de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Maak een nieuwe versie van de apparaat-sjabloon](howto-version-devicetemplate.md)
