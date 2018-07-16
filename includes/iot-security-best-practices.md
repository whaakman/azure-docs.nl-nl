---
title: bestand opnemen
description: bestand opnemen
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 2c6f5cf2d89da0c2418ac58ca5d47a8aa05e732f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38945257"
---
# <a name="internet-of-things-security-best-practices"></a>Aanbevolen procedures voor beveiliging van Internet of Things

Beveiligen van een Internet of Things (IoT)-infrastructuur is een strenge beveiliging in de diepte strategie vereist. Deze strategie vereist dat u voor het beveiligen van gegevens in de cloud, de integriteit van gegevens in transit beveiligen via het openbare internet en apparaten veilig inrichten. Elke laag bouwt groter beveiligingscontrole in de algehele infrastructuur.

## <a name="secure-an-iot-infrastructure"></a>Een IoT-infrastructuur beveiligen

Deze strategie voor beveiliging in de diepte kan worden ontwikkeld en uitgevoerd met actieve deelname van de verschillende spelers betrokken zijn bij de productie, ontwikkeling en implementatie van IoT-apparaten en infrastructuur. Hieronder volgt een beschrijving op hoog niveau van deze spelers.

* **IoT hardware fabrikant/integrator**: deze spelers zijn meestal de fabrikanten van IoT-hardware die wordt geïmplementeerd, integrators aan elkaar te koppelen hardware van verschillende fabrikanten of leveranciers leveren van hardware voor een IoT-implementatie gemaakte of geïntegreerd met andere leveranciers.
* **Ontwikkelaarshandleiding voor IoT-oplossing**: de ontwikkeling van een IoT-oplossing wordt doorgaans uitgevoerd door een ontwikkelaar van de oplossing. Deze ontwikkelaar mogelijk deel van een interne team of een systeemintegrator (SI) dat is gespecialiseerd in deze activiteit. De ontwikkelaar van de IoT-oplossing kunt verschillende onderdelen van de IoT-oplossing helemaal ontwikkelen, integreren van verschillende gebruiksklare of open-source-onderdelen of oplossingsversnellers met kleine aanpassing vast.
* **IoT-oplossing deployer**: nadat een IoT-oplossing is ontwikkeld, moet dit worden geïmplementeerd in het veld. Dit proces omvat het implementeren van de hardware, koppeling van apparaten en implementatie van oplossingen in de hardware-apparaten en in de cloud.
* **IoT-oplossing operator**: nadat de IoT-oplossing is geïmplementeerd, moet op de lange termijn gebruik, bewaking, upgrades en onderhoud. Deze taken kunnen worden gedaan door een interne team dat bestaat uit informatie technologie-specialisten, hardwarebewerkingen en onderhoud teams en domein-specialisten die de juiste werking van algemene IoT-infrastructuur bewaken.

De volgende secties bieden aanbevolen procedures voor elk van deze spelers te ontwikkelen, implementeren en uitvoeren van een veilige IoT-infrastructuur.

## <a name="iot-hardware-manufacturerintegrator"></a>IoT hardware fabrikant/integrator

Hier volgen de aanbevolen procedures voor IoT-hardwarefabrikanten en hardware integrators.

* **Het bereik van hardware aan de minimale vereisten voor**: het hardwareontwerp van de de minimale functies die vereist zijn voor de werking van de hardware, en niets meer moet bevatten. Een voorbeeld is het USB-poorten zijn alleen indien nodig voor de werking van het apparaat. Deze extra functies opent u het apparaat voor ongewenste aanvalsvectoren die moeten worden vermeden.
* **Hardwarewijziging knoeien bewijs**: bouw mechanismen voor het detecteren van fysieke knoeien, zoals het openen van de dekking van het apparaat of een deel van het apparaat verwijderen. Deze signalen knoeien deel uitmaken van de gegevensstroom geüpload naar de cloud, daardoor exploitanten van deze gebeurtenissen waarschuwen kan.
* **Bouwen om beveiligde hardware**: als COGS toestaat, beveiligingsfuncties, zoals beveiligd en versleuteld opslag bouwen, of deze opstarten functionaliteit op basis van Trusted Platform Module (TPM). Deze functies kunt u apparaten meer beveiligen en beschermen van de algemene IoT-infrastructuur.
* **Beveiligen van upgrades**: Firmware-upgrades tijdens de levensduur van het apparaat zijn onvermijdelijk. Het bouwen van apparaten met beveiligde paden voor upgrades en cryptografische controle van de firmware-versies kunnen worden beveiligd tijdens en na de upgrade van het apparaat.

## <a name="iot-solution-developer"></a>Ontwikkelaarshandleiding voor IoT-oplossing

Hier volgen de aanbevolen procedures voor ontwikkelaars van IoT-oplossingen:

* **Ga als volgt beveiligde software ontwikkelingsmethodologie**: ontwikkeling van veilige software vereist volledig nadenken over de beveiliging van het begin van het project helemaal tot aan de implementatie, testen en implementeren. De keuzes van platforms, talen en hulpprogramma's, worden met deze methode beïnvloed. De Microsoft Security Development Lifecycle biedt een stapsgewijze benadering voor het ontwikkelen van veilige software.
* **Kies open-sourcesoftware zorgvuldig**: Open-source-software biedt de mogelijkheid voor het snel ontwikkelen van oplossingen. Bij de keuze van open-source-software, houd rekening met het activiteitenniveau van de community voor elke open-source-component. Een actieve community zorgt ervoor dat de software wordt ondersteund en dat problemen worden gedetecteerd en opgelost. U kunt ook een softwareproject cryptisch en inactieve open-source-wordt mogelijk niet ondersteund en problemen worden niet waarschijnlijk worden gedetecteerd.
* **Integreren met zorg**: veel software beveiligingsfouten bestaat op de grens van de API's en bibliotheken. Functionaliteit die niet vereist voor de huidige implementatie zijn mogelijk nog steeds beschikbaar is via een API-laag. Algemene beveiliging, zorg ervoor dat zodat om te controleren of alle interfaces van onderdelen voor beveiligingsfouten wordt geïntegreerd.

