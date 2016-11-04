---
title: Overzicht van IoT Hub-apparaatbeheer | Microsoft Docs
description: 'Dit artikel bevat een overzicht van het apparaatbeheer in Azure IoT Hub: levenscyclus van het enterpriseapparaat, opnieuw opstarten, de fabrieksinstellingen herstellen, firmware-update, configuratie, apparaatdubbels, query''s, taken'
services: iot-hub
documentationcenter: ''
author: bzurcher
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/03/2016
ms.author: bzurcher

---
# <a name="overview-of-azure-iot-hub-device-management-(preview)"></a>Overzicht van apparaatbeheer via Azure IoT Hub (preview-versie)
## <a name="introduction"></a>Inleiding
Azure IoT Hub biedt de functies en het uitbreidingsmodel waarmee apparaat- en back-end-ontwikkelaars krachtige IoT-beheeroplossingen voor apparaten kunnen bouwen. IoT-apparaten variëren van beperkte sensoren en microcontrollers met één doel tot krachtige gateways die communicatie routeren voor groepen apparaten.  Bovendien verschillen de gebruiksvoorbeelden en de vereisten voor IoT-operators aanzienlijk in de verschillende sectoren.  Ondanks deze variatie biedt Azure IoT Hub-apparaatbeheer mogelijkheden, patronen en codebibliotheken die geschikt zijn voor een groot aantal apparaten en eindgebruikers.

Een essentieel onderdeel van het maken van een geslaagde enterprise-IoT-oplossing is het bieden van een strategie voor hoe operators omgaan met het continue beheer van de apparaten in hun bedrijf. IoT-operators hebben eenvoudige en betrouwbare hulpprogramma's en toepassingen nodig waarmee ze zich kunnen concentreren op de strategische aspecten van hun werk. Dit artikel bevat:

* Een kort overzicht van de Azure IoT Hub-aanpak van apparaatbeheer.
* Een beschrijving van de algemene principes van apparaatbeheer.
* Een beschrijving van de levenscyclus van het apparaat.
* Een overzicht van de algemene patronen van apparaatbeheer.

## <a name="iot-device-management-principles"></a>Principes van IoT-apparaatbeheer
IoT zorgt voor unieke apparaatbeheeruitdagingen en elke oplossing van enterprise-klasse moet rekening houden met de volgende principes:

![Afbeelding van de principes van apparaatbeheer via Azure IoT Hub][img-dm_principles]

* **Schaal en automatisering**: voor IoT-oplossingen zijn eenvoudige hulpprogramma's nodig waarmee routinetaken kunnen worden geautomatiseerd en waarmee een relatief klein aantal operationele medewerkers miljoenen apparaten kan beheren. Operators verwachten dagelijks apparaatbewerkingen extern en bulksgewijs uit te voeren en alleen te worden gewaarschuwd wanneer er problemen opduiken waarvoor hun directe aandacht is vereist.
* **Openheid en compatibiliteit**: het ecosysteem van IoT-apparaten is buitengewoon divers. Beheerhulpprogramma's moeten zo worden aangepast dat ze kunnen omgaan met een groot aantal apparaatklassen, platforms en protocollen. Operators moeten vele typen apparaten kunnen ondersteunen, van de meest beperkte ingesloten chips voor enkelvoudige processen tot krachtige en volledig functionele computers.
* **Contextbewustzijn**: IoT-omgevingen zijn dynamisch en veranderen voortdurend. Betrouwbaarheid van de service is cruciaal. Bij apparaatbeheerbewerkingen moet rekening worden gehouden met SLA-onderhoudsvensters, netwerk- en energiestatussen, in-gebruiksomstandigheden en apparaatgeolocatie om ervoor te zorgen dat de uitval vanwege onderhoud niet van invloed is op kritieke bedrijfsactiviteiten of dat er geen gevaarlijke situaties ontstaan.
* **Vele rollen bedienen**: ondersteuning voor de unieke werkstromen en processen van IoT-bewerkingsrollen is cruciaal. Beheerders moeten in harmonie met de gegeven beperkingen van interne IT-afdelingen werken.  Ze moeten ook duurzame manieren vinden om in realtime gegevens over apparaatbewerkingen door te sturen naar toezichthouders en andere zakelijke leidinggevende rollen.

