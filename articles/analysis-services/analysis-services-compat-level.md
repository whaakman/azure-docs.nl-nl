---
title: Gegevensmodel compatibiliteitsniveau in Azure Analysis Services | Microsoft Docs
description: Understanding tabelgegevens model compatibiliteitsniveau.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: d68d544a66448fbbf193ff53fa43e179b1edb706
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34602064"
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a>Het compatibiliteitsniveau voor Analysis Services-modellen in tabelvorm

*Het compatibiliteitsniveau* verwijst naar de release-specifieke gedrag in de Analysis Services-engine. Wijzigingen in het compatibiliteitsniveau wordt doorgaans samenvallen met belangrijke releases van SQL Server. Deze wijzigingen zijn ook geïmplementeerd in Azure Analysis Services onderhouden pariteit tussen beide platforms. Het compatibiliteitsniveau wijzigt ook effect die beschikbaar zijn in uw modellen in tabelvorm. Bijvoorbeeld, hebben DirectQuery en metagegevens in tabelvorm object verschillende implementaties, afhankelijk van het compatibiliteitsniveau. Het compatibiliteitsniveau is opgegeven in het model in tabelvorm-project in Visual Studio (SSDT). Alleen het 1400 compatibiliteitsniveau zijn modellen in tabelvorm gemaakt in en uit Power BI Desktop geïmporteerd.

Azure Analysis Services biedt ondersteuning voor tabelmodellen op het niveau van de compatibiliteit 1200 en 1400. 

Het compatibiliteitsniveau van de meest recente is 1400. Dit niveau samenvalt met SQL Server 2017 Analysis Services. Belangrijke functies in het compatibiliteitsniveau 1400:

*  Nieuwe functies voor toegang tot gegevens en importeren met ondersteuning voor TOM APIs en TMSL scripting. 
*  Gegevenstransformatie en gegevens mashup mogelijkheden met behulp van expressies gegevens ophalen en M.
*  Metingen ondersteuning voor een eigenschap detailrijen met een DAX-expressie. Deze eigenschap kunt clienthulpprogramma's zoals Microsoft Excel om in te zoomen naar beneden gedetailleerde gegevens van een samengevoegde lijst. Bijvoorbeeld, wanneer gebruikers totale verkoop voor een regio en de maand bekijken, kunnen ze de details van de bijbehorende bekijken. 
*  Beveiliging op objectniveau voor tabel- en kolomnamen, naast de gegevens daarin.
*  Verbeterde ondersteuning voor niet-aaneengesloten hiërarchieën.
*  Bewaking van prestaties en verbeteringen.
  
## <a name="set-compatibility-level"></a>Het compatibiliteitsniveau instellen 
 Wanneer u een nieuw model in tabelvorm-project in SSDT, kunt u het compatibiliteitsniveau opgeven op de **model in tabelvorm designer** dialoogvenster. 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 Als u selecteert de **dit bericht niet meer weergeven** optie, alle volgende projecten gebruik van het compatibiliteitsniveau die u hebt opgegeven als de standaardwaarde. U kunt het standaardniveau voor compatibiliteit in SSDT in wijzigen **extra** > **opties**.  
  
 Om een upgrade van een bestaand project model in tabelvorm in SSDT, te stellen de **compatibiliteitsniveau** eigenschap in het model **eigenschappen** venster. Houd in gedachten, upgraden van het compatibiliteitsniveau wordt niet ongedaan worden gemaakt.
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a>Controleer het compatibiliteitsniveau voor een tabellaire modeldatabase in SQL Server Management Studio 
 SSMS, met de rechtermuisknop op de databasenaam > **eigenschappen** > **compatibiliteitsniveau**.  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a>Controleer het compatibiliteitsniveau van de ondersteunde versies voor een server in SSMS  
 SSMS, met de rechtermuisknop op de servernaam > **eigenschappen** > **compatibiliteitsniveau ondersteund**.  
  
 Deze eigenschap geeft het hoogste compatibiliteitsniveau van een database die wordt uitgevoerd op de server (met uitzondering van de preview). Het compatibiliteitsniveau van de ondersteunde kan niet worden gewijzigd.  

## <a name="next-steps"></a>Volgende stappen
  [Een model maken in Azure portal](analysis-services-create-model-portal.md)   
  [Analyseservices beheren](analysis-services-manage.md)  
