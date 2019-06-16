---
title: Aanbevelingen voor prestaties in Azure Database voor PostgreSQL - één Server
description: Dit artikel beschrijft de functie prestaties aanbeveling in Azure Database voor PostgreSQL - één Server.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 31d8c0fdf1b4df3ee00f3652c933b4b738384bea
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65068850"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql---single-server"></a>Aanbevelingen voor prestaties in Azure Database voor PostgreSQL - één Server

**Van toepassing op:** Azure Database for PostgreSQL - servergegevens 9.6 en 10

De functie voor aanbevelingen voor prestaties analyseert uw databases voor het maken van aangepaste suggesties voor betere prestaties. Voor het produceren van de aanbevelingen voor kijkt de analyse naar verschillende database-kenmerken, met inbegrip van schema. Schakel [Query Store](concepts-query-store.md) op uw server volledig gebruikmaken van de functie voor aanbevelingen voor prestaties. Na de implementatie van elke aanbeveling prestaties, moet u de prestaties voor het evalueren van de impact van deze wijzigingen testen. 

## <a name="permissions"></a>Machtigingen
De machtigingen van **Eigenaar** of **Inzender** zijn vereist voor het uitvoeren van analyses met de functie Prestatieaanbevelingen.

## <a name="performance-recommendations"></a>Aanbevelingen voor prestaties
De functie [Prestatieaanbevelingen](concepts-performance-recommendations.md) analyseert workloads op de server om indexen te analyseren met de mogelijkheid om prestaties te verbeteren.

Open **aanbevelingen voor prestaties** uit de **intelligente prestaties** sectie van de in de menubalk op de Azure portal-pagina voor uw PostgreSQL-server.

![Landingspagina van Prestatieaanbevelingen](./media/concepts-performance-recommendations/performance-recommendations-page.png)

Selecteer **analyseren** en kiest u een database, die de analyse wordt gestart. Th analyse kan enkele minuten duren, afhankelijk van uw workload. Wanneer de analyse is voltooid, verschijnt er een melding in de portal. Analyse voert een grondige onderzoek van de database. U wordt aangeraden dat u analyse uitvoeren tijdens daluren. 

De **aanbevelingen** venster ziet u een lijst met aanbevelingen als deze zijn gevonden.

![Nieuwe pagina voor prestatie-aanbevelingen](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Aanbevelingen worden niet automatisch toegepast. Als u wilt de aanbeveling toepast, Kopieer de querytekst en uitvoeren vanaf de client naar keuze. Houd er rekening mee te testen en controleren om te evalueren van de aanbeveling. 

## <a name="recommendation-types"></a>Aanbeveling typen

Op dit moment worden twee typen aanbevelingen ondersteund: *Index maken* en *Drop Index*.

### <a name="create-index-recommendations"></a>Aanbevelingen van Index maken
*Index maken* aanbevelingen voorgesteld nieuwe indexen om de meest uitgevoerd of tijdrovende query's in de werkbelasting te versnellen. Dit type aanbeveling is vereist [Query Store](concepts-query-store.md) zijn ingeschakeld. Query Store querygegevens verzamelt en voorziet in de gedetailleerde query-runtime en de frequentie statistieken die de analyse gebruikt voor het maken van de aanbeveling.

### <a name="drop-index-recommendations"></a>DROP indexaanbevelingen
Azure Database for PostgreSQL analyseert naast het detecteren van ontbrekende indexen, de prestaties van bestaande indexen. Als u een index wordt zelden gebruikt of redundante, de analyzer raadt aan om deze neer te zetten.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [bewaking en afstemming](concepts-monitoring.md) in Azure Database for PostgreSQL.

