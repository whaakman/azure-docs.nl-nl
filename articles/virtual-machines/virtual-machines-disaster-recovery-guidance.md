---
title: Herstel na noodgevallen voor Azure VM's | Microsoft Docs
description: Meer informatie over wat te doen in het geval dat een onderbreking van de Azure-service heeft impact op virtuele machines van Azure.
services: virtual-machines
documentationcenter: ''
author: kmouss
manager: gwallace
editor: ''
ms.assetid: 65272148-ff06-4bce-91f1-851d706d4d40
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: kmouss;aglick
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f071e1ae97228a16799d391e226ba44b99f6096e
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721181"
---
# <a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-vms"></a>Wat te doen in het geval dat een onderbreking van de Azure-service heeft impact op virtuele Azure-machines
Bij Microsoft werken we er hard om ervoor te zorgen dat onze services altijd beschikbaar zijn wanneer u ze nodig hebt. Krachten buiten de controle soms invloed hebben op ons op een manier die ertoe leiden niet-geplande serviceonderbrekingen dat.

Microsoft biedt een Service Level Agreement (SLA) voor de services als een toezegging voor actieve tijdsduur en connectiviteit. De SLA voor afzonderlijke Azure-services kunt u vinden op [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

Azure heeft al veel ingebouwde platformfuncties die ondersteuning bieden voor toepassingen met hoge beschikbaarheid. Lees voor meer informatie over deze services, [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

In dit artikel bevat informatie over een waar noodherstelscenario, wanneer een hele regio optreedt in een uitval vanwege de grote natuurramp of wijdverbreid service wordt onderbroken. Dit zelden zijn, maar u moet voorbereiden voor de mogelijkheid dat er een storing van een hele regio is. Als een hele regio optreedt in een service wordt onderbroken, het lokaal redundante kopieën van uw gegevens tijdelijk niet beschikbaar. Als u geo-replicatie hebt ingeschakeld, worden drie extra kopieën van uw Azure Storage-blobs en tabellen zijn opgeslagen in een andere regio. In het geval van een volledige regionale stroomstoring of een noodsituatie waarin de primaire regio kan niet worden hersteld, remaps van alle van de DNS-vermeldingen naar de regio geo-gerepliceerd Azure.

We bieden de volgende richtlijnen voor Azure-machines in het geval van een onderbreking van de service van de hele regio waar uw virtuele machine van Azure-toepassing is geïmplementeerd, kunt u deze zelden worden verwerkt.

## <a name="option-1-initiate-a-failover-by-using-azure-site-recovery"></a>Optie 1: Initieer een failover met behulp van Azure Site Recovery
U kunt Azure Site Recovery voor uw VM's configureren, zodat u uw toepassing met één klik in een paar minuten kunt herstellen. U kunt repliceren naar Azure-regio van uw keuze en niet beperkt tot het gekoppelde regio's. U kunt aan de slag door [repliceren van uw virtuele machines](https://aka.ms/a2a-getting-started). U kunt [maken van een herstelplan](../site-recovery/site-recovery-create-recovery-plans.md) zodat u de volledige failoverproces voor uw toepassing automatiseren kunt. U kunt [uw testfailovers](../site-recovery/site-recovery-test-failover-to-azure.md) vooraf zonder gevolgen voor productietoepassing of de voortdurende replicatie. In het geval van een onderbreking van de primaire regio, die u zojuist hebt [initieer een failover](../site-recovery/site-recovery-failover.md) en zorg voor uw toepassing in de doelregio.


## <a name="option-2-wait-for-recovery"></a>Optie 2: Wachten op herstel
In dit geval is geen actie van uw kant vereist. Op de hoogte dat we hard werken voor het herstellen van de beschikbaarheid van de service. U kunt de status van de huidige service zien op onze [Azure Service Health Dashboard](https://azure.microsoft.com/status/).

Dit is de beste optie als u geen Azure Site Recovery, geografisch redundante opslag met leestoegang of geografisch redundante opslag voorafgaand aan de onderbreking hebt ingesteld. Als u hebt ingesteld geografisch redundante opslag of geografisch redundante opslag met leestoegang voor het opslagaccount waar uw virtuele machine virtuele harde schijven (VHD's) worden opgeslagen, kunt u bekijken om te herstellen van de VHD-basisinstallatiekopie en probeert voor het inrichten van een nieuwe virtuele machine uit. Dit is niet een voorkeursoptie omdat er geen garanties van synchronisatie van gegevens. Als gevolg daarvan kan is deze optie niet noodzakelijkerwijs werken.


> [!NOTE]
> Let erop dat u geen controle over dit proces hebt en deze alleen wordt uitgevoerd voor de gehele regio serviceonderbrekingen. Als gevolg hiervan moet u ook zijn afhankelijk van andere back-upstrategieën toepassingsspecifieke te bereiken van het hoogste niveau van beschikbaarheid. Zie voor meer informatie de sectie over [strategieën voor herstel na noodgevallen](https://docs.microsoft.com/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan).
>
>

## <a name="next-steps"></a>Volgende stappen

- Start [beveiligen van uw toepassingen die worden uitgevoerd op virtuele Azure-machines](https://aka.ms/a2a-getting-started) met Azure Site Recovery

- Zie voor meer informatie over het implementeren van een herstel na noodgevallen en hoge beschikbaarheid-strategie, [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

- Zie voor het ontwikkelen van een gedetailleerde technische kennis van de mogelijkheden van een cloudplatform, [technische richtlijnen voor Azure flexibiliteit](../resiliency/resiliency-technical-guidance.md).


- Als u de instructies zijn niet wissen, of als u Microsoft de bewerkingen namens dat wilt, neem dan contact op met [Customer Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
