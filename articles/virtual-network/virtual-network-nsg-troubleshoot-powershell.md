---
title: Problemen met Netwerkbeveiligingsgroepen - PowerShell | Microsoft Docs
description: Informatie over het oplossen van problemen met Netwerkbeveiligingsgroepen in het Azure Resource Manager-implementatiemodel met Azure PowerShell.
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 4c732bb7-5cb1-40af-9e6d-a2a307c2a9c4
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 5edaf7197576ac1c0bd1fc6bed21fd65ed135106
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Problemen met Netwerkbeveiligingsgroepen met Azure PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
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

## <a name="detailed-troubleshooting-steps"></a>Gedetailleerde stappen voor probleemoplossing
De volgende stappen voor het oplossen van nsg's voor een virtuele machine:

1. Start een Azure PowerShell-sessie en meld u aan bij Azure. Als u niet bekend bent met behulp van Azure PowerShell, leest u de [installeren en configureren van Azure PowerShell](/powershell/azure/overview) artikel.
2. Voer de volgende opdracht om te retourneren van alle NSG-regels toegepast op een NIC met de naam *VM1 NIC1* in de resourcegroep *RG1*:
   
        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > Als u de naam van een NIC niet weet, voert u de volgende opdracht voor het ophalen van de namen van alle NIC's in een resourcegroep: 
   > 
   > `Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`
   > 
   > 
   
    De volgende tekst is een voorbeeld van de effectieve regels-uitvoer geretourneerd voor de *VM1 NIC1* NIC:
   
        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
   
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]
   
    Houd rekening met de volgende informatie in de uitvoer:
   
   * Er zijn twee **NetworkSecurityGroup** secties: een is gekoppeld aan een subnet (*Subnet1*) en een is gekoppeld aan een NIC (*VM1 NIC1*). In dit voorbeeld is elk een NSG toegepast.
   * **Koppeling** wordt de resource (subnet of NIC) in een opgegeven NSG is gekoppeld. Als het NSG-resource verplaatst/ontkoppeld is onmiddellijk voordat u deze opdracht uitvoert, moet u wellicht wacht een paar seconden om de wijziging door te geven in de opdrachtuitvoer. 
   * De namen van de regel die worden voorafgegaan door *defaultSecurityRules*: wanneer een NSG is gemaakt, verschillende standaard beveiligingsregels worden gemaakt binnen deze. Standaardregels kunnen niet worden verwijderd, maar kunnen ze met hogere prioriteitregels worden overschreven.
     Lees de [NSG overzicht](virtual-networks-nsg.md#default-rules) artikel voor meer informatie over het NSG standaard beveiligingsregels voor verbindingen.
   * **ExpandedAddressPrefix** de adresvoorvoegsels voor standaardtags NSG wordt uitgebreid. Labels vertegenwoordigen meerdere adresvoorvoegsels. Uitbreiding van de labels kan nuttig zijn bij het oplossen van VM-connectiviteit van specifieke adresvoorvoegsels. Bijvoorbeeld, met een VNET-peering, VIRTUAL_NETWORK tag uitgebreid VNet-voorvoegsels peer is ingesteld in de vorige uitvoer weergegeven.
     
     > [!NOTE]
     > De opdracht alleen toont effectieve regels als een NSG gekoppeld aan een subnet, een NIC of beide is. Een virtuele machine mogelijk meerdere NIC's met verschillende nsg's die zijn toegepast. Bij het oplossen, voert u de opdracht voor elke NIC.
     > 
     > 
3. Om te vereenvoudigen via een groter aantal NSG-regels filteren, voert u de volgende opdrachten probleem verder oplossen: 
   
        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView
   
    Een filter voor RDP-verkeer (TCP-poort 3389), wordt toegepast op de rasterweergave, zoals wordt weergegeven in de volgende afbeelding:
   
    ![Lijst met regels](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
4. Zoals u in de rasterweergave ziet, er zijn beide toestaan en weigeren voor regels voor RDP. De uitvoer van stap 2 ziet u dat de *DenyRDP* regel in de NSG toegepast op het subnet is. Voor binnenkomende regels worden toegepast op het subnet nsg's eerst verwerkt. Als een overeenkomst wordt gevonden, wordt het NSG wordt toegepast op de netwerkinterface is niet verwerkt. In dit geval de *DenyRDP* regel van het subnet blokkeert RDP naar de virtuele machine (**VM1**).
   
   > [!NOTE]
   > Een virtuele machine mogelijk meerdere NIC's gekoppeld zijn. Elk mogen worden verbonden met een ander subnet. Omdat de opdrachten in de vorige stappen worden uitgevoerd op basis van een NIC, is het belangrijk om ervoor te zorgen dat u opgeeft dat de NIC die je de verbinding is mislukt hebt. Als u niet zeker weet, kunt u de opdrachten kunt altijd uitvoeren op elke NIC die is gekoppeld aan de virtuele machine.
   > 
   > 
5. Voor RDP in VM1, wijzig de *weigeren RDP (3389)* regel naar *toestaan RDP(3389)* in de **Subnet1 NSG** NSG. Controleer of de TCP-poort 3389 door het openen van een RDP-verbinding naar de virtuele machine of het hulpprogramma voor het psping om open is. U meer informatie over psping om door te lezen de [psping om downloadpagina.](https://technet.microsoft.com/sysinternals/psping.aspx)
   
    U kunt regels of verwijderen uit een NSG met behulp van de informatie in de uitvoer van de volgende opdracht:
   
        Get-Help *-AzureRmNetworkSecurityRuleConfig

## <a name="considerations"></a>Overwegingen
Houd rekening met de volgende punten bij het oplossen van problemen met de netwerkconnectiviteit:

* Standaard NSG-regels wordt inkomend geblokkeerd vanaf het internet en alleen toestaan VNet binnenkomend verkeer. Regels moeten expliciet worden toegevoegd aan het toestaan van binnenkomende toegang vanaf Internet, zoals vereist.
* Als er geen NSG beveiligingsregels voor verbindingen van een virtuele machine netwerkverbinding mislukken veroorzaakt, kan het probleem worden veroorzaakt:
  * Firewall-software die in de VM-besturingssysteem worden uitgevoerd
  * Routes die zijn geconfigureerd voor virtuele apparaten of lokale verkeer. Internetverkeer kan worden omgeleid naar on-premises via geforceerde tunneling. Een RDP/SSH-verbinding via Internet met uw virtuele machine werkt mogelijk niet met deze instelling kan, afhankelijk van hoe deze verkeer in het lokale netwerkhardware worden verwerkt. Lees de [probleemoplossing Routes](virtual-network-routes-troubleshoot-powershell.md) artikel voor informatie over het analyseren van route problemen die kunnen worden belemmeren de verkeersstroom en afmelden van de virtuele machine. 
* Als u de VNets, standaard brengen hebt, wordt automatisch de tag VIRTUAL_NETWORK uitgebreid met voorvoegsels voor VNets peer is ingesteld. U vindt deze voorvoegsels in de **ExpandedAddressPrefix** lijst, los eventuele problemen die betrekking hebben op de VNet-peering connectiviteit. 
* Effectieve beveiligingsregels worden alleen weergegeven als er een NSG is gekoppeld aan de VM NIC en of het subnet is. 
* Als er geen nsg's die zijn gekoppeld aan de NIC of subnet en u een openbaar IP-adres is toegewezen aan uw virtuele machine hebt, is alle poorten zijn geopend voor inkomend en uitgaand verkeer. Als de virtuele machine een openbaar IP-adres heeft, wordt nsg's toepassen op de NIC of subnet sterk aanbevolen.  

