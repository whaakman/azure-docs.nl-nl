---
title: Hoe werkt Hyper-V-migratie met Azure Migrate-servermigratie? | Microsoft Docs
description: Biedt een overzicht van Hyper-V-migratie in Azure Migrate-servermigratie
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: raynew
ms.openlocfilehash: 9148e76a9f2abd369ae595422d785a347e58dfab
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811711"
---
# <a name="how-does-hyper-v-replication-work"></a>Hoe werkt Hyper-V-replicatie?

Dit artikel bevat een overzicht van de architectuur en processen die worden gebruikt wanneer u Hyper-V-machines met het hulpprogramma voor migratie van Azure-Server migreren migreren.

[Azure Migrate](migrate-services-overview.md) biedt een centraal punt voor het volgen van detectie, beoordeling en migratie van uw on-premises toepassingen, workloads en persoonlijke/openbare cloud, virtuele machines naar Azure. De hub biedt hulpprogramma's voor Azure Migrate voor evaluatie en migratie, evenals van derden independent software vendor (ISV)-aanbiedingen.

## <a name="agentless-migration"></a>Migratie zonder agent

Het hulpprogramma voor migratie van Azure-Server migreren biedt zonder agent replicatie voor on-premises Hyper-V-machines, met behulp van een Migratiewerkstroom die geoptimaliseerd voor Hyper-V. U installeert een agent voor software alleen op Hyper-V-hosts of knooppunten van het cluster. Er niets hoeft te worden geïnstalleerd op de Hyper-V-machines.

## <a name="server-migration-and-azure-site-recovery"></a>Migratie van servers en Azure Site Recovery

Azure Migrate servermigratie is een hulpprogramma voor migratie van on-premises werkbelastingen en cloud-gebaseerde virtuele machines, naar Azure. Site Recovery is een hulpprogramma van de herstel na noodgevallen. De hulpprogramma's voor sommige algemene technologie-onderdelen die worden gebruikt voor replicatie van gegevens delen, maar voor verschillende doeleinden. 


## <a name="architectural-components"></a>Architectuuronderdelen

![Architectuur](./media/hyper-v-replication-architecture/architecture.png)



**Onderdeel** | **Implementatie** | 
--- | --- 
**Replicatieprovider** | De Microsoft Azure Site Recovery-provider is geïnstalleerd op de Hyper-V-hosts en geregistreerd bij Azure migratie-servermigratie.<br/> De provider coördineert de replicatie voor Hyper-V-machines.
**Recovery Services-agent** | De Microsoft Azure Recovery Services-agent handelt de gegevensreplicatie af. Het werkt met de provider om gegevens te repliceren van Hyper-V-machines naar Azure.<br/> De gerepliceerde gegevens geüpload naar een opslagaccount in uw Azure-abonnement. De migratie van de hulpprogramma voor de processen de gerepliceerde gegevens, en deze toegepast op de replicaschijven in het abonnement. De replicaschijven worden gebruikt voor het maken van de Azure VM's wanneer u migreert.

- Onderdelen worden geïnstalleerd door een enkele setup-bestand gedownload van Azure Migrate-servermigratie in de portal.
- De provider en het apparaat moet u uitgaande HTTPS-poort 443 verbindingen gebruiken om te communiceren met Azure Migrate-servermigratie.
- Communicatie van de provider en agent is beveiligd en versleuteld.


## <a name="replication-process"></a>Replicatieproces

1. Wanneer u replicatie voor een Hyper-V-virtuele machine inschakelt, wordt de initiële replicatie begint.
2. Een Hyper-V-VM-momentopname wordt gemaakt.
3. VHD's op de virtuele machine zijn gerepliceerde één-op-één, tot ze allemaal zijn gekopieerd naar Azure. Tijd van de initiële replicatie is afhankelijk van de VM-grootte, en bandbreedte van het netwerk.
4. Schijfwijzigingen die zich tijdens de initiële replicatie voordoen worden bijgehouden met behulp van Hyper-V Replica en wordt opgeslagen in logboekbestanden (hrl-bestanden).
    - Logboekbestanden zijn in dezelfde map als de schijven.
    - Elke schijf heeft een bijbehorende hrl-bestand dat wordt verzonden naar een secundaire opslag.
    - De momentopname- en logboekbestanden nemen schijfbronnen in beslag terwijl de eerste replicatie wordt uitgevoerd.
4. Nadat de initiële replicatie is voltooid, de VM-momentopname is verwijderd en begint de replicatie van verschillen.
5. Incrementele schijfwijzigingen worden bijgehouden in hrl-bestanden. Replicatielogboeken worden regelmatig geüpload naar een Azure storage-account door de Recovery Services-agent.


## <a name="performance-and-scaling"></a>Prestaties en schalen

Prestaties van de replicatie voor Hyper-V wordt beïnvloed door factoren die in Azure VM-grootte, de veranderingssnelheid van gegevens (verloop) van de virtuele machines, de beschikbare ruimte op de Hyper-V-host voor log file storage, uploaden bandbreedte voor de gegevens van replicatie en doel-opslagaccount.

- Als u meerdere virtuele machines op hetzelfde moment repliceert, gebruikt u de [Azure Site Recovery Deployment Planner](../site-recovery/hyper-v-deployment-planner-overview.md) voor Hyper-V, voor het optimaliseren van replicatie.
- Plan uw Hyper-V-replicatie en replicatie verspreiden over Azure storage-accounts, in overeenstemming met de capaciteit.

### <a name="control-upload-throughput"></a>Besturingselement voor uploaden van doorvoer

De hoeveelheid bandbreedte die wordt gebruikt om gegevens te uploaden naar Azure op elke Hyper-V-host, kunt u beperken. Wees voorzichtig. Als u de waarden te laag ingesteld wordt nadelige invloed hebben op replicatie en vertraagt de migratie.


1. Aanmelden bij de Hyper-V-host of cluster-knooppunt.
2. Voer **C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.msc**, open de Windows Azure Backup MMC-module.
3. Selecteer in de module **eigenschappen wijzigen**.
4. In **beperking**, selecteer **inschakelen voor gebruik van internetbandbreedte voor back-upbewerkingen**. Stel de limieten in voor werktijden en niet-wordt gewerkt. Geldige bereiken liggen tussen 512 Kbps en 1023 Mbps.
I

### <a name="influence-upload-efficiency"></a>Invloed uploaden efficiëntie

Als u ongebruikte bandbreedte voor replicatie hebt en wilt uploads verhogen, kunt u het aantal threads dat is toegewezen voor de taak uploaden als volgt verhogen:

1. Open het register met Regedit.
2. Navigeer naar de sleutel HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM
3. Verhoog de waarde voor het aantal threads die worden gebruikt voor het uploaden van gegevens voor elke replicerende virtuele machine. De standaardwaarde is 4 en de maximumwaarde is 32. 




## <a name="next-steps"></a>Volgende stappen

Probeer [Hyper-V migration](tutorial-migrate-hyper-v.md) met behulp van Azure Migrate-servermigratie.
