---
title: Uw Internet of Things (IoT) beveiligen in azure | Microsoft Docs
description: " Micro services van Azure Internet der dingen (IoT) bieden een breed scala aan mogelijkheden. In dit artikel wordt uitgelegd hoe u uw IoT-oplossingen in azure kunt beveiligen. "
services: security
documentationcenter: na
author: TomShinder
manager: barbkess
editor: TomSh
ms.assetid: 1473c8dd-8669-48fb-86db-b3c50e2eaf59
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 670fcc5e92cdb9937026d91ee85b7e8e856a3025
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727035"
---
# <a name="internet-of-things-security-overview"></a>Overzicht van Internet of Things beveiliging
Micro services van Azure Internet der dingen (IoT) bieden een breed scala aan mogelijkheden. Met deze hoogwaardige services kunt u het volgende doen:

* Gegevens van apparaten verzamelen
* Gegevensstromen in beweging analyseren
* Grote gegevenssets opslaan en er query’s op uitvoeren
* Gegevens in realtime en historische gegevens visualiseren
* Integraties met back-officesystemen uitvoeren

Om deze mogelijkheden te bieden, bundelen Azure IoT Solution Accelerators meerdere Azure-Services met aangepaste extensies als vooraf geconfigureerde oplossingen. Deze vooraf geconfigureerde oplossingen zijn basisimplementaties van algemene patronen van IoT-oplossingen die u helpen de tijd voor het leveren van uw IoT-oplossingen te verkorten. Met de IoT-Software Development Kits kunt u deze oplossingen aanpassen en uitbreiden om te voldoen aan uw eigen vereisten. U kunt deze oplossingen gebruiken als voorbeelden of sjablonen als u nieuwe IoT-oplossingen ontwikkelt.

De Azure IoT-oplossings versnellers zijn krachtige oplossingen voor uw IoT-behoeften. Het is echter van het grootste belang dat uw IoT-oplossingen zijn ontworpen met het oog op het begin van de beveiliging. Vanwege het Sheer-aantal IoT-apparaten kan elk beveiligings incident snel worden omgezet in een wijd verspreide gebeurtenis met belang rijke gevolgen.

We hebben de volgende informatie om te begrijpen hoe u uw IoT-oplossingen kunt beveiligen.

## <a name="security-architecture"></a>Beveiligingsarchitectuur
Bij het ontwerpen van een systeem is het belang rijk om inzicht te krijgen in de mogelijke dreigingen van dat systeem en de juiste verdedigingen dienovereenkomstig, aangezien het systeem is ontworpen en gearchitectd. Het is belang rijk dat u het product ontwerpt van het begin met beveiliging in het gedrang, omdat u kunt zien hoe een aanvaller een systeem kan misbruiken om ervoor te zorgen dat de juiste oplossingen van het begin worden uitgevoerd.

Lees [Internet of Things beveiligings architectuur](/azure/iot-fundamentals/iot-security-architecture)voor meer informatie over IOT-beveiligings architectuur.

In dit artikel komen de volgende onderwerpen aan bod:

* [Beveiliging begint met een bedreigings model](/azure/iot-fundamentals/iot-security-architecture#security-starts-with-a-threat-model)
* [Beveiliging in IoT](/azure/iot-fundamentals/iot-security-architecture#security-in-iot)
* [Bedreiging model leren van de Azure IoT-referentie architectuur](/azure/iot-fundamentals/iot-security-architecture)

## <a name="security-from-the-ground-up"></a>Fundamentele beveiliging
De IoT vormt unieke beveiligings-, privacy-en nalevings uitdagingen voor bedrijven wereld wijd. In tegens telling tot traditionele Cyber technologie waarbij deze problemen rond software en hoe deze worden geïmplementeerd, is IoT wat er gebeurt wanneer de Cyber en de fysieke werelden convergeren. Het beveiligen van IoT-oplossingen vereist dat beveiligde inrichting van apparaten, beveiligde connectiviteit tussen deze apparaten en de Cloud, en de beveiliging van gegevens in de Cloud wordt gewaarborgd tijdens de verwerking en opslag. Het is echter wel mogelijk om aan dergelijke functies te werken. Dit zijn apparaten met een beperkt aantal resources, geografische distributie van implementaties en veel apparaten binnen een oplossing.

Lees [Internet of Things beveiliging van het wegdek](/azure/iot-fundamentals/iot-security-ground-up)voor meer informatie over het afhandelen van beveiliging op deze gebieden.

In dit artikel komen de volgende onderwerpen aan bod:

* [Infra structuur vanaf het begin beveiligen](/azure/iot-fundamentals/iot-security-ground-up#secure-infrastructure-from-the-ground-up)
* [Microsoft Azure-veilige IoT-infra structuur voor uw bedrijf](/azure/iot-fundamentals/iot-security-ground-up#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>Aanbevolen procedures
Voor het beveiligen van een IoT-infra structuur is een strenge, diep gaande strategie vereist. Van het beveiligen van gegevens in de Cloud, het beschermen van gegevens integriteit tijdens de overdracht via het open bare Internet, om apparaten veilig in te richten, bouwt elke laag een hogere beveiligings garantie in de algehele infra structuur.

Lees [Internet of Things aanbevolen procedures](/azure/iot-fundamentals/iot-security-best-practices)voor beveiliging voor meer informatie over Internet of Things aanbevolen procedures voor beveiliging.

In dit artikel komen de volgende onderwerpen aan bod:

* [IoT-hardwarefabrikant/integrator](/azure/iot-fundamentals/iot-security-best-practices#iot-hardware-manufacturerintegrator)
* [Ontwikkel aars van IoT-oplossingen](/azure/iot-fundamentals/iot-security-best-practices#iot-solution-developer)
* [IoT-oplossings implementatie](/azure/iot-fundamentals/iot-security-best-practices#iot-solution-deployer)
* [IoT-oplossings operator](/azure/iot-fundamentals/iot-security-best-practices#iot-solution-operator)
