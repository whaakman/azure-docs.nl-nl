---
title: Beheren van kosten effectief voor SQL Server op Azure virtual machines | Microsoft Docs
description: Bevat de aanbevolen procedures voor het kiezen van de juiste SQL-Server-machine prijsmodel.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/09/2018
ms.author: jroth
ms.openlocfilehash: b19cf9cd4b67479d811a590cb80a618680f3b3d5
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52496482"
---
# <a name="pricing-guidance-for-sql-server-azure-vms"></a>Prijsinformatie voor SQL Server Azure VM 's

Dit artikel bevat richtlijnen voor prijzen voor [virtuele machines met SQL Server](virtual-machines-windows-sql-server-iaas-overview.md) in Azure. Er zijn verschillende opties die invloed hebben op de kosten en het is belangrijk om op te halen van de juiste installatiekopie die kosten met zakelijke vereisten een.

> [!TIP]
> Als u alleen hoeft te weten een geschatte kosten voor een specifieke combinatie van de editie van SQL Server en de grootte van virtuele machine, Zie de pagina met prijzen voor [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows) of [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux). Selecteer uw platform en de SQL Server-editie van de **besturingssysteem/Software** lijst.
>
> ![Gebruikersinterface op de pagina met prijzen van virtuele machine](./media/virtual-machines-windows-sql-server-pricing-guidance/virtual-machines-pricing-ui.png)
>
> Of gebruik de [prijscalculator](https://azure.microsoft.com/pricing/#explore-cost) toevoegen en configureren van een virtuele machine. 

## <a name="free-licensed-sql-server-editions"></a>SQL Server-edities gratis

Als u wilt ontwikkelen, testen of een concepttest te bouwen, gebruikt u de gratis licentie **SQL Server Developer edition**. Deze editie heeft alle functies van SQL Server Enterprise edition, zodat u kunt bouwen en testen van elk type toepassing. U kunt de Developer edition echter niet uitvoeren in de productieomgeving. Een virtuele machine van de SQL Server Developer-editie worden alleen kosten in rekening gebracht voor de kosten van de virtuele machine, omdat er geen gekoppelde SQL Server-licentiekosten.

Als u wilt een lichte werkbelasting in productie uitvoeren (< 4 kernen, < 1 GB geheugen, < 10 GB/database), gebruikt u de gratis licentie **SQL Server Express edition**. Een SQL Server Express-editie van virtuele machine wordt ook alleen leidt tot kosten voor de kosten van de virtuele machine.

Voor deze ontwikkelen/testen en lichte productieworkloads, kunt u ook geld besparen door het kiezen van een kleinere VM-grootte die overeenkomt met deze werkbelastingen. De DS1v2 mogelijk een goede keuze in sommige scenario's.

Zie de volgende koppelingen voor informatie over het maken van een SQL Server 2017 Azure VM met een van deze installatiekopieën:

| Platform | Gratis licentie installatiekopieën |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS) |

## <a name="paid-sql-server-editions"></a>Betaalde SQL Server-edities

Als u een niet-lightweight productieworkload hebt, gebruikt u een van de volgende edities van SQL Server:

| SQL Server-editie | Workload |
|-----|-----|
| Web | Kleine web sites |
| Standard | Kleine tot middelgrote workloads |
| Enterprise | Grote of uw essentiële workloads|

U hebt twee opties om te betalen voor SQL Server-licenties voor deze edities: *betaalt per gebruik* of *uw eigen licentie (BYOL)*.

## <a name="pay-per-usage"></a>Betalen per gebruik

**De SQL Server-licentie per gebruik betalen** betekent dat de kosten per seconde van het uitvoeren van de virtuele machine van Azure de kosten van de SQL Server-licentie bevat. U kunt zien met de prijzen voor de verschillende edities van SQL Server (Web, Standard, Enterprise) in de Azure-VM-pagina voor prijzen [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows) of [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux).

De kosten zijn hetzelfde voor alle versies van SQL Server (2012 SP3-2017). De licentiekosten per seconde, is afhankelijk van het aantal VM-vcpu's.

De SQL Server betalen wordt per gebruik-licentieverlening aanbevolen voor:

- **Tijdelijke of periodieke workloads**. Bijvoorbeeld, een app die moet ondersteuning bieden voor een gebeurtenis voor een aantal maanden elk jaar of het zakelijke analyses op elke maandag.

- **Workloads met een onbekende levensduur of schaal**. Bijvoorbeeld, een app die in een paar maanden kan niet worden vereist, of waarvoor mogelijk meer of minder rekenkracht, afhankelijk van de aanvraag.

Voor het maken van een SQL Server 2017 Azure VM met een van de betalen per gebruik-installatiekopieën, Zie de volgende koppelingen:

| Platform | Installatiekopieën met licentie |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonWindowsServer2016)<br/>[SQL Server 2017 standaard Azure-VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonWindowsServer2016)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 standaard Azure-VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2)<br/>[SQL Server 2017 standaard Azure-VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS)<br/>[SQL Server 2017 standaard Azure-VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS) |

