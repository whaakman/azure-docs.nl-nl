---
title: Toepassen van aanbevelingen voor prestaties - Azure SQL Database | Microsoft Docs
description: De Azure portal gebruiken om te zoeken prestaties aanbevelingen die u kunnen de prestaties van uw Azure SQL Database optimaliseren.
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: cda8a646-0584-4368-b28a-85cdd9b54fcd
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 2b725c60dbcb9737b00ffd5fb43273b26dda1ea8
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="find-and-apply-performance-recommendations"></a>Zoeken en toepassen van aanbevelingen voor prestaties

Kunt u de Azure-portal prestaties aanbevelingen die u kunnen de prestaties van uw Azure SQL Database optimaliseren zoeken of om bepaalde geïdentificeerd in de werkbelasting van uw probleem te verhelpen. **Prestaties aanbeveling** pagina in de Azure-portal kunt u de aanbevelingen op basis van de potentiële impact vinden. 

## <a name="viewing-recommendations"></a>Aanbevelingen weergeven

Als u wilt bekijken en toepassen van aanbevelingen voor prestaties, moet u de juiste [toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-what-is.md) machtigingen in Azure. **Lezer**, **SQL DB Contributor** machtigingen zijn vereist voor het weergeven van aanbevelingen en **eigenaar**, **SQL DB Contributor** machtigingen zijn vereist voor uitvoeren van acties; Maak of verwijder indexen en maken van een index te annuleren.

Gebruik de volgende stappen uit om te zoeken prestaties aanbevelingen in Azure-portal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Ga naar **meer services** > **SQL-databases**, en selecteer uw database.
3. Navigeer naar **prestaties aanbeveling** om beschikbare aanbevelingen voor de geselecteerde database weer te geven.

Prestaties aanbevelingen worden weergegeven in de tabel die vergelijkbaar is met wordt weergegeven op de volgende afbeelding:

![Aanbevelingen](./media/sql-database-advisor-portal/recommendations.png)

Aanbevelingen worden gesorteerd op de potentiële impact op de prestaties in de volgende categorieën:

| Impact | Beschrijving |
|:--- |:--- |
| Hoog |Aanbevelingen voor grote invloed moeten een van de belangrijkste prestatie-invloed. |
| Middelgroot |Gemiddeld impact aanbevelingen moeten de prestaties verbeteren, maar niet aanzienlijk. |
| Laag |Lage impact aanbevelingen moeten zorgen voor betere prestaties dan zonder, maar mogelijk aanzienlijke verbeteringen niet. |


> [!NOTE]
> Azure SQL-Database moet activiteiten volgen die ten minste een dag om te kunnen identificeren van enkele aanbevelingen. De Azure SQL Database kunt gemakkelijker optimaliseren voor consistente querypatronen dan dit kunt doen voor willekeurige slechte bursts van activiteit. Als de aanbevelingen zijn momenteel niet beschikbaar, de **prestaties aanbeveling** pagina vindt u een bericht waarin wordt uitgelegd waarom.
> 

U kunt ook de status van de historische bewerkingen weergeven. Selecteer een aanbeveling of de status voor meer informatie.

Hier volgt een voorbeeld van een aanbeveling 'Index maken' in de Azure portal.

