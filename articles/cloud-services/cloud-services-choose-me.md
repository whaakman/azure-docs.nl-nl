---
title: Opties voor Azure compute - Azure-Cloudservices | Microsoft Docs
description: 'Meer informatie over Azure compute hosting-opties en hoe deze werken: App Service, Azure Cloud Services en virtuele Machines'
services: cloud-services
documentationcenter: ''
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
ms.openlocfilehash: 2871a8c02db0ffc6d9033724e7c9f4a454afef8e
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
ms.locfileid: "29120281"
---
# <a name="should-i-choose-azure-cloud-services-or-something-else"></a>Moet ik kiezen Azure Cloud Services of iets anders?
Is Azure Cloud Services de keuze voor u? Azure biedt verschillende hosting modellen voor het uitvoeren van toepassingen. Elke voorziet in een andere set van services. Welke regel die u kiest, is afhankelijk van de exact wat u probeert te doen.

[!INCLUDE [compute-table](../../includes/compute-options-table.md)]

<a name="tellmecs"></a>

## <a name="tell-me-about-azure-cloud-services"></a>Meer informatie over Azure Cloud Services
Azure Cloud Services is een voorbeeld van een [platform als een service](https://azure.microsoft.com/overview/what-is-paas/) (PaaS). Zoals [Azure App Service](../app-service/app-service-web-overview.md), deze technologie is ontworpen ter ondersteuning van toepassingen die schaalbare, betrouwbare en goedkope werken. Op dezelfde manier dat App Service wordt gehost op virtuele machines (VM's), dus is Azure Cloud Services. U hebt echter meer controle over de virtuele machines. U kunt uw eigen software installeren op virtuele machines die gebruikmaken van Azure Cloud Services en u er toegang toe op afstand.

![Azure Cloud Services-diagram](./media/cloud-services-choose-me/diagram.png)

Meer controle betekent ook minder eenvoudig te gebruiken. Tenzij u de aanvullende controle-opties moet, is doorgaans sneller en eenvoudiger voor een webtoepassing en functie van App Service vergeleken uitgevoerd in de Web-Apps met Azure Cloud Services.

Er zijn twee soorten Azure Cloud Services-functies. Het enige verschil tussen de twee is hoe uw rol wordt gehost op de virtuele machines:

* **Webrol**: automatisch wordt geïmplementeerd en fungeert als host voor uw app via IIS.

* **Werkrol**: IIS niet gebruikt en uw zelfstandige app wordt uitgevoerd.

Een eenvoudige toepassing kan bijvoorbeeld slechts een enkele Webrol, voor een website te gebruiken. Een complexere toepassing mogelijk een Webrol gebruiken voor het verwerken van binnenkomende aanvragen van gebruikers en geeft deze aanvragen bij een werkrol voor verwerking. (Deze communicatie mogelijk gebruik [Azure Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md) of [Azure Queue storage](../storage/common/storage-introduction.md).)

Als de voorgaande afbeelding wordt voorgesteld, worden alle virtuele machines in één toepassing uitgevoerd in dezelfde cloudservice. Gebruikers toegang tot de toepassing via één openbaar IP-adres, met aanvragen automatisch laden verdeeld zijn over virtuele machines van de toepassing. Het platform [schaalt en implementeert](cloud-services-how-to-scale-portal.md) de virtuele machines in een Azure Cloud Services-toepassing op een manier die een potentieel hardwarefout voorkomt.

Hoewel de toepassingen uitvoeren in virtuele machines, is het belangrijk te weten dat Azure Cloud Services PaaS, niet de infrastructuur als een service (IaaS biedt). Hier volgt één manier om na te denken. Met IaaS, zoals Azure Virtual Machines u eerst maakt en configureert u de omgeving die in uw toepassing wordt uitgevoerd. Vervolgens implementeert u uw toepassing in deze omgeving. U bent verantwoordelijk voor het beheren van veel van deze wereld doen, zoals het implementeren van nieuwe patches versies van het besturingssysteem op elke virtuele machine. In PaaS, daarentegen is als de omgeving al bestaat. Hoeft u het implementeren van uw toepassing is. Beheer van het platform dat op wordt uitgevoerd, zoals het implementeren van nieuwe versies van het besturingssysteem wordt uitgevoerd voor u.

## <a name="scaling-and-management"></a>Schaalbaarheid en beheer
U kunt virtuele machines niet maken met Azure Cloud Services. In plaats daarvan het bieden van een configuratiebestand dat Azure hoeveel van elke gewenste aangeeft, zoals 'rolinstanties drie web' en 'twee exemplaren van worker-rol' Het platform vervolgens deze voor u gemaakt. U nog steeds kiezen [welke grootte](cloud-services-sizes-specs.md) die back-ups maken van virtuele machines moeten worden, maar u niet expliciet ze zelf maken. Als uw toepassing een grotere belasting te verwerken moet, kunt u vragen voor meer virtuele machines en Azure maakt die exemplaren. Als de belasting afneemt, kunt u die exemplaren afgesloten en betaalt voor hen stoppen.

Een Azure Cloud Services-toepassing is doorgaans beschikbaar gesteld aan gebruikers via een proces in twee stappen. Een ontwikkelaar eerste [uploadt u de toepassing](cloud-services-how-to-create-deploy-portal.md) naar het faseringsgebied voor het platform. Wanneer de ontwikkelaar is gereed om te maken van de toepassing live, gebruiken ze de Azure-portal wisselen met productie voor fasering. Dit [schakelen tussen Faseren en productie](cloud-services-how-to-manage-portal.md#swap-deployments-to-promote-a-staged-deployment-to-production) kan plaatsvinden zonder uitvaltijd, waarmee een actieve toepassing naar een nieuwe versie worden bijgewerkt zonder de gebruikers te verstoren.

## <a name="monitoring"></a>Bewaking
Azure Cloud Services biedt ook de bewaking. Net als virtuele Machines, het detecteert een mislukte fysieke server en de virtuele machines die werden uitgevoerd op die server op een nieuwe machine opnieuw is opgestart. Maar Azure Cloud Services detecteert ook mislukte VM's en toepassingen, niet alleen hardwarefouten. In tegenstelling tot virtuele Machines, heeft een agent binnen elke web- en werkrollen rol en het is daarom kunnen nieuwe VM's en exemplaren van een toepassing starten als er fouten optreden.

De PaaS aard van Azure Cloud Services heeft een andere gevolgen te. Een van de belangrijkste is dat toepassingen is gebaseerd op deze technologie correct uit te voeren wanneer een willekeurig exemplaar van de functie web- of worker niet kan worden geschreven. Om dit te bereiken, mag niet een Azure Cloud Services-toepassing staat in het bestandssysteem van een eigen virtuele machines bijhouden. In tegenstelling tot virtuele machines met virtuele Machines worden gemaakt, zijn aangebracht in Azure Cloud Services VMs schrijfbewerkingen niet permanente. Er is niet op een gegevensschijf virtuele Machines. Een Azure Cloud Services-toepassing moet alle statussen in plaats daarvan expliciet schrijven naar Azure SQL Database, blobs, tabellen of andere externe opslag. Maken van toepassingen op deze manier zorgt ervoor dat ze gemakkelijker te schalen en beter bestand zijn tegen fout, die beide belangrijke doelstellingen van Azure Cloud Services.

## <a name="next-steps"></a>Volgende stappen
* [Een cloud service-app maken in .NET](cloud-services-dotnet-get-started.md) 
* [Een cloud service-app maken in Node.js](cloud-services-nodejs-develop-deploy-app.md) 
* [Een cloud service-app maken in PHP](../cloud-services-php-create-web-role.md) 
* [Een cloud service-app maken in Python](cloud-services-python-ptvs.md)



