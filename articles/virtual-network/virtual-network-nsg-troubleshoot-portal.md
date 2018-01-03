---
title: Problemen met Netwerkbeveiligingsgroepen - Portal | Microsoft Docs
description: Informatie over het oplossen van problemen met Netwerkbeveiligingsgroepen in het Azure Resource Manager-implementatiemodel met de Azure-Portal.
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: a54feccf-0123-4e49-a743-eb8d0bdd1ebc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 92816a5b1a74be5b64d974ad9ade47374db5db10
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="troubleshoot-network-security-groups-using-the-azure-portal"></a>Netwerkbeveiligingsgroepen met de Azure Portal oplossen
> [!div class="op_single_selector"]
> * [Azure-portal](virtual-network-nsg-troubleshoot-portal.md)
> * [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

Als u Netwerkbeveiligingsgroepen (nsg's) geconfigureerd op de virtuele machine (VM) en problemen met de netwerkverbinding van de VM ondervinden, is dit artikel bevat een overzicht van diagnostische gegevens mogelijkheden voor het nsg's om op te lossen verder.

Nsg's kunnen u bepalen welke typen verkeer stromen en naar uw virtuele machines (VM's). Nsg's kunnen worden toegepast op subnetten in een Azure-netwerk (VNet), netwerkinterfaces (NIC) of beide. De effectieve regels toegepast op een NIC zijn een aggregatie van de regels die zijn opgenomen in het nsg's toegepast op een NIC en het is verbonden met subnet. Regels in deze nsg's kunnen soms met elkaar conflicteren en invloed van een virtuele machine netwerkverbinding.  

U kunt de effectieve beveiligingsregels weergeven van uw nsg's, zoals toegepast op de NIC's van de VM. Dit artikel ziet het oplossen van problemen met de virtuele machine netwerkverbinding met deze regels in het Azure Resource Manager-implementatiemodel. Als u niet bekend met concepten VNet en NSG bent, leest u de [virtueel netwerk](virtual-networks-overview.md) en [Netwerkbeveiligingsgroepen](virtual-networks-nsg.md) overzicht artikelen.

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Effectieve beveiligingsregels voor verbindingen gebruiken bij het oplossen van VM-netwerkverkeer
De volgende scenario is een voorbeeld van een veelvoorkomend verbindingsprobleem:

Een virtuele machine met de naam *VM1* maakt deel uit van een subnet met de naam *Subnet1* binnen een VNet met de naam *WestUS VNet1*. Een poging verbinding maken met de virtuele machine met RDP via TCP-poort 3389 is mislukt. Nsg's worden toegepast op beide de NIC *VM1 NIC1* en het subnet *Subnet1*. -Verkeer op TCP-poort 3389 is toegestaan in de NSG die is gekoppeld aan de netwerkinterface *VM1 NIC1*, maar TCP ping naar VM1 de poort 3389 mislukt.

Hoewel dit voorbeeld wordt de TCP-poort 3389 gebruikt, kunnen de volgende stappen uit om te bepalen van binnenkomende en uitgaande verbindingsfouten via een willekeurige poort worden gebruikt.

### <a name="vm"></a>Regels voor een effectieve beveiligingsmethode voor een virtuele machine weergeven
De volgende stappen voor het oplossen van nsg's voor een virtuele machine:

U kunt de volledige lijst van de regels voor een effectieve beveiligingsmethode weergeven op een Netwerkinterfacekaart, van de virtuele machine zelf. U kunt ook toevoegen, wijzigen en verwijderen van zowel NIC en het subnetmasker NSG-regels op de blade effectieve regels als u machtigingen hebt voor deze bewerkingen uitvoeren.

1. Meld u aan bij de Azure portal op https://portal.azure.com met een Azure-account. Uw account moet worden toegewezen aan de *Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action* bewerking voor de netwerkinterface. Zie voor meer bewerkingen toewijzen aan accounts [aangepaste rollen maken voor op rollen gebaseerd toegangsbeheer](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#actions).
2. Klik op **meer services**, klikt u vervolgens op **virtuele machines** in de lijst die wordt weergegeven.
3. Selecteer een virtuele machine om op te lossen in de lijst die wordt weergegeven en een VM-blade met opties wordt weergegeven.
4. Klik op **spoor & oplossen van problemen met** en selecteer vervolgens een veelvoorkomend probleem. In dit voorbeeld **ik kan geen verbinding maken met mijn Windows-VM** is geselecteerd. 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)
5. Stappen worden vermeld onder het probleem, zoals wordt weergegeven in de volgende afbeelding: 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)
   
    Klik op *een effectieve beveiligingsmethode groep regels* in de lijst met aanbevolen stappen.
