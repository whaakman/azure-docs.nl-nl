---
title: "Eenvoudig gegevens kopiëren met de Wizard kopiëren - Azure | Microsoft Docs"
description: "Meer informatie over het gebruik van de Data Factory-Wizard kopiëren om gegevens van ondersteunde gegevensbronnen te put kopiëren."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f904972f-cd33-48db-9755-2b3196ae4168
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: c993b1dfb0055da84751c042efccf42d943375d9
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-or-move-data-easily-with-azure-data-factory-copy-wizard"></a>Gegevens eenvoudig met Azure Data Factory-Wizard voor kopiëren kopiëren of verplaatsen
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service gebruikt, welke nog een preview is, raadpleegt u de [zelfstudie kopieeractiviteit in de documentatie voor versie 2](../quickstart-create-data-factory-dot-net.md). 


De Wizard kopiëren van Azure Data Factory is om te vereenvoudigen het opnemen van gegevens, die meestal een eerste stap in een end-to-end gegevens integratiescenario is. Bij gebruik van de Wizard kopiëren van Azure Data Factory, hoeft u niet elk JSON-definities voor de gekoppelde services, gegevenssets en pijplijnen begrijpen. Echter, nadat u de stappen in de wizard hebt voltooid, maakt de wizard automatisch een pijplijn om gegevens te kopiëren van de geselecteerde gegevensbron naar de geselecteerde bestemming. Daarnaast de Wizard kopiëren helpt u om de gegevens wordt ingenomen op het moment van ontwerpen, waarin wordt opgeslagen veel van de tijd, te valideren met name wanneer u zijn ophalen van gegevens voor de eerste keer uit de gegevensbron. Voor het starten van de Wizard kopiëren, klikt u op de **gegevens kopiëren** tegel op de startpagina van uw gegevensfactory.

![De wizard Kopiëren](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="an-intuitive-wizard-for-copying-data"></a>Een intuïtieve wizard voor het kopiëren van gegevens
Deze wizard kunt u eenvoudig om gegevens te verplaatsen van een groot aantal gegevensbronnen naar bestemmingen in minuten. Na het doorlopen van de wizard wordt een pijplijn met een kopieeractiviteit automatisch gemaakt voor u samen met afhankelijke Data Factory-entiteiten (gekoppelde services en gegevenssets). Er zijn geen extra stappen nodig voor het maken van de pijplijn.   

![Gegevensbron selecteren](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Zie [Wizard kopiëren zelfstudie](data-factory-copy-data-wizard-tutorial.md) artikel voor stapsgewijze instructies voor het maken van een voorbeeldpijplijn kopiëren van gegevens van een Azure-blob naar een Azure SQL Database-tabel. 
> 
> 

De wizard is ontworpen met big data in gedachten vanaf het begin. Het is eenvoudig en efficiënt Data Factory-pijplijnen verplaatsen honderden mappen, bestanden of tabellen met de wizard kopiëren schrijven. De wizard ondersteunt de volgende drie functies: voorbeeld van automatische gegevens, schema vastleggen en toewijzing en het filteren van gegevens. 

## <a name="automatic-data-preview"></a>Voorbeeld van automatische gegevens
De wizard kopiëren kunt u bekijken deel uit van de gegevens van de geselecteerde gegevensbron u wilt controleren of de gegevens is de juiste gegevens die u wilt kopiëren. Als de brongegevens zich in een tekstbestand, parseert de wizard kopiëren bovendien het tekstbestand voor meer informatie over automatisch rij en kolom scheidingstekens en schema. 

![Bestandsindelingsinstellingen](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Schema vastleggen en toewijzing
Het schema van de invoergegevens mogelijk niet overeen met het schema van uitvoergegevens in sommige gevallen. In dit scenario moet u de kolommen van het schema van de gegevensbron aan kolommen van het doelschema toewijzen. 

De wizard kopiëren kolommen in het schema van de gegevensbron wordt automatisch worden toegewezen aan kolommen in het schema van de bestemming. U kunt de toewijzingen overschrijven met behulp van de vervolgkeuzelijsten (of) opgeven of een kolom worden overgeslagen moet bij het kopiëren van de gegevens.   

![Schematoewijzing](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Filteren van gegevens
De wizard kunt u filteren brongegevens om alleen de gegevens selecteren die moet worden gekopieerd naar de bestemming/sink-gegevensopslag. Filteren vermindert het volume van de gegevens worden gekopieerd naar het gegevensarchief sink en daarom verhoogt de doorvoer van de kopieerbewerking. Het biedt een flexibele manier om gegevens te filteren in een relationele database met behulp van SQL query language (of)-bestanden in de map van een Azure-blob met behulp van [Data Factory-functies en variabelen](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Filteren van gegevens in een database
In het voorbeeld wordt de SQL-query gebruikt de `Text.Format` functie en `WindowStart` variabele. 

![Expressies valideren](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Filteren van gegevens in een Azure blob-map
U kunt variabelen gebruiken in het pad van de gegevens gekopieerd van een map die is bepaald tijdens runtime op basis van [systeemvariabelen](data-factory-functions-variables.md#data-factory-system-variables). De variabelen die ondersteund zijn: **{year}**, **{month}**, **{day}**, **{uur}**, **{minuut}**, en  **{{aangepaste}** . Voorbeeld: inputfolder / {year} / {month} / {day}.

Stel dat ingevoerde mappen in de volgende indeling hebben:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Klik op de **Bladeren** knop voor **bestand of map**, blader naar een van deze mappen (bijvoorbeeld: 2016 -> 03 -> 01-02 >), en klik op **Kies**. U ziet `2016/03/01/02` in het tekstvak. Vervang nu, **2016** met **{year}**, **03** met **{month}**, **01** met **{day}** , en **02** met **{uur}**, en druk op Tab. Hier ziet u vervolgkeuzelijsten om de indeling voor deze vier variabelen te selecteren:

![Met behulp van systeemvariabelen](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

In de volgende schermafbeelding ziet u kunt ook een **aangepaste** variabele en eventuele [indeling tekenreeksen ondersteund](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Selecteer een map met die structuur, gebruiken de **Bladeren** eerst knop. Vervangt een waarde met **{aangepaste}**, en druk op Tab om te zien van het tekstvak typt u de opmaakreeks op.     

![Met behulp van aangepaste variabele](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="support-for-diverse-data-and-object-types"></a>Ondersteuning voor diverse gegevens en objecttypen
Met behulp van de Wizard kopiëren, kunt u honderden mappen, bestanden of tabellen efficiënt te verplaatsen.

![Selecteer tabellen waarvan om gegevens te kopiëren](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Opties voor het plannen
U kunt de kopieerbewerking uitvoeren nadat de of volgens een schema (elk uur, dagelijks, enzovoort). Beide opties kunnen worden gebruikt voor de breedte van de verbindingslijnen over on-premises, cloud en lokale bureaublad kopie.

Een eenmalige kopieerbewerking kan slechts één keer verplaatsing van gegevens van een bron naar doel. Dit geldt voor gegevens van elke grootte en elke ondersteunde indeling. De geplande kopie kunt u kopiëren van gegevens op een voorgeschreven terugkeerpatroon. U kunt uitgebreide instellingen (zoals opnieuw proberen, time-out en waarschuwingen) gebruiken voor het configureren van de geplande kopie.

![Eigenschappen plannen](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor een snel overzicht van de Data Factory-Wizard kopiëren via een pijplijn maken met de Kopieeractiviteit [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md).

