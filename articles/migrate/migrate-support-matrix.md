---
title: Ondersteuningsmatrix voor Azure Migrate
description: Bevat een samenvatting van instellingen voor de ondersteuning en beperkingen voor de service Azure Migrate.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: raynew
ms.openlocfilehash: b2ca1b9118ecc3d112a49bb4c79b413c46fe67cb
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811555"
---
# <a name="azure-migrate-support-matrix"></a>Ondersteuningsmatrix voor Azure Migrate

U kunt de [Azure Migrate-service](migrate-overview.md) om te beoordelen en migreren van machines naar de Microsoft Azure-cloud. In dit artikel bevat een overzicht van instellingen voor de algemene ondersteuning en beperkingen voor Azure Migrate scenario's en implementaties.


## <a name="azure-migrate-versions"></a>Azure Migrate-versies

Er zijn twee versies van de Azure Migrate-service:

- **Huidige versie**: Met deze versie dat kunt u nieuwe Azure Migrate projecten werkt, Ontdek on-premises beoordeelt en maakt u gecoördineerde evaluaties en migraties. [Meer informatie](whats-new.md#azure-migrate-new-version).
- **Vorige versie**: Voor klanten met behulp van de vorige versie van Azure Migrate (alleen voor evaluatie van on-premises VMware-VM's wordt ondersteund), moet u nu de huidige versie gebruiken. U kunt in de vorige versie, nieuwe Azure Migrate-projecten maken of uitvoeren van nieuwe detecties.

## <a name="supported-migration-scenarios"></a>Ondersteunde migratiescenario 's

De tabel bevat een overzicht van ondersteunde migratiescenario's.

**Implementatie** | **Details*** 
--- | --- 
**Evaluatie van de on-premises** | On-premises werkbelastingen en de gegevens die worden uitgevoerd op virtuele machines van VMware en Hyper-V-machines evalueren. Met behulp van Azure Migrate-evaluatie-Server en Microsoft Data Migration Assistant (DMA), evenals hulpprogramma's van derden, zoals Cloudamize, Corent technische en Turbonomic-Server te beoordelen.
**On-premises migratie naar Azure** | Workloads en gegevens die worden uitgevoerd op fysieke servers, VMware-VM's, Hyper-V-machines en AWS/GCP-instanties naar Azure te migreren. Migreren met behulp van Azure Migrate-Server-evaluatie en Azure Database Migration Service (DMS) en als u met behulp van hulpprogramma's van derden die Carbonite en CorentTech bevatten.

Ondersteuning voor specifieke hulpprogramma worden als volgt samengevat.

**Hulpprogramma** | **Evaluatie/migratie** | **Details**
--- | --- | ---
Azure Migrate Server-evaluatie | Beoordeling | Server-evaluatie voor uitproberen [Hyper-V](tutorial-prepare-hyper-v.md) en [VMware](tutorial-prepare-vmware.md).
Cloudamize | Beoordeling | [Meer informatie](https://www.cloudamize.com/platform#tab-0).
CorentTech | Beoordeling | [Meer informatie](https://www.corenttech.com/).
Turbonomic | Beoordeling | [Meer informatie](https://turbonomic.com/solutions/technologies/azure-cloud/).
Azure Migrate-servermigratie | Migratie | Probeer de migratie van de server voor [Hyper-V](tutorial-migrate-hyper-v.md) en [VMware](tutorial-migrate-vmware.md).
Carbonite | Migratie | [Meer informatie](https://www.carbonite.com/data-protection-resources/resource/Datasheet/carbonite-migrate-for-microsoft-azure).
CorentTech | Migratie | [Meer informatie](https://www.corenttech.com/).


## <a name="azure-migrate-projects"></a>Azure Migrate-projecten

**Ondersteuning** | **Details**
--- | ---
Subscription | U kunt een enkel Azure Migrate-project in een abonnement hebben.
Azure-machtigingen | U moet machtigingen voor Inzender of eigenaar in het abonnement voor het maken van een Azure Migrate-project.
Virtuele VMware-machines  | Evalueer tot 35.000 VMware-VM's in een enkel project.
Virtuele Hyper-V-machines | Beoordeling van maximaal 10.000 Hyper-V-machines in een enkel project.

Een project kan bestaan zowel VMware-VM's en Hyper-V-machines, totdat de evaluatie.


## <a name="vmware-assessment-and-migration"></a>VMware-evaluatie en migratie

[Beoordeling](migrate-support-matrix-vmware.md) de Azure Migrate-Server-evaluatie en migratie van Server ondersteuningsmatrix voor VMware-VM's.

## <a name="hyper-v-assessment-and-migration"></a>Hyper-V-evaluatie en migratie

[Beoordeling](migrate-support-matrix-hyper-v.md) de Azure Migrate-Server-evaluatie en migratie van Server ondersteuningsmatrix voor Hyper-V-machines.


## <a name="next-steps"></a>Volgende stappen

- [VMware-VM's beoordelen](tutorial-assess-vmware.md) voor migratie.
- [Hyper-V-machines beoordelen](tutorial-assess-hyper-v.md) voor migratie.

