---
title: Aanbevolen procedures voor apparaat configuratie voor Azure IoT Hub | Microsoft Docs
description: Meer informatie over aanbevolen procedures voor het configureren van IoT-apparaten op schaal
author: chrisgre
manager: briz
ms.author: chrisgre
ms.date: 06/24/2018
ms.topic: conceptual
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: 7314c5ec05bec61e5bbbc406b6a39a7c5a8f011f
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035876"
---
# <a name="best-practices-for-device-configuration-within-an-iot-solution"></a>Aanbevolen procedures voor de configuratie van het apparaat binnen een IoT-oplossing

Automatische Apparaatbeheer in Azure IoT Hub automatiseert veel herhaalde en complexe taken van het beheer van grote apparaat vloten via het geheel van hun levenscycli. In dit artikel worden veel van de aanbevolen procedures voor de verschillende rollen die zijn betrokken bij de ontwikkeling en het besturingssysteem van een IoT-oplossing.

* **IoT hardware fabrikant/integrator:** fabrikanten van IoT-hardware, SI's hardware montage van verschillende fabrikanten of leveranciers bieden hardware voor de implementatie van een IoT geproduceerd of geïntegreerd met andere leveranciers. Betrokken bij de ontwikkeling en integratie van de firmware, embedded-besturingssystemen en ingesloten software.
* **IoT-oplossing developer:** de ontwikkeling van een IoT-oplossing gewoonlijk wordt uitgevoerd door de ontwikkelaar van een oplossing. Deze ontwikkelaar kan deel uitmaken van een intern team dat of een system integrator die zijn gespecialiseerd in deze activiteit. De ontwikkelaar van de IoT-oplossing kan verschillende onderdelen van de IoT-oplossing vanaf het begin te ontwikkelen, integreren van verschillende standaard of open source-onderdelen of aanpassen een [IoT oplossingsverbetering][lnk-solution].
* **IoT-oplossing operator:** na de IoT-oplossing is geïmplementeerd, langlopende bewerkingen, bewaking, upgrades en onderhoud is vereist. Deze taken kunnen worden uitgevoerd door een interne team dat bestaat uit gegevens technologiespecialisten, hardwarebewerkingen en onderhoud teams en domein specialisten die de juiste werking van de algemene IoT-infrastructuur bewaken.

## <a name="understand-automatic-device-management-for-configuring-iot-devices-at-scale"></a>Automatische Apparaatbeheer voor het configureren van IoT-apparaten op schaal begrijpen

Automatische Apparaatbeheer omvat de vele voordelen van [apparaat horende] [ lnk-device-twins] en [module horende] [ lnk-module-twins] gewenst synchroniseren en gerapporteerde Staten tussen cloud en apparaten.  [Automatische apparaatconfiguraties] [ lnk-auto-device-config] automatisch bijwerken van grote gegevenssets horende en summerize uitgevoerd en naleving. De volgende geavanceerde stappen wordt beschreven hoe automatische Apparaatbeheer is ontwikkeld en gebruikt:

* De **IoT hardware fabrikant/integrator** implementeert beheerfuncties voor apparaten in een ingesloten toepassing met behulp [apparaat horende][lnk-device-twins]. Deze functies kunnen omvatten firmware-updates, software-installatie en update en instellingen beheren.
* De **IoT-oplossing developer** implementeert de beheerlaag van het beheer van apparaten met behulp van [apparaat horende] [ lnk-device-twins] en [automatische inrichting configuraties][lnk-auto-device-config]. De oplossing moet opnemen met het definiëren van een operator-interface om uit te voeren taken voor het beheer van apparaten.
* De **IoT-oplossing operator** maakt gebruik van de IoT-oplossing beheertaken apparaat, met name om apparaten te groeperen samen, configuratiewijzigingen zoals firmware-updates initiëren voortgang controleren en oplossen van problemen die ontstaan.

## <a name="iot-hardware-manufacturerintegrator"></a>IoT hardware fabrikant/integrator

De volgende zijn aanbevolen procedures voor hardwarefabrikanten en omgaan met ingebouwde software development SI's:

