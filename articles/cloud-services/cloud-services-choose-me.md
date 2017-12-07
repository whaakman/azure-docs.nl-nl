---
title: Opties voor Azure compute - Cloudservices | Microsoft Docs
description: 'Meer informatie over Azure compute hosting-opties en hoe deze werken: App Service, Cloud Services en virtuele Machines'
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
ms.assetid: ed7ad348-6018-41bb-a27d-523accd90305
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 17ecf39128994dad93f017f87f105254f3017230
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="should-i-choose-cloud-services-or-something-else"></a>Moet ik kiezen cloudservices of iets anders?
Is Azure Cloud Services de keuze voor u? Azure biedt verschillende hosting modellen voor het uitvoeren van toepassingen. Elke biedt een andere set van services, zodat waarvoor u kiest een afhankelijk precies wat u probeert te doen.

[!INCLUDE [compute-table](../../includes/compute-options-table.md)]

<a name="tellmecs"></a>

## <a name="tell-me-about-cloud-services"></a>Meer informatie over cloudservices
Cloud-Services is een voorbeeld van [Platform as a Service](https://azure.microsoft.com/overview/what-is-paas/) (PaaS). Zoals [App Service](../app-service/app-service-web-overview.md), deze technologie is ontworpen ter ondersteuning van toepassingen die schaalbare, betrouwbare en goedkope werken. Net zoals een App Service wordt gehost op virtuele machines, te zijn Cloud Services, maar u hebt meer controle over de virtuele machines. U kunt uw eigen software installeren op virtuele machines voor Cloud Services en u kunt externe in deze.

![cs_diagram](./media/cloud-services-choose-me/diagram.png)

Meer controle betekent ook minder eenvoudig te gebruiken. Tenzij u de aanvullende controle-opties moet, is doorgaans sneller en eenvoudiger voor een webtoepassing en uitgevoerd in de Web-Apps in App Service vergeleken met Cloud-Services.

Er zijn twee soorten Cloud Service-rollen. Het enige verschil tussen de twee is hoe uw rol wordt gehost op de virtuele machine.

* **Webrol**  
Automatisch implementeert en als host fungeert voor uw app via IIS.

* **Werkrol**  
Maakt geen gebruik van IIS en wordt uw zelfstandige app wordt uitgevoerd.

Een eenvoudige toepassing kan bijvoorbeeld slechts een enkele Webrol, voor een website te gebruiken. Een complexere toepassing kan een Webrol gebruiken voor het verwerken van binnenkomende aanvragen van gebruikers en geeft deze aanvragen bij een werkrol voor verwerking. (Deze communicatie kan gebruiken [Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md) of [Azure wachtrijen](../storage/common/storage-introduction.md).)

Als de voorgaande afbeelding wordt voorgesteld, worden alle virtuele machines in één toepassing uitgevoerd in dezelfde cloudservice. Gebruikers toegang tot de toepassing via één openbaar IP-adres, met aanvragen automatisch laden verdeeld zijn over virtuele machines van de toepassing. Het platform [schaalt en implementeert](cloud-services-how-to-scale-portal.md) de virtuele machines in een Cloud Services-toepassing op een manier die een potentieel hardwarefout voorkomt.

Hoewel de toepassingen uitvoeren in virtuele machines, is het belangrijk te weten dat Cloudservices PaaS, niet IaaS biedt. Hier volgt één manier om na te denken: met IaaS, zoals Azure Virtual Machines u maakt eerst en configureert u de omgeving in uw toepassing wordt uitgevoerd, en implementeren van uw toepassing in deze omgeving. U bent zelf verantwoordelijk voor het beheren van veel van deze wereld doen, zoals het implementeren van nieuwe patches versies van het besturingssysteem op elke virtuele machine. In PaaS, daarentegen is als de omgeving al bestaat. Hoeft u het implementeren van uw toepassing is. Beheer van het platform dat op wordt uitgevoerd, zoals het implementeren van nieuwe versies van het besturingssysteem wordt uitgevoerd voor u.

## <a name="scaling-and-management"></a>Schaalbaarheid en beheer
U kunt virtuele machines niet maken met Cloud-Services. In plaats daarvan het bieden van een configuratiebestand dat Azure aangeeft hoeveel van elk gewenst zoals **drie rolinstanties web** en **twee exemplaren van worker-rol**, en het platform maakt deze voor u.  U nog steeds kiezen [welke grootte](cloud-services-sizes-specs.md) die back-ups maken van virtuele machines moeten worden, maar u niet expliciet ze zelf maken. Als uw toepassing een grotere belasting te verwerken moet, kunt u vragen voor meer virtuele machines en Azure maakt die exemplaren. Als de belasting afneemt, kunt u die exemplaren afgesloten en betaalt voor hen stoppen.

Een Cloud Services-servicetoepassing is doorgaans beschikbaar gesteld aan gebruikers via een proces in twee stappen. Een ontwikkelaar eerste [uploadt u de toepassing](cloud-services-how-to-create-deploy-portal.md) naar het faseringsgebied voor het platform. Wanneer de ontwikkelaar is gereed om te maken van de toepassing live, gebruiken ze de Azure-portal wisselen met productie voor fasering. Dit [schakelen tussen Faseren en productie](cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production) kan plaatsvinden zonder uitvaltijd, waarmee een actieve toepassing naar een nieuwe versie worden bijgewerkt zonder de gebruikers te verstoren.

## <a name="monitoring"></a>Bewaking
Cloud-Services biedt ook de bewaking. Als Azure virtuele Machines die het detecteert een mislukte fysieke server en de virtuele machines die werden uitgevoerd op die server op een nieuwe machine opnieuw is opgestart. Maar Cloudservices detecteert ook mislukte VM's en toepassingen, niet alleen hardwarefouten. In tegenstelling tot virtuele Machines, heeft een agent binnen elke web- en werkrollen rol en het is daarom kunnen nieuwe VM's en exemplaren van een toepassing starten als er fouten optreden.

De PaaS aard van Cloudservices heeft andere gevolgen te. Een van de belangrijkste is dat toepassingen is gebaseerd op deze technologie correct uit te voeren wanneer een willekeurig exemplaar van de functie web- of worker niet kan worden geschreven. Een Cloud Services-servicetoepassing mag niet om dit te bereiken, staat in het bestandssysteem van een eigen virtuele machines worden onderhouden. In tegenstelling tot VM's zijn gemaakt met Azure Virtual Machines, schrijfbewerkingen die zijn aangebracht in de Cloud Services-virtuele machines niet persistent; Er is niet op een gegevensschijf virtuele Machines. In plaats daarvan moet een Cloud Services-toepassing expliciet alle statussen naar schrijven SQL-Database, blobs, tabellen of andere externe opslag. Maken van toepassingen op deze manier zorgt ervoor dat ze gemakkelijker te schalen en beter bestand zijn tegen mislukt, beide belangrijke doelstellingen van Cloudservices.

## <a name="next-steps"></a>Volgende stappen
[Een cloud service-app maken in .NET](cloud-services-dotnet-get-started.md)  
[Een cloud service-app maken in Node.js](cloud-services-nodejs-develop-deploy-app.md)  
[Een cloud service-app maken in PHP](../cloud-services-php-create-web-role.md)  
[Een cloud service-app maken in Python](cloud-services-python-ptvs.md)

