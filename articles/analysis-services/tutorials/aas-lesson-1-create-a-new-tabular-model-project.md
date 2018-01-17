---
title: 'Zelfstudie Azure Analysis Services - Les 1: Een nieuw project voor een tabellair model maken | Microsoft Docs'
description: In deze les wordt beschreven hoe u een nieuw project voor een tabellair model in Azure Analysis Services maakt.
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
ms.openlocfilehash: fbe0784ae133a0b9a54c94b4ba3db317c14b3766
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="create-a-tabular-model-project"></a>Een project voor een tabellair model maken

In deze les gebruikt u Visual Studio met SQL Server Data Tools (SSDT) om een nieuw project voor een tabellair model te maken op het compatibiliteitsniveau 1400. Zodra het nieuwe project is gemaakt, kunt u gegevens gaan toevoegen en het model gaan ontwerpen. In deze les krijgt u ook een korte inleiding op de omgeving in Visual Studio voor het ontwerpen van een tabellair model.  
  
Geschatte tijd voor het voltooien van deze les: **10 minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Dit is de eerste les in een zelfstudie over het ontwerpen van een tabellair model. U kunt deze les alleen voltooien als aan verschillende vereisten is voldaan. Zie [Azure Analysis Services - Adventure Works-zelfstudie](../tutorials/aas-adventure-works-tutorial.md) voor meer informatie.  
  
## <a name="create-a-new-tabular-model-project"></a>Een nieuw project voor een tabellair model maken  
  
#### <a name="to-create-a-new-tabular-model-project"></a>Een nieuw project voor een tabellair model maken:  
  
1.  Open in Visual Studio het menu **File** en klik op **New** > **Project**.  
  
2.  Vouw in het dialoogvenster **New Project** **Installed** > **Business Intelligence** > **Analysis Services** uit en klik vervolgens op **Analysis Services Tabular Project**.  
  
3.  Typ **AW Internet Sales** in het vak **Name** en geef vervolgens een locatie voor de projectbestanden op.  
  
    De waarde voor **Solution Name** is standaard hetzelfde als de naam van het project, maar u kunt desgewenst een andere naam opgeven voor de oplossing.  
  
4.  Klik op **OK**.  
  
5.  Selecteer **Integrated workspace** in het dialoogvenster **Tabular model designer**.  
  
    De werkruimte bevat tijdens het ontwerpen van het model een tabellaire modeldatabase met dezelfde naam als het project. De optie Integrated workspace betekent dat Visual Studio een ingebouwd exemplaar gebruikt, zodat u niet alleen voor het ontwerpen van het model een afzonderlijke Analysis Services-server hoeft te installeren.
      
6.  Selecteer **SQL Server 2017 / Azure Analysis Services (1400)** bij **Compatibility level**.   
 
    ![aas-lesson1-tmd](../tutorials/media/aas-lesson1-tmd.png)
      
    Als u SQL Server 2017 / Azure Analysis Services (1400) niet ziet staan in de vervolgkeuzelijst Compatibility level, gebruikt u niet de nieuwste versie van SQL Server Data Tools. Zie [SQL Server Data Tools installeren](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) om de nieuwste versie te downloaden.  
      
  
## <a name="understanding-the-ssdt-tabular-model-authoring-environment"></a>Overzicht van de SSDT-ontwerpomgeving voor tabellaire modellen  
U hebt net een nieuw project voor een tabellair model gemaakt. Het is handig om nu eerst even de omgeving van Visual Studio voor het ontwerpen van tabellaire modellen te bekijken.  
  
Nadat uw project is gemaakt, wordt het geopend in Visual Studio. Aan de rechterkant, bij **Tabular Model Explorer**, ziet u een structuurweergave van de objecten in uw model. Aangezien u nog geen gegevens hebt geïmporteerd, zijn de mappen leeg. Klik met de rechtermuisknop op een objectmap om bewerkingen uit te voeren. De beschikbare opties zijn ook op te roepen via de menubalk. Tijdens het doorlopen van de stappen van deze zelfstudie, kunt u Tabular Model Explorer gebruiken om door verschillende objecten in uw modelproject te bladeren.

![aas-lesson1-tme](../tutorials/media/aas-lesson1-tme.png)

Klik op het tabblad **Solution Explorer**. Hier ziet u het bestand **Model.bim**. Als u het ontwerpvenster aan de linkerkant niet ziet (het lege venster met het tabblad Model.bim), gaat u in **Solution Explorer** naar **AW Internet Sales** en dubbelklikt u op het bestand **Model.bim**. Het bestand Model.bim bevat de metagegevens voor het modelproject. 

![aas-lesson1-se](../tutorials/media/aas-lesson1-se.png)
  
Klik op **Model.bim**. In het venster **Properties** ziet u de modeleigenschappen, met als belangrijkste eigenschap **DirectQuery Mode**. Deze eigenschap bepaalt of het model wordt geïmplementeerd in de modus In-Memory (Off) of de modus DirectQuery (On). Voor deze zelfstudie gaat u het model ontwerpen en implementeren in de modus In-Memory.

![aas-lesson1-properties](../tutorials/media/aas-lesson1-properties.png)
  
Wanneer u een modelproject maakt, worden bepaalde eigenschappen van het model automatisch ingesteld volgens de instellingen voor het modelleren van gegevens (die kunnen worden opgegeven via **Tools** > **Options**). De eigenschappen Data Backup, Workspace Retention en Workspace Server bepalen hoe en waar er een back-up van de werkruimtedatabase (de database met het modelontwerp) wordt gemaakt, hoe de database in het geheugen wordt opgeslagen en hoe de database wordt gebouwd. U kunt deze instellingen later altijd nog wijzigen, maar voorlopig laten we deze eigenschappen even voor wat ze zijn.  

Klik in **Solution Explorer** met de rechtermuisknop op **AW Internet Sales** (project) en klik vervolgens op **Properties**. Het dialoogvenster **AW Internet Sales Property Pages** wordt weergegeven. Enkele van deze eigenschappen gaat u later instellen, wanneer u het model implementeert.  
  
Tijdens de installatie van SSDT, zijn er een paar nieuwe menuopdrachten toegevoegd aan de Visual Studio-omgeving. Klik op het menu **Model**. Met de opdrachten in dit menu kunt u gegevens importeren, gegevens in de werkruimte vernieuwen, door het model bladeren in Excel, perspectieven en rollen maken, de modelweergave selecteren en berekeningsopties instellen. Klik op het menu **Table**. Hier ziet u opdrachten voor het maken en beheren van relaties, het opgeven van instellingen voor gegevenstabellen, het maken van partities en het bewerken van tabeleigenschappen. Als u op het menu **Column** klikt, kunt u kolommen toevoegen en verwijderen in een tabel, kolommen blokkeren en de sorteervolgorde opgeven. Er worden ook enkele knoppen door SSDT toegevoegd aan de werkbalk. De handigste functie is AutoSum. Hiermee kunt u een standaardaggregatiemeting maken voor een geselecteerde kolom. Andere knoppen op de werkbalk bieden snelle toegang tot veelgebruikte functies en opdrachten.  
  
Bekijk enkele dialoogvensters en locaties voor verschillende functies die specifiek zijn voor het ontwerpen van tabellaire modellen. Hoewel sommige items nog niet actief zijn, kunt u zo toch een goed beeld krijgen de ontwerpomgeving voor tabellaire modellen.  
  

## <a name="whats-next"></a>Volgende stappen
[Les 2: Gegevens ophalen](../tutorials/aas-lesson-2-get-data.md).

  
  
  
