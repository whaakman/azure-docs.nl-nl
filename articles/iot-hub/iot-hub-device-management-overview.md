---
title: Overzicht van Apparaatbeheer met Azure IoT Hub | Microsoft Docs
description: Overzicht van Apparaatbeheer in Azure IoT Hu--enterprise-apparaat levenscyclus en apparaatbeheerpatronen, zoals opnieuw opstarten, Fabrieksinstellingen terugzetten, firmware-update, configuratie, apparaatdubbels, query's, -taken.
author: bzurcher
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/24/2017
ms.author: briz
ms.openlocfilehash: bdc55af23568b5785a831e81f352400c728c902e
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043551"
---
# <a name="overview-of-device-management-with-iot-hub"></a>Overzicht van apparaatbeheer met IoT Hub

Azure IoT Hub biedt de functies en het uitbreidingsmodel waarmee apparaat- en back-end-ontwikkelaars krachtige beheeroplossingen voor apparaten kunnen bouwen. Apparaten variëren van beperkte sensoren en microcontrollers met één doel tot krachtige gateways die communicatie routeren voor groepen apparaten.  Bovendien verschillen de gebruiksvoorbeelden en de vereisten voor IoT-operators aanzienlijk in de verschillende sectoren.  Ondanks deze variatie biedt apparaatbeheer met IoT Hub mogelijkheden, patronen en codebibliotheken die geschikt zijn voor een groot aantal apparaten en eindgebruikers.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Een essentieel onderdeel van het maken van een geslaagde enterprise-IoT-oplossing is het bieden van een strategie voor hoe operators omgaan met het continue beheer van de apparaten in hun bedrijf. IoT-operators hebben eenvoudige en betrouwbare hulpprogramma's en toepassingen nodig waarmee ze zich kunnen concentreren op de strategische aspecten van hun werk. Dit artikel bevat:

* Een kort overzicht van de Azure IoT Hub-aanpak van apparaatbeheer.
* Een beschrijving van de algemene principes van apparaatbeheer.
* Een beschrijving van de levenscyclus van het apparaat.
* Een overzicht van de algemene patronen van apparaatbeheer.

## <a name="device-management-principles"></a>Principes van apparaatbeheer

IoT zorgt voor unieke apparaatbeheeruitdagingen en elke oplossing van enterprise-klasse moet rekening houden met de volgende principes:

![Afbeelding Principes van apparaatbeheer](media/iot-hub-device-management-overview/image4.png)

* **Schaal en automatisering**: voor IoT-oplossingen zijn eenvoudige hulpprogramma's nodig waarmee routinetaken kunnen worden geautomatiseerd en waarmee een relatief klein aantal operationele medewerkers miljoenen apparaten kan beheren. Operators verwachten dagelijks apparaatbewerkingen extern en bulksgewijs uit te voeren en alleen te worden gewaarschuwd wanneer er problemen opduiken waarvoor hun directe aandacht is vereist.

* **Openheid en compatibiliteit**: het ecosysteem van apparaten is buitengewoon divers. Beheerhulpprogramma's moeten zo worden aangepast dat ze kunnen omgaan met een groot aantal apparaatklassen, platforms en protocollen. Operators moeten vele typen apparaten kunnen ondersteunen, van de meest beperkte ingesloten chips voor enkelvoudige processen tot krachtige en volledig functionele computers.

* **Contextbewustzijn**: IoT-omgevingen zijn dynamisch en veranderen voortdurend. Betrouwbaarheid van de service is cruciaal. Tijdens bewerkingen voor apparaatbeheer moet er rekening worden gehouden met de volgende factoren, om ervoor te zorgen dat uitvaltijd wegens onderhoud de kritieke bedrijfsbewerkingen niet beïnvloedt en geen gevaarlijke situaties creëert:

    * SLA-onderhoudsvensters
    * Netwerk- en voedingsstatussen
    * Voorwaarden wanneer in gebruik
    * Geolocatie van apparaat

* **Vele rollen bedienen**: ondersteuning voor de unieke werkstromen en processen van IoT-bewerkingsrollen is cruciaal. Beheerders moeten in harmonie met de gegeven beperkingen van interne IT-afdelingen werken.  Ze moeten ook duurzame manieren vinden om in realtime gegevens over apparaatbewerkingen door te sturen naar toezichthouders en andere zakelijke leidinggevende rollen.

