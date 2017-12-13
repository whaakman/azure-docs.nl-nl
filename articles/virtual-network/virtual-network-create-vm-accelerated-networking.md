---
title: Maken van een virtuele machine van Azure met versnelde toegang | Microsoft Docs
description: Informatie over het maken van een virtuele machine met versnelde netwerken.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 233e0449bc0803709f0aa369a446c2ec5d3f177e
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-virtual-machine-with-accelerated-networking"></a>Een virtuele machine met versnelde toegang maken

In deze zelfstudie leert u hoe een Azure-virtuele Machine (VM) met versnelde netwerken maken. Versnelde netwerken wordt GA voor Windows en een openbare Preview voor specifieke Linux-distributies. Versnelde netwerken kan één i/o-virtualisatie hoofdmap (SR-IOV) voor een virtuele machine, aanzienlijk verbeteren de prestaties van netwerken. Dit pad hoge prestaties wordt de host van het gegevenspad waardoor latentie en jitter CPU-gebruik, voor gebruik met de zwaarste netwerkbelasting op ondersteunde VM typen overgeslagen. De volgende afbeelding ziet communicatie tussen twee virtuele machines (VM) met en zonder versnelde netwerken:

![Vergelijking](./media/virtual-network-create-vm-accelerated-networking/image1.png)

