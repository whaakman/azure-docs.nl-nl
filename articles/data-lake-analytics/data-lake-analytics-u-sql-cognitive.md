---
title: Met behulp van U-SQL cognitieve mogelijkheden in Azure Data Lake Analytics | Microsoft Docs
description: Informatie over het gebruik van de intelligence cognitieve mogelijkheden in U-SQL
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: 019c1d53-4e61-4cad-9b2c-7a60307cbe19
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: saveenr
ms.openlocfilehash: ec48a07af0aba78f2e508bad232f34102f0c2073
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2017
---
# <a name="tutorial-get-started-with-the-cognitive-capabilities-of-u-sql"></a>Zelfstudie: Aan de slag met de cognitieve mogelijkheden van U-SQL

## <a name="overview"></a>Overzicht
Cognitieve mogelijkheden voor de U-SQL kunnen ontwikkelaars put intelligence gebruiken in hun big data-programma's. 

De volgende cognitieve mogelijkheden zijn beschikbaar:
* Imaging: Vlakken detecteren
* Imaging: Emotion detecteren
* Imaging: Objecten (tagging) detecteren
* Imaging: OCR (OCR)
* Tekst: Uitpakken van de belangrijkste woordgroep
* Tekst: Analyse van de gevoel

## <a name="how-to-use-cognitive-in-your-u-sql-script"></a>Het gebruik van Cognitive in uw U-SQL-script

Het algehele proces is eenvoudig:

* De referentie-ASSEMBLY-instructie voor het inschakelen van de cognitieve functies voor de U-SQL-Script gebruiken
* Gebruik van het proces op een Invoerrijenset uitvoer genereren met behulp van een cognitieve UDO rijenset

### <a name="detecting-objects-in-images"></a>Objecten in de afbeeldingen detecteren

Het volgende voorbeeld laat zien hoe de cognitieve mogelijkheden voor het detecteren van objecten in installatiekopieën gebruiken.

```
REFERENCE ASSEMBLY ImageCommon;
REFERENCE ASSEMBLY FaceSdk;
REFERENCE ASSEMBLY ImageEmotion;
REFERENCE ASSEMBLY ImageTagging;
REFERENCE ASSEMBLY ImageOcr;

// Get the image data

@imgs =
    EXTRACT 
        FileName string, 
        ImgData byte[]
    FROM @"/usqlext/samples/cognition/{FileName}.jpg"
    USING new Cognition.Vision.ImageExtractor();

//  Extract the number of objects on each image and tag them 

@tags =
    PROCESS @imgs 
    PRODUCE FileName,
            NumObjects int,
            Tags SQL.MAP<string, float?>
    READONLY FileName
    USING new Cognition.Vision.ImageTagger();

@tags_serialized =
    SELECT FileName,
           NumObjects,
           String.Join(";", Tags.Select(x => String.Format("{0}:{1}", x.Key, x.Value))) AS TagsString
    FROM @tags;

OUTPUT @tags_serialized
    TO "/tags.csv"
    USING Outputters.Csv();
```
Bekijk voor meer voorbeelden de **U-SQL/Cognitive voorbeelden** in de **Vervolgstappen** sectie.

## <a name="next-steps"></a>Volgende stappen
* [U-SQL/cognitieve voorbeelden](https://github.com/Azure-Samples?utf8=✓&q=usql%20cognitive)
* [U-SQL-scripts ontwikkelen met Data Lake-hulpmiddelen voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [U-SQL-vensterfuncties gebruiken voor Azure Data Lake Analytics-taken](data-lake-analytics-use-window-functions.md)
