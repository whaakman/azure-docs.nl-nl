---
title: 'Zelfstudie: Een openbare Basic Load Balancer maken - Azure Portal | Microsoft Docs'
description: In deze zelfstudie ziet u hoe u een interne Basic Load Balancer maakt met behulp van Azure Portal.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: As an IT administrator, I want to create a load balancer that load balances incoming internet traffic to virtual machines within a specific zone in a region.
ms.assetid: aa9d26ca-3d8a-4a99-83b7-c410dd20b9d0
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 7902b5ad2d680a22a2d132187cdad5f96a334447
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37061842"
---
# <a name="tutorial-load-balance-internal-traffic-with-basic-load-balancer-to-vms-using-the-azure-portal"></a>Zelfstudie: Intern verkeer met Basic Load Balancer verdelen over virtuele machines met Azure Portal

Taakverdeling zorgt voor een hogere beschikbaarheid en betere schaalbaarheid door binnenkomende aanvragen te spreiden over meerdere virtuele machines. U kunt Azure Portal gebruiken om intern verkeer naar virtuele machines te verdelen met behulp van een Basic Load Balancer. In deze snelstart wordt beschreven hoe u netwerkresources, back-endservers en een interne Basic Load Balancer maakt.

U kunt deze zelfstudie desgewenst volgen met behulp van de [Azure CLI](load-balancer-get-started-ilb-arm-cli.md) of [Azure PowerShell](load-balancer-get-started-ilb-arm-ps.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
1. Klik linksboven in het scherm op **Nieuw** > **Netwerken** > **Virtueel netwerk** en voer deze waarden in voor het virtuele netwerk:
    - *myVnet* als naam van het virtuele netwerk.
    - *myResourceGroupILB* als naam van de bestaande resourcegroep
    - *myBackendSubnet* als naam van het subnet.
2. Klik op **Maken** om het virtuele netwerk te maken.

![Een load balancer maken](./media/tutorial-load-balancer-basic-internal-portal/1-load-balancer.png)

## <a name="create-a-basic-load-balancer"></a>Een Basic load balancer maken
Maak een interne Basic Load Balancer met behulp van de portal.

1. Klik linksboven in het scherm op **Een resource maken** > **Netwerken** > **Load balancer**.
2. Voer op de pagina **Een load balancer maken** deze waarden in voor de load balancer:
    - *myLoadBalancer* als naam van de load balancer.
    - **Intern** als het type load balancer.
    - **Basic** als SKU-versie.
    - **10.1.0.7** als het statisch privé-IP-adres.
    - *myVNet* als het virtuele netwerk dat u kiest uit de lijst met bestaande netwerken.
    - *mySubnet* als het subnet dat u kiest uit de lijst met bestaande subnetten.
    - *myResourceGroupILB* als naam van de nieuwe resourcegroep die u maakt.
3. Klik op **Maken** om de load balancer te maken.
   
    ## <a name="create-backend-servers"></a>Back-endservers maken

In deze sectie maakt u twee virtuele machines voor de back-endpool van de Basic Load Balancer en installeert u IIS op de virtuele machines om de load balancer te testen.

### <a name="create-virtual-machines"></a>Virtuele machines maken

1. Klik linksboven in het scherm op **Een resource maken** > **Compute** > **Windows Server 2016 Datacenter** en voer deze waarden in voor de virtuele machine:
    - *myVM1* als naam van de virtuele machine.        
    - *azureuser* als gebruikersnaam van de beheerder.   
    - *myResourceGroupILB* als **Resourcegroep**. Selecteer **Bestaande gebruiken** en vervolgens *myResourceGroupILB*.
2. Klik op **OK**.
3. Selecteer **DS1_V2** als grootte van de virtuele machine en klik op **Selecteren**.
4. Voer deze waarden in voor de instellingen van de VM:
    - *myAvailabilitySet* als naam van de nieuwe beschikbaarheidsset die u maakt.
    -  *myVNet*: controleer of dit als virtueel netwerk is geselecteerd.
    - *myBackendSubnet*: controleer of dit als subnet is geselecteerd.
    - *myNetworkSecurityGroup* als naam van de nieuwe netwerkbeveiligingsgroep (firewall) die u moet maken.
5. Klik op **Uitgeschakeld** om diagnostische gegevens over opstarten uit te schakelen.
6. Klik op **OK**, controleer de instellingen op de overzichtspagina en klik op **Maken**.
7. Volg stap 1 t/m 6 om een tweede VM met de naam *VM2* te maken, met *myAvailibilityset* als beschikbaarheidsset, *myVnet* als het virtuele netwerk, *myBackendSubnet* als het subnet en *myNetworkSecurityGroup* als de netwerkbeveiligingsgroep. 

### <a name="install-iis-and-customize-the-default-web-page"></a>IIS installeren en de standaardwebpagina aanpassen

1. Klik in het linkermenu op **Alle resources** en klik in de lijst met resources op **myVM1**, die zich in de resourcegroep *myResourceGroupILB* bevindt.
2. Klik op de pagina **Overzicht** op **Verbinding maken** om extern verbinding te maken met de VM.
3. Meld u aan bij de VM.
4. Ga op de serverdesktop naar **Windows Systeembeheer**>**Serverbeheer**.
5. Start Windows PowerShell op VM1 en gebruik de volgende opdrachten om de IIS-server te installeren en het standaard htm-bestand bij te werken.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```
5. Sluit de RDP-verbinding met *myVM1*.
6. Herhaal stap 1-5 met *myVM2* om IIS te installeren en de standaardwebpagina aan te passen.

## <a name="create-nsg-rules"></a>NSG-regels maken

In deze sectie maakt u NSG-regels om inkomende verbindingen via HTTP en RDP toe te staan.

1. Klik in het linkermenu op **Alle resources** en klik in de lijst met resources op **myNetworkSecurityGroup**, die zich in de resourcegroep **myResourceGroupLB** bevindt.
2. Klik onder **Instellingen** op **Inkomende beveiligingsregels** en vervolgens op **Toevoegen**.
3. Voer deze waarden in voor de inkomende beveiligingsregel met de naam *myHTTPRule* om een binnenkomende HTTP-verbinding via poort 80 toe te staan:
    - *Service Tag* bij **Bron**.
    - *Internet* bij **Bronservicetag**
    - *80* bij **Poortbereiken van doel**
    - *TCP* bij **Protocol**
    - *Allow* bij **Actie**
    - *100* bij **Prioriteit**
    - *myHTTPRule* als naam
    - *Allow HTTP* als beschrijving
4. Klik op **OK**.
 
5. Herhaal stap 2 t/m 4 om nog een regel te maken (*myRDPRule*) om een binnenkomende RDP-verbinding toe te staan via poort 3389 met de volgende waarden:
    - *Service Tag* bij **Bron**.
    - *Internet* bij **Bronservicetag**
    - *3389* bij **Poortbereiken van doel**
    - *TCP* bij **Protocol**
    - *Allow* bij **Actie**
    - *200* bij **Prioriteit**
    - *myRDPRule* als naam
    - *Allow RDP* als beschrijving

## <a name="create-basic-load-balancer-resources"></a>Resources voor Basic load balancer maken

In deze sectie configureert u de instellingen voor de load balancer voor een back-endadresgroep en een statustest en geeft u regels voor de load balancer en NAT-regels op.


### <a name="create-a-backend-address-pool"></a>Een back-endadresgroep maken

Om verkeer te distribueren naar de VM's bevat een back-end-adresgroep de IP-adressen van de virtuele netwerkinterfacekaarten (NIC's) die zijn verbonden met de load balancer. Maak de back-endadresgroep *myBackendPool* om *VM1* en *VM2* op te nemen.

1. Klik in het linkermenu op **Alle resources** en vervolgens in de lijst met resources op **myLoadBalancer**.
2. Klik onder **Instellingen** op **Back-endpools** en vervolgens op **Toevoegen**.
3. Ga als volgt te werk op de pagina **Een back-endpool toevoegen**:
    - Typ *myBackEndPool voor de naam van uw back-endpool.
    - Klik voor **Gekoppeld aan** in de vervolgkeuzelijst op **Beschikbaarheidsset**
    - Klik voor **Beschikbaarheidsset** op **myAvailabilitySet**.
    - Klik op **Een doelnetwerk-IP-configuratie toevoegen** om elke virtuele machine die u hebt gemaakt (*myVM1* & *myVM2*), toe te voegen aan de back-endpool.
    - Klik op **OK**.

        ![Toevoegingen doen aan de back-endadresgroep ](./media/tutorial-load-balancer-basic-internal-portal/3-load-balancer-backend-02.png)

3. Controleer of de instelling voor de back-endpool van de load balancer beide VM's (**VM1** en **VM2**) weergeeft.

### <a name="create-a-health-probe"></a>Een statustest maken

U gebruikt een statustest om de Basic load balancer de status van uw app te laten bewaken. De statustest voegt dynamisch VM's toe aan de load balancer-rotatie of verwijdert ze, op basis van hun reactie op statuscontroles. Maak een statustest (*myHealthProbe*) om de status van de VM's te bewaken.

1. Klik in het linkermenu op **Alle resources** en vervolgens in de lijst met resources op **myLoadBalancer**.
2. Klik onder **Instellingen** op **Tests** en klik op **Toevoegen**.
3. Gebruik deze waarden om de statustest te maken:
    - *myHealthProbe* als naam van de statustest.
    - **HTTP** als protocoltype.
    - *80* als poortnummer.
    - *15* als **interval** in seconden tussen tests.
    - *2* als aantal **drempelwaarden voor onjuiste status** of opeenvolgende mislukte tests dat moet optreden voordat wordt besloten dat een VM een onjuiste status heeft.
4. Klik op **OK**.

   ![Een test toevoegen](./media/tutorial-load-balancer-basic-internal-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Een load balancer-regel maken

Een load balancer-regel wordt gebruikt om de verdeling van het verkeer over de VM's te definiëren. U definieert de front-end-IP-configuratie voor het inkomende verkeer en de back-end-IP-groep om het verkeer te ontvangen, samen met de gewenste bron- en doelpoort. Maak de regel *myLoadBalancerRuleWeb* voor de load balancer voor het luisteren naar poort 80 in de front-end *LoadBalancerFrontEnd* en het verzenden van netwerkverkeer met gelijke taakverdeling naar de back-endadresgroep *myBackEndPool* waarbij ook van poort 80 gebruik wordt gemaakt. 

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
    
    ![Een taakverdelingsregel toevoegen](./media/tutorial-load-balancer-basic-internal-portal/5-load-balancing-rules.png)

## <a name="create-a-virtual-machine-to-test-the-load-balancer"></a>Een virtuele machine maken om de load balancer te testen
Om de interne load balancer te testen, moet u een virtuele machine maken die zich in hetzelfde virtuele netwerk bevindt als de virtuele machines van de back-endserver.
1. Klik linksboven in het scherm op **Een resource maken** > **Compute** > **Windows Server 2016 Datacenter** en voer deze waarden in voor de virtuele machine:
    - *myVMTest* als naam van de virtuele machine.        
    - *myResourceGroupILB* als **Resourcegroep**. Selecteer **Bestaande gebruiken** en vervolgens *myResourceGroupILB*.
2. Klik op **OK**.
3. Selecteer **DS1_V2** als grootte van de virtuele machine en klik op **Selecteren**.
4. Voer deze waarden in voor de instellingen van de VM:
    -  *myVNet*: controleer of dit als virtueel netwerk is geselecteerd.
    - *myBackendSubnet*: controleer of dit als subnet is geselecteerd.
5. Klik op **Uitgeschakeld** om diagnostische gegevens over opstarten uit te schakelen.
6. Klik op **OK**, controleer de instellingen op de overzichtspagina en klik op **Maken**.

## <a name="test-the-load-balancer"></a>Load balancer testen
1. Achterhaal in Azure Portal op het scherm **Overzicht** het privé-IP-adres voor de load balancer. Dit doet u als volgt: a. Klik in het linkermenu op **Alle resources** en vervolgens in de lijst met resources op **myLoadBalancer**.
    b. Op de detailpagina in **Overzicht** kopieert u het privé-IP-adres (in dit voorbeeld is het 10.1.0.7).

2. Maak als volgt een externe verbinding met *myVMTest*: a. Klik in het linkermenu op **Alle resources** en klik in de lijst met resources op **myVMTest**, die zich in de resourcegroep *myResourceGroupILB* bevindt.
2. Klik op de pagina **Overzicht** op **Verbinding maken** om een externe sessie met de virtuele machine te starten.
3. Meld u aan bij *myVMTest*.
3. Plak het privé-IP-adres in de adresbalk van de browser in *myVMTest*. De standaardpagina van IIS-webserver wordt weergegeven in de browser.

      ![IIS-webserver](./media/tutorial-load-balancer-basic-internal-portal/9-load-balancer-test.png)

Als u wilt zien hoe de load balancer verkeer distribueert naar beide VM's waarop uw app wordt uitgevoerd, kunt u vernieuwing van uw webbrowser afdwingen.

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder de resourcegroep, de load balancer en alle gerelateerde resources, wanneer u deze niet meer nodig hebt. Selecteer hiertoe de resourcegroep met de load balancer en klik op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een resourcegroep, netwerkresources en back-endservers gemaakt. Daarna hebt u deze resources gebruikt om een interne load balancer te maken om het interne verkeer naar virtuele machines te verdelen. Hierna leert u hoe u [Taken verdeelt over VM's in meerdere beschikbaarheidszones](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