> [!IMPORTANT]
> Wanneer u een SQL Server-machine in de portal, maakt de **Kies een grootte** venster toont een geschatte kosten. Het is belangrijk te weten dat deze schatting alleen de compute-kosten is voor het uitvoeren van de virtuele machine samen met eventuele OS licentiekosten (Windows of Linux-systemen van derden).
>
> ![Blade VM-grootte kiezen](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-choose-size-pricing-estimate.png)
>
>Deze omvatten geen aanvullende SQL Server-licentiekosten voor Web, Standard en Enterprise-edities. Als u de meest nauwkeurige schatting van de prijzen, selecteert u uw besturingssysteem en de editie van SQL Server op de pagina met prijzen voor [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) of [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

> [!NOTE]
> Het is nu mogelijk om te wijzigen van de licentiemodel van betalen per gebruik uw eigen licentie (BYOL) en vervolgens weer. Zie voor meer informatie, [de licentiemodel voor een SQL-VM wijzigen](virtual-machines-windows-sql-ahb.md). 

## <a id="byol"></a> Uw eigen licentie (BYOL)

**Uw eigen SQL Server-licentie brengen License Mobility through**, ook wel **BYOL**houdt met behulp van een bestaande SQL Server-Volume-licentie met Software Assurance in een Azure-VM. Een SQL Server-VM met BYOL alleen kosten in rekening gebracht voor de kosten van het uitvoeren van de virtuele machine, niet voor SQL Server-licentieverlening, gezien het feit dat u al licenties en Software Assurance hebt aangeschaft via een Volume Licensing-programma.

> [!IMPORTANT]
> BYOL-installatiekopieën vereist een Enterprise Agreement met Software Assurance. Ze zijn op dit moment niet beschikbaar als onderdeel van de Azure Cloud Solution Partner (CSP).

> [!NOTE]
> De BYOL-installatiekopieën zijn momenteel alleen beschikbaar voor Windows virtuele machines. U kunt echter handmatig SQL Server installeren op een alleen-Linux-VM. Zie de richtlijnen in de [Veelgestelde vragen over Linux SQL VM](../../linux/sql/sql-server-linux-faq.md).

Om uw eigen SQL wordt-licentieverlening License Mobility through aanbevolen voor:

- **Continue workloads**. Bijvoorbeeld, een app die moet ondersteuning bieden voor zakelijke activiteiten 24 x 7.

- **Workloads met bekende levensduur en schaal**. Bijvoorbeeld, een app die is vereist voor het hele jaar en die aanvraag heeft zijn prognose.

Voor het gebruik van BYOL met een SQL Server-VM, moet u beschikken over een licentie voor SQL Server Standard of Enterprise en [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx#tab=1), dit is een vereiste optie via een volumelicentieprogramma's en een optionele aankoop met anderen. Het niveau van de prijzen geleverd via Volume Licensing-programma's kan variëren, afhankelijk van het type van de overeenkomst en de hoeveelheid en/of toezegging voor SQL Server. Maar als vuistregel, uw eigen licentie bevat voor continue productieworkloads biedt de volgende voordelen:

| BYOL-voordeel | Beschrijving |
|-----|-----|
| **Profiteer van kostenbesparing** | Om uw eigen SQL Server-licentie is voordeliger dan betalen per gebruik als een werkbelasting wordt continu uitgevoerd, SQL Server Standard of Enterprise voor *meer dan 10 maanden*. |
| **Besparingen op lange termijn** | Gemiddeld is *30% lager tarief per jaar* te kopen of vernieuwen van een SQL Server-licentie voor de eerste drie jaar. Na drie jaar overneemt hoeft u verder te meer vernieuwen van de licentie, alleen betalen voor Software Assurance. Op dat moment is *200% goedkoper*. |
| **Gratis passieve secundaire replica** | Een ander voordeel voortzetten van uw eigen licentie is de [gratis licentie voor één passieve secundaire replica](https://azure.microsoft.com/pricing/licensing-faq/) per SQL-Server voor maximale beschikbaarheid dient. Dit wordt in de helft licentiekosten van een maximaal beschikbare SQL Server-implementatie (bijvoorbeeld met behulp van Always On Availability Groups). De rechten voor het uitvoeren van de passieve secundaire worden geleverd via de failover-Servers Software Assurance-benefit. |

Voor het maken van een SQL Server 2017 Azure VM met een van deze installatiekopieën bring-your-own-license, raadpleegt u de virtuele machines voorafgegaan door '{BYOL}':

- [SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017EnterpriseWindowsServer2016)
- [SQL Server 2017 standaard Azure-VM](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017StandardonWindowsServer2016)

> [!IMPORTANT]
> Laat het ons weten binnen 10 dagen hoeveel SQL Server-licenties die u in Azure gebruikt. De koppelingen naar de vorige afbeeldingen hebben instructies over hoe u dit doet.

> [!NOTE]
> Het is nu mogelijk om te wijzigen van de licentiemodel van betalen per gebruik uw eigen licentie (BYOL) en vervolgens weer. Zie voor meer informatie, [de licentiemodel voor een SQL-VM wijzigen](virtual-machines-windows-sql-ahb.md). 



## <a name="reduce-costs"></a>Kosten verlagen

Kies een grootte van de optimale virtuele machine om onnodige kosten voorkomen, en houd rekening met onregelmatige afsluiten voor niet-doorlopende workloads.

### <a id="machinesize"></a> Grootte van uw virtuele machine correct

De licentiekosten van SQL Server is direct gerelateerd aan het aantal vcpu's. Kies een VM-grootte die overeenkomt met de verwachte behoeften voor CPU, geheugen, opslag en i/o-bandbreedte. Zie voor een volledige lijst van opties voor machinegrootte [Windows VM-grootten](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) en [Linux VM-grootten](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Er zijn nieuwe machinegrootten die goed met bepaalde typen SQL Server-workloads werken. Deze grootten machines onderhouden hoge mate van geheugen, opslag- en i/o-bandbreedte, maar ze hebben een lagere gevirtualiseerde kerngeheugens. Bijvoorbeeld, houd rekening met het volgende voorbeeld:

| VM-grootte | vCPU's | Geheugen | Maximum aantal schijven | Max. i/o-doorvoer | SQL-licentiekosten | Totale kosten (Compute + licentieverlening) |
|---|---|---|---|---|---|---|
| **Standard_DS14v2** | 16 | 112 GB | 32 | 51.200 IOP's of 768 MB/s | | |
| **Standard_DS14-4v2** | 4 | 112 GB | 32 | 51.200 IOP's of 768 MB/s | 75% lagere | 57% lagere |

> [!IMPORTANT]
> Dit is een voorbeeld van een point-in-time. Raadpleeg de artikelen van machine-grootten en de Azure-pagina voor prijzen voor de meest recente specificaties, [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) en [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

In het vorige voorbeeld, kunt u zien dat de specificaties voor **Standard_DS14v2** en **Standard_DS14 4v2** identiek zijn met uitzondering van vcpu's. Het achtervoegsel **-4v2** aan het einde van de **Standard_DS14 4v2** machinegrootte geeft het aantal actieve vcpu's. Omdat SQL Server-licentiekosten mee gemoeid zijn gekoppeld aan het aantal vcpu's, vermindert dit de kosten van de virtuele machine in scenario's waarbij de extra Vcpu's niet nodig zijn. Dit is een voorbeeld en er zijn veel machinegrootten met beperkte vcpu's die worden aangeduid met dit patroon achtervoegsel. Zie voor meer informatie het blogbericht [aankondiging van nieuwe Azure-VM-grootten voor meer rendabele database werk](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/).

### <a name="shut-down-your-vm-when-possible"></a>Uw virtuele machine mogelijk afsluiten

Als u van een werklast die niet continu wordt uitgevoerd gebruikmaakt, kunt u overwegen de virtuele machine wordt afgesloten tijdens de inactieve perioden. U betaalt alleen voor wat u gebruikt.

Bijvoorbeeld, als u SQL Server op een Azure-VM gewoon uitprobeert, wilt u niet kosten in rekening gebracht door over te laten per ongeluk het weken uitgevoerd. Eén oplossing is het gebruik van de [functie voor automatisch afsluiten](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![SQL-VM autoshutdown](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-auto-shutdown.png)

Automatisch afsluiten maakt deel uit van een grotere set van vergelijkbare functies geleverd door [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab).

Voor andere werkstromen, kunt u automatisch afsluiten en opnieuw starten van virtuele Azure-machines met een scripting-oplossing, zoals [Azure Automation](https://azure.microsoft.com/services/automation/).

> [!IMPORTANT]
> Afsluiten en de toewijzing van uw virtuele machine ongedaan maken is de enige manier om kosten te voorkomen. Gewoon stoppen of met behulp van de opties voor energiebeheer om de virtuele machine af te sluiten nog steeds leidt tot kosten voor het gebruik.

## <a name="next-steps"></a>Volgende stappen

Voor algemene Azure prijzen voor meer informatie Zie [voorkomen van onverwachte kosten met Azure-facturering en kostenbeheer](../../../billing/billing-getting-started.md). Voor de meest recente virtuele Machines, prijzen, met inbegrip van SQL Server, Zie de pagina voor prijzen van Azure VM Azure [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) en [virtuele Linux-machines](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

Zie voor een overzicht van SQL Server wordt uitgevoerd op Azure Virtual Machines, de volgende artikelen:

- [Overzicht van SQL Server op Windows-VM 's](virtual-machines-windows-sql-server-iaas-overview.md)
- [Overzicht van SQL Server op virtuele Linux-machines](../../linux/sql/sql-server-linux-virtual-machines-overview.md)