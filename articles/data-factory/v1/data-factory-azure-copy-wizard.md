---
title: "Data Factory-Wizard Azure kopiëren | Microsoft Docs"
description: "Meer informatie over het gebruik van de Data Factory-Wizard voor kopiëren van Azure om gegevens van ondersteunde gegevensbronnen te put kopiëren."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 0974eb40-db98-4149-a50d-48db46817076
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: d9f3fea0db5a08fc91d9e4dc525b48575c512634
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="azure-data-factory-copy-wizard"></a>Azure Data Factory-Wizard kopiëren
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. 

De Wizard kopiëren van Azure Data Factory vereenvoudigt het proces van het opnemen van gegevens, die meestal een eerste stap in een end-to-end gegevens integratiescenario is. Bij gebruik van de Wizard kopiëren van Azure Data Factory, hoeft u niet elk JSON-definities voor de gekoppelde services, gegevenssets en pijplijnen begrijpen. De wizard maakt automatisch een pijplijn om gegevens te kopiëren van de geselecteerde gegevensbron naar de geselecteerde bestemming. Daarnaast helpt de Wizard kopiëren u om de gegevens op het moment van ontwerpen wordt ingenomen te valideren. Dit bespaart tijd, vooral wanneer u zijn ophalen van gegevens voor de eerste keer uit de gegevensbron. Voor het starten van de Wizard kopiëren, klikt u op de **gegevens kopiëren** tegel op de startpagina van uw gegevensfactory.

