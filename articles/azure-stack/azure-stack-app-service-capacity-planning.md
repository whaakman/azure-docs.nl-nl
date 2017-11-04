---
title: Capaciteitsplanning voor Azure App Service-serverfuncties in Azure-Stack | Microsoft Docs
description: Capaciteitsplanning voor Azure App Service-serverfuncties in Azure-Stack
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: anwestg
ms.openlocfilehash: 4ad91def00ca73f91f0ffd8e57afa442a93176f6
ms.sourcegitcommit: b83781292640e82b5c172210c7190cf97fabb704
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="capacity-planning-for-azure-app-service-server-roles-in-azure-stack"></a>Capaciteitsplanning voor Azure App Service-serverfuncties in Azure-Stack

Voor het inrichten van een gereed productie-implementatie van Azure App Service op Azure-Stack, moet u plannen voor de capaciteit die u verwacht dat het systeem om te ondersteunen.  Hier vindt u richtlijnen voor het minimum aantal exemplaren en rekencapaciteit SKU's moet u voor een productie-implementatie.

Uw strategie voor App Service-capaciteit met behulp van deze richtlijnen voldoet, kunt u plannen. Toekomstige versies van Azure-Stack biedt opties voor hoge beschikbaarheid voor App Service.

| App Service-serverrol | Minimaal aanbevolen aantal exemplaren | Aanbevolen compute SKU|
| --- | --- | --- |
| Domeincontroller | 2 | A1 |
| Front-End | 2 | A1 |
| Beheer | 2 | A3 |
| Uitgever | 2 | A1 |
| Werknemers van de Web - gedeeld | 2 | A1 |
| Werknemers van de Web - specifiek | 2 per laag | A1 |

## <a name="controller-role"></a>Functie van domeincontroller

**Aanbevolen minimum**: twee exemplaren van A1 standaard

De Azure App Service-Controller ervaringen doorgaans lage verbruik van CPU, geheugen en netwerkbronnen. Voor hoge beschikbaarheid, moet u echter twee domeincontrollers hebben. Twee domeincontrollers zijn ook het maximum aantal domeincontrollers dat is toegestaan. Kunt u de tweede websites Controller direct van het installatieprogramma tijdens de implementatie.

## <a name="front-end-role"></a>Front End role

**Aanbevolen minimum**: twee exemplaren van A1 standaard

De front-end stuurt aanvragen naar de Web werknemers, afhankelijk van de beschikbaarheid van Web Worker. Er is meer dan één Front-End voor hoge beschikbaarheid, en u kunt meer dan twee hebben. Voor capaciteitsplanning doeleinden kunt dat elke core ongeveer 100 aanvragen per seconde kan verwerken.

## <a name="management-role"></a>Beheer van rol

**Aanbevolen minimum**: twee exemplaren van A3

De functie voor beheer van Azure App Service is verantwoordelijk voor de App Service Azure Resource Manager en API-eindpunten, portal uitbreidingen (admin, tenant, Functions-portal) en de data-service. De Management Server-rol is doorgaans alleen over 4 GB RAM-geheugen in een productieomgeving. Deze merken echter hoog CPU-niveaus wanneer veel beheertaken (zoals het maken van websites) worden uitgevoerd. Er is meer dan één server die aan deze rol toegewezen voor hoge beschikbaarheid, en ten minste twee cores per server.

## <a name="publisher-role"></a>Uitgeversrol

**Aanbevolen minimum**: twee exemplaren van A1

Als veel gebruikers tegelijk publiceert, is de rol Publisher ondervinden intensief CPU-gebruik. Voor hoge beschikbaarheid, meer dan één rol voor Publisher beschikbaar maken.  De uitgever verwerkt alleen de FTP-/ FTPS-verkeer.

## <a name="web-worker-role"></a>Web-werkrol

**Aanbevolen minimum**: twee exemplaren van A1

