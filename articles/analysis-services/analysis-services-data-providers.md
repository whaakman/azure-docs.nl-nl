---
title: Voor het verbinden met Azure Analysis Services is nodig clientbibliotheken | Microsoft Docs
description: Beschrijft de clientbibliotheken vereist is voor clienttoepassingen en hulpprogramma's om verbinding maken met Azure Analysis Services
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 12/14/2017
ms.author: owend
ms.openlocfilehash: 870d430d1926859894f452e0af812d794272a9e6
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a>Clientbibliotheken voor het verbinden met Azure Analysis Services

Clientbibliotheken zijn nodig voor client-toepassingen en hulpprogramma's die verbinding maken met Analysis Services-servers. 

## <a name="download-the-latest-client-libraries"></a>Download de meest recente clientbibliotheken  

|Downloaden  |Versie  | 
|---------|---------|
|[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)    |    14.0.801.241      |
|[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)     |    14.0.801.241      |
|[AMO](https://go.microsoft.com/fwlink/?linkid=829578)     |   14.0.800.117      |
|[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)     |    14.0.801.241      |

## <a name="understanding-client-libraries"></a>Understanding clientbibliotheken

Drie clientbibliotheken, ook wel bekend als gegevensproviders gebruikmaken van analyseservices. ADOMD.NET en Analysis Services Management Objects (AMO) zijn beheerde clientbibliotheken. De Analysis Services OLE DB-Provider (MSOLAP DLL) is een native clientbibliotheek. Alle drie worden doorgaans geïnstalleerd op hetzelfde moment. **Azure Analysis Services is vereist voor de nieuwste versies van alle drie bibliotheken**. 

Microsoft-clienttoepassingen zoals Power BI Desktop- en Excel alle drie clientbibliotheken installeren en deze relaties bijwerken wanneer er nieuwe versies beschikbaar zijn. Sommige clientbibliotheken mogelijk, afhankelijk van de versie of de frequentie van updates, niet de meest recente versies die vereist zijn voor Azure Analysis Services. Dit geldt ook voor aangepaste toepassingen of andere interfaces, zoals AsCmd, TOM en ADOMD.NET. Deze toepassingen vereisen handmatig of programmatisch installeren van de tapewisselaars. De clientbibliotheken voor handmatige installatie zijn opgenomen in SQL Server featurepacks die als pakketten te distribueren. Echter deze clientbibliotheken zijn gekoppeld aan de SQL Server-versie en mogelijk niet de nieuwste versie.  

Clientbibliotheken voor clientverbindingen zijn anders dan gegevensproviders waarmee verbinding wordt gemaakt van een Azure Analysis Services-server met een gegevensbron. Zie voor meer informatie over datasource-verbindingen, [Datasource verbindingen](analysis-services-datasource.md).

## <a name="client-library-types"></a>Bibliotheek clienttypen

### <a name="analysis-services-ole-db-provider-msolap"></a>Analysis Services OLE DB-Provider (MSOLAP) 

 Analysis Services OLE DB Provider (MSOLAP) is de native-clientbibliotheek voor verbindingen van Analysis Services-database. Er wordt indirect gebruikt door zowel ADOMD.NET en AMO verbindingsaanvragen delegeren aan de gegevensprovider. U kunt ook de OLE DB-Provider rechtstreeks vanuit de toepassingscode aanroepen.  
  
 De OLE DB-Provider voor Analysis Services wordt automatisch geïnstalleerd door de meeste hulpprogramma's en clienttoepassingen die worden gebruikt voor toegang tot Analysis Services-databases. Het moet worden geïnstalleerd op computers die worden gebruikt voor toegang tot Analysis Services-gegevens.  
  
 OLE DB-providers zijn vaak opgegeven in de verbindingsreeksen. Een verbindingsreeks Analysis Services maakt gebruik van een andere naamgeving volgt de namen om te verwijzen naar de OLE DB-Provider: MSOLAP. \<versie > dll-bestand.

### <a name="amo"></a>AMO  

 AMO is een beheerde client-bibliotheek voor Serverbeheer en de gegevensdefinitie van de worden gebruikt. Het is geïnstalleerd en gebruikt door de hulpprogramma's en clienttoepassingen. SQL Server Management Studio (SSMS) wordt bijvoorbeeld AMO gebruikt om verbinding met Analysis Services.  
  
 Een verbinding met AMO is gewoonlijk minimaal, die bestaan uit `“data source=\<servername>”`. Nadat een verbinding tot stand is gebracht, kunt u de API gebruiken om te werken met verzamelingen van de database en hoofdobjecten. Zowel SSDT SSMS gebruik AMO verbinding maken met een Analysis Services-exemplaar.  

  
### <a name="adomd"></a>ADOMD

 ADOMD.NET is een beheerde gegevens clientbibliotheek gebruikt voor het opvragen van gegevens van de Analysis Services. Het is geïnstalleerd en gebruikt door de hulpprogramma's en clienttoepassingen. 
  
 Wanneer u verbinding maakt met een database, worden de eigenschappen van een verbindingsreeks voor alle drie bibliotheken lijken. Bijna alle verbindingsreeks die u definieert voor ADOMD.NET door [Microsoft.AnalysisServices.AdomdClient.AdomdConnection.ConnectionString](https://msdn.microsoft.com/library/microsoft.analysisservices.adomdclient.adomdconnection.connectionstring.aspx) werkt ook voor AMO en de Analysis Services OLE DB-Provider (MSOLAP). Zie voor meer informatie, [eigenschappen van een verbindingsreeks &#40; Analyseservices &#41; ](https://docs.microsoft.com/sql/analysis-services/instances/connection-string-properties-analysis-services).  

  
##  <a name="bkmk_LibUpdate"></a>Het bepalen van de clientbibliotheek van de versie   
  
### <a name="oleddb-msolap"></a>OLEDDB (MSOLAP)  
  
1.  Ga naar `C:\Program Files\Microsoft Analysis Services\AS OLEDB\140`. Als u meerdere mappen hebt, kiest u een hoger nummer.
  
2.  Met de rechtermuisknop op **msolap140.dll** > **eigenschappen** > **Details**.  
    
    ![Details van tapewisselaar client](media/analysis-services-data-providers/aas-msolap-details.png)
  
### <a name="amo"></a>AMO

1. Ga naar `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices\v4.0_14.0.0.0__89845dcd8080cc91`.
2. Met de rechtermuisknop op **Microsoft.AnalysisServices** > **eigenschappen** > **Details**.  

### <a name="adomd"></a>ADOMD

1. Ga naar `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices.AdomdClient\v4.0_14.0.0.0__89845dcd8080cc91`.
2. Met de rechtermuisknop op **Microsoft.AnalysisServices.AdomdClient** > **eigenschappen** > **Details**.  


## <a name="next-steps"></a>Volgende stappen
[Verbinding maken met Excel](analysis-services-connect-excel.md)    
[Verbinden met Power BI](analysis-services-connect-pbi.md)