## <a name="iot-device-lifecycle"></a>Levenscyclus van IoT-apparaat
Er is een set algemene fasen voor apparaatbeheer die hetzelfde zijn voor alle enterprise-IoT-projecten. In Azure IoT zijn er vijf fasen binnen de levenscyclus van een IoT-apparaat:

![De vijf fasen van de levenscyclus van een Azure IoT-apparaat: plannen, inrichten, configureren, bewaken, buiten gebruik stellen][img-device_lifecycle]

Binnen elk van deze vijf fasen zijn er verschillende vereisten voor de apparaatoperator waaraan moet worden voldaan voor een volledige oplossing:

* **Plannen**: operators in staat stellen een schema voor de metagegevens van een apparaat te maken waarmee ze eenvoudig en nauwkeurig kunnen zoeken naar en zich richten op een groep apparaten voor bulksgewijze beheerbewerkingen. U kunt de apparaatdubbel gebruiken om de metagegevens van dit apparaat op te slaan in de vorm van tags en eigenschappen.
  
    *Meer weten*: [Aan de slag met apparaatdubbels][lnk-twins-getstarted], [Meer informatie over apparaatdubbels][lnk-twins-devguide], [Dubbeleigenschappen gebruiken][lnk-twin-properties]
* **Inrichten**: nieuwe apparaten veilig inrichten voor IoT Hub en operators in staat stellen apparaatmogelijkheden onmiddellijk te detecteren.  Gebruik het IoT Hub-apparaatregister om flexibele apparaatidentiteiten en -referenties te maken en voer dit uit in één bulkbewerking met behulp van een taak. Bouw apparaten om hun mogelijkheden en voorwaarden via apparaateigenschappen te rapporteren in de apparaatdubbel.
  
    *Meer weten*: [Apparaatidentiteiten beheren][lnk-identity-registry], [Apparaatidentiteiten bulksgewijs beheren][lnk-bulk-identity], [Dubbeleigenschappen gebruiken][lnk-twin-properties]
* **Configureren**: bulksgewijze configuratiewijzigingen en firmware-updates op apparaten ondersteunen terwijl de status en beveiliging behouden blijven. Voer deze apparaatbeheerbewerkingen bulksgewijs uit met behulp van de gewenste eigenschappen of met rechtstreekse methoden en broadcast-taken.
  
    *Meer weten*: [Rechtstreekse methoden gebruiken][lnk-c2d-methods], [Een rechtstreekse methode aanroepen op een apparaat][lnk-methods-devguide], [Dubbeleigenschappen gebruiken][lnk-twin-properties], [Taken plannen en uitzenden][lnk-jobs], [Taken op meerdere apparaten plannen][lnk-jobs-devguide]
* **Bewaken**: de verzamelde status van alle apparaten bewaken, de status van lopende bewerkingen bewaken en operators attenderen op problemen die mogelijk hun aandacht vereisen.  Pas de apparaatdubbel toe, zodat apparaten in realtime bewerkingsvoorwaarden en de status van de update-bewerkingen kunnen rapporteren. Bouw krachtige dashboardrapporten die de meeste directe problemen melden via apparaatdubbel-query's.
  
    *Meer weten*: [Dubbeleigenschappen gebruiken][lnk-twin-properties], [Querytaal voor dubbels en taken][lnk-query-language]
