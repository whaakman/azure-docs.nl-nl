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
ms.date: 12/13/2017
ms.author: jroth
ms.openlocfilehash: 8b556b01aa47aeb3588138dfa61e517c00dc44dc
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="frequently-asked-questions-for-sql-server-on-linux-azure-virtual-machines"></a>Veelgestelde vragen over SQL Server op Linux Azure Virtual Machines

> [!div class="op_single_selector"]
> * [Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](sql-server-linux-faq.md)

In dit artikel vindt u antwoorden op enkele veelgestelde vragen over het uitvoeren [SQL Server op Linux Azure Virtual Machines](sql-server-linux-virtual-machines-overview.md).

> [!NOTE]
> Dit artikel is gericht op problemen die specifiek zijn voor SQL Server op virtuele Linux-machines. Als u SQL Server op VM's van Windows uitvoert, ziet de [Veelgestelde vragen over Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a id="images"></a>Installatiekopieën

1. **Welke SQL Server-virtuele machine afbeeldingen zijn beschikbaar?**

   Azure onderhoudt installatiekopieën van virtuele machines voor alle ondersteunde belangrijke releases van SQL Server op alle edities voor Linux- en Windows. Zie voor meer informatie, de volledige lijst met [Linux VM-installatiekopieën](sql-server-linux-virtual-machines-overview.md#create) en [Windows VM-installatiekopieën](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md#payasyougo).

1. **Worden de bestaande SQL Server-virtuele machine afbeeldingen bijgewerkt?**

   Elke twee maanden SQL Server-installatiekopieën in de galerie met virtuele machine worden bijgewerkt met de meest recente Linux en Windows-updates. Dit omvat de meest recente updates voor Linux-installatiekopieën. Dit omvat alle updates die zijn gemarkeerd als belangrijk in Windows Update, met inbegrip van belangrijke updates voor SQL Server-beveiliging en servicepacks voor Windows-installatiekopieën. Cumulatieve updates voor SQL Server zijn voor Linux en Windows verwerkt. Voor Linux, cumulatieve updates voor SQL Server ook opgenomen in het vernieuwen. Maar Windows VM's worden op dit moment niet bijgewerkt met SQL Server of Windows Server cumulatieve updates.

1. **Wat betrekking pakketten van de SQL Server zijn geïnstalleerd?**

   Zie voor de SQL Server-pakketten die worden standaard geïnstalleerd op SQL Server-VM's Linux [geïnstalleerde pakketten](sql-server-linux-virtual-machines-overview.md#packages).

1. **Kunnen SQL Server-installatiekopieën voor virtuele machine ophalen verwijderd uit de galerie?**

   Ja. Azure onderhoudt slechts één afbeelding per primaire versie en editie. Bijvoorbeeld wanneer een nieuw servicepack van SQL Server wordt uitgebracht, voegt Azure een nieuwe installatiekopie naar de galerie voor die servicepack. De SQL Server-afbeelding voor de voorafgaande servicepack wordt onmiddellijk verwijderd uit de Azure-portal. Het is echter nog steeds beschikbaar voor het inrichten van PowerShell voor de volgende drie maanden. Een installatiekopie met de vorige versie van service pack is niet meer beschikbaar na drie maanden. Dit beleid verwijderd is ook van toepassing als een SQL Server-versie niet ondersteund wordt wanneer het einde van de levenscyclus is bereikt.

## <a name="creation"></a>Het maken van

1. **Hoe maak ik een virtuele machine Linux Azure met SQL Server**

   De gemakkelijke oplossing is het maken van een virtuele Linux-Machine met SQL Server. Zie voor een zelfstudie over het aanmelden voor Azure en een SQL-VM maken vanuit de portal, [een SQL-Server voor Linux-machine inrichten in de Azure-portal](provision-sql-server-linux-virtual-machine.md). U hebt ook de optie van het handmatig installeren van SQL Server op een virtuele machine met ofwel een vrijelijk gelicentieerde versie (Developer of Express) of door een lokale licentie opnieuw te gebruiken. Als u uw eigen licentie zetten, hebt u [License Mobility through Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility).

1. **Waarom niet kan ik inrichten met een RHEL of SLES SQL Server-VM met een Azure-abonnement een bestedingslimiet heeft?**

   De virtuele machines RHEL en SLES vereisen een abonnement met geen uitgavenlimiet en een geverifieerde betalingsmethode (meestal een creditcard) dat is gekoppeld aan het abonnement. Als u een RHEL of SLES VM zonder te verwijderen van de bestedingslimiet inricht, uw abonnement wordt uitgeschakeld en alle virtuele machines/services is gestopt. Als u uitvoeren in deze toestand is, het abonnement opnieuw inschakelen [de bestedingslimiet verwijderen](https://account.windowsazure.com/subscriptions). Uw resterende tegoed voor de huidige factureringscyclus worden hersteld, maar een aanvulling RHEL of SLES VM-installatiekopie op basis van uw creditcard gaan als u wilt opnieuw te starten en doorgaan met het.

## <a name="licensing"></a>Licentieverlening

1. **Hoe kan ik mijn gelicentieerd exemplaar van SQL Server installeren op een Azure VM?**

   Maak eerst een virtuele Linux OS alleen-lezen-machine. Voer vervolgens de [SQL Server-installatiestappen](https://docs.microsoft.com/sql/linux/sql-server-linux-setup#platforms) voor uw Linux-distributie. Tenzij u een van de vrijelijk gelicentieerde versies van SQL Server installeert, moet u ook een SQL Server-licentie hebben en [License Mobility through Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. **Zijn er installatiekopieën van virtuele machines van Linux Bring-Your-eigenaar-License (BYOL) voor SQL Server?**

   Op dit moment is zijn er geen afbeeldingen van de virtuele machine BYOL Linux voor SQL Server. U kunt echter handmatig SQL Server installeren op een virtuele machine alleen Linux zoals beschreven in de vorige vragen.

1. **Kan ik een virtuele machine voor het gebruik van mijn eigen SQL Server-licentie als deze is gemaakt van een van de installatiekopieën betalen naar gebruik galerie wijzigen?**

   Nee. U activeren vanuit het betalen per minuut licentieverlening voor het gebruik van uw eigen licentie niet. U moet maken van een nieuwe Linux VM, SQL Server installeren en uw gegevens migreren. Zie de vorige vragen voor meer informatie over uw eigen licentie te brengen.

## <a name="administration"></a>Beheer

1. **Kan ik een SQL-Server voor Linux virtuele machine met SQL Server Management Studio (SSMS) beheren?**

   Ja, maar SSMS is momenteel een alleen-Windows-hulpprogramma. U moet extern verbinding maakt vanaf een Windows-machine voor het gebruik van SSMS met VM's Linux SQL Server. Lokaal op Linux, de nieuwe [mssql-conf](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf) hulpprogramma veel administratieve taken kunt uitvoeren. Zie het volgende voorbeeld van een hulpprogramma voor het beheer van meerdere platforms database door [SQL Server Operations Studio (preview)](https://docs.microsoft.com/sql/sql-operations-studio/what-is).

## <a name="updating-and-patching"></a>Bijwerken en patchen

1. **Hoe voer ik een upgrade naar een nieuwe versie /-editie van SQL Server in een Azure VM?**

   Er is momenteel geen in-place upgrade voor SQL Server wordt uitgevoerd in een Azure VM. Een nieuwe virtuele machine van Azure maken met de gewenste SQL Server-versie /-editie, en vervolgens migreren uw databases naar de nieuwe server met [standaardgegevens migratie technieken](https://docs.microsoft.com/sql/linux/sql-server-linux-migrate-overview).

## <a name="general"></a>Algemeen

1. **Worden oplossingen voor hoge beschikbaarheid van SQL Server op Azure Virtual machines ondersteund?**

   Momenteel niet. Altijd op beschikbaarheidsgroepen en Failover Clustering moet een oplossing voor clustering in Linux, zoals pacemaker heeft. De ondersteunde Linux-distributies voor SQL Server bieden geen ondersteuning voor hun invoegtoepassingen hoge beschikbaarheid in de Cloud.

## <a name="resources"></a>Resources

**Virtuele Linux-machines**:

* [Overzicht van SQL Server op een virtuele Linux-machine](sql-server-linux-virtual-machines-overview.md)
* [Een SQL Server virtuele Linux-machine inrichten](provision-sql-server-linux-virtual-machine.md)
* [SQL Server op Linux-documentatie](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)

**Windows-VM's**:

* [Overzicht van SQL Server op een Windows VM](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [Inrichten van een SQL Server Windows VM](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md)
* [Veelgestelde vragen (Windows)](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)