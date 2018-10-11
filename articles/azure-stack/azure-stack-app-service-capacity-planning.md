---
title: Capaciteitsplanning voor Azure App Service-server-rollen in Azure Stack | Microsoft Docs
description: Capaciteitsplanning voor Azure App Service-server-rollen in Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: sethm
ms.reviewer: anwestg
ms.openlocfilehash: 7cdcd8b7e9814c206255077fae0af2029fab6583
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078110"
---
# <a name="capacity-planning-for-azure-app-service-server-roles-in-azure-stack"></a>Capaciteitsplanning voor Azure App Service-server-rollen in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Als u een gereed productie-implementatie van Azure App Service in Azure Stack instelt, moet u van plan bent voor de capaciteit die u verwacht dat het systeem om te ondersteunen.  

In dit artikel bevat richtlijnen voor het minimum aantal rekenprocessen en rekencapaciteit SKU's die u voor een productie-implementatie gebruiken moet.

U kunt met behulp van deze richtlijnen strategie voor de capaciteit van uw App Service plannen. Toekomstige versies van Azure Stack biedt opties voor hoge beschikbaarheid voor App Service.

| App Service-serverrol | Minimaal aanbevolen aantal exemplaren | Aanbevolen compute SKU|
| --- | --- | --- |
| Controller | 2 | A1 |
| Front-end | 2 | A1 |
| Beheer | 2 | A3 |
| Uitgever | 2 | A1 |
| Webwerkrollen - gedeeld | 2 | A1 |
| Webwerkrollen - toegewezen | 2 per laag | A1 |

## <a name="controller-role"></a>De functie van domeincontroller

**Aanbevolen minimum**: twee exemplaren van Standard A1

De Azure App Service-Controller ervaringen doorgaans laag verbruik van CPU, geheugen- en netwerkbronnen. Voor hoge beschikbaarheid, moet u echter twee controllers hebben. Twee controllers zijn ook het maximum aantal domeincontrollers dat is toegestaan. Kunt u de tweede Web Sites-Controller direct van het installatieprogramma tijdens de implementatie.

## <a name="front-end-role"></a>Front-End role

**Aanbevolen minimum**: twee exemplaren van Standard A1

De Front-End worden aanvragen gerouteerd naar Webwerkrollen afhankelijk van beschikbaarheid van de Webwerkrol. Voor hoge beschikbaarheid, hebt u meer dan één Front-End en u kunt meer dan twee hebben. Voor doeleinden voor capaciteitsplanning, kunt u dat elke core ongeveer 100 aanvragen per seconde kan verwerken.

## <a name="management-role"></a>Beheerrol

**Aanbevolen minimum**: twee exemplaren van Standard A3

De functie voor beheer van Azure App Service is verantwoordelijk voor het App Service Azure Resource Manager en API-eindpunten, portal-extensies (admin, tenant, Functions-portal) en de data-service. De Management Server-rol wordt doorgaans alleen over 4 GB RAM-geheugen in een productie-omgeving vereist. Het kan echter hoog CPU-niveaus optreden wanneer u veel beheertaken (zoals het maken van web site) worden uitgevoerd. Voor hoge beschikbaarheid, moet u meer dan één server die is toegewezen aan deze rol hebben en ten minste twee cores per server.

## <a name="publisher-role"></a>Uitgeverrol

**Aanbevolen minimum**: twee exemplaren van Standard A1

Als veel gebruikers gelijktijdig publiceert, kan de rol van de uitgever intensief CPU-gebruik optreden. Voor hoge beschikbaarheid, meer dan één rol van de uitgever beschikbaar maken.  FTP-/ FTPS-verkeer wordt alleen verwerkt door de uitgever.

## <a name="web-worker-role"></a>Web-werkrol

**Aanbevolen minimum**: twee exemplaren van Standard A1

Voor hoge beschikbaarheid, moet u ten minste vier Webwerkrollen hebben, twee voor gedeelde web sitemodus en twee voor elke laag werkgroepen te te wijzen u van plan bent te bieden. De gedeelde en speciale berekenings-modi bieden verschillende niveaus van de service aan tenants. U moet mogelijk meer Webwerkrollen als veel van uw klanten zijn:

