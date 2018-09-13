---
title: Beveiligen van uw Internet of Things (IoT) in Azure | Microsoft Docs
description: " Azure internet der dingen (IoT)-services bieden een breed scala aan mogelijkheden. In dit artikel vindt u informatie over het beveiligen van uw IoT-oplossingen in Azure. "
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
ms.openlocfilehash: 41e422d0808cafb45b182c5f0a6bb7176a35516f
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35758617"
---
# <a name="internet-of-things-security-overview"></a>Overzicht van de beveiliging van Internet of Things
Azure internet der dingen (IoT)-services bieden een breed scala aan mogelijkheden. Met deze hoogwaardige services kunt u het volgende doen:

* Gegevens van apparaten verzamelen
* Gegevensstromen in beweging analyseren
* Grote gegevenssets opslaan en er query’s op uitvoeren
* Gegevens in realtime en historische gegevens visualiseren
* Integraties met back-officesystemen uitvoeren

Voor deze mogelijkheden, Azure IoT-oplossing accelerators pakket samen meerdere Azure-services met aangepaste extensies als vooraf geconfigureerde oplossingen. Deze vooraf geconfigureerde oplossingen zijn basisimplementaties van algemene patronen van IoT-oplossingen die u helpen de tijd voor het leveren van uw IoT-oplossingen te verkorten. Met behulp van de IoT-software development kits, kunt u aanpassen en uitbreiden van deze oplossingen om te voldoen aan uw eigen vereisten. U kunt deze oplossingen gebruiken als voorbeelden of sjablonen als u nieuwe IoT-oplossingen ontwikkelt.

De Azure IoT-oplossingsversnellers zijn krachtige oplossingen voor uw IoT-behoeften. Het is echter van belang zijn upmost dat uw IoT-oplossingen zijn ontworpen met beveiliging waarmee u rekening moet vanaf het begin. Vanwege het grote aantal IoT-apparaten, alle beveiligingsincidenten kunt korte tijd uitgegroeid tot een wijdverbreide gebeurtenis met aanzienlijke gevolgen.

We hebben de volgende informatie als u wilt, kunt u meer informatie over het beveiligen van uw IoT-oplossingen.

## <a name="security-architecture"></a>Beveiligingsarchitectuur
Bij het ontwerpen van een systeem, is het belangrijk dat u de potentiële bedreigingen voor dat systeem te begrijpen en dienovereenkomstig, juiste beveiliging toevoegen wanneer het systeem worden ontworpen en ontworpen. Het is belangrijk om te ontwerpen van het product vanaf het begin rekening met beveiliging omdat informatie over hoe een aanvaller kan mogelijk een systeem kunt u ervoor dat de juiste oplossingen worden in plaats van het begin.

Vindt u informatie over IoT-beveiligingsarchitectuur door te lezen [Internet van dingen beveiligingsarchitectuur](/azure/iot-fundamentals/iot-security-architecture).

Dit artikel wordt beschreven in de volgende onderwerpen:

* [Beveiliging begint met een risicomodel](/azure/iot-fundamentals/iot-security-architecture#security-starts-with-a-threat-model)
* [Beveiliging in IoT](/azure/iot-fundamentals/iot-security-architecture#security-in-iot)
* [Threat Modeling van de Azure IoT Reference Architecture](/azure/iot-fundamentals/iot-security-architecture#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-the-ground-up"></a>Fundamentele beveiliging
De IoT vormt unieke uitdagingen voor de beveiliging, privacy en naleving voor bedrijven over de hele wereld. In tegenstelling tot traditionele cyber-technologie waar deze problemen draaien om software en hoe deze is geïmplementeerd, IoT heeft betrekking op wat er gebeurt wanneer de cyberaanvallen en de fysieke werelden geconvergeerd. IoT-oplossingen beveiligen, moet ervoor te zorgen dat beveiligde inrichting van apparaten, veilige connectiviteit tussen deze apparaten en de cloud en de bescherming van de beveiligde gegevens in de cloud tijdens verwerking en opslag. Deze functionaliteit werkt, zijn echter apparaten resource beperkt, geografische verdeling van implementaties en veel apparaten binnen een oplossing.

U kunt leren hoe u beveiliging op deze gebieden worden verwerkt door te lezen [beveiliging van Internet of Things vanaf de grond](/azure/iot-fundamentals/iot-security-ground-up).

Het artikel wordt beschreven in de volgende onderwerpen:

* [Veilige infrastructuur implementeren vanaf het begin te beginnen](/azure/iot-fundamentals/iot-security-ground-up#secure-infrastructure-from-the-ground-up)
* [Microsoft Azure: beveiligde IoT-infrastructuur voor uw bedrijf](/azure/iot-fundamentals/iot-security-ground-up#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>Beste praktijken
Een IoT-infrastructuur beveiligen, is een strenge beveiliging in de diepte strategie vereist. Van de beveiliging van gegevens in de cloud, beveiligt de integriteit van gegevens onderweg zijn via het openbare internet, voor het veilig inrichten van apparaten, maakt elke laag groter beveiligingscontrole in de algehele infrastructuur.

Vindt u informatie over Internet of Things security best practices voor door te lezen [best practices voor beveiliging van Internet of Things](/azure/iot-fundamentals/iot-security-best-practices).

Het artikel wordt beschreven in de volgende onderwerpen:

* [IoT hardware fabrikant/integrator](/azure/iot-fundamentals/iot-security-best-practices#iot-hardware-manufacturerintegrator)
* [Ontwikkelaarshandleiding voor IoT-oplossing](/azure/iot-fundamentals/iot-security-best-practices#iot-solution-developer)
* [Implementatie van IoT-oplossing](/azure/iot-fundamentals/iot-security-best-practices#iot-solution-deployer)
* [Operator voor IoT-oplossing](/azure/iot-fundamentals/iot-security-best-practices#iot-solution-operator)
