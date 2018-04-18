---
title: Load Balancer VMs over beschikbaarheid zones - Azure-portal | Microsoft Docs
description: Maak een standaard Load Balancer met zone-redundante frontend naar de taakverdeling van virtuele machines over beschikbaarheid zones met Azure portal
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/18
ms.author: kumud
ms.openlocfilehash: ad476922342844a908961960407eb344711932f5
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="load-balance-vms-across-availability-zones-with-a-standard-load-balancer-using-the-azure-portal"></a>Taakverdeling van virtuele machines over beschikbaarheid zones met een standaard Load Balancer met de Azure portal

Dit artikel stappen bij het maken van een openbare Load Balancer Standard met een zone redundant frontend bereiken bereiken zone redundantie zonder afhankelijkheid van meerdere DNS-records. Er is één front-end-IP-adres in een standaard Load Balancer automatisch zone-redundante. U kunt nu een elke virtuele machine in een virtueel netwerk in een regio die op alle Zones voor beschikbaarheid bereiken met een zone redundant frontend voor de load balancer, met een enkel IP-adres. Gebruik beschikbaarheidszones om uw apps en gegevens te beschermen tegen het onwaarschijnlijke risico van een storing of het verloren gaan van een heel datacenter. Met zone-redundantie één of meer beschikbaarheid Zones kan mislukken en het gegevenspad blijft zo lang een zone in de regio blijft in orde is. 

