---
title: Migratie op basis van de agent architectuur in Azure Migrate-servermigratie
description: Biedt een overzicht van VMware-VM-migratie op basis van een agent met de migratie van Azure-Server migreren.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: raynew
ms.openlocfilehash: 21c779587842c976ba93d7fa592a91ee714bc55c
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811152"
---
# <a name="agent-based-migration-architecture"></a>Migratie op basis van de agent architectuur

Dit artikel bevat een overzicht van de architectuur en processen die worden gebruikt voor replicatie op basis van een agent met het hulpprogramma voor migratie van Azure-Server migreren.

[Azure Migrate](migrate-services-overview.md) biedt een centraal punt voor het volgen van detectie, beoordeling en migratie van uw on-premises toepassingen, workloads en AWS/GCP VM-instanties naar Azure. De hub biedt hulpprogramma's voor Azure Migrate voor evaluatie en migratie, evenals van derden independent software vendor (ISV)-aanbiedingen.

## <a name="agent-based-replication"></a>Replicatie op basis van een agent

Replicatie op basis van een agent in de Azure Migrate Serverreplicatie hulpprogramma wordt gebruikt voor het migreren van on-premises virtuele VMware-machines en fysieke servers naar Azure. Het kan ook worden gebruikt voor het migreren van andere on-premises gevirtualiseerde servers, evenals persoonlijke en openbare cloud virtuele machines, met inbegrip van AWS-exemplaren en GCP VM's.

Voor VMware-migratie biedt het hulpprogramma voor migratie van Azure Migrate-Server een aantal opties:

- Migratie met behulp van replicatie op basis van een agent, zoals beschreven in dit artikel.
- Zonder agent replicatie, voor het migreren van virtuele machines zonder dat u hoeft te installeren op deze.

Meer informatie over [een migratiemethode selecteren voor VMware](server-migrate-overview.md).

## <a name="server-migration-and-azure-site-recovery"></a>Migratie van servers en Azure Site Recovery

Azure Migrate servermigratie is een hulpprogramma voor migratie on-premises en de openbare cloud-workloads naar Azure. Dit geoptimaliseerd voor migratie. Site Recovery is een hulpprogramma van de herstel na noodgevallen. Azure migratie-Server en Site Recovery deelt sommige algemene technologie-onderdelen die worden gebruikt voor replicatie van gegevens, maar voor verschillende doeleinden.

## <a name="architectural-components"></a>Architectuuronderdelen

![Architectuur](./media/agent-based-replication-architecture/architecture.png)

De tabel bevat een overzicht van de onderdelen die worden gebruikt voor de migratie op basis van een agent.

**Onderdeel** | **Details** | **Installatie**
--- | --- | ---
**Replicatie-apparaat** | De replicatie-apparaat (configuratieserver) is een on-premises computer die als een brug tussen de on-premises omgeving en het hulpprogramma voor migratie van Azure-Server migreren fungeert. Het apparaat detecteert de voorraad van on-premises VM, zodat Azure-servermigratie replicatie en migratie kunt indelen. Het apparaat bestaat uit twee onderdelen:<br/><br/> **Configuratieserver**: Maakt verbinding met Azure Migrate-servermigratie en coördineert de replicatie.<br/> **Processerver**: Handelt de gegevensreplicatie af. Het VM-gegevens ontvangt, worden gecomprimeerd versleutelt deze en verzendt naar de Azure-abonnement. Servermigratie schrijft, de gegevens naar beheerde schijven. | De processerver wordt standaard geïnstalleerd samen met de configuratieserver op het apparaat van de replicatie.
**Mobility-service** | De Mobility-service is een agent is geïnstalleerd op elke machine die u wilt repliceren en migreren. Gegevens van replicatie stuurt van de machine het naar de processerver. Er zijn een aantal verschillende agents van de Mobility-service beschikbaar. | Installatiebestanden voor de Mobility-service zich op het apparaat van de replicatie. U downloadt en installeert de agent die u nodig hebt, in overeenstemming met het besturingssysteem en versie van de computer die u wilt repliceren.

### <a name="mobility-service-installation"></a>Installatie van de Mobility-service

U kunt de Mobility-Service met behulp van de volgende manieren implementeren:

- **Push-installatie**: De Mobility-service wordt geïnstalleerd door de processerver, wanneer u beveiliging voor een machine inschakelt. 
- **Handmatig installeren**: U kunt de Mobility-service handmatig installeren op elke machine via de gebruikersinterface of opdrachtprompt.

De Mobility-service communiceert met de replicatie-apparaat en machines gerepliceerd. Als er antivirussoftware die worden uitgevoerd op het apparaat van de replicatie, processervers of machines die worden gerepliceerd, moeten de volgende mappen worden uitgesloten van scannen:


- C:\Program Files\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Recovery
- C:\Program Files (x86) \Microsoft Azure Site Recovery
- C:\ProgramData\ASR\agent (op Windows-machines met de Mobility-service geïnstalleerd)

## <a name="replication-process"></a>Replicatieproces

