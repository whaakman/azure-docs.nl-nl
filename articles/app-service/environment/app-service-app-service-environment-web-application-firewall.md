---
title: Web Application Firewall (WAF) configureren voor App Service-omgeving
description: Informatie over het configureren van een web application firewall voor uw App Service-omgeving.
services: app-service\web
documentationcenter: 
author: naziml
manager: erikre
editor: jimbe
ms.assetid: a2101291-83ba-4169-98a2-2c0ed9a65e8d
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/17/2016
ms.author: naziml
ms.custom: mvc
ms.openlocfilehash: bfe36ee5365e71db4280e8e2ccff6db8e552dd39
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Web Application Firewall (WAF) configureren voor App Service-omgeving
## <a name="overview"></a>Overzicht
Web application firewalls, zoals de [Barracuda WAF voor Azure](https://www.barracuda.com/programs/azure) die beschikbaar is op de [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) helpt bij het beveiligen van uw webtoepassingen door te inspecteren inkomend webverkeer blokkeren SQL-injectie, Cross-Site Scripting, malware uploads & DDoS-toepassing en andere aanvallen. Deze ook inspecteert de antwoorden van de back-end-webservers gegevens gegevensverlies te voorkomen (DLP). In combinatie met de isolatie en aanvullende schalen geleverd door App Service-omgevingen, biedt dit een ideale omgeving naar host business kritieke webtoepassingen die bestand zijn tegen schadelijke aanvragen en grote hoeveelheden verkeer wilt.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Instellen
Voor dit document wordt de App-serviceomgeving achter meerdere exemplaren van de taakverdeling van Barracuda WAF zodanig configureren dat alleen het verkeer van de WAF App Service-omgeving bereiken kan en niet toegankelijk is vanaf het Perimeternetwerk is. We hebben ook Azure Traffic Manager voor de Barracuda WAF exemplaren te verdelen over Azure-datacenters en regio's. Een diagram op hoog niveau van de installatie eruit als in de volgende afbeelding:

![Architectuur][Architecture] 

> [!NOTE]
> Dankzij de introductie van [ILB ondersteuning voor App Service-omgeving](app-service-environment-with-internal-load-balancer.md), kunt u het as-omgeving om te worden niet toegankelijk vanaf het Perimeternetwerk en alleen beschikbaar voor het particuliere netwerk configureren. 
> 
> 

## <a name="configuring-your-app-service-environment"></a>Uw App-serviceomgeving configureren
Raadpleeg voor het configureren van een App-serviceomgeving [onze documentatie](app-service-web-how-to-create-an-app-service-environment.md) over dit onderwerp. Als u een App Service-omgeving gemaakt hebt, kunt u Web-Apps, API-Apps maken en [Mobile Apps](../../app-service-mobile/app-service-mobile-value-prop.md) in deze omgeving die al worden beveiligd achter de WAF we in de volgende sectie configureren.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Uw Barracuda WAF Cloud Service configureren
Barracuda heeft een [gedetailleerde artikel](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) over het implementeren van de WAF op een virtuele machine in Azure. Maar omdat we redundantie wilt en geen een potentieel risico introduceren, u wilt implementeren ten minste twee WAF exemplaar virtuele machines in dezelfde Cloud-Service wanneer deze instructies te volgen.

### <a name="adding-endpoints-to-cloud-service"></a>Toevoegen van eindpunten in de Cloud Service
Nadat u 2 hebt of meer WAF VM-in uw Cloud-Service exemplaren, kunt u de [Azure-portal](https://portal.azure.com/) toevoegen van HTTP en HTTPS-eindpunten die worden gebruikt door uw toepassing, zoals wordt weergegeven in de volgende afbeelding:

![-Eindpunt configureren][ConfigureEndpoint]

Als uw toepassingen met andere eindpunten, zorg er dan voor dat toe te voegen aan deze lijst ook. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Barracuda WAF via de beheerportal configureren
Barracuda WAF maakt gebruik van TCP-poort 8000 voor configuratie via de beheerportal. Als u meerdere exemplaren van de WAF virtuele machines hebt, moet u hier de stappen herhalen voor elke VM-instantie. 

> [!NOTE]
> Wanneer u klaar bent met WAF configuratie, moet u het TCP/8000-eindpunt verwijderen uit uw WAF VMs uw WAF om veilig te houden.
> 
> 

Het eindpunt management zoals weergegeven in de volgende afbeelding voor het configureren van uw Barracuda WAF toevoegen

![Management-eindpunt toevoegen][AddManagementEndpoint]

Een browser gebruiken om te bladeren naar het eindpunt van het beheer op uw Cloud-Service. Als uw Cloudservice test.cloudapp.net aangeroepen wordt, zou u dit eindpunt openen door te bladeren naar http://test.cloudapp.net:8000. U ziet een aanmeldingspagina zoals de volgende afbeelding die u kunt aanmelden met referenties die u hebt opgegeven in de installatiefase WAF VM.

![Aanmeldingspagina voor beheer][ManagementLoginPage]

Zodra u zich aanmeldt, moet u een dashboard zoals in de volgende afbeelding, waarin een elementaire statistieken over de beveiliging WAF zien.

![Management Dashboard][ManagementDashboard]

Klik op de **Services** tabblad kunt u uw WAF voor services die deze beveiligt configureren. Zie voor meer informatie over het configureren van uw WAF Barracuda [hun documentatie](https://techlib.barracuda.com/waf/getstarted1). In het volgende voorbeeld wordt is een Azure-Web-App voor verkeer op HTTP- en HTTPS geconfigureerd.

![Management-Services toevoegen][ManagementAddServices]

> [!NOTE]
> Afhankelijk van hoe uw toepassingen zijn geconfigureerd en welke functies worden gebruikt in uw App Service-omgeving, moet u voor het doorsturen van verkeer voor TCP-poorten dan 80 en 443, bijvoorbeeld, als u setup voor IP-SSL voor een Web-App. Zie voor een lijst met netwerkpoorten die worden gebruikt in App Service-omgevingen, [inkomend verkeer voor beheer-documentatie](app-service-app-service-environment-control-inbound-traffic.md) netwerkpoorten sectie.
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Configureren van Microsoft Azure Traffic Manager (optioneel)
Als uw toepassing beschikbaar in meerdere regio's, is wordt u wilt laden saldo achter [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md). Om dit te doen, kunt u een eindpunt in toevoegen de [Azure-portal](https://portal.azure.com) met de naam van de Cloudservice voor uw WAF in Traffic Manager-profiel, zoals wordt weergegeven in de volgende afbeelding. 

![Traffic Manager-eindpunt][TrafficManagerEndpoint]

Als uw toepassing verificatie vereist, zorg ervoor dat u hebt een resource die geen verificatie voor Traffic Manager te pingen niet vereist voor de beschikbaarheid van uw toepassing. U kunt de URL configureren op de **configuratie** pagina in de [Azure-portal](https://portal.azure.com) zoals weergegeven in de volgende afbeelding:

![Traffic Manager configureren][ConfigureTrafficManager]

Het Traffic Manager-pings verder vanaf uw WAF voor uw toepassing, moet u vertalingen van de Website op uw WAF Barracuda instellen voor het doorsturen van verkeer naar uw toepassing, zoals wordt weergegeven in het volgende voorbeeld:

![Vertalingen van de website][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Beveiligen van verkeer naar App Service-omgeving met behulp van Netwerkbeveiligingsgroepen (NSG)
Ga als volgt de [besturingselement binnenkomend verkeer documentatie](app-service-app-service-environment-control-inbound-traffic.md) voor meer informatie over het verkeer beperkt tot uw App Service-omgeving van de WAF alleen met behulp van de VIP-adres van uw Cloud-Service. Hier volgt een voorbeeld-Powershell-opdracht voor het uitvoeren van deze taak voor TCP-poort 80.

    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

De SourceAddressPrefix vervangen door het virtuele IP-adres (VIP) van uw WAF-Cloudservice.

> [!NOTE]
> Het VIP van uw Cloud-Service wordt gewijzigd wanneer u verwijderen en opnieuw maken van de Cloudservice. Zorg ervoor dat het IP-adres in de resourcegroep netwerk bijwerken nadat u doet dit. 
> 
> 

<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
