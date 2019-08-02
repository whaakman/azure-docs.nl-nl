---
title: Een bron transformatie instellen in de functie gegevens stroom toewijzen van Azure Data Factory
description: Meer informatie over het instellen van een bron transformatie in het toewijzen van gegevens stroom.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 974ece9cd035ae29ada38f34b7933d86f682194f
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68696236"
---
# <a name="source-transformation-for-mapping-data-flow"></a>Bron transformatie voor het toewijzen van gegevens stroom 

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Met een bron transformatie configureert u de gegevens bron voor de gegevens stroom. Een gegevens stroom kan meer dan één bron transformatie bevatten. Bij het ontwerpen van gegevens stromen begint altijd met een bron transformatie.

Voor elke gegevens stroom is ten minste één bron transformatie vereist. Voeg zoveel bronnen toe als nodig is om uw gegevens transformaties te volt ooien. U kunt deze bronnen samen voegen met een koppelings transformatie of een Union-trans formatie.

> [!NOTE]
> Wanneer u fouten opspoort in uw gegevens stroom, worden gegevens uit de bron gelezen met behulp van de bemonsterings instelling of de bron limieten voor fout opsporing. Als u gegevens naar een Sink wilt schrijven, moet u uw gegevens stroom uitvoeren vanuit een gegevens stroom activiteit van een pijp lijn. 

![Opties voor bron transformatie op het tabblad Bron instellingen](media/data-flow/source.png "bron")

Koppel uw gegevens stroom bron transformatie met precies één Data Factory gegevensset. De gegevensset definieert de vorm en locatie van de gegevens waarnaar u wilt schrijven of waaruit u wilt lezen. U kunt joker tekens en bestands lijsten in uw bron gebruiken om met meer dan één bestand tegelijk te werken.

Met behulp van een **Joker patroon** optie geeft u aan dat elke overeenkomende map en elk bestand in één bron transformatie moet worden herhaald. Dit is een zeer efficiënte manier om meerdere bestanden binnen één stroom te verwerken. Als u de bestands naam wilt bijhouden die momenteel wordt verwerkt, stelt u een veld naam in voor het veld "kolom voor het opslaan van de bestands naam" in de bron opties.

> [!NOTE]
> Stel meerdere Joker teken patronen in met het plus teken naast uw bestaande Joker teken om meer Joker regels toe te voegen.

## <a name="data-flow-staging-areas"></a>Faserings gebieden van gegevens stroom
De gegevens stroom werkt met faserings gegevens sets die allemaal in azure zijn. Gebruik deze gegevens sets voor fase ring wanneer u uw gegevens transformeert. 

Data Factory heeft toegang tot bijna 80 systeem eigen connectors. Als u gegevens uit deze andere bronnen in uw gegevens stroom wilt toevoegen, gebruikt u het hulp programma voor het kopiëren van de activiteit om de gegevens in een van de faserings ruimte van de gegevensstroom gegevensset te stageren.

## <a name="options"></a>Opties

Kies schema-en bemonsterings opties voor uw gegevens.

### <a name="schema-drift"></a>Schema-afwijking
[Schema-drift](concepts-data-flow-schema-drift.md) is de mogelijkheid van ADF om systeem eigen flexibele schema's in uw gegevens stromen af te handelen zonder dat er expliciet kolom wijzigingen hoeven te worden gedefinieerd.

* Selecteer **schema-drift toestaan** als de bron kolommen vaak worden gewijzigd. Met deze instelling kunnen alle binnenkomende bron velden door de trans formaties worden getransporteerd naar de sink.

* Bij het kiezen van een afleidende **kolom typen** wordt ADF weer gegeven om gegevens typen te definiëren voor elke nieuwe kolom die wordt gedetecteerd. Als deze functie is uitgeschakeld, neemt ADF een teken reeks.

### <a name="validate-schema"></a>Schema valideren

Als de inkomende versie van de bron gegevens niet overeenkomt met het gedefinieerde schema, kan de gegevens stroom niet worden uitgevoerd.

![Instellingen voor open bare bron, met de opties voor schema valideren, schema-drift toestaan en steek proeven](media/data-flow/source1.png "open bare bron 1")

### <a name="sample-the-data"></a>Voor beeld van de gegevens
Schakel **steek proeven** in om het aantal rijen van de bron te beperken. Gebruik deze instelling wanneer u gegevens uit uw bron test of van uw source gebruikt voor fout opsporing.

