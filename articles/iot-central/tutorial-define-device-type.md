---
title: Een nieuw apparaattype definiëren in Azure IoT Central | Microsoft Docs
description: Deze zelfstudie laat zien hoe u als bouwer een nieuw apparaattype kunt definiëren in uw Azure IoT Central-toepassing. U definieert de telemetrie, status, eigenschappen en instellingen voor uw type.
author: dominicbetts
ms.author: dobett
ms.date: 01/28/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: e4c5942dfba62a2c869e4eeceb9018fc926d9de9
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58259535"
---
# <a name="tutorial-define-a-new-device-type-in-your-azure-iot-central-application-new-ui-design"></a>Zelfstudie: Een nieuw apparaattype definiëren in uw Azure IoT Central-toepassing (nieuwe gebruikersinterface)

Deze zelfstudie laat zien hoe u als bouwer een apparaatsjabloon kunt gebruiken om een nieuw apparaattype te definiëren in uw Microsoft Azure IoT Central-toepassing. Een apparaatsjabloon definieert de telemetrie, status, eigenschappen en instellingen voor uw apparaattype.

Om u in staat te stellen uw toepassing te testen voordat u een echt apparaat aansluit, genereert IoT Central een gesimuleerd apparaat op basis van de apparaatsjabloon wanneer u deze maakt.

In deze zelfstudie maakt u een apparaatsjabloon voor een **aangesloten airconditioner**. Een aangesloten airconditioningapparaat:

* Verzendt telemetrie zoals temperatuur en luchtvochtigheid.
* Rapporteert zijn status, bijvoorbeeld of het apparaat aan of uit is.
* Heeft apparaateigenschappen zoals de firmwareversie en het serienummer.
* Heeft instellingen zoals de doeltemperatuur.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een nieuwe apparaatsjabloon maken
> * Telemetrie toevoegen aan uw apparaat
> * Gesimuleerde telemetrie bekijken
> * Gebeurtenismeting definiëren
> * Gesimuleerde gebeurtenissen bekijken
> * Statusmeting definiëren
> * Gesimuleerde status bekijken
> * Instellingen en eigenschappen gebruiken
> * Opdrachten gebruiken
> * Het gesimuleerde apparaat in het dashboard bekijken

## <a name="prerequisites"></a>Vereisten

U hebt een Azure IoT Central-toepassing nodig om deze zelfstudie te voltooien. Als u de snelstart [Een Azure IoT Central-toepassing maken](quick-deploy-iot-central.md) hebt voltooid, kunt u de toepassing die u in de snelstart hebt gemaakt, weer gebruiken. Voer anders de volgende stappen uit om een lege Azure IoT Central-toepassing te maken:

