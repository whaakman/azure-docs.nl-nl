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
ms.date: 11/27/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 2e4e4e7cb1ae49a856bbfed0716936b7b5b13d19
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52635098"
---
# <a name="quickstart-create-a-public-basic-load-balancer-by-using-the-azure-portal"></a>Snelstart: Een openbare Basic Load Balancer maken met behulp van de Azure Portal

Taakverdeling zorgt voor een hogere beschikbaarheid en betere schaalbaarheid door binnenkomende aanvragen te spreiden over meerdere virtuele machines (VM's). U kunt de Azure-portal gebruiken om een load balancer te maken en verkeer over virtuele machines te verdelen. In deze quickstart wordt beschreven hoe u een load balancer, back-endservers en netwerkresources kunt maken in de prijscategorie Basic.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

Als u de taken in deze quickstart wilt uitvoeren, moet u zich aanmelden bij de [Azure-portal](http://portal.azure.com).

## <a name="create-a-basic-load-balancer"></a>Een Basic Load Balancer maken

Maak eerst een openbare Basic Load Balancer met behulp van de portal. De naam en het openbare IP-adres die u maakt, worden automatisch geconfigureerd als de front-end van de load balancer.

1. Selecteer **Een resource maken** > **Netwerken** > **Load Balancer** linksboven in het scherm.
   
1. Typ of selecteer de volgende waarden in het deelvenster **Load balancer maken**:
   
   - **Naam**: typ *MyLoadBalancer*.
   - **Type**: selecteer **Openbaar**. 
   - **SKU**: selecteer **Basic**.
   - **Openbaar IP-adres**: selecteer **Nieuwe maken**. 
     - Veld **Openbaar IP-adres**: typ *MyPublicIP*.
     - **Openbaar IP-adres configureren** > **Toewijzing**: selecteer **Dynamisch**.
   - **Resourcegroep**: selecteer **Nieuwe maken**, voer vervolgens *MyResourceGroupLB* in en selecteer **OK**. 
   
1. Selecteer **Maken**.
   
![Een load balancer maken](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)

## <a name="create-back-end-servers"></a>Back-endservers maken

Vervolgens maakt u een virtueel netwerk en twee virtuele machines voor de back-endpool van uw Basic Load Balancer. 

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

1. Selecteer **Een resource maken** > **Netwerken** > **Virtueel netwerk** linksboven in de portal.
   
1. In het deelvenster **Virtueel netwerk maken** typt of selecteert u de volgende waarden:
   
   - **Naam**: typ *MyVnet*.
   - **Resourcegroep**: selecteer in de vervolgkeuzelijst **Selecteer een bestaande** de optie **MyResourceGroupLB**. 
   - **Subnet** > **Naam**: typ *MyBackendSubnet*.
   
1. Selecteer **Maken**.

   ![Een virtueel netwerk maken](./media/load-balancer-get-started-internet-portal/2-load-balancer-virtual-network.png)

### <a name="create-virtual-machines"></a>Virtuele machines maken

1. Selecteer **Een resource maken** > **Compute** > **Windows Server 2016 Datacenter** linksboven in de portal. 
   
1. In **Een virtuele machine maken** typt of selecteert u de volgende waarden op het tabblad **Basisinformatie**:
   - **Abonnement** > **Resourcegroep**: selecteer **MyResourceGroupLB** in de vervolgkeuzelijst.
   - **Instantiedetails** > **Naam van virtuele machine**: typ *MyVM1*.
   - **Instantiedetails** > **Beschikbaarheidsopties**: 
     1. Selecteer **Beschikbaarheidsset** in de vervolgkeuzelijst. 
     2. Selecteer **Nieuwe maken**, typ *MyAvailabilitySet* en selecteer **OK**.
   - **Administrator-account** > **Gebruikersnaam**: typ *azureuser*.
   - **Administrator-account** > **Wachtwoord**: typ *Azure1234567*. 
     Typ nogmaals het wachtwoord in het veld **Wachtwoord bevestigen**.
   
1. Selecteer het tabblad **Netwerken** of selecteer **Volgende: Schijven** en vervolgens **Volgende: Netwerken**. 
   
   Zorg ervoor dat de volgende opties zijn geselecteerd:
   - **Virtueel netwerk**: **MyVnet**
   - **Subnet**: **MyBackendSubnet**
   - **Public IP**: **MyVM1-ip**
   
   Als u een nieuwe netwerkbeveiligingsgroep (NSG) wilt maken, een soort firewall, selecteert u onder **Netwerkbeveiligingsgroep** de optie **Geavanceerd**. 
   1. Selecteer in het veld **Netwerkbeveiligingsgroep configureren** de optie **Nieuwe maken**. 
   1. Typ *MyNetworkSecurityGroup* en selecteer **OK**. 
   
1. Selecteer het tabblad **Beheer** of selecteer **Volgende** > **Beheer**. Stel bij **Bewaking** **Diagnostische gegevens over opstarten** in op **Uit**.
   
1. Selecteer **Controleren + maken**.
   
1. Controleer de instellingen en selecteer vervolgens **Maken**. 

1. Volg de stappen voor het maken van een tweede virtuele machine met de naam *MyVM2* en het **openbaar IP**-adres *MyVM2-ip*, en stel daarvoor alle andere instellingen in zoals bij MyVM1. 

### <a name="create-nsg-rules-for-the-vms"></a>NSG-regels maken voor de virtuele machines

In deze sectie maakt u regels voor de netwerkbeveiligingsgroep (NSG) voor de VM's om binnenkomende internet- (HTTP) en extern-bureaublad (RDP)-verbindingen toe te staan.

1. Selecteer **Alle resources** in het menu aan de linkerkant. Selecteer in de lijst met resources **MyNetworkSecurityGroup** in de resourcegroep **MyResourceGroupLB**.
   
1. Selecteer onder **Instellingen** **Inkomende beveiligingsregels** en selecteer vervolgens **Toevoegen**.
   
1. Typ of selecteer in het dialoogvenster **Binnenkomende beveiligingsregel toevoegen** voor de HTTP-regel de volgende waarden:
   
   - **Bron**: selecteer **Servicetag**.  
   - **Bronservicetag**: selecteer **Internet**. 
   - **Poortbereiken van doel**: typ *80*.
   - **Protocol**: selecteer **TCP** . 
   - **Actie**: selecteer **Toestaan**.  
   - **Prioriteit**: typ *100*. 
   - **Naam**: typ *MyHTTPRule*. 
   - **Beschrijving**: typ *HTTP toestaan*. 
   
1. Selecteer **Toevoegen**. 
   
   ![Een NSG-regel maken](./media/load-balancer-get-started-internet-portal/8-load-balancer-nsg-rules.png)
   
1. Herhaal de stappen voor de inkomende RDP-regel met de volgende, afwijkende waarden:
   - **Poortbereiken van doel**: typ *3389*.
   - **Prioriteit**: typ *200*. 
   - **Naam**: typ *MyRDPRule*. 
   - **Beschrijving**: typ *RDP toestaan*. 

## <a name="create-resources-for-the-load-balancer"></a>Resources voor de load balancer maken

In deze sectie configureert u de instellingen van de load balancer voor een back-endadresgroep, een statustest en een regel voor de load balancer.

### <a name="create-a-back-end-address-pool"></a>Een back-endadresgroep maken

De load balancer gebruikt een back-end-adresgroep om verkeer te distribueren over de virtuele machines. De back-endadresgroep bevat de IP-adressen van de virtuele netwerkinterfaces (NIC's) die zijn verbonden met de load balancer. 

**Voor het maken van een back-end-adresgroep die VM1 en VM2 bevat:**

1. Selecteer **Alle resources** in het linkermenu en selecteer vervolgens **MyLoadBalancer** in de lijst met resources.
   
1. Selecteer onder **Instellingen** de optie **Back-endpools** en selecteer vervolgens **Toevoegen**.
   
1. Op de pagina **Een back-endpool toevoegen** typt of selecteert u de volgende waarden:
   
   - **Naam**: typ *MyBackendPool*.
   - **Gekoppeld aan**: selecteer in de vervolgkeuzelijst **Beschikbaarheidsset**.
   - **Beschikbaarheidsset**: selecteer **MyAvailabilitySet**.
   
1. Selecteer **Een doel-netwerk-IP-configuratie toevoegen**. 
   1. Voeg elke virtuele machine (**MyVM1** en **MyVM2**) die u hebt gemaakt toe aan de back-endpool.
   2. Nadat u alle machines hebt toegevoegd, selecteert u in de vervolgkeuzelijst de bijbehorende **netwerk-IP-configuratie**. 
   
1. Selecteer **OK**.
   
   ![De back-endadresgroep toevoegen](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)
   
1. Vouw op de pagina **Back-endpools** **MyBackendPool** uit en controleer of zowel **VM1** als **VM2** worden vermeld.

### <a name="create-a-health-probe"></a>Een statustest maken

U gebruikt een statustest om de load balancer toe te staan de status van uw virtuele machine te bewaken. De statustest voegt dynamisch VM's toe aan de load balancer-rotatie of verwijdert ze, op basis van hun reactie op statuscontroles. 

**Voor het maken van een statustest om de status van de VM's te bewaken:**

1. Selecteer **Alle resources** in het linkermenu en selecteer vervolgens **MyLoadBalancer** in de lijst met resources.
   
1. Selecteer onder **Instellingen** de optie **Statustests** en selecteer vervolgens **Toevoegen**.
   
1. Op de pagina **Een statustest toevoegen** typt of selecteert u de volgende waarden:
   
   - **Naam**: typ *MyHealthProbe*.
   - **Protocol**: selecteer **HTTP** in de vervolgkeuzelijst. 
   - **Poort**: typ *80*. 
   - **Pad**: accepteer */* als de standaard-URI. U kunt deze waarde vervangen door een andere URI. 
   - **Interval**: typ *15*. Een interval is het aantal seconden tussen tests.
   - **Drempelwaarde voor onjuiste status**: typ *2*. Deze waarde duidt het aantal opeenvolgende mislukte tests aan dat moet optreden voordat wordt besloten dat een VM een onjuiste status heeft.
   
1. Selecteer **OK**.
   
   ![Test toevoegen](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Een load balancer-regel maken

Een load balancer-regel bepaalt hoe het verkeer over de VM's wordt verdeeld. De regel definieert de front-end-IP-configuratie voor het inkomende verkeer, de back-end-IP-groep om het verkeer te ontvangen en de vereiste bron- en doelpoorten. 

De load balancer-regel met de naam **myLoadBalancerRuleWeb** luistert op poort 80 van de front-end **LoadBalancerFrontEnd**. Met de regel wordt netwerkverkeer naar de back-endadresgroep **MyBackEndPool** verzonden, ook op poort 80. 

**Voor het maken van een load balancer-regel:**


1. Selecteer **Alle resources** in het linkermenu en selecteer vervolgens **MyLoadBalancer** in de lijst met resources.
   
1. Selecteer onder **Instellingen** **Taakverdelingsregels** en selecteer vervolgens **Toevoegen**.
   
1. Op de pagina **Load balancer-regel toevoegen** typt of selecteert u de volgende waarden:
   
   - **Naam**: typ *MyLoadBalancerRule*.
   - **Frontend-IP-adres**: typ *LoadBalancerFrontEnd*.
   - **Protocol**: selecteer **TCP** .
   - **Poort**: typ *80*.
   - **Back-endpoort**: typ *80*.
   - **Back-endpool**: selecteer **MyBackendPool**.
   - **Statustest**: selecteer **MyHealthProbe**. 
   
1. Selecteer **OK**.
   
  ![Load balancer-regel toevoegen](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

## <a name="test-the-load-balancer"></a>Load balancer testen

U gebruikt het openbare IP-adres om de load balancer op de VM's te testen. 

Zoek in de portal, op de **overzichtspagina** voor **MyLoadBalancer**, het openbare IP-adres onder **Openbaar IP-adres**. Beweeg de muisaanwijzer boven het adres en selecteer het pictogram **Kopiëren** om het te kopiëren. 

### <a name="install-iis-on-the-vms"></a>IIS installeren op de VM's

Installeer Internet Information Services (IIS) op de virtuele machines om de load balancer te testen.

**Via Extern bureaublad naar de VM:**

1. Selecteer in de portal **Alle resources** in het menu aan de linkerkant. Selecteer in de lijst met resources **MyVM1** in de resourcegroep **MyResourceGroupLB**.
   
1. Op de **overzichtspagina** selecteert u **Verbinding maken** en vervolgens **RDP-bestand downloaden**. 
   
1. Open het gedownloade RDP-bestand en selecteer **Verbinding maken**.
   
1. Selecteer in het scherm Windows-beveiliging **Meer opties** en vervolgens **Een ander account gebruiken**. 
   
   Voer de gebruikersnaam *azureuser* en het wachtwoord *Azure1234567* in en selecteer **OK**.
   
1. Antwoord met **Ja** als u om een certificaat wordt gevraagd. 
   
   Het bureaublad van de virtuele machine wordt in een nieuw venster geopend. 
   
**IIS installeren op de VM:**

1. Als **Serverbeheer** nog niet op het bureaublad van de server is geopend, gaat u naar **Windows Systeembeheer** > **Serverbeheer**.
   
1. Selecteer in **Serverbeheer** de optie **Add roles and features**.
   
   ![Serverbeheerdersrol toevoegen](./media/load-balancer-get-started-internet-portal/servermanager.png)
   
1. In **Add Roles and Features Wizard**:
   1. Selecteer op de pagina **Type installatie selecteren** de optie **Installatie die op de functie of het onderdeel is gebaseerd**.
   1. Selecteer op de pagina **De doelserver selecteren** de optie **MyVM1**.
   1. Selecteer op de pagina **Een serverfunctie selecteren** de optie **Webserver (IIS)**. 
   1. Als u wordt gevraagd de vereiste hulpprogramma's te installeren, selecteert u **Onderdelen toevoegen**. 
   1. Accepteer de standaardwaarden en selecteer **Installeren**. 
   1. Als de onderdelen zijn geïnstalleerd, selecteert u **Sluiten**. 
   
1. Herhaal de stappen voor de virtuele machine **MyVM2**, maar stel de doelserver in op **MyVM2**.

### <a name="test-the-load-balancer"></a>Load balancer testen

Open voor elke VM een browser en antwoord met **OK** op eventuele vragen over de configuratie. 

Plak het openbare IP-adres van de load balancer in de adresbalk van de browser. De standaardpagina van de IIS-webserver wordt weergegeven in de browser.

![IIS-webserver](./media/load-balancer-get-started-internet-portal/9-load-balancer-test.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u de load balancer en alle bijbehorende resources wilt verwijderen omdat u deze niet meer nodig hebt, opent u de resourcegroep **MyResourceGroupLB** en selecteert u **Resourcegroep verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een load balancer van het niveau Basic gemaakt. U hebt een resourcegroep, netwerkresources, back-endservers, een statustest en regels voor de load balancer gemaakt en geconfigureerd. U hebt IIS op de VM's geïnstalleerd en deze gebruikt om de load balancer te testen. 

Voor meer informatie over Azure Load Balancer gaat u door naar de zelfstudies.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Load Balancer](tutorial-load-balancer-basic-internal-portal.md)
