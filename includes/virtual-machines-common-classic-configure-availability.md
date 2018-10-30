---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 32f533d06b7db0284459951e65f9c04fe0bb0285
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227208"
---
Een beschikbaarheidsset helpt houden van uw virtuele machines die beschikbaar zijn tijdens de downtime, zoals tijdens onderhoud. Als u twee of meer op soortgelijke wijze geconfigureerde virtuele machines in een beschikbaarheidsset plaatst, maakt u de redundantie die nodig is voor de beschikbaarheid van de toepassingen of services die uw virtuele machine wordt uitgevoerd. Zie voor meer informatie over hoe dit werkt [de beschikbaarheid van virtuele machines beheren][Manage the availability of virtual machines].

Er is een aanbevolen procedure zowel beschikbaarheidssets en eindpunten van load balancer gebruiken om ervoor te zorgen dat uw toepassing altijd beschikbaar en worden uitgevoerd efficiënt is. Zie voor meer informatie over taakverdeling eindpunten [taakverdeling voor Azure-infrastructuurservices][Load balancing for Azure infrastructure services].

U kunt klassieke virtuele machines toevoegen aan een beschikbaarheidsset met behulp van een van twee opties:

* [Optie 1: Maak een virtuele machine en een beschikbaarheidsset op hetzelfde moment][Option 1: Create a virtual machine and an availability set at the same time]. Vervolgens nieuwe virtuele machines toevoegen aan de set bij het maken van deze virtuele machines.
* [Optie 2: Een bestaande virtuele machine toevoegen aan een beschikbaarheidsset][Option 2: Add an existing virtual machine to an availability set].

> [!NOTE]
> In het klassieke model, moet de virtuele machines die u wilt plaatsen in dezelfde beschikbaarheidsset behoren tot dezelfde cloudservice.
> 
> 

## <a id="createset"> </a>Optie 1: Maak een virtuele machine en een beschikbaarheidsset op hetzelfde moment
U kunt de Azure portal of Azure PowerShell-opdrachten gebruiken om dit te doen.

Het gebruik van Azure portal:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) als u dat nog niet hebt gedaan.
2. Klik op **een resource maken** > **Compute**.
3. Selecteer de Marketplace virtual machine-installatiekopie die u wilt gebruiken. U kunt een Linux- of Windows-machine maken.
4. Voor de geselecteerde virtuele machine, controleert u of dat het implementatiemodel is ingesteld op **klassieke** en klik vervolgens op **maken**
   
    ![Alt-tekst voor afbeelding](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. Voer een naam van virtuele machine, de gebruikersnaam en het wachtwoord (voor Windows-machines) of de openbare SSH-sleutel (voor Linux-machines). 
6. De VM-grootte en klik vervolgens op **Selecteer** om door te gaan.
7. Kies **optionele configuratie > beschikbaarheidsset**, en selecteer de beschikbaarheidsset die u wilt toevoegen van de virtuele machine.
   
    ![Alt-tekst voor afbeelding](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. Controleer de configuratieinstellingen. Wanneer u klaar bent, klikt u op **maken**.
9. Terwijl Azure de virtuele machine maakt, u kunt de voortgang volgen onder **virtuele Machines** in het hubmenu.

Zie voor het gebruik van Azure PowerShell-opdrachten te maken van een virtuele machine van Azure toe te voegen aan een nieuwe of bestaande beschikbaarheidsset, [Azure PowerShell gebruiken om te maken en vooraf configureren van virtuele Windows-machines](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a id="addmachine"> </a>Optie 2: Een bestaande virtuele machine toevoegen aan een beschikbaarheidsset
In de Azure-portal, kunt u bestaande klassieke virtuele machines aan een bestaande beschikbaarheidsset instellen of maak een nieuwe voor ze kunt toevoegen. (Houd er rekening mee dat de virtuele machines in dezelfde beschikbaarheidsset tot dezelfde cloudservice behoren moeten.) De stappen zijn vrijwel hetzelfde. Met Azure PowerShell, kunt u de virtuele machine toevoegen aan een bestaande beschikbaarheidsset.

1. Als u hebt nog niet gedaan, meld u aan de [Azure-portal](https://portal.azure.com).
2. Klik in het menu links op **virtuele Machines (klassiek)**.
   
    ![Alt-tekst voor afbeelding](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. Selecteer in de lijst met virtuele machines, de naam van de virtuele machine die u wilt toevoegen aan de set.
4. Kies **beschikbaarheidsset** van de virtuele machine **instellingen**.
   
    ![Alt-tekst voor afbeelding](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. Selecteer de beschikbaarheidsset die u wilt toevoegen van de virtuele machine. De virtuele machine moet behoren tot dezelfde cloudservice als de beschikbaarheidsset.
   
    ![Alt-tekst voor afbeelding](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. Klik op **Opslaan**.

Azure PowerShell-opdrachten gebruiken door een beheerder op serverniveau Azure PowerShell-sessie openen en voer de volgende opdracht uit. Voor de tijdelijke aanduidingen (zoals &lt;VmCloudServiceName&gt;), vervangt u alles binnen de aanhalingstekens in, met inbegrip van de < en > tekens, met de juiste namen.

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> De virtuele machine mogelijk opnieuw worden gestart voor het voltooien van deze toe te voegen aan de beschikbaarheidsset.
> 
> 

<!-- LINKS -->
[Option 1: Create a virtual machine and an availability set at the same time]: #createset
[Option 2: Add an existing virtual machine to an availability set]: #addmachine

[Load balancing for Azure infrastructure services]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Manage the availability of virtual machines]:../articles/virtual-machines/linux/manage-availability.md

[Create a virtual machine running Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtual Network overview]: ../articles/virtual-network/virtual-networks-overview.md