6. De **een effectieve beveiligingsmethode regels ophalen** blade wordt weergegeven, zoals wordt weergegeven in de volgende afbeelding:
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)
   
    U ziet de volgende secties van de afbeelding:
   
   * **Bereik:** ingesteld op *VM1*, de virtuele machine in stap 3 hebt geselecteerd.
   * **Netwerkinterface:** *VM1 NIC1* is geselecteerd. Een virtuele machine kan meerdere netwerkinterfaces (NIC) hebben. Elke NIC kan unieke effectieve Beveiligingsregels hebben. Wanneer het oplossen van problemen moet u mogelijk een effectieve beveiligingsmethode regels bekijken voor elke NIC.
   * **Gekoppelde nsg's:** nsg's kunnen worden toegepast op zowel de NIC en het subnet met de NIC is verbonden. In de afbeelding is op de NIC en het subnet dat is verbonden met een NSG toegepast. U kunt klikken op de namen van de NSG regels in het nsg's rechtstreeks wijzigen.
   * **Tabblad VM1 nsg:** de lijst met regels die worden weergegeven in de afbeelding is voor de NSG naar de NIC. toegepast Verschillende standaardregels worden gemaakt door Azure wanneer er een NSG wordt gemaakt. U kunt de standaardregels niet verwijderen, maar u kunt deze overschrijft met regels met een hogere prioriteit. Lees voor meer informatie over standaardregels voor de [NSG overzicht](virtual-networks-nsg.md#default-rules) artikel.
   * **Doelkolom:** sommige regels tekst in de kolom hebben, terwijl anderen adresvoorvoegsels hebben. De tekst is de naam van de standaardlabels die worden toegepast op de beveiligingsregel wanneer deze is gemaakt. De labels zijn systeem-id's die meerdere voorvoegsels vertegenwoordigen. Selecteren van een regel met een label, zoals *AllowInternetOutBound*, geeft een lijst van de voorvoegsels in de **adres voorvoegsels** blade.
   * **Download:** de lijst met regels lang kan zijn. U kunt een CSV-bestand van de regels voor offline-analyse downloaden door te klikken op **downloaden** en opslaan van het bestand.
   * **AllowRDP** binnenkomende regel: met deze regel kunnen RDP-verbindingen naar de virtuele machine.
7. Klik op de **Subnet1 NSG** tabblad om de effectieve regels van de NSG toegepast op het subnet, zoals wordt weergegeven in de volgende afbeelding: 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)
   
    U ziet de *denyRDP* **inkomend** regel. Regels voor binnenkomende verbindingen toegepast op het subnet worden geëvalueerd vóór regels toegepast op de netwerkinterface. Omdat de regel voor weigeren is toegepast op het subnet, wordt de aanvraag voor het verbinding maken met de TCP-3389 mislukt, omdat de regel voor toestaan op de NIC nooit wordt geëvalueerd. 
   
    De *denyRDP* regel is de reden waarom de RDP-verbinding is mislukt. Verwijdert, moet het probleem oplossen.
   
   > [!NOTE]
   > Als de virtuele machine die is gekoppeld aan de NIC niet actief is is of nsg's nog niet zijn toegepast op de NIC of een subnet, worden geen regels weergegeven.
   > 
   > 
8. NSG-regels bewerken, klikt u op *Subnet1 NSG* in de **nsg's die zijn gekoppeld** sectie.
   Hiermee opent u de **Subnet1 NSG** blade. U kunt de regels voor rechtstreeks bewerken door te klikken op **inkomende beveiligingsregels**.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)
9. Na het verwijderen van de *denyRDP* binnenkomende regel in de **Subnet1 NSG** en het toevoegen van een *allowRDP* regel, de effectieve regels lijst lijkt erop in de volgende afbeelding:
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)
   
    Controleer of de TCP-poort 3389 door het openen van een RDP-verbinding naar de virtuele machine of het hulpprogramma voor het psping om open is. U meer informatie over psping om door te lezen de [psping om downloadpagina](https://technet.microsoft.com/sysinternals/psping.aspx).

### <a name="nic"></a>Regels voor een effectieve beveiligingsmethode voor een netwerkinterface weergeven
Als uw VM-netwerkverkeer wordt gebracht voor een specifieke NIC, kunt u een volledige lijst van de effectieve regels voor de Netwerkkaart op uit de context van de netwerkinterfaces weergeven door de volgende stappen:

1. Meld u aan bij de Azure portal op https://portal.azure.com.
2. Klik op **meer services**, klikt u vervolgens op **netwerkinterfaces** in de lijst die wordt weergegeven.
3. Selecteer een netwerkinterface. In de volgende afbeelding een NIC met de naam *VM1 NIC1* is geselecteerd.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)
   
    U ziet dat de **bereik** is ingesteld op de netwerkinterface die is geselecteerd. Lees voor meer informatie over de aanvullende informatie wordt weergegeven, stap 6 van de **nsg's oplossen voor een VM** sectie van dit artikel.
   
   > [!NOTE]
   > Als een NSG wordt verwijderd uit een netwerkinterface, het subnet NSG is nog steeds een effectieve op de opgegeven netwerkadapter. De uitvoer weergegeven in dit geval wordt alleen regels van het NSG-subnet. Regels worden alleen weergegeven als de NIC is gekoppeld aan een virtuele machine.
   > 
   > 
4. U kunt rechtstreeks bewerken van regels voor het nsg's die zijn gekoppeld aan een NIC en een subnet. Voor meer informatie over hoe, lees stap 8 van de **effectieve beveiligingsregels voor een virtuele machine weergeven** sectie van dit artikel.

