---
title: Zelfstudie over het controleren en afstemmen in Azure Database voor PostgreSQL - één Server
description: Deze zelfstudie leidt u door bewaking en afstemming in Azure Database voor PostgreSQL - één Server.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: tutorial
ms.date: 5/6/2019
ms.openlocfilehash: dcd8363db33809e2a33885f43e570c53807505fd
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "65073092"
---
# <a name="tutorial-monitor-and-tune-azure-database-for-postgresql---single-server"></a>Zelfstudie: Controleren en afstemmen van Azure Database voor PostgreSQL - één Server

Azure Database for PostgreSQL bevat functies die u helpen om inzicht te krijgen in de prestaties van uw server en om deze te verbeteren. In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Het verzamelen van query- en wachtstatistieken inschakelen
> * De verzamelde gegevens weergeven en gebruiken
> * Queryprestaties en wachtstatistieken na verloop van tijd weergeven
> * Een database analyseren om prestatieaanbevelingen op te halen
> * Prestatie-aanbevelingen toepassen

## <a name="before-you-begin"></a>Voordat u begint
U hebt een Azure Database for PostgreSQL-server met PostgreSQL versie 9.6 of 10 nodig. U kunt de stappen in de zelfstudie [Maken](tutorial-design-database-using-azure-portal.md) volgen als u een server wilt maken.

> [!IMPORTANT]
> **Query Store**, **Query Performance Insight** en **Prestatieaanbeveling** bevinden zich in openbare preview.

## <a name="enabling-data-collection"></a>Gegevensverzameling inschakelen
[Query Store](concepts-query-store.md) legt de geschiedenis van query's en wachtstatistieken op uw server vast en slaat deze op in de database **azure_sys** database op uw server. Het is een optionele functie. U schakelt deze als volgt in:

1. Open Azure Portal.

2. Selecteer uw Azure Database for PostgreSQL-server.

3. Selecteer **Serverparameters** in de sectie **Instellingen** van het menu aan de linkerkant.

4. Stel **pg_qs.query_capture_mode** in op **TOP** om queryprestatiegegevens te gaan verzamelen. Stel **pgms_wait_sampling.query_capture_mode** in op **ALLE** om wachtstatistieken te gaan verzamelen. Sla op.
   
   ![Parameters voor Query Store-server](./media/tutorial-performance-intelligence/query-store-parameters.png)

5. Wacht 20 minuten tot de eerste batch met gegevens is vastgelegd in database **azure_sys**.


## <a name="performance-insights"></a>Prestatie-inzichten
De weergave [Query Performance Insight](concepts-query-performance-insight.md) in de Azure Portal toont visualisaties van belangrijke informatie uit de Query Store. 

1. Selecteer op de portalpagina van uw Azure Database for PostgreSQL-server de optie **Query Performance Insight** in de sectie **Ondersteuning en probleemoplossing** van het menu aan de linkerkant.

2. Op het tabblad **Langlopende query's** ziet u de top 5 query's op de gemiddelde duur per uitvoering, samengevoegd in intervallen van 15 minuten. 
   
   ![Landingspagina van Query Performance Insight](./media/tutorial-performance-intelligence/query-performance-insight-landing-page.png)

   U kunt meer query's weergeven door een optie te selecteren in de vervolgkeuzelijst **Aantal query's**. Het is mogelijk dat de grafiekkleuren voor een specifieke query-id verschillen wanneer u dit doet.

3. U kunt in de grafiek klikken en slepen om de tijdspanne te beperken tot een specifiek tijdvenster.

4. Gebruik de pictogrammen Inzoomen/Uitzoomen als u respectievelijk een kortere of langere periode wilt weergeven.

5. Bekijk de tabel onder de grafiek voor meer informatie over de langlopende query's in dat tijdvenster.

6. Selecteer het tabblad **Wachtstatistieken** om de bijbehorende visualisaties voor wachttijden in de server weer te geven.
   
   ![Wachtstatistieken van Query Performance Insight](./media/tutorial-performance-intelligence/query-performance-insight-wait-statistics.png)

### <a name="permissions"></a>Machtigingen
De machtigingen **Eigenaar** of **Inzender** zijn vereist om de tekst van de query's weer te geven in Query Performance Insight. Met de machtiging **Lezer** kunt u grafieken en tabellen weergeven maar geen tekst opvragen.


## <a name="performance-recommendations"></a>Prestatieaanbevelingen
De functie [Prestatieaanbevelingen](concepts-performance-recommendations.md) analyseert workloads op de server om indexen te analyseren met de mogelijkheid om prestaties te verbeteren.

1. Open **Prestatieaanbevelingen** in de sectie **Ondersteuning en probleemoplossing** van de menubalk op de Azure Portal-pagina voor uw PostgreSQL-server.
   
   ![Landingspagina van Prestatieaanbevelingen](./media/tutorial-performance-intelligence/performance-recommendations-landing-page.png)

2. Selecteer **Analyseren** en kies een database. De analyse wordt hiermee gestart.

3. Afhankelijk van de workload kan de analyse enkele minuten duren. Wanneer de analyse is voltooid, verschijnt er een melding in de portal.

4. Het venster **Prestatieaanbevelingen** toont een lijst met aanbevelingen als deze zijn gevonden. 

5. Een aanbeveling bevat informatie over de relevante **Database**, **Tabel**, **Kolom** en **Indexgrootte**.

   ![Resultaat van Prestatieaanbevelingen](./media/tutorial-performance-intelligence/performance-recommendations-result.png)

6. Als u de aanbeveling wilt implementeren, kopieert u de querytekst en voert u deze uit vanaf de gewenste client.

### <a name="permissions"></a>Machtigingen
De machtigingen van **Eigenaar** of **Inzender** zijn vereist voor het uitvoeren van analyses met de functie Prestatieaanbevelingen.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [bewaking en afstemming](concepts-monitoring.md) in Azure Database for PostgreSQL.