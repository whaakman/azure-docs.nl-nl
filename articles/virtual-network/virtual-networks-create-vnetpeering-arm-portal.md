---
title: VNet-peering maken met behulp van de Azure Portal | Microsoft Docs
description: Informatie over het maken van een virtueel netwerk met Azure Portal in Resource Manager.
services: virtual-network
documentationcenter: ''
author: NarayanAnnamalai
manager: jefco
editor: ''
tags: azure-resource-manager

ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: narayanannamalai;annahar

---
# Een gelijkwaardig virtueel netwerk maken met Azure Portal
[!INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[!INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Volg de onderstaande stappen om VNet-peering te maken met behulp van de Azure Portal op basis van het bovenstaande scenario.

1. Navigeer via een browser naar http://portal.azure.com en log, indien nodig, in met uw Azure-account.
2. Voor het tot stand brengen van VNET-peering moet u twee koppelingen maken tussen twee VNets, één voor elke richting. U kunt de VNET-peeringkoppeling eerst maken voor VNET1 naar VNET2. Klik op de portal en klik op **Bladeren** > **kies Virtuele netwerken**
   
    ![VNet-peering maken in Azure Portal](./media/virtual-networks-create-vnetpeering-arm-portal/figure01.png)
3. In de blade Virtuele netwerken kiest u VNET1, klikt u op Peerings en klikt u vervolgens op Add
   
    ![Kies peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure02.png)
4. In de blade Add peering geeft u een peering de koppelingsnaam LinkToVnet2, kiest u het abonnement en het virtuele peernetwerk VNET2, en klikt u op OK.
   
    ![Koppeling naar VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure03.png)
5. Zodra deze VNET-peeringkoppeling is gemaakt. U ziet de status van de koppeling als volgt:
   
    ![Status van koppeling](./media/virtual-networks-create-vnetpeering-arm-portal/figure04.png)
6. Daarna maakt u de VNET-peeringkoppeling voor VNET2 naar VNET1. In de blade Virtuele netwerken kiest u VNET2, klikt u op Peerings en klikt u vervolgens op Toevoegen
   
    ![Peer van andere VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure05.png)
7. In de blade Peering toevoegen geeft u een peering de koppelingsnaam LinkToVnet1, kiest u het abonnement en het virtuele peernetwerk, en klikt u op OK.
   
    ![Tegel Virtueel netwerk aanmaken](./media/virtual-networks-create-vnetpeering-arm-portal/figure06.png)
8. Zodra deze VNET-peeringkoppeling is gemaakt. U ziet de status van de koppeling als volgt:
   
    ![Eindstatus van koppeling](./media/virtual-networks-create-vnetpeering-arm-portal/figure07.png)
9. Controleer de status voor LinkToVnet2; deze wijzigt nu ook in Verbonden.  
   
    ![Eindstatus van koppeling 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure08.png)
   
   > [!NOTE]
   > VNET-peering wordt alleen ingesteld als beide koppelingen zijn verbonden.
   > 
   > 

Er zijn enkele configureerbare eigenschappen voor elke koppeling:

| Optie | Beschrijving | Standaard |
|:--- |:--- |:--- |
| AllowVirtualNetworkAccess |Of de adresruimte van gelijkwaardige VNet moet worden opgenomen als onderdeel van de tag Virtual_network |Ja |
| AllowForwardedTraffic |Zorgt ervoor dat verkeer dat niet afkomstig is van gelijkwaardig VNet wordt geaccepteerd of verwijderd |Nee |
| AllowGatewayTransit |Hiermee kan de gelijkwaardige VNet uw VNet-gateway gebruiken |Nee |
| UseRemoteGateways |Gebruik de VNet-gateway van uw peer. De gelijkwaardige VNet moeten een geconfigureerde gateway hebben en AllowGatewayTransit moet zijn geselecteerd. U kunt deze optie niet gebruiken als u een gateway hebt geconfigureerd |Nee |

Elke koppeling in VNet-peering heeft een aantal van de hierboven genoemde eigenschappen. U kunt vanuit de portal op de VNet-peeringkoppeling klikken en de beschikbare opties wijzigen. Klik op Opslaan om de wijziging door te voeren.

[!INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

1. Navigeer via een browser naar http://portal.azure.com en log, indien nodig, in met uw Azure-account.
2. In dit voorbeeld gebruiken we twee abonnementen, A en B, en twee gebruikers, UserA en UserB met bevoegdheden in de respectieve abonnementen
3. Klik op de portal en klik op Bladeren en kies Virtuele netwerken. Klik op de VNET en klik op Toevoegen.
   
    ![Scenario 2 bladeren](./media/virtual-networks-create-vnetpeering-arm-portal/figure09.png)
4. Klik op de blade Toegang toevoegen, klik om een rol te selecteren en kies Inzender voor netwerken. Klik op Gebruikers toevoegen, typ de aanmeldnaam voor UserB en klik op OK.
   
    ![RBAC](./media/virtual-networks-create-vnetpeering-arm-portal/figure10.png)
   
    Dit is geen vereiste. Peering kan ook tot stand worden gebracht als gebruikers afzonderlijk een peeringaanvraag voor hun respectieve Vnets genereren, mits de aanvragen met elkaar overeenkomen. Door de bevoegde gebruiker van de andere VNet toe te voegen als gebruiker in de lokale VNet, kunt u de peering gemakkelijker in de portal instellen.
5. Vervolgens meld u zich aan bij de Azure Portal met UserB, die de bevoegde gebruiker is voor SubscriptionB. Volg de bovenstaande stappen om UserA toe te voegen als Network Contributor.
   
    ![RBAC2](./media/virtual-networks-create-vnetpeering-arm-portal/figure11.png)
   
   > [!NOTE]
   > U kunt zich bij beide gebruikerssessies af- en aanmelden in de browser om ervoor te zorgen dat de autorisatie is ingeschakeld.
   > 
   > 
6. Meld u bij de portal aan als UserA, ga naar de blade VNET3, klikt op Peering, schakel het selectievakje 'Ik weet mijn resource-id' in, en typ de resource-id voor VNET5 in onderstaande indeling.
   
    /subscriptions/<Subscription- ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/<VNET name>
   
    ![Resource-id](./media/virtual-networks-create-vnetpeering-arm-portal/figure12.png)
7. Meld u bij de portal aan als UserB en volg de hierboven genoemde stap om de peeringkoppeling te maken van VNET5 naar VNet3.
   
    ![Resource-id 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure13.png)
8. Peering wordt tot stand gebracht en elke virtuele machine in VNet3 kan communiceren met een virtuele machine in VNet5

[!INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Als eerste stap koppelt de VNET-peering van HubVnet naar VNET1. Houd er rekening mee dat de optie Doorgestuurd verkeer toestaan niet is geselecteerd voor de koppeling.
   
    ![Basic peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure14.png)
2. Als volgende stap kunnen peeringkoppelingen van VNET1 naar HubVnet worden gemaakt. Houd er rekening mee dat de optie Doorgestuurd verkeer toestaan is geselecteerd.
   
    ![Basic peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure15a.png)
3. Nadat peering tot stand is gebracht, kunt u verwijzen naar dit [artikel](virtual-network-create-udr-arm-ps.md) en een door de gebruiker gedefinieerde route (UDR) definiëren om VNet1-verkeer om te leiden via een virtueel apparaat om de mogelijkheden te gebruiken. Wanneer u het 'volgende hop'-adres in de route opgeeft, kunt u dit instellen op het IP-adres van het virtuele apparaat in het HubVNet van het peer-VNet

[!INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

1. Navigeer via een browser naar http://portal.azure.com en log, indien nodig, in met uw Azure-account.
2. Als u VNET-peering wilt instellen in dit scenario, moet u slechts één koppeling maken van het virtuele netwerk in Azure Resource Manager naar het virtuele netwerk in de klassieke versie. Dat wil zeggen van **VNET1** naar **VNET2**. Klik in de portal op en klik op **Bladeren** > en kies **Virtuele netwerken**
3. Kies in de blade Virtuele netwerken **VNET1**. Klik op **Peerings** en klik vervolgens op **Toevoegen**.
4. Geef uw koppeling een naam in de blade Peering toevoegen. Hier wordt deze **LinkToVNet2** genoemd. Selecteer **Klassiek** onder Peerdetails.
5. Kies vervolgens het abonnement en het virtuele peernetwerk **VNET2**. Klik vervolgens op OK.
   
    ![Vnet1 koppelen aan Vnet 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure18.png)
6. Zodra deze VNet-koppeling voor peering is gemaakt, zijn de twee virtuele netwerken gepeerd en ziet u mogelijk het volgende:
   
    ![Peeringverbinding controleren](./media/virtual-networks-create-vnetpeering-arm-portal/figure19.png)

## VNet-peering verwijderen
1. Navigeer via een browser naar http://portal.azure.com en log, indien nodig, in met uw Azure-account.
2. Ga naar de blade Virtueel netwerk, klik op Peerings, klik op de koppeling die u wilt verwijderen en klik op Verwijderen.
   
   ![Delete1](./media/virtual-networks-create-vnetpeering-arm-portal/figure15.png)
3. Als u een koppeling in VNET-peering verwijdert, verandert de verbindingsstatus van de peer in niet-verbonden.
   
    ![Delete2](./media/virtual-networks-create-vnetpeering-arm-portal/figure16.png)
4. In deze status kunt u de koppeling niet opnieuw maken totdat de verbindingsstatus van de peer wordt gewijzigd in Gestart. We raden u aan beide koppelingen te verwijderen voordat u de VNET-peering opnieuw maakt.

<!--HONumber=Sep16_HO3-->


