---
title: Ondersteuning voor SQL Server 2008 en SQL Server 2008 R2 met Azure uitbreiden
description: Informatie over het uitbreiden van ondersteuning voor SQL Server 2008 en SQL Server 2008 R2 door uw SQL Server-exemplaar naar Azure migreren of het aanschaffen van uitgebreide ondersteuning voor instanties on-premises houden.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/08/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 398b7a78c9bc13631e15c8725efb8cc0fba3f127
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65806474"
---
# <a name="extend-support-for-sql-server-2008-and-sql-server-2008-r2-with-azure"></a>Ondersteuning voor SQL Server 2008 en SQL Server 2008 R2 met Azure uitbreiden

SQL Server 2008 en SQL Server 2008 R2 zijn beide nadert de [einde van de levenscyclus voor ondersteuning (EOS)](https://www.microsoft.com/sql-server/sql-server-2008). Omdat veel van onze klanten nog steeds van beide versies gebruikmaken, bieden we verschillende opties om te blijven ontvangen van ondersteuning. U kunt uw on-premises SQL Server-exemplaren migreren naar virtuele Azure-machines (VM's), migreren naar Azure SQL Database, of on-premises blijven en uitgebreide beveiligingsupdates kopen.

In tegenstelling tot met een beheerd exemplaar is migreren naar een Azure-VM niet vereist recertifying van uw toepassingen. En in tegenstelling tot bij on-premises blijft ontvangt u gratis uitgebreide beveiligingspatches door te migreren naar een Azure-VM. 

De rest van dit artikel bevat overwegingen voor het migreren van uw SQL Server-exemplaar naar een Azure-VM. 

## <a name="provisioning"></a>Inrichten 

Er is een betalen per gebruik `SQL Server 2008 R2 on Windows Server 2008 R2` afbeelding die beschikbaar zijn op de Azure marketplace. 

Klanten die SQL Server 2008 moet ofwel zelf installeren of upgraden naar SQL Server 2008 R2. Evenzo moet klanten op Windows Server 2008 ofwel hun VM implementeren vanaf een aangepaste VHD of een upgrade naar Windows Server 2008 R2. 

Installatiekopieën die zijn geïmplementeerd via de Marketplace worden geleverd met de SQL IaaS-extensie die vooraf zijn geïnstalleerd. De SQL IaaS-extensie is een vereiste voor flexibele licentiëring en patchen. Klanten die zelf geïnstalleerde VM's implementeren moet handmatig de SQL IaaS-extensie installeren. De SQL IaaS-extensie wordt niet ondersteund op Windows 2008. 

  > [!NOTE]
  > Terwijl de SQL-Server `Create` en `Manage` blades werkt met de SQL Server 2008R2-installatiekopie in Azure portal, de volgende functies zijn _niet ondersteund_: Automatische back-ups, Azure Key Vault-integratie, R Services en configuratie van de opslag.

## <a name="licensing"></a>Licenties
Betalen per gebruik SQL Server 2008R2 implementaties kunnen converteren naar [Azure Hybrid Benefit (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/).

Als u wilt converteren een licentie met Software Assurance (SA) op basis van naar betalen per gebruik, moeten klanten zich inschrijven met de SQL-VM [resourceprovider](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider). Eenmaal hebt geregistreerd bij de resourceprovider van de SQL-VM, is het licentietype SQL wordt niet uitwisselbaar tussen AHB en betalen per gebruik. 

Zelf geïnstalleerde SQL Server 2008 of SQL Server 2008 R2-exemplaren op Azure-VM kunnen registreren bij de SQL-resourceprovider en hun Licentietype converteren naar betalen per gebruik.

## <a name="migration"></a>Migratie
U kunt EOS SQL Server-exemplaren migreren naar een Azure-VM met handmatige back-up/herstel methoden; Dit is de meest voorkomende migratiemethode van on-premises met een Azure-VM.

### <a name="azure-site-recovery"></a>Azure Site Recovery

Voor bulksgewijs migraties, raden wij aan [Azure Site Recovery](/azure/site-recovery/site-recovery-overview) service. Met Azure Site Recovery, kunnen klanten de volledige virtuele machine met inbegrip van SQL Server on-premises met een Azure-VM repliceren.

SQL Server is vereist voor app-consistente momentopnamen van Azure Site Recovery om te waarborgen recovery; en Azure Site Recovery biedt ondersteuning voor app-consistente momentopnamen met minimale interval van 1 uur. De minimale RPO die mogelijk is voor SQL Server met Azure Site Recovery-migraties is 1 uur en de RTO bestaat uit twee uur plus hersteltijd van SQL Server.

### <a name="database-migration-service"></a>Database Migration Service

De [Database Migration Service](/dms/dms-overview) is een optie voor klanten als migreren van on-premises met Azure-VM met SQL Server upgraden naar SQL Server 2012 en hoger.

## <a name="disaster-recovery"></a>Herstel na noodgeval

Noodhersteloplossingen voor EOS SQL Server op virtuele Azure-machine zijn als volgt:

- **SQL Server back-ups**: Back-ups van SQL Server kunnen worden gebruikt om SQL Server in het geval van regionale of zone-fouten te herstellen. Aangezien beheerde back-up wordt niet ondersteund voor EOS SQL Server, moet klanten handmatig uitvoeren van back-ups.
- **Back-upfunctie**: U kunt een replica van de verzending log maken in een andere zone of een Azure-regio met continue herstelbewerkingen te verminderen van de RTO bepaalt. Klanten moet het handmatig configureren van back-upfunctie.
- **Azure Site Recovery**: U kunt uw virtuele machine repliceren tussen zones en -regio's via Azure Site Recovery-replicatie. SQL Server is vereist voor toepassingsconsistente momentopnamen van de App om te waarborgen van herstel in geval van een ramp. Azure Site Recovery biedt ten minste 1 uur RPO en 2 uur + SQL Server-hersteltijd RTO voor EOS SQL Server DR.

## <a name="security-patching"></a>Toepassen van beveiligingspatches
Extended security updates voor SQL Server-VM's worden geleverd via de kanalen van Microsoft Update als de SQL Server-VM is geregistreerd bij de SQL [resourceprovider](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider). Patches kunnen ofwel handmatig of automatisch worden gedownload. 

**Automatisch patchen** is standaard ingeschakeld. Met automatisch patchen kan Azure automatisch een patch uitvoeren voor SQL Server en het besturingssysteem. U kunt een dag van de week, de tijd en de duur voor een onderhoudsvenster opgeven als SQL IaaS-extensie is geïnstalleerd. Azure voert de patch uit tijdens deze onderhoudssessie. Het onderhoudsschema maakt voor de tijd gebruik van de landinstellingen van de virtuele machine.  Zie voor meer informatie [Automatisch patchen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-patching.md).


## <a name="next-steps"></a>Volgende stappen

Uw SQL Server-machine te migreren naar Azure

* [Een SQL Server-database naar een SQL Server in een virtuele machine van Azure migreren](virtual-machines-windows-migrate-sql.md)

Aan de slag met SQL Server op virtuele machines van Azure:

* [Create a SQL Server VM in the Azure portal](quickstart-sql-vm-create-portal.md) (Een SQL Server-VM maken in Azure Portal)

Antwoorden op veelgestelde vragen over SQL-VM's:

* [Veelgestelde vragen over SQL Server op virtuele machines van Azure](virtual-machines-windows-sql-server-iaas-faq.md)
