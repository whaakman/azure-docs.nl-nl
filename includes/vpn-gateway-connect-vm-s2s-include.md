U kunt verbinding maken met een VM die op uw VNet is geïmplementeerd door een verbinding met extern bureaublad te maken voor uw VM. De beste manier om eerst te controleren of u verbinding met uw VM kunt maken is door verbinding te maken met behulp van het privé-IP-adres in plaats van de computernaam. Op die manier test u of u verbinding kunt maken, niet of naamomzetting correct is geconfigureerd.

1. **Zoek het privé-IP-adres.** U vindt het privé-IP-adres van een VM door naar de eigenschappen voor de VM te kijken in Azure Portal of door PowerShell te gebruiken.

  - Azure Portal: vind uw virtuele machine in Azure Portal. Bekijk de eigenschappen voor de VM. Het privé-IP-adres wordt vermeld.

  - PowerShell: gebruik het voorbeeld om een lijst met VM's en privé-IP-adressen uit uw resourcegroepen weer te geven. U hoeft het voorbeeld niet te wijzigen voordat u het gebruikt.

    ```powershell
    $vms = get-azurermvm
    $nics = get-azurermnetworkinterface | where VirtualMachine -NE $null

    foreach($nic in $nics)
    {
      $vm = $vms | where-object -Property Id -EQ $nic.VirtualMachine.id
      $prv = $nic.IpConfigurations | select-object -ExpandProperty PrivateIpAddress
      $alloc = $nic.IpConfigurations | select-object -ExpandProperty PrivateIpAllocationMethod
      Write-Output "$($vm.Name): $prv,$alloc"
    }
    ```

2. **Maak verbinding met de VM.** Controleer of u met uw VNet bent verbonden met behulp van de VPN-verbinding. Open Verbinding met extern bureaublad door "RDP" of "Verbinding met extern bureaublad" te typen in het zoekvak op de taakbalk en selecteer vervolgens Verbinding met extern bureaublad. U kunt Verbinding met extern bureaublad ook openen met behulp van de opdracht `mstsc` in PowerShell. Voer het privé-IP-adres van de VM in Verbinding met extern bureaublad in. U kunt op Opties weergeven klikken om aanvullende instellingen aan te passen en vervolgens verbinding maken.

### <a name="to-troubleshoot-an-rdp-connection-to-a-vm"></a>Problemen met de RDP-verbinding met een VM oplossen

Als u problemen ondervindt bij het verbinding maken met een virtuele machine via de VPN-verbinding, kunt u een aantal zaken controleren.

- Controleer of uw VPN-verbinding tot stand is gebracht.
- Controleer of u verbinding maakt met het privé-IP-adres voor de VM.
- Als u verbinding met de VM kunt maken met behulp van het privé-IP-adres, maar niet met de computernaam, controleert u of DNS correct is geconfigureerd.
- Zie [Troubleshoot Remote Desktop connections to a VM](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md) (Problemen met Extern-bureaubladverbindingen met een VM oplossen) voor meer informatie.