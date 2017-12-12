---
title: Hybride verbindingen maken en beheren | Microsoft Docs
description: Informatie over het maken van een hybride verbinding, het beheren van de verbinding en het installeren van het hybride Verbindingsbeheer. MABS, WABS
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: erikre
editor: 
ms.assetid: aac0546b-3bae-41e0-b874-583491a395ea
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: 1751d33b5f6f6a506654daedd15bbd75ae271483
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="create-and-manage-hybrid-connections"></a>Hybride verbindingen maken en beheren

> [!IMPORTANT]
> Hybrid Connections van BizTalk is buiten gebruik gesteld en vervangen door App Service Hybrid Connections. Zie [Azure App Service Hybrid Connections](../app-service/app-service-hybrid-connections.md) voor meer informatie, zoals het beheren van uw bestaande BizTalk Hybrid Connections.

>[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="overview-of-the-steps"></a>Overzicht van de stappen
1. Een hybride verbinding maken met het invoeren van de **hostnaam** of **FQDN** van de lokale resource in uw particuliere netwerk.
2. Koppel uw Azure-web-apps of mobiele apps in Azure aan de hybride verbinding.
3. De hybride Verbindingsbeheer installeren op uw lokale resource en maak verbinding met de specifieke hybride verbinding. De Azure portal biedt een één-op-ervaring voor het installeren en verbinding maken.
4. Hybride verbindingen en hun verbindingssleutels beheren.

In dit onderwerp vindt u deze stappen. 

> [!IMPORTANT]
> Het is mogelijk een hybride verbinding eindpunt instellen op een IP-adres. Als u een IP-adres gebruikt, moet u mogelijk of de lokale resource, kan niet worden bereikt, afhankelijk van de client. De hybride verbinding is afhankelijk van de client een DNS-zoekopdracht uitvoeren. In de meeste gevallen de **client** is uw toepassingscode. Als de client een DNS-zoekopdracht voert (deze probeert zichzelf niet omzetten van het IP-adres, alsof deze een domeinnaam (x.x.x.x)), en vervolgens verkeer niet via de hybride verbinding wordt verzonden.
> 
> Bijvoorbeeld (pseudocode), definieert u **10.4.5.6** als uw lokale host:
> 
> **Het volgende scenario werkt:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **Het volgende scenario werkt niet:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`
> 
> 

## <a name="CreateHybridConnection"></a>Een hybride verbinding maken
Een hybride verbinding kunnen worden gemaakt in [Azure App Service hybride verbindingen](../app-service/app-service-hybrid-connections.md) **of** met [BizTalk Services REST-API's](https://msdn.microsoft.com/library/azure/dn232347.aspx). 

<!-- **To create Hybrid Connections using Web Apps**, see [Connect Azure Web Apps to an On-Premises Resource](../app-service-web/web-sites-hybrid-connection-get-started.md). You can also install the Hybrid Connection Manager (HCM) from your web app, which is the preferred method.  -->

#### <a name="additional"></a>Aanvullend
* Meerdere hybride verbindingen kunnen worden gemaakt. Zie de [BizTalk Services: grafiek van edities](biztalk-editions-feature-chart.md) voor het aantal toegestane verbindingen. 
* Elke hybride verbinding is gemaakt met een paar verbindingsreeksen: toepassing sleutels die verzenden en On-premises sleutels die LUISTEREN. Elk paar heeft een primaire en secundaire sleutel. 

## <a name="LinkWebSite"></a>Koppel uw Azure App Service-Web-App of mobiele App
Selecteer voor een Web-App of mobiele apps in Azure App Service een koppeling naar een bestaande hybride verbinding, **gebruik een bestaande hybride verbinding** in de blade hybride verbindingen. 
<!-- See [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md). -->

## <a name="InstallHCM"></a>De hybride Verbindingsbeheer on-premises installeren
Nadat een hybride verbinding is gemaakt, moet u de hybride Verbindingsbeheer installeren op de lokale resource. Worden kan gedownload van uw Azure-web-apps of van uw BizTalk Service. 

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]
 
[Azure App Service hybride verbindingen](../app-service/app-service-hybrid-connections.md) is ook een goede resource.

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Aanvullend
* Hybride Verbindingsbeheer kan worden geïnstalleerd op de volgende besturingssystemen:
  
  * Windows Server 2008 R2 (.NET Framework 4.5 + en Windows Management Framework 4.0 + vereist)
  * Windows Server 2012 (Windows Management Framework 4.0 + vereist)
  * Windows Server 2012 R2
* Nadat u de hybride Verbindingsbeheer hebt geïnstalleerd, gebeurt het volgende: 
  
  * De hybride verbinding gehost in Azure wordt automatisch geconfigureerd voor het gebruik van de primaire verbindingsreeks voor de toepassing. 
  * De On-Premises resource wordt automatisch geconfigureerd voor het gebruik van de primaire verbindingsreeks voor On-Premises.
* De hybride Verbindingsbeheer moet een geldige lokale verbindingsreeks gebruiken voor autorisatie. De Azure-Web-Apps of mobiele Apps moet een geldige aanvraag-verbindingsreeks gebruiken voor autorisatie.
* U kunt hybride verbindingen schalen door een ander exemplaar van het hybride Verbindingsbeheer installeren op een andere server. Configureer de listener lokale voor het gebruik van hetzelfde adres als de eerste lokale listener. In dit geval is het verkeer willekeurig gedistribueerde (round robin) tussen de actieve lokale listeners. 

## <a name="ManageHybridConnection"></a>Hybride verbindingen beheren

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)] 

[Azure App Service hybride verbindingen](../app-service/app-service-hybrid-connections.md) is ook een goede resource.

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Kopieer/opnieuw genereren de hybride-verbindingsreeksen

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)] 

[Azure App Service hybride verbindingen](../app-service/app-service-hybrid-connections.md) is ook een goede resource.

#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Gebruik Groepsbeleid om te bepalen van de lokale bronnen die worden gebruikt door een hybride verbinding
1. Download de [hybride Verbindingsbeheer Beheersjablonen](http://www.microsoft.com/download/details.aspx?id=42963).
2. Pak de bestanden.
3. Op de computer die wordt gewijzigd van Groepsbeleid, het volgende doen:  
   
   * Kopieer de. ADMX-bestanden toe aan de *%WINROOT%\PolicyDefinitions* map.
   * Kopieer de. ADML bestanden naar de *%WINROOT%\PolicyDefinitions\en-us* map.

Eenmaal zijn gekopieerd, kunt u Groepsbeleid-Editor om het beleid te wijzigen.

## <a name="next"></a>Volgende
[Overzicht van hybride verbindingen](integration-hybrid-connection-overview.md)

## <a name="see-also"></a>Zie ook
[REST-API voor het beheren van BizTalk Services op Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk Services: grafiek van edities](biztalk-editions-feature-chart.md)  
[Een BizTalk Service maken](biztalk-provision-services.md)  
[BizTalk Services: de tabbladen Dashboard, Bewaken en Schalen](biztalk-dashboard-monitor-scale-tabs.md)

[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