## <a name="define-schema"></a>Schema definiëren

Als uw bron bestanden niet sterk worden getypt (bijvoorbeeld platte bestanden in plaats van Parquet-bestanden), definieert u de gegevens typen voor elk veld in de bron transformatie.  

![Instellingen voor bron transformatie op het tabblad schema definiëren](media/data-flow/source2.png "bron 2")

U kunt later de kolom namen wijzigen in een trans formatie selecteren. Gebruik een trans formatie met afgeleide kolommen om de gegevens typen te wijzigen. Voor sterk getypeerde bronnen kunt u de gegevens typen wijzigen in een latere trans formatie selecteren. 

![Gegevens typen in een geselecteerde trans formatie](media/data-flow/source003.png "gegevens typen")

### <a name="optimize-the-source-transformation"></a>De bron transformatie optimaliseren

Op het tabblad **optimaliseren** voor de bron transformatie ziet u mogelijk een type **bron** partitie. Deze optie is alleen beschikbaar als uw bron Azure SQL Database is. Dit komt omdat Data Factory verbinding probeert te maken met het parallel uitvoeren van grote query's op uw SQL Database bron.

![Instellingen van de bron partitie](media/data-flow/sourcepart3.png "partitioneren")

U hoeft geen gegevens op uw SQL Database bron te partitioneren, maar partities zijn handig voor grote query's. U kunt de partitie baseren op een kolom of een query.

### <a name="use-a-column-to-partition-data"></a>Een kolom gebruiken om gegevens te partitioneren

Selecteer in de bron tabel een kolom waarop moet worden gepartitioneerd. Stel ook het aantal partities in.

### <a name="use-a-query-to-partition-data"></a>Een query gebruiken om gegevens te partitioneren

U kunt ervoor kiezen om de verbindingen te partitioneren op basis van een query. Voer eenvoudigweg de inhoud van een WHERE-predikaat in. Voer bijvoorbeeld jaar > 1980 in.

## <a name="source-file-management"></a>Bron bestands beheer

Kies instellingen om bestanden in uw bron te beheren. 

![Nieuwe bron instellingen](media/data-flow/source2.png "Nieuwe instellingen")

* **Pad met Joker tekens**: Kies in uw bron container een reeks bestanden die overeenkomen met een patroon. Deze instelling overschrijft elk bestand in de definitie van de gegevensset.

Voor beelden van joker tekens:

* ```*```Vertegenwoordigt een wille keurige set tekens
* ```**```Recursief nesten van mappen
* ```?```Vervangt één teken
* ```[]```Komt overeen met een van de tekens in de vier Kante haken

* ```/data/sales/**/*.csv```Hiermee worden alle CSV-bestanden opgehaald onder/Data/Sales
* ```/data/sales/20??/**```Hiermee worden alle bestanden in de twintigste eeuw opgehaald
* ```/data/sales/2004/*/12/[XY]1?.csv```Hiermee worden alle CSV-bestanden in 2004 december opgehaald, beginnend met X of Y, voorafgegaan door een nummer van twee cijfers

Er moet een container worden opgegeven in de gegevensset. Het pad naar uw Joker teken moet daarom ook uw mappad van de hoofdmap bevatten.

* Basispad: Als u gepartitioneerde mappen hebt in de bestands bron van ```key=value``` een indeling (bijvoorbeeld Year = 2019), kunt u een ADF stellen om het hoogste niveau van die partitie mappen structuur toe te wijzen aan een kolom naam in de gegevens stroom van de gegevensstroom.

Stel eerst een Joker teken in om alle paden op te nemen van de gepartitioneerde mappen plus de blad bestanden die u wilt lezen.

![Bron Bestands instellingen] voor partitioneren (media/data-flow/partfile2.png "Instelling voor partitie bestand")

Gebruik nu de instelling basis pad van partitie om ADF te laten zien wat het hoogste niveau van de mappen structuur is. Wanneer u nu de inhoud van uw gegevens bekijkt, ziet u dat de opgeloste partities in elk van uw mapniveau worden toegevoegd.

Basispad voor ![partitie](media/data-flow/partfile1.png "Voor beeld van basispad")

