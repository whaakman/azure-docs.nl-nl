<properties
 pageTitle="Overzicht van apparaatbeheer | Microsoft Azure"
 description="Overzicht van apparaatbeheer via Azure IoT Hub"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/16/2016"
 ms.author="bzurcher"/>




# Overzicht van apparaatbeheer via Azure IoT Hub (preview-versie)

## De methode voor apparaatbeheer via Azure IoT

Apparaatbeheer via Azure IoT Hub biedt de functies en het uitbreidingsmodel voor apparaten en back-ends om gebruik te maken van IoT-apparaatbeheer voor de uiteenlopende apparaten en protocollen in IoT.  Apparaten in IoT variëren van zeer beperkte sensoren, microcontrollers met één doel, tot krachtigere gateways waarmee andere apparaten en protocollen worden ingeschakeld.  IoT-oplossingen verschillen ook aanzienlijk in de verticale domeinen en toepassingen met unieke gebruiksvoorbeelden voor operators in elk domein.  Voor IoT-oplossingen kan gebruik worden gemaakt van apparaatbeheermogelijkheden, patronen en codebibliotheken via IoT Hub, zodat apparaatbeheer voor de grote verscheidenheid aan apparaten en gebruikers mogelijk wordt.  

## Inleiding

Een essentieel onderdeel van het maken van een geslaagde IoT-oplossing, is het bieden van een strategie voor hoe operators omgaan met het continue beheer van de apparaten in hun bedrijf. IoT-operators hebben hulpprogramma's en toepassingen nodig die zowel eenvoudig als betrouwbaar zijn en waarmee ze zich kunnen concentreren op de strategische aspecten van hun werk. Azure IoT Hub biedt u de bouwstenen voor het maken van IoT-toepassingen die de belangrijkste apparaatbeheerpatronen ondersteunen.

Apparaten worden beschouwd als beheerd met IoT Hub wanneer er een eenvoudige toepassing, een apparaatbeheeragent, op wordt uitgevoerd. Hiermee wordt het apparaat veilig met de cloud verbonden. Met de agentcode kan een operator vanaf de toepassingszijde extern de apparaatstatus bevestigen en beheerbewerkingen uitvoeren zoals het toepassen van netwerkconfiguratiewijzigingen of het implementeren van firmware-updates.

## Principes van IoT-apparaatbeheer

IoT zorgt voor unieke beheeruitdagingen en een oplossing moet rekening houden met de volgende principes van IoT-apparaatbeheer:

![][img-dm_principles]

- **Schaal en automatisering**: voor IoT zijn eenvoudige hulpprogramma's nodig waarmee routinetaken kunnen worden geautomatiseerd en waarmee een relatief klein aantal operationele medewerkers miljoenen apparaten kan beheren. Operators verwachten dagelijks apparaatbewerkingen extern en bulksgewijs uit te voeren en alleen te worden gewaarschuwd wanneer er problemen opduiken waarvoor hun directe aandacht is vereist.

- **Openheid en compatibiliteit**: het ecosysteem van IoT-apparaten is buitengewoon divers. Beheerhulpprogramma's moeten zo worden aangepast dat ze kunnen omgaan met een groot aantal apparaatklassen, platforms en protocollen. Operators moeten alle apparaten kunnen ondersteunen, van de meest beperkte ingesloten chips voor enkelvoudige processen tot krachtige en volledig functionele computers.

- **Contextbewustzijn**: IoT-omgevingen zijn dynamisch en veranderen voortdurend. Betrouwbaarheid van de service is cruciaal. Bij apparaatbeheerbewerkingen moet rekening worden gehouden met SLA-onderhoudsvensters, netwerk- en energiestatussen, in-gebruiksomstandigheden en apparaatgeolocatie om ervoor te zorgen dat de uitval vanwege onderhoud niet van invloed is op kritieke bedrijfsactiviteiten of dat er geen gevaarlijke situaties ontstaan.

- **Vele rollen bedienen**: ondersteuning voor de unieke werkstromen en processen van IoT-bewerkingsrollen is cruciaal. De operationele medewerkers moeten ook harmonieus werken met de gegeven beperkingen van interne IT-afdelingen en relevante apparaatbewerkingsgegevens zichtbaar maken voor supervisors en andere managementrollen.

## Levenscyclus van IoT-apparaat 

Hoewel IoT-projecten veel van elkaar verschillen, is er een set algemene patronen voor het beheren van apparaten. In Azure IoT worden deze patronen geïdentificeerd binnen de levenscyclus van het IoT-apparaat die uit vijf afzonderlijke fasen bestaat:

