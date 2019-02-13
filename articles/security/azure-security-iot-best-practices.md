---
title: Internet of Things Security Best Practices | Microsoft Docs
description: Het artikel biedt een gecureerde lijst met Microsoft Internet of Things Security Best Practices en algemene aanbevelingen.
services: security
documentationcenter: na
author: barclayn
manager: barbkess
editor: TomSh
ms.assetid: 2d5598c5-4c30-481d-b8f4-51ee024ea9a7
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2018
ms.author: barclayn
ms.openlocfilehash: 9413c0503c1b78550776d1c2f6ab8239205a788b
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56117864"
---
# <a name="internet-of-things-security-best-practices"></a>Internet of Things Security Best Practices

Beveiligen van het Internet of Things (IoT)-infrastructuur is een kritieke onderneming voor iedereen die betrokken zijn bij IoT-oplossingen. Vanwege het aantal apparaten bij betrokken zijn en de gedistribueerde aard van deze apparaten, de impact die een veiligheidsrisico met betrekking tot het binnendringen van miljoenen IoT-apparaten is essentieel en wijdverbreid invloed kan hebben.

IoT-beveiliging moet daarom een benadering van beveiliging in de diepte. Gegevens moeten worden beveiligd in de cloud en deze worden verplaatst via persoonlijke en openbare netwerken. Methoden moeten worden voldaan voor het veilig inrichten van de IoT-apparaten zelf. Elke laag van het apparaat, netwerk, naar de cloud back-end moet sterke goed beveiligingsprogramma.

Best practices voor IoT kunnen worden onderverdeeld in de volgende manier:

* De fabrikant van de IoT-hardware of integrator
* Ontwikkelaarshandleiding voor IoT-oplossing
* Implementatie van IoT-oplossing
* Operator voor IoT-oplossing

In dit artikel bevat een overzicht van [Internet van dingen Best Practices voor beveiliging](../iot-suite/iot-security-best-practices.md). Raadpleeg dit artikel voor meer gedetailleerde informatie.

## <a name="iot-hardware-manufacturer-or-integrator"></a>De fabrikant van de IoT-hardware of integrator

Volg de onderstaande aanbevolen procedures als u een IoT-hardwarefabrikant of een integrator hardware:

* **Het bereik van hardware aan de minimale vereisten voor**: het hardwareontwerp van de minimale functies die vereist zijn voor de werking van de hardware, en niets meer moet bevatten. 
* **Hardwarewijziging knoeien bewijs**: bouw mechanismen voor het detecteren van fysieke manipulatie van hardware, zoals het openen van de apparaat-dekking, verwijderen van een onderdeel van het apparaat, enzovoort. 
* **Bouwen om beveiligde hardware**: als [COGS](https://en.wikipedia.org/wiki/Cost_of_goods_sold) toestaan, het bouwen van beveiligingsfuncties, zoals beveiligd en versleuteld opslag en functionaliteit op basis van een Trusted Platform Module TPM opstarten.
* **Beveiligen van upgrades**: firmware bijwerken tijdens de levensduur van het apparaat is onvermijdelijk.

## <a name="iot-solution-developer"></a>Ontwikkelaarshandleiding voor IoT-oplossing

Volg de onderstaande aanbevolen procedures als u een ontwikkelaar van de IoT-oplossing:

* **Ga als volgt beveiligde software ontwikkelingsmethodologie**: volledig nadenken over de beveiliging van het begin van het project helemaal tot aan de implementatie, testen en implementeren voor het ontwikkelen van veilige software vereist.
* **Kies open-sourcesoftware zorgvuldig**: open source-software biedt de mogelijkheid voor het snel ontwikkelen van oplossingen.
* **Integreren met zorg**: veel van de software-beveiligingsfouten bestaat op de grens van de API's en bibliotheken. 

## <a name="iot-solution-deployer"></a>Implementatie van IoT-oplossing

Volg de onderstaande aanbevolen procedures als u de implementatie van een IoT-oplossing:

* **Hardware veilig implementeren**: IoT-implementaties mogelijk hardware om te worden geïmplementeerd in niet-beveiligde locaties, zoals in openbare ruimten of zonder supervisie landinstellingen.
* **Verificatiesleutels veilig te houden**: tijdens de implementatie van elk apparaat vereist dat apparaat-id's en verificatiesleutels die worden gegenereerd door de service in de cloud die is gekoppeld. Deze sleutels fysiek veilig afschermen na de implementatie. Een willekeurige toets waarmee is geknoeid kan door een schadelijke apparaat worden gebruikt voor het zich voordoen als een bestaand apparaat.

## <a name="iot-solution-operator"></a>Operator voor IoT-oplossing

Volg de onderstaande aanbevolen procedures als u een IoT-oplossing operator:

* **Systemen up-to-date te houden**: Zorg ervoor dat de besturingssystemen van apparaten en alle apparaatstuurprogramma's worden bijgewerkt naar de nieuwste versie. 
* **Bescherming tegen schadelijke activiteiten**: als het besturingssysteem toestaat, plaatst u de nieuwste mogelijkheden van antivirusprogramma's en anti-malware op elk besturingssysteem. 
* **Regelmatig controleren**: controle van IoT-infrastructuur voor beveiliging problemen is essentieel gerelateerde bij het reageren op beveiligingsincidenten.
* **De IoT-infrastructuur fysiek beveiligen**: de slechtste beveiligingsaanvallen tegen IoT-infrastructuur worden gestart met behulp van de fysieke toegang tot apparaten.
* **Cloudreferenties beveiligen**: referenties voor cloud-verificatie gebruikt voor het configureren en gebruiken van een IoT-implementatie zijn mogelijk de eenvoudigste manier om toegang te krijgen en een IoT-systeem. 

