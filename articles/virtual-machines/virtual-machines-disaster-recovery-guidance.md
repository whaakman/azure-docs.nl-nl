---
title: Herstel na noodgevallen voor Azure Virtual machines | Microsoft Docs
description: Meer informatie over wat te doen in het geval dat een onderbreking van de Azure-service heeft impact op de virtuele machines in Azure.
services: virtual-machines
documentationcenter: 
author: kmouss
manager: timlt
editor: 
ms.assetid: 65272148-ff06-4bce-91f1-851d706d4d40
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: kmouss;aglick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fb986a41e33501ee71c93a48457ac4114e33c671
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-vms"></a>Wat te doen in het geval dat een onderbreking van de Azure-service heeft impact op Azure Virtual machines
Bij Microsoft werken we hard om ervoor te zorgen dat onze services altijd voor u beschikbaar zijn wanneer u deze nodig. Dwingt buiten ons wil soms invloed hebben op ons op een manier die niet-geplande serviceonderbrekingen veroorzaken.

Microsoft biedt een Service Level Agreement (SLA) voor de services als een toezegging voor bedrijfstijd en connectiviteit. De SLA voor afzonderlijke Azure-services kan worden gevonden op [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

Azure heeft al veel ingebouwde platformfuncties die ondersteuning bieden voor maximaal beschikbare toepassingen. Lees voor meer informatie over deze services, [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

In dit artikel bevat informatie over waar noodherstelscenario wanneer een hele regio optreedt in een storing veroorzaakt door grote natuurramp of wijdverbreid service wordt onderbroken. Dit zeldzame instanties zijn, maar u moet voorbereiden voor de mogelijkheid dat er een storing van een hele regio is. Als een hele regio optreedt in een onderbreking van de service, de lokaal redundante exemplaren van uw gegevens tijdelijk niet beschikbaar. Als u geo-replicatie hebt ingeschakeld, worden de drie extra kopieën van uw Azure Storage-blobs en tabellen opgeslagen in een andere regio. In het geval van een volledige regionale uitval of een ramp optreedt waarbij de primaire regio kan niet worden hersteld, remaps Azure alle van de DNS-vermeldingen voor de regio geo-replicatie.

Als u deze zeldzame exemplaren verwerken, bieden we de volgende richtlijnen voor Azure virtual machines in het geval van een onderbreking van de service van de hele regio waar uw virtuele machine van Azure-toepassing wordt geïmplementeerd.

## <a name="option-1-initiate-a-failover-by-using-azure-site-recovery"></a>Optie 1: Start een failover met behulp van Azure Site Recovery
U kunt Azure Site Recovery voor uw virtuele machines configureren zodat u uw toepassing met één klik in kwestie van minuten kan herstellen. U kunt repliceren naar Azure-regio van uw keuze en niet beperkt tot gekoppelde regio's. U kunt aan de slag door [uw virtuele machines repliceren](https://aka.ms/a2a-getting-started). U kunt [een herstelplan maken](../site-recovery/site-recovery-create-recovery-plans.md) zodat u de volledige failoverproces voor uw toepassing automatiseren kunt. U kunt [uw testfailovers](../site-recovery/site-recovery-test-failover-to-azure.md) vooraf zonder enige impact op de productietoepassing of de lopende replicatie. In het geval van een onderbreking van de primaire regio die u zojuist hebt [initieer een failover](../site-recovery/site-recovery-failover.md) en uw toepassing te brengen in de doelregio.


## <a name="option-2-wait-for-recovery"></a>Optie 2: Wachten op herstel
In dit geval is geen actie ondernemen vereist. Weten dat we naar eer en geweten werken voor het herstellen van de beschikbaarheid van de service. U ziet de status van de huidige op onze [Azure Service Health Dashboard](https://azure.microsoft.com/status/).

Dit is de beste optie als u geen Azure Site Recovery, geografisch redundante opslag met leestoegang of geografisch redundante opslag voorafgaand aan de onderbreking hebt ingesteld. Als u hebt ingesteld geografisch redundante opslag of geografisch redundante opslag met leestoegang voor het opslagaccount waar uw virtuele machine virtuele harde schijven (VHD's) zijn opgeslagen, kunt u bekijken om te herstellen van de basisinstallatiekopie VHD en probeert voor het inrichten van een nieuwe virtuele machine uit. Dit is geen voorkeur optie omdat er geen garanties van synchronisatie van gegevens. Deze optie kan als gevolg daarvan kan niet worden gegarandeerd werken.


> [!NOTE]
> Let erop dat u geen controle over dit proces hebt en alleen voor de regio hele serviceonderbrekingen kunnen ontstaan geldt. Daarom moet u ook zijn afhankelijk van andere toepassingsspecifieke back-strategieën voor het hoogste niveau van beschikbaarheid. Zie voor meer informatie het gedeelte over [gegevens strategieën voor herstel na noodgevallen](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications#data-strategies-for-disaster-recovery).
>
>

## <a name="next-steps"></a>Volgende stappen

- Start [beveiligen van uw toepassingen die worden uitgevoerd op virtuele machines in Azure](https://aka.ms/a2a-getting-started) met Azure Site Recovery

- Zie voor meer informatie over het implementeren van een strategie van hoge beschikbaarheid voor herstel na noodgevallen en [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

- Zie voor het ontwikkelen van een gedetailleerde technische kennis van een cloudplatform mogelijkheden [technische richtlijnen voor Azure tolerantie](../resiliency/resiliency-technical-guidance.md).


- Als u de instructies zijn niet wissen of als u Microsoft dat wilt voor de bewerkingen namens u, neem dan contact op met [Customer Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
