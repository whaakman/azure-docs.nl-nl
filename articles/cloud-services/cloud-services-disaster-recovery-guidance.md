---
title: Wat te doen in het geval van een Azure-service wordt onderbroken die van invloed is op Azure Cloud Services | Microsoft Docs
description: Meer informatie over wat te doen in het geval van een onderbreking van de Azure-service die van invloed is op Azure Cloud Services.
services: cloud-services
documentationcenter: 
author: mmccrory
manager: timlt
editor: 
ms.assetid: e52634ab-003d-4f1e-85fa-794f6cd12ce4
ms.service: cloud-services
ms.workload: cloud-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: mmccrory
ms.openlocfilehash: db6a980b85ea5ef8cbbba4ba5a36f9d033739df1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Wat te doen in het geval van een Azure-service wordt onderbroken die van invloed is op Azure Cloud Services
Bij Microsoft werken we hard om ervoor te zorgen dat onze services altijd voor u beschikbaar zijn wanneer u deze nodig. Dwingt buiten ons wil soms invloed hebben op ons op een manier die niet-geplande serviceonderbrekingen veroorzaken.

Microsoft biedt een Service Level Agreement (SLA) voor de services als een toezegging voor bedrijfstijd en connectiviteit. De SLA voor afzonderlijke Azure-services kan worden gevonden op [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

Azure heeft al veel ingebouwde platformfuncties die ondersteuning bieden voor maximaal beschikbare toepassingen. Lees voor meer informatie over deze services, [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

In dit artikel bevat informatie over waar noodherstelscenario wanneer een hele regio optreedt in een storing veroorzaakt door grote natuurramp of wijdverbreid service wordt onderbroken. Dit zeldzame instanties zijn, maar u moet voorbereiden voor de mogelijkheid dat er een storing van een hele regio is. Als een hele regio optreedt in een onderbreking van de service, de lokaal redundante exemplaren van uw gegevens tijdelijk niet beschikbaar. Als u geo-replicatie hebt ingeschakeld, worden de drie extra kopieën van uw Azure Storage-blobs en tabellen opgeslagen in een andere regio. In het geval van een volledige regionale uitval of een ramp optreedt waarbij de primaire regio kan niet worden hersteld, remaps Azure alle van de DNS-vermeldingen voor de regio geo-replicatie.

> [!NOTE]
> Let erop dat u geen controle over dit proces hebt en alleen voor het hele datacenter serviceonderbrekingen kunnen ontstaan geldt. Daarom moet u ook zijn afhankelijk van andere toepassingsspecifieke back-strategieën voor het hoogste niveau van beschikbaarheid. Zie voor meer informatie [herstel na noodgevallen en hoge beschikbaarheid voor Microsoft Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md). Als u wilt kunnen invloed hebben op uw eigen failover mogelijk wilt u het gebruik van [geografisch redundante opslag met leestoegang (RA-GRS)](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage), waarbij automatisch een alleen-lezen kopie van uw gegevens in een andere regio.
>
>


## <a name="option-1-use-a-backup-deployment-through-azure-traffic-manager"></a>Optie 1: Een back-implementatie via Azure Traffic Manager gebruiken
De meest robuuste noodherstel omvat het onderhouden van meerdere implementaties van uw toepassing in verschillende regio's en klik vervolgens met behulp van [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) om verkeer tussen hen te regelen. Azure Traffic Manager biedt meerdere [methoden voor het doorsturen](../traffic-manager/traffic-manager-routing-methods.md), zodat u kiezen of u kunt voor het beheren van uw implementaties die gebruikmaken van een model primaire/back-up of scheiden van verkeer tussen hen.

![Azure Cloud Services te verdelen over regio's met Azure Traffic Manager](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

Voor de snelste reactie op het verlies van een regio, is het belangrijk dat u de Traffic Manager configureert [eindpuntcontrole](../traffic-manager/traffic-manager-monitoring.md).

## <a name="option-2-deploy-your-application-to-a-new-region"></a>Optie 2: Uw toepassing implementeren naar een nieuw gebied
Aanvullende lopende kosten leidt ertoe dat meerdere actieve implementaties behouden, zoals beschreven in de vorige optie. Als uw beoogde hersteltijd (RTO) flexibel genoeg is en u de oorspronkelijke code of gecompileerde Cloud Services-pakket hebt, kunt u een nieuw exemplaar van uw toepassing in een andere regio maken en bijwerken van de DNS-records om te verwijzen naar de nieuwe implementatie.

Zie voor meer informatie over het maken en implementeren van een servicetoepassing cloud [maken en implementeren van een cloudservice](cloud-services-how-to-create-deploy-portal.md).

Afhankelijk van uw toepassing gegevensbronnen, moet u wellicht de herstelprocedures voor uw toepassingsgegevensbron controleren.

* Zie voor gegevensbronnen voor Azure Storage, [Azure Storage-replicatie](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage) bij het controleren van de opties die beschikbaar zijn op basis van het model van de replicatie hebt geselecteerd voor uw toepassing.
* Lees voor de bronnen van de SQL-Database, [overzicht: Cloud zakelijke continuïteit en database noodherstel met SQL Database](../sql-database/sql-database-business-continuity.md) bij het controleren van de opties die beschikbaar zijn op basis van het gekozen replicatiemodel voor uw toepassing.


## <a name="option-3-wait-for-recovery"></a>Optie 3: Wacht voor herstel
In dit geval is geen actie ondernemen vereist, maar de service is niet beschikbaar totdat de regio is hersteld. U kunt de status van de huidige zien op de [Azure Service Health Dashboard](https://azure.microsoft.com/status/).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het implementeren van een strategie van hoge beschikbaarheid voor herstel na noodgevallen en [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Zie voor het ontwikkelen van een gedetailleerde technische kennis van een cloudplatform mogelijkheden [technische richtlijnen voor Azure tolerantie](../resiliency/resiliency-technical-guidance.md).