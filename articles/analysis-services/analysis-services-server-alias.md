---
title: Azure Analysis Services server aliasnamen | Microsoft Docs
description: Beschrijft hoe maken en gebruiken van server-naam-aliassen.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: a947dde1551c653faa54f088c1712c41c7657aa0
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="alias-server-names"></a>Servernamen alias

Met behulp van een server name alias, kunnen gebruikers verbinding maken met uw Azure Analysis Services-server met een kortere *alias* in plaats van de naam van de server. Bij het verbinden van een clienttoepassing, de alias die is opgegeven als een eindpunt met behulp van de **koppeling: / /** protocol-indeling. Het eindpunt retourneert vervolgens de werkelijke servernaam om verbinding te kunnen.

Alias servernamen zijn geschikt voor:

- Modellen migreren tussen servers zonder gevolgen voor gebruikers. 
- Beschrijvende namen zijn gemakkelijker voor gebruikers te onthouden. 
- Gebruikers direct naar andere servers op verschillende tijdstippen van de dag. 
- Directe gebruikers in verschillende regio's met exemplaren die geografisch dichter bij, zoals het bij het gebruik van Azure Traffic Manager. 

Een HTTP-eindpunt dat een geldige naam voor de Azure Analysis Services-server retourneert kan fungeren als een alias.

![Indeling voor de koppeling met alias](media/analysis-services-alias/aas-alias-browser.png)

Wanneer u verbinding maakt vanaf een client, de naam van de alias wordt ingevoerd met **koppeling: / /** protocol-indeling. Bijvoorbeeld in Power BI Desktop:

![Power BI Desktop-verbinding](media/analysis-services-alias/aas-alias-connect-pbid.png)

## <a name="create-an-alias"></a>Een alias maken

Een alias om eindpunt te maken, kunt u een TriggerSet-methode die een geldige naam voor de Azure Analysis Services-server retourneert. Bijvoorbeeld: een verwijzing naar een bestand in Azure Blob Storage waarin de werkelijke server name, of maken en publiceren van een webformulieren van ASP.NET-toepassing.

In dit voorbeeld wordt een ASP.NET-webtoepassing formulieren gemaakt in Visual Studio. De basispagina reference en beheer worden verwijderd van de pagina Default.aspx. De inhoud van Default.aspx zijn gewoon de volgende pagina-instructie:

```
<%@ Page Title="Home Page" Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="FriendlyRedirect._Default" %>
```

De gebeurtenis Page_Load in Default.aspx.cs gebruikt de methode Response.Write() om de naam van de Azure Analysis Services-server.

```
protected void Page_Load(object sender, EventArgs e)
{
    this.Response.Write("asazure://<region>.asazure.windows.net/<servername>");
}
```

## <a name="see-also"></a>Zie ook

[Clientbibliotheken](analysis-services-data-providers.md)   
[Verbinding maken vanuit Power BI Desktop](analysis-services-connect-pbi.md)
