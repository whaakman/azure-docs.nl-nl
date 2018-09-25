---
title: SQL Server op Linux Azure Virtual Machines Veelgestelde vragen over | Microsoft Docs
description: In dit artikel vindt u antwoorden op veelgestelde vragen over het uitvoeren van SQL Server op Linux Azure Virtual machines.
services: virtual-machines-linux
documentationcenter: ''
author: rothja
manager: jhubbard
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: troubleshooting
ms.workload: iaas-sql-server
ms.date: 12/13/2017
ms.author: jroth
ms.openlocfilehash: e8297892c533f3b0126f925f81d3e9bc429828ef
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039952"
---
# <a name="frequently-asked-questions-for-sql-server-on-linux-azure-virtual-machines"></a>Veelgestelde vragen over SQL Server op Linux Azure Virtual Machines

> [!div class="op_single_selector"]
> * [Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](sql-server-linux-faq.md)

In dit artikel vindt u antwoorden op enkele veelgestelde vragen over het uitvoeren [SQL Server op Linux Azure Virtual Machines](sql-server-linux-virtual-machines-overview.md).

> [!NOTE]
> In dit artikel is gericht op problemen die specifiek zijn voor SQL Server op virtuele Linux-machines. Als u SQL Server op virtuele machines van Windows uitvoert, ziet de [Veelgestelde vragen over Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a id="images"></a> Installatiekopieën

1. **Welke galerie met installatiekopieën van SQL Server virtuele machines zijn beschikbaar?**

   Azure heeft installatiekopieën van virtuele machines voor alle ondersteunde primaire versies van SQL Server op alle edities voor zowel Windows als Linux. Zie voor meer informatie, de volledige lijst van [Linux VM-installatiekopieën](sql-server-linux-virtual-machines-overview.md#create) en [Windows VM-installatiekopieën](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md#payasyougo).

1. **Zijn bestaande galerie met installatiekopieën van de SQL Server-machines bijgewerkt?**

   Elke twee maanden, SQL Server-installatiekopieën in de galerie met virtuele machines worden bijgewerkt met het nieuwste Linux en Windows-updates. Dit omvat de meest recente updates voor Linux-installatiekopieën. Dit omvat alle updates die zijn gemarkeerd als urgent in Windows Update, met inbegrip van belangrijke updates voor SQL Server-beveiliging en servicepacks voor Windows-installatiekopieën. Cumulatieve updates voor SQL Server worden verwerkt voor Linux en Windows. Voor Linux, zijn de cumulatieve updates voor SQL Server ook opgenomen in het vernieuwen. Maar Windows VM's worden op dit moment niet bijgewerkt met cumulatieve updates voor SQL Server of Windows Server.

1. **Wat betrekking SQL Server-pakketten worden ook geïnstalleerd?**

   Zie voor de SQL Server-pakketten die zijn geïnstalleerd op SQL Server Linux-VM's standaard [installatiepakketten](sql-server-linux-virtual-machines-overview.md#packages).

1. **Kunnen SQL Server-installatiekopieën voor virtuele machines verwijderd uit de galerie?**

   Ja. Azure heeft slechts één afbeelding per primaire versie en editie. Bijvoorbeeld, wanneer een nieuw servicepack van SQL Server wordt uitgebracht, voegt Azure een nieuwe installatiekopie naar de galerie voor die servicepack. De SQL Server-installatiekopie voor de voorafgaande servicepack wordt onmiddellijk verwijderd uit de Azure-portal. Het is echter nog steeds beschikbaar voor het inrichten van PowerShell voor de komende drie maanden. De vorige installatiekopie met service pack is niet meer beschikbaar na drie maanden. Dit verwijderingsbeleid is ook van toepassing als een SQL Server-versie niet ondersteund wordt wanneer het einde van de levenscyclus is bereikt.

## <a name="creation"></a>Het maken

1. **Hoe maak ik een Linux Azure-machine met SQL Server?**

   De eenvoudigste oplossing is het maken van een virtuele Linux-Machine met SQL Server. Zie voor een zelfstudie over het aanmelden voor Azure en het maken van een SQL-VM vanuit de portal, [een virtuele SQL Server-machine inrichten in Azure portal](provision-sql-server-linux-virtual-machine.md). U hebt ook de optie van het handmatig installeren van SQL Server op een virtuele machine met ofwel een vrij gelicentieerde versie (ontwikkelaar of Express) of door een on-premises licentie opnieuw te gebruiken. Als u uw eigen licentie meenemen, hebt u [License Mobility through Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility).

1. **Waarom kan niet kan ik een RHEL of SLES SQL Server-VM inrichten met een Azure-abonnement met een bestedingslimiet?**

   Virtuele machines met RHEL- en SLES vereist een abonnement zonder uitgavelimiet en een geverifieerde betalingsmethode (meestal een creditcard) die zijn gekoppeld aan het abonnement. Als u een RHEL of SLES VM inricht zonder de bestedingslimiet verwijderd, wordt uw abonnement wordt uitgeschakeld en alle virtuele machines/services is gestopt. Als u in deze staat uitvoert, het abonnement opnieuw inschakelen [de bestedingslimiet verwijderen](https://account.windowsazure.com/subscriptions). Uw resterende tegoed voor de huidige factureringscyclus wordt hersteld, maar een RHEL of SLES VM-installatiekopie toeslag gaat via uw creditcard betaald als u wilt opnieuw te starten en te blijven.

## <a name="licensing"></a>Licentieverlening

1. **Hoe kan ik mijn gelicentieerd exemplaar van SQL Server installeren op een Azure VM?**

   Maak eerst een Linux OS-alleen virtuele machine. Voer vervolgens de [stappen van de SQL Server-installatie](https://docs.microsoft.com/sql/linux/sql-server-linux-setup#platforms) voor uw Linux-distributie. Tenzij u een van de gratis licentie-edities van SQL Server installeert, moet u ook een SQL Server-licentie hebben en [License Mobility through Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. **Zijn er Bring-Your-Own-License (BYOL) Linux-installatiekopieën voor virtuele machines voor SQL Server?**

   Op dit moment zijn er geen BYOL Linux-installatiekopieën voor virtuele machines voor SQL Server. U kunt echter handmatig SQL Server installeren op een alleen-Linux-VM zoals beschreven in de vorige vragen.

1. **Kan ik een virtuele machine voor het gebruik van mijn eigen SQL Server-licentie, als deze is gemaakt op basis van een van de galerie met betalen per gebruik-installatiekopieën wijzigen?**

   Nee. U kunt niet overstappen van het betalen-per-seconde voor het gebruik van uw eigen licentie-licentieverlening. U moet een nieuwe Linux-VM maken, installatie van SQL Server en Migreer uw gegevens. Zie de vorige vraag voor meer informatie over uw eigen licentie brengen.

## <a name="administration"></a>Beheer

1. **Kan ik een virtuele machine voor Linux SQL Server met SQL Server Management Studio (SSMS) beheren?**

   Ja, maar SSMS is momenteel een alleen-Windows-hulpprogramma. U moet op afstand verbinding maken vanaf een Windows-machine SSMS gebruiken met Linux SQL Server-VM's. Lokaal op Linux, de nieuwe [mssql-conf](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf) hulpprogramma veel beheertaken kan uitvoeren. Zie voor een database van de platformoverschrijdende hulpprogramma, [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is).

1. **Kan ik SQL Server geheel verwijderen uit een SQL-VM?**

   Ja, maar blijft u kosten in rekening gebracht voor uw SQL-VM wordt beschreven in [prijsinformatie voor SQL Server Azure VMs](../../windows/sql/virtual-machines-windows-sql-server-pricing-guidance.md?toc=%2fazure%2fvirtual-machines%2flinux%2fsql%2ftoc.json). Als u SQL Server niet meer nodig hebt, kunt u een nieuwe virtuele machine implementeren en migreren van de gegevens en toepassingen naar de nieuwe virtuele machine. Vervolgens kunt u de SQL Server-machine.

## <a name="updating-and-patching"></a>Updates en patches

1. **Hoe voer ik een upgrade naar een nieuwe versie/editie van SQL Server in een Azure VM?**

   Er is momenteel geen in-place upgrade voor SQL Server in een Azure-VM wordt uitgevoerd. Een nieuwe Azure-machine maken met de gewenste versie/editie van SQL Server en migreer de databases naar de nieuwe server met [standaard gegevensmigratietechnieken](https://docs.microsoft.com/sql/linux/sql-server-linux-migrate-overview).

## <a name="general"></a>Algemeen

1. **Ondersteunt hoge beschikbaarheid van SQL Server-oplossingen op Azure Virtual machines?**

   Momenteel niet. Always On Availability Groups en Failover Clustering beide moet u een oplossing clusters in Linux, zoals Pacemaker nodig hebt. De ondersteunde Linux-distributies voor SQL Server bieden geen ondersteuning voor hun invoegtoepassingen voor hoge beschikbaarheid in de Cloud.

## <a name="resources"></a>Resources

**Virtuele Linux-machines**:

* [Overzicht van SQL Server op een virtuele Linux-machine](sql-server-linux-virtual-machines-overview.md)
* [Een SQL Server Linux-VM inrichten](provision-sql-server-linux-virtual-machine.md)
* [Documentatie voor SQL Server op Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)

**Windows-VM's**:

* [Overzicht van SQL Server op een Windows VM](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [Een SQL Server Windows VM inrichten](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md)
* [Veelgestelde vragen over (Windows)](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)