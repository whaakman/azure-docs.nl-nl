---
title: Voor het verbinden met Azure Analysis Services is nodig clientbibliotheken | Microsoft Docs
description: Beschrijft de clientbibliotheken vereist is voor clienttoepassingen en hulpprogramma's om verbinding maken met Azure Analysis Services
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/31/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: ad330c5f17b6e151918511916b0aef89bef3a6f7
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699849"
---
# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a>Clientbibliotheken voor het verbinden met Azure Analysis Services

Clientbibliotheken zijn nodig voor client-toepassingen en hulpprogramma's die verbinding maken met Analysis Services-servers. 

## <a name="download-the-latest-client-libraries-windows-installer"></a>Download de meest recente clientbibliotheken (Windows Installer)  

|Downloaden  |Productversie  | 
|---------|---------|
|[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)    |    15.0.1.492      |
|[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)     |    15.0.1.492      |
|[AMO](https://go.microsoft.com/fwlink/?linkid=829578)     |   15.1.0.0    |
|[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)     |    15.1.0.0     |

## <a name="amo-and-adomd-nuget-packages"></a>AMO en ADOMD (NuGet-pakketten)

Analysis Services Management Objects (AMO) en ADOMD clientbibliotheken zijn beschikbaar als een installeerbare pakketten uit [NuGet.org](https://www.nuget.org/). Het raadzaam dat u migreert naar NuGet-verwijzingen in plaats van Windows Installer. 

|Pakket  | Productversie  | 
|---------|---------|
|[AMO](https://www.nuget.org/packages/Microsoft.AnalysisServices.retail.amd64/)    |    15.1.0.0      |
|[ADOMD](https://www.nuget.org/packages/Microsoft.AnalysisServices.AdomdClient.retail.amd64/)     |   15.1.0.0      |

NuGet-pakket assembly's AssemblyVersion Volg semantische versiebeheer: belangrijke. SECUNDAIRE. PATCH. NuGet-verwijzingen laden de verwachte versie, zelfs als er een andere versie in de GAC (die voortvloeien uit MSI-installatie). PATCH wordt voor elke release verhoogd. AMO en ADOMD versies bewaard in de synchronisatie.

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

 AMO is een beheerde client-bibliotheek voor Serverbeheer en de gegevensdefinitie van de worden gebruikt. Het is geïnstalleerd en gebruikt door de hulpprogramma's en clienttoepassingen. SQL Server Management Studio (SSMS) wordt bijvoorbeeld AMO gebruikt om verbinding met Analysis Services. Een verbinding met AMO is gewoonlijk minimaal, die bestaan uit `“data source=\<servername>”`. Nadat een verbinding tot stand is gebracht, kunt u de API gebruiken om te werken met verzamelingen van de database en hoofdobjecten. Zowel SSDT SSMS gebruik AMO verbinding maken met een Analysis Services-exemplaar.  

  
### <a name="adomd"></a>ADOMD

 ADOMD.NET is een beheerde gegevens clientbibliotheek gebruikt voor het opvragen van gegevens van de Analysis Services. Het is geïnstalleerd en gebruikt door de hulpprogramma's en clienttoepassingen. 
  
 Wanneer u verbinding maakt met een database, worden de eigenschappen van een verbindingsreeks voor alle drie bibliotheken lijken. Bijna alle verbindingsreeks die u definieert voor ADOMD.NET door [Microsoft.AnalysisServices.AdomdClient.AdomdConnection.ConnectionString](https://msdn.microsoft.com/library/microsoft.analysisservices.adomdclient.adomdconnection.connectionstring.aspx) werkt ook voor AMO en de Analysis Services OLE DB-Provider (MSOLAP). Zie voor meer informatie, [eigenschappen van een verbindingsreeks &#40;Analysis Services&#41;](https://docs.microsoft.com/sql/analysis-services/instances/connection-string-properties-analysis-services).  

  
##  <a name="bkmk_LibUpdate"></a> Het bepalen van de clientbibliotheek van de versie   
  
### <a name="oleddb-msolap"></a>OLEDDB (MSOLAP)  
  
1.  Ga naar `C:\Program Files\Microsoft Analysis Services\AS OLEDB\`. Als u meerdere mappen hebt, kiest u een hoger nummer.
  
2.  Met de rechtermuisknop op **msolap.dll** > **eigenschappen** > **Details**. Als de bestandsnaam msolap140.dll is, ouder is dan de nieuwste versie en moet worden bijgewerkt.
    
    ![Details van tapewisselaar client](media/analysis-services-data-providers/aas-msolap-details.png)
    
  
### <a name="amo"></a>AMO

1. Ga naar `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices\`. Als u meerdere mappen hebt, kiest u een hoger nummer.
2. Met de rechtermuisknop op **Microsoft.AnalysisServices** > **eigenschappen** > **Details**.  

### <a name="adomd"></a>ADOMD

1. Ga naar `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices.AdomdClient\`. Als u meerdere mappen hebt, kiest u een hoger nummer.
2. Met de rechtermuisknop op **Microsoft.AnalysisServices.AdomdClient** > **eigenschappen** > **Details**.  


## <a name="next-steps"></a>Volgende stappen
[Verbinding maken met Excel](analysis-services-connect-excel.md)    
[Verbinden met Power BI](analysis-services-connect-pbi.md)
