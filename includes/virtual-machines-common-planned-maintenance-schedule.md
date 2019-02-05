---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: c7fe0d6f8e03501cca7a8b98f95286b6a21c0476
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55735723"
---
## <a name="multi-and-single-instance-vms"></a>Multi- en virtuele machines van één exemplaar
Veel klanten uitvoeren op Azure aantal kritieke dat ze plannen kunnen wanneer hun VM's ondergaan gepland onderhoud vanwege de downtime--ongeveer 15 minuten, dat wordt uitgevoerd tijdens het onderhoud. Beschikbaarheidssets kunt u helpen bepalen wanneer ingerichte virtuele machines gepland onderhoud wordt.

Er zijn twee mogelijke configuraties voor virtuele machines die worden uitgevoerd op Azure. Virtuele machines zijn geconfigureerd als meerdere exemplaren of één exemplaar. Als VM's in een beschikbaarheidsset, zijn klikt u vervolgens ze geconfigureerd als meerdere exemplaren. Houd er rekening mee, zelfs één VM's kunnen worden geïmplementeerd in een beschikbaarheidsset, zodat ze worden behandeld als meerdere exemplaren. Als VM's zich niet in een beschikbaarheidsset, zijn klikt u vervolgens ze geconfigureerd als één exemplaar.  Zie voor meer informatie over beschikbaarheidssets, [de beschikbaarheid van uw Windows-Machines beheren](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) of [de beschikbaarheid van uw virtuele Linux-Machines beheren](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Updates voor gepland onderhoud aan virtuele machines van één exemplaar en meerdere exemplaren afzonderlijk gebeuren. Opnieuw configureren van uw virtuele machines worden één exemplaar (als ze meerdere exemplaren zijn) of meerdere instanties (als ze één exemplaar zijn), kunt u bepalen wanneer het geplande onderhoud voor het ontvangen van hun VM's. Zie [gepland onderhoud voor virtuele Azure Linux-machines](../articles/virtual-machines/linux/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) of [gepland onderhoud voor virtuele machines van Azure Windows](../articles/virtual-machines/windows/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor meer informatie over gepland onderhoud voor virtuele Azure-machines.

## <a name="for-multi-instance-configuration"></a>Voor configuratie met meerdere exemplaren
U kunt de tijd die is gepland onderhoud gevolgen heeft voor uw virtuele machines die in de configuratie van een Beschikbaarheidsset zijn geïmplementeerd door het verwijderen van deze VM's van beschikbaarheidssets.

1. Een e-mailbericht is verzonden naar u zeven kalenderdagen vóór het geplande onderhoud op uw virtuele machines in een configuratie met meerdere exemplaren. De abonnement-id's en de namen van de betrokken virtuele machines van meerdere exemplaren zijn opgenomen in de hoofdtekst van het e-mailbericht.
2. Tijdens deze zeven dagen, kunt u de tijd dat uw exemplaren worden bijgewerkt door het verwijderen van uw exemplaar van meerdere virtuele machines in deze regio van de beschikbaarheidsset. Deze wijzigingen in de configuratie zorgt ervoor dat een opnieuw opstarten, als de virtuele Machine wordt verplaatst van de ene fysieke host, gericht voor onderhoud, naar een andere fysieke host die niet is bedoeld voor onderhoud.
3. U kunt de virtuele machine verwijderen uit de beschikbaarheidsset in de Azure-portal.

   1. Selecteer de virtuele machine verwijderen uit de Beschikbaarheidsset in de portal.  

   2. Onder **instellingen**, klikt u op **beschikbaarheidsset**.

      ![Selectie van Beschikbaarheidsset](./media/virtual-machines-planned-maintenance-schedule/availabilitysetselection.png)

   3. In de beschikbaarheid in het vervolgkeuzemenu instellen, selecteert u "Geen deel uitmaakt van een beschikbaarheidsset."

      ![Verwijderen uit groep](./media/virtual-machines-planned-maintenance-schedule/availabilitysetwarning.png)

   4. Klik aan de bovenkant **opslaan**. Klik op **Ja** om te bevestigen dat deze actie de virtuele machine start.

   >[!TIP]
   >U kunt de virtuele machine om meerdere exemplaren later opnieuw configureren door een van de vermelde beschikbaarheidssets te selecteren.

4. Virtuele machines die zijn verwijderd uit beschikbaarheidssets worden verplaatst naar één exemplaar hosts en worden niet bijgewerkt tijdens het geplande onderhoud op beschikbaarheid instellen configuraties.
5. Nadat de update van beschikbaarheid van virtuele machines is voltooid (volgens de planning die worden beschreven in het oorspronkelijke e-mailbericht), moet u de virtuele machines toevoegen in hun beschikbaarheidssets. Partnerverband als onderdeel van een beschikbaarheidsset configureert u de virtuele machines opnieuw als meerdere exemplaren en resulteert in een opnieuw opstarten. Normaal gesproken nadat alle updates voor meerdere exemplaren zijn voltooid binnen de gehele Azure-omgeving, volgt één exemplaar onderhoud.

Een virtuele machine verwijderen uit een beschikbaarheidsset kan ook worden bereikt met behulp van Azure PowerShell:

```
Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Remove-AzureAvailabilitySet | Update-AzureVM
```

## <a name="for-single-instance-configuration"></a>Voor configuratie met één exemplaar
U kunt de tijd die is gepland onderhoud gevolgen heeft voor u virtuele machines in een configuratie met één exemplaar door deze VM's toe te voegen in beschikbaarheidssets selecteren.

Stapsgewijs

1. Een e-mailbericht is verzonden naar u zeven kalenderdagen vóór het geplande onderhoud aan virtuele machines in een configuratie met één exemplaar. De abonnement-id's en de namen van de betrokken virtuele machines van één exemplaar zijn opgenomen in de hoofdtekst van het e-mailbericht.
2. Tijdens deze zeven dagen, kunt u de tijd dat uw exemplaar wordt opnieuw opgestart door uw virtuele machines van één exemplaar toe te voegen aan een beschikbaarheidsset in die dezelfde regio. Deze wijzigingen in de configuratie zorgt ervoor dat een opnieuw opstarten, als de virtuele Machine wordt verplaatst van de ene fysieke host, gericht voor onderhoud, naar een andere fysieke host die niet is bedoeld voor onderhoud.
3. Volg de instructies opvolgen voor het toevoegen van bestaande VM's in beschikbaarheidssets met de Azure portal en Azure PowerShell. (Zie het voorbeeld van Azure PowerShell die deze stappen volgt.)
4. Zodra deze virtuele machines zijn geconfigureerd als meerdere exemplaren, worden uitgesloten van het geplande onderhoud aan virtuele machines van één exemplaar.
5. Nadat de update van de virtuele machine één exemplaar is voltooid (op basis van de planning in het oorspronkelijke e-mailbericht), kunt u de virtuele machines terugkeren naar één exemplaar door het verwijderen van de virtuele machines van de beschikbaarheidssets.

Een virtuele machine toe te voegen aan een beschikbaarheidsset ook kan worden bereikt met behulp van Azure PowerShell:

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

<!--Anchors-->



<!--Link references-->
[Virtual Machines Manage Availability]: virtual-machines-windows-tutorial.md
[Understand planned versus unplanned maintenance]: virtual-machines-manage-availability.md#Understand-planned-versus-unplanned-maintenance/
