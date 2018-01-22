---
title: IoT-opties voor Microsoft Azure | Microsoft Docs
description: Kies hoe u uw IoT-oplossing wilt implementeren met behulp van Azure Iot Suite, Microsoft IoT Central of Azure IoT Hub.
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.assetid: 2d38d08a-4133-4e5c-8b28-f93cadb5df05
ms.service: iot-suite
ms.topic: get-started-article
ms.date: 11/10/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1a979bde37d247da5c630547924cadbd79c4a6a4
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="compare-azure-iot-options"></a>Azure IoT-opties vergelijken

Het artikel [Azure en het Internet of Things](iot-suite-what-is-azure-iot.md) beschrijft een typische IoT-oplossingsarchitectuur met de volgende lagen:

* Connectiviteit en beheer van apparaten
* Gegevensverwerking en -analyse
* Presentatie en bedrijfsintegratie

Azure IoT biedt verschillende opties voor het implementeren van deze architectuur, elk geschikt voor verschillende klantvereisten:

* [Azure IoT Suite](index.md) is een verzameling met [vooraf geconfigureerde oplossingen](iot-suite-what-are-preconfigured-solutions.md) op bedrijfsniveau, gebouwd op Azure PaaS (Platform as a Service), waarmee u aangepaste IoT-oplossingen sneller kunt ontwikkelen.

* [Microsoft IoT Central](https://www.microsoft.com/internet-of-things/iot-central-saas-solutions) is een SaaS-oplossing (Software as a Service) die gebruikmaakt van een op modellen gebaseerde benadering waarmee u IoT-oplossingen op bedrijfsniveau kunt bouwen zonder kennis van het ontwikkelen van cloudoplossingen.

## <a name="azure-iot-hub"></a>Azure IoT Hub

Azure IoT Hub is de kern-PaaS van Azure waarvan zowel Microsoft IoT Central als Azure IoT Suite gebruikmaken. IoT Hub maakt stabiele en veilige tweerichtingscommunicatie tussen miljoenen IoT-apparaten en een cloudoplossing mogelijk. Met IoT Hub kunt u de uitdagingen van IoT-implementatie aan, zoals:

* Connectiviteit en beheer van grote volumes aan apparaten.
* Telemetrieopname van groot volume.
* Opdracht en besturing van apparaten.
* Afdwinging van apparaatbeveiliging.

## <a name="compare-azure-iot-suite-and-microsoft-iot-central"></a>Azure IoT Suite en Microsoft IoT Central vergelijken

Het kiezen van uw Azure IoT-product is een belangrijk onderdeel van het plannen van uw IoT-oplossing. IoT Hub is een afzonderlijke Azure-service die zelfstandig geen volledige IoT-oplossing biedt. IoT Hub kan worden gebruikt als uitgangspunt voor elke IoT-oplossing. U hoeft hiervoor niet Azure IoT Suite of Microsoft IoT Central te gebruiken. Zowel Azure IoT Suite als Microsoft IoT Central maken gebruik van IoT Hub, samen met andere Azure-services. De volgende tabel geeft een overzicht van de belangrijkste verschillen tussen Azure IoT Suite en Microsoft IoT Central, om u te helpen de juiste oplossing voor uw vereisten te kiezen:

|                        | Azure IoT Suite | Microsoft IoT Central |
| ---------------------- | --------- | ----------- |
| Primair gebruik | Snellere ontwikkeling van een aangepaste IoT-oplossing die maximale flexibiliteit nodig heeft. | Kortere time-to-market voor eenvoudige IoT-oplossingen waarvoor geen uitgebreide serviceaanpassing is vereist. |
| Toegang tot onderliggende PaaS-services          | U hebt toegang tot de onderliggende Azure-services om deze te beheren of indien nodig te vervangen. | SaaS. Volledig beheerde oplossing; de onderliggende services worden niet weergegeven. |
| Flexibiliteit            | Hoog. De code voor de microservices is open-source en u kunt deze op elke gewenste manier wijzigen. Daarnaast kunt u de implementatie-infrastructuur aanpassen.| Gemiddeld. U kunt de ingebouwde browsergebaseerde gebruikerservaring gebruiken om het oplossingsmodel en aspecten van de gebruikersinterface aan te passen. De infrastructuur kan niet worden aangepast, omdat de verschillende onderdelen niet worden weergegeven.|
| Kennisniveau                 | Gemiddeld-hoog. U heeft ervaring met Java of .NET nodig om de back-end van de oplossing aan te passen. U heeft ervaring met JavaScript nodig om de visualisatie aan te passen. | Laag. U heeft modelleerervaring nodig om de oplossing aan te passen. Ervaring met codering is niet vereist. |
| Opstartervaring | Vooraf geconfigureerde oplossingen implementeren algemene IoT-scenario's. Kan in enkele minuten worden geïmplementeerd. | Toepassingssjablonen en apparaatsjablonen bieden vooraf gebouwde modellen. Kan in enkele minuten worden geïmplementeerd. |
| Prijzen                | U kunt de services afstellen om de kosten te beperken. | Eenvoudige, voorspelbare prijsstructuur. |

Uw keuze voor welk product u wilt gebruiken om uw IoT-oplossing te maken, wordt uiteindelijk bepaald door:

* Uw zakelijke vereisten.
* Het type oplossing dat u wilt maken.
* De kwalificaties van uw organisatie voor het maken en op de lange termijn onderhouden van de oplossing.

## <a name="next-steps"></a>Volgende stappen

Op basis van uw gekozen product en benadering, zijn dit de voorgestelde volgende stappen:

* **Azure IoT Suite**: [wat zijn de vooraf geconfigureerde Azure IoT-oplossingen?](iot-suite-what-are-preconfigured-solutions.md)
* **Microsoft IoT Central**: [Microsoft IoT Central](https://www.microsoft.com/internet-of-things/iot-central-saas-solutions).
* **IoT Hub**: [overzicht van de service Azure IoT Hub](../iot-hub/iot-hub-what-is-iot-hub.md).