## <a name="iot-solution-deployer"></a>Implementatie van IoT-oplossing

Hier volgen de aanbevolen procedures voor IoT-oplossing deployers:

* **Hardware veilig implementeren**: IoT-implementaties vereisen hardware om te worden geïmplementeerd in niet-beveiligde locaties, zoals in openbare ruimten of zonder supervisie landinstellingen. In dergelijke situaties, zorg ervoor dat de implementatie van de hardware fraudebestendig zoveel. Als het USB- of andere poorten beschikbaar op de hardware zijn, zorgt u ervoor dat ze veilig zijn gedekt. Veel aanvalsvectoren kunnen deze als toegangspunten gebruiken.
* **Verificatiesleutels veilig te houden**: tijdens de implementatie van elk apparaat vereist dat apparaat-id's en verificatiesleutels die worden gegenereerd door de service in de cloud die is gekoppeld. Deze sleutels fysiek veilig afschermen na de implementatie. Een willekeurige toets waarmee is geknoeid kan door een schadelijke apparaat worden gebruikt voor het zich voordoen als een bestaand apparaat.

## <a name="iot-solution-operator"></a>Operator voor IoT-oplossing

Hier volgen de aanbevolen procedures voor IoT-oplossing-operators:

* **Het systeem up-to-date houden**: Zorg ervoor dat de besturingssystemen van apparaten en alle apparaatstuurprogramma's worden bijgewerkt naar de meest recente versies. Als u automatische updates in Windows 10 (IoT of andere SKU's), Microsoft bijgehouden, bieden een veilige besturingssysteem voor IoT-apparaten. Andere besturingssystemen (zoals Linux) te houden ervoor up-to-date zorgt dat ze ook zijn beschermd tegen aanvallen van buitenaf.
* **Bescherming tegen schadelijke activiteiten**: als het besturingssysteem toestaat, installeert u de nieuwste antivirus- en -mogelijkheden op het besturingssysteem voor elk apparaat. Met deze procedure kunt de meeste externe bedreigingen te verhelpen. U kunt de meeste moderne besturingssystemen tegen bedreigingen beveiligen door passende maatregelen te nemen.
* **Regelmatig controleren**: controle van IoT-infrastructuur voor het aanpakken van beveiligingsproblemen is essentieel bij het reageren op beveiligingsincidenten. De meeste besturingssystemen bieden ingebouwde logboekregistratie die vaak moet worden gecontroleerd om ervoor te zorgen dat er geen inbreuk op de beveiliging is opgetreden. Controle-informatie kan worden verzonden als een afzonderlijke telemetriestroom in de cloud-service waar ze kan worden geanalyseerd.
* **De IoT-infrastructuur fysiek beveiligen**: de slechtste beveiligingsaanvallen tegen IoT-infrastructuur worden gestart met behulp van de fysieke toegang tot apparaten. Een belangrijk veiligheid procedure is ter bescherming tegen kwaadaardig gebruik van USB-poorten en andere fysieke toegang. Een sleutel te onthullen schendingen die zijn opgetreden is logboekregistratie van fysieke toegang, zoals USB-poort gebruikt. Windows 10 (IoT en andere SKU's) kunt ook gedetailleerde logboekregistratie van deze gebeurtenissen.
* **Cloudreferenties beveiligen**: referenties voor Cloud-verificatie gebruikt voor het configureren en gebruiken van een IoT-implementatie zijn mogelijk de eenvoudigste manier om toegang te krijgen en een IoT-systeem. De referenties worden beveiligd door het wachtwoord te vaak te wijzigen en ervan weerhouden met deze referenties op openbare virtuele machines.

Mogelijkheden van andere IoT-apparaten variëren. Sommige apparaten mogelijk computers met algemene pc-besturingssystemen en sommige apparaten kunnen zeer lichte-besturingssystemen worden uitgevoerd. De aanbevolen procedures voor beveiliging beschreven eerder mogelijk van toepassing zijn op deze apparaten in verschillende mate. Indien opgegeven, kan extra beveiliging en implementatie van best practices uit de fabrikanten van deze apparaten moeten worden gevolgd.

Sommige verouderde en beperkte apparaten mogelijk niet hebben is speciaal ontworpen voor IoT-implementatie. Deze apparaten hebben de mogelijkheid voor het versleutelen van gegevens, verbinding maken met het Internet of geavanceerde controle bieden mogelijk geen. In dergelijke gevallen kan een moderne en veilige veldgateway aggregatiegegevens van verouderde apparaten en voorzien in de beveiliging die vereist zijn voor deze apparaten via Internet verbinding te maken. Veldgateways kunnen bieden veilige verificatie, onderhandeling van versleutelde sessies, ontvangst van de opdrachten in de cloud en veel andere beveiligingsfuncties.