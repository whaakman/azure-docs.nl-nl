---
title: "Prijscategorieën in Azure-Database voor MySQL | Microsoft Docs"
description: "Prijscategorieën in Azure MySQL-Database"
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 11/03/2017
ms.openlocfilehash: ae7e57e9b40f5194c15525a48843060bbccaa956
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="azure-database-for-mysql-options-and-performance-understand-whats-available-in-each-pricing-tier"></a>Azure-Database voor de MySQL-opties en prestaties: inzicht in wat er beschikbaar is in elke prijscategorie
Wanneer u een Azure-Database voor de MySQL-server maakt, besluit u over drie belangrijkste keuzes voor het configureren van de resources die voor die server toegewezen. Deze keuzes invloed hebben op de prestaties en schaalbaarheid van de server.
- Prijscategorie
- Rekeneenheden
- Storage (GB)

Elke prijscategorie heeft een bereik van prestatieniveaus (Compute Units) om te kiezen, afhankelijk van uw werkbelastingen. Hogere prestaties bieden aanvullende bronnen voor de server die is ontworpen voor hogere doorvoer. U kunt het prestatieniveau van de server binnen een prijscategorie met vrijwel geen uitvaltijd voor toepassingen kunt wijzigen.

> [!IMPORTANT]
> Terwijl de service zich in de openbare preview, is er geen een gegarandeerde Service Level Agreement (SLA).

Een Azure-database voor MySQL-server kan een of meerdere databases bevatten. U kunt kiezen voor het maken van één database per server om gebruik te maken van alle resources of voor meerdere databases om de resources te delen. 

## <a name="choose-a-pricing-tier"></a>Een prijscategorie kiezen
In de preview, biedt Azure Database voor MySQL twee Prijscategorieën: Basic en Standard. Premium-laag is nog niet beschikbaar, maar wordt binnenkort beschikbaar. 

De volgende tabel bevat voorbeelden van de beste Prijscategorieën geschikt voor werklasten met verschillende groepen van toepassingen.

| Prijscategorie | Beoogde workloads |
| :----------- | :----------------|
| Basic | Het meest geschikt voor kleine werkbelastingen waarvoor schaalbare berekeningen en opslag zonder IOPS garanderen. Voorbeelden zijn onder meer servers die worden gebruikt voor ontwikkeling of tests of kleinschalige, onregelmatig gebruikte toepassingen. |
| Standard | De optie voor cloud-toepassingen die IOPS moeten garanderen met hoge doorvoer. Voorbeelden zijn web of analytische toepassingen. |
| Premium | Het meest geschikt voor werklasten die lage latentie voor i/o- en moeten. Biedt de beste ondersteuning voor veel gelijktijdige gebruikers. Van toepassing op databases die bedrijfskritieke toepassingen ondersteunen.<br />De Premium-prijscategorie is niet beschikbaar in preview. |

Om te beslissen over een prijscategorie, eerst bepalen of uw werkbelasting een garantie IOPS moet. Als dit het geval is, gebruikt u de Standard-prijscategorie.

| **Functies van de lagen prijzen** | **Basic** | **Standard** |
| :------------------------ | :-------- | :----------- |
| Maximale Compute-eenheden | 100 | 800 | 
| Maximale totale opslagruimte | 1 TB | 1 TB | 
| IOPS-garantie van opslag | N.v.t. | Ja | 
| Maximale opslag IOP 's | N.v.t. | 3.000 | 
| De back-up bewaarperiode database | 7 dagen | 35 dagen | 

Tijdens de preview-periode u de prijscategorie niet wijzigen zodra de server is gemaakt. In de toekomst, is het mogelijk zijn om te upgraden of downgraden van een server uit één prijscategorie naar een andere laag.