![Index maken](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a>Toepassen van aanbevelingen
Azure SQL Database hebt u volledige controle over hoe aanbevelingen worden ingeschakeld met een van de volgende drie opties: 

* Afzonderlijke aanbevelingen één toepassing tegelijk.
* Schakel de automatische afstemming voor automatisch toepassen van aanbevelingen.
* Een aanbeveling als handmatig wilt implementeren, de aanbevolen T-SQL-script worden uitgevoerd op basis van uw database.

Selecteer elke aanbeveling om de details weergeven en klik vervolgens op **script weergeven** om te controleren van de exacte details van hoe de aanbeveling wordt gemaakt.

De database blijft online terwijl de aanbeveling wordt toegepast--met prestaties aanbeveling of automatische afstemming een database nooit offline neemt.

### <a name="apply-an-individual-recommendation"></a>Een afzonderlijke aanbeveling toepassen
U kunt bekijken en aanbevelingen een tegelijk accepteren.

1. Op de **aanbevelingen** pagina, selecteert u een aanbeveling.
2. Op de **Details** pagina, klikt u op **toepassen** knop.
   
    ![Aanbeveling toepassen](./media/sql-database-advisor-portal/apply.png)

Geselecteerde aanbeveling worden toegepast op de database.

### <a name="removing-recommendations-from-the-list"></a>Aanbevelingen verwijderen uit de lijst
Als uw lijst met aanbevelingen items die u wilt verwijderen uit de lijst bevat, kunt u de aanbeveling negeren:

1. Selecteer een aanbeveling in de lijst met **aanbevelingen** de details te openen.
2. Klik op **negeren** op de **Details** pagina.

Indien gewenst, kunt u toevoegen verwijderde items terug naar de **aanbevelingen** lijst:

1. Op de **aanbevelingen** pagina, klikt u op **weergave verwijderd**.
2. Selecteer een verwijderde item uit de lijst om de details ervan weer te geven.
3. Klik desgewenst op **ongedaan negeren** om toe te voegen van de index terug naar de belangrijkste lijst van **aanbevelingen**.

> [!NOTE]
> Houd er rekening mee dat als SQL-Database [automatische afstemming](sql-database-automatic-tuning.md) is ingeschakeld, en als u handmatig een aanbeveling uit de lijst hebt verwijderd, de aanbeveling nooit automatisch worden toegepast. Verwijderen van een aanbeveling is een handige manier voor gebruikers om automatische afstemming ingeschakeld ingeval wanneer vereisen dat een specifieke aanbeveling mag niet worden toegepast.
> U kunt dit gedrag herstellen door het toevoegen van verwijderde aanbevelingen terug naar de lijst met aanbevelingen door de optie negeren ongedaan maken.
> 

### <a name="enable-automatic-tuning"></a>Automatisch instellen inschakelen
U kunt de Azure SQL Database voor het implementeren van aanbevelingen automatisch instellen. Aanbevelingen beschikbaar komen, worden automatisch toegepast. Net als bij alle aanbevelingen worden beheerd door de service als het prestatieresultaat negatief is, is de aanbeveling is hersteld.

1. Op de **aanbevelingen** pagina, klikt u op **automatiseren**:
   
    ![Advisor-instellingen](./media/sql-database-advisor-portal/settings.png)
2. Selecteer de acties die voor het automatiseren van:
   
    ![Aanbevolen indexen](./media/sql-database-advisor-portal/automation.png)

### <a name="manually-run-the-recommended-t-sql-script"></a>Voer handmatig het aanbevolen T-SQL-script
Selecteer elke aanbeveling en klik op **script weergeven**. Dit script wordt uitgevoerd in de database naar de aanbeveling handmatig toe te passen.

*Indexen die handmatig worden uitgevoerd wordt niet gecontroleerd en gevalideerd voor invloed op de prestaties door de service* zodat het wordt aangeraden deze indexen te bewaken na herstart maken om te controleren of ze bieden betere prestaties en aanpassen of verwijder ze indien nodig. Zie voor meer informatie over het maken van indexen [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).

### <a name="canceling-recommendations"></a>Aanbevelingen annuleren
Aanbevelingen die zich in een **in behandeling**, **controleren**, of **geslaagd** status kan worden geannuleerd. Aanbevelingen met de status van **Executing** kan niet worden geannuleerd.

1. Selecteer een aanbeveling in de **afstemmen geschiedenis** gebied openen de **aanbevelingen details** pagina.
2. Klik op **annuleren** om af te breken van het proces van het toepassen van de aanbeveling.

## <a name="monitoring-operations"></a>Bewakingsbewerkingen
Toepassen van een aanbeveling kan niet direct gebeuren. De portal biedt informatie over de status van een aanbeveling. Hier volgen de mogelijke statussen van een index worden weergegeven in:

| Status | Beschrijving |
|:--- |:--- |
| In behandeling |Aanbeveling opdracht is ontvangen en is gepland voor uitvoering toepassen. |
| In uitvoering |De aanbeveling wordt toegepast. |
| Controleren |Aanbeveling is toegepast en de service is de voordelen te meten. |
| Geslaagd |Aanbeveling is toegepast en voordelen zijn gemeten. |
| Fout |Er is een fout opgetreden tijdens het toepassen van de aanbeveling. Dit is een tijdelijk probleem of mogelijk een schema wijzigen in de tabel en het script is niet meer geldig. |
| Terugdraaien |De aanbeveling is toegepast, maar heeft vastgesteld dat is niet zodat en automatisch wordt omgezet. |
| Teruggedraaid |De aanbeveling is hersteld. |

Klik op een aanbeveling in proces uit de lijst voor meer informatie:

![Aanbevolen indexen](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a>Herstellen van een aanbeveling
Als u de prestaties van aanbevelingen om toe te passen de aanbeveling gebruikt teruggezet (wat betekent dat u de T-SQL-script handmatig niet hebt uitgevoerd), het automatisch de wijziging als het wordt het prestatieresultaat negatief gevonden. Als voor een of andere reden u gewoon een aanbeveling te herstellen wilt, kunt u het volgende doen:

1. Selecteer een toegepaste aanbeveling in de **afstemmen geschiedenis** gebied.
2. Klik op **Revert** op de **gegevens over de aanbeveling** pagina.

![Aanbevolen indexen](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a>Bewaking van prestatie-invloed van de indexaanbevelingen
Nadat de aanbevelingen zijn geïmplementeerd (momenteel index-bewerkingen en alleen query's aanbevelingen voorzien), klikt u op **Query Insights** op aanbeveling details pagina als u wilt openen [Query Inzichten](sql-database-query-performance.md) en de impact van de prestaties van uw meest gebruikte query's.

![Prestatie-invloed van monitor](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a>Samenvatting
Azure SQL-Database bevat aanbevelingen voor het verbeteren van de prestaties van de SQL-database. Dankzij de T-SQL-scripts, u u hulp nodig hebt in uw database te optimaliseren en uiteindelijk queryprestaties te verbeteren.

## <a name="next-steps"></a>Volgende stappen
Bewaken van uw aanbevelingen en blijven toepassen om de prestaties te verfijnen. Database-werkbelastingen zijn dynamisch en continu wijzigen. Azure SQL Database blijft om te controleren en aanbevelingen die potentieel prestaties van uw database kunt verbeteren. 

* Zie [automatische afstemming](sql-database-automatic-tuning.md) voor meer informatie over automatische afstemming in Azure SQL Database.
* Zie [prestaties aanbevelingen](sql-database-advisor.md) voor een overzicht van Azure SQL Database prestaties aanbevelingen.
* Zie [inzichten voor queryprestaties](sql-database-query-performance.md) voor meer informatie over het weergeven van de prestatie-invloed van de meest gebruikte query's.

## <a name="additional-resources"></a>Aanvullende resources
* [Query Store](https://msdn.microsoft.com/library/dn817826.aspx)
* [INDEX MAKEN](https://msdn.microsoft.com/library/ms188783.aspx)
* [Toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-what-is.md)

