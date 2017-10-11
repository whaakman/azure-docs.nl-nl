---
title: Virtuele machines van AWS migreren naar Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe virtuele machines met in Amazon Web Services (AWS) naar Azure met Azure Site Recovery te migreren.
services: site-recovery
documentationcenter: 
author: bsiva
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: b3c0727a279649f4f7dae30d41027129ce5b04ee
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="migrate-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>Virtuele machines in Amazon Web Services (AWS) migreren naar Azure met Azure Site Recovery

In dit artikel wordt beschreven hoe Windows AWS-exemplaren migreren naar Azure virtuele machines met de [Azure Site Recovery](site-recovery-overview.md) service.

Migratie is in feite een failover van AWS naar Azure. U kunt geen failback machines AWS en er is geen lopende replicatie. In dit artikel beschrijft de stappen voor migratie in de Azure-portal en zijn gebaseerd op de instructies voor het [een fysieke machine repliceren naar Azure](site-recovery-vmware-to-azure.md).

Eventuele opmerkingen of vragen plaatsen aan de onderkant van dit artikel of op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

Site Recovery kan worden gebruikt voor het migreren van EC2 exemplaren met een van de volgende besturingssystemen:

- Windows (alleen 64 bits)
    - Windows Server 2008 R2 SP1 + (Citrix HW-stuurprogramma's of alleen AWS HW-stuurprogramma's. **Exemplaren met RedHat HW-stuurprogramma's worden niet ondersteund**) Windows Server 2012 Windows Server 2012 R2
- Linux (alleen 64 bits)
    - Red Hat Enterprise Linux 6.7 (alleen HVM gevirtualiseerd exemplaren)

## <a name="prerequisites"></a>Vereisten

Dit is wat u nodig hebt voor deze implementatie:

* **Configuratieserver**: een Amazon EC2 VM waarop Windows Server 2012 R2 is geïmplementeerd als de configuratieserver. De andere Azure Site Recovery-onderdelen (processerver en hoofddoelserver) worden standaard geïnstalleerd wanneer u de configuratieserver implementeert. In dit artikel beschrijft de stappen voor migratie in de Azure-portal en zijn gebaseerd op de instructies voor het [meer informatie](site-recovery-components.md)

* **EC2 exemplaren**: de Amazon EC2 virtuele machines exemplaren die u wilt migreren.

## <a name="deployment-steps"></a>Implementatiestappen

1. Maak een Recovery Services-kluis.
2. De beveiligingsgroep van uw exemplaren EC2 moet hebben regels die zijn geconfigureerd, zodat de communicatie tussen de EC2-instantie die u wilt migreren en het exemplaar waarop u van plan bent om de configuratie-Server te implementeren.

3. Op de dezelfde Amazon virtuele Privécloud als uw exemplaren EC2, implementeert u een configuratieserver ASR. Raadpleeg de VMware / fysieke naar Azure-vereisten voor implementatie van de configuratievereisten-server.

    ![DeployCS](./media/site-recovery-migrate-aws-to-azure/migration_pic2.png)

4.  Zodra de configuratieserver is geïmplementeerd in AWS en geregistreerd met de Recovery Services-kluis, ziet u de configuratieserver en de processerver onder Site Recovery-infrastructuur zoals hieronder wordt weergegeven:

    ![CSinVault](./media/site-recovery-migrate-aws-to-azure/migration_pic3.png)

5. Nadat u de configuratieserver (het duurt maximaal 15 minustes te worden weergegeven) hebt geïmplementeerd, moet u controleren of deze kan communiceren met de virtuele machines die u wilt migreren.

6. [Replicatie-instellingen instellen](site-recovery-setup-replication-settings-vmware.md).

7. Replicatie inschakelen: replicatie inschakelen voor de virtuele machines die u wilt migreren. U kunt de EC2-exemplaren die gebruikmaken van de persoonlijke IP-adressen, u via de console EC2 krijgen kunt detecteren.

    ![SelectVM](./media/site-recovery-migrate-aws-to-azure/migration_pic4.png)

8. Zodra de exemplaren EC2 zijn beveiligd en de replicatie naar Azure voltooid is, [een Testfailover uitvoeren](site-recovery-test-failover-to-azure.md) valideren van de prestaties van uw toepassing in Azure.

    ![TFI](./media/site-recovery-migrate-aws-to-azure/migration_pic5.png)

9. Een Failover van AWS naar Azure uitvoert voor elke virtuele machine. U kunt desgewenst een herstelplan maken en uitvoeren van een Failover, om meerdere virtuele machines van AWS migreren naar Azure. [Meer informatie](site-recovery-create-recovery-plans.md) over plannen voor herstel.

10. Voor migratie hoeft u geen failover door te voeren. In plaats daarvan selecteert u de optie volledige migratie voor elke computer die u wilt migreren. De actie uitvoeren van de migratie is van het migratieproces is voltooid, verwijdert u de replicatie voor de machine en Hiermee stopt u Site Recovery facturering voor de machine.

    ![Migreren](./media/site-recovery-migrate-aws-to-azure/migration_pic6.png)

## <a name="next-steps"></a>Volgende stappen

- [Gemigreerde machines voorbereiden op het inschakelen van replicatie](site-recovery-azure-to-azure-after-migration.md) naar een andere regio voor herstel na noodgevallen.
- Beginnen met het beveiligen van uw werkbelastingen door [virtuele machines in Azure te repliceren.](site-recovery-azure-to-azure.md)