* **Implementeer [apparaat horende][lnk-device-twins]:** apparaat horende inschakelen synchroniseren van de gewenste configuratie uit de cloud en voor het melden van eigenschappen van het huidige configuratie en het apparaat.  De beste manier voor het implementeren van apparaat horende binnen embedded-toepassingen is via de [Azure IoT SDK's][lnk-azure-sdk].  Apparaat horende geschikt zijn voor configuratie omdat ze: een. ondersteuning voor bidirectionele communicatie b. toestaan dat voor beide statussen verbonden en niet-verbonden apparaat c. Volg het principe van de uiteindelijke consistentie d. volledig queriable zijn in de cloud
* **Structuur van het apparaat twin voor Apparaatbeheer:** de apparaat-twin moet zodanig zijn dat eigenschappen voor beheer van apparaten logisch zijn gegroepeerd in secties. In dat geval kunnen configuratiewijzigingen kunnen worden geïsoleerd zonder enige impact op andere gedeelten van de twin. Maak bijvoorbeeld een sectie binnen de gewenste eigenschappen voor firmware, een andere sectie voor software, en een derde punt voor instellingen voor netwerken. 
* **Apparaatkenmerken die handig voor het beheer van apparaten zijn rapporteren:** kenmerken zoals fysiek apparaat merk en model, firmware, besturingssystemen, serienummer, en andere id's zijn handig voor rapportage en als parameters voor het afstemmen wijzigingen in configuratie.
* **Definieer de belangrijkste statussen voor het melden van de status en voortgang:** op het hoogste niveau status moeten worden geïnventariseerd, zodat ze worden gerapporteerd aan de operator. Bijvoorbeeld, rapporteren een firmware-update status als de huidige, downloaden, toepassen, wordt uitgevoerd en fout.  Extra velden voor meer informatie over elke status definiëren.  

## <a name="iot-solution-developer"></a>Ontwikkelaars van IoT-oplossing

Hier volgen de aanbevolen procedures voor ontwikkelaars van de IoT-oplossingen die op basis van Azure-systemen bouwen zijn:

* **Implementeer [apparaat horende][lnk-device-twins]:** apparaat horende inschakelen synchroniseren van de gewenste configuratie uit de cloud en voor het melden van eigenschappen van het huidige configuratie en het apparaat.  De beste manier voor het implementeren van apparaat horende binnen cloud solutions toepassingen is via de [Azure IoT SDK's][lnk-azure-sdk].  Apparaat horende geschikt zijn voor configuratie omdat ze: een. ondersteuning voor bidirectionele communicatie b. toestaan dat voor beide statussen verbonden en niet-verbonden apparaat c. Volg het principe van de uiteindelijke consistentie d. volledig queriable zijn in de cloud
* **Apparaten met behulp van apparaat twin labels indelen:** de oplossing moet de operator voor het definiëren van de kwaliteit ringen of andere groepen van apparaten op basis van verschillende implementatiestrategieën zoals Canarische toestaan. Apparaat-organisatie kan worden geïmplementeerd in uw oplossing met apparaat twin tags en [query's][lnk-queries].  Apparaat-organisatie is moet mogelijk configuratie roll outs veilig en correct.
* **Implementeer [automatische apparaatconfiguraties][lnk-auto-device-config]:** automatische apparaatconfiguraties implementeren en monitor configuratiewijzigingen naar grote verzamelingen van IoT-apparaten via horende apparaten.  Automatische apparaatconfiguraties doel sets horende apparaten via de **doel voorwaarde** die wordt een query op apparaat twin tags of eigenschappen gerapporteerd. De **inhoud** is het aantal gewenste eigenschappen die binnen het betreffende apparaat horende worden ingesteld. De inhoud van het doel moet worden uitgelijnd met de apparaat-twin structuur gedefinieerd door de fabrikant IoT hardware/integrator.  De **metrische gegevens** zijn query's op het apparaat twin eigenschappen gerapporteerd, en moeten ook worden uitgelijnd met de apparaat-twin structuur gedefinieerd door de fabrikant IoT hardware/integrator. Automatische inrichting configuraties hebben ook het voordeel van IoT-Hub uitvoeren apparaat twin bewerkingen met een frequentie die wordt nooit groter is dan de [beperking limieten] [ lnk-throttling] voor apparaat twin leest en updates.
* **Gebruik [apparaat inrichtingsservice][lnk-dps]:** oplossing moet u de Service voor het inrichten van apparaten apparaat twin labels toewijzen aan nieuwe apparaten, zodat ze automatisch worden geconfigureerd door **automatische apparaatconfiguraties** die zijn gericht op horende met dit label. 

## <a name="iot-solution-operator"></a>IoT-oplossing operator

Hier volgen de aanbevolen procedures voor operators van de IoT-oplossing die met behulp van een IoT-oplossing gebouwd op Azure:

* **Apparaten voor beheer indelen:** de IoT-oplossing moet definiëren of voor het maken van de kwaliteit ringen of andere groepen van apparaten op basis van verschillende implementatiestrategieën zoals Canarische toestaan. De sets van apparaten wordt gebruikt uitrolt configuratiewijzigingen en andere beheerbewerkingen voor geschaald apparaat uitvoeren.
* **Uitvoeren met behulp van een gefaseerde implementeert configuratiewijzigingen:** een gefaseerde implementeert is een algemene proces waarbij een operator wijzigingen op een leven reeks IoT-apparaten implementeert. Het doel is om te wijzigen geleidelijk vermindert het risico van het maken van grote schaal wijzigingen op te splitsen.  De operator moet van de oplossing interface gebruiken om te maken van een [automatische apparaatconfiguratie][lnk-auto-device-config], en de doelitems voorwaarde moet het doel een eerste set apparaten (zoals een kokospalm groep).  De operator moet de wijziging in de configuratie in de eerste set apparaten vervolgens te valideren.  Zodra de validatie is voltooid, wordt de configuratie van de automatische inrichting om een grotere set van apparaten bijgewerkt in de operator. De operator moet de prioriteit voor de configuratie van hoger zijn dan andere configuraties die momenteel zijn gericht op die apparaten ook worden ingesteld.  Het implementatieproces worden gecontroleerd met behulp van de metrische gegevens die door de automatische configuratie. 
* **Rollback in geval van fouten of configuratiefouten uitvoeren:** een automatische apparaatconfiguratie dat ervoor zorgt fouten of configuratiefouten dat kan worden teruggedraaid door het wijzigen van de **die gericht is op voorwaarde** zodat de apparaten geen meer aan de doelitems voorwaarde voldoen.  Zorg ervoor dat een andere automatische configuratie met een lagere prioriteit nog steeds is bedoeld voor deze apparaten.  Controleer of dat het terugdraaien is gelukt door de metrische gegevens weer te geven: de teruggedraaid-back-configuratie moet de status voor untargeted apparaten niet meer weergeven en de tweede configuratie metrische gegevens behoren nu tellingen voor de apparaten die nog steeds zijn gericht.


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het implementeren van apparaat horende in [Understand and gebruik apparaat horende in IoT-Hub][lnk-device-twins]
* Doorloop de stappen voor het maken, bijwerken of verwijderen van een configuratie voor automatische inrichting in [configureren en controleren van de IoT-apparaten op schaal][lnk-auto-device-config].
* Implementeren van een firmware-update-patroon met apparaat horende en configuraties van automatische inrichting in [zelfstudie: een proces apparaat firmware-update implementeren][lnk-firmware-update].

<!-- Links -->
[lnk-device-twins]: iot-hub-devguide-device-twins.md
[lnk-module-twins]: iot-hub-devguide-module-twins.md
[lnk-auto-device-config]: iot-hub-auto-device-config.md
[lnk-firmware-update]: tutorial-firmware-update.md
[lnk-throttling]: iot-hub-devguide-quotas-throttling.md
[lnk-queries]: iot-hub-devguide-query-language.md
[lnk-dps]: ../iot-dps/how-to-manage-enrollments.md
[lnk-azure-sdk]: https://github.com/Azure/azure-iot-sdks
[lnk-solution]: https://azure.microsoft.com/features/iot-accelerators/