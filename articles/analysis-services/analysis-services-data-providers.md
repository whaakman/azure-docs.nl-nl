---
title: Voor het verbinden met Azure Analysis Services is nodig clientbibliotheken | Microsoft Docs
description: Beschrijft de clientbibliotheken vereist is voor clienttoepassingen en hulpprogramma's om verbinding maken met Azure Analysis Services
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: a96e7fe671dc051da35168fa7b49ecba53b4c4a5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a>Clientbibliotheken voor het verbinden met Azure Analysis Services

Clientbibliotheken zijn nodig voor client-toepassingen en hulpprogramma's die verbinding maken met Analysis Services-servers. 

Analyseservices gebruikmaken van drie clientbibliotheken. ADOMD.NET en Analysis Services Management Objects (AMO), zijn beheerde clientbibliotheken. De Analysis Services OLE DB-provider (MSOLAP DLL) is een native clientbibliotheek. Alle drie worden doorgaans geïnstalleerd op hetzelfde moment. Azure Analysis Services is vereist voor de meest recente versies. 

Microsoft-clienttoepassingen, zoals Power BI Desktop- en Excel installeren alle drie clientbibliotheken. Echter, afhankelijk van de versie of de frequentie van updates, clientbibliotheken mogelijk niet de meest recente versies die vereist zijn voor Azure Analysis Services. Dit geldt ook voor aangepaste toepassingen of andere interfaces, zoals AsCmd, TOM en ADOMD.NET. Deze toepassingen moet handmatig installeren van de tapewisselaars. De clientbibliotheken voor handmatige installatie zijn opgenomen in SQL Server featurepacks die als pakketten te distribueren. Echter deze clientbibliotheken zijn gekoppeld aan de SQL Server-versie en mogelijk niet de nieuwste versie.  

Clientbibliotheken voor clientverbindingen zijn anders dan gegevensproviders waarmee verbinding wordt gemaakt van een Azure Analysis Services-server met een gegevensbron. Zie voor meer informatie over datasource-verbindingen, [Datasource verbindingen](analysis-services-datasource.md).

## <a name="download-the-latest-client-libraries"></a>Download de meest recente clientbibliotheken  
De volgende clientbibliotheken gebruiken als u in een productieomgeving en volledig vrijgegeven en ondersteunde versies vereist.

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>

## <a name="next-steps"></a>Volgende stappen
[Verbinding maken met Excel](analysis-services-connect-excel.md)    
[Verbinden met Power BI](analysis-services-connect-pbi.md)
