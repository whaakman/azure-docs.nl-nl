---
title: Aanbevolen beveiligings procedures voor Internet of Things | Microsoft Docs
description: In dit artikel vindt u een overzicht van de aanbevolen procedures voor micro soft Internet of Things Security en algemene aanbevelingen.
services: security
documentationcenter: na
author: barclayn
manager: barbkess
editor: TomSh
ms.assetid: 2d5598c5-4c30-481d-b8f4-51ee024ea9a7
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2018
ms.author: barclayn
ms.openlocfilehash: 14f1905b1acd60acdf7b3dad0686e3fa34629953
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727059"
---
# <a name="internet-of-things-security-best-practices"></a>Aanbevolen procedures voor Internet of Things beveiliging

Het beveiligen van de infra structuur van Internet of Things (IoT) is een belang rijke onderneming voor iedereen die betrokken is bij IoT-oplossingen. Vanwege het aantal betrokken apparaten en de gedistribueerde aard van deze apparaten is de impact van een beveiligings gebeurtenis met betrekking tot inbreuk op miljoenen IoT-apparaten niet-trivial en kan dit tot wijde invloed hebben.

Daarom heeft IoT Security een diep gaande benadering van beveiliging nodig. Gegevens moeten worden beveiligd in de Cloud en worden verplaatst via particuliere en open bare netwerken. Er moeten methoden worden ingevoerd om de IoT-apparaten zelf veilig in te richten. Voor elke laag, van apparaat, naar netwerk, is een sterke beveiligings garantie vereist voor de back-end van de Cloud.

Best practices voor IoT kunnen op de volgende manier worden gecategoriseerd:

* IoT-hardwarefabrikant of integrator
* Ontwikkel aars van IoT-oplossingen
* IoT-oplossings implementatie
* IoT-oplossings operator

Dit artikel bevat een overzicht van [Internet of Things aanbevolen beveiligings procedures](/azure/iot-fundamentals/iot-security-best-practices). Raadpleeg dit artikel voor meer gedetailleerde informatie.

## <a name="iot-hardware-manufacturer-or-integrator"></a>IoT-hardwarefabrikant of integrator

Volg de onderstaande aanbevolen procedures als u een IoT-hardware-productie of een hardware integrator:

* **Hardware beperken tot minimale vereisten**: het hardware-ontwerp moet mini maal vereiste functies bevatten voor de werking van de hardware en niets meer. 
* Een hardware-wijzigings **bewijs maken**: ontwikkel mechanismen voor het detecteren van fysieke manipulatie van hardware, zoals het openen van de apparaat-dekking, het verwijderen van een deel van het apparaat, enzovoort. 
* **Bouwen rond beveiligde hardware**: als [KPV](https://en.wikipedia.org/wiki/Cost_of_goods_sold) is toegestaan, bouwt u beveiligings functies zoals beveiligde en versleutelde opslag en op Trusted Platform Module (TPM) gebaseerde opstart functionaliteit.
* **Beveiligings updates beveiligen**: tijdens het levens duur van het apparaat wordt er onvermijdelijk een upgrade uitgevoerd voor de firmware.

## <a name="iot-solution-developer"></a>Ontwikkel aars van IoT-oplossingen

Volg de onderstaande aanbevolen procedures als u een IoT-oplossing voor ontwikkel aars bent:

* **Volg de methode voor de ontwikkeling van beveiligde software**: het ontwikkelen van beveiligde software vereist dat u op de hoogte bent van de beveiliging van het begin van het project, al naar gelang de implementatie, testen en implementatie.
* **Kies open source software with Care**: open source software biedt u de mogelijkheid om snel oplossingen te ontwikkelen.
* **Integreer met zorg**: veel van de software beveiligings fouten bestaan op de grens van bibliotheken en api's. 

## <a name="iot-solution-deployer"></a>IoT-oplossings implementatie

Volg de onderstaande aanbevolen procedures als u een IoT-oplossing wilt implementeren:

* **Hardware veilig implementeren**: IoT-implementaties vereisen mogelijk dat de hardware moet worden geïmplementeerd op onbeveiligde locaties, zoals in open bare ruimten of in niet-gecontroleerde land instellingen.
* **Verificatie sleutels veilig houden**: tijdens de implementatie moeten voor elk apparaat apparaat-id's en bijbehorende verificatie sleutels worden gegenereerd door de Cloud service. Bewaar deze sleutels fysiek veilig, zelfs na de implementatie. Alle versleutelde sleutels kunnen worden gebruikt door een schadelijk apparaat om als een bestaand apparaat te maskeren.

## <a name="iot-solution-operator"></a>IoT-oplossings operator

Volg de onderstaande aanbevolen procedures als u een IoT-oplossings operator bent:

* **Systeem up-to-date houden**: Controleer of besturings systemen van apparaten en alle apparaatstuurprogramma's zijn bijgewerkt naar de nieuwste versie. 
* **Beveiligen tegen schadelijke activiteiten**: als het besturings systeem dit toestaat, plaatst u de nieuwste anti-virus-en anti-malware-mogelijkheden op elk besturings systeem van het apparaat. 
* **Regel matig controleren**: de IOT-infra structuur voor beveiligings problemen controleren is de sleutel bij het beantwoorden van beveiligings incidenten.
* **De IOT-infra structuur fysiek beschermen**: de ergste beveiligings aanvallen op IOT-infra structuur worden gestart met fysieke toegang tot apparaten.
* **Cloud referenties beveiligen**: Cloud authenticatie referenties die worden gebruikt voor het configureren en uitvoeren van een IOT-implementatie, zijn mogelijk de eenvoudigste manier om toegang te krijgen tot een IOT-systeem. 

