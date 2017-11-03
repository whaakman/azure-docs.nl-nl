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
ms.openlocfilehash: 7b8b9072d0e2fd054ca07873c0a9ce772dc2941e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-hybrid-connections"></a>Hybride verbindingen maken en beheren

> [!IMPORTANT]
> Hybrid Connections van BizTalk is buiten gebruik gesteld en vervangen door App Service Hybrid Connections. Zie [Azure App Service Hybrid Connections](../app-service/app-service-hybrid-connections.md) voor meer informatie, zoals het beheren van uw bestaande BizTalk Hybrid Connections.


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
Een hybride verbinding kan worden gemaakt in de Azure portal met behulp van Web-Apps **of** met behulp van BizTalk Services. 

<!-- **To create Hybrid Connections using Web Apps**, see [Connect Azure Web Apps to an On-Premises Resource](../app-service-web/web-sites-hybrid-connection-get-started.md). You can also install the Hybrid Connection Manager (HCM) from your web app, which is the preferred method.  -->

**Hybride verbindingen maken in BizTalk Services**:

1. Meld u aan bij de [klassieke Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Selecteer in het navigatiedeelvenster links **BizTalk Services** en selecteer vervolgens uw BizTalk Service. 
   
    Als u een bestaande BizTalk Service hebt, kunt u [een BizTalk Service maakt](biztalk-provision-services.md).
3. Selecteer de **hybride verbindingen** tabblad:  
   ![Tabblad hybride verbindingen][HybridConnectionTab]
4. Selecteer **maken van een hybride verbinding** of Selecteer de **toevoegen** knop in de taakbalk. Voer het volgende:
   
   | Eigenschap | Beschrijving |
   | --- | --- |
   | Naam |Naam van de hybride verbinding moet uniek zijn en mag niet dezelfde naam als de BizTalk Service. U kunt elke willekeurige naam invoeren, maar worden specifieke met het doel. Voorbeelden zijn:<br/><br/>Salarissen*SQLServer*<br/>SupplyList*SharepointServer*<br/>Klanten*OracleServer* |
   | Hostnaam |Voer de volledig gekwalificeerde hostnaam alleen de hostnaam of het IPv4-adres van de lokale resource. Voorbeelden zijn:<br/><br/>mySQLServer<br/>*mySQLServer*. *Domein*. corp.*uwbedrijf*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. *uwbedrijf*.com<br/>10.100.10.10<br/><br/>Als u het IPv4-adres gebruikt, houd er rekening mee dat uw code client of een toepassing kan het IP-adres niet omzetten. Zie de opmerking aan het begin van dit onderwerp belangrijk. |
   | Poort |Voer het poortnummer van de lokale resource. Als u Web-Apps gebruikt, voert u bijvoorbeeld poort 80 of poort 443. Als u SQL Server gebruikt, voert u poort 1433. |
5. Selecteer het vinkje om de installatie te voltooien. 

#### <a name="additional"></a>Aanvullend
* Meerdere hybride verbindingen kunnen worden gemaakt. Zie de [BizTalk Services: grafiek van edities](biztalk-editions-feature-chart.md) voor het aantal toegestane verbindingen. 
* Elke hybride verbinding is gemaakt met een paar verbindingsreeksen: toepassing sleutels die verzenden en On-premises sleutels die LUISTEREN. Elk paar heeft een primaire en secundaire sleutel. 

## <a name="LinkWebSite"></a>Koppel uw Azure App Service-Web-App of mobiele App
Selecteer voor een Web-App of mobiele apps in Azure App Service een koppeling naar een bestaande hybride verbinding, **gebruik een bestaande hybride verbinding** in de blade hybride verbindingen. 
<!-- See [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md). -->

## <a name="InstallHCM"></a>De hybride Verbindingsbeheer on-premises installeren
Nadat een hybride verbinding is gemaakt, moet u de hybride Verbindingsbeheer installeren op de lokale resource. Worden kan gedownload van uw Azure-web-apps of van uw BizTalk Service. Stappen voor BizTalk Services: 

1. Meld u aan bij de [klassieke Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Selecteer in het navigatiedeelvenster links **BizTalk Services** en selecteer vervolgens uw BizTalk Service. 
3. Selecteer de **hybride verbindingen** tabblad:  
   ![Tabblad hybride verbindingen][HybridConnectionTab]
4. Selecteer in de taakbalk **On-Premises Setup**:  
   ![On-Premises Setup][HCOnPremSetup]
5. Selecteer **installeren en configureren van** uitvoeren of de hybride Verbindingsbeheer op de on-premises systeem te downloaden. 
6. Selecteer het vinkje om de installatie te starten. 

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
Voor het beheren van uw hybride verbindingen, kunt u het volgende doen:

* Gebruik de Azure-portal en gaat u naar uw BizTalk Service. 
* Gebruik [REST-API's](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Kopieer/opnieuw genereren de hybride-verbindingsreeksen
1. Meld u aan bij de [klassieke Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Selecteer in het navigatiedeelvenster links **BizTalk Services** en selecteer vervolgens uw BizTalk Service. 
3. Selecteer de **hybride verbindingen** tabblad:  
   ![Tabblad hybride verbindingen][HybridConnectionTab]
4. Selecteer de hybride verbinding. Selecteer in de taakbalk **beheren verbinding**:  
   ![Opties beheren][HCManageConnection]
   
    **Verbinding beheren** geeft een lijst van de toepassing en On-Premises-verbindingsreeksen. U kunt de verbindingsreeksen kopiëren of opnieuw genereren van de toegangssleutel die is gebruikt in de verbindingstekenreeks. 
   
    **Als u opnieuw genereren selecteert**, de toegang tot gedeelde sleutel gebruikt in de verbindingsreeks wordt gewijzigd. Ga als volgt te werk:
   
   * Selecteer in de klassieke Azure portal **sleutels synchroniseren** in de Azure-toepassing.
   * Voer de **On-Premises Setup**. Wanneer u de On-Premises Setup opnieuw uitvoert, wordt de lokale resource automatisch geconfigureerd voor gebruik van de bijgewerkte primaire verbindingsreeks.

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
[Een BizTalk Service met behulp van de klassieke Azure portal maken](biztalk-provision-services.md)  
[BizTalk Services: de tabbladen Dashboard, Bewaken en Schalen](biztalk-dashboard-monitor-scale-tabs.md)

[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
