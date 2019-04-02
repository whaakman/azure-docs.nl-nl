---
title: Migratie van een groot aantal virtuele machines naar Azure automatiseren | Microsoft Docs
description: Hierin wordt beschreven hoe u kunt scripts gebruiken voor het migreren van een groot aantal virtuele machines met Azure Site Recovery
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: article
ms.date: 04/01/2019
ms.author: snehaa
ms.openlocfilehash: f90140e9464ee72e9ceae8ca140bd060c51aade8
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762647"
---
# <a name="scale-migration-of-vms-using-azure-site-recovery"></a>Migratie van de schaal van virtuele machines met Azure Site Recovery

In dit artikel krijgt u inzicht in het proces van het gebruik van scripts voor het migreren van groot aantal virtuele machines met Azure Site Recovery. Deze scripts zijn beschikbaar voor het downloaden op [Azure PowerShell Samples](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/migrate-at-scale-with-site-recovery) op GitHub. De scripts kunnen worden gebruikt voor het migreren van VMware, AWS, GCP virtuele machines en fysieke servers naar Azure en ondersteuning voor migratie naar beheerde schijven. U kunt ook deze scripts gebruiken voor het migreren van Hyper-V-machines als u de virtuele machines als fysieke servers migreren. De scripts gebruikmaken van Azure Site Recovery PowerShell beschreven [hier](https://docs.microsoft.com/azure/site-recovery/vmware-azure-disaster-recovery-powershell).

## <a name="current-limitations"></a>Huidige beperkingen:
- Ondersteuning voor het statische IP-adres op te geven alleen voor de primaire NIC van de doel-VM
- De scripts worden pas van kracht Azure Hybrid Benefit gerelateerde invoer, moet u de eigenschappen van de gerepliceerde virtuele machine in de portal handmatig bijwerken

## <a name="how-does-it-work"></a>Hoe werkt het?

### <a name="prerequisites"></a>Vereisten
Voordat u begint, moet u de volgende stappen uit:
- Zorg ervoor dat de Site Recovery-kluis is gemaakt in uw Azure-abonnement
- Zorg ervoor dat de configuratieserver en processerver worden geïnstalleerd in de bronomgeving en de kluis kan voor het detecteren van de omgeving
- Zorg ervoor dat een replicatiebeleid is gemaakt en die zijn gekoppeld aan de configuratieserver
- Zorg ervoor dat u de VM-beheerdersaccount hebt toegevoegd aan de configuratieserver (die wordt gebruikt voor het repliceren van de on-premises VM's)
- Zorg ervoor dat de doel-artefacten in Azure worden gemaakt
    - Doelresourcegroep
    - Doelopslagaccount (en de resourcegroep) - maken een premium storage-account als u van plan bent om te migreren naar premium-beheerde schijven
    - Cache-Opslagaccount (en de resourcegroep) - maken een standard storage-account in dezelfde regio als de kluis
    - Virtueelnetwerk voor failover (en de resourcegroep)
    - Target Subnet
    - Virtueelnetwerk voor Test-failover (en de resourcegroep)
    - Beschikbaarheidsset (indien nodig)
    - Doel-Netwerkbeveiligingsgroep en de resourcegroep
- Zorg ervoor dat u hebt besloten in de eigenschappen van de doel-VM
    - Naam van de doel-VM
    - Doel-VM-grootte in Azure (kan worden gemaakt met behulp van Azure Migrate-evaluatie)
    - Privé IP-adres van de primaire NIC in de virtuele machine
- De scripts downloaden [Azure PowerShell Samples](https://github.com/Azure/azure-docs-powershell-samples/tree/master/azure-migrate/migrate-at-scale-with-site-recovery) op GitHub

### <a name="csv-input-file"></a>Invoer van de CSV-bestand
Zodra u alle vereisten is voltooid hebt, moet u maken van een CSV-bestand met gegevens voor elke bron-VM die u wilt migreren. De invoer CSV beschikken over een headerregel met de details van de invoer en een rij met details voor elke machine die moet worden gemigreerd. Alle scripts zijn ontworpen om te werken op hetzelfde CSV-bestand. Een CSV-voorbeeldsjabloon is beschikbaar in de scriptmap ter referentie.

### <a name="script-execution"></a>Uitvoering van script
Zodra de CSV gereed is, kunt u de volgende stappen uit om uit te voeren van de migratie van de on-premises VM's kunt uitvoeren:

**Stap #** | **Scriptnaam** | **Beschrijving**
--- | --- | ---
1 | asr_startmigration.ps1 | Replicatie inschakelen voor alle virtuele machines die worden vermeld in de csv, het script maakt u een CSV-uitvoer met de taakdetails voor elke virtuele machine
2 | asr_replicationstatus.ps1 | Controleer de status van replicatie, het script maakt u een csv met de status voor elke virtuele machine
3 | asr_updateproperties.ps1 | Zodra de virtuele machines gerepliceerd/beveiligde zijn, dit script gebruiken voor het bijwerken van de eigenschappen van het doel van de virtuele machine (eigenschappen van berekening en netwerk)
4 | asr_propertiescheck.ps1 | Controleer of als de eigenschappen op de juiste wijze zijn bijgewerkt
5 | asr_testmigration.ps1 |  Start de testfailover van de virtuele machines die worden vermeld in de csv, het script maakt u een CSV-uitvoer met de taakdetails voor elke virtuele machine
6 | asr_cleanuptestmigration.ps1 | Wanneer u handmatig controleren of de virtuele machines die zijn failover testen, kunt u dit script gebruiken voor het opschonen van de testfailover-VM 's
7 | asr_migration.ps1 | Een niet-geplande failover uitvoeren voor de virtuele machines die worden vermeld in de csv, het script maakt u een CSV-uitvoer met de taakdetails voor elke virtuele machine. Het script wordt niet afgesloten de on-premises VM's voordat u activeert de failover, voor consistentie van toepassingen, is het raadzaam dat u handmatig de VM's af voordat het script wordt uitgevoerd.
8 | asr_completemigration.ps1 | Voer de doorvoerbewerking is op de virtuele machines en de Azure Site Recovery-entiteiten verwijderen
9 | asr_postmigration.ps1 | Als u van plan bent om toe te wijzen netwerkbeveiligingsgroepen op de NIC's nadat failover is uitgevoerd, kunt u dit script gebruiken om dat te doen. Een NSG wilt toewijzen aan een één-NIC's in de doel-VM.

## <a name="how-to-migrate-to-managed-disks"></a>Hoe kan ik migreren naar managed disks?
Het script, migreert standaard, u de VM's naar beheerde schijven in Azure. Als de doel-opslagaccount dat is opgegeven, een premium storage-account is, premium-beheerde schijven gemaakt na migratie. Het cacheopslagaccount is nog steeds een standard-account. Als het doelopslagaccount een standaardopslagaccount is, standaardschijven gemaakt nadat de migratie. 

## <a name="next-steps"></a>Volgende stappen

[Meer informatie](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure) over servers migreren naar Azure met Azure Site Recovery
