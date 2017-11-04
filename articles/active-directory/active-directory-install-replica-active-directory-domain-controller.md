---
title: Een replica Active Directory-domeincontroller installeren in Azure | Microsoft Docs
description: Een zelfstudie waarin wordt uitgelegd hoe u een domeincontroller installeren vanaf een lokale Active Directory-forest op een virtuele machine van Azure.
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 8c9ebf1b-289a-4dd6-9567-a946450005c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: fb0bacac346445e6bde9df22f3355419e3162a3c
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Een replica Active Directory-domeincontroller installeren in een Azure-netwerk
Dit onderwerp leest het installeren van extra domeincontrollers (ook wel bekend als replica DC's) voor de lokale Active Directory-domein op Azure virtuele machines (VM's) in een Azure-netwerk.

> [!IMPORTANT]
> Microsoft raadt u aan Azure AD te beheren met het [Azure AD-beheercentrum](https://aad.portal.azure.com) in Azure Portal in plaats van de klassieke Azure portal waarnaar in dit artikel wordt verwezen.

U is mogelijk ook geïnteresseerd in de volgende Verwante onderwerpen:

* U kunt desgewenst een nieuw Active Directory-forest installeren op een virtuele Azure-netwerk. Zie voor deze stappen [een nieuw Active Directory-forest installeren op een virtuele Azure-netwerk](active-directory-new-forest-virtual-machine.md).
* Raadpleeg voor algemene richtlijnen over het installeren van Active Directory Domain Services (AD DS) op een virtuele Azure-netwerk [richtlijnen voor het implementeren van Windows Server Active Directory op Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Scenario-diagram
In dit scenario moeten externe gebruikers toegang krijgen tot toepassingen die worden uitgevoerd op servers die lid zijn van een domein. De virtuele machines die de toepassingsservers worden uitgevoerd en de replica-DC's zijn geïnstalleerd in een Azure-netwerk. Het virtuele netwerk kan worden verbonden met de on-premises netwerk door een [site-naar-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) verbinding, zoals wordt weergegeven in het volgende diagram, of u kunt gebruiken [ExpressRoute](../expressroute/expressroute-locations-providers.md) voor een snellere verbinding.

De toepassingsservers en de DC's zijn geïmplementeerd in afzonderlijke cloud-services voor het distribueren van compute-verwerking en binnen [beschikbaarheidssets](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor verbeterde fouttolerantie.
De DC's repliceren met elkaar en met lokale DC's met behulp van Active Directory-replicatie. Er zijn geen hulpprogramma's voor de synchronisatie nodig.

![Diagram pf replica Active Directory-domeincontroller een Azure vnet][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>Een Active Directory-site voor de virtuele Azure-netwerk maken
Er is een goed idee om te maken van een site in Active Directory waaraan de netwerk-regio die overeenkomt met het virtuele netwerk vertegenwoordigt. Die helpt het optimaliseren van verificatie, replicatie en andere bewerkingen DC-locatie. De volgende stappen beschrijven het maken van een site en Zie voor meer achtergrondinformatie [toevoegen van een nieuwe Site](https://technet.microsoft.com/library/cc781496.aspx).

1. Open Active Directory-Sites en Services: **Serverbeheer** > **extra** > **Active Directory-Sites en Services**.
2. Maak een site staat voor de regio waar u een Azure-netwerk gemaakt: klik op **Sites** > **actie** > **nieuwe site** > Typ de naam van de nieuwe site, zoals Azure VS-West > een sitekoppeling selecteren > **OK**.
3. Een subnet maken en koppelen aan de nieuwe site: Dubbelklik op **Sites** > met de rechtermuisknop op **subnetten** > **nieuw subnet** > typt u het IP-adresbereik van het virtuele netwerk (zoals 10.1.0.0/16 in het diagram scenario) > Selecteer de nieuwe Azure site > **OK**.

## <a name="create-an-azure-virtual-network"></a>Een Azure-netwerk maken
1. In de [klassieke Azure-portal](https://manage.windowsazure.com), klikt u op **nieuw** > **netwerkservices** > **virtueel netwerk** > **aangepast maken** en de volgende waarden gebruiken om de wizard te voltooien.

   | Op deze wizardpagina... | Deze waarden opgeven |
   | --- | --- |
   |  **Details van virtueel netwerk** |<p>Naam: Typ een naam voor het virtuele netwerk, zoals WestUSVNet.</p><p>Regio: Kies de dichtstbijzijnde regio.</p> |
   |  **DNS- en VPN-verbinding** |<p>DNS-Servers: Geef de naam en IP-adres van een of meer lokale DNS-servers.</p><p>Connectiviteit: Selecteer **een site-naar-site-VPN configureren**.</p><p>Lokale netwerk: Geef een nieuw lokaal netwerk.</p><p>Als u ExpressRoute gebruikt in plaats van een VPN, Zie [configureren van een ExpressRoute-verbinding via een Exchange-Provider](../expressroute/expressroute-locations-providers.md).</p> |
   |  **Site-naar-site-connectiviteit** |<p>Naam: Typ een naam voor de on-premises netwerk.</p><p>VPN-apparaat IP-adres: Geef het openbare IP-adres van het apparaat die met het virtuele netwerk verbinden zullen. Het VPN-apparaat kan zich niet achter een NAT bevinden.</p><p>Adres: Geef de adresbereiken voor uw on-premises netwerk (zoals 192.168.0.0/16 in het diagram scenario).</p> |
   |  **Adresruimten voor virtueel netwerk** |<p>Adresruimte: Geef het IP-adresbereik voor virtuele machines die u wilt uitvoeren in de virtuele Azure-netwerk (zoals 10.1.0.0/16 in het diagram scenario). Dit adresbereik overlappen niet met de adresbereiken van de on-premises netwerk.</p><p>Subnetten: Geef een naam en adres voor een subnet voor de toepassingsservers (zoals front-end 10.1.1.0/24) en voor de DC's (zoals back-end 10.1.2.0/24).</p><p>Klik op **gatewaysubnet toevoegen**.</p> |
2. Vervolgens configureert u de virtuele netwerkgateway voor het maken van een beveiligde site-naar-site VPN-verbinding. Zie [configureren van een virtuele netwerkgateway](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) voor instructies.
3. Maak de site-naar-site VPN-verbinding tussen de nieuwe virtuele netwerk en een on-premises VPN-apparaat. Zie [configureren van een virtuele netwerkgateway](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) voor instructies.

## <a name="create-azure-vms-for-the-dc-roles"></a>Azure virtuele machines maken voor de DC-rollen
Herhaal de volgende stappen voor het maken van virtuele machines als host voor de rol domeincontroller naar behoefte. U moet ten minste twee virtuele DC's als u wilt mogelijk fouttolerantie en redundantie implementeren. Als u de virtuele Azure-netwerk bevat ten minste twee domeincontrollers die op dezelfde manier zijn geconfigureerd (ze zijn beide aangegeven, voer DNS-server, en geen van beide bevat FSMO-functie, enzovoort) plaatst u de virtuele machines die worden uitgevoerd die DC's in een beschikbaarheidsset voor verbeterde fouttolerantie.
Zie voor informatie over het maken van de virtuele machines met Windows PowerShell in plaats van de gebruikersinterface [gebruik Azure PowerShell om te maken en vooraf configureren van virtuele Machines op basis van Windows](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

1. In de [klassieke Azure-portal](https://manage.windowsazure.com), klikt u op **nieuw** > **Compute** > **virtuele Machine** > **uit galerie**. De volgende waarden gebruiken om de wizard te voltooien. Accepteer de standaardwaarde voor een instelling tenzij een andere waarde wordt voorgesteld of vereist.

   | Op deze wizardpagina... | Deze waarden opgeven |
   | --- | --- |
   |  **Een afbeelding kiezen** |Windows Server 2012 R2 Datacenter |
   |  **Configuratie van virtuele Machine** |<p>De naam van de virtuele Machine: Typ een naam met één label (zoals AzureDC1).</p><p>Nieuwe gebruikersnaam: Typ de naam van een gebruiker. Deze gebruiker is lid van de lokale groep Administrators op de virtuele machine. U moet deze naam voor het eerst aanmelden bij de virtuele machine. Het ingebouwde Administrator-account, werkt niet.</p><p>Nieuwe wachtwoord/bevestigen: Typ een wachtwoord</p> |
   |  **Configuratie van virtuele Machine** |<p>Cloudservice: Kies <b>Maak een nieuwe cloudservice</b> voor de eerste virtuele machine en selecteer die dezelfde cloudservicenaam-service bij het maken van meer virtuele machines die de rol domeincontroller fungeert als host.</p><p>Cloud-Service DNS-naam: Geef een globaal unieke naam</p><p>Regio/Affiniteitsgroep/virtuele netwerk: Geef de naam van het virtuele netwerk (zoals WestUSVNet).</p><p>Storage-Account: Kies <b>een automatisch gegenereerde opslagaccount gebruiken</b> voor de eerste virtuele machine en selecteer vervolgens die dezelfde opslagaccountnaam bij het maken van meer virtuele machines die de rol domeincontroller fungeert als host.</p><p>Beschikbaarheidsset: Kies <b>maken van een beschikbaarheidsset</b>.</p><p>Beschikbaarheidsset: Typ een naam voor de beschikbaarheidsset wanneer u de eerste virtuele machine maken en selecteer vervolgens dezelfde naam als u meer virtuele machines maakt.</p> |
   |  **Configuratie van virtuele Machine** |<p>Selecteer <b>de VM-Agent installeren</b> en eventuele andere uitbreidingen die u nodig hebt.</p> |
2. Een schijf koppelen aan elke virtuele machine die de DC-serverfunctie wordt uitgevoerd. De extra schijf is nodig voor het opslaan van de AD-database, logboekbestanden en SYSVOL. Geef een grootte voor de schijf (zoals 10 GB) en laat de **Host Cache voorkeur** ingesteld op **geen**. Zie voor de stappen [hoe een gegevensschijf koppelen aan een virtuele Windows-Machine](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. Nadat u eerst zich aanmeldt bij de virtuele machine, opent u **Serverbeheer** > **File and Storage Services** een volume maken op deze schijf met NTFS.
4. Reserveer een statisch IP-adres voor virtuele machines die de DC-functie wordt uitgevoerd. Als u wilt reserveren van een statisch IP-adres, download Microsoft Web Platform Installer en [Azure PowerShell installeren](/powershell/azure/overview) en voer de cmdlet Set-AzureStaticVNetIP. Bijvoorbeeld:

    ' Get-AzureVM - ServiceName AzureDC1-naam AzureDC1 | Set-AzureStaticVNetIP - IP-adres 10.0.0.4 | Update-AzureVM

Zie voor meer informatie over het instellen van een statisch IP-adres [een statische interne IP-adres configureren voor een virtuele machine](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Installatie van AD DS op virtuele machines in Azure
Aanmelden bij een virtuele machine en controleer of dat er een verbinding via de site-naar-site VPN- of ExpressRoute-verbinding tot bronnen op uw on-premises netwerk. AD DS installeert op de Azure VM's. U kunt hetzelfde proces dat u kunt een extra domeincontroller installeren op uw on-premises netwerk (gebruikersinterface, Windows PowerShell of een antwoordbestand) gebruiken. Als u AD DS installeert, moet dat u het nieuwe volume opgeven voor de locatie van de AD-database, logboekbestanden en SYSVOL. Als u een refresher op AD DS-installatie moet, Zie [Install Active Directory Domain Services (Level 100)](https://technet.microsoft.com/library/hh472162.aspx) of [een Replica Windows Server 2012-domeincontroller installeren in een bestaand domein (niveau 200)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>DNS-server voor het virtuele netwerk configureren
1. In de [Azure-portal](https://portal.azure.com), in de **zoeken bronnen** Voer *virtuele netwerken*, klikt u vervolgens op **virtuele netwerken (klassiek)** in de zoekresultaten. Klik op de naam van het virtuele netwerk en vervolgens [opnieuw configureren van de DNS-server IP-adressen voor het virtuele netwerk](../virtual-network/virtual-network-manage-network.md#dns-servers) gebruiken de statische IP-adressen toegewezen aan de replica-DC's in plaats van de IP-adressen van een lokale DNS-servers.
2. Om ervoor te zorgen dat alle replica DC virtuele machines op het virtuele netwerk zijn geconfigureerd voor het gebruik van DNS-servers op het virtuele netwerk, klikt u op **virtuele Machines**, klikt u op de statuskolom voor elke virtuele machine en klik vervolgens op **opnieuw**. Wacht totdat de virtuele machine bevat **met** voordat u probeert te melden bij deze status.

## <a name="create-vms-for-application-servers"></a>Virtuele machines voor toepassingsservers maken
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

## <a name="additional-resources"></a>Aanvullende bronnen
* [Richtlijnen voor het implementeren van Windows Server Active Directory op virtuele Machines in Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [Het bestaande lokale Hyper-V-domeincontrollers uploaden naar Azure met behulp van Azure PowerShell](http://support.microsoft.com/kb/2904015)
* [Een nieuw Active Directory-forest installeren op een virtuele Azure-netwerk](active-directory-new-forest-virtual-machine.md)
* [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* [Microsoft Azure IT Pro IaaS: grondbeginselen (01) virtuele Machine](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure IT Pro IaaS: (05) maken van virtuele netwerken en Cross-Premises connectiviteit](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Management-Cmdlets](/powershell/module/azurerm.compute/#virtual_machines)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
