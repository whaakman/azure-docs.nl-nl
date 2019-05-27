---
title: Wat te doen in het geval van een Azure-service wordt onderbroken gevolgen heeft voor Azure Cloud Services | Microsoft Docs
description: Meer informatie over wat te doen in het geval van een onderbreking van de Azure-service die gevolgen heeft voor Azure Cloud Services.
services: cloud-services
documentationcenter: ''
author: mmccrory
manager: timlt
editor: ''
ms.assetid: e52634ab-003d-4f1e-85fa-794f6cd12ce4
ms.service: cloud-services
ms.workload: cloud-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: memccror
ms.openlocfilehash: 976bb43fd3e6d6fdb19c733affd4afa2e49e482c
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65967681"
---
# <a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Wat te doen in het geval van een Azure-service wordt onderbroken gevolgen heeft voor Azure Cloud Services
Bij Microsoft werken we er hard om ervoor te zorgen dat onze services altijd beschikbaar zijn wanneer u ze nodig hebt. Krachten buiten de controle soms invloed hebben op ons op een manier die ertoe leiden niet-geplande serviceonderbrekingen dat.

Microsoft biedt een Service Level Agreement (SLA) voor de services als een toezegging voor actieve tijdsduur en connectiviteit. De SLA voor afzonderlijke Azure-services kunt u vinden op [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

Azure heeft al veel ingebouwde platformfuncties die ondersteuning bieden voor toepassingen met hoge beschikbaarheid. Lees voor meer informatie over deze services, [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

In dit artikel bevat informatie over een waar noodherstelscenario, wanneer een hele regio optreedt in een uitval vanwege de grote natuurramp of wijdverbreid service wordt onderbroken. Dit zelden zijn, maar u moet voorbereiden voor de mogelijkheid dat er een storing van een hele regio is. Als een hele regio optreedt in een service wordt onderbroken, het lokaal redundante kopieën van uw gegevens tijdelijk niet beschikbaar. Als u geo-replicatie hebt ingeschakeld, worden drie extra kopieën van uw Azure Storage-blobs en tabellen zijn opgeslagen in een andere regio. In het geval van een volledige regionale stroomstoring of een noodsituatie waarin de primaire regio kan niet worden hersteld, remaps van alle van de DNS-vermeldingen naar de regio geo-gerepliceerd Azure.

> [!NOTE]
> Let erop dat u geen controle over dit proces hebt en dit alleen doet zich voor een heel datacenter serviceonderbrekingen. Als gevolg hiervan moet u ook zijn afhankelijk van andere back-upstrategieën toepassingsspecifieke te bereiken van het hoogste niveau van beschikbaarheid. Zie voor meer informatie, [herstel na noodgevallen en hoge beschikbaarheid voor toepassingen die zijn gebouwd op Microsoft Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md). Als u wilt mogelijk van invloed zijn op uw eigen failover, kunt u rekening houden met het gebruik van [geo-redundante opslag met leestoegang (RA-GRS)](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage), deze toepassing maakt een alleen-lezen kopie van uw gegevens in een andere regio.
>
>


## <a name="option-1-use-a-backup-deployment-through-azure-traffic-manager"></a>Optie 1: Een back-ups via Azure Traffic Manager-implementatie gebruiken
De meest robuuste oplossing voor noodherstel omvat het onderhouden van meerdere implementaties van uw toepassing in verschillende regio's en klik vervolgens met behulp van [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) om verkeer tussen hen te regelen. Met Azure Traffic Manager biedt meerdere [routeringsmethoden](../traffic-manager/traffic-manager-routing-methods.md), zodat u kiezen of u kunt voor het beheren van uw implementaties die gebruikmaken van een primaire/back-up-model of scheiden van verkeer tussen beide.

![Azure Cloud Services te verdelen over regio's met Azure Traffic Manager](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

Voor het snelste antwoord aan het verlies van een regio, is het belangrijk dat u de Traffic Manager configureert [eindpuntbewaking](../traffic-manager/traffic-manager-monitoring.md).

## <a name="option-2-deploy-your-application-to-a-new-region"></a>Optie 2: De toepassing implementeren naar een nieuwe regio
Meerdere actieve implementaties beheren zoals beschreven in de vorige optie leidt tot extra doorlopende kosten. Als de beoogde hersteltijd (RTO) flexibel genoeg is en u de oorspronkelijke code of het gecompileerde Cloud Services-pakket hebt, kunt u een nieuw exemplaar van uw toepassing in een andere regio maken en bijwerken van uw DNS-records om te verwijzen naar de nieuwe implementatie.

Zie voor meer informatie over het maken en implementeren van een cloud service-toepassing [maken en implementeren van een cloudservice](cloud-services-how-to-create-deploy-portal.md).

Afhankelijk van de gegevensbronnen van uw toepassing moet u mogelijk om te controleren of de procedures voor het herstellen voor de gegevensbron van uw toepassing.

* Zie voor Azure Storage-gegevensbronnen, [Azure Storage-replicatie](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) bij het controleren van de opties die beschikbaar zijn op basis van het model van de replicatie hebt gekozen voor uw toepassing.
* Lees voor SQL Database-bronnen, [overzicht: Zakelijke bedrijfscontinuïteit en noodherstel met SQL-Database in de cloud](../sql-database/sql-database-business-continuity.md) bij het controleren van de opties die beschikbaar zijn op basis van het replicatiemodel gekozen voor uw toepassing.


## <a name="option-3-wait-for-recovery"></a>Optie 3: Wachten op herstel
In dit geval is geen actie van uw kant vereist, maar de service is niet beschikbaar totdat de regio is hersteld. U kunt de status van de huidige service zien op de [Azure Service Health Dashboard](https://azure.microsoft.com/status/).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het implementeren van een herstel na noodgevallen en hoge beschikbaarheid-strategie, [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Zie voor het ontwikkelen van een gedetailleerde technische kennis van de mogelijkheden van een cloudplatform, [technische richtlijnen voor Azure flexibiliteit](../resiliency/resiliency-technical-guidance.md).