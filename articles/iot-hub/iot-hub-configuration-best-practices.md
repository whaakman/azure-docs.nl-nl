---
title: Apparaat-configuratie aanbevolen procedures voor Azure IoT Hub | Microsoft Docs
description: Meer informatie over aanbevolen procedures voor het configureren van IoT-apparaten op schaal
author: chrisgre
ms.author: chrisgre
ms.date: 06/24/2018
ms.topic: conceptual
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: 5eb0ba659961d809d0ae471034b03263f87e3894
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985495"
---
# <a name="best-practices-for-device-configuration-within-an-iot-solution"></a>Aanbevolen procedures voor de configuratie van het apparaat in een IoT-oplossing

Automatische Apparaatbeheer in Azure IoT Hub automatiseert veel herhaalde en complexe taken van grote apparaat vloten via het geheel van de levenscycli hiervan te beheren. In dit artikel worden veel van de aanbevolen procedures voor de verschillende rollen die betrokken zijn bij het ontwikkelen en het besturingssysteem van een IoT-oplossing.

* **IoT hardware fabrikant/integrator:** fabrikanten van IoT-hardware, integrators aan elkaar te koppelen hardware van verschillende fabrikanten of leveranciers leveren van hardware voor een IoT-implementatie geproduceerd of geïntegreerd met andere leveranciers. Bij het ontwikkelen en integreren van de firmware, embedded-besturingssystemen en ingesloten software.

* **Ontwikkelaarshandleiding voor IoT-oplossing:** de ontwikkeling van een IoT-oplossing wordt doorgaans uitgevoerd door een ontwikkelaar van de oplossing. Deze ontwikkelaar kan deel uitmaken van een interne team of een systeemintegrator dat is gespecialiseerd in deze activiteit. De ontwikkelaar van de IoT-oplossing kan verschillende onderdelen van de IoT-oplossing helemaal ontwikkelen, integreren van verschillende standard- of open-source-onderdelen of aanpassen een [IoT-oplossingsversnellers](/azure/iot-accelerators/).

* **IoT-oplossing operator:** na de IoT-oplossing is geïmplementeerd, moet op de lange termijn gebruik, bewaking, upgrades en onderhoud. Deze taken kunnen worden gedaan door een interne team dat bestaat uit informatie technologie-specialisten, hardwarebewerkingen en onderhoud teams en domein-specialisten die de juiste werking van de algemene IoT-infrastructuur bewaken.

## <a name="understand-automatic-device-management-for-configuring-iot-devices-at-scale"></a>Informatie over het beheer van de automatische inrichting voor het configureren van IoT-apparaten op schaal

Automatische inrichting beheer omvat de vele voordelen van [apparaatdubbels](iot-hub-devguide-device-twins.md) en [moduledubbels](iot-hub-devguide-module-twins.md) om te synchroniseren van de gewenste en de gerapporteerde status tussen de cloud en apparaten. [Automatische apparaatconfiguraties] [lnk-auto-apparaat-config] automatisch bijwerken van grote gegevenssets dubbels en summerize wordt uitgevoerd en naleving. De volgende geavanceerde stappen wordt beschreven hoe automatische Apparaatbeheer is ontwikkeld en gebruikt:

* De **IoT hardware fabrikant/integrator** functies voor Apparaatbeheer in een ingesloten toepassing met behulp implementeert [apparaatdubbels](iot-hub-devguide-device-twins.md). Deze functies kunnen omvatten firmware-updates, software-installatie en update en instellingen beheren.

* De **IoT-oplossing developer** implementeert de beheerlaag van bewerkingen voor Apparaatbeheer met behulp van [apparaatdubbels](iot-hub-devguide-device-twins.md) en [automatische apparaatconfiguraties](iot-hub-auto-device-config.md). De oplossing moet opnemen met het definiëren van een operatorinterface om apparaat-beheertaken uitvoeren.

* De **IoT-oplossing operator** maakt gebruik van de IoT-oplossing beheertaken apparaat, met name om apparaten te groeperen samen, wijzigingen in de configuratie, zoals firmware-updates initiëren, voortgang controleren en oplossen van problemen die ontstaan.

