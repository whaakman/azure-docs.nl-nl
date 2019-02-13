---
title: IoT-opties voor Microsoft Azure | Microsoft Docs
description: Kies hoe u uw Azure IoT-oplossing wilt implementeren met behulp van Azure IoT Central, IoT-oplossingsversnellers of met IoT Hub.
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: 370e9068ea4395069a34e70a44664ffd72edb3fb
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823689"
---
# <a name="compare-azure-iot-central-and-azure-iot-options"></a>Opties van Azure IoT Central vergelijken met die van Azure IoT

Microsoft Azure IoT Central en Azure IoT bieden verschillende opties voor het implementeren van een IoT-oplossing, die elk geschikt zijn voor verschillende klantvereisten:

* [Azure IoT Central](overview-iot-central-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) is een SaaS-oplossing (Software as a Service) die gebruikmaakt van een op modellen gebaseerde benadering waarmee u hoogwaardige IoT-oplossingen kunt bouwen zonder kennis van het ontwikkelen van cloudoplossingen.

* [Azure IoT-oplossingsversnellers](https://docs.microsoft.com/azure/iot-accelerators/) is een verzameling geavanceerde [oplossingsversnellers](../iot-accelerators/iot-accelerators-what-are-solution-accelerators.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json), gebouwd op Azure PaaS (Platform as a Service), waarmee u sneller aangepaste IoT-oplossingen kunt ontwikkelen.

## <a name="azure-iot-hub"></a>Azure IoT Hub

Azure IoT Hub is de kern-PaaS van Azure waarvan zowel Azure IoT Central als Azure IoT-oplossingsversnellers gebruikmaken. IoT Hub zorgt voor stabiele en veilige tweerichtingscommunicatie tussen miljoenen IoT-apparaten en een cloudoplossing. Met IoT Hub kunt u de uitdagingen van IoT-implementatie aan, zoals:

* Connectiviteit en beheer van grote volumes aan apparaten.
* Telemetrieopname van groot volume.
* Opdracht en besturing van apparaten.
* Afdwinging van apparaatbeveiliging.

## <a name="compare-azure-iot-central-and-azure-iot-solution-accelerators"></a>Azure IoT Central en Azure IoT-oplossingsversnellers met elkaar vergelijken

Het kiezen van uw Azure IoT-product is een belangrijk onderdeel van het plannen van uw IoT-oplossing. IoT Hub is een afzonderlijke Azure-service die zelfstandig geen volledige IoT-oplossing biedt. IoT Hub kan worden gebruikt als uitgangspunt voor elke IoT-oplossing. U hoeft hiervoor niet Azure IoT-oplossingsversnellers of Azure IoT Central te gebruiken. Zowel IoT-oplossingsversnellers als Azure IoT Central maken gebruik van IoT Hub, samen met andere Azure-services. De volgende tabel geeft een overzicht van de belangrijkste verschillen tussen IoT-oplossingsversnellers en Azure IoT Central, om u te helpen de juiste oplossing voor uw vereisten te kiezen:

|     | Azure IoT Central | Azure IoT-oplossingsversnellers |
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

* **Azure IoT Central**: [Azure IoT Central](overview-iot-central-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).
* **IoT-oplossingsversnellers**: [Wat zijn de Azure IoT-oplossingsversnellers?](../iot-accelerators/iot-accelerators-what-are-solution-accelerators.md)
* **IoT Hub**: [Overzicht van de service Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub).