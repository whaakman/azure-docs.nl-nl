---
title: SQL Server op Linux Azure Virtual Machines Veelgestelde vragen | Microsoft Docs
description: In dit artikel vindt u antwoorden op veelgestelde vragen over het uitvoeren van SQL Server op Azure Virtual machines van Linux.
services: virtual-machines-linux
documentationcenter: 
author: rothja
manager: jhubbard
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: troubleshooting
ms.workload: iaas-sql-server
ms.date: 10/05/2017
ms.author: jroth
ms.openlocfilehash: a001ae116e33e0b7be4431b0bc4c8bb319f4e801
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="frequently-asked-questions-for-sql-server-on-linux-azure-virtual-machines"></a>Veelgestelde vragen over SQL Server op Linux Azure Virtual Machines

> [!div class="op_single_selector"]
> * [Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](sql-server-linux-faq.md)

In dit onderwerp vindt u antwoorden op enkele veelgestelde vragen over het uitvoeren [SQL Server op Linux Azure Virtual Machines](sql-server-linux-virtual-machines-overview.md).

> [!NOTE]
> In dit onderwerp legt de nadruk op specifieke met SQL Server op virtuele Linux-machines. Als u SQL Server op VM's van Windows uitvoert, ziet de [Veelgestelde vragen over Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

1. **Hoe maak ik een virtuele machine Linux Azure met SQL Server**

   De gemakkelijke oplossing is het maken van een virtuele Linux-Machine met SQL Server. Zie voor een zelfstudie over het aanmelden voor Azure en een SQL-VM maken vanuit de portal, [een SQL-Server voor Linux-machine inrichten in de Azure-portal](provision-sql-server-linux-virtual-machine.md). U hebt ook de optie van het handmatig installeren van SQL Server op een virtuele machine met ofwel een vrijelijk gelicentieerde versie (Developer of Express) of door een lokale licentie opnieuw te gebruiken. Als u uw eigen licentie zetten, hebt u [License Mobility through Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility).

1. **Hoe voer ik een upgrade naar een nieuwe versie /-editie van SQL Server in een Azure VM?**

   Er is momenteel geen in-place upgrade voor SQL Server wordt uitgevoerd in een Azure VM. Een nieuwe virtuele machine van Azure maken met de gewenste SQL Server-versie /-editie, en vervolgens migreren uw databases naar de nieuwe server met [standaardgegevens migratie technieken](https://docs.microsoft.com/sql/linux/sql-server-linux-migrate-overview).

1. **Hoe kan ik mijn gelicentieerd exemplaar van SQL Server installeren op een Azure VM?**

   Maak eerst een virtuele Linux OS alleen-lezen-machine. Voer vervolgens de [SQL Server-installatiestappen](https://docs.microsoft.com/sql/linux/sql-server-linux-setup#platforms) voor uw Linux-distributie. Tenzij u een van de vrijelijk gelicentieerde versies van SQL Server installeert, moet u ook een SQL Server-licentie hebben en [License Mobility through Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. **Zijn er installatiekopieën van virtuele machines van Linux Bring-Your-eigenaar-License (BYOL) voor SQL Server?**

   Op dit moment is zijn er geen afbeeldingen van de virtuele machine BYOL Linux voor SQL Server. U kunt echter handmatig SQL Server installeren op een virtuele machine alleen Linux zoals beschreven in de vorige vragen.

1. **Kan ik een virtuele machine voor het gebruik van mijn eigen SQL Server-licentie als deze is gemaakt van een van de installatiekopieën betalen naar gebruik galerie wijzigen?**

   Nee. U activeren vanuit het betalen per minuut licentieverlening voor het gebruik van uw eigen licentie niet. U moet maken van een nieuwe Linux VM, SQL Server installeren en uw gegevens migreren. Zie de vorige vragen voor meer informatie over uw eigen licentie te brengen.

1. **Wat betrekking pakketten van de SQL Server zijn geïnstalleerd?**

   Zie voor de SQL Server-pakketten die worden standaard geïnstalleerd op SQL Server-VM's Linux [geïnstalleerde pakketten](sql-server-linux-virtual-machines-overview.md#packages).

1. **Worden oplossingen voor hoge beschikbaarheid van SQL Server op Azure Virtual machines ondersteund?**

   Momenteel niet. Altijd op beschikbaarheidsgroepen en Failover Clustering moet een oplossing voor clustering in Linux, zoals pacemaker heeft. De ondersteunde Linux-distributies voor SQL Server bieden geen ondersteuning voor hun invoegtoepassingen hoge beschikbaarheid in de Cloud.

1. **Waarom niet kan ik inrichten met een RHEL of SLES SQL Server-VM met een Azure-abonnement een bestedingslimiet heeft?**

   De virtuele machines RHEL en SLES vereisen een abonnement met geen uitgavenlimiet en een geverifieerde betalingsmethode (meestal een creditcard) dat is gekoppeld aan het abonnement. Als u een RHEL of SLES VM zonder te verwijderen van de bestedingslimiet inricht, uw abonnement wordt uitgeschakeld en alle virtuele machines/services is gestopt. Als u uitvoeren in deze toestand is, het abonnement opnieuw inschakelen [de bestedingslimiet verwijderen](https://account.windowsazure.com/subscriptions). Uw resterende tegoed voor de huidige factureringscyclus worden hersteld, maar een aanvulling RHEL of SLES VM-installatiekopie op basis van uw creditcard gaan als u wilt opnieuw te starten en doorgaan met het.

## <a name="resources"></a>Resources

**Virtuele Linux-machines**:

* [Overzicht van SQL Server op een virtuele Linux-machine](sql-server-linux-virtual-machines-overview.md)
* [Een SQL Server virtuele Linux-machine inrichten](provision-sql-server-linux-virtual-machine.md)
* [SQL Server op Linux-documentatie](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)

**Windows-VM's**:

* [Overzicht van SQL Server op een Windows VM](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [Inrichten van een SQL Server Windows VM](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md)
* [Veelgestelde vragen (Windows)](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)