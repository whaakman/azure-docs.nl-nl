---
title: Meerdere sites met Azure Application Gateway | Microsoft Docs
description: Deze pagina bevat instructies voor het configureren van een bestaande Azure application gateway voor het hosten van meerdere webtoepassingen op dezelfde gateway met de Azure-portal.
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 95f892f6-fa27-47ee-b980-7abf4f2c66a9
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: davidmu
ms.openlocfilehash: 28a7fcb3e08a9c4b6a27e9fbc8d3ebae309adc62
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Een bestaande toepassingsgateway voor het hosten van meerdere webtoepassingen configureren

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-multisite-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)
> 
> 

Meerdere hosting-site, kunt u meer dan een webtoepassing op dezelfde toepassingsgateway implementeren. Is afhankelijk van de aanwezigheid van de host-header in de binnenkomende HTTP-aanvraag om te bepalen welke listener verkeer ontvangt. De listener stuurt vervolgens verkeer naar de juiste back-endpool zoals geconfigureerd in de definitie van de gateway. In webtoepassingen SSL is ingeschakeld, is de toepassingsgateway afhankelijk van de Server de naam van vermelding (SNI)-extensie voor kiest u de juiste listener voor internetverkeer. Vaak gebruikt voor het hosten van meerdere site is van aanvragen verdelen over voor verschillende webdomeinen op verschillende back-endserver opslaggroepen. Op dezelfde manier kunnen meerdere subdomeinen van het hoofddomein dezelfde ook worden gehost op dezelfde toepassingsgateway.

## <a name="scenario"></a>Scenario

In het volgende voorbeeld toepassingsgateway verkeer voor contoso.com en fabrikam.com bedient met twee back-endserver van toepassingen: contoso-servergroep en fabrikam-servergroep. Vergelijkbare setup kan worden gebruikt voor de host subdomeinen zoals app.contoso.com en blog.contoso.com.

![scenario voor meerdere locaties][multisite]

## <a name="before-you-begin"></a>Voordat u begint

Dit scenario voegt ondersteuning voor meerdere locaties aan een bestaande toepassingsgateway. Als u dit scenario, moet een bestaande toepassingsgateway beschikbaar om te configureren. Ga naar [een toepassingsgateway maken met behulp van de portal](application-gateway-create-gateway-portal.md) voor informatie over het maken van een basic-toepassingsgateway in de portal.

Hier volgen de stappen die nodig is om de toepassingsgateway:

1. Maak back-end-adresgroepen wilt gebruiken voor elke site.
2. Maak een listener voor elke site toepassingsgateway ondersteunt.
3. Maak regels om toe te wijzen elke listener met de juiste back-end.

## <a name="requirements"></a>Vereisten

* **Back-endserverpool:** de lijst met IP-adressen van de back-endservers. De IP-adressen moeten ofwel deel uitmaken van het subnet van het virtuele netwerk, ofwel openbare IP-/VIP-adressen zijn. FQDN-naam kan ook worden gebruikt.
* **Back-endserverpoolinstellingen:** elke pool heeft instellingen, zoals voor de poort, het protocol en de op cookies gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een pool en worden toegepast op alle servers in de pool.
* **Front-endpoort:** dit is de openbare poort die in de toepassingsgateway wordt geopend. Het verkeer komt binnen via deze poort en wordt vervolgens omgeleid naar een van de back-endservers.
* **Listener:** de listener beschikt over een front-endpoort, een protocol (Http of Https; deze waarden zijn hoofdlettergevoelig) en de SSL-certificaatnaam (als u SSL-offloading configureert). Voor meerdere locaties ingeschakeld Toepassingsgateways, hostnaam en SNI indicatoren ook worden toegevoegd.
* **Regel:** de regel verbindt de listener met de back-end-servergroep, en definieert naar welke back-endserverpool het verkeer moet worden omgeleid wanneer dit bij een bepaalde listener aankomt. Regels worden verwerkt in de volgorde die ze worden weergegeven en verkeer wordt doorgestuurd via de eerste regel die overeenkomt met ongeacht specifieke karakter. Bijvoorbeeld, als u een regel met behulp van een basic-listener en een regel met meerdere sites listener beide op dezelfde poort hebt, kan de regel met de listener voor meerdere locaties moet worden vermeld voordat u de regel met de basic-listener in volgorde voor de regel voor meerdere locaties werken zoals verwacht. 
* **Certificaten:** elke listener vereist een uniek certificaat, in dit voorbeeld 2 listeners worden gemaakt voor meerdere locaties. Twee pfx-certificaten en de wachtwoorden voor deze moeten worden gemaakt.

