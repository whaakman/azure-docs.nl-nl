---
title: Het inschakelen van geneste virtualisatie in Azure Virtual Machines | Microsoft Docs
description: Het geneste virtualisatie in Azure Virtual Machines inschakelen
services: virtual-machines-windows
documentationcenter: virtual-machines
author: philmea
manager: timlt
ms.author: philmea
ms.date: 10/09/2017
ms.topic: howto
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.openlocfilehash: 180b87e18d98bb1e7ddefdcce09fc45d2fc26d0f
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="how-to-enable-nested-virtualization-in-an-azure-vm"></a>Het inschakelen van geneste netwerkvirtualisatie in een Azure VM

Geneste virtualisatie wordt ondersteund in de reeks Dv3 en Ev3 van virtuele machines in Azure. Deze mogelijkheid biedt grote flexibiliteit bij het ondersteunen van scenario's zoals ontwikkelen, testen, training en demonstratie omgevingen. 

In dit artikel wordt stapsgewijs via geneste virtualisatie op een virtuele machine van Azure inschakelen en configureren van internetverbinding naar die virtuele gastmachine.

## <a name="create-a-dv3-or-ev3-series-azure-vm"></a>Een Dv3 of Ev3 reeksen virtuele Azure-machine maken

Maken van een nieuwe Windows Server 2016 Azure virtuele machine en kies een grootte van de reeks Dv3 of Ev3. Zorg ervoor dat u kiest een grootte die groot genoeg is voor de ondersteuning van de vereisten van een virtuele gastmachine. In dit voorbeeld gebruiken we een grootte D3_v3 Azure VM. 

