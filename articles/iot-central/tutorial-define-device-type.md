---
title: Een nieuw apparaattype definiëren in Azure IoT Central | Microsoft Docs
description: Deze zelfstudie laat zien hoe u als bouwer een nieuw apparaattype kunt definiëren in uw Azure IoT Central-toepassing. U definieert de telemetrie, status, eigenschappen en instellingen voor uw type.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 7121c83aea75f3b23820a7b0504fa704ec9f3016
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984302"
---
# <a name="tutorial-define-a-new-device-type-in-your-azure-iot-central-application"></a>Zelfstudie: Een nieuw apparaattype definiëren in uw Azure IoT Central-toepassing

Deze zelfstudie laat zien hoe u als bouwer een apparaatsjabloon kunt gebruiken om een nieuw apparaattype te definiëren in uw Microsoft Azure IoT Central-toepassing. Een apparaatsjabloon definieert de telemetrie, status, eigenschappen en instellingen voor uw apparaattype.

Om u in staat te stellen uw toepassing te testen voordat u een echt apparaat aansluit, genereert Azure IoT Central een gesimuleerd apparaat op basis van de apparaatsjabloon wanneer u deze maakt.

In deze zelfstudie maakt u een apparaatsjabloon voor een **aangesloten airconditioner**. Een aangesloten airconditioningapparaat:

* Verzendt telemetrie zoals temperatuur en luchtvochtigheid.
* Rapporteert zijn status, zoals of het aan of uit is.
* Heeft eigenschappen zoals de firmwareversie en het serienummer.
* Heeft instellingen zoals de doeltemperatuur en ventilatorsnelheid.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een nieuwe apparaatsjabloon maken
> * Telemetrie toevoegen aan uw apparaat
> * Gesimuleerde telemetrie bekijken
> * Gebeurtenismeting definiëren
> * Gesimuleerde gebeurtenissen bekijken
> * Statusmeting definiëren
> * Gesimuleerde status bekijken
> * Apparaateigenschappen gebruiken
> * Apparaatinstellingen gebruiken
> * Opdrachten gebruiken

## <a name="prerequisites"></a>Vereisten

U hebt een Azure IoT Central-toepassing nodig om deze zelfstudie te voltooien. Als u de snelstart [Een Azure IoT Central-toepassing maken](quick-deploy-iot-central.md) hebt voltooid, kunt u de toepassing die u in de snelstart hebt gemaakt, weer gebruiken. Voer anders de volgende stappen uit om een lege Azure IoT Central-toepassing te maken:

