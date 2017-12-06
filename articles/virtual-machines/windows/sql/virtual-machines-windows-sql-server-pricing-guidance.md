---
title: Beheren van kosten effectief voor SQL Server op virtuele machines in Azure | Microsoft Docs
description: Bevat de aanbevolen procedures voor het kiezen van de juiste SQL Server-virtuele machine model prijzen.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/17/2017
ms.author: jroth
ms.openlocfilehash: fa1611944d266001a54c4d78205c942a5226d97b
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="pricing-guidance-for-sql-server-azure-vms"></a>Prijsinformatie voor Azure VM's van SQL Server

In dit artikel biedt prijsstelling richtlijnen voor SQL Server virtuele machines in Azure. Er zijn verschillende opties die invloed hebben op de kosten en het is belangrijk om op te halen van de installatiekopie van het juiste die een compromis tussen de kosten op zakelijke vereisten.

## <a name="free-licensed-sql-server-editions"></a>Vrije-gelicentieerde SQL Server-edities

Als u wilt ontwikkelen, testen of een POC bouwen, gebruikt u de gratis licentie **ontwikkelaarsversie van SQL Server**. Deze editie biedt alles wat in SQL Server Enterprise edition, dus u kunt deze ongeacht welke toepassing die u wilt maken. Het is niet alleen toegestaan in productie uit te voeren. Een SQL Server Developer VM alleen de kosten voor de kosten van de virtuele machine, niet voor SQL Server-licentieverlening.

Als u wilt uitvoeren van een lichte werkbelasting in productie (< 4 kernen < 1 GB geheugen, < 10 GB/database), gebruikt u de gratis licentie **SQL Server Express edition**. Een VM SQL Express alleen kosten voor de kosten van de virtuele machine geen SQL-licentieverlening.

Voor deze ontwikkeling en testen of lightweight productieworkloads, kunt u ook geld besparen door het kiezen van een kleinere VM-grootte die overeenkomt met deze werkbelastingen. De DS1v2 mogelijk een goede keuze voor deze werkbelastingen.

Als een virtuele machine van SQL Server 2017 Azure maken met een van deze installatiekopieën, Zie de volgende koppelingen:

| Platform | Vrijelijk gelicentieerde installatiekopieën |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016)<br/>[SQL Server-2017 snelle Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74)<br/>[SQL Server-2017 snelle Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2)<br/>[SQL Server-2017 snelle Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS)<br/>[SQL Server-2017 snelle Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS) |

## <a name="paid-sql-server-editions"></a>Betaalde edities van SQL Server

Als u een werkbelasting niet lightweight productie hebt, gebruikt u een van de volgende versies van SQL Server:

| SQL Server-editie | Workload |
|-----|-----|
| Web | Kleine websites |
| Standard | Kleine tot middelgrote werkbelastingen |
| Enterprise | Grote of bedrijfskritieke werkbelastingen|

Hebt u twee opties om te betalen voor SQL Server-licentieverlening voor deze edities: *betaalde per gebruik* of *brengt uw eigen license (BYOL)*.

### <a name="pay-per-usage"></a>Betalen per gebruik

**Betalen per gebruik van de SQL Server-licentie** betekent dat de kosten per minuut van het uitvoeren van de Azure VM de kosten van de SQL Server-licentie bevat. U kunt de prijzen voor de verschillende edities van SQL Server (Web, Standard, Enterprise) zien in de [Azure VM pagina met prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard). De kosten zijn hetzelfde voor alle versies van SQL Server (2012 SP3-2017). Met een SQL Server-licentieverlening in het algemeen, is de licentiekosten per minuut afhankelijk van het aantal kernen VM.

Betalen van de SQL Server wordt-licentieverlening per gebruik aanbevolen voor:

- Tijdelijke of periodieke werkbelastingen. Bijvoorbeeld, een app die moet ondersteuning bieden voor een gebeurtenis voor een aantal maanden elk jaar of business-analyse op maandag.
- Werklasten met onbekende levensduur of schaal. Bijvoorbeeld, een app die mogelijk niet vereist zijn in een paar maanden of waarvoor meer of minder rekencapaciteit, afhankelijk van de aanvraag.

Als een virtuele machine van SQL Server 2017 Azure maken met een van deze installatiekopieën betalen per gebruik, Zie de volgende koppelingen:

| Platform | Gelicentieerde installatiekopieën |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonWindowsServer2016)<br/>[SQL Server 2017 standaard Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonWindowsServer2016)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 standaard Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2)<br/>[SQL Server 2017 standaard Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS)<br/>[SQL Server 2017 standaard Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS) |

