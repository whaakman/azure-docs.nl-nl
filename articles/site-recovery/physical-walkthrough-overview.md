---
title: Fysieke repliceren lokale servers naar Azure met Azure Site Recovery | Microsoft Docs
description: Biedt een overzicht van de stappen voor het repliceren van workloads die worden uitgevoerd op fysieke on-premises Windows of Linux-servers naar Azure met de Azure Site Recovery-service.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 20122f01-929a-4675-b85b-a9b99d2618bc
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 0a09b35e98dc0b2f5283c2a707a3a2b8ac9a39f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-physical-servers-to-azure-with-site-recovery"></a>Fysieke servers repliceren naar Azure met Site Recovery

In dit artikel biedt een overzicht van de stappen die nodig zijn voor het repliceren van fysieke on-premises Windows of Linux-servers naar Azure, met behulp van de [Azure Site Recovery](site-recovery-overview.md) service in de Azure portal.


## <a name="step-1-review-architecture-and-prerequisites"></a>Stap 1: Bekijk de architectuur en vereisten

Voordat u de implementatie begint, Controleer de scenarioarchitectuur en zorg dat u begrijpt dat alle onderdelen die u nodig hebt voor het instellen van de implementatie.

Ga naar [stap 1: Bekijk de architectuur](physical-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>Stap 2: Controleer vereisten

Zorg ervoor dat u beschikken over de vereisten voor elk onderdeel van de implementatie:

- **Vereisten voor Azure**: U moet een Microsoft Azure-account Azure-netwerken en opslagaccounts.
- **Site Recovery-onderdelen op lokale**: U moet een machine met on-premises Site Recovery-onderdelen.
- **Gerepliceerde machines**: u wilt repliceren Servers moeten voldoen aan de on-premises en Azure-vereisten.

Ga naar [stap 2: Controleer de vereisten en beperkingen](physical-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>Stap 3: Plan capaciteit

Als u een volledige implementatie uitvoert moet u achterhalen welke replicatie bronnen die u nodig. Als u een snelle instellen voor het testen van de omgeving doet, kunt u deze stap overslaan.

[Stap 3: Capaciteit plannen](physical-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>Stap 4: Netwerken plannen

U moet uitvoeren van een netwerk om ervoor te zorgen dat virtuele Azure-machines zijn verbonden met netwerken nadat er failover wordt uitgevoerd en die dat ze het juiste IP hebben-adressen.

Ga naar [stap 4: netwerken plannen](physical-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>Stap 5: Azure-resources voorbereiden

Instellen van Azure-netwerken en opslag voordat u begint. 

Ga naar [stap 5: Azure voorbereiden](physical-walkthrough-prepare-azure.md)


## <a name="step-6-set-up-a-vault"></a>Stap 6: Een kluis instellen

U instellen kunt een Recovery Services-kluis organiseren en beheren van replicatie. Wanneer u de kluis instelt, geeft u wat u wilt repliceren en waar u naar wilt repliceren naar.

Ga naar [stap 6: een kluis instellen](physical-walkthrough-create-vault.md)

## <a name="step-7-configure-source-and-target-settings"></a>Stap 7: De bron en doel-instellingen configureren

Configureer instellingen voor de bron en doelsite (Azure). Instellingen van de bronserver omvat Unified installatieprogramma voor het installeren van de on-premises Site Recovery-onderdelen uit te voeren.

Ga naar [stap 7: de bron en doel instellen](physical-walkthrough-source-target.md)

## <a name="step-8-set-up-a-replication-policy"></a>Stap 8: Een replicatiebeleid instellen

U moet een beleid om op te geven welke fysieke servers repliceren.

Ga naar [stap 8: een replicatiebeleid instellen](physical-walkthrough-replication.md)

## <a name="step-9-install-the-mobility-service"></a>Stap 9: Installeer de Mobility-service

De Mobility-service moet worden geïnstalleerd op elke server die u wilt repliceren. Er zijn een aantal manieren voor het instellen van de service, push of pull-installatie.

Ga naar [stap 9: de Mobility-service installeren](physical-walkthrough-install-mobility.md)

## <a name="step-10-enable-replication"></a>Stap 10: De replicatie inschakelen

Nadat de Mobility-service wordt uitgevoerd op een server, kunt u replicatie inschakelen. Na het inschakelen, treedt initiële replicatie van de virtuele machine op.

Ga naar [stap 10: replicatie inschakelen](physical-walkthrough-enable-replication.md)

## <a name="step-11-run-a-test-failover"></a>Stap 11: Een testfailover uitvoeren

Nadat de eerste replicatie is voltooid en de replicatie van verschillen wordt uitgevoerd, kunt u een testfailover om te controleren of dat alles werkt zoals verwacht uitvoeren.

Ga naar [stap 11: een testfailover uitvoeren](physical-walkthrough-test-failover.md)

