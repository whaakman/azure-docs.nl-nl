---
title: 'Azure Analysis Services-zelfstudie - Les 8: Perspectieven maken| Microsoft Docs'
description: In deze les wordt beschreven hoe u perspectieven maakt in de zelfstudie over Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c26da5d047a1e904510f96b44d81632b6e98689e
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54190966"
---
# <a name="create-perspectives"></a>Perspectieven maken

In deze les maakt u een perspectief voor internetverkopen. Een perspectief definieert een subset van een model dat u kunt weergeven om gerichte, bedrijfsspecifieke of toepassingsspecifieke standpunten inzichtelijk te maken. Wanneer een gebruiker via een perspectief verbinding maakt met een model, worden alleen de modelobjecten (tabellen, kolommen, metingen, hiërarchieën en KPI's) weergegeven die als veld zijn gedefinieerd in dat perspectief. Zie [Perspectieven](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular) voor meer informatie.
  
In deze les gaat u een perspectief maken (Internet Sales) waarmee het tabelobject DimCustomer wordt uitgesloten van weergave. Wanneer u een perspectief maakt waarmee bepaalde objecten worden uitgesloten voor weergave, is dit object nog steeds aanwezig in het model. Het object wordt echter niet vermeld in de lijst met velden van de rapportageclient. Berekende kolommen en metingen die al dan niet zijn opgenomen in een perspectief, kunnen nog steeds werken met uitgesloten objectgegevens.  
  
In deze les leert u hoe u perspectieven maakt en hoe u de hulpmiddelen van het programma voor het ontwerpen van tabellaire modellen gebruikt. Als u dit model later gaat uitbreiden met aanvullende tabellen, kunt u ook extra perspectieven maken om verschillende standpunten van het model te definiëren, zoals Inventory en Sales.  
  
Geschatte tijd voor het voltooien van deze les: **Vijf minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Dit onderwerp maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. De lessen van de zelfstudie moeten op volgorde worden uitgevoerd. Voordat u de taken in deze les gaat uitvoeren, moet u de vorige les hebben voltooid: [Les 7: Key Performance Indicators maken](../tutorials/aas-lesson-7-create-key-performance-indicators.md).  
  
## <a name="create-perspectives"></a>Perspectieven maken  
  
#### <a name="to-create-an-internet-sales-perspective"></a>Een perspectief Internet Sales maken:  
  
1.  Klik op het menu **Model** > **Perspectives** > **Create and Manage**.  
  
2.  Klik in het dialoogvenster **Perspectives** op **New Perspective**.  
  
3.  Dubbelklik op de kolomkop **New Perspective** en wijzig de naam van de kolom in **Internet Sales**.  
  
4.  Selecteer alle tabellen *behalve* **DimCustomer**.  
  
    ![aas-lesson8-perspectives](../tutorials/media/aas-lesson8-perspectives.png)
  
    In een latere les gebruikt u de functie Analyseren van Excel om dit perspectief te testen. De lijst met draaitabelvelden van Excel bevat dan alle tabellen, behalve de tabel DimCustomer.  

## <a name="whats-next"></a>Volgende stappen
[Les 9: Hiërarchieën maken](../tutorials/aas-lesson-9-create-hierarchies.md).
  
  
  
  