> [!IMPORTANT]
> Wanneer u een virtuele machine van SQL Server in de portal maakt de **een grootte kiezen** venster bevat de geschatte kosten. Het is belangrijk te weten dat deze schatting alleen de compute-kosten is voor het uitvoeren van de virtuele machine samen met eventuele Windows licentiekosten voor VM's van Windows. Dit omvat geen aanvullende SQL Server-licentiekosten voor Web, Standard en Enterprise-edities. Deze ook omvat geen licentie extra kosten voor Linux-systemen van derden voor virtuele Linux-machines. Als u de meest nauwkeurige prijscategorie schatting, selecteert u het besturingssysteem en de editie van SQL Server op de pagina met prijzen voor [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) en [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).
>
> ![Blade voor VM-grootte kiezen](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-choose-size-pricing-estimate.png)

### <a name="bring-your-own-license-byol"></a>BYOL (Bring your own license)

**U brengt uw eigen SQL Server-licentie via License Mobility**, ook wel **BYOL**, betekent het gebruik van een bestaande SQL Server-volumelicentie met Software Assurance in een Azure VM. Een virtuele SQL Server-machine met behulp van BYOL alleen de kosten voor de kosten van de virtuele machine wordt uitgevoerd, niet voor SQL Server-licentieverlening, gezien het feit dat u al licenties en Software Assurance hebt aangeschaft via een Volume Licensing-programma.

> [!NOTE]
> De afbeeldingen BYOL zijn alleen beschikbaar voor Windows virtuele machines. U kunt echter handmatig SQL Server installeren op een virtuele machine Linux alleen-lezen. Raadpleeg de richtlijnen in de [Linux SQL VM Veelgestelde vragen over](../../linux/sql/sql-server-linux-faq.md).

U brengt uw eigen SQL wordt licenties via License Mobility aanbevolen voor:

- Continue werkbelastingen. Bijvoorbeeld, een app die moet ondersteuning bieden voor zakelijke activiteiten 24 x 7.
- Werklasten met bekende levensduur en schaal. Bijvoorbeeld, een app die is vereist voor het hele jaar en welke vraag is prognose.