## <a name="iot-hardware-manufacturerintegrator"></a>IoT hardware fabrikant/integrator

Hier volgen de aanbevolen procedures voor hardwarefabrikanten en omgaan met ingesloten softwareontwikkeling integrators:

* **Implementeer [apparaatdubbels](iot-hub-devguide-device-twins.md):** apparaatdubbels inschakelen voor het synchroniseren van gewenst Configuratiebeheer vanuit de cloud en voor het rapporteren van de huidige configuratie- en apparaat-eigenschappen. De beste manier voor het implementeren van apparaatdubbels binnen embedded-toepassingen is via de [Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks). Apparaatdubbels zijn bij uitstek geschikt voor de configuratie, omdat ze:

    * Ondersteuning voor bidirectionele communicatie.
    * Voor beide apparaatstatussen verbonden en niet-verbonden toestaan.
    * Ga als volgt het principe van uiteindelijke consistentie.
    * Volledig Apparaatmetagegevens zich in de cloud.

* **Structuur van het dubbele apparaat voor beheer van apparaten:** het dubbele apparaat moet worden gestructureerd zodat management apparaateigenschappen logisch zijn gegroepeerd in secties. In dat geval kunnen wijzigingen in de configuratie moeten worden geïsoleerd zonder gevolgen voor andere gedeelten van het dubbele. Maak bijvoorbeeld een sectie in de gewenste eigenschappen voor de firmware, een andere sectie voor software, en een derde sectie netwerkinstellingen. 

* **Rapport apparaatkenmerken die nuttig voor het beheer van apparaten zijn:** kenmerken, zoals fysieke apparaten merk en model, firmware, besturingssystemen, serienummer, en andere id's zijn nuttig voor rapportage en als parameters die zijn gericht op wijzigingen in de configuratie.

* **De belangrijkste Staten voor het melden van status en voortgang te definiëren:** op het hoogste niveau status moeten worden geïnventariseerd, zodat deze kunnen worden gerapporteerd aan de operator. Bijvoorbeeld, rapporteren een firmware-update status als de huidige, downloaden, toepassen, wordt uitgevoerd en fout. Extra velden voor meer informatie over elke status definiëren.

## <a name="iot-solution-developer"></a>Ontwikkelaarshandleiding voor IoT-oplossing

Hier volgen de aanbevolen procedures voor ontwikkelaars van de IoT-oplossingen die aan systemen op basis van in Azure werken:

* **Implementeer [apparaatdubbels](iot-hub-devguide-device-twins.md):** apparaatdubbels inschakelen voor het synchroniseren van gewenst Configuratiebeheer vanuit de cloud en voor het rapporteren van de huidige configuratie- en apparaat-eigenschappen. De beste manier voor het implementeren van apparaatdubbels in cloud solutions toepassingen is via de [Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks). Apparaatdubbels zijn bij uitstek geschikt voor de configuratie, omdat ze:

    * Ondersteuning voor bidirectionele communicatie.
    * Voor beide apparaatstatussen verbonden en niet-verbonden toestaan. 
    * Ga als volgt het principe van uiteindelijke consistentie.
    * Volledig Apparaatmetagegevens zich in de cloud.