Voor hoge beschikbaarheid, moet u ten minste vier Web werkrollen hebben, twee voor gedeelde website modus en twee voor elke laag toegewezen werknemer die u wilt aanbieden. De gedeelde en speciale berekenings-modi bieden verschillende niveaus van service aan tenants. U moet mogelijk meer Web werknemers als er veel klanten:
 - speciale berekenings-modus worker lagen (die bronintensieve) gebruiken
 - in gedeelde compute-modus uitgevoerd.

Nadat een gebruiker is gemaakt op een App Service-Plan voor een speciale berekenings-modus SKU, het aantal Web werknemer (s) die worden opgegeven in de betreffende App Service-Plan niet langer beschikbaar voor gebruikers.

Azure Functions om aan te bieden gebruikers in het plan verbruik model, moet u gedeelde Web werknemers implementeren.

Bij het kiezen van het aantal gedeelde Web-werkrollen te gebruiken, controleert deze overwegingen:

- **Geheugen**: geheugen is de meest kritieke bron voor een Web-werkrol. Er is onvoldoende geheugen invloed website op de prestaties wanneer virtueel geheugen van de schijf wordt gewisseld. Elke server vereist ongeveer 1,2 GB aan RAM-geheugen voor het besturingssysteem. RAM-geheugen boven deze drempelwaarde kan worden gebruikt om uit te voeren van websites.
- **Percentage van actieve websites**: normaal gesproken ongeveer 5 procent van de toepassingen in een Azure App Service op Azure-Stack implementatie actief zijn. Het percentage van de toepassingen die actief op elk gewenst moment zijn kan echter hoger of lager zijn. Met een actieve toepassingsrol frequentie van 5 procent, het maximum aantal toepassingen in een Azure App Service op de implementatie van de Azure-Stack te plaatsen moet kleiner zijn dan:
    - 20 keer het aantal actieve websites (5 x 20 = 100).
- **Gemiddelde geheugengebruik:**: het gemiddelde geheugengebruik voor toepassingen die zijn waargenomen in productieomgevingen is ongeveer 70 MB. Daarom kan het geheugen toegewezen aan alle Web Worker-rol computers of virtuele machines als volgt worden berekend:

    *Aantal toepassingen ingericht * 70 MB * 5% - (nummer van Web-werkrollen * 1044 MB)*

   Als er 5000 toepassingen op een omgeving met 10 Web werkrollen, moet elke Web Worker-rol VM 7060 MB RAM-geheugen hebben:

   5.000 * 70 * 0,05 – (10 * 1044) = 7060 (= ongeveer 7 GB)

   Zie voor meer informatie over het toevoegen van meer werkrolinstanties [toe te voegen meer werkrollen](azure-stack-app-service-add-worker-roles.md).

## <a name="file-server-role"></a>Bestandsserverfunctie

Voor de rol bestandsserver, kunt u een zelfstandige bestandsserver voor ontwikkeling en testen, bijvoorbeeld bij het implementeren van Azure App Service op de Azure-Stack Development Kit kunt u deze sjabloon - https://aka.ms/appsvconmasdkfstemplate. Voor productiedoeleinden moet u een vooraf geconfigureerde Windows-bestandsserver of een vooraf geconfigureerde niet-Windows-bestandsserver.

De functie bestandsserver optreedt in een productieomgeving intensieve schijf-i/o. Omdat ook alle inhoud en toepassing bestanden voor websites van de gebruiker nieuwste, moet u een van de volgende voor deze rol vooraf configureren:
- een Windows-bestandsserver
- Bestandsservercluster
- een niet-Windows-bestandsserver
- bestandsservercluster
- Apparaat NAS (Network Attached Storage) voor meer informatie Zie [inrichten van een bestandsserver](azure-stack-app-service-before-you-get-started.md#prepare-the-file-server).

## <a name="next-steps"></a>Volgende stappen

[Voordat u aan de slag met App-Service op Azure-Stack](azure-stack-app-service-before-you-get-started.md)
