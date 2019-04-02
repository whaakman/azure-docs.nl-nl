---
title: Gegevens modelleren compatibiliteitsniveau in Azure Analysis Services | Microsoft Docs
description: Inzicht in gegevens in tabelvorm model compatibiliteitsniveau.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 67a6c99253c549f0b8d3b55809b35b81756843eb
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58803490"
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a>Compatibiliteitsniveau voor tabellaire modellen van Analysis Services

*Compatibiliteitsniveau* verwijst naar de release-specifieke gedrag in de Analysis Services-engine. Wijzigingen in het compatibiliteitsniveau is doorgaans valt samen met belangrijke releases van SQL Server. Deze wijzigingen worden ook geïmplementeerd in Azure Analysis Services te onderhouden pariteit tussen beide platforms. Compatibiliteitsniveau verandert ook effect die beschikbaar zijn in uw modellen in tabelvorm. Bijvoorbeeld, hebben DirectQuery en metagegevens in tabelvorm object verschillende implementaties, afhankelijk van het compatibiliteitsniveau. Compatibiliteitsniveau is opgegeven in het project voor tabellair model in Visual Studio (SSDT).

Azure Analysis Services ondersteunt tabellaire modellen met het compatibiliteitsniveau 1200 en 1400. De meest recente versie van het compatibiliteitsniveau is 1400. Dit niveau overeenkomt met de SQL Server 2017 Analysis Services. Belangrijkste functies in het compatibiliteitsniveau 1400 zijn onder andere:

*  Nieuwe functies voor toegang tot gegevens en importeren met ondersteuning voor TOM APIs en het uitvoeren van TMSL-scripts. 
*  Gegevenstransformatie en mashup mogelijkheden om gegevens met behulp van gegevens ophalen en M-expressies.
*  Metingen ondersteuning voor detailrijen eigenschap met een DAX-expressie. Met deze eigenschap kunt clienthulpprogramma's zoals Microsoft Excel om te zoomen op gedetailleerde gegevens van een samengevoegde rapport. Wanneer gebruikers totale verkoop voor een regio en de maand bekijken, kunnen ze bijvoorbeeld de details van de bijbehorende weergeven. 
*  Beveiliging op objectniveau voor tabel- en kolomnamen, naast de gegevens daarin.
*  Verbeterde ondersteuning voor niet-aaneengesloten hiërarchieën.
*  Bewaking van prestaties en verbeteringen.

> [!NOTE]
> Azure Analysis Services biedt ondersteuning voor geïmporteerde Power BI Desktop-bestanden op het compatibiliteitsniveau 1465. Het importeren van Power BI Desktop-functionaliteit, die een preview-functie altijd is, is echter buiten gebruik gesteld en verwijderd uit de service in maart 2019. Bestaande modellen op het compatibiliteitsniveau 1465 blijven ondersteund.  


## <a name="set-compatibility-level"></a>Compatibiliteitsniveau van de set

 Als u een nieuw project voor tabellair model in SSDT, kunt u het compatibiliteitsniveau van de **ontwerpfunctie Tabellair model** dialoogvenster. 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 Als u selecteert de **dit bericht niet opnieuw weergeven** optie, alle volgende projecten wordt gebruikgemaakt van het compatibiliteitsniveau die u hebt opgegeven als de standaardwaarde. U kunt het standaardcompatibiliteitsniveau in SSDT in wijzigen **extra** > **opties**.  
  
 Als u wilt upgraden van een bestaand project voor tabellair model in SSDT, stel de **compatibiliteitsniveau** eigenschap in het model **eigenschappen** venster. Denk eraan dat, de uitbreiding van het compatibiliteitsniveau is niet ongedaan worden gemaakt.
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a>Controleer het compatibiliteitsniveau voor een tabellaire modeldatabase in SQL Server Management Studio 

 In SSMS, met de rechtermuisknop op de databasenaam > **eigenschappen** > **compatibiliteitsniveau**.  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a>Ondersteunde compatibiliteitsniveau voor een server in SSMS controleren  

 In SSMS, met de rechtermuisknop op de servernaam > **eigenschappen** > **compatibiliteitsniveau ondersteund**.  
  
 Deze eigenschap geeft u het hoogste compatibiliteitsniveau van een database die wordt uitgevoerd op de server (exclusief Preview-versie). Het compatibiliteitsniveau van de ondersteunde kan niet worden gewijzigd.  

> [!NOTE]
> In SSMS, als die zijn verbonden met een Azure Analysis Services-server, de **compatibiliteitsniveau ondersteund** ziet u de eigenschap **1200**. Dit is een bekend probleem en wordt opgelost in een toekomstige SSMS update. Wanneer is opgelost, wordt deze eigenschap het hoogste compatibiliteitsniveau van de ondersteunde weergegeven.

## <a name="next-steps"></a>Volgende stappen

  [Analyseservices beheren](analysis-services-manage.md)  
