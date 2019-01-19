---
title: Verbinding maken met Azure Stack naar Azure met behulp van VPN
description: Over het verbinden van virtuele netwerken in Azure Stack met virtuele netwerken in Azure met behulp van VPN.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2018
ms.author: sethm
ms.reviewer: scottnap
ms.openlocfilehash: 6a82204e8401cbc325485d3882505395528bc52d
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2019
ms.locfileid: "54414860"
---
# <a name="connect-azure-stack-to-azure-using-vpn"></a>Verbinding maken met Azure Stack naar Azure met behulp van VPN

*Van toepassing op: Azure Stack-geïntegreerde systemen*

In dit artikel wordt beschreven hoe u een site-naar-site-VPN als u een virtueel netwerk in Azure Stack met een virtueel netwerk in Azure verbinding wilt maken.

## <a name="before-you-begin"></a>Voordat u begint

De om verbindingsconfiguratie te voltooien, moet dat u de volgende items hebt voordat u begint:

* Een Azure Stack geïntegreerde systemen (meerdere knooppunten)-implementatie die rechtstreeks is verbonden met internet. Uw externe openbare IP-adresbereik moet rechtstreeks bereikbaar is vanaf het openbare internet.
* Een geldig Azure-abonnement. Als u geen Azure-abonnement hebt, kunt u een [gratis Azure account hier](https://azure.microsoft.com/free/?b=17.06).

### <a name="vpn-connection-diagram"></a>Diagram van VPN-verbinding

De volgende afbeelding ziet u hoe de verbindingsconfiguratie eruit moet zien wanneer u klaar bent:

![Configuratie van site-naar-site VPN-verbinding](media/azure-stack-connect-vpn/image2.png)

### <a name="network-configuration-example-values"></a>Voorbeeld van configuratiewaarden voor netwerk

De netwerk-configuratietabel voorbeelden ziet u de waarden die worden gebruikt voor voorbeelden in dit artikel. U kunt deze waarden gebruiken of u kunt ze raadplegen om inzicht te krijgen in de voorbeelden in dit artikel:

|   |Azure Stack|Azure|
|---------|---------|---------|
|Naam van virtueel netwerk     |Azs-VNet|AzureVNet |
|Virtuele netwerkadresruimte |10.1.0.0/16|10.100.0.0/16|
|Subnetnaam     |FrontEnd|FrontEnd|
|Subnetadresbereik|10.1.0.0/24 |10.100.0.0/24 |
|Gatewaysubnet     |10.1.1.0/24|10.100.1.0/24|

## <a name="create-the-network-resources-in-azure"></a>De netwerkresources in Azure maken

U maakt eerst de netwerkbronnen voor Azure. De volgende instructies laten zien hoe u de resources maakt met behulp van de [Azure-portal](https://portal.azure.com/).

### <a name="create-the-virtual-network-and-virtual-machine-vm-subnet"></a>Het virtuele netwerk en subnet met virtuele machines (VM) maken

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) met behulp van uw Azure-account.
2. Selecteer in de gebruikersportal **+ een resource maken**.
3. Ga naar **Marketplace**, en selecteer vervolgens **netwerken**.
4. Selecteer **Virtueel netwerk**.
5. Gebruik de informatie uit de tabel van de configuratie van netwerk voor informatie over de waarden voor Azure **naam**, **adresruimte**, **subnetnaam**, en **subnetadres bereik**.
6. Voor **resourcegroep**, maak een nieuwe resourcegroep of, als u al een hebt, selecteert u **gebruik bestaande**.
7. Selecteer de **locatie** van uw VNet.  Als u de voorbeelden van waarden, selecteert u **VS-Oost** of gebruik een andere locatie.
8. Selecteer **Vastmaken aan dashboard**.
9. Selecteer **Maken**.

### <a name="create-the-gateway-subnet"></a>Het gatewaysubnet maken

1. Open de VM-resource die u hebt gemaakt (**AzureVNet**) vanuit het dashboard.
2. Op de **instellingen** sectie, selecteer **subnetten**.
3. Selecteer **gatewaysubnet** een gatewaysubnet toevoegen aan het virtuele netwerk.
4. De naam van het subnet is standaard ingesteld op **Gatewaysubnet**.

   >[!IMPORTANT]
   >Gatewaysubnetten zijn speciaal en hebben deze specifieke naam nodig om goed te functioneren.

5. In de **adresbereik** veld, Controleer of het adres **10.100.1.0/24**.
6. Selecteer **OK** om het gatewaysubnet te maken.

### <a name="create-the-virtual-network-gateway"></a>De gateway van het virtuele netwerk maken

1. Selecteer in de Azure portal, **+ een resource maken**.  
2. Ga naar **Marketplace**, en selecteer vervolgens **netwerken**.
3. Selecteer in de lijst van netwerkbronnen **gateway van virtueel netwerk**.
4. In **naam**, type **Azure-GW**.
5. Als u een virtueel netwerk, selecteert u **virtueel netwerk**. Selecteer vervolgens **AzureVnet** in de lijst.
6. Selecteer **Openbaar IP-adres**. Wanneer de **openbare IP-adres kiezen** sectie wordt geopend, selecteer **nieuw**.
7. In **naam**, type **Azure-GW-PiP**, en selecteer vervolgens **OK**.
8. Standaard voor **VPN-type**, **op Route gebaseerde** is geselecteerd. Houd de **op Route gebaseerde** VPN-type.
9. Controleer of **Abonnement** en **Locatie** juist zijn. U kunt de resource toe aan het dashboard vastmaken. Selecteer **Maken**.

### <a name="create-the-local-network-gateway-resource"></a>De lokale netwerkgatewayresource maken

1. Selecteer in de Azure portal, **+ een resource maken**.
2. Ga naar **Marketplace**, en selecteer vervolgens **netwerken**.
3. Selecteer in de lijst met resources **lokale netwerkgateway**.
4. In **naam**, type **Azs-GW**.
5. In **IP-adres**, typt u het openbare IP-adres van uw Azure Stack virtuele netwerkgateway die eerder is vermeld in de tabel van de configuratie van netwerk.
6. In **adresruimte**, Azure Stack, typ de **10.1.0.0/24** en **10.1.1.0/24** -adresruimte voor **AzureVNet**.
7. Controleer uw **abonnement**, **resourcegroep**, en **locatie** juist zijn, en selecteer vervolgens **maken**.

## <a name="create-the-connection"></a>De verbinding maken

1. Selecteer in de gebruikersportal **+ een resource maken**.
2. Ga naar **Marketplace**, en selecteer vervolgens **netwerken**.
3. Selecteer in de lijst met resources **verbinding**.
4. Op de **Basic** sectie instellingen voor de **verbindingstype**, kiest u **Site-naar-site (IPSec)**.
5. Selecteer de **abonnement**, **resourcegroep**, en **locatie**, en selecteer vervolgens **OK**.
6. Op de **instellingen** sectie, selecteer **gateway van virtueel netwerk**, en selecteer vervolgens **Azure-GW**.
7. Selecteer **lokale netwerkgateway**, en selecteer vervolgens **Azs-GW**.
8. In **verbindingsnaam**, type **Azure Azs**.
9. In **gedeelde sleutel (PSK)**, type **12345**en selecteer vervolgens **OK**.

   >[!NOTE]
   >Als u een andere waarde voor de gedeelde sleutel gebruiken, houd er rekening mee dat deze moet overeenkomen met de waarde voor de gedeelde sleutel die u op de andere kant van de verbinding maakt.

10. Controleer de **samenvatting** uit en selecteer vervolgens **OK**.

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Nu een virtuele machine maken in Azure en deze op uw VM-subnet in uw virtuele netwerk te plaatsen.

1. Selecteer in de Azure portal, **+ een resource maken**.
2. Ga naar **Marketplace**, en selecteer vervolgens **Compute**.
3. Selecteer in de lijst met installatiekopieën van virtuele machines, de **Windows Server 2016 Datacenter Eval** installatiekopie.
4. Op de **basisbeginselen** sectie voor **naam**, type **AzureVM**.
5. Typ een geldige gebruikersnaam en wachtwoord. Dit account kunt u zich aanmeldt bij de virtuele machine nadat deze is gemaakt.
6. Geef een **abonnement**, **resourcegroep**, en **locatie**, en selecteer vervolgens **OK**.
7. Op de **grootte** sectie, selecteer een VM-grootte voor dit exemplaar en selecteer vervolgens **Selecteer**.
8. In de **instellingen** sectie, kunt u de standaardinstellingen. Voordat u **OK**, controleert u of:

   * De **AzureVnet** virtueel netwerk is geselecteerd.
   * Het subnet is ingesteld op **10.100.0.0/24**.

   Selecteer **OK**.

9. Controleer de instellingen op de **samenvatting** uit en selecteer vervolgens **OK**.

## <a name="create-the-network-resources-in-azure-stack"></a>De netwerkresources maken in Azure Stack

Maak vervolgens de netwerkresources in Azure Stack.

### <a name="sign-in-as-a-user"></a>Meld u aan als een gebruiker

Een servicebeheerder kan zich aanmelden als een gebruiker voor het testen van het plannen, aanbiedingen en abonnementen die hun gebruikers mogelijk gebruiken. Als u er nog geen hebt, [maken van een gebruikersaccount](../azure-stack-add-new-user-aad.md) voordat u zich aanmeldt.

### <a name="create-the-virtual-network-and-a-vm-subnet"></a>Het virtuele netwerk en een VM-subnet maken

1. Een gebruikersaccount zich aanmeldt bij de gebruikersportal gebruiken.
2. Selecteer in de gebruikersportal **+ een resource maken**.

    ![Nieuw virtueel netwerk maken](media/azure-stack-connect-vpn/image3.png)

3. Ga naar **Marketplace**, en selecteer vervolgens **netwerken**.
4. Selecteer **Virtueel netwerk**.
5. Voor **naam**, **adresruimte**, **subnetnaam**, en **subnetadresbereik**, gebruikt u de waarden uit de tabel van de configuratie van netwerk.
6. In **abonnement**, het abonnement dat u eerder hebt gemaakt, wordt weergegeven.
7. Voor **resourcegroep**, kunt u een resourcegroep maken of als u nog niet hebt, selecteert u **gebruik bestaande**.
8. Controleer de standaardlocatie.
9. Selecteer **Vastmaken aan dashboard**.
10. Selecteer **Maken**.

### <a name="create-the-gateway-subnet"></a>Her gatewaysubnet maken

1. Open de virtueelnetwerkresource Azs-VNet, die u hebt gemaakt op het dashboard.
2. Op de **instellingen** sectie, selecteer **subnetten**.
3. Selecteer om een gatewaysubnet toe aan het virtuele netwerk **Gatewaysubnet**.

    ![Gatewaysubnet toevoegen](media/azure-stack-connect-vpn/image4.png)

4. Naam van het subnet is standaard ingesteld op **GatewaySubnet**. Voor de gateway-subnetten te laten functioneren, ze moeten gebruiken de **GatewaySubnet** naam.
5. In **adresbereik**, Controleer of het adres **10.1.1.0/24**.
6. Selecteer **OK** om het gatewaysubnet te maken.

### <a name="create-the-virtual-network-gateway"></a>De gateway van het virtuele netwerk maken

1. Selecteer in de Azure Stack-portal, **+ een resource maken**.
2. Ga naar **Marketplace**, en selecteer vervolgens **netwerken**.
3. Selecteer in de lijst van netwerkbronnen **gateway van virtueel netwerk**.
4. In **naam**, type **Azs-GW**.
5. Selecteer de **virtueel netwerk** item naar een virtueel netwerk kiezen. Selecteer **Azs-VNet** in de lijst.
6. Selecteer de **openbaar IP-adres** menu-item. Wanneer de **openbare IP-adres kiezen** sectie wordt geopend, selecteer **nieuw**.
7. In **naam**, type **Azs-GW-PiP**, en selecteer vervolgens **OK**.
8. Standaard **op Route gebaseerde** is geselecteerd voor **VPN-type**. Houd de **op Route gebaseerde** VPN-type.
9. Controleer of **Abonnement** en **Locatie** juist zijn. U kunt de resource toe aan het dashboard vastmaken. Selecteer **Maken**.

### <a name="create-the-local-network-gateway"></a>De lokale netwerkgateway maken

Het concept van een *lokale netwerkgateway* in Azure Stack is iets anders dan in een Azure-implementatie.

In een Azure-implementatie vertegenwoordigt een lokale netwerkgateway een on-premises (op de locatie van de gebruiker) fysiek apparaat waarmee u verbinding met een virtuele netwerkgateway in Azure maken. In Azure Stack zijn beide uiteinden van de verbinding virtuele netwerkgateways.

Een algemene beschrijving is dat de lokale netwerkgatewayresource altijd geeft aan de RAS-gateway op de andere kant van de verbinding dat.

### <a name="create-the-local-network-gateway-resource"></a>De lokale netwerkgatewayresource maken

1. Aanmelden bij de Azure Stack-portal.
2. Selecteer in de gebruikersportal **+ een resource maken**.
3. Ga naar **Marketplace**, en selecteer vervolgens **netwerken**.
4. Selecteer in de lijst met resources **lokale netwerkgateway**.
5. In **naam**, type **Azure-GW**.
6. In **IP-adres**, typt u het openbare IP-adres voor de virtuele netwerkgateway in Azure **Azure-GW-PiP**. Dit adres wordt eerder in de tabel van de configuratie van netwerk weergegeven.
7. In **adresruimte**, typt u de adresruimte van het Azure-VNET die u hebt gemaakt, **10.100.0.0/24** en **10.100.1.0/24**.
8. Controleer uw **abonnement**, **resourcegroep**, en **locatie** waarden correct zijn, en selecteer vervolgens **maken**.

### <a name="create-the-connection"></a>De verbinding maken

1. Selecteer in de gebruikersportal **+ een resource maken**.
2. Ga naar **Marketplace**, en selecteer vervolgens **netwerken**.
3. Selecteer in de lijst met resources **verbinding**.
4. Op de **basisbeginselen** sectie instellingen voor de **verbindingstype**, selecteer **Site-naar-site (IPSec)**.
5. Selecteer de **abonnement**, **resourcegroep**, en **locatie**, en selecteer vervolgens **OK**.
6. Op de **instellingen** sectie, selecteer **gateway van virtueel netwerk**, en selecteer vervolgens **Azs-GW**.
7. Selecteer **lokale netwerkgateway**, en selecteer vervolgens **Azure-GW**.
8. In **verbindingsnaam**, type **Azs-Azure**.
9. In **gedeelde sleutel (PSK)**, type **12345**, en selecteer vervolgens **OK**.
10. Op de **samenvatting** sectie, selecteer **OK**.

### <a name="create-a-virtual-machine-vm"></a>Een virtuele machine (VM) maken

Om te controleren of de VPN-verbinding, twee virtuele machines maken: één in Azure en één in Azure Stack. Nadat u deze VM's gemaakt, kunt u ze kunt gebruiken voor het verzenden en ontvangen van gegevens via de VPN-tunnel.

1. Selecteer in de Azure portal, **+ een resource maken**.
2. Ga naar **Marketplace**, en selecteer vervolgens **Compute**.
3. Selecteer in de lijst met installatiekopieën van virtuele machines, de **Windows Server 2016 Datacenter Eval** installatiekopie.
4. Op de **basisbeginselen** in sectie **naam**, type **Azs-VM**.
5. Typ een geldige gebruikersnaam en wachtwoord. Dit account kunt u zich aanmeldt bij de virtuele machine nadat deze is gemaakt.
6. Geef een **abonnement**, **resourcegroep**, en **locatie**, en selecteer vervolgens **OK**.
7. Op de **grootte** sectie voor dit exemplaar, selecteer de grootte van een virtuele machine, en selecteer vervolgens **Selecteer**.
8. Op de **instellingen** sectie, accepteer de standaardwaarden. Zorg ervoor dat de **Azs-VNet** virtueel netwerk is geselecteerd. Controleren of het subnet is ingesteld op **10.1.0.0/24**. Selecteer vervolgens **OK**.
9. Op de **samenvatting** sectie, Controleer de instellingen en selecteer vervolgens * OK **.

## <a name="test-the-connection"></a>De verbinding testen

Nadat de site-naar-site-verbinding tot stand is gebracht, moet u controleren of dat u de gegevens die binnenkomen in beide richtingen kunt krijgen. De eenvoudigste manier voor het testen van de verbinding is aan de hand van een ping-test:

* Meld u aan de virtuele machine die u hebt gemaakt in Azure Stack en ping de virtuele machine in Azure.
* Meld u aan de virtuele machine die u hebt gemaakt in Azure en de virtuele machine in Azure Stack pingen.

>[!NOTE]
>Om ervoor te zorgen dat u verkeer via de site-naar-site-verbinding verzendt, pingt u het adres van het Direct IP (DIP) van de virtuele machine op het externe subnet, niet het VIP.

### <a name="sign-in-to-the-user-vm-in-azure-stack"></a>Aanmelden bij de gebruikers-VM in Azure Stack

1. Aanmelden bij de Azure Stack-portal.
2. Selecteer in de linker navigatiebalk **virtuele Machines**.
3. Zoek in de lijst met virtuele machines, **Azs-VM** die u eerder hebt gemaakt en selecteer dit.
4. Selecteer in de sectie voor de virtuele machine, **Connect**, en open vervolgens het bestand Azs-VM.rdp.

     ![De knop verbinding maken](media/azure-stack-connect-vpn/image17.png)

5. Meld u aan met het account die u hebt geconfigureerd tijdens het maken van de virtuele machine.
6. Open een verhoogde Windows PowerShell-prompt.
7. Typ **ipconfig /all**.
8. Zoek in de uitvoer de **IPv4-adres**, en sla het adres voor later gebruik. Dit is het adres dat u de opdracht ping van Azure. In de voorbeeldomgeving, het adres is **10.1.0.4**, maar in uw omgeving mogelijk andere. Het moet vallen binnen de **10.1.0.0/24** subnet dat u eerder hebt gemaakt.
9. Voer de volgende PowerShell-opdracht voor het maken van een firewallregel waarmee de virtuele machine op pings reageren:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

### <a name="sign-in-to-the-tenant-vm-in-azure"></a>Meld u aan bij de tenant-VM in Azure

1. Meld u aan bij Azure Portal.
2. Selecteer in de linker navigatiebalk **virtuele Machines**.
3. Zoek in de lijst met virtuele machines, **Azure-VM** die u eerder hebt gemaakt en selecteer dit.
4. Selecteer in de sectie voor de virtuele machine, **Connect**.
5. Meld u aan met het account die u hebt geconfigureerd tijdens het maken van de virtuele machine.
6. Open een opdrachtprompt met verhoogde bevoegdheid **Windows PowerShell** venster.
7. Typ **ipconfig /all**.
8. Er is een IPv4-adres dat binnen **10.100.0.0/24**. In de voorbeeldomgeving, het adres is **10.100.0.4**, maar uw adres kan afwijken.
9. Voer de volgende PowerShell-opdracht voor het maken van een firewallregel waarmee de virtuele machine op pings reageren:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

10. Ping van de virtuele machine in Azure, de virtuele machine in Azure Stack, via de tunnel. U doet dit door u het DIP pingen dat u hebt genoteerd bij Azs-VM. In de voorbeeldomgeving is dit **10.1.0.4**, maar zorg ervoor dat het adres dat u hebt genoteerd in uw testomgeving pingen. U ziet een resultaat dat lijkt op de volgende schermafbeelding:

    ![Geslaagde ping](media/azure-stack-connect-vpn/image19b.png)

11. Een reactie van de externe virtuele machine geeft aan dat de test is geslaagd. U kunt het venster van de virtuele machine sluiten.

U moet ook doen strengere gegevensoverdracht testen; kopiëren van verschillende grootte bijvoorbeeld bestanden in beide richtingen.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Statistieken van gegevensoverdracht via de gatewayverbinding weergeven

Als u weten hoeveel gegevens wordt doorgegeven via de site-naar-site-verbinding wilt, deze informatie is beschikbaar in de **verbinding** sectie. Deze test is ook een andere manier om te verifiëren dat het pingen die u zojuist hebt verzonden daadwerkelijk is verkeerd via de VPN-verbinding.

1. Terwijl u aangemeld bij de gebruiker virtuele machine in Azure Stack, gebruikt u uw gebruikersaccount aanmelden bij de gebruikersportal.
2. Ga naar **alle resources**, en selecteer vervolgens de **Azs-Azure** verbinding. **Verbindingen** wordt weergegeven.
3. Op de **verbinding** sectie, de statistieken voor **gegevens in** en **uitgaande gegevens** worden weergegeven. In de volgende schermopname, worden de grote getallen toegeschreven aan extra bestandsoverdracht. U ziet er enkele andere waarden.

    ![Inkomende/uitgaande gegevens](media/azure-stack-connect-vpn/Connection.png)

## <a name="next-steps"></a>Volgende stappen

[Apps implementeren op Azure en Azure Stack](azure-stack-solution-pipeline.md)