Zie voor meer informatie over het gebruik van de beschikbaarheid van zones met standaard Load Balancer [standaard Load Balancer en beschikbaarheid Zones](load-balancer-standard-availability-zones.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij de Azure-portal op [http://portal.azure.com](http://portal.azure.com).

## <a name="create-a-public-standard-load-balancer"></a>Een openbare standaard Load Balancer maken

Standaard Load Balancer biedt alleen ondersteuning voor een standaard openbare IP-adres. Wanneer u een nieuwe openbare IP-adres tijdens het maken van de load balancer maakt, wordt automatisch geconfigureerd als een standaard SKU-versie en is ook automatisch zone-redundante.

1. Klik op de bovenste linkerkant van het scherm en **maken van een resource** > **Networking** > **Load Balancer**.
2. In de **maken van een load balancer** pagina Voer deze waarden in voor de load balancer:
    - *myLoadBalancer* - voor de naam van de load balancer.
    - **Openbare** : voor het type van de load balancer.
     - *myPublicIP* - voor de nieuwe openbare IP-adres dat u maakt. Om dit te doen, klikt u op **Kies een openbaar IP-adres**, en klik vervolgens op **nieuw**. Voor naamtype *myPublicIP*, SKU is standaard standaard en selecteer **Zone-redundante** voor **beschikbaarheid zone**.
    - *myResourceGroupLBAZ* - voor de naam van de nieuwe resourcegroep die u maakt.
    - **westeurope** - voor de locatie.
3. Klik op **maken** voor het maken van de load balancer.
   
    ![Een load balancer maken](./media/load-balancer-standard-public-availability-zones-portal/1a-load-balancer.png)


## <a name="create-backend-servers"></a>Maken van back-endservers

In deze sectie maakt u een virtueel netwerk maken, maakt u virtuele machines in verschillende zones (zone 1, 2 zone en zone 3) voor de regio waarin u wilt toevoegen aan de back-endpool van de load balancer en Installeer IIS op de virtuele machines om te testen van de zone-redundante load Proefbala ncer. Daarom als een zone mislukt, mislukt de test status voor de virtuele machine in dezelfde regio bevindt, en verkeer blijft door virtuele machines in de andere zones worden geleverd.

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
1. Klik op de bovenste linkerkant van het scherm **maken van een resource** > **Networking** > **virtueel netwerk** en voer deze waarden voor de virtueel netwerk:
    - *myVnet* - voor de naam van het virtuele netwerk.
    - *myResourceGroupLBAZ* - voor de naam van de bestaande resourcegroep
    - *myBackendSubnet* - voor de subnetnaam.
2. Klik op **maken** voor het maken van het virtuele netwerk.

    ![Een virtueel netwerk maken](./media/load-balancer-standard-public-availability-zones-portal/2-load-balancer-virtual-network.png)

## <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

1. Op de bovenste linkerkant van het scherm en klikt u op **maken van een resource**, de Typ in het zoekvak *Netwerkbeveiligingsgroep*, en op de pagina network security groep **maken**.
2. Voer deze waarden in de pagina maken netwerk beveiliging groep:
    - *myNetworkSecurityGroup* - voor de naam van de netwerkbeveiligingsgroep.
    - *myResourceGroupLBAZ* - voor de naam van de bestaande resourcegroep.
   
![Een virtueel netwerk maken](./media/load-balancer-standard-public-availability-zones-portal/create-nsg.png)

### <a name="create-nsg-rules"></a>NSG-regels maken

In deze sectie maakt u NSG-regels voor binnenkomende verbindingen met behulp van HTTP- en RDP via Azure portal.

1. Klik in de Azure-portal op **alle resources** in het menu links, en vervolgens zoeken en op **myNetworkSecurityGroup** die zich bevindt de **myResourceGroupLBAZ** resourcegroep.
2. Onder **instellingen**, klikt u op **inkomende beveiligingsregels**, en klik vervolgens op **toevoegen**.
3. Voer deze waarden in voor de inkomende beveiligingsregel met de naam *myHTTPRule* om toe te staan voor een binnenkomende HTTP-verbindingen via poort 80:
    - *Service-Tag* - voor **bron**.
    - *Internet* - voor **servicetag bron**
    - *80* - voor **poortbereiken bestemming**
    - *TCP* - voor **Protocol**
    - *Toestaan dat* - voor **actie**
    - *100* voor **prioriteit**
    - *myHTTPRule* voor de naam
    - *HTTP toestaan* - voor de beschrijving
4. Klik op **OK**.
 
 ![Een virtueel netwerk maken](./media/load-balancer-standard-public-availability-zones-portal/8-load-balancer-nsg-rules.png)
5. Herhaal stappen 2 tot 4 voor het maken van een andere regel met de naam *myRDPRule* om toe te staan voor een binnenkomend RDP-verbinding met behulp van poort 3389 met de volgende waarden:
    - *Service-Tag* - voor **bron**.
    - *Internet* - voor **servicetag bron**
    - *3389* - voor **poortbereiken bestemming**
    - *TCP* - voor **Protocol**
    - *Toestaan dat* - voor **actie**
    - *200* voor **prioriteit**
    - *myRDPRule* voor de naam
    - *RDP toestaan* - voor de beschrijving


### <a name="create-virtual-machines"></a>Virtuele machines maken

1. Klik op de bovenste linkerkant van het scherm en **maken van een resource** > **Compute** > **Windows Server 2016 Datacenter** en voer deze waarden voor de virtuele machine:
    - *myVM1* - voor de naam van de virtuele machine.        
    - *azureuser* - voor de gebruikersnaam van de beheerder.    
    - *myResourceGroupLBAZ* - voor **resourcegroep**, selecteer **gebruik bestaande**, en selecteer vervolgens *myResourceGroupLBAZ*.
2. Klik op **OK**.
3. Selecteer **DS1_V2** voor de grootte van de virtuele machine en klik op **Selecteer**.
4. Voer deze waarden voor de VM-instellingen:
    - *1 zone* - voor de zone waar u de virtuele machine wilt plaatsen.
    -  *myVNet* -Zorg ervoor dat deze optie is geselecteerd als het virtuele netwerk.
    - *myBackendSubnet* -Zorg ervoor dat deze optie is geselecteerd als het subnet.
    - *myNetworkSecurityGroup* - voor de naam van de netwerkbeveiligingsgroep (firewall).
5. Klik op **uitgeschakelde** diagnostische gegevens over opstarten uitschakelen.
6. Klik op **OK**, Controleer de instellingen op de pagina Samenvatting en klik vervolgens op **maken**.
  
 ![Een virtuele machine maken](./media/load-balancer-standard-public-availability-zones-portal/create-vm-standard-ip.png)

7. Maak een tweede VM, met de naam *VM2* in Zone 2 en de derde VM in Zone 3 en met *myVnet* als het virtuele netwerk *myBackendSubnet* als het subnet en * *myNetworkSecurityGroup* als het netwerk beveiliging groep met de stappen 1-6.

### <a name="install-iis-on-vms"></a>IIS installeren op virtuele machines

1. Klik op **alle resources** links in het menu en klik vervolgens in de lijst met resources op **myVM1** die zich bevindt de *myResourceGroupLBAZ* resourcegroep.
2. Op de **overzicht** pagina, klikt u op **Connect** voor RDP in de virtuele machine.
3. Meld u aan bij de virtuele machine met gebruikersnaam *azureuser*.
4. Op het bureaublad van de server, gaat u naar **Windows Systeembeheer**>**Serverbeheer**.
5. Klik op de pagina Quick Start Serverbeheer **functies en onderdelen toevoegen**.

   ![Toevoegingen doen aan de back-endadresgroep ](./media/load-balancer-standard-public-availability-zones-portal/servermanager.png)    

1. In de **Wizard Functies toevoegen en onderdelen**, gebruik de volgende waarden:
    - In de **installatietype selecteren** pagina, klikt u op **op basis van functie of onderdeel gebaseerde installatie**.
    - In de **Select doelserver** pagina, klikt u op **myVM1**.
    - In de **Selecteer serverfunctie** pagina, klikt u op **webserver (IIS)**.
    - Volg de instructies voor de rest van de wizard.
2. De RDP-sessie met de virtuele machine, sluit *myVM1*.
3. Herhaal stap 1 tot en met 7 IIS te installeren op virtuele machines *myVM2* en *myVM3*.

## <a name="create-load-balancer-resources"></a>Resources voor load balancer maken

In deze sectie kunt u configureren van instellingen van de load balancer voor een back-end-adresgroep en een health test en geef load balancer en NAT-regels.


### <a name="create-a-backend-address-pool"></a>Een back-end-adresgroep maken

Om verkeer te distribueren naar de VM's bevat een back-end-adresgroep de IP-adressen van de virtuele netwerkinterfacekaarten (NIC's) die zijn verbonden met de load balancer. Maak de back-end-adresgroep *myBackendPool* om op te nemen *VM1* en *VM2*.

1. Klik op **alle resources** in het menu links en klik vervolgens op **myLoadBalancer** uit de lijst met resources.
2. Onder **instellingen**, klikt u op **back-endpools**, klikt u vervolgens op **toevoegen**.
3. Op de **toevoegen van een back-endpool** pagina, de volgende handelingen uit:
    - Naam, typt u * myBackEndPool als de naam voor uw back endpool.
    - Voor **virtueel netwerk**, klik in de vervolgkeuzelijst op **myVNet**
    - Voor **virtuele machine**, in de vervolgkeuzelijst op klikt, **myVM1**.
    - Voor **IP-adres**, klik op het IP-adres van myVM1 in de vervolgkeuzelijst.
4. Klik op **nieuwe back-end-resource toevoegen** elke virtuele machine toe te voegen (*myVM2* en *myVM3*) toevoegen aan de back-endpool van de load balancer.
5. Klik op **Add**.

    ![Toevoegingen doen aan de back-endadresgroep ](./media/load-balancer-standard-public-availability-zones-portal/add-backend-pool.png)

3. Controleer of de instelling van de load balancer back-end van toepassingen worden weergegeven voor alle drie VM - **myVM1**, **myVM2** en **myVM3**.

### <a name="create-a-health-probe"></a>Een statustest maken

U gebruikt een statustest om de load balancer de status van uw app te laten bewaken. De statustest voegt dynamisch VM's toe aan de load balancer-rotatie of verwijdert ze, op basis van hun reactie op statuscontroles. Maken van een health test *myHealthProbe* voor het bewaken van de status van de virtuele machines.

1. Klik op **alle resources** in het menu links en klik vervolgens op **myLoadBalancer** uit de lijst met resources.
2. Onder **instellingen**, klikt u op **statuscontroles**, klikt u vervolgens op **toevoegen**.
3. Deze waarden gebruiken voor het maken van de health-test:
    - *myHealthProbe* - voor de naam van de health-test.
    - **HTTP** : voor het protocoltype.
    - *80* : voor het poortnummer.
    - *15* : voor het aantal **Interval** in seconden tussen testpogingen.
    - *2* : voor het aantal **drempelwaarde voor onjuiste status** of opeenvolgende testfouten dat optreden moeten voordat een virtuele machine is in orde beschouwd.
4. Klik op **OK**.

   ![Een test toevoegen](./media/load-balancer-standard-public-availability-zones-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Een load balancer-regel maken

Een load balancer-regel wordt gebruikt om de verdeling van het verkeer over de VM's te definiëren. U definieert de front-end-IP-configuratie voor het inkomende verkeer en de back-end-IP-groep om het verkeer te ontvangen, samen met de gewenste bron- en doelpoort. Maken van een load balancer-regel *myLoadBalancerRuleWeb* om te luisteren op poort 80 van de frontend *FrontendLoadBalancer* en taakverdeling van netwerkverkeer te verzenden naar de back-end-adresgroep *myBackEndPool* ook met behulp van poort 80. 

1. Klik op **alle resources** in het menu links en klik vervolgens op **myLoadBalancer** uit de lijst met resources.
2. Onder **instellingen**, klikt u op **Taakverdelingsregels**, klikt u vervolgens op **toevoegen**.
3. Deze waarden gebruiken voor het configureren van de taakverdelingsregel:
    - *myHTTPRule* - voor de naam van de regel voor taakverdeling.
    - **TCP** : voor het protocoltype.
    - *80* : voor het poortnummer.
    - *80* - voor de back endpoort.
    - *myBackendPool* - voor de naam van de back-endpool.
    - *myHealthProbe* - voor de naam van de health-test.
4. Klik op **OK**.
    
    ![Een taakverdelingsregel toevoegen](./media/load-balancer-standard-public-availability-zones-portal/load-balancing-rule.png)

## <a name="test-the-load-balancer"></a>De load balancer testen
1. Het openbare IP-adres vinden voor de Load Balancer op de **overzicht** scherm. Klik op **alle resources** en klik vervolgens op **myPublicIP**.

2. Het openbare IP-adres Kopieer en plak deze in de adresbalk van uw browser. De standaardpagina van IIS-webserver wordt weergegeven op de browser.

      ![IIS-webserver](./media/load-balancer-standard-public-availability-zones-portal/9-load-balancer-test.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet meer nodig, verwijderen van de resourcegroep, load balancer en alle gerelateerde resources. Om dit te doen, selecteert u de resourcegroep met de load balancer en klik op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [standaard Load Balancer](load-balancer-standard-overview.md).
