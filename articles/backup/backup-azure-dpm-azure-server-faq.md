---
title: Back-Server en DPM Veelgestelde vragen over Azure
description: 'Antwoorden op veelgestelde vragen over: De Azure Backup-Server en DPM.'
services: backup
author: srinathvasireddy
manager: sivan
ms.service: backup
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: srinathv
ms.openlocfilehash: 7a598038ee435b67b9ad8f06bdec2490bc1c53c3
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705104"
---
# <a name="azure-backup-server-and-dpm---faq"></a>Azure Backup-Server en DPM - Veelgestelde vragen
In dit artikel vindt u antwoorden op veelgestelde vragen over de Azure Backup Server en DPM.

### <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Kan ik de Azure Backup-server gebruiken om een BMR-back-up (Bare Metal Recovery) te maken voor een fysieke server? <br/>
Ja.

### <a name="can-i-register-the-server-to-multiple-vaults"></a>Kan ik de server voor meerdere kluizen registreren?
Nee. Een DPM of Azure Backup-server kan voor slechts één kluis worden geregistreerd.


### <a name="can-i-use-dpm-to-back-up-apps-in-azure-stack"></a>Kan ik DPM back-up-apps in Azure Stack gebruiken?
Nee. U kunt Azure Backup gebruiken voor het beveiligen van Azure Stack, Azure Backup biedt geen ondersteuning voor het gebruik van DPM back-up-apps in Azure Stack.

### <a name="if-ive-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-install-system-center-dpm-to-back-up-on-premises-workloads-to-azure"></a>Als ik Azure backup-agent voor het beveiligen van bestanden en mappen hebt geïnstalleerd, kan ik System Center DPM back-up on-premises werkbelastingen naar Azure installeren?
Ja. Maar u moet eerst instellen van DPM en installeer vervolgens de Azure Backup-agent.  Installeren van onderdelen in deze volgorde zorgt ervoor dat de Azure Backup agent met DPM werkt. De agent installeren voordat u DPM installeert, wordt niet aangeraden of ondersteund.

### <a name="why-cant-i-add-an-external-dpm-server-after-installing-ur7-and-latest-azure-backup-agent"></a>Waarom niet kan ik een externe DPM-server toevoegen na de installatie van UR7 en de meest recente Azure backup-agent?
Voor de DPM-servers met gegevensbronnen die worden beveiligd naar de cloud (met behulp van een updatepakket ouder dan Update Rollup 7), moet u ten minste één dag na de installatie van de UR7 en de nieuwste Azure backup-agent te starten wachten **externe DPM toevoegen server**. De periode van één dag is nodig om de metagegevens van de DPM-beveiligingsgroepen te uploaden naar Azure. Metagegevens van beveiliging is de eerste keer via elke nacht een taak geüpload.

## <a name="vmware-and-hyper-v-backup"></a>VMware en Hyper-V-back-up

### <a name="can-i-back-up-vmware-vcenter-servers-to-azure"></a>Kan ik een back-up maken van VMware vCenter-servers naar Azure?
Ja. Azure Backup Server kunt u back-up van VMware vCenter-Server- en ESXi-hosts naar Azure.

- [Meer informatie](backup-mabs-protection-matrix.md) over ondersteunde versies.
- [Volg deze stappen](backup-azure-backup-server-vmware.md) back-up een VMware-server.

### <a name="do-i-need-a-separate-license-to-recover-a-full-on-premises-vmwarehyper-v-cluster"></a>Moet ik een afzonderlijke licentie voor het herstellen van een volledige on-premises VMware/Hyper-V-cluster?
U moet afzonderlijke licenties voor VMware/Hyper-V-beveiliging.

- Als u een System Center-klant bent, gebruikt u System Center Data Protection Manager (DPM) om te beveiligen van virtuele VMware-machines.
- Als u niet een System Center-klant bent, kunt u Azure Backup Server (betalen naar gebruik) om te beveiligen van virtuele VMware-machines.


## <a name="sharepoint"></a>SharePoint

### <a name="can-i-recover-a-sharepoint-item-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson-with-protection-on-disk"></a>Kan ik een SharePoint-item naar de oorspronkelijke locatie herstellen als SharePoint is geconfigureerd met behulp van SQL AlwaysOn (met beveiliging op schijf)?
Ja, het item kan worden hersteld naar de oorspronkelijke SharePoint-site.

### <a name="can-i-recover-a-sharepoint-database-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson"></a>Kan ik een SharePoint-database naar de oorspronkelijke locatie herstellen als SharePoint met behulp van SQL AlwaysOn is geconfigureerd?
Omdat SharePoint-databases zijn geconfigureerd in de SQL AlwaysOn, kan ze niet worden gewijzigd, tenzij de beschikbaarheidsgroep wordt verwijderd. DPM kan niet als gevolg hiervan, een database herstellen naar de oorspronkelijke locatie. U kunt een SQL Server-database naar een ander exemplaar van SQL Server herstellen.

## <a name="next-steps"></a>Volgende stappen

Lees de andere veelgestelde vragen over:

- [Meer informatie](backup-support-matrix-mabs-dpm.md) ondersteuningsmatrix voor informatie over Azure Backup Server en DPM.
- [Meer informatie](backup-azure-mabs-troubleshoot.md) probleemoplossing richtlijnen over de Azure Backup Server en DPM.