- Met behulp van werkrolniveaus van speciale berekenings-modus (de resource-intensieve.)
- Uitgevoerd in de gedeelde rekenmodus wordt ingeschakeld.

Nadat een gebruiker is gemaakt op een App Service-Plan voor een toegewezen rekenmodus wordt ingeschakeld SKU, het aantal Web werkrol(len) opgegeven in die App Service-Plan niet langer beschikbaar voor gebruikers.

Azure Functions om aan te bieden gebruikers in het plan verbruik model, moet u gedeelde Webwerkrollen implementeren.

Bij het bepalen van het aantal gedeelde Web werkrollen wilt gebruiken, Controleer deze overwegingen:

- **Geheugen**: geheugen is de meest kritieke resource voor een Web-werkrol. Er is onvoldoende geheugen heeft invloed op prestaties van de web site wanneer virtueel geheugen van de schijf wordt gewisseld. Elke server vereist ongeveer 1,2 GB aan RAM-geheugen voor het besturingssysteem. RAM-geheugen boven deze drempelwaarde kan worden gebruikt om uit te voeren van websites.
- **Percentage van actieve websites**: normaal gesproken ongeveer 5 procent van de toepassingen in een Azure App Service op Azure Stack-implementatie actief zijn. Het percentage van de toepassingen die actief op elk gewenst moment zijn kan echter hoger of lager zijn. Met een actieve toepassing snelheid van 5%, het maximum aantal toepassingen te plaatsen in een Azure App Service op Azure Stack-implementatie moet minder dan:
  - 20 keer het aantal actieve websites (5 x 20 = 100).
- **Het geheugengebruik van gemiddelde**: het gemiddelde geheugengebruik voor toepassingen die zijn waargenomen in een productieomgeving is ongeveer 70 MB. Met behulp van deze footprint, het geheugen toegewezen voor alle Webwerkrol rol computers of virtuele machines kan worden als volgt berekend:

    *Aantal ingerichte toepassingen * 70 MB * 5% - (nummer van de Webwerkrollen * 1044 MB)*

   Bijvoorbeeld, als er 5000 toepassingen op de omgeving die 10 Webwerkrol functies worden uitgevoerd, moet elke Web-werkrol VM 7060 MB RAM-geheugen:

   5.000 * 70 * 0,05 – (10 * 1044) = 7060 (= ongeveer 7 GB)

   Zie voor meer informatie over het toevoegen van meer werkrolinstanties [toe te voegen meer werkrollen](azure-stack-app-service-add-worker-roles.md).

## <a name="file-server-role"></a>Rol van bestandsserver

Voor de rol bestandsserver, kunt u een zelfstandige bestandsserver voor het ontwikkelen en testen, bijvoorbeeld bij het implementeren van Azure App Service op de Azure Stack Development Kit kunt u deze sjabloon - <https://aka.ms/appsvconmasdkfstemplate>. Voor productiedoeleinden moet u een vooraf geconfigureerde Windows-bestandsserver of een vooraf geconfigureerde niet-Windows-bestandsserver.

De bestandsserverfunctie ervaringen in een productieomgeving, intensieve schijf-i/o. Omdat het ook alle inhoud en -toepassingen bestanden voor gebruiker web sites nieuwste, moet u een van de volgende onderwerpen voor deze rol vooraf configureren:

- een Windows-bestandsserver
- een Windows-Bestandsservercluster
- een niet-Windows-bestandsserver
- een cluster met niet-Windows-bestandsserver
- een apparaat NAS (Network Attached Storage)

Zie voor meer informatie, [inrichten van een bestandsserver](azure-stack-app-service-before-you-get-started.md#prepare-the-file-server).

## <a name="next-steps"></a>Volgende stappen

[Voordat u aan de slag met App Service in Azure Stack](azure-stack-app-service-before-you-get-started.md)