![De wizard Kopiëren](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="designed-for-big-data"></a>Ontworpen voor big data
Deze wizard kunt u eenvoudig om gegevens te verplaatsen van een groot aantal gegevensbronnen naar bestemmingen in minuten. Nadat u de wizard doorloopt, wordt een pijplijn met een kopieeractiviteit automatisch voor u gemaakt, samen met afhankelijke Data Factory-entiteiten (gekoppelde services en gegevenssets). Er zijn geen extra stappen nodig voor het maken van de pijplijn.   

![Gegevensbron selecteren](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Voor stapsgewijze instructies voor het maken van een voorbeeldpijplijn om gegevens te kopiëren van een Azure-blob naar een Azure SQL Database-tabel, raadpleegt u de [Wizard kopiëren zelfstudie](data-factory-copy-data-wizard-tutorial.md).
>
>

De wizard is ontworpen met big data in gedachten vanaf het begin, met ondersteuning voor diverse gegevens en objecttypen. U kunt Data Factory-pijplijnen verplaatsen honderden mappen, bestanden of tabellen schrijven. De wizard ondersteunt voorbeeld van automatische gegevens, schema vastleggen en toewijzing en het filteren van gegevens.

## <a name="automatic-data-preview"></a>Voorbeeld van automatische gegevens
Onderdeel van de gegevens van de geselecteerde gegevensbron kunt u bekijken om te valideren of de gegevens wilt kopiëren. Als de brongegevens zich in een tekstbestand, parseert de Wizard kopiëren bovendien het tekstbestand voor meer informatie over de rij en kolom scheidingstekens en het schema automatisch.

![Bestandsindelingsinstellingen](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Schema vastleggen en toewijzing
Het schema van de invoergegevens mogelijk niet overeen met het schema van uitvoergegevens in sommige gevallen. In dit scenario moet u de kolommen van het schema van de gegevensbron aan kolommen van het doelschema toewijzen.

> [!TIP]
> Bij het kopiëren van gegevens uit SQL Server of Azure SQL Database in Azure SQL Data Warehouse, als de tabel bestaat niet in het doelarchief, ondersteuning voor Data Factory automatisch tabel maken met behulp van het schema van de gegevensbron. Klik hier als u meer wilt weten van [gegevens verplaatsen en naar Azure SQL Data Warehouse met behulp van Azure Data Factory](./data-factory-azure-sql-data-warehouse-connector.md).
>

Selecteer een kolom in het schema van de gegevensbron om toe te wijzen aan een kolom in het doelschema met gebruik van een vervolgkeuzelijst. De Wizard kopiëren probeert te begrijpen van het patroon voor kolomtoewijzing. Voorbeeld hetzelfde patroon voor de rest van de kolommen, zodat u niet wilt dat elk van de kolommen afzonderlijk om te voltooien van de schematoewijzing selecteren. Als u liever, kunt u deze toewijzingen overschrijven met behulp van de vervolgkeuzelijsten de kolommen van één voor één toewijzen. Het patroon wordt nauwkeuriger als u meer kolommen toewijzen. De Wizard kopiëren voortdurend bijgewerkt het patroon en bereikt uiteindelijk de juiste patroon voor de kolomtoewijzing die u wilt bereiken.     

![Schematoewijzing](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Filteren van gegevens
U kunt filteren brongegevens om alleen de gegevens selecteren die moet worden gekopieerd naar het gegevensarchief sink. Filteren vermindert het volume van de gegevens worden gekopieerd naar het gegevensarchief sink en daarom verhoogt de doorvoer van de kopieerbewerking. Dit biedt een flexibele manier om gegevens te filteren in een relationele database met behulp van de SQL-querytaal of bestanden in een Azure blob-map met behulp van [Data Factory-functies en variabelen](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Filteren van gegevens in een database
De volgende schermafbeelding ziet u een SQL-query met de `Text.Format` functie en `WindowStart` variabele.

![Expressies valideren](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Filteren van gegevens in een Azure blob-map
U kunt variabelen gebruiken in het pad van de gegevens gekopieerd van een map die is bepaald tijdens runtime op basis van [systeemvariabelen](data-factory-functions-variables.md#data-factory-system-variables). De variabelen die ondersteund zijn: **{year}**, **{month}**, **{day}**, **{uur}**, **{minuut}**, en  **{{aangepaste}** . Bijvoorbeeld: inputfolder / {year} / {month} / {day}.

Stel dat ingevoerde mappen in de volgende indeling hebben:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Klik op de **Bladeren** knop voor **bestand of map**, blader naar een van deze mappen (bijvoorbeeld: 2016 -> 03 -> 01-02 >), en klik op **Kies**. U ziet `2016/03/01/02` in het tekstvak. Vervang nu, **2016** met **{year}**, **03** met **{month}**, **01** met **{day}** , en **02** met **{uur}**, en druk op de **tabblad** sleutel. Hier ziet u vervolgkeuzelijsten om de indeling voor deze vier variabelen te selecteren:

![Met behulp van systeemvariabelen](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

In de volgende schermafbeelding ziet u kunt ook een **aangepaste** variabele en eventuele [indeling tekenreeksen ondersteund](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Selecteer een map met die structuur, gebruiken de **Bladeren** eerst knop. Vervangt een waarde met **{aangepaste}**, en druk op de **tabblad** sleutel voor een overzicht van het tekstvak typt u de opmaakreeks op.     

![Met behulp van aangepaste variabele](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="scheduling-options"></a>Opties voor het plannen
U kunt de kopieerbewerking uitvoeren nadat de of volgens een schema (elk uur, dagelijks, enzovoort). Beide opties kunnen worden gebruikt voor de breedte van de verbindingslijnen tussen omgevingen, met inbegrip van on-premises, cloud en lokale bureaublad kopie.

Een eenmalige kopieerbewerking kan slechts één keer verplaatsing van gegevens van een bron naar doel. Dit geldt voor gegevens van elke grootte en elke ondersteunde indeling. De geplande kopie kunt u kopiëren van gegevens op een voorgeschreven terugkeerpatroon. U kunt uitgebreide instellingen (zoals opnieuw proberen, time-out en waarschuwingen) gebruiken voor het configureren van de geplande kopie.

![Eigenschappen plannen](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor een snel overzicht van de Data Factory-Wizard kopiëren via een pijplijn maken met de Kopieeractiviteit [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md).
