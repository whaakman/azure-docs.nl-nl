---
title: Eenvoudig gegevens kopiëren met de Wizard kopiëren - Azure | Microsoft Docs
description: Meer informatie over het gebruik van de Kopieerwizard van Data Factory kopiëren van gegevens van ondersteunde gegevensbronnen naar sinks.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: ''
editor: ''
ms.assetid: f904972f-cd33-48db-9755-2b3196ae4168
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 90f78428601d7b039d00d39c1ca8339ab3ace9ba
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54020350"
---
# <a name="copy-or-move-data-easily-with-azure-data-factory-copy-wizard"></a>Kopiëren of verplaatsen van gegevens eenvoudig met Azure Data Factory-Wizard voor kopiëren
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u de [zelfstudie over kopieeractiviteiten](../quickstart-create-data-factory-dot-net.md). 


De Kopieerwizard van Azure Data Factory is het vereenvoudigen de migratie van gegevens opnemen, die meestal een eerste stap in een scenario voor end-to-end data-integratie. Wanneer de Wizard kopiëren van Azure Data Factory te doorlopen, hoeft u niet om een JSON-definities voor de gekoppelde services, gegevenssets en pijplijnen te begrijpen. Echter, nadat u alle stappen in de wizard hebt voltooid, maakt de wizard automatisch een pijplijn om gegevens te kopiëren uit de geselecteerde gegevensbron op de geselecteerde bestemming. Daarnaast de Wizard kopiëren helpt u om te valideren van de gegevens wordt opgenomen op het moment van schrijven, waardoor ze veel van de tijd, met name wanneer u bent ophalen van gegevens voor de eerste keer uit de gegevensbron. De Wizard kopiëren starten, klikt u op de **gegevens kopiëren** tegel op de startpagina van uw data factory.

![De wizard Kopiëren](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="an-intuitive-wizard-for-copying-data"></a>Een intuïtieve wizard voor het kopiëren van gegevens
Deze wizard kunt u eenvoudig om gegevens te verplaatsen van een groot aantal bronnen naar bestemmingen in minuten. Na het doorlopen van de wizard, wordt een pijplijn met copy activity automatisch voor u gemaakt, samen met de afhankelijke Data Factory-entiteiten (gekoppelde services en gegevenssets). Er zijn geen extra stappen vereist om de pijplijn te maken.   

![Gegevensbron selecteren](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Zie [Kopieerwizard zelfstudie](data-factory-copy-data-wizard-tutorial.md) artikel voor stapsgewijze instructies voor het maken van een voorbeeldpijplijn kopiëren van gegevens van een Azure-blob naar een Azure SQL Database-tabel. 
> 
> 

De wizard is ontworpen met big data in gedachten vanaf het begin. Het is eenvoudig en efficiënt te maken van Data Factory-pijplijnen die honderden mappen, bestanden of tabellen met de wizard kopiëren van gegevens te verplaatsen. De wizard ondersteunt de volgende drie onderdelen: Voorbeeld van automatische gegevens, schema vastleggen en toewijzing en filteren van gegevens. 

## <a name="automatic-data-preview"></a>Voorbeeld van automatische gegevens
De wizard kopiëren kunt u bekijken-deel van de gegevens van de geselecteerde gegevensbron te valideren of de gegevens is de juiste gegevens die u wilt kopiëren. Als de brongegevens bevinden zich in een tekstbestand, parseert de wizard kopiëren bovendien het tekstbestand voor rij- en kolomscheidingstekens- en schema automatisch meer. 

![Pagina Instellingen bestandsindelingen](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Vastleggen van de schema- en -toewijzing
Het schema van de ingevoerde gegevens mogelijk niet overeenkomt met het schema van de uitvoergegevens in sommige gevallen. In dit scenario moet u kolommen uit van het schema van de gegevensbron op kolommen uit het doelschema toewijzen. 

De wizard kopiëren worden kolommen in het schema van de gegevensbron automatisch toegewezen aan kolommen in de doelschema. U kunt de toewijzingen overschrijven met behulp van de vervolgkeuzelijsten (of) opgeven of een kolom worden overgeslagen moet tijdens het kopiëren van de gegevens.   

![Schematoewijzing](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Filteren van gegevens
De wizard kunt u voor het filteren van gegevens om alleen de gegevens die moet worden gekopieerd naar het doel/sink-gegevensarchief te selecteren. Filteren vermindert het volume van de gegevens moeten worden gekopieerd naar het sink-gegevensopslag en daarom verhoogt de doorvoer van de kopieerbewerking. Het biedt een flexibele manier om gegevens te filteren in een relationele database met behulp van SQL query language (of)-bestanden in een Azure blob-map met behulp van [Data Factory-functies en variabelen](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Het filteren van gegevens in een database
In het voorbeeld wordt de SQL-query gebruikt de `Text.Format` functie en `WindowStart` variabele. 

![Valideren van expressies](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Het filteren van gegevens in een Azure blob-map
U kunt variabelen gebruiken in het pad naar het kopiëren van gegevens uit een map die is bepaald tijdens uitvoering op basis van [systeemvariabelen](data-factory-functions-variables.md#data-factory-system-variables). De variabelen die ondersteund zijn: **{year}**, **{month}**, **{day}**, **{hour}**, **{minute}**, en **{aangepaste}**. Voorbeeld: inputfolder / {year} / {month} / {day}.

Stel dat u hebt opgegeven dat mappen in de volgende indeling:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Klik op de **Bladeren** knop voor **bestand of map**, blader naar een van deze mappen (bijvoorbeeld: 2016-03 > -> 01-02 >), en klikt u op **kiezen**. U ziet `2016/03/01/02` in het tekstvak in. Vervang nu, **2016** met **{year}**, **03** met **{month}**, **01** met **{day}** , en **02** met **{hour}**, en druk op Tab. Hier ziet u vervolgkeuzelijsten om de indeling voor deze vier variabelen te selecteren:

![Met behulp van de systeemvariabelen](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Zoals weergegeven in de volgende schermafbeelding, u kunt ook een **aangepaste** variabele en een [opmaaktekenreeksen ondersteund](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Selecteer een map met die structuur, gebruikt u de **Bladeren** eerst knop. Vervang vervolgens een waarde met **{aangepaste}**, en druk op Tab om te zien van het tekstvak u de opmaaktekenreeks typt.     

![Met behulp van aangepaste variabele](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="support-for-diverse-data-and-object-types"></a>Ondersteuning voor diverse gegevens en objecttypen
Met behulp van de Wizard kopiëren, kunt u honderden van mappen, bestanden of tabellen efficiënt te verplaatsen.

![Tabellen selecteren waaruit u gegevens kopiëren](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Opties voor het plannen
U kunt de kopieerbewerking uitvoeren eenmalig of volgens een schema (per uur, dagelijks, enzovoort). Beide opties kunnen worden gebruikt voor de breedte van de connectors voor de on-premises, cloud en lokale bureaublad kopie.

Een eenmalige kopieerbewerking kan slechts één keer verplaatsing van gegevens van een bron naar een bestemming. Dit geldt voor gegevens van elke grootte en elke ondersteunde indeling. De geplande kopie kunt u het kopiëren van gegevens op een voorgeschreven terugkeerpatroon. U kunt uitgebreide instellingen (zoals opnieuw proberen, time-out en waarschuwingen) gebruiken voor het configureren van de geplande kopie.

![Eigenschappen van planning](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor een snel overzicht van het gebruik van de Kopieerwizard van Data Factory te maken van een pijplijn met Copy Activity [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md).

