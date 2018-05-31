---
title: IoT-opties voor Microsoft Azure | Microsoft Docs
description: Kies hoe u uw Azure IoT-oplossing wilt implementeren met Azure IoT Central, IoT Suite of IoT Hub.
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: overview
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 8ed524e87f820f6c1e2e05da0bcbc7241bdd1ec1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34201455"
---
# <a name="compare-azure-iot-central-and-azure-iot-options"></a>Opties van Azure IoT Central vergelijken met die van Azure IoT

Microsoft Azure IoT Central en Azure IoT bieden verschillende opties voor het implementeren van een IoT-oplossing, die elk geschikt zijn voor verschillende klantvereisten:

* [Azure IoT Central](overview-iot-central.md) is een SaaS-oplossing (Software as a Service) die gebruikmaakt van een op modellen gebaseerde benadering waarmee u hoogwaardige IoT-oplossingen kunt bouwen zonder kennis van het ontwikkelen van cloudoplossingen.

* [Azure IoT Suite](https://docs.microsoft.com/azure/iot-suite/) is een verzameling met [vooraf geconfigureerde oplossingen](https://docs.microsoft.com/azure/iot-suite/iot-suite-what-are-preconfigured-solutions) op bedrijfsniveau, gebouwd op Azure PaaS (Platform as a Service), waarmee u aangepaste IoT-oplossingen sneller kunt ontwikkelen.

## <a name="azure-iot-hub"></a>Azure IoT Hub

Azure IoT Hub is de belangrijkste Azure PaaS waarvan zowel Azure IoT Central als Azure IoT Suite gebruikmaken. IoT Hub zorgt voor stabiele en veilige tweerichtingscommunicatie tussen miljoenen IoT-apparaten en een cloudoplossing. Met IoT Hub kunt u de uitdagingen van IoT-implementatie aan, zoals:

* Connectiviteit en beheer van grote volumes aan apparaten.
* Telemetrieopname van groot volume.
* Opdracht en besturing van apparaten.
* Afdwinging van apparaatbeveiliging.

## <a name="compare-azure-iot-central-and-azure-iot-suite"></a>Azure IoT Central vergelijken met Azure IoT Suite

Het kiezen van uw Azure IoT-product is een belangrijk onderdeel van het plannen van uw IoT-oplossing. IoT Hub is een afzonderlijke Azure-service die zelfstandig geen volledige IoT-oplossing biedt. IoT Hub kan worden gebruikt als startpunt voor elke IoT-oplossing. Bovendien hebt u hiervoor Azure IoT Suite of Azure IoT Central niet nodig. Zowel IoT Suite als Azure IoT Central maakt gebruik van IoT Hub, samen met andere Azure-services. In de volgende tabel worden de belangrijkste verschillen tussen IoT Suite en Azure IoT Central weergegeven, zodat u de optie kunt kiezen die het best aansluit bij uw vereisten:

|     | Azure IoT Central | Azure IoT Suite |
| --- | ----------- | --------- |
| Primair gebruik                      | Kortere time-to-market voor eenvoudige IoT-oplossingen waarvoor geen uitgebreide serviceaanpassing is vereist.                                                    | Snellere ontwikkeling van een aangepaste IoT-oplossing die maximale flexibiliteit nodig heeft.                                                                                                                             |
| Toegang tot onderliggende PaaS-services | SaaS. Volledig beheerde oplossing; de onderliggende services worden niet weergegeven.                                                                                            | U hebt toegang tot de onderliggende Azure-services om deze te beheren of indien nodig te vervangen.                                                                                                                    |
| Flexibiliteit                        | Gemiddeld. U kunt de ingebouwde browsergebaseerde gebruikerservaring gebruiken om het oplossingsmodel en aspecten van de gebruikersinterface aan te passen. De infrastructuur kan niet worden aangepast, omdat de verschillende onderdelen niet worden weergegeven. | Hoog. De code voor de microservices is open-source en u kunt deze op elke gewenste manier wijzigen. Daarnaast kunt u de implementatie-infrastructuur aanpassen.                                               |
| Kennisniveau                        | Laag. U heeft modelleerervaring nodig om de oplossing aan te passen. Ervaring met codering is niet vereist.                                                                          | Gemiddeld-hoog. U heeft ervaring met Java of .NET nodig om de back-end van de oplossing aan te passen. U heeft ervaring met JavaScript nodig om de visualisatie aan te passen.                                                                       |
| Opstartervaring             | Toepassingssjablonen en apparaatsjablonen bieden vooraf gebouwde modellen. Kan in enkele minuten worden geïmplementeerd.                                                                                                  | Vooraf geconfigureerde oplossingen implementeren algemene IoT-scenario's. Kan in enkele minuten worden geïmplementeerd.                                                                                                                            |
| Prijzen                            | Eenvoudige, voorspelbare prijsstructuur.                                                                                                                           | U kunt de services afstellen om de kosten te beperken.                                                                                                                                                            |

Uw keuze voor welk product u wilt gebruiken om uw IoT-oplossing te maken, wordt uiteindelijk bepaald door:

* Uw zakelijke vereisten.
* Het type oplossing dat u wilt maken.
* De kwalificaties van uw organisatie voor het maken en op de lange termijn onderhouden van de oplossing.

## <a name="next-steps"></a>Volgende stappen

Op basis van uw gekozen product en benadering, zijn dit de voorgestelde volgende stappen:

* **Azure IoT Central**: [Azure IoT Central](overview-iot-central.md).
* **IoT Suite**: [wat zijn de vooraf geconfigureerde Azure IoT-oplossingen?](https://docs.microsoft.com/azure/iot-suite/iot-suite-what-are-preconfigured-solutions).
* **IoT Hub**: [overzicht van de service Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub).