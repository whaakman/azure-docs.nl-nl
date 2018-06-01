---
title: "Zelfstudie: Load Balancer VM's in een zone - Azure Portal | Microsoft Docs"
description: Deze zelfstudie laat zien hoe u met behulp van Azure Portal een standaard Load Balancer kunt maken met een zonegebonden frontend voor de taakverdeling van VM's binnen een beschikbaarheidszone
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: As an IT administrator, I want to create a load balancer that load balances incoming internet traffic to virtual machines within a specific zone in a region.
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/17/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 52d0aeabab173caf4460827ca0d5984070688f0e
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/18/2018
ms.locfileid: "34304722"
---
# <a name="tutorialload-balance-vms-within-an-availability-zone-with-a-standard-load-balancer-using-the-azure-portal"></a>Zelfstudie: Met behulp van Azure Portal taakverdeling uitvoeren van virtuele machines binnen een beschikbaarheidszone met een standaard Load Balancer

Deze zelfstudie toont de stappen voor het maken van een openbare [Load Balancer Standard](https://aka.ms/azureloadbalancerstandard) met een zonegebonden frontend met behulp van een openbaar IP-standaardadres met de Azure Portal. In dit scenario geeft u een bepaald gebied op voor uw front-end- en back-endinstanties om uw gegevenspad en resources op één lijn te brengen met een specifieke zone. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een Standard Load Balancer van Azure met een zonegebonden frontend maken
> * Netwerkbeveiligingsgroepen maken voor het definiëren van regels voor inkomend verkeer
> * Zonegebonden virtuele machines maken en koppelen aan een load balancer
> * Een load balancer-statustest maken
> * Load balancer-verkeersregels maken
> * Een basis-IIS-site maken
> * Een load balancer in actie zien

Zie [Standard Load Balancer en beschikbaarheidszones](load-balancer-standard-availability-zones.md) voor meer informatie over het gebruik van beschikbaarheidszones met Standard Load Balancer.

U kunt deze zelfstudie desgewenst volgen met behulp van de [Azure CLI](load-balancer-standard-public-zonal-cli.md).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij de Azure-portal op [http://portal.azure.com](http://portal.azure.com).

## <a name="create-a-public-standard-load-balancer"></a>Een openbare Standard Load Balancer maken

Standard Load Balancer biedt alleen ondersteuning voor een standaard, openbaar IP-adres. Wanneer u een nieuwe openbaar IP-adres maakt tijdens het maken van de load balancer, wordt het automatisch geconfigureerd als een standaard SKU-versie en is het ook automatisch zone-redundant.

1. Klik linksboven in het scherm op **Een resource maken** > **Netwerken** > **Load balancer**.
2. Voer op de pagina **Een load balancer maken** deze waarden in voor de load balancer:
    - *myLoadBalancer* als naam van de load balancer.
    - **Openbaar** als het type load balancer.
     - *myPublicIPZonal* voor het nieuwe, openbare IP-adres dat u maakt. Klik hiervoor op **Kies een openbaar IP-adres** en vervolgens op **Nieuw**. Voor het naamtype *myPublicIP* is SKU altijd standaard en wordt **Zone 1** geselecteerd voor **Beschikbaarheidszone**.
    - *myResourceGroupZLB* als naam van de nieuwe resourcegroep die u maakt.
    - **westeurope** als locatie.
3. Klik op **Maken** om de load balancer te maken.
   
    ![Zonegebonden Load Balancer Standard maken met de Azure Portal](./media/tutorial-load-balancer-standard-zonal-portal/create-load-balancer-zonal-frontend.png)


## <a name="create-backend-servers"></a>Back-endservers maken

In deze sectie maakt u een virtueel netwerk, twee virtuele machines in dezelfde zone (zone 1) voor de regio om de back-endpool van uw load balancer toe te voegen en installeert u IIS op de virtuele machines om de zone-redundante load balancer te testen. Dus als één virtuele machine niet werkt, dan mislukt de teststatus voor de virtuele machine in dezelfde zone, maar wordt het verkeer nog steeds geregeld door een andere VM binnen dezelfde zone.

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
1. Klik linksboven in het scherm op **Een resource maken** > **Netwerken** > **Virtueel netwerk** en voer deze waarden in voor het virtuele netwerk:
    - *myVnet* als naam van het virtuele netwerk.
    - *myResourceGroupZLB* als naam van de bestaande resourcegroep
    - *myBackendSubnet* als naam van het subnet.
2. Klik op **Maken** om het virtuele netwerk te maken.

    ![Een virtueel netwerk maken](./media/tutorial-load-balancer-standard-zonal-portal/create-virtual-network.png)

## <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

1. Klik linksboven in het scherm op **Een resource maken**, typ *Netwerkbeveiligingsgroep* in het zoekvak en klik op **Maken** op de pagina Netwerkbeveiligingsgroep.
2. Voer deze waarden in op de pagina Netwerkbeveiligingsgroep maken:
    - *myNetworkSecurityGroup* als naam van de netwerkbeveiligingsgroep.
    - *myResourceGroupLBAZ* als naam van de bestaande resourcegroep.
   
    ![Een virtueel netwerk maken](./media/tutorial-load-balancer-standard-zonal-portal/create-network-security-group.png)

### <a name="create-nsg-rules"></a>NSG-regels maken

In deze sectie maakt u NSG-regels om inkomende verbindingen via HTTP en RDP toe te staan met de Azure Portal.

1. Klik in de Azure Portal op **Alle resources** in het linkermenu en zoek dan naar en klik op **myNetworkSecurityGroup**, die zich in de resourcegroep **myResourceGroupZLB** bevindt.
2. Klik onder **Instellingen** op **Inkomende beveiligingsregels** en vervolgens op **Toevoegen**.
3. Voer deze waarden in voor de inkomende beveiligingsregel met de naam *myHTTPRule* om een binnenkomende HTTP-verbinding via poort 80 toe te staan:
    - *Service Tag* bij **Bron**.
    - *Internet* bij **Bronservicetag**
    - *80* bij **Poortbereiken van doel**
    - *TCP* bij **Protocol**
    - *Allow* bij **Actie**
    - *100* bij **Prioriteit**
    - *myHTTPRule* als **Name**
    - *Allow HTTP* als **Beschrijving**
4. Klik op **OK**.
 
 ![Een virtueel netwerk maken](./media/load-balancer-standard-public-availability-zones-portal/8-load-balancer-nsg-rules.png)

5. Herhaal stap 2 t/m 4 om nog een regel te maken (*myRDPRule*) om een binnenkomende RDP-verbinding toe te staan via poort 3389 met de volgende waarden:
    - *Service Tag* bij **Bron**.
    - *Internet* bij **Bronservicetag**
    - *3389* bij **Poortbereiken van doel**
    - *TCP* bij **Protocol**
    - *Allow* bij **Actie**
    - *200* bij **Prioriteit**
    - *myRDPRule* als naam
    - *Allow RDP* als beschrijving

    ![RDP-regel maken](./media/tutorial-load-balancer-standard-zonal-portal/create-rdp-rule.png)

### <a name="create-virtual-machines"></a>Virtuele machines maken

1. Klik linksboven in het scherm op **Een resource maken** > **Compute** > **Windows Server 2016 Datacenter** en voer deze waarden in voor de virtuele machine:
    - *myVM1* als naam van de virtuele machine.        
    - *azureuser* als gebruikersnaam van de beheerder.    
    - *myResourceGroupZLB*: voor **Resourcegroep** selecteert u **Bestaande gebruiken** en vervolgens *myResourceGroupZLB*.
2. Klik op **OK**.
3. Selecteer **DS1_V2** als grootte van de virtuele machine en klik op **Selecteren**.
4. Voer deze waarden in voor de instellingen van de VM:
    - *zone 1* voor de Beschikbaarheidszone waar u de VM wilt plaatsen.
    -  *myVNet*: controleer of dit als virtueel netwerk is geselecteerd.
    - *myVM1PIP* voor het standaard openbare IP-adres dat u maakt. Klik op *Nieuwe maken* en selecteer vervolgens voor naamtype *myVM1PIP* en voor Zone **1**. Het IP-adres SKU is altijd standaard.
    - *myBackendSubnet*: controleer of dit als subnet is geselecteerd.
    - *myNetworkSecurityGroup* als naam van de netwerkbeveiligingsgroep (firewall) die al bestaat.
5. Klik op **Uitgeschakeld** om diagnostische gegevens over opstarten uit te schakelen.
6. Klik op **OK**, controleer de instellingen op de overzichtspagina en klik op **Maken**.
7. Volg stap 1 tot en met 6 om een tweede VM te maken met de naam *myVM2* in Zone 1, met *myVnet* als het virtuele netwerk, *myVM2PIP* als het standaard openbare IP-adres, *myBackendSubnet* als het subnet en **myNetworkSecurityGroup* als de netwerkbeveiligingsgroep.

    ![RDP-regel maken](./media/tutorial-load-balancer-standard-zonal-portal/create-virtual-machine.png) 

### <a name="install-iis-on-vms"></a>IIS installeren op VM's

1. Klik in het linkermenu op **Alle resources** en klik in de lijst met resources op **myVM1**, die zich in de resourcegroep *myResourceGroupZLB* bevindt.
2. Klik op de pagina **Overzicht** op **Verbinding maken** om extern verbinding te maken met de VM.
3. Meld u aan met de gebruikersnaam en het wachtwoord die u bij het maken van de VM hebt opgegeven (mogelijk moet u **Meer opties** en **Een ander account gebruiken** selecteren om de aanmeldingsgegevens op te geven die u hebt ingevoerd tijdens het maken van de virtuele machine) en selecteer **OK**. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Selecteer **Ja** om door te gaan met de verbinding.
4. Ga op de serverdesktop naar **Windows Systeembeheer**>**Windows Powershell**.
6. Voer in het venster PowerShell de volgende opdrachten uit om de IIS-server te installeren, het standaardbestand iisstart.htm te verwijderen en een nieuw bestand iisstart.htm toe te voegen dat de naam van de VM weergeeft:

   ```azurepowershell-interactive
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from" + $env:computername)
   ```
7. Sluit de RDP-sessie met *myVM1*
8. Herhaal stappen 1 tot en met 7 om IIS te installeren op *myVM2*.

## <a name="create-load-balancer-resources"></a>Resources voor load balancer maken

In deze sectie configureert u de instellingen voor de load balancer voor een back-endadresgroep en een statustest en geeft u regels voor de load balancer en NAT-regels op.


### <a name="create-a-backend-address-pool"></a>Een back-endadresgroep maken

Om verkeer te distribueren naar de VM's bevat een back-end-adresgroep de IP-adressen van de virtuele netwerkinterfacekaarten (NIC's) die zijn verbonden met de load balancer. Maak de back-endadresgroep *myBackendPool* om *VM1* en *VM2* op te nemen.

1. Klik in het linkermenu op **Alle resources** en vervolgens in de lijst met resources op **myLoadBalancer*.
2. Klik onder **Instellingen** op **Back-endpools** en vervolgens op **Toevoegen**.
3. Ga als volgt te werk op de pagina **Een back-endpool toevoegen**:
    - Typ *myBackEndPool* als de naam van uw back-endpool.
    - Voor **Virtueel netwerk** klikt u in de vervolgkeuzelijst op *myVNet*
    - Voor **Virtuele machine** en **IP-adres** voegt u *myVM1* en *myVM2* toe met de bijbehorende openbare IP-adressen.
4. Klik op **Add**.
5. Controleer of de instelling voor de back-endpool van de load balancer beide VM's (**myVM1** en **myVM2**) weergeeft.
 
    ![Back-endpool maken](./media/tutorial-load-balancer-standard-zonal-portal/create-backend-pool.png) 

### <a name="create-a-health-probe"></a>Een statustest maken

U gebruikt een statustest om de load balancer de status van uw app te laten bewaken. De statustest voegt dynamisch VM's toe aan de load balancer-rotatie of verwijdert ze, op basis van hun reactie op statuscontroles. Maak een statustest (*myHealthProbe*) om de status van de VM's te bewaken.

1. Klik in het linkermenu op **Alle resources** en vervolgens in de lijst met resources op **myLoadBalancer**.
2. Klik onder **Instellingen** op **Tests** en klik op **Toevoegen**.
3. Gebruik deze waarden om de statustest te maken:
    - *myHealthProbe* als naam van de statustest.
    - **HTTP** als protocoltype.
    - *80* als poortnummer.
    - *15* als **interval** in seconden tussen tests.
    - *2* als aantal **drempelwaarden voor onjuiste status** of opeenvolgende mislukte tests dat moet optreden voordat wordt besloten dat een VM een onjuiste status heeft.
4. Klik op **OK**.

   ![Een test toevoegen](./media/load-balancer-standard-public-availability-zones-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Een load balancer-regel maken

Een load balancer-regel wordt gebruikt om de verdeling van het verkeer over de VM's te definiëren. U definieert de front-end-IP-configuratie voor het inkomende verkeer en de back-end-IP-groep om het verkeer te ontvangen, samen met de gewenste bron- en doelpoort. Maak de regel *myLoadBalancerRuleWeb* voor de load balancer voor het luisteren naar poort 80 in de front-end *FrontendLoadBalancer* en het verzenden van netwerkverkeer met gelijke taakverdeling naar de back-endadresgroep *myBackEndPool* waarbij ook van poort 80 gebruik wordt gemaakt. 

1. Klik in het linkermenu op **Alle resources** en vervolgens in de lijst met resources op **myLoadBalancer**.
2. Klik onder **Instellingen** op **Taakverdelingsregels** en vervolgens op **Toevoegen**.
3. Gebruik deze waarden om de taakverdelingsregel te configureren:
    - *myHTTPRule* als naam van de taakverdelingsregel.
    - **TCP** als protocoltype.
    - *80* als poortnummer.
    - *80* als back-endpoort.
    - *myBackendPool* als naam van de back-endpool.
    - *myHealthProbe* als naam van de statustest.
4. Klik op **OK**.
    
    ![Een taakverdelingsregel toevoegen](./media/tutorial-load-balancer-standard-zonal-portal/load-balancing-rule.png)

## <a name="test-the-load-balancer"></a>Load balancer testen
1. Zoek op het scherm **Overzicht** het openbare IP-adres voor de load balancer. Klik op **Alle resources** en vervolgens op **myPublicIP**. 

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. De standaardpagina met de naam van de webpagina van de server wordt weergegeven in de browser.

      ![IIS-webserver](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)
3. Wanneer u de load balancer in actie wilt zien, kunt u de virtuele machine die wordt weergegeven dwingen te stoppen en de browser vernieuwen om de andere servernaam in de browser weer te geven.

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder de resourcegroep, de load balancer en alle gerelateerde resources, wanneer u deze niet meer nodig hebt. Selecteer hiertoe de resourcegroep met de load balancer en klik op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Standard Load Balancer](load-balancer-standard-overview.md).
- [Taken over VM's in meerdere beschikbaarheidszones verdelen](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
