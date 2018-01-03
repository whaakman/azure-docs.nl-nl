---
title: Maken of bijwerken van een toepassingsgateway met een web application firewall | Microsoft Docs
description: Informatie over het maken van een toepassingsgateway met een web application firewall via de portal
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: b561a210-ed99-4ab4-be06-b49215e3255a
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: davidmu
ms.openlocfilehash: bfc06c1b44974fd17a3794654503d21d6407a917
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-an-application-gateway-with-a-web-application-firewall-by-using-the-portal"></a>Een toepassingsgateway maken met een web application firewall via de portal

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure-CLI](application-gateway-web-application-firewall-cli.md)

Informatie over het maken van een web application firewall (WAF)-toepassingsgateway ingeschakeld.

De WAF in Azure Application Gateway beveiligt webtoepassingen van algemene web gebaseerde aanvallen, zoals SQL-injectie, cross-site scripting aanvallen en sessie hijacks. Een WAF wordt beschermd tegen veel van de OWASP bovenste 10 veelvoorkomende web beveiligingslekken.

## <a name="scenarios"></a>Scenario's

Dit artikel biedt twee scenario's. In het eerste scenario, leert u hoe u [een toepassingsgateway maken met een WAF](#create-an-application-gateway-with-web-application-firewall). In het tweede scenario leert u hoe u [een WAF toevoegen aan een bestaande toepassingsgateway](#add-web-application-firewall-to-an-existing-application-gateway).

![Voorbeeldscenario][scenario]

> [!NOTE]
> U kunt aangepaste statuscontroles, back-end-adressen en extra regels toevoegen aan de toepassingsgateway. Deze toepassingen worden geconfigureerd nadat de toepassingsgateway is geconfigureerd en niet tijdens de eerste installatie.

## <a name="before-you-begin"></a>Voordat u begint

 Een application gateway vereist een eigen subnet. Wanneer u een virtueel netwerk maakt, zorg ervoor dat u onvoldoende adresruimte voor meerdere subnetten hebt verlaten. Nadat u een toepassingsgateway met een subnet hebt geïmplementeerd, kunnen alleen andere Toepassingsgateways worden toegevoegd aan het subnet.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Web application firewall toevoegen aan een bestaande application gateway

Een bestaande toepassingsgateway ter ondersteuning van een WAF in het volgende voorbeeld bijgewerkt **preventie** modus.

1. In de Azure portal **Favorieten** deelvenster **alle resources**. Op de **alle resources** blade, selecteert u de bestaande toepassingsgateway. Als het abonnement dat u hebt geselecteerd al verschillende resources heeft, voert u de naam in de **filteren op naam** vak voor eenvoudig toegang tot de DNS-zone.

   ![Bestaande gateway selectie van toepassing][1]

2. Selecteer **Web application firewall**, en werk de instellingen voor de toepassingsgateway. Wanneer de update is voltooid, selecteert u **opslaan**. 