* **Apparaten met apparaat dubbele tags:** de oplossing moet de operator voor het definiëren van kwaliteit ringen of andere sets met apparaten op basis van verschillende implementatiestrategieën voor, zoals Canarische toestaan. Apparaat-organisatie kan worden geïmplementeerd in uw oplossing met behulp van apparaat apparaatdubbel-tags en [query's](iot-hub-devguide-query-language.md). Apparaat-organisatie is die nodig zijn om toe te staan voor de configuratie van implementatie-outs veilig en nauwkeurig.

* **Implementeer [automatische apparaatconfiguraties](iot-hub-auto-device-config.md):** automatische apparaatconfiguraties implementeren en monitorconfiguratie wordt gewijzigd op grote sets van IoT-apparaten via dubbele apparaten. Automatische apparaatconfiguraties doel sets met dubbele apparaten via de **voorwaarde, doel** die een query op apparaat apparaatdubbel-tags of gerapporteerde eigenschappen. De **inhoud richten** is de set van gewenste eigenschappen die in de betreffende apparaatdubbels worden ingesteld. De inhoud van het doel moet worden uitgelijnd met de apparaat-twin-structuur gedefinieerd door de fabrikant IoT hardware/integrator. 

   De **metrische gegevens** zijn query's op het dubbele apparaat gerapporteerde eigenschappen en moeten ook worden uitgelijnd met de apparaat-twin-structuur gedefinieerd door de fabrikant IoT hardware/integrator. Automatische inrichting configuraties hebben ook het voordeel van IoT-Hub uitvoeren van bewerkingen van dubbele apparaat met een snelheid die nooit overschrijdt de [beperkingslimieten](iot-hub-devguide-quotas-throttling.md) voor apparaat dubbele leesbewerkingen en updates.

* **Gebruik de [Device Provisioning Service](../iot-dps/how-to-manage-enrollments.md):** ontwikkelaars, moeten de Device Provisioning Service gebruiken om te device twin labels toewijzen aan nieuwe apparaten, zodat ze automatisch worden geconfigureerd door  **Automatische apparaatconfiguraties** die zijn gericht op dubbele met die tag. 

## <a name="iot-solution-operator"></a>Operator voor IoT-oplossing

Hier volgen de aanbevolen procedures voor IoT-oplossing operators die met behulp van een IoT-oplossing die is gebouwd op Azure:

* **Apparaten voor beheer:** de IoT-oplossing moet definiëren of toe te staan voor het maken van kwaliteit ringen of andere sets met apparaten op basis van verschillende implementatiestrategieën voor, zoals Canarische. De sets met apparaten wordt gebruikt om te implementeren wijzigingen in de configuratie en andere bewerkingen voor Apparaatbeheer op schaal uitvoeren.

* **Uitvoeren van wijzigingen in de configuratie met behulp van een gefaseerde uitrol:** een gefaseerde uitrol is een algemene proces waarbij wijzigingen in een operator worden geïmplementeerd op een leven set IoT-apparaten. Het doel is om wijzigingen aanbrengen geleidelijk in vermindert het risico van het maken van grote schaal belangrijke wijzigingen.  De operator moet de interface van de oplossing gebruiken om te maken een [automatische apparaatconfiguratie](iot-hub-auto-device-config.md) en de doelitems voorwaarde moet het doel een eerste set met apparaten (zoals een canary groep). De operator moet de configuratiewijziging in de eerste set met apparaten vervolgens te valideren. 

   Nadat de validatie voltooid is, wordt de operator de configuratie van automatische inrichting om op te nemen van een grotere set van apparaten bijgewerkt. De operator moet de prioriteit voor de configuratie moet hoger zijn dan andere configuraties die momenteel wordt toegepast op die apparaten ook worden ingesteld. De implementatie kan worden gecontroleerd met behulp van de metrische gegevens gerapporteerd door de configuratie van automatische inrichting. 

* **Terugdraaiacties in het geval van fouten of onjuiste configuraties uitvoeren:** een automatische apparaatconfiguratie die ervoor zorgt fouten of onjuiste configuraties dat kan worden teruggedraaid door het veranderen van de **die gericht is op voorwaarde** zodat de apparaten geen meer aan de doelitems voorwaarde voldoen. Zorg ervoor dat een andere automatische configuratie met een lagere prioriteit nog steeds voor deze apparaten is gericht. Controleren of het terugdraaien is geslaagd door de metrische gegevens weer te geven: status voor untargeted apparaten niet meer moet worden weergegeven door de service-Update-back-configuratie en metrische gegevens van de configuratie van de tweede bevatten nu aantallen voor de apparaten die nog steeds zijn gericht.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het implementeren van apparaatdubbels in [apparaatdubbels begrijpen en gebruiken in IoT Hub](iot-hub-devguide-device-twins.md).

* Doorloop de stappen voor het maken, bijwerken of verwijderen van een configuratie voor automatische inrichting in [configureren en controleren van IoT-apparaten op schaal](iot-hub-auto-device-config.md).

* Implementeren van een firmware-update-patroon met behulp van apparaatdubbels en automatische apparaatconfiguraties in [zelfstudie: een apparaat firmware-updateproces implementeren](tutorial-firmware-update.md). 