## <a name="nsg"></a>Een effectieve beveiligingsmethode regels weergeven voor een netwerkbeveiligingsgroep (NSG)
Bij het NSG-regels wijzigen, is het raadzaam om te controleren van de impact van de regels die wordt toegevoegd aan een bepaalde virtuele machine. Vindt u een volledige lijst van de regels voor een effectieve beveiligingsmethode voor de NIC's die een bepaalde NSG wordt toegepast zonder de context van de opgegeven NSG-blade overschakelen. Voor het oplossen van effectieve regels binnen een NSG, moet u de volgende stappen uitvoeren:

1. Meld u aan bij de Azure portal op https://portal.azure.com.
2. Klik op **meer services**, klikt u vervolgens op **Netwerkbeveiligingsgroepen** in de lijst die wordt weergegeven.
3. Selecteer een NSG. Een NSG VM1 nsg met de naam is geselecteerd in de volgende afbeelding.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)
   
    U ziet de volgende secties van de vorige afbeelding:
   
   * **Bereik:** ingesteld op het NSG die is geselecteerd.
   * **Virtuele machine:** wanneer een NSG wordt toegepast op een subnet, deze wordt toegepast op alle netwerkinterfaces die zijn gekoppeld aan alle virtuele machines die zijn verbonden met het subnet. Deze lijst bevat alle virtuele machines die deze NSG wordt toegepast op. U kunt elke virtuele machine uit de lijst selecteren.
     
     > [!NOTE]
     > Als een NSG wordt toegepast met alleen een lege subnet, wordt de virtuele machines worden niet weergegeven. Als een NSG wordt toegepast op een NIC die niet gekoppeld aan een virtuele machine is, wordt die NIC's ook niet weergegeven. 
     > 
     > 
   * **Netwerkinterface:** een virtuele machine kan meerdere netwerkinterfaces hebben. U kunt een netwerkinterface gekoppeld aan de geselecteerde virtuele machine selecteren.
   * **AssociatedNSGs:** op elk gewenst moment een NIC maximaal twee effectieve NSGs, toegepast op de NIC en de andere met het subnet kan hebben. Hoewel het bereik als VM1-nsg is geselecteerd als de NIC een effectieve subnet NSG, ziet de uitvoer beide nsg's.
4. U kunt rechtstreeks bewerken van regels voor het nsg's die zijn gekoppeld aan een NIC of een subnet. Voor meer informatie over hoe, lees stap 8 van de **effectieve beveiligingsregels voor een virtuele machine weergeven** sectie van dit artikel.

Lees voor meer informatie over de aanvullende informatie wordt weergegeven, stap 6 van de **effectieve beveiligingsregels voor een virtuele machine weergeven** sectie van dit artikel.

> [!NOTE]
> Hoewel een subnet en NIC elk slechts één die NSG toegepast hebben kan, is een NSG kan worden gekoppeld aan meerdere NIC's en meerdere subnetten.
> 
> 

## <a name="considerations"></a>Overwegingen
Houd rekening met de volgende punten bij het oplossen van problemen met de netwerkconnectiviteit:

* Standaard NSG-regels wordt inkomend geblokkeerd vanaf het internet en alleen toestaan VNet binnenkomend verkeer. Regels moeten expliciet worden toegevoegd aan het toestaan van binnenkomende toegang vanaf Internet, zoals vereist.
* Als er geen NSG beveiligingsregels voor verbindingen van een virtuele machine netwerkverbinding mislukken veroorzaakt, kan het probleem worden veroorzaakt:
  * Firewall-software die in de VM-besturingssysteem worden uitgevoerd
  * Routes die zijn geconfigureerd voor virtuele apparaten of lokale verkeer. Internetverkeer kan worden omgeleid naar on-premises via geforceerde tunneling. Een RDP/SSH-verbinding via Internet met uw virtuele machine werkt mogelijk niet met deze instelling kan, afhankelijk van hoe deze verkeer in het lokale netwerkhardware worden verwerkt. Lees de [probleemoplossing Routes](virtual-network-routes-troubleshoot-powershell.md) artikel voor informatie over het analyseren van route problemen die kunnen worden belemmeren de verkeersstroom en afmelden van de virtuele machine. 
* Als u de VNets, standaard brengen hebt, wordt automatisch de tag VIRTUAL_NETWORK uitgebreid met voorvoegsels voor VNets peer is ingesteld. U vindt deze voorvoegsels in de **ExpandedAddressPrefix** lijst, los eventuele problemen die betrekking hebben op de VNet-peering connectiviteit. 
* Effectieve beveiligingsregels worden alleen weergegeven als er een NSG is gekoppeld aan de VM NIC en of het subnet is. 
* Als er geen nsg's die zijn gekoppeld aan de NIC of subnet en u een openbaar IP-adres is toegewezen aan uw virtuele machine hebt, is alle poorten zijn geopend voor inkomend en uitgaand verkeer. Als de virtuele machine een openbaar IP-adres heeft, wordt nsg's toepassen op de NIC of subnet sterk aanbevolen.