## <a name="create-back-end-pools-for-each-site"></a>Back-end-adresgroepen voor elke site maken

Een back-end van toepassingen voor elke site of toepassing gateway ondersteunt is vereist, in dit geval 2 worden gemaakt voor contoso11.com en één voor fabrikam11.com.

### <a name="step-1"></a>Stap 1

Navigeer naar een bestaande toepassingsgateway in de Azure portal (https://portal.azure.com). Selecteer **back-endpools** en klik op **toevoegen**

![back-endpools toevoegen][7]

### <a name="step-2"></a>Stap 2

Vul de gegevens voor de back-end-pool **pool1**, het toevoegen van de IP-adressen of de FQDN-namen voor de back-endservers en klikt u op **OK**

![back-end-groepsinstellingen pool1][8]

### <a name="step-3"></a>Stap 3

Klik op de blade back-end-adresgroepen **toevoegen** toevoegen van een aanvullende back-end-adresgroep **pool2**, het toevoegen van de IP-adressen of de FQDN-namen voor de back-endservers en klikt u op **OK**

![back-end pool2 groepsinstellingen][9]

## <a name="create-listeners-for-each-back-end"></a>Listeners voor elke back-end maken

Application Gateway maakt gebruik van HTTP 1.1-hostheaders voor het hosten van meer dan één website op hetzelfde openbare IP-adres en dezelfde poort. De basis-listener gemaakt in de portal bevat deze eigenschap niet.

### <a name="step-1"></a>Stap 1

Klik op **Listeners** op de bestaande toepassingsgateway en op **multi-site** om toe te voegen van de eerste listener.

![overzichtsblade listeners][1]

### <a name="step-2"></a>Stap 2

Vul de informatie voor de listener. In dit voorbeeld SSL beëindiging is geconfigureerd, maakt u een nieuwe frontend-poort. Upload het pfx-certificaat moet worden gebruikt voor SSL-beëindiging. Het enige verschil op deze blade vergeleken met de standaard basic listener-blade is de hostnaam.

![listener eigenschappenblade][2]

### <a name="step-3"></a>Stap 3

Klik op **multi-site** en een ander listener te maken, zoals beschreven in de vorige stap voor de tweede site. Zorg ervoor dat u een ander certificaat gebruikt voor de tweede listener. Het enige verschil op deze blade vergeleken met de standaard basic listener-blade is de hostnaam. Vul de informatie voor de listener en klik op **OK**.

![listener eigenschappenblade][3]

> [!NOTE]
> Maken van luisteraars in de Azure-portal voor application gateway is een langlopende taak op. Dit kan enige tijd voor het maken van de twee listeners in dit scenario. Wanneer u klaar de listeners weergeven in de portal, zoals in de volgende afbeelding:

![overzicht van de listener][4]

## <a name="create-rules-to-map-listeners-to-backend-pools"></a>Maak regels om te listeners worden toegewezen aan back-endpools

### <a name="step-1"></a>Stap 1

Navigeer naar een bestaande toepassingsgateway in de Azure portal (https://portal.azure.com). Selecteer **regels** en kiest u de bestaande standaardregel **rule1** en klik op **bewerken**.

### <a name="step-2"></a>Stap 2

Vul de blade regels zoals te zien is in de volgende afbeelding. De eerste listener en de eerste groep en te klikken op **opslaan** wanneer u klaar.

![Bewerk de bestaande regel][6]

### <a name="step-3"></a>Stap 3

Klik op **Basic regel** om de tweede regel te maken. Vul het formulier met de tweede listener en tweede back-endpool en op **OK** om op te slaan.

![blade basic regel toevoegen][10]

Dit scenario is voltooid op een bestaande toepassingsgateway configureren met ondersteuning voor meerdere locaties via de Azure portal.

## <a name="next-steps"></a>Volgende stappen

Informatie over het beveiligen van uw websites met [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png
