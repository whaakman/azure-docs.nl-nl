---
title: Vereist om verbinding te maken met Azure Analysis Services-clientbibliotheken | Microsoft Docs
description: Hierin wordt beschreven vereist is voor clienttoepassingen en hulpprogramma's om verbinding maken met Azure Analysis Services-clientbibliotheken
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c67b9185f38137113b34c8f3a74ae199e50091f1
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55296321"
---
# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a>Clientbibliotheken voor het verbinden met Azure Analysis Services

-Clientbibliotheken zijn nodig om de clienttoepassingen en hulpprogramma's verbinding maken met Analysis Services-servers. 

## <a name="download-the-latest-client-libraries-windows-installer"></a>De meest recente clientbibliotheken (Windows-installatieprogramma) downloaden  

|Downloaden  |Productversie  | 
|---------|---------|
|[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)    |    15.0.2.241      |
|[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)     |    15.0.2.241      |
|[AMO](https://go.microsoft.com/fwlink/?linkid=829578)     |   15.12.1.0    |
|[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)     |    15.12.1.0     |

## <a name="amo-and-adomd-nuget-packages"></a>AMO en ADOMD (NuGet-pakketten)

Analysis Services Management Objects (AMO) en ADOMD-clientbibliotheken zijn beschikbaar als installeerbare-pakketten uit [NuGet.org](https://www.nuget.org/). Het raadzaam dat u migreert naar NuGet-verwijzingen in plaats van met behulp van Windows Installer. 

|Pakket  | Productversie  | 
|---------|---------|
|[AMO](https://www.nuget.org/packages/Microsoft.AnalysisServices.retail.amd64/)    |    15.12.1.0    |
|[ADOMD](https://www.nuget.org/packages/Microsoft.AnalysisServices.AdomdClient.retail.amd64/)     |   15.12.1.0     |

NuGet-pakket assembly's AssemblyVersion Volg semantisch versiebeheer: BELANGRIJKSTE. SECUNDAIRE. PATCH. NuGet-verwijzingen laden van de verwachte versie, zelfs als er een andere versie in de GAC (die voortvloeien uit MSI-installatie). PATCH wordt voor elke versie verhoogd. AMO en ADOMD-versies bewaard gesynchroniseerd.

## <a name="understanding-client-libraries"></a>Understanding-clientbibliotheken

Drie clientbibliotheken, ook wel bekend als gegevensproviders maken gebruik van analyseservices. ADOMD.NET en Analysis Services Management Objects (AMO) zijn beheerde-clientbibliotheken. De Analysis Services OLE DB-Provider (MSOLAP DLL) is een systeemeigen client-bibliotheek. Normaal gesproken worden alle drie geïnstalleerd op hetzelfde moment. **Azure Analysis Services is vereist voor de nieuwste versies van alle drie bibliotheken**. 

Microsoft-clienttoepassingen zoals Power BI Desktop en Excel alle drie deze clientbibliotheken geïnstalleerd en deze bijwerken wanneer er nieuwe versies beschikbaar zijn. Afhankelijk van de versie of de frequentie van updates, sommige clientbibliotheken mogelijk niet de meest recente versies die door Azure Analysis Services vereist. Dit geldt ook voor aangepaste toepassingen of andere interfaces, zoals AsCmd, TOM en ADOMD.NET. Deze toepassingen moeten handmatig of programmatisch de bibliotheken installeren. De clientbibliotheken voor handmatige installatie zijn opgenomen in SQL Server featurepacks die als pakketten te distribueren. Echter deze clientbibliotheken zijn gekoppeld aan de SQL Server-versie en zijn mogelijk niet de nieuwste versie.  

Clientbibliotheken voor clientverbindingen verschillen van de gegevensproviders is vereist voor het verbinding maken met een gegevensbron vanuit een Azure Analysis Services-server. Zie voor meer informatie over gegevensbronverbindingen [gegevensbronverbindingen](analysis-services-datasource.md).

## <a name="client-library-types"></a>Bibliotheek clienttypen

### <a name="analysis-services-ole-db-provider-msolap"></a>Analysis Services OLE DB-Provider (MSOLAP) 

 Analysis Services OLE DB Provider (MSOLAP) is de systeemeigen client-bibliotheek voor verbindingen van Analysis Services-database. Er wordt indirect gebruikt door zowel ADOMD.NET en AMO, verbindingsaanvragen overdragen naar de gegevensprovider. U kunt ook de OLE DB-Provider aanroepen rechtstreeks vanuit de toepassingscode.  
  
 De OLE DB-Provider voor Analysis Services wordt automatisch geïnstalleerd door de meeste hulpprogramma's en clienttoepassingen gebruikt voor toegang tot Analysis Services-databases. Deze moet worden geïnstalleerd op computers die worden gebruikt voor toegang tot gegevens van Analysis Services.  
  
 OLE DB-providers worden vaak in tekenreeksen voor databaseverbindingen opgegeven. De verbindingsreeks van een Analysis Services maakt gebruik van een andere naamgeving om te verwijzen naar de OLE DB-Provider: MSOLAP. \<versie > dll-bestand.

### <a name="amo"></a>AMO  

 AMO is een beheerde client-bibliotheek gebruikt voor het beheer van server en de gegevensdefinitie van de. Deze is geïnstalleerd en gebruikt door de hulpprogramma's en client-toepassingen. SQL Server Management Studio (SSMS) gebruikt bijvoorbeeld AMO verbinding maken met Analysis Services. Een verbinding maken met AMO is doorgaans minimaal, die bestaan uit `"data source=\<servername>"`. Nadat een verbinding tot stand is gebracht, kunt u de API gebruiken om te werken met verzamelingen van de database en belangrijke objecten daarvan. SSDT zowel SSMS gebruiken AMO verbinding maken met een Analysis Services-exemplaar.  

  
### <a name="adomd"></a>ADOMD

 ADOMD.NET is een beheerde gegevens clientbibliotheek gebruikt voor het uitvoeren van query's Analysis Services-gegevens. Deze is geïnstalleerd en gebruikt door de hulpprogramma's en client-toepassingen. 
  
 Wanneer u verbinding maakt met een database, is de verbindingseigenschappen van de tekenreeks voor alle drie bibliotheken zijn vergelijkbaar. Vrijwel elk verbindingsreeks die u voor ADOMD.NET met behulp van definieert [Microsoft.AnalysisServices.AdomdClient.AdomdConnection.ConnectionString](https://msdn.microsoft.com/library/microsoft.analysisservices.adomdclient.adomdconnection.connectionstring.aspx) werkt ook voor AMO en de Analysis Services OLE DB-Provider (MSOLAP). Zie voor meer informatie, [eigenschappen van een verbindingsreeks &#40;Analysis Services&#41;](https://docs.microsoft.com/sql/analysis-services/instances/connection-string-properties-analysis-services).  

  
##  <a name="bkmk_LibUpdate"></a> Het bepalen van de versie van de client-bibliotheek   
  
### <a name="oleddb-msolap"></a>OLEDDB (MSOLAP)  
  
1.  Ga naar `C:\Program Files\Microsoft Analysis Services\AS OLEDB\`. Als u meer dan één map hebt, kiest u het hogere getal.
  
2.  Met de rechtermuisknop op **msolap.dll** > **eigenschappen** > **Details**. Als de bestandsnaam msolap140.dll is, ouder is dan de meest recente versie en moet worden bijgewerkt.
    
    ![Details van de client-bibliotheek](media/analysis-services-data-providers/aas-msolap-details.png)
    
  
### <a name="amo"></a>AMO

1. Ga naar `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices\`. Als u meer dan één map hebt, kiest u het hogere getal.
2. Met de rechtermuisknop op **Microsoft.AnalysisServices** > **eigenschappen** > **Details**.  

### <a name="adomd"></a>ADOMD

1. Ga naar `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices.AdomdClient\`. Als u meer dan één map hebt, kiest u het hogere getal.
2. Met de rechtermuisknop op **Microsoft.AnalysisServices.AdomdClient** > **eigenschappen** > **Details**.  


## <a name="next-steps"></a>Volgende stappen
[Verbinding maken met Excel](analysis-services-connect-excel.md)    
[Verbinden met Power BI](analysis-services-connect-pbi.md)
