---
title: Azure Migrate replicatie toestel-architectuur | Microsoft Docs
description: Biedt een overzicht van het apparaat Azure Migrate-replicatie
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/04/2019
ms.author: raynew
ms.openlocfilehash: 4f4dc307bee4190a0e94ace493053e0cfd01150e
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811438"
---
# <a name="replication-appliance"></a>Replicatie-apparaat

Dit artikel wordt de replicatie-apparaat gebruikt door Azure Migrate beschreven: Evaluatie van de server wanneer de migratie van VMware VM's, fysieke machines en persoonlijke/openbare cloud-machines naar Azure, met behulp van een migratie op basis van een agent. 

Het hulpprogramma is beschikbaar in de [Azure Migrate](migrate-overview.md) hub. De hub biedt systeemeigen hulpprogramma's voor evaluatie en migratie, evenals hulpprogramma's van andere Azure-services en van derden independent software vendors (ISV).


## <a name="appliance-overview"></a>Overzicht van apparaat

De replicatie-apparaat wordt geïmplementeerd als een enkele on-premises machine, als een VMware-VM of een fysieke server. Deze wordt uitgevoerd:
- **Replicatie toestel**: De replicatie-toepassing coördineert de communicatie en beheert de gegevensreplicatie, voor on-premises VMware-machines en fysieke servers repliceren naar Azure.
- **Processerver**: De processerver, die standaard op het apparaat van de replicatie is geïnstalleerd, en doet het volgende:
    - **Replicatiegateway**: Het fungeert als replicatiegateway. Deze ontvangt replicatiegegevens van computers die zijn ingeschakeld voor replicatie. Het optimaliseert de gegevens van replicatie met caching, compressie en codering, en verzendt ze naar Azure.
    - **Het installatieprogramma van agent**: Voert een push-installatie van de Mobility-Service. Deze service moet worden geïnstalleerd en uitgevoerd op elk on-premises machine die u wilt repliceren voor migratie.

## <a name="appliance-deployment"></a>Implementatie van apparaat

**Implementeren als** | **Gebruikt voor** | **Details**
--- | --- |  ---
Virtuele VMware-machine | Doorgaans gebruikt bij het migreren van virtuele VMware-machines met behulp van het hulpprogramma voor migratie van Azure migreren met de migratie op basis van een agent. | U OVA-sjabloon downloaden van de Azure Migrate-hub en importeren met vCenter-Server te maken van de virtuele machine van het apparaat.
Een fysieke computer | Gebruikt bij het migreren van on-premises fysieke servers als u een VMware-infrastructuur hebt, of als u niet om een VMware-VM met behulp van een OVA-sjabloon te maken. | U een software-installatieprogramma downloaden van de Azure Migrate-hub en de App uitvoeren voor het instellen van de apparaat-machine.

## <a name="appliance-deployment-requirements"></a>Vereisten voor implementatie van apparaat

[Beoordeling](migrate-support-matrix-vmware.md#agent-based-migration-replication-appliance-requirements) de implementatievereisten.



## <a name="appliance-license"></a>Apparaat-licentie
Het apparaat wordt geleverd met een licentie van de evaluatie van Windows Server 2016, die 180 dagen geldig is. Als de evaluatieperiode bijna verlopen is, raden wij u downloaden en implementeren van een nieuw toestel of u de licentie van het besturingssysteem van de virtuele machine van het apparaat te activeren.

## <a name="replication-process"></a>Replicatieproces

1. Wanneer u replicatie voor een virtuele machine inschakelt, begint initiële replicatie naar Azure-opslag, met behulp van het opgegeven replicatiebeleid. 
2. Verkeer repliceert naar Azure storage-openbare eindpunten via internet. Het repliceren van verkeer via een site-naar-site virtueel particulier netwerk (VPN) van een on-premises site naar Azure wordt niet ondersteund.
3. Nadat de initiële replicatie is voltooid, begint de replicatie van verschillen. Bijgehouden wijzigingen voor een virtuele machine worden geregistreerd.
4. Communicatie gebeurt er als volgt uit:
    - Virtuele machines communiceren met het apparaat van de replicatie op poort 443 voor HTTPS inkomende, voor het replicatiebeheer van.
    - De replicatie-apparaat stuurt replicatie met Azure via HTTPS-poort 443 uitgaande.
    - Virtuele machines verzenden replicatiegegevens naar de processerver (die wordt uitgevoerd op het apparaat replicatie) op HTTPS-poort 9443 inkomende. Deze poort kan worden gewijzigd.
    - De processerver ontvangt replicatiegegevens, optimaliseert en versleutelt deze en verzendt ze naar Azure storage via poort 443 uitgaande.
5. De replicatiegegevens registreert eerste land in een cache-opslagaccount in Azure. Deze logboeken worden verwerkt en de gegevens worden opgeslagen in een door Azure beheerde schijf.

![Architectuur](./media/migrate-replication-appliance/architecture.png)

## <a name="appliance-upgrades"></a>Upgrades van apparaat

Het apparaat is handmatig bijgewerkt vanuit de Azure Migrate-hub. Het is raadzaam dat u altijd de nieuwste versie uitvoeren.

1. In Azure Migrate > Servers > Azure Migrate: Evaluatie van de server, infrastructuurservers, klikt u op **configuratieservers**.
2. In **configuratieservers**, een koppeling wordt weergegeven in **Agentversie** als een nieuwe versie van het toestel replicatie beschikbaar is. 
3. Download het installatieprogramma voor de replicatie-cloudapparaat-machine en de upgrade te installeren. Het installatieprogramma detecteert de versie huidige die wordt uitgevoerd op het apparaat.
 
## <a name="next-steps"></a>Volgende stappen

[Informatie over hoe](tutorial-assess-vmware.md#set-up-the-appliance-vm) kunt u het apparaat instellen voor VMware.
[Informatie over hoe](tutorial-assess-hyper-v.md#set-up-the-appliance-vm) het instellen van het apparaat voor Hyper-V.