1. Navigeer naar de pagina [​​Toepassingsbeheer ](https://aka.ms/iotcentral) van Azure IoT Central.

2. Voer het e-mailadres en wachtwoord in dat u gebruikt om toegang te krijgen tot uw Azure-abonnement:

    ![Voer uw organisatieaccount in](./media/tutorial-define-device-type/sign-in.png)

3. Als u wilt beginnen met het maken van een nieuwe Azure IoT Central-toepassing, selecteert u **nieuwe toepassing**:

    ![Pagina Toepassingsbeheer van Azure IoT Central](./media/tutorial-define-device-type/iotcentralhome.png)

4. Een nieuwe Azure IoT Central-toepassing maken:
    
   * Kies **Proef**. U hebt geen Azure-abonnement nodig om een proeftoepassing te maken.
    
      Zie de [snelstart over het maken van een toepassing](quick-deploy-iot-central.md)voor meer informatie over directory's en abonnementen.
    
   * Kies **Aangepaste toepassing**.
    
   * U kunt desgewenst een beschrijvende naam voor de toepassing kiezen, zoals **Contoso-airconditioners**. Azure IoT Central genereert een uniek URL-voorvoegsel voor u. U kunt dit URL-voorvoegsel wijzigen in iets dat gemakkelijker te onthouden is.
    
   * Selecteer **Maken**.

     ![Pagina Toepassing maken van Azure IoT Central](./media/tutorial-define-device-type/iotcentralcreate.png)

     Zie voor meer informatie de [snelstart over het maken van een toepassing](quick-deploy-iot-central.md).

## <a name="create-a-device-template"></a>Een apparaat-sjabloon maken

Als bouwer kunt u de apparaatsjablonen in uw toepassing maken en bewerken. Wanneer u een apparaatsjabloon maakt, genereert Azure IoT Central een gesimuleerd apparaat op basis van de sjabloon. Het gesimuleerde apparaat genereert telemetrie waarmee u het gedrag van uw toepassing kunt testen voordat u een echt apparaat aansluit.

Als u een nieuwe apparaatsjabloon aan uw toepassing wilt toevoegen, gaat u naar de pagina **Apparaatsjablonen**. Om te doen, dus selecteer de **Apparaatsjablonen** in het navigatiemenu links.

![De pagina Apparaatsjablonen](./media/tutorial-define-device-type/devicetemplates.png)

## <a name="add-a-device-template"></a>Een apparaatsjabloon toevoegen

De volgende stappen laten zien hoe u een nieuwe apparaatsjabloon, **Verbonden airconditioner**, maakt voor apparaten die temperatuurtelemetrie naar uw toepassing verzenden:

1. Op de **Apparaatsjablonen** weergeeft, schakelt **+ nieuw**:

    ![De pagina Apparaatsjablonen, Apparaatsjabloon maken](./media/tutorial-define-device-type/newtemplate.png)

2. De pagina ziet u de sjablonen die u kunt kiezen uit.

    ![Bibliotheek voor sjablonen van apparaat](./media/tutorial-define-device-type/devicetemplatelibrary.png)

3. Selecteer de **aangepaste**, voer **airconditioner verbonden** als de naam van uw apparaat sjabloon en selecteer vervolgens **maken**. U kunt ook een afbeelding van uw apparaat uploaden die zichtbaar is voor operators in de apparatenverkenner:

    ![Aangepast apparaat](./media/tutorial-define-device-type/createcustomdevice.png)

4. Ga naar het tabblad **Metingen** van de apparaatsjabloon **Aangesloten airconditioner**, waar u de telemetrie definieert. Elke apparaatsjabloon die u definieert, heeft afzonderlijke tabbladen waarmee u het volgende kunt doen:

   * De _metingen_ opgeven, zoals telemetrie, gebeurtenis en status, die door het apparaat worden verzonden.

   * De _instellingen_ definiëren die worden gebruikt om het apparaat te beheren.

   * De _eigenschappen_ definiëren die de metagegevens van het apparaat vormen.

   * De _opdrachten_ definiëren die rechtstreeks op het apparaat moeten worden uitgevoerd.

   * De _regels_ definiëren die u wilt koppelen aan het apparaat.

   * Het _dashboard_ van het apparaat aanpassen voor uw operators.

     ![Airconditionermetingen](./media/tutorial-define-device-type/airconmeasurements.png)

     > [!NOTE]
     > Als u de naam van de sjabloon van het apparaat, selecteert u de naam van de sjabloon aan de bovenkant van de pagina.

5. Als u wilt toevoegen de meting van de telemetrie temperatuur, selecteer **+ nieuwe meting**. Kies vervolgens **Telemetrie** als het type meting:

    ![Metingen aangesloten airconditioner](./media/tutorial-define-device-type/airconmeasurementsnew.png)

6. Elk type telemetrie dat u definieert voor een apparaatsjabloon bevat [configuratieopties](howto-set-up-template.md) zoals:

   * Weergaveopties.

   * Details van de telemetrie.

   * Simulatieparameters.

     Gebruik de informatie in de volgende tabel om uw **Temperatuur**-telemetrie te configureren:

     | Instelling              | Value         |
     | -------------------- | -----------   |
     | Weergavenaam         | Temperatuur   |
     | Veldnaam           | temperatuur   |
     | Eenheden                | F             |
     | Min.                  | 60            |
     | Max.                  | 110           |
     | Aantal decimalen       | 0             |

     U kunt ook een kleur kiezen voor de telemetrieweergave. Selecteren om op te slaan in de definitie van de telemetrie, **opslaan**:

     ![Temperatuursimulatie configureren](./media/tutorial-define-device-type/temperaturesimulation.png)

7. Na enige tijd ziet u op het tabblad **Metingen** een grafiek van de temperatuurtelemetrie van uw gesimuleerde aangesloten airconditioningapparaat. Gebruik de besturingselementen om de zichtbaarheid en aggregatie te beheren of de telemetriedefinitie te bewerken:
 
    > [!NOTE]
    > Voor telemetrie, **gemiddelde** als het standaard aggregatietype is ingesteld. 

    ![Temperatuursimulatie bekijken](./media/tutorial-define-device-type/viewsimulation.png)

8. U kunt het diagram ook aanpassen met behulp van de besturingselementen **Lijn** , **Gestapeld**en **Tijdsbereik bewerken**:

    ![De grafiek aanpassen](./media/tutorial-define-device-type/customizechart.png)

## <a name="add-an-event-measurement"></a>Een gebeurtenismeting toevoegen

Gebruik gebeurtenissen voor het definiëren van point-in-time-gegevens die door het apparaat verzonden wanneer er een gebeurtenis plaatsvindt, zoals een fout of een storing van een onderdeel. Azure IoT Central kan apparaatgebeurtenissen simuleren, zodat u het gedrag van uw toepassing kunt testen voordat u een echt apparaat aansluit. U definieert gebeurtenismetingen voor uw apparaatsjabloon in de weergave **Metingen**.

1. Om toe te voegen de **ventilator Motor fout** gebeurtenis meting, selecteer **+ nieuwe meting**. Kies vervolgens **Gebeurtenis** als het type meting:

    ![Metingen aangesloten airconditioner](./media/tutorial-define-device-type/eventnew.png)

2. Elk type gebeurtenis dat u definieert voor een apparaatsjabloon bevat [configuratieopties](howto-set-up-template.md) zoals:

   * Weergavenaam.

   * Veldnaam.

   * Ernst.

     Gebruik de informatie in de volgende tabel om de gebeurtenis **Storing ventilatormotor** te configureren:

     | Instelling              | Value             |
     | -------------------- | -----------       |
     | Weergavenaam         | Storing ventilatormotor   |
     | Veldnaam           | storingventilatormotor       |
     | Severity             | Fout             |

     Selecteren om op te slaan in de gebeurtenisdefinitie van de, **opslaan**:

     ![Gebeurtenismeting configureren](./media/tutorial-define-device-type/eventconfiguration.png)

3. Na enige tijd ziet u op het tabblad **Metingen** een grafiek van de gebeurtenissen die willekeurig door uw gesimuleerde aangesloten airconditioningapparaat zijn gegenereerd. Gebruik de besturingselementen om de zichtbaarheid te beheren of de gebeurtenisdefinitie te bewerken:

    ![Gebeurtenissimulatie bekijken](./media/tutorial-define-device-type/eventview.png)

1. Selecteer de gebeurtenis in de grafiek voor aanvullende informatie over de gebeurtenis:

    ![Gebeurtenisdetails bekijken](./media/tutorial-define-device-type/eventviewdetail.png)

## <a name="define-a-state-measurement"></a>Een statusmeting definiëren

U kunt status gebruiken om de status van het apparaat of een onderdeel ervan in de loop van de tijd te definiëren en te visualiseren. Azure IoT Central kan een apparaatstatus simuleren, zodat u het gedrag van uw toepassing kunt testen voordat u een echt apparaat aansluit. U definieert statusmetingen voor uw apparaattype in de weergave **Metingen**.

1. Om toe te voegen een **ventilator modus** meting staat, selecteer **+ nieuwe meting**. Kies vervolgens **Status** als het type meting:

    ![Statusmetingen aangesloten airconditioner](./media/tutorial-define-device-type/statenew.png)

2. Elk type status dat u definieert voor een apparaatsjabloon bevat [configuratieopties](howto-set-up-template.md) zoals:

   * Weergavenaam.

   * Veldnaam.

   * Waarden met optionele weergavelabels.

   * Kleur voor elke waarde.

     Gebruik de informatie in de volgende tabel om de status **Ventilatormodus** te configureren:

     | Instelling              | Value             |
     | -------------------- | -----------       |
     | Weergavenaam         | Ventilatormodus          |
     | Veldnaam           | ventilatormodus           |
     | Value                | 1                 |
     | Weergavelabel        | Actief         |
     | Value                | 0                 |
     | Weergavelabel        | Gestopt           |

     Selecteren om op te slaan in de definitie van de meting staat, **opslaan**:

     ![Statusmeting configureren](./media/tutorial-define-device-type/stateconfiguration.png)

3. Na enige tijd ziet u op het tabblad **Metingen** een grafiek van de statuswaarden die willekeurig door uw gesimuleerde aangesloten airconditioningapparaat zijn gegenereerd. Gebruik de besturingselementen om de zichtbaarheid te beheren of de statusdefinitie te bewerken:

    ![Statussimulatie bekijken](./media/tutorial-define-device-type/stateview.png)

4. Als er binnen een korte tijdsduur te veel gegevenspunten door het apparaat worden verzonden, wordt de statusmeting weergegeven met een andere visualisatie. Selecteer de grafiek om te zien van dat alle gegevenspunten binnen deze periode worden weergegeven in chronologische volgorde. U kunt ook het tijdsbereik verkleinen, zodat de metingen gedetailleerder worden weergegeven.

## <a name="settings-properties-and-commands"></a>Instellingen, eigenschappen en opdrachten

Instellingen, eigenschappen en opdrachten zijn verschillende waarden die worden gedefinieerd in een apparaatsjabloon en aan elk afzonderlijk apparaat worden gekoppeld:

* U gebruikt _instellingen_ om configuratiegegevens van uw toepassing naar een apparaat te verzenden. Een operator kan bijvoorbeeld een instelling gebruiken om het telemetrie-interval van het apparaat van twee seconden naar vijf seconden te wijzigen. Wanneer een operator een instelling wijzigt, wordt de instelling in de gebruikersinterface als in behandeling gemarkeerd totdat het apparaat reageert met een bevestiging.

* _Eigenschappen_ gebruikt u voor het definiëren van metagegevens die aan uw apparaat zijn gekoppeld. Er zijn twee categorieën eigenschappen:
    
  * _Toepassingseigenschappen_ gebruikt u om gegevens over het apparaat in uw toepassing vast te leggen. U kunt bijvoorbeeld toepassingseigenschappen gebruiken om de locatie van het apparaat en de laatste servicedatum vast te leggen. Deze eigenschappen worden opgeslagen in de toepassing en worden niet gesynchroniseerd met het apparaat. Een operator kan waarden toewijzen aan eigenschappen.

  * U gebruikt _apparaateigenschappen_  om een ​​apparaat in staat te stellen eigenschapswaarden naar uw toepassing te verzenden. Deze eigenschappen kunnen alleen worden bijgewerkt door het apparaat. Voor een operator zijn apparaateigenschappen alleen-lezen. In dit scenario met een aangesloten airconditioner zijn de firmwareversie en het serienummer van het apparaat apparaateigenschappen die door het apparaat zijn gemeld.
    
    Zie voor meer informatie [Eigenschappen](howto-set-up-template.md#properties) in de handleiding over het instellen van een apparaatsjabloon.

* U gebruikt _opdrachten_ om uw apparaat extern te beheren vanuit de toepassing. U kunt opdrachten rechtstreeks vanuit de cloud op het apparaat uitvoeren om de apparaten te beheren. Een operator kan bijvoorbeeld opdrachten uitvoeren zoals opnieuw opstarten om het apparaat onmiddellijk opnieuw op te starten.

## <a name="use-settings"></a>Instellingen gebruiken

U gebruikt *instellingen* om een operator in staat te stellen configuratiegegevens naar een apparaat te verzenden. In deze sectie voegt u een instelling toe aan uw apparaatsjabloon **Aangesloten airconditioner** waarmee een operator de doeltemperatuur van de aangesloten airconditioner kan instellen.

1. Ga naar het tabblad **Instellingen** van uw apparaatsjabloon **Aangesloten airconditioner**.

2. U kunt de instellingen maken van verschillende typen, zoals getallen of tekst. Selecteer **nummer** om toe te voegen van de instelling van een nummer op uw apparaat.

3. Gebruik de informatie in de volgende tabel om uw instelling **Temperatuur instellen** te configureren:

    | Veld                | Value           |
    | -------------------- | -----------     |
    | Weergavenaam         | Temperatuur instellen |
    | Veldnaam           | temperatuurInstellen  |
    | Meeteenheid      | F               |
    | Aantal decimalen       | 1               |
    | Minimumwaarde        | 20              |
    | Maximumwaarde        | 200             |
    | Initiële waarde        | 80              |
    | Description          | De doeltemperatuur voor de airconditioner instellen |

    Selecteer vervolgens **opslaan**:

    ![Stel instelling Temperatuur instellen configureren](./media/tutorial-define-device-type/configuresetting.png)

    > [!NOTE]
    > Wanneer het apparaat een instellingswijziging bevestigt, verandert de status van de instelling in **Gesynchroniseerd**.

4. U kunt de indeling van het tabblad **Instellingen**  aanpassen door instellingentegels te verplaatsen en een ander formaat te geven:

    ![De indeling van de instellingen aanpassen](./media/tutorial-define-device-type/settingslayout.png)

## <a name="use-properties"></a>Eigenschappen gebruiken

U gebruikt *toepassingseigenschappen* om gegevens over uw apparaat in de toepassing op te slaan. In deze sectie gaat u toepassingseigenschappen toevoegen aan uw apparaatsjabloon **Aangesloten airconditioner** om de locatie van het apparaat en de laatste servicedatum op te slaan. Deze eigenschappen kunnen worden gewijzigd in de toepassing. Het apparaat meldt ook eigenschappen zoals het serienummer en de firmwareversie die alleen-lezen zijn in de toepassing.

1. Ga naar het tabblad **Eigenschappen** voor uw apparaatsjabloon **Aangesloten airconditioner**.

1. U kunt apparaateigenschappen maken van verschillende typen, zoals getallen of tekst. Kies **Locatie**om een locatie-eigenschap aan uw apparaatsjabloon toe te voegen. Gebruik de informatie in de volgende tabel om uw locatie-eigenschap te configureren:

    | Veld                | Value                |
    | -------------------- | -------------------- |
    | Weergavenaam         | Locatie             |
    | Veldnaam           | location             |
    | Initiële waarde        | Seattle, WA          |
    | Description          | Locatie apparaat      |

    Laat andere velden op de standaardwaarde staan.

    ![De apparaateigenschappen configureren](./media/tutorial-define-device-type/configureproperties.png)

    Selecteer **Opslaan**.

1. Kies **Datum** om een eigenschap voor de laatste servicedatum aan uw apparaatsjabloon toe te voegen.

1. Gebruik de informatie in de volgende tabel om uw eigenschap Laatste servicedatum te configureren:

    | Veld                | Value                   |
    | -------------------- | ----------------------- |
    | Weergavenaam         | Laatste servicedatum       |
    | Veldnaam           | Servicedatum             |
    | Initiële waarde        | 1/1/2019                |
    | Description          | Laatste servicebeurt           |

    ![De apparaateigenschappen configureren](./media/tutorial-define-device-type/configureproperties2.png)

    Selecteer **Opslaan**.

1. U kunt de indeling van het tabblad **Eigenschappen**  aanpassen door eigenschapstegels te verplaatsen en het formaat ervan te wijzigen.

1. Kies **Apparaateigenschap** om een apparaateigenschap, zoals de firmwareversie, toe te voegen aan uw apparaatsjabloon.

1. Gebruik de informatie in de volgende tabel om uw firmwareversie te configureren:

    | Veld                | Value                   |
    | -------------------- | ----------------------- |
    | Weergavenaam         | Firmwareversie        |
    | Veldnaam           | firmwareversie         |
    | Gegevenstype            | tekst                    |
    | Description          | De firmwareversie van de airconditioner |

    ![Firmwareversie configureren](./media/tutorial-define-device-type/configureproperties3.png)

    Selecteer **Opslaan**.

1. Kies **Apparaateigenschap** om een apparaateigenschap, zoals een serienummer, toe te voegen aan uw apparaatsjabloon.

1. Gebruik de informatie in de volgende tabel om het serienummer te configureren:

    | Veld                | Value                   |
    | -------------------- | ----------------------- |
    | Weergavenaam         | Serienummer           |
    | Veldnaam           | Serienummer            |
    | Gegevenstype            | tekst                    |
    | Description          | Het serienummer van de airconditioner  |

    ![Het serienummer configureren](./media/tutorial-define-device-type/configureproperties4.png)

    Selecteer **Opslaan**.

    > [!NOTE]
    > De apparaateigenschap wordt van het apparaat naar de toepassing verzonden. De waarden voor de firmwareversie en het serienummer worden bijgewerkt wanneer uw fysieke apparaat verbinding maakt met IoT Central.

## <a name="use-commands"></a>Opdrachten gebruiken

U kunt _opdrachten_ gebruiken om een operator in te schakelen om opdrachten rechtstreeks op het apparaat uit te voeren. In deze sectie voegt u een opdracht toe aan de apparaatsjabloon **Aangesloten airconditioner** waarmee een operator een bepaald bericht kan doorgeven naar de aangesloten airconditioner.

1. Ga naar het tabblad **Opdrachten** van de apparaatsjabloon **Aangesloten airconditioner** om de sjabloon te bewerken.

1. Selecteer **+ nieuwe opdracht** een opdracht toevoegen aan uw apparaat en beginnen met het configureren van uw nieuwe opdracht.

1. Gebruik de informatie in de volgende tabel om de nieuwe opdracht te configureren:

    | Veld                | Value           |
    | -------------------- | -----------     |
    | Weergavenaam         | Opdracht Echo    |
    | Veldnaam           | echo            |
    | Standaardtime-out      | 30              |
    | Weergavetype         | tekst            |
    | Description          | Apparaatopdracht  |  

    U kunt aanvullende invoer aan de opdracht toevoegen door te selecteren **+** voor **invoer velden**.

    ![Toevoegen van een instelling voorbereiden](./media/tutorial-define-device-type/commandsecho1.png)

     Selecteer **Opslaan**.

1. U kunt de indeling van het tabblad **Opdrachten** aanpassen door opdrachtentegels te verplaatsen en het formaat ervan te wijzigen.

## <a name="view-your-simulated-device"></a>Gesimuleerd apparaat bekijken

Nu u uw apparaatsjabloon **Aangesloten airconditioner** hebt gedefinieerd kunt u het **Dashboard** aanpassen om de metingen, instellingen en eigenschappen die u hebt gedefinieerd op te nemen. Vervolgens kunt u het dashboard bekijken als een operator:

1. Ga naar het tabblad **Dashboard** van uw apparaatsjabloon **Aangesloten airconditioner**.

1. Selecteer **lijndiagram** om toe te voegen van het onderdeel naar de **Dashboard**.

1. Configureer het onderdeel **​​Lijndiagram** met behulp van de informatie in de volgende tabel:

    | Instelling      | Value       |
    | ------------ | ----------- |
    | Titel        | Temperatuur |
    | Tijdsbereik   | Afgelopen 30 minuten |
    | Metingen     | Temperatuur (Selecteer **zichtbaarheid** naast **temperatuur**) |

    ![Lijndiagraminstellingen](./media/tutorial-define-device-type/linechartsettings.png)

    Selecteer vervolgens **Opslaan**.

1. Selecteer de **gebeurtenissengeschiedenis** onderdeel met behulp van de informatie in de volgende tabel:

    | Instelling      | Value       |
    | ------------ | ----------- |
    | Titel        | Gebeurtenissen ventilatormotor |
    | Tijdsbereik   | Afgelopen 30 minuten |
    | Metingen     | Pas de Motor fout (Selecteer **zichtbaarheid** naast **ventilator Motor fout**) |

    ![Instellingen van gebeurtenis](./media/tutorial-define-device-type/dashboardeventchartsetting.png)

    Selecteer vervolgens **Opslaan**.

1. Configureer het onderdeel **Statusgeschiedenis** met behulp van de informatie in de volgende tabel:

    | Instelling      | Value       |
    | ------------ | ----------- |
    | Titel        | Ventilatormodus |
    | Tijdsbereik   | Afgelopen 30 minuten |
    | Metingen | Pas de modus (Selecteer **zichtbaarheid** naast **ventilator modus**) |

    ![Lijndiagraminstellingen](./media/tutorial-define-device-type/dashboardstatechartsetting.png)

    Selecteer vervolgens **Opslaan**.

1. Kies **Instellingen en eigenschappen**  om de apparaatinstellingen en -eigenschappen aan het dashboard toe te voegen. Selecteer **toevoegen/verwijderen** om toe te voegen van de instellingen of eigenschappen die u graag zou willen zien in het dashboard.

1. Configureer het onderdeel **Instellingen en eigenschappen** met behulp van de informatie in de volgende tabel:

    | Instelling                 | Value         |
    | ----------------------- | ------------- |
    | Titel                   | Apparaateigenschappen |
    | Instellingen en eigenschappen | Temperatuur instellen<br/>Serienummer<br/>Firmwareversie |

    Instellingen en eigenschappen die u eerder hebt gedefinieerd op de pagina **Instellingen en eigenschappen**, worden weergegeven bij **Beschikbare kolommen**.

    ![Instellingen voor de eigenschap Temperatuur instellen](./media/tutorial-define-device-type/propertysettings4.png)

    Selecteer vervolgens **Opslaan**.

1. U ziet nu gesimuleerde gegevens voor uw aangesloten airconditioner in het dashboard. U kunt de tegels en indeling voor het dashboard bewerken:

    ![Dashboard weergeven](./media/tutorial-define-device-type/dashboard.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Een nieuwe apparaatsjabloon maken
> * Telemetrie toevoegen aan uw apparaat
> * Gesimuleerde telemetrie bekijken
> * Apparaatgebeurtenissen definiëren
> * Gesimuleerde gebeurtenissen bekijken
> * Uw status definiëren
> * Gesimuleerde status bekijken
> * Instellingen en eigenschappen gebruiken
> * Opdrachten gebruiken
> * Het gesimuleerde apparaat in het dashboard bekijken

Nu u een apparaatsjabloon in uw Azure IoT Central-toepassing hebt gedefinieerd, volgen hier de aanbevolen volgende stappen:

* [Regels en acties voor uw apparaat configureren](tutorial-configure-rules.md)
* [De weergaven van de operator aanpassen](tutorial-customize-operator.md)