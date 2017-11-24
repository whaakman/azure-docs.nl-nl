---
title: Beveiligen van uw Internet der dingen (IoT) in Azure | Microsoft Docs
description: " Azure internet der dingen (IoT)-services bieden een breed scala aan mogelijkheden. In dit artikel helpt u begrijpen hoe voor het beveiligen van uw IoT-oplossingen in Azure. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: 1473c8dd-8669-48fb-86db-b3c50e2eaf59
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: baf9c81867f8dccf3897213121888d40b7472d03
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="internet-of-things-security-overview"></a>Overzicht van Internet of Things-beveiliging
Azure internet der dingen (IoT)-services bieden een breed scala aan mogelijkheden. Met deze hoogwaardige services kunt u het volgende doen:

* Gegevens van apparaten verzamelen
* Gegevensstromen in beweging analyseren
* Grote gegevenssets opslaan en er query’s op uitvoeren
* Gegevens in realtime en historische gegevens visualiseren
* Integraties met back-officesystemen uitvoeren

Voor deze mogelijkheden, Azure IoT Suite-pakketten samen meerdere Azure-services met aangepaste extensies als vooraf geconfigureerde oplossingen. Deze vooraf geconfigureerde oplossingen zijn basisimplementaties van algemene patronen van IoT-oplossingen die u helpen de tijd voor het leveren van uw IoT-oplossingen te verkorten. Met behulp van de IoT-SDK's, kunt u aanpassen en uitbreiden van deze oplossingen om te voldoen aan uw eigen vereisten. U kunt deze oplossingen gebruiken als voorbeelden of sjablonen als u nieuwe IoT-oplossingen ontwikkelt.

De Azure IoT suite is een krachtige oplossing voor uw IoT-behoeften. Het is echter van upmost belang dat uw IoT-oplossingen zijn ontworpen met beveiligd zijn vanaf het begin. Alle beveiligingsincidenten vanwege het grote aantal IoT-apparaten, kan snel een wijdverbreid gebeurtenis met grote gevolgen worden.

We hebben de volgende informatie om te begrijpen hoe u voor het beveiligen van uw IoT-oplossingen.

## <a name="security-architecture"></a>Beveiligingsarchitectuur
Bij het ontwerpen van een systeem, is het belangrijk om te begrijpen van de mogelijke bedreigingen tot dat systeem en dienovereenkomstig juiste beveiliging niet toevoegen als het systeem is ontworpen en ontworpen. Het is belangrijk voor het ontwerpen van het product vanaf het begin rekening met beveiliging omdat informatie over hoe een aanvaller mogelijk een systeem kunt u ervoor dat de juiste oplossingen in plaats vanaf het begin.

U kunt meer informatie over IoT-beveiligingsarchitectuur door te lezen [Internet van dingen beveiligingsarchitectuur](../iot-suite/iot-security-architecture.md).

Dit artikel wordt beschreven in de volgende onderwerpen:

* [Beveiliging begint met een risicomodel](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
* [Beveiliging in IoT](../iot-suite/iot-security-architecture.md#security-in-iot)
* [Threat Modeling de naslaginformatie over Azure IoT-architectuur](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-the-ground-up"></a>Fundamentele beveiliging
De IoT vormt unieke beveiliging, privacy en naleving uitdagingen voor bedrijven overal ter wereld. In tegenstelling tot traditionele cyberbeveiliging technologie waar deze problemen gebaseerd op software- en hoe deze wordt geïmplementeerd, IoT heeft betrekking op wat er gebeurt wanneer de cyberbeveiliging en de fysieke werelden convergeren. Beveiligen van IoT-oplossingen vereist gezorgd beveiligde inrichting van de apparaten, beveiligde verbindingen tussen deze apparaten en de cloud en de bescherming van de beveiligde gegevens in de cloud tijdens verwerking en opslag. Werken met dergelijke functionaliteit, zijn echter resource beperkte apparaten, geografische verdeling van implementaties en veel apparaten binnen een oplossing.

U leert hoe beveiliging in de volgende gebieden verwerkt door te lezen [beveiliging van Internet der dingen van een compleet nieuwe](../iot-suite/securing-iot-ground-up.md).

Het artikel worden de volgende onderwerpen:

* [Beveiligde infrastructuur niet vanuit een compleet nieuwe](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
* [Microsoft Azure: beveiligde IoT-infrastructuur voor uw bedrijf](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>Beste praktijken
Een IoT-infrastructuur beveiligen vereist een strengere beveiliging-in-depth-strategie. Elke laag is gebaseerd, is groter beveiligingscontrole in de algehele infrastructuur van de beveiliging van gegevens in de cloud, de integriteit van de gegevens onderweg te beschermen via het openbare internet, voor het inrichten van apparaten, veilig.

U kunt meer informatie over beveiliging van Internet der dingen aanbevolen procedures door te lezen [aanbevolen beveiligingsprocedures voor Internet der dingen](../iot-suite/iot-security-best-practices.md).

Het artikel worden de volgende onderwerpen:

* [IoT hardware fabrikant/integrator](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
* [Ontwikkelaars van IoT-oplossing](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
* [Implementatie van IoT-oplossing](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
* [IoT-oplossing operator](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