## <a name="device-lifecycle"></a>Levenscyclus van apparaat
Er is een set algemene fasen voor apparaatbeheer die hetzelfde zijn voor alle enterprise-IoT-projecten. In Azure IoT zijn er vijf fasen binnen de levenscyclus van een apparaat:

![De vijf fasen van de levenscyclus van een Azure IoT-apparaat: plannen, inrichten, configureren, bewaken, buiten gebruik stellen](./media/iot-hub-device-management-overview/image5.png)

Binnen elk van deze vijf fasen zijn er verschillende vereisten voor de apparaatoperator waaraan moet worden voldaan voor een volledige oplossing:

* **Plannen**: operators in staat stellen een schema voor de metagegevens van een apparaat te maken waarmee ze eenvoudig en nauwkeurig kunnen zoeken naar en zich richten op een groep apparaten voor bulksgewijze beheerbewerkingen. U kunt de apparaatdubbel gebruiken om de metagegevens van dit apparaat op te slaan in de vorm van tags en eigenschappen.
  
    *Meer weten*: 
    * [Aan de slag met apparaatdubbels](iot-hub-node-node-twin-getstarted.md)
    * [Meer informatie over apparaatdubbels](iot-hub-devguide-device-twins.md)
    * [Apparaatdubbeleigenschappen gebruiken](tutorial-device-twins.md)
    * [Aanbevolen procedures voor de configuratie van het apparaat in een IoT-oplossing](iot-hub-configuration-best-practices.md)

