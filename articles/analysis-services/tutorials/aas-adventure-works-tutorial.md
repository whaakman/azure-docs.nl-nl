---
title: Azure Analysis Services - Adventure Works-zelfstudie | Microsoft Docs
description: Hier vindt u algemene informatie over de zelfstudie Adventure Works voor Azure Analysis Services
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 06/01/2017
ms.author: owend
ms.openlocfilehash: 81a378c6b1a15cc240ca30fa19b557312ca54922
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-analysis-services---adventure-works-tutorial"></a>Azure Analysis Services - Adventure Works-zelfstudie

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Deze zelfstudie bestaat uit een aantal lessen waarin u leert hoe u met behulp van [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) een tabellair model maakt en implementeert op het compatibiliteitsniveau 1400.  

Als u geen ervaring hebt met Analysis Services en tabellaire modellen, is het volgen van deze zelfstudie de snelste manier om te leren hoe u een eenvoudig tabellair model maakt en implementeert. Zodra aan alle vereisten is voldaan, zal het afronden van de zelfstudie twee tot drie uur duren.  
  
## <a name="what-you-learn"></a>Wat u leert   
  
-   Een nieuw project voor een tabellair model maken op het **compatibiliteitsniveau 1400** in SSDT.
  
-   Gegevens uit een relationele database importeren in een project voor een tabellair model.  
  
-   Relaties tussen tabellen in het model maken en beheren.  
  
-   Berekende kolommen, metingen en Key Performance Indicators maken waarmee gebruikers essentiële zakelijke metrische gegevens kunnen analyseren.  
  
-   Perspectieven en hiërarchieën maken en beheren waarmee gebruikers eenvoudiger door modelgegevens kunnen bladeren aan de hand van bedrijfs- en toepassingsspecifieke standpunten.  
  
-   Partities maken om tabelgegevens onder te verdelen in kleinere, logische delen die onafhankelijk van andere partities kunnen worden verwerkt.  
  
-   Modelobjecten en gegevens beveiligen door rollen te maken met gebruikersleden.  
  
-   Een tabellair model implementeren op een **Azure Analysis Services**-server of een on-premises SQL Server 2017 Analysis Services-server.  
  
## <a name="prerequisites"></a>Vereisten  
Voor deze zelfstudie hebt u het volgende nodig:  
  
