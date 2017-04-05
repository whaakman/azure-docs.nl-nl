

VM-extensies kunnen u helpen bij het volgende:

* Het wijzigen van beveiligings- en identiteitsfuncties, zoals het opnieuw instellen van accountwaarden en het gebruik van antimalware
* Het starten, stoppen of configureren van bewaking en diagnostische functies
* Het opnieuw instellen of installeren van connectiviteitsfuncties, zoals RDP en SSH
* Het diagnosticeren, bewaken en beheren van virtuele machines

Daarnaast zijn er nog vele andere functies. Er worden regelmatig nieuwe functies voor VM-extensie vrijgegeven. Dit artikel beschrijft de Azure VM-agents voor Windows en Linux, en hoe deze de functionaliteit van VM-extensies ondersteunen. Zie voor een overzicht van VM-extensies op functie [Azure VM Extensions and Features](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM-extensies en -functies in Azure).

## <a name="azure-vm-agents-for-windows-and-linux"></a>Azure VM-agents voor Windows en Linux
De Azure Virtual Machines Agent (VM-agent) is een veilig en eenvoudig proces voor het installeren, configureren en verwijderen van VM-extensies op exemplaren van virtuele Azure-machines. De VM-agent fungeert als beveiligde lokale besturingsservice voor uw virtuele Azure-machine. De extensies die door de agent worden geladen, bieden specifieke functies om uw productiviteit met het exemplaar te verhogen.

Er bestaan twee Azure VM-agents: één voor virtuele Windows-machines en één voor virtuele Linux-machines.

Als u wilt dat een VM-exemplaar een of meer VM-extensies gebruikt, moet op het exemplaar een VM-agent zijn geïnstalleerd. VM-installatiekopieën die met Azure Portal zijn gemaakt en installatiekopieën uit **Marketplace** installeren tijdens het proces automatisch een VM-agent. Als een VM-exemplaar geen VM-agent heeft, kunt u de VM-agent installeren nadat het VM-exemplaar is gemaakt. U kunt de agent ook installeren in een aangepaste VM-installatiekopie, die u daarna uploadt.

> [!IMPORTANT]
> Deze VM-agents zijn zeer eenvoudige services die veilig beheer van VM-exemplaren mogelijk maken. In bepaalde gevallen wilt u mogelijk niet met een VM-agent werken. Als dat zo is, maakt u met de Azure CLI of PowerShell virtuele machines waarop geen VM-agent is geïnstalleerd. Hoewel de VM-agent fysiek kan worden verwijderd, is de werking van VM-extensies op het exemplaar onvoorspelbaar. Daarom wordt het verwijderen van een geïnstalleerde VM-agent niet ondersteund.
>

De VM-agent wordt in de volgende situaties ingeschakeld:

* Wanneer u een VM-exemplaar maakt met behulp van Azure Portal of een installatiekopie van **Marketplace** selecteert.
* Wanneer u een VM-exemplaar maakt met behulp van de cmdlet [New-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx) of [New-AzureQuickVM](https://msdn.microsoft.com/library/azure/dn495183.aspx). Wanneer u een virtuele machine zonder VM-agent maakt door de parameter **–DisableGuestAgent** toe te voegen aan de cmdlet [Add-AzureProvisioningConfig](https://msdn.microsoft.com/library/azure/dn495299.aspx).

* Wanneer u de VM-agent handmatig downloadt en op een bestaand VM-exemplaar installeert, en de waarde **ProvisionGuestAgent** instelt op **waar**. U kunt deze methode voor Windows- en Linux-agents gebruiken door een PowerShell-opdracht of een REST-aanroep te gebruiken. (Als u de waarde **ProvisionGuestAgent** niet instelt nadat u de VM-agent handmatig hebt geïnstalleerd, wordt het toevoegen van de VM-agent niet goed gedetecteerd.) In de volgende voorbeeldcode ziet u hoe u dit doet met PowerShell, waar de argumenten `$svc` en `$name` al zijn bepaald:

      $vm = Get-AzureVM –ServiceName $svc –Name $name
      $vm.VM.ProvisionGuestAgent = $TRUE
      Update-AzureVM –Name $name –VM $vm.VM –ServiceName $svc

* Wanneer u een VM-installatiekopie maakt die een geïnstalleerde VM-agent bevat. Wanneer de installatiekopie met de VM-agent ils gemaakt, kunt u de installatiekopie naar Azure uploaden. Voor een virtuele Windows-machine downloadt u het bestand [Windows VM Agent .msi](http://go.microsoft.com/fwlink/?LinkID=394789) en installeert u de VM-agent. Voor een virtuele Linux-machine installeert u de VM-agent vanuit de GitHub-opslagplaats op <https://github.com/Azure/WALinuxAgent>. Raadpleeg de [Gebruikershandleiding voor Azure VM-agents voor Linux](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie over het installeren van de VM-agent op Linux.

> [!NOTE]
> In PaaS wordt de VM-agent **WindowsAzureGuestAgent** genoemd. Deze is altijd beschikbaar op web- en werkrol-VM's. (Zie [Azure Role Architecture](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) (Architectuur van Azure-rollen) voor meer informatie.) Met de VM-agent voor rol-VM's kunnen nu extensies aan de cloudservice-VM's worden toegevoegd op dezelfde manier als voor permanente virtuele machines. Het grootste verschil tussen VM-extensies op rol-VM's en permanente VM's is het moment waarop de VM-extensies worden toegevoegd. In het geval van rol-VM's worden de extensies eerst toegevoegd aan de cloudservice en vervolgens aan de implementaties binnen die cloudservice.
>
> Gebruik de cmdlet [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) om alle beschikbare rol-VM-extensies weer te geven.
>
>

## <a name="find-add-update-and-remove-vm-extensions"></a>VM-extensies zoeken, toevoegen, bijwerken en verwijderen
Zie [Azure VM-extensies toevoegen, zoeken, bijwerken en verwijderen](../articles/virtual-machines/windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) voor meer informatie over deze taken.
