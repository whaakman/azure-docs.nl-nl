---
title: Verbinding maken met Azure-Stack in Azure maken met VPN
description: Het verbinding maken met virtuele netwerken in Azure-Stack van virtuele netwerken in Azure via VPN.
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: victorh
ms.openlocfilehash: c06eb0bb44bdfeab956e9b5051786b5bc631acf5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connect-azure-stack-to-azure-using-vpn"></a>Verbinding maken met Azure-Stack in Azure maken met VPN

*Van toepassing op: Azure Stack geïntegreerd systemen*

In dit artikel leest u hoe een site-naar-site VPN als u een virtueel netwerk in Azure-Stack verbinding met een virtueel netwerk in Azure wilt maken.

### <a name="connection-diagram"></a>Verbindingsdiagram
Het volgende diagram toont wat de verbindingsconfiguratie ziet er als u klaar bent:

![Configuratie van site-naar-site VPN-verbinding](media/azure-stack-connect-vpn/image2.png)

### <a name="before-you-begin"></a>Voordat u begint
Zorg dat u de volgende items hebt voordat u begint voor het voltooien van de configuratie van de verbinding:

* Een Azure-Stack geïntegreerd systemen (met meerdere knooppunten)-implementatie die direct is verbonden met Internet. Dit betekent dat uw externe openbaar IP-adresbereik rechtstreeks bereikbaar is vanaf het openbare Internet moet.
* Een geldige Azure-abonnement.  Als u geen Azure-abonnement hebt, kunt u een [gratis Azure account hier](https://azure.microsoft.com/free/?b=17.06).

## <a name="network-example-values-table"></a>Waarden voor netwerk voorbeeldtabel
De netwerk voorbeeld waardentabel toont de voorbeeldwaarden die worden gebruikt in dit artikel. U kunt deze waarden gebruiken of u kunt verwijzen naar deze beter inzicht in de voorbeelden in dit artikel.

**Waarden voor netwerk voorbeeldtabel**
|   |Azure Stack|Azure|
|---------|---------|---------|
|Virtuele-netwerknaam     |Azs-VNet|AzureVNet |
|Adresruimte voor virtueel netwerk |10.1.0.0/16|10.100.0.0/16|
|Subnetnaam     |FrontEnd|FrontEnd|
|Subnetadresbereik|10.1.0.0/24 |10.100.0.0/24 |
|Gatewaysubnet     |10.1.1.0/24|10.100.1.0/24|

## <a name="create-the-network-resources-in-azure"></a>De netwerkbronnen in Azure maken

U maakt eerst de netwerkbronnen voor Azure. De volgende instructies wordt aangegeven hoe de resources te maken met behulp van de [Azure-portal](http://portal.azure.com/).

### <a name="create-the-virtual-network-and-vm-subnet"></a>Het virtuele netwerk en VM-subnet maken

1. Aanmelden bij de [Azure-portal](http://portal.azure.com/) met behulp van uw Azure-account.
2. Selecteer in de gebruikersportal **nieuw**.
3. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
4. Selecteer **virtueel netwerk**.
5. Gebruik de informatie uit de tabel van network configuration voor het identificeren van de waarden voor Azure **naam**, **adresruimte**, **subnetnaam**, en **subnetadres bereik**.
6. Voor **resourcegroep**, een nieuwe resourcegroep maken of als u al hebt, selecteert u **gebruik bestaande**.
7. Selecteer de **locatie** van uw VNet.  Als u de voorbeeldwaarden, selecteert u **VS-Oost** of een andere locatie gebruiken als u liever.
8. Selecteer **Vastmaken aan dashboard**.
9. Selecteer **Maken**.

### <a name="create-the-gateway-subnet"></a>Het gatewaysubnet maken
1. Open de virtuele netwerk resource die u hebt gemaakt (**AzureVNet**) vanuit het dashboard.
2. Op de **instellingen** sectie **subnetten**.
3. Selecteer **gatewaysubnet** een gatewaysubnet toevoegen aan het virtuele netwerk.
4. De naam van het subnet is standaard ingesteld op **Gatewaysubnet**.
   Gatewaysubnetten zijn speciaal en hebben deze specifieke naam nodig om goed te functioneren.
5. In de **-adresbereik** veld, controleert u of het adres is **10.100.0.0/24**.
6. Selecteer **OK** het gatewaysubnet maken.

### <a name="create-the-virtual-network-gateway"></a>De gateway van het virtuele netwerk maken
1. Selecteer in de Azure-portal **nieuw**.  
2. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
3. Selecteer in de lijst van netwerkbronnen **virtuele netwerkgateway**.
4. In **naam**, type **Azure-GW**.
5. Als u een virtueel netwerk, selecteer **virtueel netwerk**. Selecteer vervolgens **AzureVnet** uit de lijst.
6. Selecteer **openbaar IP-adres**. Wanneer de **openbare IP-adres kiezen** sectie wordt geopend, selecteer **nieuw**.
7. In **naam**, type **Azure-GW-PiP**, en selecteer vervolgens **OK**.
8. Standaard voor **VPN-type**, **op Route gebaseerde** is geselecteerd.
    Houd de **op Route gebaseerde** VPN-type.
9. Controleer of **Abonnement** en **Locatie** juist zijn. U kunt de resource toe aan het dashboard vastmaken. Selecteer **Maken**.

### <a name="create-the-local-network-gateway-resource"></a>Het lokale netwerk gateway-resource maken

1. Selecteer in de Azure-portal **nieuw**. 
4. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
5. Selecteer in de lijst met resources **lokale netwerkgateway**.
6. In **naam**, type **Azs GW**.
7. In **IP-adres**, typt u het openbare IP-adres voor uw Azure-Stack virtuele netwerkgateway die eerder in de tabel van de configuratie van netwerk wordt vermeld.
8. In **adresruimte**, uit Azure Stack, typt u de **10.0.10.0/23** -adresruimte voor **AzureVNet**.
9. Controleer uw **abonnement**, **resourcegroep**, en **locatie** juist zijn en selecteer vervolgens **maken**.

## <a name="create-the-connection"></a>De verbinding maken
1. Selecteer in de gebruikersportal **nieuw**. 
2. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
3. Selecteer in de lijst met resources **verbinding**.
4. Op de **Basic** sectie instellingen voor de **verbindingstype**, kies **Site-naar-site (IPSec)**.
5. Selecteer de **abonnement**, **resourcegroep**, en **locatie**, en selecteer vervolgens **OK**.
6. Op de **instellingen** sectie **virtuele netwerkgateway**, en selecteer vervolgens **Azure-GW**.
7. Selecteer **lokale netwerkgateway**, en selecteer vervolgens **Azs GW**.
8. In **verbindingsnaam**, type **Azure Azs**.
9. In **gedeelde sleutel (PSK)**, type **12345**. Als u een andere waarde kiest, houd er rekening mee dat deze *moet* overeenkomen met de waarde voor de gedeelde sleutel die u op het andere einde van de verbinding maakt. Selecteer **OK**.
10. Controleer de **samenvatting** sectie en selecteer vervolgens **OK**.

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken
Nu een virtuele machine in Azure maken en op de VM-subnet in het virtuele netwerk plaatsen.

1. Selecteer in de Azure-portal **nieuw**.
2. Ga naar **Marketplace**, en selecteer vervolgens **Compute**.
3. Selecteer in de lijst van installatiekopieën van virtuele machines, de **Windows Server 2016 Datacenter Eval** installatiekopie.
4. Op de **basisbeginselen** sectie voor **naam**, type **AzureVM**.
5. Typ een geldige gebruikersnaam en wachtwoord. Dit account kunt u zich aanmeldt bij de virtuele machine nadat deze gemaakt.
6. Geef een **abonnement**, **resourcegroep**, en **locatie**, en selecteer vervolgens **OK**.
7. Op de **grootte** sectie, selecteer de grootte van een virtuele machine voor dit exemplaar en selecteer vervolgens **Selecteer**.
8. Op de **instellingen** sectie, kunt u de standaardinstellingen accepteren. Zorg ervoor dat de **AzureVnet** virtueel netwerk is geselecteerd en controleren of het subnet is ingesteld op **10.0.20.0/24**. Selecteer **OK**.
9. Controleer de instellingen op de **samenvatting** sectie en selecteer vervolgens **OK**.

## <a name="create-the-network-resources-in-azure-stack"></a>Maken van de netwerkbronnen in Azure-Stack
Vervolgens maakt u de netwerkbronnen in Azure-Stack.

### <a name="sign-in-as-a-user"></a>Meld u aan als een gebruiker
Een servicebeheerder kunt aanmelden als een gebruiker voor het testen van het plannen, aanbiedingen en abonnementen die gebruikers mogelijk gebruiken. Als u nog geen hebt, [maken van een gebruikersaccount](azure-stack-add-new-user-aad.md) voordat u zich aanmeldt.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Het virtuele netwerk en VM-subnet maken
1. Een gebruikersaccount aan te melden bij de gebruikersportal gebruiken.
2. Selecteer in de gebruikersportal **nieuw**.

    ![Nieuw virtueel netwerk maken](media/azure-stack-create-vpn-connection-one-node-tp2/image3.png)

3. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
4. Selecteer **virtueel netwerk**.
5. Voor **naam**, **adresruimte**, **subnetnaam**, en **adresbereik**, gebruik de waarden uit de tabel van de configuratie van netwerk.
6. In **abonnement**, het abonnement dat u eerder hebt gemaakt wordt weergegeven.
7. Voor **resourcegroep**, kunt u een resourcegroep maken of als u al hebt, selecteert u **gebruik bestaande**.
8. Controleer de standaardlocatie.
9. Selecteer **Vastmaken aan dashboard**.
10. Selecteer **Maken**.

### <a name="create-the-gateway-subnet"></a>Her gatewaysubnet maken
1. Open de Azs VNet virtueel netwerk resource die u hebt gemaakt op het dashboard.
2. Op de **instellingen** sectie **subnetten**.
3. Selecteer om een gatewaysubnet toe aan het virtuele netwerk **Gatewaysubnet**.
   
    ![Gatewaysubnet toevoegen](media/azure-stack-create-vpn-connection-one-node-tp2/image4.png)

4. De subnetnaam is standaard ingesteld op **GatewaySubnet**.
   Gateway-subnetten zijn speciale. Werkt alleen goed als ze moeten gebruiken de *GatewaySubnet* naam.
5. In **-adresbereik**, Controleer of het adres **10.1.1.0/24**.
6. Selecteer **OK** het gatewaysubnet maken.

### <a name="create-the-virtual-network-gateway"></a>De gateway van het virtuele netwerk maken
1. Selecteer in de Stack van Azure-portal **nieuw**. 
2. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
3. Selecteer in de lijst van netwerkbronnen **virtuele netwerkgateway**.
4. In **naam**, type **Azs GW**.
5. Selecteer de **virtueel netwerk** item een virtueel netwerk kiezen.
   Selecteer **Azs VNet** uit de lijst.
6. Selecteer de **openbaar IP-adres** menu-item. Wanneer de **openbare IP-adres kiezen** sectie wordt geopend, selecteer **nieuw**.
7. In **naam**, type **Azs-GW-PiP**, en selecteer vervolgens **OK**.
8.  Standaard voor **VPN-type**, **op Route gebaseerde** is geselecteerd.
    Houd de **op Route gebaseerde** VPN-type.
9. Controleer of **Abonnement** en **Locatie** juist zijn. U kunt de resource toe aan het dashboard vastmaken. Selecteer **Maken**.

### <a name="create-the-local-network-gateway"></a>De lokale netwerkgateway maken
Het principe van een *lokale netwerkgateway* in Azure-Stack is iets anders dan in een Azure-implementatie.

In een Azure-implementatie vertegenwoordigt een lokale netwerkgateway een on-premises (op de locatie van de gebruiker) fysieke apparaat, waarmee u verbinding maken met een virtuele netwerkgateway in Azure. Beide zijden van de verbinding zijn in Azure-Stack virtuele netwerkgateways!

Een manier om na te denken over deze meer algemeen is dat de lokale gateway netwerkbron wordt altijd aangegeven met de RAS-gateway aan het andere einde van de verbinding. 

### <a name="create-the-local-network-gateway-resource"></a>Het lokale netwerk gateway-resource maken
1. Meld u aan de Stack van Azure-portal.
2. Selecteer in de gebruikersportal **nieuw**.
3. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
4. Selecteer in de lijst met resources **lokale netwerkgateway**.
5. In **naam**, type **Azure-GW**.
6. In **IP-adres**, typt u het openbare IP-adres voor de virtuele netwerkgateway in Azure **Azure-GW-PiP**. Dit adres wordt eerder in de tabel van de configuratie van netwerk weergegeven.
7. In **adresruimte**, voor de adresruimte van het Azure VNET dat u hebt gemaakt, typt u **10.0.20.0/23**.
8. Controleer uw **abonnement**, **resourcegroep**, en **locatie** juist zijn en selecteer vervolgens **maken**.

### <a name="create-the-connection"></a>De verbinding maken
1. Selecteer in de gebruikersportal **nieuw**.
2. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
3. Selecteer in de lijst met resources **verbinding**.
4. Op de **basisbeginselen** sectie instellingen voor de **verbindingstype**, selecteer **Site-naar-site (IPSec)**.
5. Selecteer de **abonnement**, **resourcegroep**, en **locatie**, en selecteer vervolgens **OK**.
6. Op de **instellingen** sectie **virtuele netwerkgateway**, en selecteer vervolgens **Azs GW**.
7. Selecteer **lokale netwerkgateway**, en selecteer vervolgens **Azure-GW**.
8. In **verbindingsnaam**, type **Azs Azure**.
9. In **gedeelde sleutel (PSK)**, type **12345**, en selecteer vervolgens **OK**.
10. Op de **samenvatting** sectie **OK**.

### <a name="create-a-vm"></a>Een virtuele machine maken
Voor het valideren van de gegevens die worden uitgewisseld via de VPN-verbinding die u wilt maken van virtuele machines aan beide uiteinden verzenden en ontvangen van gegevens via de VPN-tunnel. 

1. Selecteer in de Azure-portal **nieuw**.
2. Ga naar **Marketplace**, en selecteer vervolgens **Compute**.
3. Selecteer in de lijst van installatiekopieën van virtuele machines, de **Windows Server 2016 Datacenter Eval** installatiekopie.
4. Op de **basisbeginselen** sectie in **naam**, type **Azs VM**.
5. Typ een geldige gebruikersnaam en wachtwoord. Dit account kunt u zich aanmeldt bij de virtuele machine nadat deze gemaakt.
6. Geef een **abonnement**, **resourcegroep**, en **locatie**, en selecteer vervolgens **OK**.
7. Op de **grootte** sectie voor dit exemplaar, selecteer de grootte van een virtuele machine en selecteer vervolgens **Selecteer**.
8. Op de **instellingen** sectie, accepteer de standaardinstellingen. Zorg ervoor dat de **Azs VNet** virtueel netwerk is geselecteerd. Controleren of het subnet is ingesteld op **10.1.0.0/24**. Selecteer vervolgens **OK**.
9. Op de **samenvatting** sectie, Controleer de instellingen en selecteer vervolgens **OK**.


## <a name="test-the-connection"></a>De verbinding testen
Nu dat de site-naar-site-verbinding tot stand is gebracht, moet u controleren dat u verkeer ermee kunt krijgen. Als u wilt valideren, moet u zich aanmelden bij een van de virtuele machines die u hebt gemaakt in Azure-Stack. Pingt de virtuele machine die u hebt gemaakt in Azure. 

Om ervoor te zorgen dat u het verkeer via de site-naar-site-verbinding verzenden, ping het Direct IP (DIP)-adres van de virtuele machine op de externe subnet, niet het VIP. U doet dit door het DIP-adres op het andere einde van de verbinding te vinden. Sla het adres voor later gebruik.

### <a name="sign-in-to-the-user-vm-in-azure-stack"></a>Aanmelden bij de virtuele machine in Azure-Stack van de gebruiker
1. Meld u aan de Stack van Azure-portal.
2. Selecteer in de linkernavigatiebalk **virtuele Machines**.
3. Zoek in de lijst van virtuele machines **Azs VM** die u eerder hebt gemaakt en selecteert u vervolgens.
4. Klik in de sectie voor de virtuele machine op **Connect**, en open vervolgens het bestand Azs VM.rdp.
   
     ![Verbinding maken met de knop](media/azure-stack-create-vpn-connection-one-node-tp2/image17.png)
5. Aanmelden met het account dat u hebt geconfigureerd tijdens het maken van de virtuele machine.
6. Open een opdrachtprompt met verhoogde bevoegdheid **Windows PowerShell** venster.
7. Typ **ipconfig /all**.
8. Zoek in de uitvoer naar de **IPv4-adres**, en sla het adres voor later gebruik. Dit is het adres dat u van Azure pingt. In de voorbeeldomgeving, het adres is **10.0.10.4**, maar in uw omgeving mogelijk andere. Deze moet vallen binnen de **10.0.10.0/24** subnet dat u eerder hebt gemaakt.
9. Voer de volgende PowerShell-opdracht voor het maken van een firewallregel waarmee de virtuele machine op pings reageren:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

### <a name="sign-in-to-the-tenant-vm-in-azure"></a>Aanmelden bij de virtuele machine in Azure-tenant
1. Meld u aan bij Azure Portal.
2. Klik in de linkernavigatiebalk op **virtuele Machines**.
3. Zoek in de lijst met virtuele machines, **Azure VM** die u eerder hebt gemaakt en selecteert u vervolgens.
4. Klik in de sectie voor de virtuele machine op **Connect**.
5. Aanmelden met het account dat u hebt geconfigureerd tijdens het maken van de virtuele machine.
6. Open een opdrachtprompt met verhoogde bevoegdheid **Windows PowerShell** venster.
7. Typ **ipconfig /all**.
8. Er is een IPv4-adres valt binnen **10.0.20.0/24**. In de voorbeeldomgeving, het adres is **10.0.20.4**, maar uw adres kan anders zijn.
9. Voer de volgende PowerShell-opdracht voor het maken van een firewallregel waarmee de virtuele machine op pings reageren:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

10. Ping van de virtuele machine in Azure, de virtuele machine in Azure-Stack via de tunnel. Om dit te doen, moet u het DIP die u hebt vastgelegd van Azs VM pingen.
   In de voorbeeldomgeving is dit **10.0.10.4**, maar zorg ervoor dat het adres dat u hebt genoteerd in uw testomgeving pingen. Hier ziet u een resultaat dat op de volgende schermafbeelding lijkt:
   
    ![Geslaagde ping](media/azure-stack-create-vpn-connection-one-node-tp2/image19b.png)
11. Een antwoord van de externe virtuele-machine geeft aan dat een geslaagde test! U kunt de virtuele machine-venster sluiten. U kunt uw om verbinding te testen, andere soorten gegevensoverdrachten zoals het kopiëren van een bestand.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Statistieken van gegevensoverdracht via de gatewayverbinding weergeven
Als u wilt weten op hoeveel gegevens worden doorgegeven via de site-naar-site-verbinding, deze informatie is beschikbaar op de **verbinding** sectie. Deze test is ook een andere manier om te verifiëren dat het pingen die u zojuist hebt verzonden daadwerkelijk is gegaan via de VPN-verbinding.

1. Terwijl u bent aangemeld bij de gebruiker virtuele machine in Azure-Stack, gebruikt u uw gebruikersaccount aanmelden bij de gebruikersportal.
2. Ga naar **alle resources**, en selecteer vervolgens de **Azs Azure** verbinding. **Verbindingen** wordt weergegeven.
4. Op de **verbinding** sectie, de statistieken voor **gegevens in** en **gegevensuitvoer** worden weergegeven. In de volgende schermafbeelding worden grote aantallen toegeschreven aan extra bestandsoverdracht. U ziet er sommige andere waarden.
   
    ![Gegevens in en uit](media/azure-stack-connect-vpn/Connection.png)

## <a name="next-steps"></a>Volgende stappen

[Apps implementeren op Azure en Azure Stack](azure-stack-solution-pipeline.md)