U kunt de regionale beschikbaarheid van Dv3 of Ev3 reeks virtuele machines weergeven [hier](https://azure.microsoft.com/regions/services/).

>[!NOTE]
>
>Zie voor gedetailleerde instructies over het maken van een nieuwe virtuele machine [maken en beheren van VM's van Windows met de Azure PowerShell-module](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm)
    
## <a name="connect-to-your-azure-vm"></a>Verbinding maken met uw Azure VM

Maak via een extern bureaublad verbinding met de virtuele machine.

1. Klik op de knop **Verbinden** in de eigenschappen van de virtuele machine. Er wordt een Remote Desktop Protocol-bestand (.rdp) gemaakt en gedownload.

2. Open het gedownloade RDP-bestand om verbinding met de VM te maken. Als u hierom wordt gevraagd, klikt u op **Verbinden**. Op een Mac hebt u een RDP-client nodig, zoals deze [Extern-bureaubladclient](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) uit de Mac App Store.

3. Voer de gebruikersnaam en het wachtwoord in die u hebt opgegeven bij het maken van de virtuele machine en klik vervolgens op **OK**.

4. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Klik op **Ja** of **Doorgaan** om door te gaan met de verbinding.

## <a name="enable-the-hyper-v-feature-on-the-azure-vm"></a>De functie Hyper-V op de virtuele machine van Azure inschakelen
U kunt deze instellingen handmatig of we een PowerShell-script voor het automatiseren van de configuratie hebt opgegeven.

### <a name="option-1-use-a-powershell-script-to-configure-nested-virtualization"></a>Optie 1: Een PowerShell-script gebruiken voor het virtualisatie geneste configureren
Er is een PowerShell-script om in te schakelen geneste virtualisatie op een host met Windows Server 2016 beschikbaar op [GitHub](https://github.com/charlieding/Virtualization-Documentation/tree/live/hyperv-tools/Nested). Het script vereisten controleert en configureert vervolgens geneste virtualisatie op de virtuele machine in Azure. Opnieuw opstarten van de Azure VM is nodig om de configuratie te voltooien. Dit script werkt in andere omgevingen, maar kan niet worden gegarandeerd. Ga naar de Azure blogbericht met een live videodemonstratie op geneste virtualisatie uitgevoerd op Azure. https://aka.ms/AzureNVblog.

### <a name="option-2-configure-nested-virtualization-manually"></a>Optie 2: Geneste virtualisatie handmatig configureren

1. Open PowerShell als beheerder op de virtuele machine van Azure. 

2. Schakel de Hyper-V-functie en beheerprogramma's.

    ```powershell
    Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
    ```

    >[!WARNING] 
    >
    >Met deze opdracht wordt opnieuw gestart met de Azure VM. Tijdens het opstarten in uw RDP-verbinding wordt verbroken.
    
3. Na het opnieuw opstarten van de Azure VM opnieuw verbinding maken met uw virtuele machine met RDP.

## <a name="set-up-internet-connectivity-for-the-guest-virtual-machine"></a>Instellen van de internet-verbindingen voor de virtuele gastmachine
Een nieuwe virtuele netwerkadapter voor de virtuele gastmachine maken en configureren van een NAT-Gateway, zodat de verbinding met Internet.

### <a name="create-a-nat-virtual-network-switch"></a>Maken van een virtuele netwerkswitch NAT

1. Open PowerShell als beheerder op de virtuele machine van Azure.
   
2. Maken van een interne switch.

    ```powershell
    New-VMSwitch -Name "InternalNATSwitch" -SwitchType Internal
    ```

3. Bekijk de eigenschappen van de switch en noteer de ifIndex voor de nieuwe adapter.

    ```powershell
    Get-NetAdapter
    ```

    ![NetAdapter](./media/virtual-machines-nested-virtualization/get-netadapter.png)

    >[!NOTE] 
    >
    >Let op de 'ifIndex' voor de virtuele switch die u zojuist hebt gemaakt.
    
4. Maak een IP-adres voor de NAT-Gateway.
    
Om de gateway configureert, moet u enkele gegevens over uw netwerk:    
  * IP-adres - het IP-adres Gateway NAT Hiermee geeft u het IPv4- of IPv6-adres gebruiken als de standaardgateway voor het subnet van het virtuele netwerk. Het generieke formulier is a.b.c.1 (bijvoorbeeld ' 192.168.0.1'). Tijdens de laatste positie hoeft niet te worden.1 dit meestal is (op basis van lengte van voorvoegsel). Normaal gesproken moet u een adresruimte van RFC 1918 particuliere netwerk. 
  * PrefixLength - de lengte van het subnetvoorvoegsel definieert de grootte van het lokale subnet (subnetmasker). De lengte van het subnetvoorvoegsel is een geheel getal tussen 0 en 32 liggen. 0 het hele internet zou toewijzen, 32 dat alleen een toegewezen IP. Algemene waarden liggen tussen 24 en 12, afhankelijk van hoeveel IP-adressen moet worden gekoppeld aan de NAT bevinden. Een algemene PrefixLength is 24--dit is een subnetmasker 255.255.255.0.
  * Interfacenummer - **ifIndex** is de interface-index van de virtuele switch in de vorige stap hebt gemaakt. 

    ```powershell
    New-NetIPAddress -IPAddress 192.168.0.1 -PrefixLength 24 -InterfaceIndex 13
    ```

### <a name="create-the-nat-network"></a>De NAT-netwerk maken

Om de gateway configureert, moet u informatie over het netwerk en NAT-Gateway:
  * Naam - dit is de naam van het NAT-netwerk. 
  * InternalIPInterfaceAddressPrefix - beschrijft het subnetvoorvoegsel NAT zowel de NAT-Gateway-IP-voorvoegsel van boven als de lengte van het subnetvoorvoegsel NAT hierboven. Het generieke formulier worden a.b.c.0/NAT lengte van voorvoegsel Subnet. 

Maak een nieuwe NAT-netwerk in PowerShell.
```powershell
New-NetNat -Name "InternalNat" -InternalIPInterfaceAddressPrefix 192.168.0.0/24
```


## <a name="create-the-guest-virtual-machine"></a>Maken van de virtuele gastmachine

1. Open Hyper-V-beheer en maak een nieuwe virtuele machine. Configureer de virtuele machine voor het gebruik van het nieuwe interne netwerk die u hebt gemaakt.
    
    ![NetworkConfig](./media/virtual-machines-nested-virtualization/configure-networking.png)
    
2. Installeer een besturingssysteem op de virtuele gastmachine.
    
    >[!NOTE] 
    >
    >U moet de installatiemedia voor een besturingssysteem te installeren op de virtuele machine. In dit geval gebruiken we Windows 10 Enterprise.

## <a name="assign-an-ip-address-to-the-guest-virtual-machine"></a>Een IP-adres toewijzen aan de virtuele gastmachine

U kunt een IP-adres toewijzen aan de virtuele gastmachine door handmatig een statisch IP-adres instellen op de virtuele gastmachine of het configureren van DHCP op de Azure-VM naar het IP-adres dynamisch toewijzen.

###  <a name="option-1-configure-dhcp-to-dynamically-assign-an-ip-address-to-the-guest-virtual-machine"></a>Optie 1: DHCP om dynamisch een IP-adres toewijzen aan de virtuele gastmachine configureren
Volg onderstaande stappen voor het configureren van DHCP op de host virtuele machine voor dynamische adrestoewijzing.

#### <a name="install-dchp-server-on-the-azure-vm"></a>DHCP-Server installeren op de Azure VM

1. Open Server Manager. Klik op het Dashboard **functies en onderdelen toevoegen**. De Wizard functies en functies toevoegen wordt weergegeven.
  
2. In de wizard op **volgende** totdat de pagina serverfuncties.
  
3. Selecteer de **DHCP-Server** selectievakje, klikt u op **onderdelen toevoegen**, en klik vervolgens op **volgende** totdat u de wizard hebt voltooid.
  
4. Klik op **Install**.

#### <a name="configure-a-new-dhcp-scope"></a>Een nieuwe DHCP-scope configureren

1. Open de DHCP-beheer.
  
2. Vouw de servernaam in het navigatiedeelvenster, met de rechtermuisknop op **IPv4**, en klik op **nieuwe Scope**. De Wizard Nieuwe Scope wordt weergegeven, klikt u op **volgende**.
  
3. Voer een naam en beschrijving voor de scope en klik op **volgende**.
  
4. Definieer een IP-adresbereik voor de DHCP-Server (bijvoorbeeld 192.168.0.100 naar 192.168.0.200).
  
5. Klik op **volgende** totdat de pagina voor de standaardgateway. Hiermee geeft u het IP-adres dat u eerder (bijvoorbeeld 192.168.0.1) hebt gemaakt als de standaardgateway.
  
6. Klik op **volgende** totdat de wizard is voltooid, klikt u vervolgens alle standaardwaarden verlaten op **voltooien**.
    
### <a name="option-2-manually-set-a-static-ip-address-on-the-guest-virtual-machine"></a>Optie 2: Stel handmatig een statisch IP-adres op de virtuele gastmachine
Als u DHCP om dynamisch een IP-adres toewijzen aan de virtuele gastmachine niet hebt geconfigureerd, volgt u deze stappen om een statisch IP-adres.

1. Open PowerShell als beheerder op de virtuele machine van Azure.

2. Met de rechtermuisknop op de virtuele gastmachine en klik op verbinding maken.

3. Meld u bij de virtuele gastmachine.

4. Open de Netwerkcentrum op de virtuele gastmachine.

5. Configureer de netwerkadapter voor een adres op binnen het bereik van de NAT-netwerk dat u in de vorige sectie hebt gemaakt.

In dit voorbeeld gebruikt u een adres in het bereik 192.168.0.0/24.

## <a name="test-connectivity-in-guest-virtual-machine"></a>Connectiviteit testen in de virtuele gastmachine

Open uw browser en Ga naar een webpagina in de virtuele gastmachine.
    ![GuestVM](./media/virtual-machines-nested-virtualization/guest-virtual-machine.png)