3. Gebruik de volgende instellingen op een bestaande toepassingsgateway ter ondersteuning van een WAF bijwerken:

   | **Instelling** | **Waarde** | **Details**
   |---|---|---|
   |**Een upgrade uitvoert naar WAF laag**| Geselecteerd | Deze optie stelt de laag van de toepassingsgateway aan de laag WAF.|
   |**Firewall-status**| Ingeschakeld | Deze instelling schakelt u de firewall op de WAF.|
   |**Firewall-modus** | Preventie | Deze instelling is hoe een WAF omgaat met schadelijk verkeer. **Detectie** modus alleen de legt gebeurtenissen vast in. **Preventie** modus legt de gebeurtenissen en de schadelijk verkeer stopt.|
   |**Regelset**|3.0|Deze instelling bepaalt de [core regelset](application-gateway-web-application-firewall-overview.md#core-rule-sets) die wordt gebruikt voor het beveiligen van de leden van de back-end-pool.|
   |**Uitgeschakelde regels configureren**|Varieert|Om te voorkomen dat mogelijk valse positieven, kunt u deze instelling gebruiken om uit te schakelen op bepaalde [regels en regelgroepen](application-gateway-crs-rulegroups-rules.md).|

    >[!NOTE]
    > Wanneer u een bestaande toepassingsgateway op de SKU WAF bijwerkt, wordt de SKU-grootte verandert in **gemiddeld**. Nadat de configuratie is voltooid, kunt u deze instelling opnieuw configureren.

    ![Basisinstellingen][2-1]

    > [!NOTE]
    > WAF logboeken bekijken, diagnostische gegevens inschakelen en selecteren **ApplicationGatewayFirewallLog**. Kies het aantal exemplaren **1** doeleinden alleen voor testdoeleinden. We raden niet exemplaren onder **2** omdat deze niet wordt gedekt door de SLA. Kleine gateways zijn niet beschikbaar wanneer u een WAF gebruikt.

## <a name="create-an-application-gateway-with-a-web-application-firewall"></a>Een toepassingsgateway maken met een web application firewall

Dit scenario wordt:

* Maak een gemiddeld WAF toepassingsgateway met twee exemplaren.
* Maak een virtueel netwerk met de naam AdatumAppGatewayVNET met een gereserveerd CIDR-blok van 10.0.0.0/16.
* Maakt u een subnet met de naam Appgatewaysubnet dat gebruikmaakt van 10.0.0.0/28 als CIDR-blok.
* Een certificaat voor SSL-offload configureren.

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Als u nog een account hebt, kunt u zich aanmelden voor een [gratis proefversie van één maand](https://azure.microsoft.com/free).

2. In de **Favorieten** deelvenster op de portal, selecteert **nieuw**.

3. Op de **nieuw** blade Selecteer **Networking**. Op de **Networking** blade Selecteer **Application Gateway**, zoals wordt weergegeven in de volgende afbeelding:

    ![Maken van de toepassingsgateway][1]

4. Op de **basisbeginselen** blade die wordt weergegeven, voer de volgende waarden en selecteer vervolgens **OK**:

   | **Instelling** | **Waarde** | **Details**
   |---|---|---|
   |**Naam**|AdatumAppGateway|De naam van de toepassingsgateway.|
   |**Laag**|WAF|Beschikbare waarden zijn standaard- en WAF. Zie voor meer informatie over een WAF, [Web application firewall](application-gateway-web-application-firewall-overview.md).|
   |**SKU-grootte**|Middelgroot|Standard-laag opties zijn **kleine**, **gemiddeld**, en **grote**. Opties voor laag WAF **gemiddeld** en **grote** alleen.|
   |**Aantal exemplaren**|2|Het aantal exemplaren van de toepassingsgateway voor hoge beschikbaarheid. Het aantal instanties van 1 alleen voor testdoeleinden gebruiken.|
   |**Abonnement**|[Uw abonnement]|Selecteer een abonnement te gebruiken om de toepassingsgateway te maken.|
   |**Resourcegroep**|**Maken van nieuwe:** AdatumAppGatewayRG|Maak een resourcegroep. De naam van de resourcegroep moet uniek zijn binnen het abonnement dat u hebt geselecteerd. Lees het overzichtsartikel over [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups) voor meer informatie over resourcegroepen.|
   |**Locatie**|VS - west||

   ![Basisinstellingen configureren][2-2]

5. Op de **instellingen** blade die wordt weergegeven onder **virtueel netwerk**, selecteer **Kies een virtueel netwerk**. Op de **virtueel netwerk kiezen** blade Selecteer **nieuw**.

   ![Virtueel netwerk kiezen][2]

6. Op de **de blade virtueel netwerk maken**, voer de volgende waarden en selecteer vervolgens **OK**. De **Subnet** op de **instellingen** blade is gevuld met het subnet dat u hebt gekozen.

   |**Instelling** | **Waarde** | **Details** |
   |---|---|---|
   |**Naam**|AdatumAppGatewayVNET|De naam van de toepassingsgateway.|
   |**Adresruimte**|10.0.0.0/16| Deze waarde is de adresruimte voor het virtuele netwerk.|
   |**Subnetnaam**|AppGatewaySubnet|De naam van het subnet voor de toepassingsgateway.|
   |**Subnetadresbereik**|10.0.0.0/28 | Dit subnet kan meer extra subnetten in het virtuele netwerk voor leden van de back-end-pool.|

7. Op de **instellingen** blade onder **Frontend IP-configuratie**, selecteer **openbare** als de **type IP-adres**.

8. Op de **instellingen** blade onder **openbaar IP-adres**, selecteer **Kies een openbaar IP-adres**. Op de **openbare IP-adres kiezen** blade Selecteer **nieuw**.

   ![Openbare IP-adres kiezen][3]

9. Op de **openbare IP-adres maken** blade, accepteer de standaardwaarde en selecteer **OK**. De **openbaar IP-adres** veld is gevuld met het openbare IP-adres dat u hebt gekozen.

10. Op de **instellingen** blade onder **listenerconfiguratie**, selecteer **HTTP** onder **Protocol**. Een certificaat is vereist voor het gebruik van **HTTPS**. De persoonlijke sleutel voor het certificaat is vereist. Geef een .pfx-export van het certificaat op en voer het wachtwoord voor het bestand.

11. Configureren met specifieke instellingen voor de **WAF**.

   |**Instelling** | **Waarde** | **Details** |
   |---|---|---|
   |**Firewall-status**| Ingeschakeld| Deze instelling schakelt de WAF in- of uitschakelen.|
   |**Firewall-modus** | Preventie| Deze instelling bepaalt de acties die de WAF schadelijk verkeer neemt. **Detectie** modus registreert alleen verkeer. **Preventie** modus logboeken en stopt verkeer met een niet-geautoriseerde 403-antwoord.|


12. Controleer de **samenvatting** pagina en selecteer **OK**. De toepassingsgateway is nu in de wachtrij geplaatst en gemaakt.

13. Nadat de gateway is gemaakt, gaat u naar het in de portal om door te gaan met de configuratie van de toepassingsgateway.

    ![Application gateway resource weergeven][10]

Deze stappen maken een basic-toepassingsgateway met standaardinstellingen voor de listener, back-end-pool, back-end-HTTP-instellingen en -regels Na de inrichting is voltooid, kunt u deze instellingen aanpassen aan uw implementatie.

> [!NOTE]
> Toepassingsgateways gemaakt met de basisconfiguratie van WAF zijn geconfigureerd met CRS 3.0 voor beveiliging.

## <a name="next-steps"></a>Volgende stappen

Voor het configureren van een aangepast domeinalias voor de [openbaar IP-adres](../dns/dns-custom-domain.md#public-ip-address), kunt u Azure DNS of een andere DNS-provider.

Zie configureren van diagnostische logboekregistratie voor logboekregistratie van de gebeurtenissen die zijn gedetecteerd of voorkomen met WAF [Application Gateway diagnostics](application-gateway-diagnostics.md).

Zie het maken van aangepaste statuscontroles [maken van een aangepaste health test](application-gateway-create-probe-portal.md).

Zie voor het configureren van SSL-offloading en nemen de kostbare SSL-abonnement uit uw webservers, [configureren van SSL-offload](application-gateway-ssl-portal.md).

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[2-1]: ./media/application-gateway-web-application-firewall-portal/figure2-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png
