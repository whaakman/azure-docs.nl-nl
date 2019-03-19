---
title: Hybride verbindingen maken en beheren | Microsoft Docs
description: Leer hoe u een hybride verbinding maken, beheren van de verbinding en het hybride Verbindingsbeheer installeren. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: erikre
editor: ''
ms.assetid: aac0546b-3bae-41e0-b874-583491a395ea
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: 584b9d67808e75222d832f045e5d3f93fbb326ae
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58105087"
---
# <a name="create-and-manage-hybrid-connections"></a>Hybride verbindingen maken en beheren

> [!IMPORTANT]
> Hybrid Connections van BizTalk is buiten gebruik gesteld en vervangen door App Service Hybrid Connections. Zie [Azure App Service Hybrid Connections](../app-service/app-service-hybrid-connections.md) voor meer informatie, zoals het beheren van uw bestaande BizTalk Hybrid Connections.
> 
> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="overview-of-the-steps"></a>Overzicht van de stappen
1. Een hybride verbinding maken met het invoeren van de **hostnaam** of **FQDN** van de on-premises bron in uw particuliere netwerk.
2. Uw Azure-web-apps of Azure mobile apps koppelen aan de hybride verbinding.
3. Hybrid Connection Manager installeren op uw on-premises bron en maak verbinding met de specifieke hybride verbinding. De Azure portal biedt een één-op-ervaring voor het installeren en verbinding maken.
4. Hybride verbindingen en hun verbindingssleutels beheren.

In dit onderwerp vindt u deze stappen. 

> [!IMPORTANT]
> Het is mogelijk om in te stellen van een eindpunt voor de hybride verbinding met een IP-adres. Als u een IP-adres gebruikt, moet u mogelijk of de on-premises bron, kan niet worden bereikt, afhankelijk van de client. De hybride verbinding is afhankelijk van de client een DNS-zoekopdracht uitvoeren. In de meeste gevallen de **client** code van uw toepassing is. Als de client een DNS-zoekopdracht voert (het probeert niet om op te lossen van het IP-adres alsof het naam van een domein (x.x.x.x)), en vervolgens verkeer niet via de hybride verbinding wordt verzonden.
> 
> Bijvoorbeeld (pseudocode), definieert u **10.4.5.6** als uw on-premises host:
> 
> **De volgende scenario werkt als volgt:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on premises host`
> 
> **De volgende scenario werkt niet:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`
> 
> 

## <a name="CreateHybridConnection"></a>Een hybride verbinding maken
Een hybride verbinding kan worden gemaakt in [Azure App Service Hybrid Connections](../app-service/app-service-hybrid-connections.md) **of** met behulp van [BizTalk Services REST-API's](https://msdn.microsoft.com/library/azure/dn232347.aspx). 

<!-- **To create Hybrid Connections using Web Apps**, see [Connect Azure Web Apps to an On-Premises Resource](../app-service-web/web-sites-hybrid-connection-get-started.md). You can also install the Hybrid Connection Manager (HCM) from your web app, which is the preferred method.  -->

#### <a name="additional"></a>Aanvullend
* Meerdere hybride verbindingen kunnen worden gemaakt. Zie de [BizTalk Services: Grafiek van edities](biztalk-editions-feature-chart.md) voor het aantal toegestane verbindingen. 
* Elke hybride verbinding is gemaakt met een combinatie van verbindingsreeksen: Sleutels van de toepassing die On-premises-sleutels die LUISTEREN en verzenden. Elk paar heeft een primaire en secundaire sleutel. 

## <a name="LinkWebSite"></a>Uw Azure App Service-Web-App of mobiele App koppelen
Als u wilt een Web-App of mobiele App in Azure App Service koppelen aan een bestaande hybride verbinding, selecteer **gebruik een bestaande hybride verbinding** in de blade Hybrid Connections. 
<!-- See [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md). -->

## <a name="InstallHCM"></a>Installeren van de Hybrid Connection Manager on-premises
Nadat een hybride verbinding is gemaakt, installeert u de Hybrid Connection Manager op de on-premises resource. Het kan worden gedownload van uw Azure-web-apps of van uw BizTalk Service. 

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]
 
[Azure App Service Hybrid Connections](../app-service/app-service-hybrid-connections.md) is ook een goede bron.

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Aanvullend
* Hybrid Connection Manager kan worden geïnstalleerd op de volgende besturingssystemen:
  
  * Windows Server 2008 R2 (.NET Framework 4.5 + en Windows Management Framework 4.0 + vereist)
  * Windows Server 2012 (Windows Management Framework 4.0 + vereist)
  * Windows Server 2012 R2
* Nadat u de Hybrid Connection Manager hebt geïnstalleerd, gebeurt het volgende: 
  
  * De hybride verbinding die wordt gehost op Azure wordt automatisch geconfigureerd voor het gebruik van de primaire verbindingsreeks voor de toepassing. 
  * De On-Premises resource wordt automatisch geconfigureerd voor het gebruik van de Primary Connection String voor On-Premises.
* Hybrid Connection Manager moet een geldige on-premises verbindingsreeks gebruiken voor autorisatie. De Azure-Web-Apps of mobiele Apps moet een geldige toepassings-verbindingsreeks gebruiken voor autorisatie.
* Hybrid Connections kunt u schalen door een ander exemplaar van de Hybrid Connection Manager installeren op een andere server. Configureer de on-premises-listener voor het gebruik van hetzelfde adres als de eerste listener voor de on-premises. In dit geval wordt het verkeer willekeurig gedistribueerde (round robin) tussen de actieve on-premises-listeners. 

## <a name="ManageHybridConnection"></a>Hybride verbindingen beheren

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)] 

[Azure App Service Hybrid Connections](../app-service/app-service-hybrid-connections.md) is ook een goede bron.

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Exemplaar/sleutel opnieuw genereren de hybride-verbindingsreeksen

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)] 

[Azure App Service Hybrid Connections](../app-service/app-service-hybrid-connections.md) is ook een goede bron.

#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Groepsbeleid gebruiken voor het beheren van de on-premises bronnen die worden gebruikt door een hybride verbinding
1. Download de [Hybrid Connection Manager Beheersjablonen](https://www.microsoft.com/download/details.aspx?id=42963).
2. Pak de bestanden.
3. Op de computer die wordt gewijzigd van Groepsbeleid, het volgende doen:  
   
   * Kopieer de. ADMX-bestanden naar de *%WINROOT%\PolicyDefinitions* map.
   * Kopieer de. ADML van bestanden naar de *%WINROOT%\PolicyDefinitions\en-us* map.

Eenmaal zijn gekopieerd, kunt u Editor voor Groepsbeleid om het beleid te wijzigen.

## <a name="next"></a>Volgende
[Overzicht van Hybrid Connections](integration-hybrid-connection-overview.md)

## <a name="see-also"></a>Zie ook
[REST API voor het beheren van BizTalk Services op Microsoft Azure](https://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk Services: Grafiek van edities](biztalk-editions-feature-chart.md)  
[Een BizTalk Service maken](biztalk-provision-services.md)  
[BizTalk Services: Tabbladen dashboard, bewaken en schalen](biztalk-dashboard-monitor-scale-tabs.md)

[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