Alle netwerkverkeer naar en vanuit de virtuele machine moet zonder versnelde netwerken passeren de host en de virtuele switch. De virtuele switch biedt alle afdwingen van beleid, zoals netwerkbeveiligingsgroepen, access control list, isolatie- en andere gevirtualiseerd netwerkservices op netwerkverkeer. Lees voor meer informatie over virtuele switches, de [Hyper-V-netwerkvirtualisatie en virtuele switch](https://technet.microsoft.com/library/jj945275.aspx) artikel.

Met versnelde netwerken netwerkverkeer binnenkomt bij de VM-netwerkadapter (NIC) en wordt vervolgens doorgestuurd naar de virtuele machine. Alle netwerkbeleid dat past u de virtuele switch zonder versnelde netwerken zijn offloaded en toegepast in de hardware. Toepassen van beleid in de hardware, kunt de NIC forward netwerkverkeer rechtstreeks naar de virtuele machine, het omzeilen van de host en de virtuele switch, terwijl alle het beleid dat wordt toegepast op de host.

De voordelen van versnelde netwerken zijn alleen van toepassing op de virtuele machine die is ingeschakeld. Voor de beste resultaten is het ideaal voor het inschakelen van deze functie op ten minste twee virtuele machines die zijn verbonden met de dezelfde Azure Virtual Network (VNet). Tijdens de communicatie tussen VNets of verbindende on-premises, heeft dit onderdeel minimale gevolgen voor de algehele latentie.

> [!WARNING]
> Deze openbare Preview van Linux kan niet hetzelfde niveau van beschikbaarheid en betrouwbaarheid hebben, zoals functies die in het algemeen beschikbaarheid release. De functie wordt niet ondersteund, kan hebben beperkte mogelijkheden en mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg de pagina Azure Virtual Network-updates voor de meest recente meldingen op beschikbaarheid en de status van deze functie.

## <a name="benefits"></a>Voordelen
* **Lagere latentie / hoger pakketten per seconde (pps):** verwijderen van de virtuele switch uit het gegevenspad verwijdert u de tijd te besteden aan de pakketten in de host voor de verwerking van beleid en verhoogt het aantal pakketten dat kan worden verwerkt in de virtuele machine.
* **Minder jitter:** virtuele switch verwerking is afhankelijk van de hoeveelheid beleid die moet worden toegepast en de werkbelasting van de CPU die bezig is met de verwerking. Offloading van het afdwingen van beleid op de hardware verwijdert die variabiliteit door het leveren van pakketten rechtstreeks naar de virtuele machine, het verwijderen van de host voor VM-communicatie en alle software-interrupts en context switches.
* **CPU-gebruik verlaagd:** overslaan van de virtuele switch op de host leidt tot minder CPU-gebruik voor het verwerken van netwerkverkeer.

## <a name="Limitations"></a>Beperkingen
De volgende beperkingen bestaan wanneer deze wordt met deze mogelijkheid:

* **Interface maken van een netwerk:** Accelerated netwerken kan alleen worden ingeschakeld voor een nieuwe NIC. Deze kan niet worden ingeschakeld voor een bestaande NIC.
* **Maken van VM:** een NIC met versnelde netwerken ingeschakeld kan alleen worden gekoppeld aan een VM wanneer de virtuele machine wordt gemaakt. De NIC kan niet worden gekoppeld aan een bestaande virtuele machine. Als de virtuele machine toe te voegen aan de bestaande beschikbaarheidsset is ingesteld, moeten alle virtuele machines in de beschikbaarheidsset ook versnelde netwerken ingeschakeld.
* **Gebieden:** Windows virtuele machines met versnelde netwerken worden aangeboden in de meeste Azure-regio's. Virtuele Linux-machines met versnelde netwerken worden aangeboden in meerdere regio's. De mogelijkheid is beschikbaar in regio's is uitgebreid. Voor de meest recente informatie, Zie de [Azure virtuele netwerken Updates](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview) blog.   
* **Ondersteunde besturingssystemen:** Windows: Microsoft Windows Server 2012 R2 Datacenter- en Windows Server 2016. Linux: Ubuntu Server 16.04 TNS met kernel 4.4.0-77 of hoger, SLES 12 SP2 RHEL 7.3 en CentOS 7.3 (gepubliceerd door 'Rogue Wave Software').
* **VM-grootte:** algemeen en de grootte van de compute-geoptimaliseerde exemplaar met acht of meer cores. Zie voor meer informatie de [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-groottes artikelen. De set ondersteunde grootten voor VM-exemplaar vergroten in de toekomst.
* **Implementatie via Azure Resource Manager (ARM) alleen:** versnelde toegang is niet beschikbaar voor implementatie via ASM/RDFE.

Wijzigingen in deze beperkingen zijn aangekondigd door de [virtuele netwerken van Azure updates](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview) pagina.

## <a name="create-a-windows-vm"></a>Een Windows-VM maken
U kunt de Azure portal of Azure [PowerShell](#windows-powershell) voor het maken van de virtuele machine.

### <a name="windows-portal"></a>Portal

1. Open Azure in een internetbrowser [portal](https://portal.azure.com) en meld u aan met uw Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Als u nog een account hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Klik in de portal op **+ nieuw** > **Compute** > **Windows Server 2016 Datacenter**. 
3. In de **Windows Server 2016 Datacenter** blade die wordt weergegeven, laat de eigenschap *Resource Manager* geselecteerde onder **een implementatiemodel selecteren**, en klik op **maken** .
4. In de **basisbeginselen** blade die wordt weergegeven, voer de volgende waarden, laat u de resterende standaardopties selecteren of uw eigen waarden, en voer de **OK** knop:

    |Instelling|Waarde|
    |---|---|
    |Naam|MyVm|
    |Resourcegroep|Laat **nieuw** geselecteerd en voer *MyResourceGroup*|
    |Locatie|VS - west 2|

    Als u geen ervaring met Azure, meer informatie over [resourcegroepen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [abonnementen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), en [locaties](https://azure.microsoft.com/regions) (die worden ook aangeduid als regio's).
5. In de **een grootte kiezen** blade die wordt weergegeven, invoeren *8* in de **Minimum kernen** vak en klik vervolgens op **weergeven van alle**.
6. Klik op **DS4_V2 standaard**, of een ondersteunde VM, klikt u op de **Selecteer** knop.
7. In de **instellingen** blade die wordt weergegeven, laat u alle instellingen als-is, met uitzondering van Klik **ingeschakeld** onder **versnelde netwerken**, klikt u vervolgens op de **OK** knop. **Opmerking:** als in de vorige stappen hebt waarden voor de grootte, het besturingssysteem of de locatie die niet zijn opgenomen in VM geselecteerd de [beperkingen](#Limitations) sectie van dit artikel **versnelde netwerken** niet zichtbaar is.
8. In de **samenvatting** blade die wordt weergegeven, klikt u op de **OK** knop. Azure start u het maken van de virtuele machine. Maken van VM duurt een paar minuten.
9. Installeer het stuurprogramma voor versnelde netwerken voor Windows, voer de stappen in de [Windows configureren](#configure-windows) sectie van dit artikel.

### <a name="windows-powershell"></a>PowerShell
1. Installeer de nieuwste versie van de Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module. Als u geen ervaring met Azure PowerShell, leest u de [overzicht van Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.
2. Een PowerShell-sessie starten door te klikken op de knop Start typen **powershell**, vervolgens te klikken op **PowerShell** in de zoekresultaten.
3. Voer in het PowerShell-venster de `login-azurermaccount` opdracht zich kunnen aanmelden met uw Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Als u nog een account hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Kopieer het volgende script in uw browser:
    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Windows `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id 

    # Create the virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    #
    ```
5. In uw PowerShell-venster met de rechtermuisknop om het script te plakken en start deze wordt uitgevoerd. U wordt gevraagd om een gebruikersnaam en wachtwoord. Gebruikt deze referenties aan te melden bij de virtuele machine wanneer er verbinding mee te maken in de volgende stap. Als het script mislukt en u de waarden in het script gewijzigd voordat deze wordt uitgevoerd, bevestigt u de waarden voor VM-grootte, besturingssysteem, en locatie worden vermeld in de [beperkingen](#Limitations) sectie van dit artikel.
6. Installeer het stuurprogramma voor versnelde netwerken voor Windows, voer de stappen in de [Windows configureren](#configure-windows) sectie van dit artikel.

### <a name="configure-windows"></a>Windows configureren
Zodra u de virtuele machine in Azure maakt, moet u de versnelde-netwerkstuurprogramma installeren voor Windows. Voordat u de volgende stappen uitvoert, moet u een virtuele Windows-machine gemaakt met versnelde netwerken met behulp van de [portal](#windows-portal) of [PowerShell](#windows-powershell) stappen in dit artikel. 

1. Open Azure in een internetbrowser [portal](https://portal.azure.com) en meld u aan met uw Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Als u nog een account hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p).
2. In het vak dat de tekst bevat *zoeken bronnen* aan de bovenkant van de Azure-portal, typ *MyVm*. Wanneer **MyVm** wordt weergegeven in de zoekresultaten, klik erop.
3. In de **MyVm** blade die wordt weergegeven, klikt u op de **Connect** knop in de linkerbovenhoek van de blade. **Opmerking:** als **maken** zichtbaar onder de **Connect** knop Azure is nog niet voltooid voor het maken van de virtuele machine. Klik op **Connect** pas nadat u niet meer zien **maken** onder de **Connect** knop.
4. Kan de browser voor het downloaden van de **MyVm.rdp** bestand.  Nadat u hebt gedownload, klikt u op het bestand te openen. 
5. Klik op de **Connect** knop in de **verbinding met extern bureaublad** vak dat wordt weergegeven om u te informeren dat de uitgever van de externe verbinding kan niet worden vastgesteld.
6. In de **Windows-beveiliging** dat verschijnt, klikt u op **meer opties**, klikt u vervolgens op **gebruik een ander account**. Geef de gebruikersnaam en wachtwoord die u hebt opgegeven in stap 4 en klik vervolgens op de **OK** knop.
7. Klik op de **Ja** knop in het dialoogvenster verbinding met extern bureaublad wordt weergegeven, om u te informeren dat de identiteit van de externe computer kan niet worden geverifieerd.
8. Met de rechtermuisknop op de knop Start en klik op **Apparaatbeheer**. Vouw de **netwerkadapters** knooppunt. Controleer of de **Mellanox ConnectX 3 virtuele functie Ethernet-Adapter** wordt weergegeven, zoals wordt weergegeven in de volgende afbeelding:
   
    ![Apparaatbeheer](./media/virtual-network-create-vm-accelerated-networking/image2.png)

9. Versnelde netwerken is nu ingeschakeld voor uw virtuele machine.

## <a name="create-a-linux-vm"></a>Een Linux-VM maken
U kunt de Azure portal of Azure [PowerShell](#linux-powershell) een Ubuntu of SLES VM te maken. Zie voor instructies RHEL en CentOS [RHEL en CentOS](#rhel-and-centos).

### <a name="linux-portal"></a>Portal
1. Registreren voor versnelde netwerken voor Linux-preview door te voeren 1-5 van stappen de [maken van een Linux-VM - PowerShell](#linux-powershell) sectie van dit artikel.  U registreren niet voor de Preview-versie in de portal.
2. Voltooi de stappen 1-8 in de [maken van een Windows-VM - portal](#windows-portal) sectie van dit artikel. Klik in stap 2 op **Ubuntu Server 16.04 LTS** in plaats van **Windows Server 2016 Datacenter**. Voor deze zelfstudie kiezen om een wachtwoord, in plaats van een SSH-sleutel al is voor productie-implementaties, kunt u een gebruiken. Als **versnelde netwerken** wordt niet weergegeven wanneer u bij het voltooien van stap 7 van de [maken van een Windows-VM - portal](#windows-portal) sectie van dit artikel, is het waarschijnlijk voor een van de volgende redenen:
    - U nog niet zijn geregistreerd voor de preview. Controleer of de status van uw registratie **geregistreerde**, zoals wordt beschreven in stap 4 van de [maken van een Linux-VM - Powershell](#linux-powershell) sectie van dit artikel. **Opmerking:** als u hebt deelgenomen aan de netwerken Accelerated voor VM's van Windows-preview (is niet meer nodig zijn om u te registreren voor het gebruik van Accelerated netwerken voor Windows VM's), u worden niet automatisch geregistreerd voor de Accelerated netwerken voor virtuele Linux-machines Voorbeeld. U moet registreren voor de Accelerated netwerken voor virtuele Linux-machines preview deel te nemen aan het.
    - U hebt een VM-grootte, het besturingssysteem, niet geselecteerd of locatie weergegeven de [beperkingen](#limitations) sectie van dit artikel.
3. Installeer het stuurprogramma voor versnelde netwerken voor Linux, voer de stappen in de [Linux configureren](#configure-linux) sectie van dit artikel.

### <a name="linux-powershell"></a>PowerShell

>[!WARNING]
>Als u virtuele Linux-machines met versnelde netwerken in een abonnement maken en vervolgens probeert te maken van een virtuele machine van Windows met versnelde netwerken in hetzelfde abonnement, kan het maken van de virtuele machine van Windows mislukken. Tijdens deze preview kunt u het beste Linux- en Windows-VM's te met versnelde netwerken in afzonderlijke abonnementen testen.
>

1. Installeer de nieuwste versie van de Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module. Als u geen ervaring met Azure PowerShell, leest u de [overzicht van Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.
2. Een PowerShell-sessie starten door te klikken op de knop Start typen **powershell**, vervolgens te klikken op **PowerShell** in de zoekresultaten.
3. Voer in het PowerShell-venster de `login-azurermaccount` opdracht zich kunnen aanmelden met uw Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Als u nog een account hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Registreren voor de versnelde netwerken voor Azure preview via de volgende stappen:
    - Een e-mail sturen naar [ axnpreview@microsoft.com ](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) met uw Azure-abonnement-ID en het beoogde gebruik. Voltooi de volgende stappen totdat niet wanneer u een e-mailbevestiging van Microsoft of uw abonnement is ingeschakeld voor versnelde netwerken hebt ontvangen.
    - Voer de volgende opdracht om te bevestigen dat u bent geregistreerd voor de Preview-versie:
    
        ```powershell
        Get-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingForLinux -ProviderNamespace Microsoft.Network
        ```

        Ga niet verder met stap 5 tot en met **geregistreerde** wordt weergegeven in de uitvoer nadat u de vorige opdracht invoeren. De uitvoer moet eruitzien als de volgende uitvoer voordat u verdergaat:
    
        ```powershell
        FeatureName                        ProviderName      RegistrationState
        -----------                        ------------      -----------------
        AllowAcceleratedNetworkingForLinux Microsoft.Network Registered
        ```
        
      >[!NOTE]
      >Als u hebt deelgenomen aan de netwerken Accelerated voor VM's van Windows-preview (is niet meer nodig zijn om u te registreren voor het gebruik van Accelerated netwerken voor Windows-VM's), worden u niet automatisch geregistreerd voor de Accelerated netwerken voor virtuele Linux-machines bekijken. U moet registreren voor de Accelerated netwerken voor virtuele Linux-machines preview deel te nemen aan het.
      >
5. Kopieer het volgende script vervangen door Ubuntu of SLES naar wens in uw browser.  Opnieuw, hebben een andere workflow uiteengezet in Redhat en CentOS [RHEL en CentOS](#rhel-and-centos):

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Create a new Storage account and define the new VM’s OSDisk name and its URI
    # Must end with ".vhd" extension
    $OSDiskName = "MyOsDiskName.vhd"
    # Storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.
    $OSDiskSAName = "thestorageaccountname"  
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $RgName -Name $OSDiskSAName -Type "Standard_GRS" -Location $Location
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName
 
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Linux `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName Canonical `
      -Offer UbuntuServer `
      -Skus 16.04-LTS `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk -Name $OSDiskName `
      -VhdUri $OSDiskUri `
      -CreateOption FromImage 

    # Create the virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    ```
    
6. In uw PowerShell-venster met de rechtermuisknop om het script te plakken en start deze wordt uitgevoerd. U wordt gevraagd om een gebruikersnaam en wachtwoord. Gebruikt deze referenties aan te melden bij de virtuele machine wanneer er verbinding mee te maken in de volgende stap. Als het script is mislukt, Controleer of:
    - U bent geregistreerd voor de preview, zoals beschreven in stap 4
    - Als u VM-grootte, besturingssysteemtype of locatie-waarden in het script gewijzigd voordat deze wordt uitgevoerd, dat de waarden worden vermeld in de [beperkingen](#Limitations) sectie van dit artikel.
7. Installeer het stuurprogramma voor versnelde netwerken voor Linux, voer de stappen in de [Linux configureren](#configure-linux) sectie van dit artikel.

### <a name="configure-linux"></a>Configureren van Linux

Zodra u de virtuele machine in Azure maakt, moet u de versnelde-netwerkstuurprogramma voor Linux installeren. Voordat u de volgende stappen uitvoert, moet u een Linux-VM gemaakt met versnelde netwerken met behulp van de [portal](#linux-portal) of [PowerShell](#linux-powershell) stappen in dit artikel. 

1. Open Azure in een internetbrowser [portal](https://portal.azure.com) en meld u aan met uw Azure [account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Als u nog een account hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Aan de bovenkant van de portal, rechts van de *zoeken bronnen* staaf-, klikt u op de **> _** pictogram een cloud Bash-shell (Preview) starten. Geeft het deelvenster Bash cloud shell wordt weergegeven aan de onderkant van de portal en na een paar seconden een  **username@Azure:~ $** prompt. Hoewel u SSH naar de virtuele machine vanaf uw computer in plaats van de cloud-shell kunt, de instructies in deze zelfstudie wordt ervan uitgegaan dat u de cloud-shell.
3. Aan de bovenkant van de portal in het vak dat de tekst bevat *zoeken bronnen*, type *MyVm*. Wanneer **MyVm** wordt weergegeven in de zoekresultaten, klik erop.
4. In de **MyVm** blade die wordt weergegeven, klikt u op de **Connect** knop in de linkerbovenhoek van de blade. **Opmerking:** als **maken** zichtbaar onder de **Connect** knop Azure is nog niet voltooid voor het maken van de virtuele machine. Klik op **Connect** pas nadat u niet meer zien **maken** onder de **Connect** knop.
5. Azure Hiermee opent u een melding dat u invoert in het `ssh adminuser@<ipaddress>`. Voer deze opdracht in de cloud shell (of kopie in het vak dat komt in stap 4 en plak deze in de cloud-beheershell), druk op Enter.
6. Voer **Ja** op de vraag waarin u als u verbinden wilt, druk op Enter.
7. Voer het wachtwoord die u hebt ingevoerd bij het maken van de virtuele machine. Eenmaal is aangemeld bij de virtuele machine, ziet u een adminuser@MyVm:~ $ prompt. U bent nu aangemeld met de virtuele machine via de cloud shell-sessie. **Opmerking:** Cloud shell sessies time-out na 10 minuten van inactiviteit.

Op dit moment afhankelijk de instructies van de distributie die u gebruikt. 

#### <a name="ubuntusles"></a>Ubuntu/SLES

1. Voer bij de prompt `uname -r` en controleer de versie voor:

    * Ubuntu is '4.4.0-77-generic,' of hoger
    * SLES is '4.4.59-92.20-default' of hoger

2. Maak een band tussen de vNIC van de standaard netwerken en de versnelde netwerken vNIC met de opdrachten die volgen. Netwerkverkeer maakt gebruik van de hogere presterende versnelde netwerken vNIC, terwijl de obligatie zorgt ervoor dat netwerkverkeer via de configuratiewijzigingen niet wordt onderbroken.
          
     ```bash
     wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/configure_hv_sriov.sh
     chmod +x ./configure_hv_sriov.sh
     sudo ./configure_hv_sriov.sh
     ```
3. Nadat het script is uitgevoerd, wordt de virtuele machine opnieuw opgestart na een onderbreking 60 seconden.
4. Zodra de virtuele machine opnieuw is opgestart, opnieuw verbinding maken met het door stap 5-7 opnieuw uit te voeren.
5. Voer de `ifconfig` opdracht in en Bevestig dat bond0 is actief en de interface wordt weergegeven als omhoog. 
 
 >[!NOTE]
      >Toepassingen die gebruikmaken van versnelde netwerken moeten communiceren via de *bond0* interface niet *eth0*.  Naam van de interface overgaan voordat versnelde netwerken algemene beschikbaarheid bereikt.

#### <a name="rhel-and-centos"></a>RHEL en CentOS

Maken van een Red Hat Enterprise Linux of CentOS 7.3 VM, is enkele extra stappen uitvoeren om het laden van de nieuwste stuurprogramma's die nodig is voor SR-IOV en de functie VF stuurprogramma (Virtual) voor de netwerkkaart vereist. De eerste fase van de instructies bereidt een installatiekopie die kan worden gebruikt voor het maken van een of meer virtuele machines die de stuurprogramma's die vooraf is geladen.

##### <a name="phase-1-prepare-a-red-hat-enterprise-linux-or-centos-73-base-image"></a>Fase 1: Een Red Hat Enterprise Linux of CentOS 7.3 basisinstallatiekopie voorbereiden 

1.  Inrichten van een niet - SRIOV CentOS 7.3 VM op Azure

2.  LIS 4.2.2 installeren:
    
    ```bash
    wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
    tar -xvf lis-rpms-4.2.2-2.tar.gz
    cd LISISO && sudo ./install.sh
    ```

3.  De config-bestanden downloaden
    
    ```bash
    cd /etc/udev/rules.d/  
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/60-hyperv-vf-name.rules 
    cd /usr/sbin/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/hv_vf_name 
    sudo chmod +x hv_vf_name
    cd /etc/sysconfig/network-scripts/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/ifcfg-vf1   
    ```

4.  Inrichting ervan ongedaan maakt deze VM

    ```bash
    sudo waagent -deprovision+user 
    ```

5.  Vanuit Azure-portal stop deze virtuele machine; en gaat u naar een van de virtuele machine '-schijven genoemd, vastleggen van de OSDisk VHD-URI. Deze URI bevat de naam van de basisinstallatiekopie VHD en de storage-account. 
 
##### <a name="phase-2-provision-new-vms-on-azure"></a>Fase 2: Inrichten van nieuwe virtuele machines in Azure

1.  Nieuwe virtuele machines inrichten op basis van met New-AzureRMVMConfig met behulp van de basisinstallatiekopie VHD vastgelegd in de fase 1, met AcceleratedNetworking ingeschakeld op de vNIC:

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
     -Name $RgName `
     -Location $Location

    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
     -Name MySubnet `
     -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
     -ResourceGroupName $RgName `
     -Location $Location `
     -Name MyVnet `
     -AddressPrefix 10.0.0.0/16 `
     -Subnet $Subnet
    
    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
     -Name MyPublicIp `
     -ResourceGroupName $RgName `
     -Location $Location `
     -AllocationMethod Static
    
    # Create a virtual network interface and associate the public IP address to it
    $Nic = New-AzureRmNetworkInterface `
     -Name MyNic `
     -ResourceGroupName $RgName `
     -Location $Location `
     -SubnetId $Vnet.Subnets[0].Id `
     -PublicIpAddressId $Pip.Id `
     -EnableAcceleratedNetworking
    
    # Specify the base image's VHD URI (from phase 1, step 5). 
    # Note: The storage account of this base image vhd should have "Storage service encryption" disabled
    # See more from here: https://docs.microsoft.com/azure/storage/storage-service-encryption
    # This is just an example URI, you will need to replace this when running this script
    $sourceUri="https://myexamplesa.blob.core.windows.net/vhds/CentOS73-Base-Test120170629111341.vhd" 

    # Specify a URI for the location from which the new image binary large object (BLOB) is copied to start the virtual machine. 
    # Must end with ".vhd" extension
    $OsDiskName = "MyOsDiskName.vhd" 
    $destOsDiskUri = "https://myexamplesa.blob.core.windows.net/vhds/" + $OsDiskName
    
    # Define a credential object for the VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential
    
    # Create a custom virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
     -VMName MyVM -VMSize Standard_DS4_v2 | `
    Set-AzureRmVMOperatingSystem `
     -Linux `
     -ComputerName myVM `
     -Credential $Cred | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk `
     -Name $OsDiskName `
     -SourceImageUri $sourceUri `
     -VhdUri $destOsDiskUri `
     -CreateOption FromImage `
     -Linux
    
    # Create the virtual machine.    
    New-AzureRmVM `
     -ResourceGroupName $RgName `
     -Location $Location `
     -VM $VmConfig
    ```

2.  Nadat de virtuele machines wordt opgestart, Controleer het VF-apparaat door 'lspci' en de vermelding Mellanox controleren. U ziet bijvoorbeeld de volgende tekst in de uitvoer lspci:
    
    ```
    0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
    ```
    
3.  Voer het script voor de binding door:

    ```bash
    sudo bondvf.sh
    ```

4.  Start opnieuw op de nieuwe virtuele machines:

    ```bash
    sudo reboot
    ```

De virtuele machine moet beginnen met bond0 geconfigureerd en het pad versnelde netwerken is ingeschakeld.  Voer `ifconfig` om te bevestigen.