## <a name="understand-the-price"></a>De prijs begrijpen
Wanneer u een nieuwe Azure-Database maken voor MySQL binnen de [Azure-portal](https://portal.azure.com/#create/Microsoft.MySQLServer), selecteer de **prijscategorie** pagina en de maandelijkse kosten wordt weergegeven op basis van de opties die u hebt geselecteerd. Als u niet een Azure-abonnement hebt, moet u de Azure prijscategorie Rekenmachine gebruiken om op te halen van een geschatte prijs. Ga naar de [Azure prijscategorie Rekenmachine](https://azure.microsoft.com/pricing/calculator/) website, klikt u vervolgens op **items toevoegen**, vouw de **Databases** categorie, en kies **Azure Database voor MySQL** voor het aanpassen van de opties.

## <a name="choose-a-performance-level-compute-units"></a>Kies een prestatieniveau (Compute eenheden)
Zodra u de prijscategorie voor uw Azure-Database voor de MySQL-server hebt vastgesteld, bent u klaar om te bepalen van het prestatieniveau door het selecteren van het aantal Compute eenheden nodig. Een goed uitgangspunt is 200 of 400 Compute-eenheden voor toepassingen die hoger gelijktijdigheid van de gebruiker nodig voor hun web of analytische workloads en incrementeel indien nodig aanpassen. 

Rekenuren voor dat eenheden zijn een meting van CPU-verwerking doorvoer die beschikbaar zijn voor één Azure-Database voor de MySQL-server is gegarandeerd. Een Compute-eenheid is een gecombineerde meting van CPU en geheugenbronnen.  Zie voor meer informatie [waarin wordt uitgelegd Compute-eenheden](concepts-compute-unit-and-storage.md)

### <a name="basic-pricing-tier-performance-levels"></a>Basic prijscategorie laag prestatieniveaus:

| **Prestatieniveau** | **50** | **100** |
| :-------------------- | :----- | :------ |
| Maximale Compute-eenheden | 50 | 100 |
| Opgenomen opslaggrootte | 50 GB | 50 GB |
| Grootte van de opslagruimte voor de maximale\* | 1 TB | 1 TB |

### <a name="standard-pricing-tier-performance-levels"></a>Standaard prijscategorie laag prestatieniveaus:

| **Prestatieniveau** | **100** | **200** | **400** | **800** |
| :-------------------- | :------ | :------ | :------ | :------ |
| Maximale Compute-eenheden | 100 | 200 | 400 | 800 |
| Opgenomen opslaggrootte en ingerichte IOP 's | 125 GB<br/> 375 IOP 'S | 125 GB<br/> 375 IOP 'S | 125 GB<br/> 375 IOP 'S | 125 GB<br/> 375 IOP 'S |
| Grootte van de opslagruimte voor de maximale\* | 1 TB | 1 TB | 1 TB | 1 TB |
| Server voor max. IOP's ingericht | 3000 IOP 'S | 3000 IOP 'S | 3000 IOP 'S | 3000 IOP 'S |
| Server voor max. IOP's per GB ingericht | Vaste 3 IOP's per GB | Vaste 3 IOP's per GB | Vaste 3 IOP's per GB | Vaste 3 IOP's per GB |

\*Grootte van de opslagruimte voor de maximale verwijst naar de maximum ingerichte opslagruimte voor uw server.

## <a name="storage"></a>Storage 
De opslagconfiguratie definieert de hoeveelheid beschikbare opslagcapaciteit voor een Azure-Database voor de MySQL-server. De opslag die wordt gebruikt door de service omvat de databasebestanden en transactielogboeken de MySQL-server-Logboeken. Houd rekening met de grootte van de opslag die nodig is voor het hosten van uw databases en de prestatie-eisen (IOPS) bij het selecteren van de opslagconfiguratie.

Sommige opslagcapaciteit is ten minste opgenomen met elke prijscategorie, vermeld in de voorgaande tabel als 'Opgenomen opslaggrootte'. Extra opslagcapaciteit kan worden toegevoegd wanneer de server is gemaakt, in stappen van 125 GB tot de maximale toegestane opslagcapaciteit. De extra opslagcapaciteit kan onafhankelijk van de configuratie van de Compute-eenheden worden geconfigureerd. De prijs wordt aangepast op basis van de hoeveelheid opslagruimte die zijn geselecteerd.

De configuratie van de IOPS in elke prestatieniveau betrekking op de prijscategorie en de maximale grootte van het gekozen. Laag Basic biedt geen garantie van een IOPS. In de Standard-prijscategorie, de IOPS proportioneel aan de maximale opslaggrootte in een vaste verhouding van 3:1. De opgenomen opslag van 125 GB garanties met betrekking tot 375 ingericht IOP's, elk met een i/o-grootte van maximaal 256 KB. U kunt extra opslagruimte van maximaal 1 TB, 3000 ingerichte IOPS garanderen.

Bewaken van de grafiek metrische gegevens in de Azure portal of Azure CLI-opdrachten voor het meten van het verbruik van opslag en IOP's schrijven. Relevante metrische gegevens voor het bewaken van zijn opslaglimiet bereikt, opslagpercentage opslag gebruikt en i/o-procent.

>[!IMPORTANT]
> Kies in de preview, de hoeveelheid opslagruimte op het moment waarop de server is gemaakt. Wijzigen van de opslaggrootte van de op een bestaande server is nog niet ondersteund. 

## <a name="scaling-a-server-up-or-down"></a>Een server schaal omhoog of omlaag
U kiezen het niveau van de prijscategorie prijscategorie en prestatieniveau in eerste instantie wanneer u uw Azure-Database voor MySQL maken. Later kunt u de Compute-eenheden schalen omhoog of omlaag dynamisch, binnen het bereik van de dezelfde prijscategorie. Schuif in de Azure portal, de Compute-eenheden op van de server prijzen laag pagina of het script door dit voorbeeld te volgen: [bewaken en schalen van een Azure-Database voor de MySQL-server met Azure CLI](scripts/sample-scale-server.md)

De Compute-eenheden schalen gebeurt onafhankelijk van de maximum opslagruimte die u hebt gekozen.

Achter de schermen voor het wijzigen van het prestatieniveau van een server maakt een kopie van de oorspronkelijke server op het nieuwe prestatieniveau en vervolgens wordt overgeschakeld verbindingen naar de gekopieerde server. Gegevens niet verloren tijdens dit proces. Verbindingen met de database zijn tijdens de korte ogenblikken wanneer het systeem overgeschakeld naar de nieuwe kopie van de server uitgeschakeld, zodat het aantal transacties tijdens de vlucht kunnen worden teruggedraaid. Deze tijdsduur varieert, maar is gemiddeld korter dan 4 seconden en in meer dan 99% van de gevallen minder dan 30 seconden. Als er veel transacties actief zijn op het moment dat de verbindingen worden uitgeschakeld, wordt deze tijdsduur mogelijk ook langer.

De duur van het gehele scale-proces is afhankelijk van zowel de grootte en prijscategorie van de server vóór en na de wijziging. Bijvoorbeeld, moet een server die Compute eenheden wordt gewijzigd in de prijscategorie Standard uitvoeren binnen enkele minuten. De nieuwe eigenschappen voor de server worden niet toegepast totdat de wijzigingen voltooid zijn.

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over de Compute-eenheden [waarin wordt uitgelegd Compute-eenheden](concepts-compute-unit-and-storage.md)
- Meer informatie over hoe [bewaken en schalen van een Azure-Database voor de MySQL-server met Azure CLI](scripts/sample-scale-server.md)
