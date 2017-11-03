---
title: U-SQL-assembly's voor Azure Data Lake Analytics-taken te ontwikkelen | Microsoft Docs
description: 'Informatie over het ontwikkelen van assembly''s worden gebruikt en hergebruikt in Data Lake Analytics-taken. '
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/30/2016
ms.author: jejiang
ms.openlocfilehash: c49f80f8dcd330d7f46726241e7178351b9cc28f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="develop-u-sql-assemblies-for-azure-data-lake-analytics-jobs"></a>Ontwikkelen van U-SQL-assembly's voor Azure Data Lake Analytics-taken
Informatie over het code-behind omzetten in assembly's die moeten worden gebruikt en hergebruikt in Data Lake Analytics-taken. 

U-SQL kunt eenvoudig uw eigen aangepaste code toevoegen in .net-talen, zoals C#, of traditiegetrouw F #. U kunt zelfs uw eigen runtime ter ondersteuning van andere talen implementeren.

De eenvoudigste manier om het gebruik van aangepaste code is met het Data Lake Tools voor Visual Studio code-behind mogelijkheden. Zie voor meer informatie [zelfstudie: U-SQL-scripts ontwikkelen met Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md). Er zijn enkele nadelen van het gebruik van code-behind:

- De broncode wordt voor de verzending van elk script geüpload.
- code-behind kan niet worden gedeeld met andere taken.

Om deze nadelen op te lossen, kunt u code-behind omzetten in assembly's en registreer de assembly's naar de Data Lake Analytics-catalogus.

## <a name="prerequisites"></a>Vereisten
* Visual Studio 2017, Visual Studio 2015, Visual Studio 2013 update 4 of Visual Studio 2012 met Visual C++ geïnstalleerd
* Microsoft Azure SDK voor .NET versie 2.5 of hoger.  Installeren met behulp van de webplatforminstallatieprogramma of Visual Studio Installer
* Een Data Lake Analytics-account.  Zie [Aan de slag met Azure Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md).
* Ga via de [aan de slag met Azure Data Lake Analytics U-SQL-Studio](data-lake-analytics-u-sql-get-started.md) zelfstudie.
* Verbinding maken met Azure.
* Uploaden van de brongegevens, Zie [aan de slag met Azure Data Lake Analytics U-SQL-Studio](data-lake-analytics-u-sql-get-started.md). 

## <a name="develop-assemblies-for-u-sql"></a>Assembly's voor de U-SQL ontwikkelen

**Maken en verzenden van een U-SQL-taak**

1. Klik in het menu **File** op **New** en klik vervolgens op **Project**.
2. Vouw **geïnstalleerde**, **sjablonen**, **Azure Data Lake**, **U-SQL(ADLA)**, selecteer de **Class Library (voor U-SQL-toepassing)** sjabloon en klik vervolgens op **OK**.
3. Schrijf uw code in Class1.cs.  Hier volgt een voorbeeld van code.

        using Microsoft.Analytics.Interfaces;

        namespace USQLApplication_codebehind
        {
            [SqlUserDefinedProcessor]
            public class MyProcessor : IProcessor
            {
                public override IRow Process(IRow input, IUpdatableRow output)
                {
                    output.Set(0, input.Get<string>(0));
                    output.Set(0, input.Get<string>(0));
                    return output.AsReadOnly();
                }
            }
        }
4. Klik op de **bouwen** menu en klik vervolgens op **Build Solution** voor het maken van het DLL-bestand.

## <a name="register-assemblies"></a>Assembly's registreren

Zie [gebruik Data Lake Analytics(U-SQL) catalogus](data-lake-analytics-use-u-sql-catalog.md).


## <a name="use-the-assemblies"></a>De assembly's gebruiken

Zie [gebruik Azure Data Lake Tools voor Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).

## <a name="see-also"></a>Zie ook
* [Aan de slag met Data Lake Analytics met PowerShell](data-lake-analytics-get-started-powershell.md)
* [Aan de slag met Data Lake Analytics met Azure portal](data-lake-analytics-get-started-portal.md)
* [Data Lake Tools voor Visual Studio gebruiken voor het ontwikkelen van U-SQL-toepassingen](data-lake-analytics-data-lake-tools-get-started.md)
* [Gebruik Data Lake Analytics(U-SQL) catalogus](data-lake-analytics-use-u-sql-catalog.md)
* [De Azure Data Lake-tools gebruiken voor Visual Studio-code](data-lake-analytics-data-lake-tools-for-vscode.md)