![][img-device_lifecycle]

1. **Plannen**: operators in staat stellen een apparaateigenschappenschema te maken waarmee ze eenvoudig en nauwkeurig kunnen zoeken naar en zich richten op een groep apparaten voor bulksgewijze beheerbewerkingen.

    *Gerelateerde bouwstenen*: [Aan de slag met apparaattwins][lnk-twins-getstarted], [Twineigenschappen gebruiken][lnk-twin-properties]

2. **Inrichten**: nieuwe apparaten veilig verifiëren voor IoT Hub en operators in staat stellen apparaatmogelijkheden en de huidige status onmiddellijk te detecteren.

    *Gerelateerde bouwstenen*: [Aan de slag met IoT Hub][lnk-hub-getstarted], [Twineigenschappen gebruiken][lnk-twin-properties]

3. **Configureren**: bulksgewijze configuratiewijzigingen en firmware-updates op apparaten ondersteunen terwijl de status en beveiliging behouden blijven.

    *Gerelateerde bouwstenen*: [Twineigenschappen gebruiken][lnk-twin-properties], [C2D-methoden][lnk-c2d-methods], [Taken plannen/uitzenden][lnk-jobs]

4. **Bewaken**: algehele status van alle apparaten en de status van lopende update-implementaties bewaken om operators te attenderen op problemen die mogelijk hun aandacht vereisen.

    *Gerelateerde bouwstenen*: [Twineigenschappen gebruiken][lnk-twin-properties]

5. **Buiten gebruik stellen**: apparaten vervangen of uit bedrijf nemen na een storing, upgradecyclus of aan het einde van de levensduur.

    *Gerelateerde bouwstenen*:
    
## Patronen voor apparaatbeheer via IoT Hub

Met IoT Hub wordt de volgende set (eerste) apparaatbeheerpatronen mogelijk gemaakt.  Zoals getoond in de [zelfstudies][lnk-get-started] kunt u deze patronen aanpassen aan uw exacte scenario en nieuwe patronen ontwerpen voor andere scenario's op basis van deze kernpatronen.

1. **Opnieuw opstarten** - Via de back-endtoepassing wordt het apparaat met een D2C-methode geïnformeerd dat opnieuw opstarten is gestart.  Het apparaat maakt gebruik van de gerapporteerde eigenschappen in de apparaattwin om de opstartstatus van het apparaat bij te werken. 

    ![][img-reboot_pattern]

2. **Fabrieksinstellingen terugzetten ** - Via de back-endtoepassing wordt het apparaat via een D2C-methode geïnformeerd dat het terugzetten van de fabrieksinstellingen is gestart.  Het apparaat maakt gebruik van de gerapporteerde eigenschappen van de apparaattwin om de status van het terugzetten van de fabrieksinstellingen van het apparaat bij te werken.

    ![][img-facreset_pattern]

3. **Configuratie** - Voor de back-endtoepassing worden de gewenste eigenschappen van de apparaattwin gebruikt voor het configureren van software die op het apparaat wordt uitgevoerd.  Het apparaat maakt gebruik van de gerapporteerde eigenschappen van de apparaattwin om de configuratiestatus van het apparaat bij te werken. 

    ![][img-config_pattern]

4. **Firmware-update** - Via de back-endtoepassing wordt het apparaat via een D2C-methode geïnformeerd dat een firmware-update is gestart.  Voor het apparaat wordt een proces met meerdere stappen gestart om het firmwarepakket te downloaden, het firmwarepakket toe te passen en ten slotte opnieuw verbinding te maken met de IoT Hub-service.  Gedurende het proces met meerdere stappen wordt gebruikgemaakt van de gerapporteerde eigenschappen van de apparaattwin om de voortgang en status van het apparaat bij te werken. 

    ![][img-fwupdate_pattern]

5. **Voortgang en status rapporteren** Met de back-endtoepassing worden query's voor apparaattwins uitgevoerd op een set apparaten om de status en voortgang te rapporteren van acties die op het apparaat worden uitgevoerd.

    ![][img-report_progress_pattern]

## Volgende stappen

Met de bouwstenen die met Azure IoT Hub worden geleverd, kunnen ontwikkelaars IoT-toepassingen maken die voldoen aan de unieke IoT-operatorvereisten in elke levenscyclusfase van een apparaat.

Zie de zelfstudie [Get started with Azure IoT Hub device management][lnk-get-started] (Aan de slag met apparaatbeheer via Azure IoT Hub) om nog meer te leren over de functies voor apparaatbeheer via Azure IoT Hub.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md


<!--HONumber=Oct16_HO1-->


