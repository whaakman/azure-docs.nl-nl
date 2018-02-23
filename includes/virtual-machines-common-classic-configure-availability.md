


Een beschikbaarheidsset helpt uw virtuele machines beschikbaar zijn tijdens de uitvaltijd, zoals behouden tijdens onderhoud. Als u twee of meer op vergelijkbare wijze geconfigureerde virtuele machines in een beschikbaarheidsset maakt de redundantie die nodig zijn voor het onderhouden van de beschikbaarheid van de toepassingen of services die de virtuele machine wordt uitgevoerd. Zie voor meer informatie over hoe dit werkt [de beschikbaarheid van virtuele machines beheren][Manage the availability of virtual machines].

Het is beste beschikbaarheidssets en eindpunten van taakverdeling gebruiken om ervoor te zorgen dat uw toepassing altijd beschikbaar en worden uitgevoerd efficiënt is. Zie voor meer informatie over Netwerktaakverdeling eindpunten [taakverdeling voor Azure-infrastructuurservices][Load balancing for Azure infrastructure services].

U kunt de klassieke virtuele machines toevoegen in een beschikbaarheidsset met behulp van een van twee opties:

* [Optie 1: Maak een virtuele machine en een beschikbaarheidsset op hetzelfde moment][Option 1: Create a virtual machine and an availability set at the same time]. Vervolgens nieuwe virtuele machines toevoegen aan de set bij het maken van deze virtuele machines.
* [Optie 2: Een bestaande virtuele machine toevoegen aan een beschikbaarheidsset][Option 2: Add an existing virtual machine to an availability set].

> [!NOTE]
> In het klassieke model, moet virtuele machines die u wilt plaatsen in dezelfde beschikbaarheidsset behoren tot dezelfde cloudservice.
> 
> 

## <a id="createset"> </a>Optie 1: Maak een virtuele machine en een beschikbaarheidsset op hetzelfde moment
U kunt de Azure-portal of Azure PowerShell-opdrachten gebruiken om dit te doen.

De Azure portal gebruiken:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) als u dat nog niet hebt gedaan.
2. Klik op **maken van een resource** > **Compute**.
3. Selecteer de Marketplace virtuele machine-installatiekopie die u wilt gebruiken. U kunt een Linux- of Windows virtuele machine maken.
4. Controleer of het implementatiemodel is ingesteld voor de geselecteerde virtuele machine, **klassieke** en klik vervolgens op **maken**
   
    ![De installatiekopie van de alternatieve tekst](./media/virtual-machines-common-classic-configure-availability/ChooseClassicModel.png)
5. Voer een naam van virtuele machine, de gebruikersnaam en het wachtwoord (voor Windows-machines) of de openbare SSH-sleutel (voor Linux-machines). 
6. Kies de VM-grootte en klik vervolgens op **Selecteer** om door te gaan.
7. Kies **optionele configuratie > beschikbaarheidsset**, en selecteer de beschikbaarheidsset die u wilt toevoegen van de virtuele machine.
   
    ![De installatiekopie van de alternatieve tekst](./media/virtual-machines-common-classic-configure-availability/ChooseAvailabilitySet.png) 
8. Controleer uw configuratie-instellingen. Wanneer u bent klaar, klikt u op **maken**.
9. Terwijl Azure de virtuele machine maakt, u kunt de voortgang volgen onder **virtuele Machines** in het hubmenu.

Zie voor het gebruik van Azure PowerShell-opdrachten voor het maken van een virtuele machine van Azure en toe te voegen aan een nieuwe of bestaande beschikbaarheidsset, [Azure PowerShell gebruiken voor het maken en vooraf configureren van virtuele machines op basis van Windows](../articles/virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a id="addmachine"> </a>Optie 2: Een bestaande virtuele machine toevoegen aan een beschikbaarheidsset
In de Azure portal kunt u bestaande klassieke virtuele machines aan een bestaande beschikbaarheidsset instellen of maak een nieuwe voor ze kunt toevoegen. (Houd er rekening mee dat de virtuele machines in dezelfde beschikbaarheidsset tot dezelfde cloudservice behoren moet). De stappen zijn bijna hetzelfde. Met Azure PowerShell, kunt u de virtuele machine toevoegen aan een bestaande beschikbaarheidsset.

1. Als u dit nog niet hebt gedaan, meld u bij de [Azure-portal](https://portal.azure.com).
2. Klik in het menu links op **virtuele Machines (klassiek)**.
   
    ![De installatiekopie van de alternatieve tekst](./media/virtual-machines-common-classic-configure-availability/ChooseClassicVM.png)
3. Selecteer in de lijst met virtuele machines, de naam van de virtuele machine die u wilt toevoegen aan de set.
4. Kies **beschikbaarheidsset** van de virtuele machine **instellingen**.
   
    ![De installatiekopie van de alternatieve tekst](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetSettings.png)
5. Selecteer de beschikbaarheidsset die u wilt toevoegen van de virtuele machine. De virtuele machine moet behoren tot dezelfde cloudservice als de beschikbaarheidsset.
   
    ![De installatiekopie van de alternatieve tekst](./media/virtual-machines-common-classic-configure-availability/AvailabilitySetPicker.png)
6. Klik op **Opslaan**.

Azure PowerShell-opdrachten gebruiken door een administrator-niveau Azure PowerShell-sessie openen en voer de volgende opdracht. Voor de tijdelijke aanduidingen (zoals &lt;VmCloudServiceName&gt;), vervangt u alles binnen de aanhalingstekens, met inbegrip van de < en > tekens lang zijn en de juiste namen.

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

> [!NOTE]
> De virtuele machine wellicht opnieuw worden gestart voor het voltooien van deze toe te voegen aan de beschikbaarheidsset.
> 
> 

<!-- LINKS -->
[Option 1: Create a virtual machine and an availability set at the same time]: #createset
[Option 2: Add an existing virtual machine to an availability set]: #addmachine

[Load balancing for Azure infrastructure services]: ../articles/virtual-machines/virtual-machines-linux-load-balance.md
[Manage the availability of virtual machines]:../articles/virtual-machines/linux/manage-availability.md

[Create a virtual machine running Windows]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md
[Virtual Network overview]: ../articles/virtual-network/virtual-networks-overview.md

