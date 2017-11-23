---
title: Internet der dingen aanbevolen beveiligingsprocedures | Microsoft Docs
description: Het artikel biedt een samengestelde lijst met Microsoft Internet van dingen Best Practices voor beveiliging en algemene aanbevelingen.
services: security
documentationcenter: na
author: TomShinder
manager: StevenPo
editor: TomSh
ms.assetid: 2d5598c5-4c30-481d-b8f4-51ee024ea9a7
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: yurid
ms.openlocfilehash: 8937437652a78e05b94574cb9fe5df7962edb1ad
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="internet-of-things-security-best-practices"></a>Internet der dingen Best Practices voor beveiliging
Beveiligen van de infrastructuur van het Internet der dingen (IoT) is een kritieke onderneming voor iedereen die betrokken zijn bij IoT-oplossingen. Vanwege het aantal apparaten bij betrokken zijn en de gedistribueerde aard van deze apparaten, het effect dat een beveiligingsgebeurtenis gerelateerde tot miljoenen IoT-apparaten is niet-triviale en wijdverbreid invloed kan hebben.

IoT-beveiliging moet daarom een security-in-depth-benadering. Gegevens moeten worden beveiligd in de cloud en wordt particuliere en publieke netwerken. Methoden moeten aanwezig zijn voor het inrichten van de IoT-apparaten zelf veilig. Elke laag van apparaat, met netwerk naar de cloud back-end moet sterke beveiliging garantie.

Aanbevolen procedures van IoT kunnen worden ingedeeld in de volgende manier:

* IoT-hardwarefabrikant of integrator
* Ontwikkelaars van IoT-oplossing
* Implementatie van IoT-oplossing
* IoT-oplossing operator

In dit artikel bevat een overzicht van [Internet van dingen aanbevolen beveiligingsprocedures](../iot-suite/iot-security-best-practices.md). Raadpleeg dit artikel voor meer gedetailleerde informatie.

## <a name="iot-hardware-manufacturer-or-integrator"></a>IoT-hardwarefabrikant of integrator
Volg de onderstaande procedures als u een hardwarefabrikant IoT of een integrator hardware:

* **Bereik van hardware aan de minimumvereisten**: het hardwareontwerp van de minimale functies die vereist zijn voor de werking van de hardware, en niets meer moet bevatten. 
* **Controleer hardware bewijs knoeien**: bouwen in mechanismen voor het detecteren van fysieke knoeien met de hardware, zoals het openen van het apparaat behandeld, verwijderen van een deel van het apparaat gebruikt, enzovoort. 
* **Bouwen om beveiligde hardware**: als [omzet](https://en.wikipedia.org/wiki/Cost_of_goods_sold) toestaan, beveiligingsfuncties zoals beveiligd en versleuteld opslag en functionaliteit van de Trusted Platform Module TPM-opstart opbouwen.
* **Beveiligen van upgrades**: firmware bijwerken tijdens de levensduur van het apparaat is onvermijdelijk.

## <a name="iot-solution-developer"></a>Ontwikkelaars van IoT-oplossing
Volg de onderstaande procedures als u een ontwikkelaar IoT-oplossing:

* **Ga als volgt beveiligde software development methodologie**: a t/m nadenken over de beveiliging van het begin van het project helemaal tot aan de implementatie, testen en implementatie ontwikkelen van beveiligde software vereist.
* **Kies open-sourcesoftware zorgvuldig**: open-sourcesoftware biedt de mogelijkheid snel om oplossingen te ontwikkelen.
* **Integreren met zorg**: veel van de software-beveiligingsfouten bestaat op de grens van bibliotheken en -API. 

## <a name="iot-solution-deployer"></a>Implementatie van IoT-oplossing
Volg de onderstaande procedures als u een deployer IoT-oplossing:

* **Hardware veilig implementeren**: IoT-implementaties vereisen hardware in niet-beveiligde locaties, zoals in openbare ruimten of zonder supervisie landinstellingen worden geïmplementeerd.
* **Verificatiesleutels veilig te houden**: tijdens de implementatie van elk apparaat vereist dat apparaat-id's en bijbehorende verificatiesleutels die worden gegenereerd door de cloudservice. Beveilig deze sleutels fysiek zelfs na de implementatie. Een willekeurige toets waarmee is geknoeid kan worden gebruikt door een kwaadwillende apparaat naar zich voordoen als een bestaand apparaat.

## <a name="iot-solution-operator"></a>IoT-oplossing operator
Volg de onderstaande procedures als u een IoT-oplossing operator:

* **Systemen up-to-date te houden**: Zorg ervoor dat de besturingssystemen van apparaten en alle apparaatstuurprogramma's worden bijgewerkt naar de meest recente versies. 
* **Bescherming tegen schadelijke activiteiten**: als het besturingssysteem toestaat, plaatst u de nieuwste antivirus- en anti-malware-mogelijkheden op elk besturingssysteem. 
* **Regelmatig controleren**: controle IoT-infrastructuur voor beveiliging problemen is sleutel verwante bij het reageren op beveiligingsincidenten.
* **Fysiek de IoT-infrastructuur beveiligen**: de slechtste beveiligingsaanvallen tegen IoT-infrastructuur worden gestart met behulp van de fysieke toegang tot apparaten.
* **Beveiligen van referenties van de cloud**: referenties voor cloud-verificatie gebruikt voor het configureren en gebruiken van een IoT-implementatie zijn mogelijk de eenvoudigste manier om toegang te krijgen en een IoT-systeem. 