* **Buiten gebruik stellen**: apparaten vervangen of uit bedrijf nemen na een storing, upgradecyclus of aan het einde van de levensduur.  Gebruik de apparaatdubbel om apparaatgegevens te onderhouden als het fysieke apparaat wordt vervangen, of te archiveren als het apparaat buiten gebruik wordt gesteld. Gebruik het IoT Hub-apparaatregister voor het veilig intrekken van apparaatidentiteiten en -referenties.
  
    *Meer weten*: [Dubbeleigenschappen gebruiken][lnk-twin-properties], [Apparaatidentiteiten beheren][lnk-identity-registery]

## <a name="iot-hub-device-management-patterns"></a>Patronen voor apparaatbeheer via IoT Hub
Met IoT Hub wordt de volgende set apparaatbeheerpatronen mogelijk gemaakt.  In de [Zelfstudies apparaatbeheer][lnk-get-started] wordt uitgebreid beschreven hoe u deze patronen zo kunt uitbreiden dat ze exact aansluiten bij uw scenario en hoe u nieuwe patronen ontwerpt op basis van deze kernsjablonen.

* **Opnieuw opstarten**: via de back-endtoepassing wordt het apparaat met een rechtstreekse methode geïnformeerd dat er opnieuw wordt opgestart.  Het apparaat maakt gebruik van de gerapporteerde eigenschappen in de apparaattwin om de opstartstatus van het apparaat bij te werken.
  
    ![Afbeelding van het opstartpatroon van apparaatbeheer via Azure IoT Hub][img-reboot_pattern]
* **Fabrieksinstellingen terugzetten **: via de back-endtoepassing wordt het apparaat via een rechtstreekse methode geïnformeerd dat het terugzetten van de fabrieksinstellingen is gestart.  Het apparaat maakt gebruik van de gerapporteerde eigenschappen van de apparaattwin om de status van het terugzetten van de fabrieksinstellingen van het apparaat bij te werken.
  
    ![Afbeelding van het patroon van herstel naar fabrieksinstellingen voor apparaatbeheer via Azure IoT Hub][img-facreset_pattern]
* **Configuratie**: voor de back-endtoepassing worden de gewenste eigenschappen van de apparaattwin gebruikt voor het configureren van software die op het apparaat wordt uitgevoerd.  Het apparaat maakt gebruik van de gerapporteerde eigenschappen van de apparaattwin om de configuratiestatus van het apparaat bij te werken.
  
    ![Afbeelding van het configuratiepatroon van apparaatbeheer via Azure IoT Hub][img-config_pattern]
* **Firmware-update**: via de back-endtoepassing wordt het apparaat via een rechtstreekse methode geïnformeerd dat er een firmware-update is gestart.  Voor het apparaat wordt een proces met meerdere stappen gestart om het firmwarepakket te downloaden, het firmwarepakket toe te passen en ten slotte opnieuw verbinding te maken met de IoT Hub-service.  Gedurende het proces met meerdere stappen wordt gebruikgemaakt van de gerapporteerde eigenschappen van de apparaattwin om de voortgang en status van het apparaat bij te werken.
  
    ![Afbeelding van het firmware-updatepatroon van apparaatbeheer via Azure IoT Hub][img-fwupdate_pattern]
* **Voortgang en status rapporteren**: met de back-endtoepassing worden query's voor apparaatdubbels uitgevoerd op een set apparaten om de status en voortgang te rapporteren van acties die op de apparaten worden uitgevoerd.
  
    ![Afbeelding van het proces- en statuspatroon van apparaatbeheerrapportage in Azure IoT Hub][img-report_progress_pattern]

## <a name="next-steps"></a>Volgende stappen
U kunt de mogelijkheden, patronen en codebibliotheken die Azure IoT Hub-apparaatbeheer biedt, gebruiken voor het maken van IoT-toepassingen die voldoen aan de enterprise-IoT-operatorvereisten in elke fase van de levenscyclus van een apparaat.

Zie de zelfstudie [Get started with Azure IoT Hub device management][lnk-get-started] (Aan de slag met apparaatbeheer via Azure IoT Hub) om nog meer te leren over de functies voor apparaatbeheer via Azure IoT Hub.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md


<!--HONumber=Oct16_HO3-->


