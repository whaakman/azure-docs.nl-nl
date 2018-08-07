---
title: Toepassen van aanbevelingen voor prestaties - Azure SQL Database | Microsoft Docs
description: De Azure portal gebruiken voor het vinden van de aanbevelingen voor prestaties die prestaties van uw Azure SQL Database kunnen optimaliseren.
services: sql-database
author: danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: v-daljep
ms.reviewer: carlrab
ms.openlocfilehash: 7df4033a378ce3b37a99ed0335200e7f71696594
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39528643"
---
# <a name="find-and-apply-performance-recommendations"></a>Zoeken en toepassen van aanbevelingen voor prestaties

U kunt Azure portal gebruiken om te vinden van de aanbevelingen voor prestaties die prestaties van uw Azure SQL Database kunnen optimaliseren of om bepaalde aangeduid in uw werkbelasting probleem te verhelpen. **Prestaties aanbeveling** pagina in Azure portal kunt u de aanbevelingen op basis van de potentiële impact vinden. 

## <a name="viewing-recommendations"></a>Aanbevelingen weergeven

Als u wilt weergeven en prestatie-aanbevelingen toepassen, moet u de juiste [op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md) machtigingen in Azure. **Lezer**, **' SQL DB Contributor '** machtigingen zijn vereist om aanbevelingen weer te geven en **eigenaar**, **' SQL DB Contributor '** machtigingen zijn vereist voor alle acties; uitvoeren Maak of verwijder indexen en maken van een index annuleren.

Gebruik de volgende stappen uit om te vinden van aanbevelingen voor prestaties in Azure portal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Ga naar **alle services** > **SQL-databases**, en selecteer uw database.
3. Navigeer naar **prestaties aanbeveling** om beschikbare aanbevelingen voor de geselecteerde database weer te geven.

Aanbevelingen voor prestaties worden weergegeven in de tabel die vergelijkbaar is met de architectuur die wordt weergegeven op de volgende afbeelding:

![Aanbevelingen](./media/sql-database-advisor-portal/recommendations.png)

Aanbevelingen worden gesorteerd op hun mogelijke invloed op de prestaties in de volgende categorieën:

| Impact | Beschrijving |
|:--- |:--- |
| Hoog |Aanbevelingen voor hoge impact moeten de meeste impact prestaties bieden. |
| Middelgroot |Normale impact aanbevelingen voor moeten de prestaties verbeteren, maar niet aanzienlijk. |
| Laag |Weinig impact aanbevelingen voor moeten zorgen voor betere prestaties dan zonder, maar mogelijk aanzienlijke verbeteringen niet. |


> [!NOTE]
> Azure SQL-Database moet activiteiten volgen die ten minste een dag om enkele aanbevelingen. De Azure SQL Database kunt u eenvoudiger optimaliseren voor consistente querypatronen dan kan worden voor willekeurige keuze pieken van activiteit. Als u aanbevelingen zijn momenteel niet beschikbaar, de **prestaties aanbeveling** pagina vindt u een bericht waarin wordt uitgelegd waarom.
> 

U kunt ook de status van de historische bewerkingen bekijken. Selecteer een aanbeveling of de status voor meer informatie.

Hier volgt een voorbeeld van de aanbeveling voor 'Index maken' in de Azure-portal.

