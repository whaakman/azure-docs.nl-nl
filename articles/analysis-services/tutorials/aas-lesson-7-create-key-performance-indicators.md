---
title: 'Azure Analysis Services-zelfstudie - Les 7: Key Performance Indicators maken | Microsoft Docs'
description: In deze les wordt beschreven hoe u Key Performance Indicator maakt in de zelfstudie over Azure Analysis Services.
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 01/08/2018
ms.author: owend
ms.openlocfilehash: 8f25773541deba2a94d3adc0c9b61c1b90a90aa6
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="create-key-performance-indicators"></a>Key Performance Indicators maken

In deze les gaat u Key Performance Indicators (KPI's) maken. KPI's worden gebruikt om de prestaties van een waarde te meten door een basislijn (*Base*), opgegeven als een meting, te vergelijken met een doelwaarde (*Target*), die ook is gedefinieerd met een meting of met een absolute waarde. In rapportageclienttoepassingen zijn KPI's een snelle en gemakkelijke manier om een overzicht te krijgen van de succesfactoren van een onderneming of om trends te identificeren. Zie [KPI's](https://docs.microsoft.com/sql/analysis-services/tabular-models/kpis-ssas-tabular) voor meer informatie.
  
Geschatte tijd voor het voltooien van deze les: **15 minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Dit onderwerp maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. De lessen van de zelfstudie moeten op volgorde worden uitgevoerd. Voordat u de taken in deze les gaat uitvoeren, moet u de vorige les hebben voltooid: [Les 6: Metingen maken](../tutorials/aas-lesson-6-create-measures.md).   
  
## <a name="create-key-performance-indicators"></a>Key Performance Indicators maken  
  
#### <a name="to-create-an-internetcurrentquartersalesperformance-kpi"></a>Een KPI InternetCurrentQuarterSalesPerformance maken:  
  
1.  Klik in de ontwerpfunctie voor modellen op de tabel **FactInternetSales**.  
  
2.  Klik in het metingenraster op een lege cel.  
  
3.  Typ op de formulebalk boven de tabel de volgende formule: 
 
    ```  
    InternetCurrentQuarterSalesPerformance :=DIVIDE([InternetCurrentQuarterSales]/[InternetPreviousQuarterSalesProportionToQTD],BLANK())  
    ```

    Deze meting fungeert als de basislijn voor de KPI.  
  
4.  Klik in het metingenraster met de rechtermuisknop op **InternetCurrentQuarterSalesPerformance** > **Create KPI**.   
  
5.  Ga naar **Target** in het dialoogvenster Key Performance Indicator (KPI) en selecteer **Absolute value** en typ vervolgens **1.1**.  
  
7.  Typ **1** in het veld met de schuifregelaar voor lage waarden (links) en **1.07** in het veld met de schuifregelaar voor hoge waarden (rechts).  
  
8.  Selecteer bij **Select icon style** de reeks met een ruit (rood), driehoek (geel) en cirkel (groen).
  
    ![aas-lesson7-kpi](../tutorials/media/aas-lesson7-kpi.png)
    
    > [!TIP]  
    > U kunt desgewenst een beschrijving weergeven door **Descriptions** uit te vouwen onder de beschikbare pictogramreeksen. Gebruik beschrijvingen voor de verschillende onderdelen van de KPI, zodat ze makkelijker herkenbaar zijn in clienttoepassingen.  
  
9. Klik op **OK** om de KPI te voltooien.  
  
    In het metingenraster ziet u nu een pictogram naast de meting **InternetCurrentQuarterSalesPerformance**. Dit pictogram geeft aan dat deze meting fungeert als een basislijn voor een KPI.  
  
#### <a name="to-create-an-internetcurrentquartermarginperformance-kpi"></a>Een KPI InternetCurrentQuarterMarginPerformance maken:  
  
1.  Klik in metingenraster voor de tabel **FactInternetSales** op een lege cel.  
  
2.  Typ op de formulebalk boven de tabel de volgende formule:  

    ```
    InternetCurrentQuarterMarginPerformance :=IF([InternetPreviousQuarterMarginProportionToQTD]<>0,([InternetCurrentQuarterMargin]-[InternetPreviousQuarterMarginProportionToQTD])/[InternetPreviousQuarterMarginProportionToQTD],BLANK())  
    ```
 
3.  Klik met de rechtermuisknop op **InternetCurrentQuarterMarginPerformance** > **Create KPI**.  
  
4.  Ga naar **Target** in het dialoogvenster Key Performance Indicator (KPI) en selecteer **Absolute value** en typ vervolgens **1.25**.   
  
5.  Sleep de schuifregelaar voor lage waarden (links) totdat **0.8** wordt weergegeven en sleep vervolgens de schuifregelaar voor hoge waarden (rechts) totdat **1.03** wordt weergegeven.  
  
6.  Selecteer bij **Select icon style** de reeks met een ruit (rood), driehoek (geel) en cirkel (groen), en klik vervolgens op **OK**.  
  
## <a name="whats-next"></a>Volgende stappen
[Les 8: Perspectieven maken](../tutorials/aas-lesson-8-create-perspectives.md).
  
  
