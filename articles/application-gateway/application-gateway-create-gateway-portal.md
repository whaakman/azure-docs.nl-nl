---
title: Maken van een toepassingsgateway - Azure-Portal | Microsoft Docs
description: Informatie over het maken van een toepassingsgateway met behulp van de portal
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 54dffe95-d802-4f86-9e2e-293f49bd1e06
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.openlocfilehash: 17d09ce98c40717d1db0927f791a7c97ea7835e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-application-gateway-with-the-portal"></a>Een toepassingsgateway maken met de portal

[Toepassingsgateway](application-gateway-introduction.md) is een toegewezen virtueel apparaat toepassing levering controller (ADC) bieden een service en biedt verschillende laag 7 taakverdeling mogelijkheden voor uw toepassing. In dit artikel leert u de stappen voor het maken van een toepassingsgateway met behulp van de Azure-portal en het toevoegen van een bestaande server als een back-end-lid.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij de Azure portal op [http://portal.azure.com](http://portal.azure.com)

## <a name="create-application-gateway"></a>Toepassingsgateway maken

Voltooi de stappen volgen voor het maken van een toepassingsgateway. Toepassingsgateway vereist een eigen subnet. Zorg ervoor dat u onvoldoende adresruimte voor meerdere subnetten hebben laten bij het maken van een virtueel netwerk. Nadat de toepassingsgateway met een subnet is geïmplementeerd, kunnen alleen andere Toepassingsgateways worden toegevoegd aan het.

1. Klik in het deelvenster Favorieten van de portal op **nieuw**
1. Klik op de blade **Nieuw** op **Netwerken**. In de **Networking** blade, klikt u op **Application Gateway**, zoals wordt weergegeven in de volgende afbeelding:

    ![Toepassingsgateway maken][1]

1. In de **basisbeginselen** blade die wordt weergegeven, voer de volgende waarden en klik vervolgens op **OK**:

   | **Instelling** | **Waarde** | **Details**|
   |---|---|---|
   |**Naam**|AdatumAppGateway|De naam van de toepassingsgateway|
   |**Laag**|Standard|Beschikbare waarden zijn standaard- en WAF. Ga naar [web application firewall](application-gateway-web-application-firewall-overview.md) voor meer informatie over WAF.|
   |**SKU-grootte**|Middelgroot|Opties bij het kiezen van de Standard-laag zijn een klein, middelgroot en groot. Bij het kiezen van WAF laag zijn opties alleen Medium en Large.|
   |**Aantal exemplaren**|2|Aantal exemplaren van de toepassingsgateway voor hoge beschikbaarheid. Het aantal instanties van 1 moeten alleen worden gebruikt voor testdoeleinden.|
   |**Abonnement**|[Uw abonnement]|Selecteer het abonnement waarin u de toepassingsgateway wilt maken.|
   |**Resourcegroep**|**Maken van nieuwe:** AdatumAppGatewayRG|Maak een resourcegroep. De naam van de resourcegroep moet uniek zijn binnen het abonnement dat u hebt geselecteerd. Lees het overzichtsartikel over [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups) voor meer informatie over resourcegroepen.|
   |**Locatie**|VS - west||

1. In de **instellingen** blade die wordt weergegeven onder **virtueel netwerk**, klikt u op **Kies een virtueel netwerk**. De **virtueel netwerk kiezen** blade wordt geopend.  Klik op **nieuw** openen de **virtueel netwerk maken** blade.

   ![Kies een virtueel netwerk][2]

1. Op de **de blade virtueel netwerk maken** Voer de volgende waarden en klik vervolgens op **OK**. De **virtueel netwerk maken** en **virtueel netwerk kiezen** blades sluiten. Deze stap vult de **Subnet** op de **instellingen** blade met het gekozen subnet.

   | **Instelling** | **Waarde** | **Details**|
   |---|---|---|
   |**Naam**|AdatumAppGatewayVNET|Naam van de toepassingsgateway|
   |**Adresruimte**|10.0.0.0/16|Dit is de adresruimte voor het virtuele netwerk|
   |**Subnetnaam**|AppGatewaySubnet|Naam van het subnet voor de toepassingsgateway|
   |**Subnetadresbereik**|10.0.0.0/28|Dit subnet kunt u meer extra subnetten in het virtuele netwerk backend groepsleden|

1. Op de **instellingen** blade onder **Frontend IP-configuratie**, kies **openbare** als de **type IP-adres**

1. Op de **instellingen** blade onder **openbaar IP-adres** klikt u op **Kies een openbaar IP-adres**, wordt de **openbare IP-adres kiezen** blade wordt geopend, klikt u op **nieuw**.

   ![openbare IP-adres kiezen][3]

1. Op de **openbare IP-adres maken** blade, accepteer de standaardwaarde, en klik op **OK**. De blade wordt gesloten en vult de **openbaar IP-adres** met het openbare IP-adres dat is gekozen.

1. Op de **instellingen** blade onder **listenerconfiguratie**, klikt u op **HTTP** onder **Protocol**. Geef de poort moet worden gebruikt in de **poort** veld.

2. Klik op **OK** op de **instellingen** blade om door te gaan.

1. Controleer de instellingen op de **samenvatting** blade en klik op **OK** te maken van de toepassingsgateway te starten. Een toepassingsgateway maken, is een langdurige taak en tijd in beslag neemt.

## <a name="add-servers-to-backend-pools"></a>Servers toevoegen aan back-endpools

Wanneer u de toepassingsgateway, de systemen die als host fungeren voor de toepassing maakt taakverdeling nog moeten worden toegevoegd aan de toepassingsgateway. De IP-adressen, FQDN-naam of NIC's van deze servers worden toegevoegd aan de back-end-adresgroepen.

### <a name="ip-address-or-fqdn"></a>IP-adres of FQDN-naam

1. Met de toepassingsgateway gemaakt in de Azure portal **Favorieten** deelvenster, klikt u op **alle resources**. Klik op de **AdatumAppGateway** toepassingsgateway in de blade met alle bronnen. Als het abonnement dat u hebt geselecteerd al verschillende bronnen heeft, kunt u **AdatumAppGateway** in de **filteren op naam...** voor eenvoudige toegang tot de toepassingsgateway.

1. De toepassingsgateway die u hebt gemaakt, wordt weergegeven. Klik op **back-endpools**, en selecteert u de huidige back-endpool **appGatewayBackendPool**, wordt de **appGatewayBackendPool** blade wordt geopend.

   ![Back-end van de toepassingsgroepen-Gateway][4]

1. Klik op **doel toevoegen** IP-adressen van de FQDN-waarden toevoegen. Kies **IP-adres of FQDN-naam** als de **Type** en voer uw IP-adres of FQDN-naam in het veld. Herhaal deze stap voor aanvullende back-end-groepsleden. Klik wanneer klaar **opslaan**.

### <a name="virtual-machine-and-nic"></a>Virtuele Machine en NIC

U kunt ook NIC's voor virtuele Machine toevoegen als leden van back-end-adresgroep. Alleen virtuele machines in hetzelfde virtuele netwerk als de toepassingsgateway zijn beschikbaar via de vervolgkeuzelijst.

1. Met de toepassingsgateway gemaakt in de Azure portal **Favorieten** deelvenster, klikt u op **alle resources**. Klik op de **AdatumAppGateway** toepassingsgateway in de blade met alle bronnen. Als het abonnement dat u hebt geselecteerd al verschillende bronnen heeft, kunt u **AdatumAppGateway** in de **filteren op naam...** voor eenvoudige toegang tot de toepassingsgateway.

1. De toepassingsgateway die u hebt gemaakt, wordt weergegeven. Klik op **back-endpools**, en selecteert u de huidige back-endpool **appGatewayBackendPool**, wordt de **appGatewayBackendPool** blade wordt geopend.

   ![Back-end van de toepassingsgroepen-Gateway][5]

1. Klik op **doel toevoegen** IP-adressen van de FQDN-waarden toevoegen. Kies **virtuele Machine** als de **Type** en selecteer de virtuele machine en NIC moet worden gebruikt. Klik wanneer klaar **opslaan**

   > [!NOTE]
   > Alleen virtuele machines in hetzelfde virtuele netwerk als de toepassingsgateway zijn beschikbaar in de vervolgkeuzelijst.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, de resourcegroep, toepassingsgateway en alle bijbehorende resources verwijderd. Om dit te doen, selecteert u de resourcegroep uit de application gateway-blade en klik op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In dit scenario moet u een toepassingsgateway geïmplementeerd en een server hebt toegevoegd aan de back-end. De volgende stappen zijn voor het configureren van de toepassingsgateway door instellingen aanpassen en aanpassen regels in de gateway. Deze stappen vindt u via de volgende artikelen:

Informatie over het maken van aangepaste statuscontroles in via [een aangepaste health test maken](application-gateway-create-probe-portal.md)

Informatie over het configureren van SSL-Offloading en nemen de kostbare SSL-ontsleuteling uit uw webservers in via [SSL-Offload configureren](application-gateway-ssl-portal.md)

Informatie over het beveiligen van uw toepassingen met [Web Application Firewall](application-gateway-webapplicationfirewall-overview.md) een functie van de toepassingsgateway.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