Als u wilt BYOL met een virtuele machine van SQL Server gebruiken, moet u een licentie hebben voor SQL Server Standard of Enterprise en [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx#tab=1), dit is een vereiste optie door enkele [Volume Licensing](https://www.microsoft.com/en-us/download/details.aspx?id=10585) programma's en een optionele aankoop met anderen.  De prijscategorie niveau opgegeven via volumelicentieprogramma's varieert, afhankelijk van het type van de overeenkomst en de hoeveelheid en of het streven naar SQL Server. Maar als een vuistregel brengen van uw eigen licentie voor continue productieworkloads heeft de volgende voordelen:

| BYOL benefit | Beschrijving |
|-----|-----|
| **Kostenbesparingen** | U brengt uw eigen SQL Server-licentie is rendabeler dan betalen per gebruik, als een werkbelasting wordt continu uitgevoerd, SQL Server Standard of Enterprise voor *meer dan 10 maanden*. |
| **Besparingen op lange termijn** | Gemiddeld is *30% goedkoper per jaar* te kopen of vernieuwen van een SQL Server-licentie voor de eerste drie jaar. Bovendien na drie jaar hoeft u niet te vernieuwen van de licentie meer, betaalt u voor Software Assurance. Op dat moment is *200% goedkoper*. |
| **Gratis passieve secundaire replica** | Een ander voordeel van uw eigen licentie meebrengen is de [gratis licentieverlening voor één passieve secundaire replica](https://azure.microsoft.com/pricing/licensing-faq/) per SQL-Server voor maximale beschikbaarheid dient. Deze geknipt in half licentiekosten van een maximaal beschikbare SQL Server-implementatie (bijvoorbeeld altijd op beschikbaarheidsgroepen). De rechten voor het uitvoeren van de passieve secundaire worden geleverd via de failover-Servers Software Assurance benefit. |

Als een virtuele machine van SQL Server 2016 Azure maken met een van deze installatiekopieën bring-your-eigenaar-licentie, Zie de virtuele machines voorafgegaan door '{BYOL}':

- [SQL Server 2016 Enterprise Azure VM](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1EnterpriseWindowsServer2016)
- [SQL Server 2016 standaard Azure VM](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016)

> [!IMPORTANT]
> Laat ons weten binnen tien dagen hoeveel SQL Server-licenties u in Azure. De koppelingen naar de vorige afbeeldingen hebben instructies over hoe u dit doet.

> [!NOTE]
> Het is niet mogelijk om de licentie voor een SQL Server-VM waarbij per minuut wordt betaald te wijzigen in uw eigen licentie. Als u dit wilt doen, moet u een nieuwe BYOL-VM maken en uw databases naar de nieuwe VM migreren. 

## <a name="avoid-unnecessary-costs"></a>Onnodige kosten te voorkomen

Om te voorkomen onnodige kosten, kies de grootte van een optimale virtuele machine en overweeg onregelmatige afsluiten voor niet-doorlopende werkbelastingen.

### <a id="machinesize"></a>Formaat van uw virtuele machine correct

De licentiekosten van SQL Server is direct gerelateerd aan het aantal kernen. Kies een VM-grootte die overeenkomt met de verwachte behoeften voor CPU, geheugen, opslag en i/o-bandbreedte. Zie voor een volledige lijst met opties voor machinegrootte, [Windows VM-grootten](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) en [Linux VM-grootten](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Er zijn nieuwe machine-grootten die goed met bepaalde typen SQL Server-werkbelasting werken. Deze machines formaten hoge serviceniveaus geheugen, opslag en i/o-bandbreedte te behouden, maar ze hebben een lagere gevirtualiseerde core-telling. Neem bijvoorbeeld het volgende voorbeeld:

| VM-grootte | vcpu 's | Geheugen | Maximum aantal schijven | Maximum aantal i/o-doorvoer | SQL-licentie kosten | Totale kosten (Compute + licentieverlening) |
|---|---|---|---|---|---|---|
| **Standard_DS14v2** | 16 | 112 GB | 32 | 51.200 IOPS of 768 MB/s | | |
| **Standard_DS14 4v2** | 4 | 112 GB | 32 | 51.200 IOPS of 768 MB/s | 75% lager | 57% lager |

> [!IMPORTANT]
> Dit is een voorbeeld van een punt in tijd. Voor de meest recente specificaties, raadpleegt u de machine grootten artikelen en de prijzen voor Azure [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) en [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

In het vorige voorbeeld, kunt u zien dat de specificaties voor **Standard_DS14v2** en **Standard_DS14 4v2** identiek zijn met uitzondering van vcpu's. Het achtervoegsel **-4v2** aan het einde van de **Standard_DS14 4v2** machinegrootte geeft het aantal actieve vcpu's. Omdat SQL Server-licentiekosten zijn gekoppeld aan het aantal kernen, vermindert dit de kosten van de virtuele machine in scenario's waar de extra vcpu's niet nodig zijn. Dit is een voorbeeld en er zijn veel grootten van de machine met beperkte vcpu's die worden aangeduid met dit patroon achtervoegsel. Zie voor meer informatie het blogbericht [aankondigen van nieuwe Azure VM-grootten voor meer rendabele databaseactiviteiten](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/).

### <a name="shut-down-your-vm-when-possible"></a>Uw virtuele machine mogelijk afsluiten

Als u van alle werkbelastingen die niet continu uitgevoerd gebruikmaakt, kunt u overwegen de virtuele machine afgesloten tijdens de inactieve perioden. U betaalt alleen voor wat u gebruikt.

Bijvoorbeeld, als u gewoon uit SQL Server op een virtuele machine in Azure probeert, wilt u niet worden kosten in rekening door per ongeluk draait weken. Eén oplossing is met de [automatisch afsluiten functie](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![SQL VM autoshutdown](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-auto-shutdown.png)

Automatisch afsluiten maakt deel uit van een grotere set van soortgelijke functies van [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab).

Voor andere werkstromen, kunt u automatisch afsluiten en opnieuw starten van virtuele Azure-machines met een scripting-oplossing, zoals [Azure Automation](https://azure.microsoft.com/services/automation/).

> [!IMPORTANT]
> Afgesloten en toewijzing van uw virtuele machine is de enige manier om kosten te voorkomen. Gewoon stoppen of met behulp van de opties voor energiebeheer afsluiten van de virtuele machine nog steeds leidt ertoe dat gebruikskosten.

## <a name="next-steps"></a>Volgende stappen

Voor algemene Azure prijzen richtlijnen, Zie [te voorkomen dat onverwachte kosten met Azure-facturering en kostenbeheer](../../../billing/billing-getting-started.md).

Voor de meest recente virtuele Machines prijzen, waaronder SQL Server, Zie de [Azure VM pagina met prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard).

Meer informatie over virtuele machines van SQL Server voor zowel [VM's van SQL Server Windows](virtual-machines-windows-sql-server-iaas-overview.md) en [Linux VM's van SQL Server](../../linux/sql/sql-server-linux-virtual-machines-overview.md).
