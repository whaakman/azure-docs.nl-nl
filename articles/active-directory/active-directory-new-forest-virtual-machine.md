---
title: Een Active Directory-forest installeren op een virtuele Azure-netwerk | Microsoft Docs
description: Een zelfstudie waarin wordt uitgelegd hoe u een nieuw Active Directory-forest maken op een virtuele machine (VM) op een Azure Virtual Network.
services: active-directory, virtual-network
keywords: 'Active directory virtuele machine van de active directory-forest installeren, azure active directory-video ''s '
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
tags: 
ms.assetid: eb7170d0-266a-4caa-adce-1855589d65d1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/06/2017
ms.author: joflore
ms.openlocfilehash: 0a45a563d8aed45dd30cc76a13b0e197c248be84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Een nieuw Active Directory-forest installeren op een virtuele Azure-netwerk
Dit onderwerp wordt beschreven hoe u een nieuwe Windows Server Active Directory-omgeving maakt op een virtuele Azure-netwerk op een virtuele machine (VM) op een [virtuele Azure-netwerk](../virtual-network/virtual-networks-overview.md). De virtuele Azure-netwerk is in dit geval niet verbonden met een on-premises netwerk.

U is mogelijk ook geïnteresseerd in de volgende Verwante onderwerpen:

* Zie voor een video ziet u deze stappen [een nieuw Active Directory-forest installeren op een virtuele Azure-netwerk](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
* U kunt eventueel [een site-naar-site-VPN configureren](../vpn-gateway/vpn-gateway-site-to-site-create.md) en vervolgens een nieuw forest installeert of uitbreiden van een lokale forest met een Azure virtual network. Zie voor deze stappen [een Replica Active Directory-domeincontroller installeren in een Azure Virtual Network](active-directory-install-replica-active-directory-domain-controller.md).
* Raadpleeg voor algemene richtlijnen over het installeren van Active Directory Domain Services (AD DS) op een virtuele Azure-netwerk [richtlijnen voor het implementeren van Windows Server Active Directory op Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Scenario-Diagram
In dit scenario moeten externe gebruikers toegang krijgen tot toepassingen die worden uitgevoerd op servers die lid zijn van een domein. De virtuele machines die worden uitgevoerd van de toepassingsservers als de virtuele machines die worden uitgevoerd-domeincontrollers zijn geïnstalleerd in hun eigen cloudservice in een Azure-netwerk zijn geïnstalleerd. Ze zijn ook opgenomen in een beschikbaarheidsset voor verbeterde fouttolerantie.

![Active Directory-forest op een virtuele machines in Azure Virtual Network ][1] 7

## <a name="how-does-this-differ-from-on-premises"></a>Hoe verschilt dit van on-premises?
Er is niet het verschil tussen een domeincontroller installeren in Azure tegenover on-premises. De belangrijkste verschillen worden in de volgende tabel weergegeven.

| Configureren... | On-premises | Azure Virtual Network |
| --- | --- | --- |
| **IP-adres voor de domeincontroller** |Wijs statische IP-adres op de eigenschappen van netwerkadapter |Voer de cmdlet Set-AzureStaticVNetIP om een statisch IP-adres toewijzen |
| **DNS-clientresolver** |Voorkeur en alternatieve DNS-serveradres ingesteld op het netwerk eigenschappen van de netwerkadapter van de leden van een domein |DNS-serveradres ingesteld voor de eigenschappen van het virtuele netwerk |
| **Active Directory-database-opslag** |Wijzig eventueel de standaardopslaglocatie van C:\ |U moet standaardopslaglocatie van C:\ wijzigen |

## <a name="create-an-azure-virtual-network"></a>Een Azure-netwerk maken
1. Meld u aan bij de klassieke Azure-portal.
2. Een virtueel netwerk maken. Klik op **netwerken** > **een virtueel netwerk maken**. Gebruik de waarden in de volgende tabel om de wizard te voltooien.

   | Op deze wizardpagina... | Deze waarden opgeven |
   | --- | --- |
   |  **Details van virtueel netwerk** |<p>Naam: Geef een naam voor het virtuele netwerk</p><p>Regio: De dichtstbijzijnde regio kiezen</p> |
   |  **DNS- en VPN** |<p>DNS-server leeg laten</p><p>Een VPN-optie niet selecteert</p> |
   |  **Adresruimten voor virtueel netwerk** |<p>Subnetnaam: Voer een naam voor uw subnet</p><p>Eerste IP: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p> |

## <a name="create-vms-to-run-the-domain-controller-and-dns-server-roles"></a>Virtuele machines voor uitvoering van de domeincontroller en DNS-serverfuncties maken
Herhaal de volgende stappen voor het maken van virtuele machines als host voor de rol domeincontroller naar behoefte. U moet ten minste twee virtuele DC's als u wilt mogelijk fouttolerantie en redundantie implementeren. Als u de virtuele Azure-netwerk bevat ten minste twee domeincontrollers die op dezelfde manier zijn geconfigureerd (ze zijn beide aangegeven, voer DNS-server, en geen van beide bevat FSMO-functie, enzovoort) plaatst u de virtuele machines die worden uitgevoerd die DC's in een beschikbaarheidsset voor verbeterde fouttolerantie.

Zie voor informatie over het maken van de virtuele machines met Windows PowerShell in plaats van de gebruikersinterface [gebruik Azure PowerShell om te maken en vooraf configureren van virtuele Machines op basis van Windows](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

1. Klik in de klassieke portal **nieuw** > **Compute** > **virtuele Machine** > **uit galerie**. De volgende waarden gebruiken om de wizard te voltooien. Accepteer de standaardwaarde voor een instelling tenzij een andere waarde wordt voorgesteld of vereist.

   | Op deze wizardpagina... | Deze waarden opgeven |
   | --- | --- |
   |  **Een afbeelding kiezen** |Windows Server 2012 R2 Datacenter |
   |  **Configuratie van virtuele Machine** |<p>De naam van de virtuele Machine: Typ een naam met één label (zoals AzureDC1).</p><p>Nieuwe gebruikersnaam: Typ de naam van een gebruiker. Deze gebruiker is lid van de lokale groep Administrators op de virtuele machine. U moet deze naam voor het eerst aanmelden bij de virtuele machine. Het ingebouwde Administrator-account, werkt niet.</p><p>Nieuwe wachtwoord/bevestigen: Typ een wachtwoord</p> |
   |  **Configuratie van virtuele Machine** |<p>Cloudservice: Kies <b>Maak een nieuwe cloudservice</b> voor de eerste virtuele machine en selecteer die dezelfde cloudservicenaam-service bij het maken van meer virtuele machines die de rol domeincontroller fungeert als host.</p><p>Cloud-Service DNS-naam: Geef een globaal unieke naam</p><p>Regio/Affiniteitsgroep/virtuele netwerk: Geef de naam van het virtuele netwerk (zoals WestUSVNet).</p><p>Storage-Account: Kies <b>een automatisch gegenereerde opslagaccount gebruiken</b> voor de eerste virtuele machine en selecteer vervolgens die dezelfde opslagaccountnaam bij het maken van meer virtuele machines die de rol domeincontroller fungeert als host.</p><p>Beschikbaarheidsset: Kies <b>maken van een beschikbaarheidsset</b>.</p><p>Beschikbaarheidsset: Typ een naam voor de beschikbaarheidsset wanneer u de eerste virtuele machine maken en selecteer vervolgens dezelfde naam als u meer virtuele machines maakt.</p> |
   |  **Configuratie van virtuele Machine** |<p>Selecteer <b>de VM-Agent installeren</b> en eventuele andere uitbreidingen die u nodig hebt.</p> |
2. Een schijf koppelen aan elke virtuele machine die de DC-serverfunctie wordt uitgevoerd. De extra schijf is nodig voor het opslaan van de AD-database, logboekbestanden en SYSVOL. Geef een grootte voor de schijf (zoals 10 GB) en laat de **Host Cache voorkeur** ingesteld op **geen**. Zie voor de stappen [hoe een gegevensschijf koppelen aan een virtuele Windows-Machine](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. Nadat u eerst zich aanmeldt bij de virtuele machine, opent u **Serverbeheer** > **File and Storage Services** een volume maken op deze schijf met NTFS.
4. Reserveer een statisch IP-adres voor virtuele machines die de DC-functie wordt uitgevoerd. Als u wilt reserveren van een statisch IP-adres, download Microsoft Web Platform Installer en [Azure PowerShell installeren](/powershell/azure/overview) en voer de cmdlet Set-AzureStaticVNetIP. Bijvoorbeeld:

    `Get-AzureVM -ServiceName AzureDC1 -Name AzureDC1 | Set-AzureStaticVNetIP -IPAddress 10.0.0.4 | Update-AzureVM`

Zie voor meer informatie over het instellen van een statisch IP-adres [een statische interne IP-adres configureren voor een virtuele machine](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-windows-server-active-directory"></a>Windows Server Active Directory installeren
Gebruik dezelfde routine naar [installatie van AD DS](https://technet.microsoft.com/library/jj574166.aspx) dat u lokale (dat wil zeggen, kunt u de gebruikersinterface, een antwoordbestand of Windows PowerShell). U moet beheerdersreferenties voor het installeren van een nieuw forest opgeven. Wanneer u de locatie voor de Active Directory-database, logboekbestanden en SYSVOL, wijzigt u de standaardopslaglocatie van het systeemstation naar de schijf aanvullende gegevens die u hebt gekoppeld aan de virtuele machine.

Nadat de installatie van de domeincontroller is voltooid, verbinding maken met de virtuele machine opnieuw en meld u aan bij de domeincontroller. Vergeet niet om op te geven domeinreferenties.

## <a name="reset-the-dns-server-for-the-azure-virtual-network"></a>Opnieuw instellen van de DNS-server voor de virtuele Azure-netwerk
1. De instelling van de DNS-doorstuurserver op de nieuwe domeincontroller en DNS-server opnieuw.
   1. Klik in Serverbeheer op **extra** > **DNS**.
   2. In **DNS-beheer**, met de rechtermuisknop op de naam van de DNS-server en klik op **eigenschappen**.
   3. Op de **doorstuurservers** tabblad op het IP-adres van de doorstuurserver en klik **bewerken**.  Selecteer het IP-adres en klik op **verwijderen**.
   4. Klik op **OK** sluit de editor en **Ok** opnieuw te sluiten van de eigenschappen van de DNS-server.
2. De DNS-serverinstellingen voor het virtuele netwerk bijwerken.
   1. Klik op **virtuele netwerken** > Dubbelklik op het virtuele netwerk dat u hebt gemaakt > **configureren** > **DNS-servers**, typ de naam en het DIP van een van de virtuele machines die wordt uitgevoerd de domeincontroller en DNS-serverfunctie en klik op **opslaan**.
   2. Selecteer de virtuele machine en klik op **opnieuw** voor het activeren van de virtuele machine voor het configureren van instellingen DNS-omzetter met het IP-adres van de nieuwe DNS-server.

## <a name="create-vms-for-domain-members"></a>Maken van virtuele machines voor leden van een domein
1. Herhaal de volgende stappen voor het maken van virtuele machines uit te voeren als toepassingsservers. Accepteer de standaardwaarde voor een instelling tenzij een andere waarde wordt voorgesteld of vereist.

   | Op deze wizardpagina... | Deze waarden opgeven |
   | --- | --- |
   |  **Een afbeelding kiezen** |Windows Server 2012 R2 Datacenter |
   |  **Configuratie van virtuele Machine** |<p>De naam van de virtuele Machine: Typ een naam met één label (zoals AppServer1).</p><p>Nieuwe gebruikersnaam: Typ de naam van een gebruiker. Deze gebruiker is lid van de lokale groep Administrators op de virtuele machine. U moet deze naam voor het eerst aanmelden bij de virtuele machine. Het ingebouwde Administrator-account, werkt niet.</p><p>Nieuwe wachtwoord/bevestigen: Typ een wachtwoord</p> |
   |  **Configuratie van virtuele Machine** |<p>Cloudservice: Kies **Maak een nieuwe cloudservice** voor de eerste virtuele machine en selecteer de toepassing wordt die dezelfde cloudservicenaam-service bij het maken van meer virtuele machines die wordt gehost.</p><p>Cloud-Service DNS-naam: Geef een globaal unieke naam</p><p>Regio/Affiniteitsgroep/virtuele netwerk: Geef de naam van het virtuele netwerk (zoals WestUSVNet).</p><p>Storage-Account: Kies **een automatisch gegenereerde opslagaccount gebruiken** voor de eerste virtuele machine en selecteer vervolgens de toepassing wordt die dezelfde opslagaccountnaam bij het maken van meer virtuele machines die wordt gehost.</p><p>Beschikbaarheidsset: Kies **maken van een beschikbaarheidsset**.</p><p>Beschikbaarheidsset: Typ een naam voor de beschikbaarheidsset wanneer u de eerste virtuele machine maken en selecteer vervolgens dezelfde naam als u meer virtuele machines maakt.</p> |
   |  **Configuratie van virtuele Machine** |<p>Selecteer <b>de VM-Agent installeren</b> en eventuele andere uitbreidingen die u nodig hebt.</p> |
2. Nadat elke virtuele machine is ingericht, aanmelden en aan het domein toevoegt. In **Serverbeheer**, klikt u op **lokale Server** > **werkgroep** > **wijzigen...** en selecteer vervolgens **domein** en typ de naam van uw lokale domein. Geef referenties op van een domeingebruiker en start de virtuele machine voor het voltooien van het domein.

Zie voor informatie over het maken van de virtuele machines met Windows PowerShell in plaats van de gebruikersinterface [gebruik Azure PowerShell om te maken en vooraf configureren van virtuele Machines op basis van Windows](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Zie voor meer informatie over het gebruik van Windows PowerShell [aan de slag met Azure-Cmdlets](/powershell/azure/overview) en [Azure Cmdlet Reference](/powershell/azure/get-started-azureps).

## <a name="see-also"></a>Zie ook
* [Een nieuw Active Directory-forest installeren op een virtuele Azure-netwerk](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
* [Richtlijnen voor het implementeren van Windows Server Active Directory op virtuele Machines in Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [Een Site-naar-Site-VPN configureren](../vpn-gateway/vpn-gateway-site-to-site-create.md)
* [Een Replica Active Directory-domeincontroller installeren in een Azure-netwerk](active-directory-install-replica-active-directory-domain-controller.md)
* [Microsoft Azure IT Pro IaaS: grondbeginselen (01) virtuele Machine](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure IT Pro IaaS: (05) maken van virtuele netwerken en Cross-Premises connectiviteit](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Overzicht van Virtual Network](../virtual-network/virtual-networks-overview.md)
* [Azure PowerShell installeren en configureren](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure-cmdlet-naslaginformatie](/powershell/azure/get-started-azureps)
* [Azure VM statische IP-adres instellen](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
* [Vaste IP-adres toewijzen aan virtuele machine in Azure](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
* [Een nieuwe Active Directory-Forest installeren](https://technet.microsoft.com/library/jj574166.aspx)
* [Inleiding tot virtualisatie van Active Directory Domain Services (AD DS) (niveau 100)](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
