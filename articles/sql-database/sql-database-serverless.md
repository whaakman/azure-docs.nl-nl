---
title: Azure SQL-Database zonder server (preview) | Microsoft Docs
description: Dit artikel worden de nieuwe laag voor serverless Computing en vergelijkt deze met de bestaande ingerichte Computing-laag
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 07/05/2019
ms.openlocfilehash: 5a1a5ea39c9c0ed8973e1ecfa46977d2d06f83e7
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603615"
---
# <a name="azure-sql-database-serverless-preview"></a>Azure SQL-Database zonder server (preview)

Azure SQL Database zonder server (preview) is een compute-laag voor individuele databases die automatisch wordt geschaald op basis van vraag van de werkbelasting en facturen uit voor de hoeveelheid rekenkracht compute gebruikt per seconde. De serverloze compute-laag wordt ook automatisch databases onderbroken tijdens inactieve perioden wanneer alleen opslag wordt in rekening gebracht en databases automatisch hervat wanneer activiteit retourneert.

## <a name="serverless-compute-tier"></a>Serverloze compute-laag

De serverloze compute-laag voor één database is door een bereik compute-automatisch schalen en een vertraging autopause parameters.  De configuratie van deze parameters vorm van de database-prestaties en kosten berekenen.

![serverloze facturering](./media/sql-database-serverless/serverless-billing.png)

### <a name="performance-configuration"></a>Prestaties configureren

- De **minimale vCores** en **maximale vCores** configureerbare parameters die het bereik van de rekencapaciteit die beschikbaar zijn voor de database te definiëren. Geheugen en i/o-limieten zijn in verhouding met het vCore-bereik dat is opgegeven.  
- De **autopause vertraging** is een configureerbare parameter die wordt gedefinieerd welke periode de database inactief moet zijn voordat deze automatisch is onderbroken. De database wordt automatisch hervat wanneer de volgende aanmelding of andere activiteit plaatsvindt.  U kunt ook kan autopausing worden uitgeschakeld.

### <a name="cost"></a>Kosten

- De kosten voor een database zonder servers is de som van de compute-kosten en de kosten voor gegevensopslag.
- Wanneer compute-gebruik tussen de minimale en maximale limieten die zijn geconfigureerd is, wordt de compute-kosten zijn gebaseerd op vCore en geheugen dat wordt gebruikt.
- Wanneer compute-gebruik lager dan de min-limieten geconfigureerd is, worden de kosten is gebaseerd op de min-vCores en de minimale geheugen dat is geconfigureerd.
- Wanneer de database is onderbroken, wordt de rekenkosten gelijk is aan nul en alleen de opslagkosten worden in rekening gebracht.
- De opslagkosten voor wordt bepaald op dezelfde manier als in de ingerichte Computing-laag.