-   Een exemplaar van Azure Analysis Services of SQL Server 2017 Analysis Services waarnaar het model moet worden geïmplementeerd. Meld u aan voor een gratis [proefversie van Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) en [maak een server](../analysis-services-create-server.md). U kunt zich ook registreren voor [SQL Server 2017 Community Technology Preview](https://www.microsoft.com/evalcenter/evaluate-sql-server-vnext-ctp) en dit product downloaden. 

-   Een SQL Server- of Azure SQL-datawarehouse met de [voorbeelddatabase AdventureWorksDW2014](http://go.microsoft.com/fwlink/?LinkID=335807). Deze voorbeelddatabase bevat de gegevens die nodig zijn om deze zelfstudie te voltooien. Download [gratis edities van SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads). U kunt zich ook aanmelden voor een gratis [proefversie van Azure SQL Database](https://azure.microsoft.com/services/sql-database/). 

    **Belangrijk:** Als u de voorbeelddatabase installeert op een on-premises SQL Server en u uw model implementeert naar een Azure Analysis Services-server, is een [on-premises gegevensgateway](../analysis-services-gateway.md) vereist.

-   De nieuwste versie van [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).

-   De nieuwste versie van [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).    

-   Een clienttoepassing zoals [Power BI Desktop](https://powerbi.microsoft.com/desktop/) of Excel. 

## <a name="scenario"></a>Scenario  
Deze zelfstudie is gebaseerd op Adventure Works Cycles, een fictief bedrijf. Adventure Works is een bedrijf met vestigingen in meerdere landen dat fietsen, onderdelen en accessoires bouwt en distribueert voor Noord-Amerika, Europa en Azië. Het bedrijf heeft 500 werknemers. Bovendien maakt Adventure Works gebruik van verschillende regionale verkoopteams op de verschillende markten. Uw project bestaat uit het maken van een tabellair model dat verkoop- en marketingpersoneel kan gebruiken om gegevens van internetverkopen te analyseren in de AdventureWorksDW-database.  
  
U moet verschillende lessen afronden om de zelfstudie te voltooien. In elke les moet u enkele taken uitvoeren. Om de les te voltooien, moeten de taken in volgorde worden uitgevoerd. Het is mogelijk dat een bepaalde les verschillende taken bevat die een vergelijkbare uitkomst opleveren, maar waarvoor dan net iets andere stappen worden gebruikt. Hiermee wordt aangetoond dat er vaak meer dan één manier is om een taak te voltooien en wordt u uitgedaagd om de vaardigheden toe te passen die u in eerdere lessen en taken hebt geleerd.  
  
Het doel van de lessen is om u met behulp van een aantal functies in SSDT te helpen bij het ontwerpen van een eenvoudig tabellair model. Omdat elke les voortbouwt op de vorige les, moet u de lessen op volgorde uitvoeren.
  
Deze zelfstudie bevat geen lessen over het beheren van een server in Azure Portal, het beheren van een server of database met behulp van SSMS of het bladeren door modelgegevens met een clienttoepassing. 


## <a name="lessons"></a>Lessen  
Deze zelfstudie bevat de volgende lessen:  
  
|Les|Geschatte tijdsduur|  
|----------|------------------------------|  
|[1 - Een nieuw project voor een tabellair model maken](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md)|10 minuten|  
|[2 - Gegevens ophalen](../tutorials/aas-lesson-2-get-data.md)|10 minuten|  
|[3: Als gegevenstabel markeren](../tutorials/aas-lesson-3-mark-as-date-table.md)|3 minuten|  
|[4: Relaties maken](../tutorials/aas-lesson-4-create-relationships.md)|10 minuten|  
|[5 - Berekende kolommen maken](../tutorials/aas-lesson-5-create-calculated-columns.md)|15 minuten|
|[6 - Metingen maken](../tutorials/aas-lesson-6-create-measures.md)|30 minuten|  
|[7 - Key Performance Indicators (KPI) maken](../tutorials/aas-lesson-7-create-key-performance-indicators.md)|15 minuten|  
|[8 - Perspectieven maken](../tutorials/aas-lesson-8-create-perspectives.md)|5 minuten|  
|[9 - Hiërarchieën maken](../tutorials/aas-lesson-9-create-hierarchies.md)|20 minuten|  
|[10 - Partities maken](../tutorials/aas-lesson-10-create-partitions.md)|15 minuten|  
|[11 - Rollen maken](../tutorials/aas-lesson-11-create-roles.md)|15 minuten|  
|[12: Analyseren in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md)|5 minuten| 
|[13 - Implementeren](../tutorials/aas-lesson-13-deploy.md)|5 minuten|  
  
## <a name="supplemental-lessons"></a>Aanvullende lessen  
Deze lessen zijn niet vereist om de zelfstudie te voltooien, maar kunnen bijdragen aan een beter begrip van de geavanceerde functies voor het ontwerpen van een tabellair model.  
  
|Les|Geschatte tijdsduur|  
|----------|------------------------------|  
|[Detailrijen](../tutorials/aas-supplemental-lesson-detail-rows.md)|10 minuten|
|[Dynamische beveiliging](../tutorials/aas-supplemental-lesson-dynamic-security.md)|30 minuten|
|[Onregelmatige hiërarchieën](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)|20 minuten| 

  
## <a name="next-steps"></a>Volgende stappen  
Ga naar [Les 1: Een nieuw project voor een tabellair model maken](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md) om te beginnen met de zelfstudie.  
  
  
  

