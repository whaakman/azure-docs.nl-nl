---
title: Kopieerwizard van Data Factory Azure | Microsoft Docs
description: Meer informatie over hoe u de Kopieerwizard van Data Factory Azure gebruikt om te kopiëren van gegevens uit ondersteunde gegevensbronnen naar sinks.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: ''
editor: ''
ms.assetid: 0974eb40-db98-4149-a50d-48db46817076
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: d13e304b0d10e8bd34d306426f1f9164bcc6be94
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54014247"
---
# <a name="azure-data-factory-copy-wizard"></a>Azure Data Factory-Wizard kopiëren
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. 

De Kopieerwizard van Azure Data Factory vereenvoudigt het proces voor het opnemen gegevens, die meestal een eerste stap in een scenario voor end-to-end data-integratie. Wanneer de Wizard kopiëren van Azure Data Factory te doorlopen, hoeft u niet om een JSON-definities voor de gekoppelde services, gegevenssets en pijplijnen te begrijpen. De wizard maakt automatisch een pijplijn om gegevens te kopiëren uit de geselecteerde gegevensbron op de geselecteerde bestemming. Bovendien helpt de Wizard kopiëren u om de gegevens die wordt opgenomen op het moment van ontwerpen te valideren. Dit bespaart tijd, met name wanneer u bent ophalen van gegevens voor de eerste keer uit de gegevensbron. De Wizard kopiëren starten, klikt u op de **gegevens kopiëren** tegel op de startpagina van uw data factory.