Zie voor meer informatie voor kosten, [facturering](sql-database-serverless.md#billing).

## <a name="scenarios"></a>Scenario's

Zonder server is geoptimaliseerd voor individuele databases met onregelmatige, onvoorspelbare gebruikspatronen waarvoor het niet erg enige vertraging in compute warmen na gebruik van niet-actieve perioden van prijs-prestatieverhouding. Daarentegen is de ingerichte Computing-laag prijs-prestatieverhouding geoptimaliseerd voor individuele databases of meerdere databases in elastische pools met hogere Gemiddeld gebruik dat geen enkele eventuele vertragingen in compute warmen.

### <a name="scenarios-well-suited-for-serverless-compute"></a>Scenario's die zeer geschikt voor serverless Computing

- Individuele databases met onregelmatige, onvoorspelbare gebruikspatronen afgewisseld met perioden van inactiviteit en lagere gemiddelde compute-gebruik na verloop van tijd.
- Individuele databases in de ingerichte Computing-laag die zijn vaak schaal gebracht weglating en klanten die liever delegeren compute schaling aanpassen aan de service.
- Nieuwe individuele databases zonder gebruiksgeschiedenis waar compute-grootte is moeilijk of onmogelijk is om in te schatten voorafgaand aan de implementatie in SQL-Database.

### <a name="scenarios-well-suited-for-provisioned-compute"></a>Scenario's die zeer geschikt voor de ingerichte Computing

- Individuele databases met meer reguliere, voorspelbare gebruikspatronen en hogere gemiddelde compute-gebruik na verloop van tijd.
- Databases die kunnen tolereren prestaties wisselwerking resultaat zijn van meer geheugen bijsnijden frequente of vertraging in autoresuming van de status onderbroken ingeschakeld.
- Meerdere databases met onregelmatige, onvoorspelbare gebruikspatronen die kunnen worden geconsolideerd in elastische pools voor betere prijs-prestatieverhouding optimalisatie.

## <a name="comparison-with-provisioned-compute-tier"></a>Vergelijking met ingerichte Computing-laag

De volgende tabel geeft een overzicht van de verschillen tussen de serverloze compute-laag en de ingerichte Computing-laag:

| | **Serverless Computing** | **Ingerichte Computing** |
|:---|:---|:---|
|**Gebruikspatroon van de database**| Onregelmatige, onvoorspelbare gebruik met lagere gemiddelde compute-gebruik na verloop van tijd. |  Meer reguliere gebruikspatronen met hogere gemiddelde compute-gebruik gedurende de tijd of meerdere databases met behulp van elastische pools.|
| **Prestaties management inspanning** |Lagere|Hogere|
|**COMPUTE schalen**|Automatisch|Handmatig|
|**COMPUTE-reactietijd**|Lagere na inactieve perioden|Direct|
|**Facturering granulariteit**|Per seconde|Per uur|

## <a name="purchasing-model-and-service-tier"></a>Model en de servicelaag aanschaffen

SQL-Database zonder server is momenteel alleen ondersteund in de categorie Algemeen gebruik op 5 generatie hardware in de vCore model kopen.

## <a name="autoscaling"></a>Automatisch schalen

### <a name="scaling-responsiveness"></a>Reactiesnelheid schalen

In het algemeen worden serverloze databases uitgevoerd op een computer met onvoldoende capaciteit om te voldoen aan de vraag naar resources zonder onderbreking voor elke hoeveelheid compute aangevraagd binnen de grenzen van de maximale vCores-waarde. Taakverdeling automatisch gebeurt af en toe als de computer kan niet voldoen aan de vraag naar resources binnen een paar minuten. Bijvoorbeeld, als de vraag naar resources 4 vCores is, maar alleen 2 vCores beschikbaar zijn, kan klikt u vervolgens het duren met een paar minuten om taken te verdelen voordat 4 vCores zijn opgegeven. De database blijft online tijdens de taakverdeling, met uitzondering van een korte periode aan het einde van de bewerking wanneer verbindingen worden verwijderd.

### <a name="memory-management"></a>Geheugenbeheer van

Geheugen voor serverloze databases wordt vrijgemaakt meer vaak dan voor de ingerichte Computing-databases. Dit gedrag is belangrijk om te beheersen in zonder server en de prestaties kan beïnvloeden.

#### <a name="cache-reclamation"></a>Cache vrijmaken

In tegenstelling tot databases, ingerichte Computing en geheugen van de SQL-cache van een database zonder servers vrijgemaakt, wanneer het CPU- of cache gebruik laag is.

- Gebruik van de cache wordt beschouwd als laag wanneer de totale grootte van de meest cache vermeldingen valt onder de drempelwaarde voor een bepaalde periode recent.
- Wanneer het vrijmaken van de cache wordt geactiveerd, de grootte van de doel-cache is incrementeel verkort tot een fractie van de vorige grootte en vrijmaken alleen als u gebruik laag blijft blijft.
- Wanneer er cache vrijmaken optreedt, is het beleid voor het selecteren van vermeldingen in de cache onbeschikbaar maken hetzelfde beleid selecteren als die voor de ingerichte Computing databases als geheugendruk hoog is.
- De cachegrootte is nooit verkleind onder de min-geheugenlimiet zoals gedefinieerd door min vCores die kan worden geconfigureerd.

Compute-databases, cache vermeldingen kunnen worden verwijderd als alle beschikbare geheugen wordt gebruikt in zowel zonder server en is ingericht.

#### <a name="cache-hydration"></a>Cache-hydratie

De SQL-cache neemt toe naarmate er gegevens worden opgehaald van de schijf op dezelfde manier en met dezelfde snelheid als voor de ingerichte databases. Als de database bezet is, wordt de cache te laten groeien onbeperkte maximaal de maximale geheugenlimiet toegestaan.

## <a name="autopausing-and-autoresuming"></a>Autopausing en autoresuming

### <a name="autopausing"></a>Autopausing

Autopausing wordt geactiveerd als alle volgende voorwaarden waar voor de duur van de vertraging autopause zijn:

- Aantal sessies = 0
- CPU = 0 voor de werkbelasting van de gebruiker die wordt uitgevoerd in de groep gebruikers

Een optie wordt geboden om uit te schakelen autopausing indien gewenst.

De volgende functies bieden geen ondersteuning voor autopausing.  Dat wil zeggen, als een van de volgende functies worden gebruikt, blijft klikt u vervolgens de database online, ongeacht de duur van inactiviteit van de database:

- Geo-replicatie (actieve geo-replicatie en automatische failover-groepen).
- Langetermijnretentie (LTR).
- De synchronisatiedatabase die wordt gebruikt in SQL data sync.

Autopausing wordt tijdelijk voorkomen tijdens de implementatie van een service-updates waarvoor dat de database online zijn.  In dergelijke gevallen wordt autopausing toegestaan opnieuw zodra de service-update is voltooid.

### <a name="autoresuming"></a>Autoresuming

Autoresuming wordt geactiveerd als een van de volgende voorwaarden voldaan op elk gewenst moment wordt:

|Functie|Autoresume trigger|
|---|---|
|Verificatie en autorisatie|Aanmelden|
|Detectie van bedreigingen|Threat detection-instellingen op het niveau van de database of server inschakelen/uitschakelen.<br>Wijzigen van instellingen voor bedreigingsdetectie op het niveau van de database of server.|
|Gegevensdetectie en -classificatie|Toevoegen, wijzigen, verwijderen of de gevoeligheidslabels weergeven|
|Controleren|Controle records weergeven.<br>Bij te werken of weergeven van controlebeleid.|
|Gegevensmaskering|Toevoegen, wijzigen, verwijderen of gegevensmaskering regels weergeven|
|Transparent Data Encryption|Status weergeven of de status van transparante gegevensversleuteling|
|Gegevensarchief query (prestaties)|Wijzigen of weergeven van instellingen voor query store; automatisch afstemmen|
|Autotuning|Toepassings- en controle van autotuning aanbevelingen, zoals automatisch indexeren|
|Database kopiëren|Database maken als kopie.<br>Exporteren naar een BACPAC-bestand.|
|SQL data sync|Synchronisatie tussen hub en lid van de databases die worden uitgevoerd op een configureerbaar schema of handmatig worden uitgevoerd|
|Metagegevens van een bepaalde database wijzigen|Nieuwe database-labels toe te voegen.<br>Maximale vCores, min vCores of autopause vertraging wijzigen.|
|SQL Server Management Studio (SSMS)|Met behulp van SSMS versie 18 en openen van een nieuw queryvenster voor elke database op de server wordt elke database automatisch onderbroken op dezelfde server hervat. Dit probleem treedt niet als met behulp van SSMS versie 17.9.1 met IntelliSense uitgeschakelde.|

Autoresuming wordt ook tijdens de implementatie van een service-updates waarvoor dat de database online worden geactiveerd.

### <a name="connectivity"></a>Connectiviteit

Als een database zonder servers wordt onderbroken, wordt de eerste aanmelding de hervatten van de database en een foutmelding waarin staat dat de database niet beschikbaar met foutcode 40613 is retourneren. Zodra de database wordt hervat, moet de aanmelding opnieuw worden uitgevoerd voor het maken van verbinding. Database-clients met de logica voor opnieuw proberen verbinding hoeft niet te worden gewijzigd.

### <a name="latency"></a>Latentie

De latentie autoresume en autopause een database zonder servers is doorgaans om van 1 minuut autoresume en 1-10 minuten autopause.

## <a name="onboarding-into-serverless-compute-tier"></a>Onboarding naar serverloze compute-laag

Het maken van een nieuwe database of het verplaatsen van dat een bestaande database in een serverloze compute-laag volgt hetzelfde patroon als het maken van een nieuwe database in compute-laag ingericht en omvat de volgende twee stappen.

1. Geef de naam van de serviceniveaudoelstelling. De servicedoelstelling schrijft de servicelaag, hardware-generatie en maximale vCores. De volgende tabel ziet u de service objectieve opties:

   |Naam van serviceniveaudoelstelling service|Servicelaag|Hardware-generatie|Maximum aantal vCores|
   |---|---|---|---|
   |GP_S_Gen5_1|Algemeen doel|Gen5|1|
   |GP_S_Gen5_2|Algemeen doel|Gen5|2|
   |GP_S_Gen5_4|Algemeen doel|Gen5|4|

2. Geef desgewenst de min-vCores en autopause vertraging als u wilt wijzigen, hun standaardwaarden. De volgende tabel toont de beschikbare waarden voor deze parameters.

   |Parameter|Opties voor waarde|Standaardwaarde|
   |---|---|---|---|
   |Min vCores|Een van de {0,5, 1, 2, 4} niet meer dan max vCores|0,5 vCores|
   |Autopause vertraging|Minimum: 60 minuten (1 uur)<br>Maximum aantal: 10080 minuten (7 dagen)<br>Stappen: 60 minuten<br>Autopause uitschakelen: -1|60 minuten|

> [!NOTE]
> Met behulp van T-SQL te verplaatsen van een bestaande database naar serverloze of te wijzigen van de compute-grootte wordt momenteel niet ondersteund, maar kan worden gedaan via de Azure portal of PowerShell.

### <a name="create-new-database-in-serverless-compute-tier"></a>Nieuwe database maken in een serverloze compute-laag 

#### <a name="use-azure-portal"></a>Azure Portal gebruiken

Zie [Quickstart: Een individuele database maken in Azure SQL Database met behulp van de Azure-portal](sql-database-single-database-get-started.md).

#### <a name="use-powershell"></a>PowerShell gebruiken

Het volgende voorbeeld wordt een nieuwe database in de serverloze compute-laag.  In dit voorbeeld geeft expliciet de vCores min, max vCores en autopause vertraging.

```powershell
New-AzSqlDatabase `
  -ResourceGroupName $resourceGroupName `
  -ServerName $serverName `
  -DatabaseName $databaseName `
  -ComputeModel Serverless `
  -Edition GeneralPurpose `
  -ComputeGeneration Gen5 `
  -MinVcore 0.5 `
  -MaxVcore 2 `
  -AutoPauseDelayInMinutes 720
```

### <a name="move-database-from-provisioned-compute-tier-into-serverless-compute-tier"></a>Database overstappen van ingerichte Computing in serverloze compute-laag

#### <a name="use-powershell"></a>PowerShell gebruiken

Het volgende voorbeeld wordt een database van de ingerichte Computing-laag verplaatst naar de serverloze compute-laag. In dit voorbeeld geeft expliciet de vCores min, max vCores en autopause vertraging.

```powershell
Set-AzSqlDatabase
  -ResourceGroupName $resourceGroupName `
  -ServerName $serverName `
  -DatabaseName $databaseName `
  -Edition GeneralPurpose `
  -ComputeModel Serverless `
  -ComputeGeneration Gen5 `
  -MinVcore 1 `
  -MaxVcore 4 `
  -AutoPauseDelayInMinutes 1440
```

### <a name="move-database-from-serverless-compute-tier-into-provisioned-compute-tier"></a>Database verplaatsen van serverloze compute-laag naar ingerichte Computing-laag

Een database zonder servers kan worden verplaatst naar een laag ingerichte Computing op dezelfde manier als het verplaatsen van een database van de ingerichte Computing in een serverloze compute-laag.

## <a name="modifying-serverless-configuration"></a>Serverloze-configuratie wijzigen

### <a name="maximum-vcores"></a>Maximum vCores

#### <a name="use-powershell"></a>PowerShell gebruiken

Wijzigen van de maximale vCores is uitgevoerd met behulp van de [Set AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) opdracht in PowerShell met behulp van de `MaxVcore` argument.

### <a name="minimum-vcores"></a>Minimum vCores

#### <a name="use-powershell"></a>PowerShell gebruiken

Wijzigen van de min-vCores wordt uitgevoerd met behulp van de [Set AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) opdracht in PowerShell met behulp van de `MinVcore` argument.

### <a name="autopause-delay"></a>Autopause vertraging

#### <a name="use-powershell"></a>PowerShell gebruiken

Wijzigen van de vertraging autopause wordt uitgevoerd met behulp van de [Set AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase) opdracht in PowerShell met behulp van de `AutoPauseDelayInMinutes` argument.

## <a name="monitoring"></a>Bewaking

### <a name="resources-used-and-billed"></a>Resources die worden gebruikt en in rekening gebracht

De resources van een database zonder servers worden door het app-pakket, SQL-exemplaar en gebruikersentiteiten resource pool ingekapseld.

#### <a name="app-package"></a>App-pakket

Het app-pakket is met de buitenste meeste resource management grens voor een database, ongeacht of de database zich in een serverloze of ingerichte compute-laag. Het app-pakket bevat de SQL-exemplaar en externe services die samen scope alle gebruikers- en resources die worden gebruikt door een database in SQL-Database. Voorbeelden van externe services zijn de R- en zoeken in volledige tekst. Het SQL-exemplaar bepaalt het totale Resourcegebruik in het algemeen via het app-pakket.

#### <a name="user-resource-pool"></a>Resourcegroep van de gebruiker

De resourcegroep van de gebruiker de binnenste meeste resource management-grens voor een database, ongeacht of de database zich in een serverloze of ingerichte compute-laag is. De gebruiker resource pool-bereiken CPU- en i/o-voor de werkbelasting van de gebruiker, zoals die worden gegenereerd door een DDL-query's, zoals het maken en wijzigen en DML-query's selecteren, invoegen, bijwerken en verwijderen. Deze query's vertegenwoordigen meestal de meest aanzienlijk deel van het gebruik binnen het app-pakket.

### <a name="metrics"></a>Metrische gegevens

Metrische gegevens voor het bewaken van het Resourcegebruik van de app-pakket en de gebruiker de groep van een database zonder servers worden vermeld in de volgende tabel:

|Entiteit|Gegevens|Description|Eenheden|
|---|---|---|---|
|App-pakket|app_cpu_percent|Percentage van de vCores die worden gebruikt door de app ten opzichte van maximale vCores toegestaan voor de app.|Percentage|
|App-pakket|app_cpu_billed|De hoeveelheid rekenkracht in rekening gebracht voor de app tijdens de rapportageperiode. Het aankoopbedrag tijdens deze periode is het product van deze metrische gegevens en de vCore-prijs per eenheid. <br><br>Waarden van deze metrische gegevens worden bepaald door het samenvoegen van na verloop van tijd gebruikt voor het maximum van CPU en geheugen gebruikt per seconde. Als het aantal gebruikte kleiner dan het minimum aan nodige ingericht zoals ingesteld door de min-vCores en min-geheugen, is en vervolgens het minimum aan nodige ingericht wordt in rekening gebracht. Als u wilt vergelijken CPU met geheugen voor factureringsdoeleinden, geheugen genormaliseerd in eenheden van vCores door schaling aanpassen van de hoeveelheid geheugen in GB met 3 GB per vCore.|vCore seconden|
|App-pakket|app_memory_percent|Hoeveelheid geheugen die wordt gebruikt door de app ten opzichte van maximale geheugen die is toegestaan voor de app.|Percentage|
|Gebruiker van toepassingen|cpu_percent|Percentage van de vCores die worden gebruikt door de werkbelasting voor gebruikers ten opzichte van maximale vCores toegestaan voor de werkbelasting van de gebruiker.|Percentage|
|Gebruiker van toepassingen|data_IO_percent|Percentage van de gegevens die worden gebruikt door de werkbelasting voor gebruikers ten opzichte van de gegevens IOPS van de maximale IOPS toegestaan voor de werkbelasting van de gebruiker.|Percentage|
|Gebruiker van toepassingen|log_IO_percent|Percentage van logboek MB/s die worden gebruikt door de werkbelasting voor gebruikers ten opzichte van maximale log MB/s is toegestaan voor de werkbelasting van de gebruiker.|Percentage|
|Gebruiker van toepassingen|workers_percent|Percentage van de werknemers die worden gebruikt door de werkbelasting voor gebruikers ten opzichte van maximale werknemers die zijn toegestaan voor de werkbelasting van de gebruiker.|Percentage|
|Gebruiker van toepassingen|sessions_percent|Percentage van de sessies die worden gebruikt door de werkbelasting voor gebruikers ten opzichte van het maximumaantal sessies toegestaan voor de werkbelasting van de gebruiker.|Percentage|

### <a name="pause-and-resume-status"></a>Onderbreken en hervatten van status

In de Azure-portal, wordt de status van de database weergegeven in het overzichtsvenster van de server met een lijst met de databases die deze bevat. De status van de database wordt ook weergegeven in het overzichtsvenster voor de database.

Query uitvoeren op het onderbreken en hervatten van de status van een database via de volgende PowerShell-opdracht:

```powershell
Get-AzSqlDatabase `
  -ResourceGroupName $resourcegroupname `
  -ServerName $servername `
  -DatabaseName $databasename `
  | Select -ExpandProperty "Status"
```

## <a name="resource-limits"></a>Bronlimieten

Zie voor de resourcelimieten, [Serverloze compute-laag](sql-database-vCore-resource-limits-single-databases.md#serverless-compute-tier)

## <a name="billing"></a>Billing

De hoeveelheid rekenkracht in rekening gebracht, is het maximum van CPU-gebruik en het geheugen gebruikt per seconde. Als de hoeveelheid CPU gebruikt en gebruikt geheugen kleiner dan het minimum aan nodige ingericht voor elk is, klikt u vervolgens het ingerichte bedrag wordt in rekening gebracht. Als u wilt vergelijken CPU met geheugen voor factureringsdoeleinden, geheugen genormaliseerd in eenheden van vCores door schaling aanpassen van de hoeveelheid geheugen in GB met 3 GB per vCore.

- **Resource in rekening gebracht**: CPU en geheugen
- **Gefactureerd bedrag**: eenheidsprijs vCore * max (min vCores, vCores gebruikt, min memory GB * geheugen van 1/3 GB gebruikt * 1/3) 
- **Factureringsfrequentie**: Per seconde

De prijs van de eenheid vCore in de kosten per vCore per seconde. Raadpleeg de [pagina met prijzen van Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/single/) voor specifieke eenheidsprijzen in een bepaalde regio.

De hoeveelheid rekenkracht in rekening gebracht, wordt weergegeven met de volgende metrische gegevens:

- **Metrische gegevens**: app_cpu_billed (vCore seconden)
- **Definitie**: max (min vCores, vCores gebruikt, min memory GB * geheugen van 1/3 GB gebruikt * 1/3)
- **Frequentie Reporting**: Per minuut

Deze hoeveelheid wordt berekend per seconde en meer dan 1 minuut geaggregeerd.

Houd rekening met een serverloze database die is geconfigureerd met 1 minuut vCore en 4 maximale vCores.  Dit komt overeen met ongeveer 3 GB min geheugen en het maximale geheugen van 12 GB.  Stel de vertraging automatisch onderbreken is ingesteld op 6 uur en de werkbelasting van de database actief is gedurende de eerste 2 uur van een periode van 24 uur en anders niet actief.    

In dit geval wordt de database wordt in rekening gebracht voor rekenen en opslag in de eerste 8 uur.  Hoewel de database niet actief gestart na het tweede uur is, wordt dit nog steeds gefactureerd voor Computing in de volgende zes uur op basis van de minimale compute terwijl de database online is ingericht.  Alleen in rekening gebracht tijdens de rest van de periode van 24 uur tijdens de database is onderbroken.

Preciezer nog, wordt de compute-factuur in dit voorbeeld als volgt berekend:

|Tijdsinterval|vCores gebruikt per seconde|GB per seconde gebruikt|COMPUTE-dimensie kosten in rekening gebracht|vCore seconden kosten in rekening gebracht via tijdsinterval|
|---|---|---|---|---|
|0:00-1:00|4|9|vCores gebruikt|4 vCores * 3600 seconden = 14400 vCore seconden|
|1:00-2:00|1|12|Gebruikt geheugen|12 GB * 1/3 * 3600 seconden = 14400 vCore-seconden|
|2:00-8:00|0|0|Min memory ingericht|3 GB * 1/3 * 21600 seconden = 21600 vCore-seconden|
|8:00-24:00|0|0|Er zijn geen kosten in rekening gebracht terwijl onderbroken compute|seconden van 0 vCore|
|Totaal aantal vCore seconden meer dan 24 uur in rekening gebracht||||50400 vCore seconden|

Stel de compute-eenheidsprijs $0.000073/vCore/second is.  Klik aan de rekenresources in rekening gebracht voor deze periode van 24 uur het product van de compute-eenheid prijs- en vCore seconden kosten in rekening gebracht is: $0.000073/vCore/second * 50400 vCore seconden = $3,68

## <a name="available-regions"></a>Beschikbare regio's

De serverloze compute-laag is wereldwijd beschikbaar, met uitzondering van de volgende regio's: Australië-centraal, China-Oost, China-Noord, Frankrijk-Zuid, Duitsland-centraal, Duitsland-Noordoost, West-India, Zuid-Korea, Zuid-Afrika West, UK-Noord, UK-Zuid, UK-West en West-Centraal VS.

## <a name="next-steps"></a>Volgende stappen

- Als u wilt beginnen, Zie [Quick Start: Een individuele database maken in Azure SQL Database met behulp van de Azure-portal](sql-database-single-database-get-started.md).
- Zie voor de resourcelimieten, [zonder compute resource categorielimieten](sql-database-vCore-resource-limits-single-databases.md#serverless-compute-tier).
