---
title: Wat is Azure Cloud Services | Microsoft Docs
description: Meer informatie over wat Azure Cloud Services is.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
ms.assetid: ed7ad348-6018-41bb-a27d-523accd90305
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeconnoc
ms.openlocfilehash: ce88dcaedf32f293fc121cda2a088388c99badee
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53603824"
---
# <a name="overview-of-azure-cloud-services"></a>Overzicht van Azure Cloudservices
Azure Cloud Services is een voorbeeld van een [platform als een service](https://azure.microsoft.com/overview/what-is-paas/) (PaaS). Zoals [Azure App Service](../app-service/overview.md), deze technologie is ontworpen ter ondersteuning van toepassingen die schaalbare, betrouwbare en voordelig uit te voeren. Op dezelfde manier dat App Service wordt gehost op virtuele machines (VM's), dus is Azure Cloud Services. U hebt echter meer controle over de virtuele machines. U kunt uw eigen software installeren op virtuele machines die gebruikmaken van Azure Cloud Services, en kunt u ze op afstand openen.

![Azure Cloud Services-diagram](./media/cloud-services-choose-me/diagram.png)

Meer controle houdt ook minder gebruiksgemak. Tenzij u de extra controle-opties nodig hebt, is doorgaans sneller en gemakkelijker aan de slag van een web-App en die worden uitgevoerd in de Web-Apps-functie van App Service in vergelijking met Azure Cloud Services.

Er zijn twee soorten Azure Cloud Services-rollen. Het enige verschil tussen de twee is hoe uw rol wordt gehost op de virtuele machines:

* **Webrol**: Automatisch wordt geïmplementeerd en als host fungeert voor uw app met IIS.

* **Werkrol**: Maakt geen gebruik van IIS en wordt uw zelfstandige app wordt uitgevoerd.

Een eenvoudige toepassing kan bijvoorbeeld alleen een één-Webrol, die zijn voor een website gebruiken. Een complexere toepassing mogelijk een Webrol gebruiken voor het verwerken van binnenkomende aanvragen van gebruikers en geeft deze aanvragen bij een werkrol voor verwerking. (Deze communicatie mogelijk gebruik [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) of [Azure Queue storage](../storage/common/storage-introduction.md).)

Als de afbeelding hierboven al aangeeft, alle virtuele machines in één toepassing worden uitgevoerd in dezelfde cloudservice. Gebruikers toegang tot de toepassing via een enkel openbaar IP-adres, met aanvragen automatisch worden verdeeld met gelijke taakverdeling voor virtuele machines van de toepassing. Het platform [wordt geschaald en implementeert](cloud-services-how-to-scale-portal.md) de virtuele machines in een Azure Cloud Services-toepassing op een manier die een single point of hardware-uitval voorkomt.

Hoewel toepassingen worden uitgevoerd in virtuele machines, is het belangrijk om te weten dat Azure Cloud Services PaaS, niet voor infrastructuur als een dienst (IaaS biedt). Hier volgt één manier om na te denken over het. Met IaaS, zoals Azure Virtual Machines, u eerst maakt en configureert u de omgeving die in uw toepassing wordt uitgevoerd. Vervolgens implementeert u uw toepassing in deze omgeving. U bent verantwoordelijk voor het beheren van veel van deze wereld door het uitvoeren van handelingen zoals het implementeren van nieuwe patches versies van het besturingssysteem op elke virtuele machine. In PaaS, daarentegen, is het alsof de omgeving al bestaat. U moet doen, is uw toepassing implementeert. Beheer van het platform waarop het wordt uitgevoerd, zoals het implementeren van nieuwe versies van het besturingssysteem, wordt voor u afgehandeld.

## <a name="scaling-and-management"></a>Schaalbaarheid en beheer
Met Azure Cloud Services kunt maken u virtuele machines niet. In plaats daarvan het bieden van een configuratiebestand dat Azure wordt aangegeven hoeveel van elke gewenste, zoals "drie exemplaren van de Webrol" en "twee instanties van de werkrol." Het platform maakt ze vervolgens voor u. U nog steeds kiezen [welke grootte](cloud-services-sizes-specs.md) die back-ups van virtuele machines moeten zijn, maar u niet expliciet deze zelf maken. Als uw toepassing nodig heeft om een hoger belasting te verwerken, kunt u vragen voor meer virtuele machines en Azure maakt deze instanties. Als de belasting afneemt, kunt u de exemplaren afgesloten en betalen voor deze stoppen.

Een Azure Cloud Services-toepassing is algemeen beschikbaar gesteld aan gebruikers via een proces in twee stappen. Een ontwikkelaar eerste [de toepassing uploadt](cloud-services-how-to-create-deploy-portal.md) naar het faseringsgebied van het platform. Als de ontwikkelaar is gereed om te maken van de toepassing live, ze de Azure-portal gebruiken om te wisselen met de productie voor fasering. Dit [schakelen tussen fasering en productie](cloud-services-how-to-manage-portal.md#swap-deployments-to-promote-a-staged-deployment-to-production) kan worden uitgevoerd zonder uitvaltijd, waarmee een toepassing die wordt uitgevoerd naar een nieuwe versie worden bijgewerkt zonder de gebruikers verstoord.

## <a name="monitoring"></a>Bewaking
Azure Cloud Services biedt ook de bewaking. Zoals virtuele Machines, het detecteert een mislukte fysieke server en de virtuele machines die werden uitgevoerd op die server op een nieuwe virtuele machine opnieuw wordt opgestart. Maar Azure Cloud Services detecteert ook mislukte VM's en toepassingen, niet alleen hardwarefouten. In tegenstelling tot virtuele Machines, heeft een agent in elke rol web- en werkrollen en het is dus kunnen nieuwe VM's en exemplaren van een toepassing starten wanneer er fouten optreden.

De aard van de PaaS van Azure Cloud Services heeft andere implicaties te. Een van de belangrijkste is dat deze technologie-toepassingen moeten worden geschreven naar het correct wordt uitgevoerd als een web- of werkrollen rolinstantie is mislukt. Een Azure Cloud Services-toepassing mag niet om dit te bereiken, staat in het bestandssysteem van een eigen virtuele machines worden onderhouden. In tegenstelling tot virtuele machines met virtuele Machines gemaakt, zijn schrijfbewerkingen die zijn aangebracht in Azure Cloud Services-VM's niet persistent. Er is niets, zoals een gegevensschijf van virtuele Machines. Een Azure Cloud Services-toepassing moet alle statussen in plaats daarvan expliciet schrijven naar Azure SQL Database, blobs, tabellen of andere externe opslag. Het bouwen van toepassingen op deze manier kunt u ze gemakkelijker te schalen en beter bestand tegen bij een storing, die beide belangrijke doelstellingen van Azure Cloud Services.

## <a name="next-steps"></a>Volgende stappen
* [Een cloudservice-app maken in .NET](cloud-services-dotnet-get-started.md) 
* [Een cloudservice-app maken in Node.js](cloud-services-nodejs-develop-deploy-app.md) 
* [Een cloudservice-app maken in PHP](../cloud-services-php-create-web-role.md) 
* [Een cloudservice-app maken in Python](cloud-services-python-ptvs.md)



