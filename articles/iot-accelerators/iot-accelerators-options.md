---
title: IoT-opties voor Microsoft Azure | Microsoft Docs
description: Kies hoe u uw IoT-oplossing wilt implementeren met behulp van Azure IoT-oplossingsversnellers, Azure IoT Central of Azure IoT Hub.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: 56a19e9cbeb6e7d30171d23ff918a34ba423d2f2
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34725578"
---
# <a name="compare-azure-iot-options"></a>Azure IoT-opties vergelijken

Het artikel [Azure en het Internet of Things](iot-accelerators-what-is-azure-iot.md) beschrijft een typische IoT-oplossingsarchitectuur met de volgende lagen:

* Connectiviteit en beheer van apparaten
* Gegevensverwerking en -analyse
* Presentatie en bedrijfsintegratie

Azure IoT biedt verschillende opties voor het implementeren van deze architectuur, elk geschikt voor verschillende klantvereisten:

* [Azure IoT-oplossingsversnellers](../active-directory-domain-services/index.yml) is een verzameling geavanceerde [oplossingsversnellers](iot-accelerators-what-are-solution-accelerators.md), gebouwd op Azure PaaS (Platform as a Service), waarmee u sneller aangepaste IoT-oplossingen kunt ontwikkelen.

* [Azure IoT Central](https://www.microsoft.com/internet-of-things/iot-central-saas-solutions) is een SaaS-oplossing (Software as a Service) die gebruikmaakt van een op modellen gebaseerde benadering waarmee u hoogwaardige IoT-oplossingen kunt bouwen zonder kennis van het ontwikkelen van cloudoplossingen.

## <a name="azure-iot-hub"></a>Azure IoT Hub

Azure IoT Hub is de kern-PaaS van Azure waarvan zowel Azure IoT Central als Azure IoT-oplossingsversnellers gebruikmaken. IoT Hub maakt stabiele en veilige tweerichtingscommunicatie tussen miljoenen IoT-apparaten en een cloudoplossing mogelijk. Met IoT Hub kunt u de uitdagingen van IoT-implementatie aan, zoals:

* Connectiviteit en beheer van grote volumes aan apparaten.
* Telemetrieopname van groot volume.
* Opdracht en besturing van apparaten.
* Afdwinging van apparaatbeveiliging.

## <a name="compare-azure-iot-solution-accelerators-and-azure-iot-central"></a>Azure IoT-oplossingsversnellers en Azure IoT Central vergelijken

Het kiezen van uw Azure IoT-product is een belangrijk onderdeel van het plannen van uw IoT-oplossing. IoT Hub is een afzonderlijke Azure-service die zelfstandig geen volledige IoT-oplossing biedt. IoT Hub kan worden gebruikt als uitgangspunt voor elke IoT-oplossing. U hoeft hiervoor niet Azure IoT-oplossingsversnellers of Azure IoT Central te gebruiken. Zowel Azure IoT-oplossingsversnellers als Azure IoT Central maken gebruik van IoT Hub, samen met andere Azure-services. De volgende tabel geeft een overzicht van de belangrijkste verschillen tussen Azure IoT-oplossingsversnellers en Azure IoT Central, om u te helpen de juiste oplossing voor uw vereisten te kiezen:

|                        | Azure IoT-oplossingsversnellers | Azure IoT Central |
| ---------------------- | --------- | ----------- |
| Primair gebruik | Snellere ontwikkeling van een aangepaste IoT-oplossing die maximale flexibiliteit nodig heeft. | Kortere time-to-market voor eenvoudige IoT-oplossingen waarvoor geen uitgebreide serviceaanpassing is vereist. |
| Toegang tot onderliggende PaaS-services          | U hebt toegang tot de onderliggende Azure-services om deze te beheren of indien nodig te vervangen. | SaaS. Volledig beheerde oplossing; de onderliggende services worden niet weergegeven. |
| Flexibiliteit            | Hoog. De code voor de microservices is open-source en u kunt deze op elke gewenste manier wijzigen. Daarnaast kunt u de implementatie-infrastructuur aanpassen.| Gemiddeld. U kunt de ingebouwde browsergebaseerde gebruikerservaring gebruiken om het oplossingsmodel en aspecten van de gebruikersinterface aan te passen. De infrastructuur kan niet worden aangepast, omdat de verschillende onderdelen niet worden weergegeven.|
| Kennisniveau                 | Gemiddeld-hoog. U heeft ervaring met Java of .NET nodig om de back-end van de oplossing aan te passen. U heeft ervaring met JavaScript nodig om de visualisatie aan te passen. | Laag. U heeft modelleerervaring nodig om de oplossing aan te passen. Ervaring met codering is niet vereist. |
| Opstartervaring | Oplossingsversnellers implementeren algemene IoT-scenario's. Kan in enkele minuten worden geïmplementeerd. | Toepassingssjablonen en apparaatsjablonen bieden vooraf gebouwde modellen. Kan in enkele minuten worden geïmplementeerd. |
| Prijzen                | U kunt de services afstellen om de kosten te beperken. | Eenvoudige, voorspelbare prijsstructuur. |

Uw keuze voor welk product u wilt gebruiken om uw IoT-oplossing te maken, wordt uiteindelijk bepaald door:

* Uw zakelijke vereisten.
* Het type oplossing dat u wilt maken.
* De kwalificaties van uw organisatie voor het maken en op de lange termijn onderhouden van de oplossing.

## <a name="next-steps"></a>Volgende stappen

Op basis van uw gekozen product en benadering, zijn dit de voorgestelde volgende stappen:

* **Azure IoT-oplossingsversnellers**: [Wat zijn de Azure IoT-oplossingsversnellers?](iot-accelerators-what-are-solution-accelerators.md)
* **Azure IoT Central**: [Azure IoT Central](https://www.microsoft.com/internet-of-things/iot-central-saas-solutions).
* **IoT Hub**: [overzicht van de service Azure IoT Hub](../iot-hub/iot-hub-what-is-iot-hub.md).