![De wizard Kopiëren](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="designed-for-big-data"></a>Ontworpen voor big data
Deze wizard kunt u eenvoudig om gegevens te verplaatsen van een groot aantal bronnen naar bestemmingen in minuten. Nadat u de wizard doorloopt, wordt een pijplijn met copy activity automatisch gemaakt voor u, samen met de afhankelijke Data Factory-entiteiten (gekoppelde services en gegevenssets). Er zijn geen extra stappen vereist om de pijplijn te maken.   

![Gegevensbron selecteren](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Voor stapsgewijze instructies voor het maken van een voorbeeldpijplijn voor het kopiëren van gegevens van een Azure-blob naar een Azure SQL Database-tabel, raadpleegt u de [Kopieerwizard zelfstudie](data-factory-copy-data-wizard-tutorial.md).
>
>

De wizard is ontworpen met big data in gedachten vanaf het begin, met ondersteuning voor diverse gegevens en objecttypen. U kunt Data Factory-pijplijnen die overschakelen van honderden mappen, bestanden of tabellen maken. De wizard ondersteunt automatische Gegevensvoorbeeld, schema vastleggen en -toewijzing en filteren van gegevens.

## <a name="automatic-data-preview"></a>Voorbeeld van automatische gegevens
Onderdeel van de gegevens van de geselecteerde gegevensbron kunt u bekijken om te valideren of de gegevens is wat u wilt kopiëren. Als de brongegevens bevinden zich in een tekstbestand, parseert de Wizard kopiëren bovendien het tekstbestand dat voor de rij- als scheidingstekens en het schema automatisch meer.

![Pagina Instellingen bestandsindelingen](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Vastleggen van de schema- en -toewijzing
Het schema van de ingevoerde gegevens mogelijk niet overeenkomt met het schema van de uitvoergegevens in sommige gevallen. In dit scenario moet u kolommen uit van het schema van de gegevensbron op kolommen uit het doelschema toewijzen.

> [!TIP]
> Bij het kopiëren van gegevens uit SQL Server of Azure SQL Database in Azure SQL Data Warehouse, als de tabel bestaat niet in het doelarchief, ondersteuning voor Data Factory automatisch tabel maken met behulp van het schema van de gegevensbron. Meer informatie uit [verplaatsen van gegevens en naar Azure SQL Data Warehouse met behulp van Azure Data Factory](./data-factory-azure-sql-data-warehouse-connector.md).
>

Gebruik een vervolgkeuzelijst met een kolom selecteren uit het schema van de gegevensbron toe te wijzen aan een kolom in het schema van de bestemming. De Wizard kopiëren probeert te begrijpen van het patroon voor kolomtoewijzing. Het geldt hetzelfde patroon voor de rest van de kolommen, zodat u niet wilt selecteert u elk van de kolommen afzonderlijk om uit te voeren van de schematoewijzing van het. Als u liever, kunt u deze toewijzingen overschrijven met behulp van de vervolgkeuzelijsten om toe te wijzen, één voor één van de kolommen. Het patroon wordt meer nauwkeurige als u meer kolommen toewijzen. De Wizard kopiëren voortdurend bijgewerkt het patroon en uiteindelijk de juiste patroon voor de kolomtoewijzing van de die u wilt bereiken bereikt.     

![Schematoewijzing](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Filteren van gegevens
U kunt filteren brongegevens om alleen de gegevens die moet worden gekopieerd naar het sink-gegevensopslag te selecteren. Filteren vermindert het volume van de gegevens moeten worden gekopieerd naar het sink-gegevensopslag en daarom verhoogt de doorvoer van de kopieerbewerking. Deze biedt een flexibele manier om gegevens te filteren in een relationele database met behulp van de SQL-query-taal, of in een map voor Azure blob-bestanden met behulp van [Data Factory-functies en variabelen](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Het filteren van gegevens in een database
De volgende schermafbeelding ziet u een SQL-query met de `Text.Format` functie en `WindowStart` variabele.

![Valideren van expressies](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Het filteren van gegevens in een Azure blob-map
U kunt variabelen gebruiken in het pad naar het kopiëren van gegevens uit een map die is bepaald tijdens uitvoering op basis van [systeemvariabelen](data-factory-functions-variables.md#data-factory-system-variables). De variabelen die ondersteund zijn: **{year}**, **{month}**, **{day}**, **{hour}**, **{minute}**, en **{aangepaste}**. Bijvoorbeeld: inputfolder / {year} / {month} / {day}.

Stel dat u hebt opgegeven dat mappen in de volgende indeling:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Klik op de **Bladeren** knop voor **bestand of map**, blader naar een van deze mappen (bijvoorbeeld: 2016-03 > -> 01-02 >), en klikt u op **kiezen**. U ziet `2016/03/01/02` in het tekstvak in. Vervang nu, **2016** met **{year}**, **03** met **{month}**, **01** met **{day}** , en **02** met **{hour}**, en druk op de **tabblad** sleutel. Hier ziet u vervolgkeuzelijsten om de indeling voor deze vier variabelen te selecteren:

![Met behulp van de systeemvariabelen](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Zoals weergegeven in de volgende schermafbeelding, u kunt ook een **aangepaste** variabele en een [opmaaktekenreeksen ondersteund](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Selecteer een map met die structuur, gebruikt u de **Bladeren** eerst knop. Vervang vervolgens een waarde met **{aangepaste}**, en druk op de **tabblad** sleutel om te zien van het tekstvak u de opmaaktekenreeks typt.     

![Met behulp van aangepaste variabele](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="scheduling-options"></a>Opties voor het plannen
U kunt de kopieerbewerking uitvoeren eenmalig of volgens een schema (per uur, dagelijks, enzovoort). Beide van deze opties kunnen worden gebruikt voor de breedte van de connectors voor datacenteromgevingen, met inbegrip van on-premises, cloud en lokale bureaublad kopie.

Een eenmalige kopieerbewerking kan slechts één keer verplaatsing van gegevens van een bron naar een bestemming. Dit geldt voor gegevens van elke grootte en elke ondersteunde indeling. De geplande kopie kunt u het kopiëren van gegevens op een voorgeschreven terugkeerpatroon. U kunt uitgebreide instellingen (zoals opnieuw proberen, time-out en waarschuwingen) gebruiken voor het configureren van de geplande kopie.

![Eigenschappen van planning](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor een snel overzicht van het gebruik van de Kopieerwizard van Data Factory te maken van een pijplijn met Copy Activity [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md).