![Index maken](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a>Toepassen van aanbevelingen
Azure SQL Database biedt u volledige controle over hoe de aanbevelingen zijn ingeschakeld met behulp van een van de volgende drie opties: 

* Afzonderlijke aanbevelingen voor een toepassing op een tijdstip.
* Schakel de functie automatisch afstemmen voor het automatisch toepassen van aanbevelingen.
* Voer voor het handmatig implementeren van een aanbeveling, de aanbevolen T-SQL-script voor de database.

Selecteer elke aanbeveling klikken de details ervan te bekijken en klik vervolgens op **script weergeven** om te controleren van de exacte details van de manier waarop de aanbeveling wordt gemaakt.

De database blijft online terwijl de aanbeveling is toegepast--met behulp van de prestaties aanbeveling of automatisch afstemmen van een database nooit offline gaat.

### <a name="apply-an-individual-recommendation"></a>Een afzonderlijke aanbeveling toepast
U kunt bekijken en aanbevelingen één bewerking tegelijk accepteren.

1. Op de **aanbevelingen** pagina, selecteert u een aanbeveling.
2. Op de **Details** pagina, klikt u op **toepassen** knop.
   
    ![Aanbeveling toepast](./media/sql-database-advisor-portal/apply.png)

De geselecteerde aanbeveling worden toegepast op de database.

### <a name="removing-recommendations-from-the-list"></a>Aanbevelingen verwijderen in de lijst
Als uw lijst met aanbevelingen items die u wilt verwijderen uit de lijst bevat, kunt u de aanbeveling negeren:

1. Selecteer een aanbeveling in de lijst met **aanbevelingen** de details te openen.
2. Klik op **negeren** op de **Details** pagina.

Indien gewenst, kunt u toevoegen verwijderde items terug naar de **aanbevelingen** lijst:

1. Op de **aanbevelingen** pagina, klikt u op **Genegeerde items weergeven**.
2. Selecteer een verwijderde item in de lijst om de details ervan weer te geven.
3. Klik desgewenst op **ongedaan negeren** om toe te voegen van de index terug naar de belangrijkste lijst van **aanbevelingen**.

> [!NOTE]
> Houd er rekening mee dat als SQL-Database [automatisch afstemmen](sql-database-automatic-tuning.md) is ingeschakeld, en als u een aanbeveling in de lijst handmatig hebt genegeerd, deze aanbeveling niet automatisch worden toegepast. Verwijderen van een aanbeveling is een handige manier voor gebruikers van automatisch afstemmen is ingeschakeld in gevallen wanneer vereisen dat een specifieke aanbeveling mag niet worden toegepast.
> U kunt dit gedrag herstellen door genegeerde aanbevelingen terug naar de lijst met aanbevelingen toe te voegen met de optie verwijderen ongedaan maken.
> 

### <a name="enable-automatic-tuning"></a>Automatisch instellen inschakelen
U kunt de Azure SQL-Database voor het implementeren van aanbevelingen automatisch instellen. Wanneer aanbevelingen beschikbaar komen, worden automatisch toegepast. Net als bij alle aanbevelingen die worden beheerd door de service als de invloed op de prestaties negatief is, is de aanbeveling is hersteld.

1. Op de **aanbevelingen** pagina, klikt u op **automatiseren**:
   
    ![Advisor-instellingen](./media/sql-database-advisor-portal/settings.png)
2. Selecteer acties automatiseren:
   
    ![Aanbevolen indexen](./media/sql-database-automatic-tuning-enable/server.png)

> [!NOTE]
> Houd er rekening mee dat **DROP_INDEX** optie op dit moment is niet compatibel met toepassingen die gebruikmaken van partitie schakelen en de index-hints en mag niet worden ingeschakeld in deze gevallen.
>

Nadat u de gewenste configuratie hebt geselecteerd, klikt u op toepassen.

### <a name="manually-run-the-recommended-t-sql-script"></a>Handmatig uitvoeren het aanbevolen T-SQL-script
Selecteer elke aanbeveling klikken en klik vervolgens op **script weergeven**. Voer dit script op basis van de database om handmatig de aanbeveling toepast.

*Indexen die handmatig worden uitgevoerd worden niet gecontroleerd en gevalideerd voor prestatie-impact door de service* , zodat het wordt aangeraden dat u deze indexen na het maken om te controleren of ze leveren prestaties controleren en aanpassen of verwijder ze indien nodig. Zie voor meer informatie over het maken van indexen [INDEX maken (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).

### <a name="canceling-recommendations"></a>Aanbevelingen annuleren
Aanbevelingen die zich in een **in behandeling**, **controleren**, of **succes** status kan worden geannuleerd. Aanbevelingen met de status van **Executing** kan niet worden geannuleerd.

1. Selecteer een aanbeveling in de **afstemmen geschiedenis** gebied te openen de **gegevens over de aanbevelingen** pagina.
2. Klik op **annuleren** om af te breken van het proces van het toepassen van de aanbeveling.

## <a name="monitoring-operations"></a>Controlebewerkingen
Toepassen van een aanbeveling mogelijk niet onmiddellijk plaatsvindt. De portal bevat informatie over de status van de aanbeveling. Hier volgen de mogelijke statussen die een index, kan zich in:

| Status | Beschrijving |
|:--- |:--- |
| In behandeling |Aanbeveling opdracht is ontvangen en gepland voor uitvoering van toepassing. |
| In uitvoering |De aanbeveling is toegepast. |
| Controleren |De aanbeveling is toegepast en de service is meten van de voordelen. |
| Geslaagd |De aanbeveling is toegepast en voordelen zijn gemeten. |
| Fout |Er is een fout opgetreden tijdens het toepassen van de aanbeveling. Dit is een tijdelijk probleem of mogelijk een schema wijzigen in de tabel en het script is niet meer geldig. |
| Terugdraaien |De aanbeveling is toegepast, maar niet-goed presterende ervan en wordt automatisch hersteld. |
| Teruggedraaid |De aanbeveling is hersteld. |

Klik op een in-process-aanbeveling in de lijst voor meer informatie:

![Aanbevolen indexen](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a>Herstellen van een aanbeveling
Als u de aanbevelingen voor prestaties gebruikt voor het toepassen van de aanbeveling (wat betekent dat u handmatig niet de T-SQL-script hebt uitgevoerd), wordt automatisch hersteld de wijziging als de invloed op de prestaties negatief is gevonden. Als voor een bepaalde reden herstellen van een aanbeveling wilt, kunt u het volgende doen:

1. Selecteer een aanbeveling is toegepast in de **Afstemmingsgeschiedenis** gebied.
2. Klik op **Revert** op de **details van de aanbeveling** pagina.

![Aanbevolen indexen](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a>Bewaking van invloed op de prestaties van indexaanbevelingen voor de
Nadat de aanbevelingen zijn geïmplementeerd (momenteel, index-bewerkingen en alleen de aanbevelingen voor de query's parameteriseren), klikt u op **Queryinzichten** op de aanbeveling details pagina als u wilt openen [Query Prestatie-inzichten](sql-database-query-performance.md) en de impact van de prestaties van uw belangrijkste query's zien.

![Invloed op de prestaties van de monitor](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a>Samenvatting
Azure SQL-Database bevat aanbevelingen voor het verbeteren van de prestaties van de SQL-database. Door op te geven T-SQL-scripts, krijgt u hulp bij het optimaliseren van uw database en uiteindelijk verbeteren van de prestaties van query's.

## <a name="next-steps"></a>Volgende stappen
Bewaak uw aanbevelingen en doorgaan om toe te passen om prestaties te zetten. Databaseworkloads zijn dynamisch en veranderen voortdurend. Azure SQL Database blijft bewaakt en doet aanbevelingen die u kunnen mogelijk de verbeteren de prestaties van uw database. 

* Zie [automatisch afstemmen](sql-database-automatic-tuning.md) voor meer informatie over het automatisch afstemmen in Azure SQL Database.
* Zie [aanbevelingen voor prestaties](sql-database-advisor.md) voor een overzicht van Azure SQL Database-aanbevelingen voor prestaties.
* Zie [Query prestatie-inzichten](sql-database-query-performance.md) voor meer informatie over het weergeven van de invloed op de prestaties van uw belangrijkste query's.

## <a name="additional-resources"></a>Aanvullende resources
* [Query Store](https://msdn.microsoft.com/library/dn817826.aspx)
* [INDEX MAKEN](https://msdn.microsoft.com/library/ms188783.aspx)
* [Op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md)