* **Inrichten**: nieuwe apparaten veilig inrichten voor IoT Hub en operators in staat stellen apparaatmogelijkheden onmiddellijk te detecteren.  Gebruik het id-register van IoT Hub om flexibele apparaat-id's en -referenties te maken. Voer dit uit in één bulkbewerking met behulp van een taak. Bouw apparaten om hun mogelijkheden en voorwaarden via apparaateigenschappen te rapporteren in de apparaatdubbel.
  
    *Meer weten*: 
    * [Apparaatidentiteiten beheren](iot-hub-devguide-identity-registry.md)
    * [Bulk-beheer van apparaat-id 's](iot-hub-bulk-identity-mgmt.md)
    * [Apparaatdubbeleigenschappen gebruiken](tutorial-device-twins.md)
    * [Aanbevolen procedures voor de configuratie van het apparaat in een IoT-oplossing](iot-hub-configuration-best-practices.md)
    * [Azure IoT Hub Device Provisioning Service](https://azure.microsoft.com/documentation/services/iot-dps)

* **Configureren**: bulksgewijze configuratiewijzigingen en firmware-updates op apparaten ondersteunen terwijl de status en beveiliging behouden blijven. Voer deze apparaatbeheerbewerkingen bulksgewijs uit met behulp van de gewenste eigenschappen of met rechtstreekse methoden en broadcast-taken.
  
    *Meer weten*:
    * [Apparaatdubbeleigenschappen gebruiken](tutorial-device-twins.md)
    * [Configureren en controleren van IoT-apparaten op schaal](iot-hub-auto-device-config.md)
    * [Aanbevolen procedures voor de configuratie van het apparaat in een IoT-oplossing](iot-hub-configuration-best-practices.md)

* **Bewaken**: de verzamelde status van alle apparaten bewaken, de status van lopende bewerkingen bewaken en operators attenderen op problemen die mogelijk hun aandacht vereisen.  Pas de apparaatdubbel toe, zodat apparaten in realtime bewerkingsvoorwaarden en de status van de update-bewerkingen kunnen rapporteren. Bouw krachtige dashboardrapporten die de meeste directe problemen melden via apparaatdubbel-query's.
  
    *Meer weten*: 
    * [Apparaatdubbeleigenschappen gebruiken](tutorial-device-twins.md)
    * [IoT Hub-querytaal voor apparaatdubbels, taken en berichtroutering](iot-hub-devguide-query-language.md)
    * [Configureren en controleren van IoT-apparaten op schaal](iot-hub-auto-device-config.md)
    * [Aanbevolen procedures voor de configuratie van het apparaat in een IoT-oplossing](iot-hub-configuration-best-practices.md)

* **Buiten gebruik stellen**: Vervang of apparaten uit bedrijf nemen na een storing, upgradecyclus of aan het einde van de levensduur van de service.  Gebruik de apparaatdubbel om apparaatgegevens te onderhouden als het fysieke apparaat wordt vervangen, of te archiveren als het apparaat buiten gebruik wordt gesteld. Gebruik het id-register van IoT Hub voor het veilig intrekken van apparaat-id's en -referenties.
  
    *Meer weten*: 
    * [Apparaatdubbeleigenschappen gebruiken](tutorial-device-twins.md)
    * [Apparaatidentiteiten beheren](iot-hub-devguide-identity-registry.md)

## <a name="device-management-patterns"></a>Patronen voor apparaatbeheer

Met IoT Hub wordt de volgende set apparaatbeheerpatronen mogelijk gemaakt. De [zelfstudies Apparaatbeheer](iot-hub-node-node-device-management-get-started.md) ziet u hoe u deze patronen aanpassen aan uw exacte scenario en hoe u nieuwe patronen op basis van deze kernsjablonen ontwerpt in meer detail.

* **Opnieuw opstarten**: de back-endtoepassing wordt het apparaat geïnformeerd via een rechtstreekse methode dat er een opnieuw opstarten is gestart.  Het apparaat maakt gebruik van de gerapporteerde eigenschappen om de opstartstatus van het apparaat bij te werken.
  
    ![Afbeelding van het opstartpatroon van apparaatbeheer](./media/iot-hub-device-management-overview/reboot-pattern.png)

* **Fabrieksinstellingen terugzetten**: de back-endtoepassing wordt het apparaat geïnformeerd via een rechtstreekse methode dat het terugzetten van de fabrieksinstellingen is gestart. Het apparaat maakt gebruik van de gerapporteerde eigenschappen om de status van het terugzetten van de fabrieksinstellingen van het apparaat bij te werken.
  
    ![Afbeelding van het patroon van herstel naar fabrieksinstellingen voor apparaatbeheer](./media/iot-hub-device-management-overview/facreset-pattern.png)

* **Configuratie**: de back-end-app maakt gebruik van de gewenste eigenschappen voor het configureren van software die wordt uitgevoerd op het apparaat. Het apparaat maakt gebruik van de gerapporteerde eigenschappen om de configuratiestatus van het apparaat bij te werken.
  
    ![Afbeelding van het configuratiepatroon van apparaatbeheer](./media/iot-hub-device-management-overview/configuration-pattern.png)

* **Firmware-Update**: de back-end-app maakt gebruik van een configuratie van automatische device management om te selecteren van de apparaten voor het ontvangen van de update, om te zien van de apparaten waar u op zoek naar de update en het controleren van het updateproces. Het apparaat wordt een proces voor het downloaden, controleren, en de firmware-installatiekopie toe te passen en vervolgens het apparaat opnieuw opstarten voordat u opnieuw verbinding maken met de IoT Hub-service. Gedurende het proces met meerdere stappen wordt gebruikgemaakt van de gerapporteerde eigenschappen om de voortgang en status van het apparaat bij te werken.
  
    ![Afbeelding van het firmware-updatepatroon van apparaatbeheer](media/iot-hub-device-management-overview/fwupdate-pattern.png)

* **Voortgang en status rapporteren**: de back-end van de oplossing worden dubbele query uitgevoerd in een set apparaten om te rapporteren over de status en voortgang van de acties die worden uitgevoerd op de apparaten.
  
    ![Afbeelding van het proces- en statuspatroon van apparaatbeheerrapportage](./media/iot-hub-device-management-overview/report-progress-pattern.png)

## <a name="next-steps"></a>Volgende stappen

U kunt de mogelijkheden, patronen en codebibliotheken die IoT Hub biedt voor apparaatbeheer, gebruiken voor het maken van IoT-toepassingen die voldoen aan de enterprise-IoT-operatorvereisten in elke fase van de levenscyclus van een apparaat.

Om meer te leren over de functies voor Apparaatbeheer in IoT Hub, Zie de [aan de slag met Apparaatbeheer](iot-hub-node-node-device-management-get-started.md) zelfstudie.