* **Lijst met bestanden**: Dit is een bestandenset. Maak een tekst bestand met een lijst met relatieve padgegevens die moeten worden verwerkt. Wijs dit tekst bestand aan.
* **Kolom voor het opslaan van de bestands naam**: Sla de naam van het bron bestand op in een kolom in uw gegevens. Voer hier een nieuwe naam in om de teken reeks voor de bestands naam op te slaan.
* **Na voltooiing**: U kunt niets doen met het bron bestand nadat de gegevens stroom is uitgevoerd, het bron bestand te verwijderen of het bron bestand te verplaatsen. De paden voor de verplaatsing zijn relatief.

Als u bron bestanden naar een andere locatie wilt verplaatsen, selecteert u eerst verplaatsen voor bestands bewerking. Stel vervolgens de map uit. Als u geen joker tekens gebruikt voor uw pad, is de instelling van ' van ' dezelfde map als de bronmap.

Als u een bronpad met Joker teken hebt, ziet uw syntaxis er als volgt uit:

```/data/sales/20??/**/*.csv```

U kunt "van" opgeven als

```/data/sales```

En "aan" als

```/backup/priorSales```

In dit geval worden alle bestanden die zijn gebrond onder/Data/Sales verplaatst naar/backup/priorSales.

### <a name="sql-datasets"></a>SQL-gegevens sets

Als uw bron zich in SQL Database of SQL Data Warehouse bevindt, hebt u extra opties voor het beheer van bron bestanden.

* **Query**: Voer een SQL-query in voor de bron. Deze instelling overschrijft elke tabel die u in de gegevensset hebt gekozen. Houd er rekening mee dat **order by** -componenten hier niet worden ondersteund, maar u kunt een volledige Select from-instructie instellen. U kunt ook door de gebruiker gedefinieerde tabel functies gebruiken. **Select * from udfGetData ()** is een UDF in SQL die een tabel retourneert. Met deze query wordt een bron tabel geproduceerd die u in uw gegevens stroom kunt gebruiken.
* **Batch grootte**: Voer een batch grootte in om grote gegevens te segmenteren in Lees bewerkingen.
* **Isolatie niveau**: De standaard waarde voor SQL-bronnen in ADF-toewijzings gegevens stromen wordt niet-doorgevoerd gelezen. U kunt het isolatie niveau hier wijzigen in een van deze waarden:
* Doorgevoerde lezen
* Lezen niet-doorgevoerd
* Herhaal bare Lees bewerking
* Serializable
* Geen (isolatie niveau negeren)

![Isolatie niveau](media/data-flow/isolationlevel.png "Isolatie niveau")

> [!NOTE]
> Bestands bewerkingen worden alleen uitgevoerd wanneer u de gegevens stroom start vanuit een pijplijn uitvoering (een uitvoering van een pijp lijn of uitvoering) die gebruikmaakt van de activiteit gegevens stroom uitvoeren in een pijp lijn. Bestands bewerkingen *worden niet* uitgevoerd in de modus voor fout opsporing van gegevens stromen.

### <a name="projection"></a>Projectie

Net als schema's in gegevens sets definieert de projectie in een bron de gegevens kolommen, typen en indelingen van de bron gegevens. 

![Instellingen op het tabblad projectie](media/data-flow/source3.png "Projectie")

Als uw tekst bestand geen gedefinieerd schema heeft, selecteert u **gegevens type detecteren** zodat Data Factory de gegevens typen worden gesampled en afgeleid. Selecteer **standaard indeling instellen** op automatische detectie van de standaard gegevens indelingen. 

U kunt de kolom gegevens typen wijzigen in een latere afgeleide-kolom transformatie. Gebruik een SELECT trans formatie om de kolom namen te wijzigen.

![Instellingen voor standaard gegevens indelingen](media/data-flow/source2.png "Standaard indelingen")

### <a name="add-dynamic-content"></a>Dynamische inhoud toevoegen
Wanneer u in het deel venster instellingen op velden klikt, wordt er een Hyper link weer geven voor ' dynamische inhoud toevoegen '. Wanneer u selecteert om de opbouw functie voor expressies te starten, worden waarden dynamisch ingesteld met behulp van expressies, statische letterlijke waarden of para meters.

![Parameters](media/data-flow/params6.png "Parameters")

## <a name="next-steps"></a>Volgende stappen

Beginnen met het bouwen van een afgeleide [kolom transformatie](data-flow-derived-column.md) en een [geselecteerde trans formatie](data-flow-select.md).
