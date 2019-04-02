---
title: Firewall van Azure integreren met Azure Standard Load Balancer
description: Meer informatie over het integreren van de Firewall van Azure met Azure Standard Load Balancer
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 4/1/2019
ms.author: victorh
ms.openlocfilehash: 7ee92a7508918635849caafab4632bbba81ee628
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58805241"
---
# <a name="integrate-azure-firewall-with-azure-standard-load-balancer"></a>Firewall van Azure integreren met Azure Standard Load Balancer

U kunt een Azure-Firewall integreren in een virtueel netwerk met een Azure Standard Load Balancer (openbare of interne). 

Het ontwerp van voorkeur is een interne load balancer integreren met uw firewall van Azure, omdat dit een ontwerp voor veel eenvoudiger. U kunt een openbare load balancer gebruiken als u nog niet geïmplementeerd hebt en u wilt bewaren en op locatie. U moet echter rekening houden met een probleem van asymmetrische routering die functionaliteit met de openbare load balancer-scenario kan breken.

Zie voor meer informatie over Azure Load Balancer [wat is Azure Load Balancer?](../load-balancer/load-balancer-overview.md)

## <a name="public-load-balancer"></a>Openbare load balancer

De load balancer is met een openbare load balancer geïmplementeerd met een openbaar front-end-IP-adres.

### <a name="asymmetric-routing"></a>Asymmetrisch routeren

Asymmetrische routering is waar een pakket wordt een pad naar de bestemming en een ander pad neemt als u terugkeert naar de bron. Dit probleem treedt op wanneer een subnet is een standaard-route naar de privé IP-adres van de firewall en u een openbare load balancer. In dit geval wordt het binnenkomende verkeer van load balancer is ontvangen via het openbare IP-adres, maar de return pad verloopt via de privé IP-adres van de firewall. Aangezien de firewall stateful is, wordt het bestaande pakket geweigerd omdat de firewall niet op de hoogte van een vastgestelde sessie.

### <a name="fix-the-routing-issue"></a>Probleem met de routering

Wanneer u een Azure-Firewall in een subnet implementeert, is er één stap het maken van een standaardroute voor het subnet directing pakketten via de firewall van privé IP-adres zich op de AzureFirewallSubnet. Zie [Zelfstudie: Implementeren en configureren van de Firewall van Azure met behulp van de Azure-portal](tutorial-firewall-deploy-portal.md#create-a-default-route).

Wanneer u de firewall in uw load balancer-scenario introduceren, wilt u uw internetverkeer naar geleverd via het openbare IP-adres van uw firewall. Van daaruit geldt de firewall de firewall-regels en de NAT's de pakketten voor openbare IP-adres van de load balancer. Dit is waar het probleem zich voordoet. Pakketten op het openbare IP-adres van de firewall aankomt, maar gaat u terug naar de firewall via de privé IP-adres (met behulp van de standaardroute).
Om te voorkomen dat dit probleem, maakt u een extra hostroute voor openbare IP-adres van de firewall. Pakketten die naar het openbare IP-adres van de firewall worden gerouteerd via Internet. Hiermee voorkomt u de standaard-route naar de privé IP-adres van de firewall geleid.

![Asymmetrisch routeren](media/integrate-lb/Firewall-LB-asymmetric.png)

Bijvoorbeeld, zijn de volgende routes voor een firewall op openbare IP-adres 13.86.122.41 en privé-IP-adres 10.3.1.4.

![Routetabel](media/integrate-lb/route-table.png)

## <a name="internal-load-balancer"></a>Interne load balancer

De load balancer is met een interne load balancer geïmplementeerd met een privé front-end-IP-adres.

Er is geen probleem van asymmetrische routering met dit scenario. De inkomende pakketten in de openbare IP-adres van de firewall aankomt, krijgen omgezet naar de privé IP-adres van de load balancer en retourneert vervolgens naar de firewall van privé IP-adres met behulp van de dezelfde return pad.

Ja, u kunt dit scenario die vergelijkbaar is met het openbare load balancer-scenario implementeren, maar zonder de noodzaak voor de firewall van het openbare IP-adres hostroute.

## <a name="additional-security"></a>Extra beveiliging

Voor het verder verbeteren van de beveiliging van uw scenario met load balancing, kunt u netwerkbeveiligingsgroepen (nsg's).

U kunt bijvoorbeeld een NSG maken in de back endsubnet waar de netwerktaakverdeling virtuele machines zich bevinden. Toestaan van binnenkomend verkeer dat afkomstig is van de firewall voor IP-adres/poort.

Zie voor meer informatie over nsg's [beveiligingsgroepen](../virtual-network/security-overview.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [implementeren en configureren van een Azure-Firewall](tutorial-firewall-deploy-portal.md).