1. Wanneer u replicatie voor een virtuele machine inschakelt, begint de initiële replicatie naar Azure.
2. Tijdens de eerste replicatie de Mobility-service leest gegevens uit de machineschijven, en verzendt dit naar de processerver.
3. Deze gegevens worden gebruikt als seeding voor een kopie van de schijf in uw Azure-abonnement. 
4. Nadat de initiële replicatie is voltooid, begint de replicatie van deltaverschillen naar Azure. Replicatie is op blokniveau, en in de buurt van de continue.
4. De Mobility-Service intercepts schrijft in het geheugen van VM-schijf of door te integreren met het opslagsubsysteem van het besturingssysteem. Deze methode voorkomt schijf-i/o-bewerkingen op de replicerende machine voor incrementele replicatie. 
5. Bijgehouden wijzigingen voor een virtuele machine worden verzonden naar de processerver op HTTPS-poort 9443 inkomende. Deze poort kan worden gewijzigd. De processerver worden gecomprimeerd en versleutelt deze en verzendt ze naar Azure. 

## <a name="ports"></a>Poorten

**apparaat** | **verbinding**
--- | --- 
VM's | De Mobility-service die wordt uitgevoerd op virtuele machines communiceert met de on-premises replicatie-apparaat op poort 443 voor HTTPS inkomende, voor het replicatiebeheer van.<br/><br/> Virtuele machines verzenden replicatiegegevens naar de processerver (die wordt uitgevoerd op het apparaat replicatie standaard) op HTTPS-poort 9443 inkomende. Deze poort kan worden gewijzigd.
Replicatie-apparaat | De replicatie-apparaat stuurt replicatie met Azure via HTTPS-poort 443 uitgaande.
Processerver | De processerver ontvangt replicatiegegevens, optimaliseert en versleutelt deze en verzendt ze naar Azure storage via poort 443 uitgaande.


## <a name="performance-and-scaling"></a>Prestaties en schalen

Standaard implementeert u een één replicatie-apparaat dat wordt uitgevoerd zowel de configuratieserver en de processerver. Als u slechts een paar machines repliceert, is deze implementatie is voldoende. Echter, als u wilt repliceren en honderden machines migreren, een enkel proces-server niet mogelijk om af te handelen het replicatieverkeer. In dit geval kunt u aanvullende, scale-out processervers implementeren.

### <a name="site-recovery-deployment-planner-for-vmware"></a>Site Recovery-Implementatieplanner voor VMware

Als u virtuele VMware-machines repliceert, kunt u de [Site Recovery Deployment Planner](../site-recovery/site-recovery-deployment-planner.md) voor VMware om te bepalen van de prestatie-eisen, met inbegrip van de dagelijkse gegevens wijzigen tarief, en de processervers u nodig hebt.

### <a name="replication-appliance-capacity"></a>Replicatie toestel capaciteit

De waarden in deze tabel kunnen worden gebruikt om te achterhalen of u een aanvullende processerver in uw implementatie moet.

- Als uw dagelijkse wijzigingssnelheid (verloop) meer dan 2 TB is, implementeert u een aanvullende processerver.
- Als u meer dan 200 machines repliceert, implementeert u een extra replicatie-apparaat.

**CPU** | **Geheugen** | **Vrije ruimte voor gegevens in cache opslaan** | **Tarief verloop** | **Replicatie-limieten**
--- | --- | --- | --- | ---
8 vcpu's (2-sockets * 4 kernen \@ 2,5 GHz) | 16 GB | 300 GB | 500 GB of minder | < 100 machines 
12 vcpu's (2-sockets * 6 kernen \@ 2,5 GHz) | 18 GB | 600 GB | 501 GB tot 1 TB | 100-150-machines.
16 vcpu's (2-sockets * 8 kernen \@ 2,5 GHz) | 32 G1 |  1 TB | 1 TB tot 2 TB | 151-200-machines.

### <a name="scale-out-process-server-sizing"></a>Schaling van de scale-out-proces

Als u een uitbreidbare processerver implementeren wilt, kan deze tabel u helpen te achterhalen schaling.

**Processerver** | **Vrije ruimte voor gegevens in cache opslaan** | **Tarief verloop** | **Replicatie-limieten**
--- | --- | --- | --- 
4 vcpu's (2-sockets * 2 kernen \@ 2,5 GHz), 8 GB geheugen | 300 GB | 250 GB of minder | Maximaal 85 machines 
8 vcpu's (2-sockets * 4 kernen \@ 2,5 GHz), 12 GB geheugen | 600 GB | 251 GB tot 1 TB    | 86 150-machines.
12 vcpu's (2-sockets * 6 kernen \@ 2,5 GHz), 24 GB geheugen | 1 TB | 1-2 TB | 151 225 machines.

## <a name="control-upload-throughput"></a>Besturingselement voor uploaden van doorvoer

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

Proberen op basis van een agent [VMware VM-migratie](tutorial-migrate-vmware-agent.md) met behulp van Azure Migrate-servermigratie.
