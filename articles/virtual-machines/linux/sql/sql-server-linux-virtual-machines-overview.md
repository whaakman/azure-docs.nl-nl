---
title: Overzicht van SQL Server op virtuele Linux-machines in Azure | Microsoft Docs
description: "Meer informatie over het uitvoeren van volledige SQL Server-edities op virtuele Linux-machines in Azure. U vindt hier rechtstreekse koppelingen naar alle installatiekopieën voor virtuele Linux-machines met SQL Server en gerelateerde inhoud."
services: virtual-machines-linux
documentationcenter: 
author: rothja
manager: jhubbard
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: get-started-article
ms.workload: iaas-sql-server
ms.date: 10/02/2017
ms.author: jroth
ms.openlocfilehash: f044fcba92eb7aa7f4ac44608571fab3db0e03f5
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="overview-of-sql-server-on-azure-virtual-machines-linux"></a>Overzicht van SQL Server op virtuele machines in Azure (Linux)

> [!div class="op_single_selector"]
> * [Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)
> * [Linux](sql-server-linux-virtual-machines-overview.md)

In dit onderwerp wordt beschreven hoe u SQL Server kunt uitvoeren op virtuele Linux-machines (VM's) in Azure. Ook vindt u hier [koppelingen naar portalinstallatiekopieën](#option-1-create-a-sql-vm-with-per-minute-licensing).

> [!NOTE]
> Zie [Een virtuele Linux-machine met SQL Server inrichten in Azure Portal](provision-sql-server-linux-virtual-machine.md) als u al bekend bent met SQL Server en alleen wilt zien hoe u een virtuele Linux-machine met SQL Server implementeert. Of zie [Provision a Windows SQL Server VM in Azure](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md) (Een virtuele Windows SQL Server-machine in Azure inrichten) als u een virtuele Windows-machine met SQL Server wilt maken.

Als u een databasebeheerder of een ontwikkelaar bent, kunt u met VM's in Azure de on-premises SQL Server-werkbelastingen en toepassingen naar de cloud verplaatsen.

## <a name="scenarios"></a>Scenario's

U kunt er om vele redenen voor kiezen om uw gegevens in Azure te hosten. Als u uw toepassing ontwikkelt voor Azure of als u naar Azure migreert, kunnen ook back-endgegevens in Azure beter worden gevonden. U hebt automatisch toegang tot meerdere datacenters voor een wereldwijde aanwezigheid en herstel na een noodgeval. De gegevens zijn ook zeer goed beveiligd en duurzaam.

SQL Server dat wordt uitgevoerd op virtuele machines in Azure is één mogelijkheid om relationele gegevens in Azure op te slaan. U hebt echter ook de mogelijkheid om de Azure SQL Database-service te gebruiken. Zie [Een SQL Server-cloudoptie kiezen: Azure SQL (PaaS) Database of SQL Server op Azure Virtual Machines (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md) voor meer informatie over een keuze voor SQL Server op virtuele machines of voor Azure SQL Database.

## <a id="create"></a> Een nieuwe SQL-VM maken

Stapsgewijze richtlijnen voor het maken van een nieuwe virtuele SQL-machine vindt u in de zelfstudie [Een virtuele SQL Server-machine inrichten in Azure](provision-sql-server-linux-virtual-machine.md).

De volgende tabel bevat een matrix met de laatste SQL Server-installatiekopieën in de galerie met virtuele machines. Klik op een koppeling om te beginnen met het maken van een nieuwe virtuele SQL-machine met de versie, de editie en het besturingssysteem die u hebt opgegeven.

> [!TIP]
> Zie de [pagina met prijzen voor virtuele SQL Server-machines voor Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) voor informatie over de VM- en SQL-prijzen voor deze installatiekopieën.

| Versie | Besturingssysteem | Editie |
| --- | --- | --- |
| **SQL Server 2017** | Red Hat Enterprise Linux (RHEL) 7.4 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74) |
| **SQL Server 2017** | SUSE Linux Enterprise Server (SLES) v12 SP2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2) |
| **SQL Server 2017** | Ubuntu 16.04 LTS |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS), [Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS) |

> [!NOTE]
> Zie [Overzicht van SQL Server op virtuele Azure-machines (Windows)](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) voor de beschikbare installatiekopieën van SQL Server voor virtuele Windows-machines.

## <a id="packages"></a> Geïnstalleerde pakketten

Wanneer u SQL Server op Linux configureert, installeert u het database-engine-pakket en vervolgens verschillende optionele pakketten afhankelijk van uw vereisten. De installatiekopieën van SQL Server voor de virtuele Linux-machines installeren de meeste pakketten automatisch voor u. De volgende tabel laat zien welke pakketten zijn geïnstalleerd voor elke distributie.

| Distributie | [Database Engine](https://docs.microsoft.com/sql/linux/sql-server-linux-setup) | [Hulpprogramma's](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools) | [SQL Server Agent](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-sql-agent) | [Zoekopdracht in volledige tekst](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-full-text-search) | [SSIS](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-ssis) | [HA-invoegtoepassing](https://docs.microsoft.com/sql/linux/sql-server-linux-business-continuity-dr) |
|---|---|---|---|---|---|---|
| RHEL | ![ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![nee](./media/sql-server-linux-virtual-machines-overview/no.png) |
| SLES | ![ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![nee](./media/sql-server-linux-virtual-machines-overview/no.png) | ![nee](./media/sql-server-linux-virtual-machines-overview/no.png) |
| Ubuntu | ![ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![ja](./media/sql-server-linux-virtual-machines-overview/yes.png) | ![ja](./media/sql-server-linux-virtual-machines-overview/yes.png) |

## <a name="next-steps"></a>Volgende stappen

Zie [Overview of SQL Server on Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview) (Overzicht van SQL Server op Linux) voor meer informatie over het configureren en gebruiken van SQL Server op Linux.
