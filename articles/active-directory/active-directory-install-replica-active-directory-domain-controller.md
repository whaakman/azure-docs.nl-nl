---
title: Replica van domeincontrollers voor de lokale Active Directory-domein installeren op virtuele machines in Azure | Microsoft Docs
description: Het installeren van de replica-DC's voor lokale Active Directory-domein op Azure virtuele machines (VM's) in een Azure-netwerk.
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 8c9ebf1b-289a-4dd6-9567-a946450005c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 3f7624d588e958985a73c5b40e8010e18e8879cb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Een replica Active Directory-domeincontroller installeren in een Azure-netwerk
Dit artikel wordt beschreven hoe u voor het installeren van extra domeincontrollers (DC's) moet worden gebruikt als replica DC's voor lokale Active Directory-domein op Azure virtuele machines (VM's) in een Azure-netwerk. U kunt ook [een Windows Server Active Directory-forest installeren op een virtuele Azure-netwerk](active-directory-new-forest-virtual-machine.md). Voor informatie over het installeren van Active Directory Domain Services (AD DS) op een virtuele Azure-netwerk, Zie [richtlijnen voor het implementeren van Windows Server Active Directory op Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Scenario-diagram
In dit scenario moeten externe gebruikers toegang krijgen tot toepassingen die worden uitgevoerd op servers die lid zijn van een domein. De virtuele machines die de toepassingsservers worden uitgevoerd en de replica-DC's zijn geïnstalleerd in een Azure-netwerk. Het virtuele netwerk kan worden verbonden met de on-premises netwerk door [ExpressRoute](../expressroute/expressroute-locations-providers.md), of kunt u een [site-naar-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) verbinding, zoals wordt weergegeven: 

![Diagram pf replica Active Directory-domeincontroller een Azure vnet][1]

De toepassingsservers en de DC's worden toegepast op afzonderlijke cloudservices voor het distribueren van compute-verwerking en in beschikbaarheidssets voor verbeterde fouttolerantie.
De DC's repliceren met elkaar en met lokale DC's met behulp van Active Directory-replicatie. Er zijn geen hulpprogramma's voor de synchronisatie nodig.

## <a name="create-an-on-premises-active-directory-site-for-the-azure-virtual-network"></a>Een lokale Active Directory-site voor de virtuele Azure-netwerk maken
U kunt een site in Active Directory dat Hiermee geeft u de netwerk-regio die overeenkomt met het virtuele netwerk maken. Deze site kunt u verificatie, replicatie en andere DC locatie-bewerkingen optimaliseren. De volgende stappen beschrijven het maken van een site en Zie voor meer achtergrondinformatie [toevoegen van een nieuwe Site](https://technet.microsoft.com/library/cc781496.aspx).

1. Open Active Directory-Sites en Services: **Serverbeheer** > **extra** > **Active Directory-Sites en Services**.
2. Maak een site staat voor de regio waar u een Azure-netwerk gemaakt: klik op **Sites** > **actie** > **nieuwe site** > Typ de naam van de nieuwe site, zoals Azure VS-West > een sitekoppeling selecteren > **OK**.
3. Een subnet maken en koppelen aan de nieuwe site: Dubbelklik op **Sites** > met de rechtermuisknop op **subnetten** > **nieuw subnet** > typt u het IP-adresbereik van het virtuele netwerk (zoals 10.1.0.0/16 in het diagram scenario) > Selecteer de nieuwe Azure site > **OK**.

## <a name="create-an-azure-virtual-network"></a>Een Azure-netwerk maken
Volg de stappen in het artikel wilt maken van een virtuele Azure-netwerk en site-naar-site VPN hebt ingesteld, [maken van een Site-naar-Site-verbinding](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md). 

U kunt ook de virtuele netwerkgateway voor het maken van een beveiligde site-naar-site VPN-verbinding configureren. Maak de site-naar-site VPN-verbinding tussen de nieuwe virtuele netwerk en een on-premises VPN-apparaat. Zie voor instructies [configureren van een virtuele netwerkgateway](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md).

## <a name="create-azure-vms-for-the-dc-roles"></a>Azure virtuele machines maken voor de DC-rollen
Herhaal de stappen in voor het maken van virtuele machines als host voor de rol domeincontroller [virtuele Windows-machine maken met de Azure portal](../virtual-machines/windows/quick-create-portal.md) indien nodig. Ten minste twee virtuele DC's als u wilt mogelijk fouttolerantie en redundantie implementeren. Als de virtuele Azure-netwerk ten minste twee domeincontrollers die op dezelfde manier zijn geconfigureerd heeft, kunt u de virtuele machines die deze DC's worden uitgevoerd in een beschikbaarheidsset voor verbeterde fouttolerantie kunt plaatsen.

Zie voor informatie over het maken van de virtuele machines met Windows PowerShell in plaats van de Azure-portal [gebruik Azure PowerShell om te maken en vooraf configureren van virtuele Machines op basis van Windows](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Reserveer een statisch IP-adres voor virtuele machines die de DC-functie wordt uitgevoerd. Als u wilt reserveren van een statisch IP-adres, download Microsoft Web Platform Installer en [Azure PowerShell installeren](/powershell/azure/overview) en voer de cmdlet Set-AzureStaticVNetIP. Bijvoorbeeld:

````
Get-AzureVM -ServiceName AzureDC1 -Name AzureDC1 | Set-AzureStaticVNetIP -IPAddress 10.0.0.4 | Update-AzureVM
````
Zie voor meer informatie over het instellen van een statisch IP-adres [een statische interne IP-adres configureren voor een virtuele machine](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Installatie van AD DS op virtuele machines in Azure
Aanmelden bij een virtuele machine en controleer of dat er een verbinding via de site-naar-site VPN- of ExpressRoute-verbinding tot bronnen op uw on-premises netwerk. AD DS installeert op de Azure VM's. U kunt hetzelfde proces dat u kunt een extra domeincontroller installeren op uw on-premises netwerk (gebruikersinterface, Windows PowerShell of een antwoordbestand) gebruiken. Als u AD DS installeert, moet dat u het nieuwe volume opgeven voor de locatie van de AD-database, logboekbestanden en SYSVOL. Als u een refresher op AD DS-installatie moet, Zie [Install Active Directory Domain Services (Level 100)](https://technet.microsoft.com/library/hh472162.aspx) of [een Replica Windows Server 2012-domeincontroller installeren in een bestaand domein (niveau 200)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>DNS-server voor het virtuele netwerk configureren
1. Voor een lijst met namen van virtueel netwerk in de [Azure-portal](https://portal.azure.com), zoeken naar *virtuele netwerken*, selecteer daarna **virtuele netwerken** om de lijst weer te geven. 
2. Open het virtuele netwerk dat u beheren wilt, en vervolgens [opnieuw configureren van de DNS-server IP-adressen voor het virtuele netwerk](../virtual-network/virtual-network-manage-network.md#dns-servers) gebruiken de statische IP-adressen toegewezen aan de replica-DC's in plaats van de IP-adressen voor on-premises DNS-servers.
3. Het virtuele netwerk zijn om te controleren of alle replica DC VM's op voor het gebruik van DNS-servers op het virtuele netwerk geconfigureerd met:
  1. Selecteer **virtuele Machines**.
  2. Selecteer de virtuele machines, en selecteer vervolgens **opnieuw**. 
  3. Wacht totdat de virtuele machine is **met** opnieuw en meld u vervolgens in de App.

## <a name="create-vms-for-application-servers"></a>Virtuele machines voor toepassingsservers maken

Herhaal de stappen in voor het maken van virtuele machines als host voor de rol toepassingsserver [virtuele Windows-machine maken met de Azure portal](../virtual-machines/windows/quick-create-portal.md) indien nodig. Zie voor informatie over het maken van de virtuele machines met behulp van Microsoft PowerShell in plaats van de Azure-portal [gebruik Azure PowerShell maken en configureren van virtuele Machines op basis van Windows](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). De volgende tabel bevat de aanbevolen instellingen.

| Instelling | Waarden |
| --- | --- |
|  **Een afbeelding kiezen** | Windows Server 2012 R2 Datacenter |
|  **Configuratie van virtuele Machine** |<p>De naam van de virtuele Machine: Typ een naam met één label (zoals AppServer1).</p><p>Nieuwe gebruikersnaam: Typ de naam van een gebruiker. Deze gebruiker is lid van de lokale groep Administrators op de virtuele machine. U moet deze naam voor het eerst aanmelden bij de virtuele machine. Het ingebouwde Administrator-account, werkt niet.</p><p>Nieuwe wachtwoord/bevestigen: Typ een wachtwoord</p> |
|  **Configuratie van virtuele Machine** |<p>Cloudservice: Kies **Maak een nieuwe cloudservice** voor de eerste virtuele machine en selecteer de toepassing wordt die dezelfde cloudservicenaam-service bij het maken van meer virtuele machines die wordt gehost.</p><p>Cloud-Service DNS-naam: Geef een globaal unieke naam</p><p>Regio/Affiniteitsgroep/virtuele netwerk: Geef de naam van het virtuele netwerk (zoals WestUSVNet).</p><p>Storage-Account: Kies **een automatisch gegenereerde opslagaccount gebruiken** voor de eerste virtuele machine en selecteer vervolgens de toepassing wordt die dezelfde opslagaccountnaam bij het maken van meer virtuele machines die wordt gehost.</p><p>Beschikbaarheidsset: Kies **maken van een beschikbaarheidsset**.</p><p>Beschikbaarheidsset: Typ een naam voor de beschikbaarheidsset wanneer u de eerste virtuele machine maken en selecteer vervolgens dezelfde naam als u meer virtuele machines maakt.</p> |
|  **Configuratie van virtuele Machine** |<p>Selecteer <b>de VM-Agent installeren</b> en eventuele andere uitbreidingen die u nodig hebt.</p> |
  
Nadat elke virtuele machine is ingericht, aanmelden en aan het domein toevoegt. 
1. In **Serverbeheer** &gt; **lokale Server** &gt; **werkgroep** &gt; **wijzigen...** , selecteer **domein**.
2. Voer de naam van uw lokale domein. 
3. Geef referenties op van een domeingebruiker.
4. Start opnieuw op de virtuele machine.

## <a name="additional-resources"></a>Aanvullende bronnen

* Zie voor meer informatie over het gebruik van Windows PowerShell [aan de slag met Azure-Cmdlets](/powershell/azure/overview) en [Azure Cmdlet Reference](/powershell/azure/get-started-azureps).
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
