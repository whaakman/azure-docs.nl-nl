---
title: 'Snelstart: Een openbare Basic Load Balancer maken met behulp van de Azure Portal | Microsoft Docs'
description: In deze snelstart vindt u informatie over het maken van een openbare Basic Load Balancer via Azure Portal.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to create a Basic Load balancer so that I can load balance internet traffic to VMs.
ms.assetid: aa9d26ca-3d8a-4a99-83b7-c410dd20b9d0
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 610aa8d5652b89f36aeb2a6ae517d378c5bfa666
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-public-basic-load-balancer-by-using-the-azure-portal"></a>Snelstart: Een openbare Basic Load Balancer maken met behulp van de Azure Portal

Taakverdeling zorgt voor een hogere beschikbaarheid en betere schaalbaarheid door binnenkomende aanvragen te spreiden over meerdere virtuele machines (VM's). U kunt in Azure Portal een load balancer maken om taken van virtuele machines te verdelen. In deze snelstart wordt beschreven hoe u netwerkresources, back-endservers en een load balancer kunt maken in het prijssegment Basic.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Voor alle taken in deze snelstartgids moet u zich aanmelden bij de [Azure Portal](http://portal.azure.com).

## <a name="create-a-basic-load-balancer"></a>Een Basic Load Balancer maken

In deze sectie maakt u een openbare Basic Load Balancer met behulp van de portal. Het openbare IP-adres wordt automatisch geconfigureerd als de front-end van de load balancer wanneer u het openbare IP-adres en de load balancer-resource maakt met behulp van de portal. De naam van de front-end is **LoadBalancerFrontend**.

1. Selecteer **Een resource maken** > **Netwerken** > **Load Balancer** linksboven in het scherm.
2. Voer in het deelvenster **Load Balancer maken** deze waarden in:
   - **myLoadBalancer** als naam van de load balancer
   - **Openbaar** als het type load balancer 
   - **myPublicIP** als het openbare IP-adres dat u moet maken met **SKU** ingesteld als **Basic** en **Toewijzing** ingesteld als **Dynamisch**
   - **myResourceGroupLB** als naam van de nieuwe resourcegroep
3. Selecteer **Maken**.
   
![Een load balancer maken](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## <a name="create-back-end-servers"></a>Back-endservers maken

In deze sectie maakt u een virtueel netwerk, en maakt u twee virtuele machines voor de back-endpool van uw Basic Load Balancer. Vervolgens installeert u Internet Information Services (IIS) op de virtuele machines om de load balancer te testen.

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
1. Linksboven in de portal selecteert u **Nieuw** > **Networken** > **Virtueel netwerk**.
2. In het deelvenster **Virtueel netwerk maken** voert u deze waarden in en selecteert u vervolgens **Maken**:
   - **myVnet** als naam van het virtuele netwerk
   - **myResourceGroupLB** als naam van de bestaande resourcegroep
   - **myBackendSubnet** als naam van het subnet

   ![Een virtueel netwerk maken](./media/load-balancer-get-started-internet-portal/2-load-balancer-virtual-network.png)

### <a name="create-virtual-machines"></a>Virtuele machines maken

1. Linksboven op de portal, selecteert u **Nieuw** > **Compute** > **Windows Server 2016 Datacenter**. 
2. Voer deze waarden in voor de virtuele machine en selecteer vervolgens **OK**:
   - **myVM1** als naam van de virtuele machine.        
   - **azureuser** als gebruikersnaam van de beheerder.    
   - **myResourceGroupLB** voor de resourcegroep. (Onder **Resource group** selecteert u **Bestaande gebruiken** en vervolgens **myResourceGroupLB**.)   
3. Selecteer **DS1_V2** als grootte van de virtuele machine en klik op **Selecteren**.
4. Voer deze waarden in voor de instellingen van de VM:
   - **myAvailabilitySet** als naam van de nieuwe beschikbaarheidsset die u maakt.
   - **myVnet** als naam van het virtuele netwerk. (Zorg ervoor dat deze optie geselecteerd.)
   - **myBackendSubnet** als naam van het subnet. (Zorg ervoor dat deze optie geselecteerd.)
   - **myVM1-ip** als het openbare IP-adres.
   - **myNetworkSecurityGroup** als naam van de nieuwe netwerkbeveiligingsgroep (NSG, een soort firewall) die u moet maken.
5. Selecteer **Uitgeschakeld** om diagnostische gegevens over opstarten uit te schakelen.
6. Selecteer **OK**, controleer de instellingen op de overzichtspagina en selecteer **Maken**.
7. Volg stap 1 tot en met 6 om een tweede VM genaamd **VM2** te maken, met:
   - **myAvailabilityset** als de beschikbaarheidsset.
   - **myVnet** als het virtuele netwerk.
   - **myBackendSubnet** als het subnet.
   - **myNetworkSecurityGroup** als de netwerkbeveiligingsgroep. 

### <a name="create-nsg-rules"></a>NSG-regels maken

In deze sectie maakt u NSG-regels om inkomende verbindingen via HTTP en RDP toe te staan.

1. Selecteer **Alle resources** in het menu aan de linkerkant. Selecteer in de lijst met resources **myNetworkSecurityGroup** in de resourcegroep **myResourceGroupLB**.
2. Selecteer onder **Instellingen** **Inkomende beveiligingsregels** en selecteer vervolgens **Toevoegen**.
3. Voer de volgende waarden in voor de inkomende beveiligingsregel met de naam **myHTTPRule** om binnenkomende HTTP-verbindingen via poort 80 toe te staan. Selecteer vervolgens **OK**.
   - **Service Tag** bij **Bron**
   - **Internet** bij **Bronservicetag**
   - **80** bij **Poortbereiken van doel**
   - **TCP** bij **Protocol**
   - **Allow** bij **Actie**
   - **100** bij **Prioriteit**
   - **myHTTPRule** bij **Naam**
   - **Allow HTTP** bij **Beschrijving**
 
   ![Een NSG-regel maken](./media/load-balancer-get-started-internet-portal/8-load-balancer-nsg-rules.png)
4. Herhaal stap 2 en 3 om nog een regel te maken (**myRDPRule**) om een binnenkomende RDP-verbinding toe te staan via poort 3389. Gebruik de volgende waarden:
   - **Service Tag** bij **Bron**
   - **Internet** bij **Bronservicetag**
   - **3389** bij **Poortbereiken van doel**
   - **TCP** bij **Protocol**
   - **Allow** bij **Actie**
   - **200** bij **Prioriteit**
   - **myRDPRule** bij **Naam**
   - **Allow RDP** bij **Beschrijving**

   

### <a name="install-iis"></a>IIS installeren

1. Selecteer **Alle resources** in het menu aan de linkerkant. Selecteer in de lijst met resources **myVM1** in de resourcegroep **myResourceGroupLB**.
2. Selecteer op de pagina **Overzicht** de optie **Verbinding maken** om extern verbinding te maken met de VM.
3. Meld u aan bij de VM met de gebruikersnaam **azureuser** en het wachtwoord **Azure123456!**.
4. Ga op de serverdesktop naar **Windows Systeembeheer** > **Serverbeheer**.
5. Klik in Serverbeheer op **Beheren** en vervolgens op **Rollen en functies toevoegen**.
   ![Serverbeheerdersrol toevoegen](./media/load-balancer-get-started-internet-portal/servermanager.png)
6. Gebruik in de wizard Rollen en functies toevoegen de volgende waarden:
   - Selecteer op de pagina **Type installatie selecteren** de optie **Installatie die op de functie of het onderdeel is gebaseerd**.
   - Selecteer op de pagina **De doelserver selecteren** de optie **myVM1**.
   - Selecteer op de pagina **Een serverfunctie selecteren** de optie **Webserver (IIS)**.
   - Volg de aanwijzingen om de rest van de wizard uit te voeren. 
7. Herhaal stap 1 t/m 6 voor virtuele machine **myVM2**.

## <a name="create-resources-for-the-basic-load-balancer"></a>Resources voor de Basic load balancer maken

In deze sectie configureert u de instellingen voor de load balancer voor een back-endadresgroep en een statustest. U geeft ook load balancer en NAT-regels op.


### <a name="create-a-back-end-address-pool"></a>Een back-endadresgroep maken

Om verkeer te distribueren naar de VM's bevat een back-endadresgroep de IP-adressen van de virtuele netwerkinterfacekaarten (NIC's) die zijn verbonden met de load balancer. Maak de back-endadresgroep **myBackendPool** om **VM1** en **VM2** op te nemen.

1. Selecteer **Alle resources** in het linkermenu en selecteer vervolgens **myLoadBalancer**  in de lijst met resources.
2. Selecteer onder **Instellingen** de optie **Back-endpools** en selecteer vervolgens **Toevoegen**.
3. Voer de volgende stappen uit op de pagina **Een back-endpool toevoegen** en selecteer dan **OK**:
   - Voer bij **Naam** **myBackEndPool** in.
   - Selecteer bij **Gekoppeld aan** de optie **Beschikbaarheidsset** in de vervolgkeuzelijst.
   - Selecteer voor **Beschikbaarheidsset** de optie **myAvailabilitySet**.
   - Selecteer **Een doelnetwerk-IP-configuratie toevoegen** om elke virtuele machine die u hebt gemaakt (**myVM1** en**myVM2**), toe te voegen aan de back-endpool.

   ![Toevoegen aan de back-endadresgroep](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Zorg dat de instelling voor de back-endpool van de load balancer beide VM's (**VM1** en **VM2**) weergeeft.

### <a name="create-a-health-probe"></a>Een statustest maken

U gebruikt een statustest om de Basic Load Balancer de status van uw app te laten bewaken. De statustest voegt dynamisch VM's toe aan de load balancer-rotatie of verwijdert ze, op basis van hun reactie op statuscontroles. Maak een statustest genaamd **myHealthProbe** om de status van de VM's te bewaken.

1. Selecteer **Alle resources** in het linkermenu en selecteer vervolgens **myLoadBalancer**  in de lijst met resources.
2. Selecteer onder **Instellingen** de optie **Statustests** en selecteer vervolgens **Toevoegen**.
3. Gebruik deze waarden en selecteer vervolgens **OK**:
   - **myHealthProbe** als naam van de statustest
   - **HTTP** als protocoltype
   - **80** als poortnummer
   - **15** als **interval**, het aantal seconden tussen tests
   - **2** als **drempelwaarden voor onjuiste status**, het aantal opeenvolgende mislukte tests dat moet optreden voordat wordt besloten dat een VM een onjuiste status heeft

   ![Een test toevoegen](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Een load balancer-regel maken

Een load balancer-regel wordt gebruikt om de verdeling van het verkeer over de VM's te definiëren. U definieert de front-end-IP-configuratie voor het inkomende verkeer en de back-end-IP-groep om het verkeer te ontvangen, samen met de gewenste bron- en doelpoort. 

Maak een taakverdelingsregel met de naam **myLoadBalancerRuleWeb** om te luisteren op poort 80 van de front-end **LoadBalancerFrontEnd**. De regel is bedoeld ook voor het verzenden van netwerkverkeer met taakverdeling naar de back-endadresgroep **myBackEndPool**, ook via poort 80. 

1. Selecteer **Alle resources** in het linkermenu en selecteer vervolgens **myLoadBalancer**  in de lijst met resources.
2. Selecteer onder **Instellingen** **Taakverdelingsregels** en selecteer vervolgens **Toevoegen**.
3. Gebruik deze waarden en selecteer vervolgens **OK**:
   - **myHTTPRule** als naam van de taakverdelingsregel
   - **TCP** als protocoltype
   - **80** als poortnummer
   - **80** als back-endpoort
   - **myBackendPool** als naam van de back-endpool
   - **myHealthProbe** als naam van de statustest
    
   ![Een load balancer-regel toevoegen](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

## <a name="test-the-load-balancer"></a>Load balancer testen
1. Zoek op het scherm **Overzicht** het openbare IP-adres voor de load balancer op. Selecteer **Alle resources** en dan **myPublicIP**.

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. De standaardpagina van IIS-webserver wordt weergegeven in de browser.

   ![IIS-webserver](./media/load-balancer-get-started-internet-portal/9-load-balancer-test.png)

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de resourcegroep, de load balancer en alle gerelateerde resources verwijderen, wanneer u deze niet meer nodig hebt. Selecteer de resourcegroep die de load balancer bevat en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een resourcegroep, netwerkresources en back-endservers gemaakt. U hebt die bronnen vervolgens gebruikt om een load balancer te maken. Als u meer wilt weten over load balancers en de bijbehorende resources, gaat u door naar de zelfstudie-artikelen.
