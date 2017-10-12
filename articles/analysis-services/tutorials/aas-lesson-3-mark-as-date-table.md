---
title: 'Azure Analysis Services-zelfstudie - Les 3: Als gegevenstabel markeren | Microsoft Docs'
description: In deze les wordt beschreven hoe u een gegevenstabel markeert in de zelfstudie over Azure Analysis Services.
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
ms.date: 09/20/2017
ms.author: owend
ms.openlocfilehash: 94ea44091c8a7befaf0c82349c47dce7a36c1fd8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="lesson-3-mark-as-date-table"></a>Les 3: Als gegevenstabel markeren

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In les 2, Gegevens ophalen, hebt u een dimensietabel met de naam DimDate geïmporteerd. Hoewel deze tabel in uw model DimDate heet, kan er ook naar worden verwezen als een *gegevenstabel*, simpelweg omdat de tabel datum- en tijdgegevens bevat.  
  
Wanneer u tijd-intelligente DAX-functies gebruikt, zoals later als u metingen gaat maken, moet u eigenschappen opgeven die een *gegevenstabel* en een unieke id *Gegevenskolom* in die tabel bevatten.
  
In deze les markeert u de tabel DimDate als de *gegevenstabel* en de kolom Date (in de tabel Date) als de *gegevenskolom* (de unieke id).  

Voordat we de gegevenstabel en de gegevenskolom gaan markeren, is dit een goed moment om een wijziging door te voeren die ervoor zorgt dat het model wat gemakkelijker te begrijpen is. U ziet in de tabel DimDate een kolom met de naam **FullDateAlternateKey**. Deze kolom bevat één rij voor elke dag in elk kalenderjaar dat is opgenomen in de tabel. U gebruikt deze kolom veel in formules voor metingen en in rapporten. Maar eigenlijk is FullDateAlternateKey niet echt een goede id voor deze kolom. We wijzigen de naam in **Date**, waardoor u de kolom gemakkelijker kan vinden en gebruiken in formules. Indien mogelijk is het altijd een goed idee om de namen van objecten zoals tabellen en kolommen te wijzigen, zodat ze gemakkelijker zijn te herkennen in SSDT en in clientrapportagetoepassingen zoals Power BI en Excel. 
  
Geschatte tijd voor het voltooien van deze les: **3 minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Dit onderwerp maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. De lessen van de zelfstudie moeten op volgorde worden uitgevoerd. Voordat u de taken in deze les gaat uitvoeren, moet u de vorige les hebben voltooid: [Les 2: Gegevens ophalen](../tutorials/aas-lesson-2-get-data.md). 

### <a name="to-rename-the-fulldatealternatekey-column"></a>De naam wijzigen van de kolom FullDateAlternateKey:

1.  Klik in de ontwerpfunctie voor modellen op de tabel **DimDate**.

2.  Dubbelklik op de kop van de kolom **FullDateAlternateKey** en wijzig de naam in **Date**.

  
### <a name="to-set-mark-as-date-table"></a>De tabel als gegevenstabel markeren:  
  
1.  Selecteer de kolom **Date** en zorg vervolgens dat in het venster **Properties**, onder **Date Type**, de optie **Date** is geselecteerd.  
  
2.  Klik op het menu **Table** en klik daarna op **Date** en **Mark as Date Table**.  
  
3.  Selecteer in het dialoogvenster **Mark as Date Table**, in de vervolgkeuzelijst **Date**, de kolom **Date** als de unieke id. Deze kolom is meestal standaard geselecteerd. Klik op **OK**. 

    ![aas-lesson3-date-table](../tutorials/media/aas-lesson3-date-table.png)
  

## <a name="whats-next"></a>Volgende stappen
[Les 4: Relaties maken](../tutorials/aas-lesson-4-create-relationships.md).
  