1. Navigeer naar de pagina [​​Toepassingsbeheer ](https://aka.ms/iotcentral) van Azure IoT Central.

2. Voer het e-mailadres en wachtwoord in dat u gebruikt om toegang te krijgen tot uw Azure-abonnement:

   ![Voer uw organisatieaccount in](./media/tutorial-define-device-type/sign-in.png)

3. Als u wilt beginnen met het maken van een nieuwe Azure IoT Central-toepassing, kiest u **Nieuwe toepassing** :

    ![Pagina Toepassingsbeheer van Azure IoT Central](./media/tutorial-define-device-type/iotcentralhome.png)

4. Een nieuwe Azure IoT Central-toepassing maken:

    * Kies een beschrijvende toepassingsnaam, zoals **Contoso-airconditioners**. Azure IoT Central genereert een uniek URL-voorvoegsel voor u. U kunt dit URL-voorvoegsel wijzigen in iets dat gemakkelijker te onthouden is.
    
    * Kies een Azure Active Directory en Azure-abonnement om te gebruiken. Zie [Een Azure IoT Central-toepassing maken ](howto-create-application.md)voor meer informatie over directory’s en abonnementen.
    
    * Gebruik een bestaande resourcegroep, of maak een nieuwe resourcegroep met een naam van uw keuze. Bijvoorbeeld **contoso-rg**.
    
    * Kies de regio die geografisch het dichtst bij u in de buurt is.
    
    * Kies de toepassingssjabloon **Aangepaste toepassing**.
    
    * Kies het betalingsplan **Gratis proeftoepassing (30 dagen)**.
    
    * Kies **Maken**.

    ![Pagina Toepassing maken van Azure IoT Central](./media/tutorial-define-device-type/iotcentralcreate.png)

Zie [Een Azure IoT Central-toepassing maken](howto-create-application.md) voor meer informatie.

## <a name="create-a-new-custom-device-template"></a>Een nieuwe aangepaste apparaatsjabloon maken

Als bouwer kunt u de apparaatsjablonen in uw toepassing maken en bewerken. Wanneer u een apparaatsjabloon maakt, genereert Azure IoT Central een gesimuleerd apparaat op basis van de sjabloon. Het gesimuleerde apparaat genereert telemetrie waarmee u het gedrag van uw toepassing kunt testen voordat u een fysiek apparaat aansluit.

Als u een nieuwe apparaatsjabloon aan uw toepassing wilt toevoegen, gaat u naar de pagina **Toepassingsbouwer**. Kies hiervoor de **Toepassingsbouwer** in het navigatiemenu links.

![Pagina Application Builder](./media/tutorial-define-device-type/builderhome.png)

## <a name="add-a-device-and-define-telemetry"></a>Een apparaat toevoegen en telemetrie definiëren

De volgende stappen laten zien hoe u een nieuwe apparaatsjabloon, **Verbonden airconditioner**, maakt voor apparaten die temperatuurtelemetrie naar uw toepassing verzenden:

1. Kies **Apparaatsjabloon maken** op de pagina **Toepassingsbouwer**:

    ![Pagina Toepassingsbouwer, Apparaatsjabloon maken](./media/tutorial-define-device-type/builderhomedevices.png)

2. Kies **Aangepast** op de pagina **Apparaatsjablonen**. Met een **aangepaste** apparaatsjabloon kunt u alle kenmerken en het gedrag van uw aangesloten airconditioner definiëren:

    ![Apparaten](./media/tutorial-define-device-type/builderhomedevicescustom.png)

3. Voer op de pagina **Nieuwe apparaatsjabloon** **Aangesloten airconditioner** in als de naam van uw apparaat en kies **Maken**. U kunt ook een afbeelding van uw apparaat uploaden die zichtbaar is voor operators in de apparatenverkenner:

    ![Aangepast apparaat](./media/tutorial-define-device-type/createcustomdevice.png)

4. Zorg ervoor dat u zich in de apparaatsjabloon **Aangesloten airconditioner** op de pagina **Metingen** bevindt waar u de telemetrie definieert. Elke apparaatsjabloon die u definieert, heeft afzonderlijke pagina's waarmee u het volgende kunt doen:

    * De metingen opgeven, zoals telemetrie, gebeurtenis en status, die door het apparaat worden verzonden.
    
    * De instellingen definiëren die worden gebruikt om het apparaat te beheren.
    
    * De eigenschappen definiëren die worden gebruikt voor het vastleggen van informatie over het apparaat.
    
    * De regels definiëren die zijn gekoppeld aan het apparaat.
    
    * Het dashboard van het apparaat aanpassen voor uw operators.

    ![Airconditionermetingen](./media/tutorial-define-device-type/airconmeasurements.png)

    > [!NOTE]
    > Als u de naam van het apparaat of de apparaatsjabloon wilt wijzigen, klikt u op de tekst bovenaan de pagina.

5. Kies **Nieuwe meting**om de temperatuurtelemetriemeting toe te voegen. Kies vervolgens **Telemetrie** als het type meting:

    ![Metingen aangesloten airconditioner](./media/tutorial-define-device-type/airconmeasurementsnew.png)

6. Elk type telemetrie dat u definieert voor een apparaatsjabloon bevat [configuratieopties](howto-set-up-template.md) zoals:

    * Weergaveopties.

    * Details van de telemetrie.

    * Simulatieparameters.

    Gebruik de informatie in de volgende tabel om uw **Temperatuur**-telemetrie te configureren:

    | Instelling              | Waarde         |
    | -------------------- | -----------   |
    | Weergavenaam         | Temperatuur   |
    | Veldnaam           | temperatuur   |
    | Eenheden                | F             |
    | Min.                  | 60            |
    | Max.                  | 110           |
    | Aantal decimalen       | 0             |

    U kunt ook een kleur kiezen voor de telemetrieweergave. Kies **Opslaan** om de telemetriedefinitie op te slaan:

    ![Temperatuursimulatie configureren](./media/tutorial-define-device-type/temperaturesimulation.png)

7. Na enige tijd geeft de pagina **Metingen** een grafiek weer van de temperatuurtelemetrie van uw gesimuleerde aangesloten airconditioningapparaat. Gebruik de besturingselementen om de zichtbaarheid en aggregatie te beheren of de telemetriedefinitie te bewerken:

    ![Temperatuursimulatie bekijken](./media/tutorial-define-device-type/viewsimulation.png)

8. U kunt het diagram ook aanpassen met behulp van de besturingselementen **Lijn** , **Gestapeld**en **Tijdsbereik bewerken**:

    ![De grafiek aanpassen](./media/tutorial-define-device-type/customizechart.png)

## <a name="define-event-measurement"></a>Gebeurtenismeting definiëren

U kunt Gebeurtenis gebruiken om tijdstipgegevens te definiëren die door het apparaat worden verzonden om iets van betekenis aan te geven, zoals een fout of een storing in een onderdeel. Net als telemetriemetingen kan Azure IoT Central apparaatgebeurtenissen simuleren, zodat u het gedrag van uw toepassing kunt testen voordat u een fysiek apparaat aansluit. U definieert gebeurtenismetingen voor uw apparaattype in de weergave **Metingen**.

1. Kies **Nieuwe meting** om de gebeurtenismeting **Storing ventilatormotor** toe te voegen. Kies vervolgens **Gebeurtenis** als het type meting:

    ![Metingen aangesloten airconditioner](./media/tutorial-define-device-type/eventnew.png)

2. Elk type gebeurtenis dat u definieert voor een apparaatsjabloon bevat [configuratieopties](howto-set-up-template.md) zoals:

   * Weergavenaam.

   * Veldnaam.

   * Ernst.

    Gebruik de informatie in de volgende tabel om de gebeurtenis **Storing ventilatormotor** te configureren:

    | Instelling              | Waarde             |
    | -------------------- | -----------       |
    | Weergavenaam         | Storing ventilatormotor   |
    | Veldnaam           | storingventilatormotor       |
    | Severity             | Fout             |

    Kies **Opslaan** om de gebeurtenisdefinitie op te slaan:

    ![Gebeurtenismeting configureren](./media/tutorial-define-device-type/eventconfiguration.png)

3. Na enige tijd geeft de pagina **Metingen** een grafiek weer van de willekeurig door uw gesimuleerde aangesloten airconditioningapparaat gegenereerde gegevens. Gebruik de besturingselementen om de zichtbaarheid te beheren of de gebeurtenisdefinitie te bewerken:

    ![Gebeurtenissimulatie bekijken](./media/tutorial-define-device-type/eventview.png)

1. Klik voor meer informatie over de gebeurtenis op de gebeurtenis in de grafiek:

    ![Gebeurtenisdetails bekijken](./media/tutorial-define-device-type/eventviewdetail.png)

## <a name="define-state-measurement"></a>Statusmeting definiëren

U kunt Status gebruiken om de status van het apparaat of een onderdeel ervan in de loop van de tijd te definiëren en te visualiseren. Net als telemetriemetingen kan Azure IoT Central de apparaatstatus simuleren, zodat u het gedrag van uw toepassing kunt testen voordat u een fysiek apparaat aansluit. U definieert statusmetingen voor uw apparaattype in de weergave **Metingen**.

1. Kies **Nieuwe meting** om de meting **Ventilatormodus**  toe te voegen. Kies vervolgens **Status** als het type meting:

    ![Statusmetingen aangesloten airconditioner](./media/tutorial-define-device-type/statenew.png)

2. Elk type status dat u definieert voor een apparaatsjabloon bevat [configuratieopties](howto-set-up-template.md) zoals:

   * Weergavenaam.

   * Veldnaam.

   * Waarden met optionele weergavelabels.

   * Kleur voor elke waarde.

    Gebruik de informatie in de volgende tabel om de status **Ventilatormodus** te configureren:

    | Instelling              | Waarde             |
    | -------------------- | -----------       |
    | Weergavenaam         | Ventilatormodus          |
    | Veldnaam           | ventilatormodus           |
    | Waarde                | 1                 |
    | Weergavelabel        | Actief         |
    | Waarde                | 0                 |
    | Weergavelabel        | Gestopt           |

    Kies **Opslaan** om de statusmetingdefinitie op te slaan:

    ![Statusmeting configureren](./media/tutorial-define-device-type/stateconfiguration.png)

3. Na enige tijd geeft de pagina **Metingen** een grafiek weer van de willekeurig door uw gesimuleerde aangesloten airconditioningapparaat gegenereerde statuswaarden. Gebruik de besturingselementen om de zichtbaarheid te beheren of de statusdefinitie te bewerken:

    ![Statussimulatie bekijken](./media/tutorial-define-device-type/stateview.png)

4. Als er binnen een korte tijdsduur te veel gegevenspunten door het apparaat worden verzonden, wordt de statusmeting weergegeven met een andere visualisatie, zoals hieronder weergegeven. Als u op het diagram klikt, worden alle gegevenspunten binnen die periode in chronologische volgorde weergegeven. U kunt ook het tijdsbereik verkleinen, zodat de meting in de grafiek wordt weergegeven.

    ![Statusdetails bekijken](./media/tutorial-define-device-type/stateviewdetail.png)

## <a name="settings-properties-and-commands"></a>Instellingen, eigenschappen en opdrachten

Instellingen, eigenschappen en apparaateigenschappen, en opdrachten zijn verschillende waarden die worden gedefinieerd in een apparaatsjabloon en gekoppeld aan elk afzonderlijk apparaat:

* U gebruikt _instellingen_ om configuratiegegevens van uw toepassing naar een apparaat te verzenden. Een operator kan bijvoorbeeld een instelling gebruiken om het telemetrie-interval van het apparaat van twee seconden naar vijf seconden te wijzigen. Wanneer een operator een instelling wijzigt, wordt de instelling in de gebruikersinterface als in behandeling gemarkeerd totdat het apparaat bevestigt dat de wijziging van de instelling is uitgevoerd.

* U gebruikt _eigenschappen_ om gegevens over het apparaat in uw toepassing te registreren. U kunt bijvoorbeeld eigenschappen gebruiken om het serienummer van een apparaat of het telefoonnummer van de apparaatfabrikant vast te leggen. Eigenschappen worden opgeslagen in de toepassing en worden niet gesynchroniseerd met het apparaat. Een operator kan waarden toewijzen aan eigenschappen.

* U gebruikt _apparaateigenschappen_  om een ​​apparaat in staat te stellen eigenschapswaarden naar uw toepassing te verzenden. Deze eigenschappen kunnen alleen worden bijgewerkt door het apparaat. Voor een operator zijn apparaateigenschappen alleen-lezen.

* U gebruikt _opdrachten_ om uw apparaat extern te beheren vanuit de toepassing. U kunt opdrachten rechtstreeks vanuit de cloud op het apparaat uitvoeren om de apparaten te beheren. Een operator kan bijvoorbeeld opdrachten uitvoeren zoals opnieuw opstarten om het apparaat onmiddellijk opnieuw op te starten.

## <a name="use-settings"></a>Instellingen gebruiken

U gebruikt *instellingen* om een operator in staat te stellen configuratiegegevens naar een apparaat te verzenden. In deze sectie voegt u een instelling toe aan uw apparaatsjabloon **Aangesloten airconditioner** waarmee een operator de doeltemperatuur van de aangesloten airconditioner kan instellen.

1. Navigeer naar de pagina **Instellingen** van uw apparaatsjabloon **Aangesloten airconditioner**:

    ![Toevoegen van een instelling voorbereiden](./media/tutorial-define-device-type/deviceaddsetting.png)

    U kunt de instellingen maken van verschillende typen, zoals getallen of tekst.

2. Kies **Getal** om een getalinstelling toe te voegen aan uw apparaat.

3. Gebruik de informatie in de volgende tabel om uw instelling **Temperatuur instellen** te configureren:

    | Veld                | Waarde           |
    | -------------------- | -----------     |
    | Weergavenaam         | Temperatuur instellen |
    | Veldnaam           | temperatuurInstellen  |
    | Meeteenheid  | F               |
    | Aantal decimalen       | 1               |
    | Minimumwaarde        | 20              |
    | Maximumwaarde        | 200             |
    | Initiële waarde        | 80              |
    | Beschrijving          | De doeltemperatuur voor de airconditioner instellen |

    Kies vervolgens **Opslaan**:

    ![Stel instelling Temperatuur instellen configureren](./media/tutorial-define-device-type/configuresetting.png)

    > [!NOTE]
    > Wanneer het apparaat een instellingswijziging bevestigt, verandert de status van de instelling in **Gesynchroniseerd**.

4. U kunt de indeling van de pagina **Instellingen**  aanpassen door instellingentegels te verplaatsen en het formaat ervan te wijzigen:

    ![De indeling van de instellingen aanpassen](./media/tutorial-define-device-type/settingslayout.png)

## <a name="use-properties--device-properties"></a>Eigenschappen / apparaateigenschappen gebruiken

U gebruikt *eigenschappen* om gegevens over uw apparaat in de toepassing op te slaan. In deze sectie voegt u cloudeigenschappen toe aan uw apparaatsjabloon **Aangesloten airconditioner** om de locatie van het apparaat en de laatste servicedatum op te slaan. Dit zijn beide bewerkbare eigenschappen van het apparaat. Er worden ook alleen-lezen-eigenschappen gemeld door het apparaat, die niet kunnen worden gewijzigd, zoals het serienummer van het apparaat en de firmwareversie.
 
1. Ga naar de pagina **Eigenschappen** voor uw apparaatsjabloon **Aangesloten airconditioner**:

    ![Toevoegen van een eigenschap voorbereiden](./media/tutorial-define-device-type/deviceaddproperty.png)

    U kunt apparaateigenschappen maken van verschillende typen, zoals getallen of tekst. Kies **Locatie**om een locatie-eigenschap aan uw apparaatsjabloon toe te voegen.

2. Gebruik de informatie in de volgende tabel om uw locatie-eigenschap te configureren:

    | Veld                | Waarde                |
    | -------------------- | -------------------- |
    | Weergavenaam         | Locatie             |
    | Veldnaam           | location             |
    | Initiële waarde        | Seattle, WA          |
    | Beschrijving          | Locatie apparaat      |

    Laat andere velden op de standaardwaarde staan.

    ![De apparaateigenschappen configureren](./media/tutorial-define-device-type/configureproperties.png)

    Kies **Opslaan**.

3. Kies **Datum** om een eigenschap voor de laatste servicedatum aan uw apparaatsjabloon toe te voegen.

4. Gebruik de informatie in de volgende tabel om uw eigenschap Laatste servicedatum te configureren:

    | Veld                | Waarde                   |
    | -------------------- | ----------------------- |
    | Weergavenaam         | Laatste servicedatum       |
    | Veldnaam           | Servicedatum             |
    | Initiële waarde        | 1-1-2018                |
    | Beschrijving          | Laatste servicebeurt           |

    ![De apparaateigenschappen configureren](./media/tutorial-define-device-type/configureproperties2.png)

    Kies **Opslaan**.

5. U kunt de indeling van de pagina **Eigenschappen**  aanpassen door eigenschapstegels te verplaatsen en het formaat ervan te wijzigen:

    ![De indeling van de eigenschappen aanpassen](./media/tutorial-define-device-type/propertieslayout.png)


## <a name="use-commands"></a>Opdrachten gebruiken

U kunt _opdrachten_ gebruiken om een operator in te schakelen om opdrachten rechtstreeks op het apparaat uit te voeren. In deze sectie voegt u een opdracht toe aan de apparaatsjabloon **Aangesloten airconditioner** waarmee een operator een bepaald bericht kan doorgeven naar de display van de aangesloten airconditioner (dit werk met MxChip-voorbeeldcode)

1. Navigeer naar de pagina **Opdrachten** van de apparaatsjabloon **Aangesloten airconditioner**:

    ![Toevoegen van een instelling voorbereiden](media/tutorial-define-device-type/commandsecho.png)

    U kunt verschillende typen opdrachten maken op basis van uw vereisten. 

1. Klik op **Nieuwe opdracht** om een opdracht toe te voegen aan uw apparaat.

1. Gebruik de informatie in de volgende tabel om de nieuwe opdracht te configureren:

    | Veld                | Waarde           |
    | -------------------- | -----------     |
    | Weergavenaam         | Opdracht Echo    |
    | Veldnaam           | echo            |
    | Standaardtime-out      | 30              |
    | Weergavetype         | tekst            |
    | Beschrijving          | Apparaatopdracht  |  

U kunt aanvullende invoer aan de opdracht toevoegen door te klikken op **+** voor invoer.

2. Kies **Opslaan**.

3. U kunt de indeling van de pagina **Opdrachten** aanpassen door opdrachtentegels te verplaatsen en het formaat ervan te wijzigen:

    ![De indeling van de instellingen aanpassen](media/tutorial-define-device-type/commandstileresize.png)

## <a name="view-your-simulated-device"></a>Gesimuleerd apparaat bekijken

Nu u uw apparaatsjabloon **Aangesloten airconditioner** hebt gedefinieerd kunt u het **Dashboard** aanpassen om de metingen, instellingen en eigenschappen die u hebt gedefinieerd op te nemen. Vervolgens kunt u het dashboard bekijken als een operator:

1. Navigeer naar de pagina **Dashboard** van uw apparaatsjabloon **Aangesloten airconditioner**:

    ![Dashboard van aangesloten airconditioner](./media/tutorial-define-device-type/aircondashboards.png)

2. Kies **Lijndiagram** om het onderdeel toe te voegen aan het **Dashboard**:

    ![Dashboardonderdelen](./media/tutorial-define-device-type/dashboardcomponents1.png)

3. Configureer het onderdeel **​​Lijndiagram** met behulp van de informatie in de volgende tabel:

    | Instelling      | Waarde       |
    | ------------ | ----------- |
    | Titel        | Temperatuur |
    | Tijdsbereik   | Afgelopen 30 minuten |
    | Metingen | temperatuur (kies **Zichtbaarheid** naast **temperatuur**) |

    ![Lijndiagraminstellingen](./media/tutorial-define-device-type/linechartsettings.png)

    Kies vervolgens **Opslaan**.

4. Configureer het onderdeel **Gebeurtenisdiagram** met behulp van de informatie in de volgende tabel:

    | Instelling      | Waarde       |
    | ------------ | ----------- |
    | Titel        | Gebeurtenissen |
    | Tijdsbereik   | Afgelopen 30 minuten |
    | Metingen | Storing ventilatormotor (kies **Zichtbaarheid** naast **Storing ventilatormotor**) |

    ![Lijndiagraminstellingen](./media/tutorial-define-device-type/dashboardeventchartsetting.png)

    Kies vervolgens **Opslaan**.

5. Configureer het onderdeel **Statusdiagram** met behulp van de informatie in de volgende tabel:

    | Instelling      | Waarde       |
    | ------------ | ----------- |
    | Titel        | Ventilatormodus |
    | Tijdsbereik   | Afgelopen 30 minuten |
    | Metingen | Ventilatormodus (kies **Zichtbaarheid** naast **Ventilatormodus**) |

    ![Lijndiagraminstellingen](./media/tutorial-define-device-type/dashboardstatechartsetting.png)

    Kies vervolgens **Opslaan**.

6. Kies **Instellingen en eigenschappen** om de instelling Temperatuur instellen aan het dashboard toe te voegen:

    ![Dashboardonderdelen](./media/tutorial-define-device-type/dashboardcomponents4.png)

7. Configureer het onderdeel **Instellingen en eigenschappen** met behulp van de informatie in de volgende tabel:

    | Instelling                 | Waarde         |
    | ----------------------- | ------------- |
    | Titel                   | Doeltemperatuur instellen |
    | Instellingen en eigenschappen | Temperatuur instellen |

    ![Instellingen voor de eigenschap Serienummer](./media/tutorial-define-device-type/propertysettings3.png)

    Kies vervolgens **Opslaan**.

8. Kies **Instellingen en eigenschappen** om het serienummer van het apparaat aan het dashboard toe te voegen:

    ![Dashboardonderdelen](./media/tutorial-define-device-type/dashboardcomponents3.png)

9. Configureer het onderdeel **Instellingen en eigenschappen** met behulp van de informatie in de volgende tabel:

    | Instelling                 | Waarde         |
    | ----------------------- | ------------- |
    | Titel                   | Serienummer |
    | Instellingen en eigenschappen | Serienummer |

    ![Instellingen voor de eigenschap Serienummer](./media/tutorial-define-device-type/propertysettings3.png)

    Kies vervolgens **Opslaan**.

10. Kies **Instellingen en eigenschappen** om de firmwareversie van het apparaat aan het dashboard toe te voegen:

    ![Dashboardonderdelen](./media/tutorial-define-device-type/dashboardcomponents4.png)

11. Configureer het onderdeel **Instellingen en eigenschappen** met behulp van de informatie in de volgende tabel:

    | Instelling                 | Waarde            |
    | ----------------------- | ---------------- |
    | Titel                   | Firmwareversie |
    | Instellingen en eigenschappen | Firmwareversie |

    ![Instellingen voor de eigenschap Serienummer](./media/tutorial-define-device-type/propertysettings3.png)

    Kies vervolgens **Opslaan**.

12. Als u het dashboard als een operator wilt weergeven, schakelt u **Ontwerpmodus** in de rechterbovenhoek van de pagina uit.

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
> * Apparaateigenschappen gebruiken
> * Apparaatinstellingen gebruiken

Nu u een apparaatsjabloon in uw Azure IoT Central-toepassing hebt gedefinieerd, volgen hier de aanbevolen volgende stappen:

* [Regels en acties voor uw apparaat configureren](tutorial-configure-rules.md)
* [De weergaven van de operator aanpassen](tutorial-customize-operator.md)
