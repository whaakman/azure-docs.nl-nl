---
title: Azure Analysis Services server aliasnamen | Microsoft Docs
description: Beschrijft hoe u het maken en gebruiken van server-aliassen voor de naam.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: d0aebbe115be9e9af697b5d93d158a263fefe55c
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448029"
---
# <a name="alias-server-names"></a>De namen van de alias

Met behulp van de alias voor een servernaam gebruikers verbinding kunnen maken met uw Azure Analysis Services-server met een kortere *alias* in plaats van de naam van de server. Wanneer u verbinding maakt vanaf een clienttoepassing, de alias is opgegeven als een eindpunt met de **koppeling: / /** protocol-indeling. Het eindpunt retourneert vervolgens de werkelijke servernaam om verbinding te kunnen.

De namen van de alias zijn geschikt voor:

- Modellen migreren tussen servers zonder gevolgen voor gebruikers. 
- Beschrijvende namen zijn gemakkelijker voor gebruikers om te onthouden. 
- Directe gebruikers naar andere servers op verschillende tijdstippen van de dag. 
- Directe gebruikers in verschillende regio's met exemplaren die geografisch dichter bij, zoals wanneer u Azure Traffic Manager. 

Een HTTPS-eindpunt dat een geldige naam voor de Azure Analysis Services-server retourneert kan fungeren als een alias. Het eindpunt moet ondersteunen van HTTPS via poort 443 en de poort niet in de URI moet worden opgegeven.

![Indeling voor de koppeling met alias](media/analysis-services-alias/aas-alias-browser.png)

Wanneer u verbinding maakt vanaf een client, de naam van de alias wordt ingevoerd met behulp van **koppeling: / /** protocol-indeling. Bijvoorbeeld, in Power BI Desktop:

![Power BI Desktop verbinding](media/analysis-services-alias/aas-alias-connect-pbid.png)

## <a name="create-an-alias"></a>Alias maken

Voor het maken van een alias-eindpunt, kunt u een methode die een geldige naam voor de Azure Analysis Services-server retourneert. Bijvoorbeeld, een verwijzing naar een bestand in Azure Blob Storage is de echte-server met een naam geven, of maken en publiceren van een ASP.NET Web Forms-toepassing.

In dit voorbeeld wordt een ASP.NET Web Forms-toepassing in Visual Studio gemaakt. De basispagina referentie en de gebruiker zijn verwijderd uit de pagina Default.aspx. De inhoud van Default.aspx zijn alleen de volgende pagina-instructie:

```
<%@ Page Title="Home Page" Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="FriendlyRedirect._Default" %>
```

De Page_Load-gebeurtenis in Default.aspx.cs maakt gebruik van de methode Response.Write() om terug te keren van de naam van de Azure Analysis Services-server.

```
protected void Page_Load(object sender, EventArgs e)
{
    this.Response.Write("asazure://<region>.asazure.windows.net/<servername>");
}
```

## <a name="see-also"></a>Zie ook

[Clientbibliotheken](analysis-services-data-providers.md)   
[Verbinding maken via Power BI Desktop](analysis-services-connect-pbi.md)
