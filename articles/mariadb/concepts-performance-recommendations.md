---
title: Aanbevelingen voor prestaties in Azure Database voor MariaDB
description: In dit artikel beschrijft de functie prestaties aanbeveling in Azure Database voor MariaDB
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/12/2019
ms.openlocfilehash: ad3e2157e7b8748174cc8b81d1debd5de0ba77b9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079481"
---
# <a name="performance-recommendations-in-azure-database-for-mariadb"></a>Aanbevelingen voor prestaties in Azure Database voor MariaDB

**Van toepassing op:** Azure Database voor MariaDB 10.2s

> [!NOTE]
> Aanbevelingen voor prestaties is beschikbaar als preview. Ondersteuning voor aanbevelingen voor prestaties in Azure portal is geïmplementeerd en kan nog niet beschikbaar in uw regio.

De functie voor aanbevelingen voor prestaties analyseert uw databases voor het maken van aangepaste suggesties voor betere prestaties. Voor het produceren van de aanbevelingen voor kijkt de analyse naar verschillende database-kenmerken, met inbegrip van schema. Schakel [Query Store](concepts-query-store.md) op uw server volledig gebruikmaken van de functie voor aanbevelingen voor prestaties. Als prestaties schema uitgeschakeld is, kunt u Query Store inschakelen performance_schema en een subset van de prestaties schema instrumenten vereist voor de functie. Na de implementatie van elke aanbeveling prestaties, moet u de prestaties voor het evalueren van de impact van deze wijzigingen testen.

## <a name="permissions"></a>Machtigingen

De machtigingen van **Eigenaar** of **Inzender** zijn vereist voor het uitvoeren van analyses met de functie Prestatieaanbevelingen.

## <a name="performance-recommendations"></a>Aanbevelingen voor prestaties

De functie [Prestatieaanbevelingen](concepts-performance-recommendations.md) analyseert workloads op de server om indexen te analyseren met de mogelijkheid om prestaties te verbeteren.

Open **aanbevelingen voor prestaties** uit de **intelligente prestaties** sectie van de in de menubalk op de Azure portal-pagina voor uw MariaDB-server.

![Landingspagina van Prestatieaanbevelingen](./media/concepts-performance-recommendations/performance-recommendations-page.png)

Selecteer **analyseren** en kiest u een database, die de analyse wordt gestart. De analyse kan enkele minuten duren, afhankelijk van uw workload. Wanneer de analyse is voltooid, verschijnt er een melding in de portal. Analyse voert een grondige onderzoek van de database. U wordt aangeraden dat u analyse uitvoeren tijdens daluren.

De **aanbevelingen** venster ziet u een lijst met aanbevelingen als een onderdelen en de gerelateerde query-ID die deze aanbeveling heeft gegenereerd. Met de query-ID, kunt u de [mysql.query_store](concepts-query-store.md#mysqlquery_store) weergeven voor meer informatie over de query.

![Nieuwe pagina voor prestatie-aanbevelingen](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Aanbevelingen worden niet automatisch toegepast. Als u wilt de aanbeveling toepast, Kopieer de querytekst en uitvoeren vanaf de client naar keuze. Houd er rekening mee te testen en controleren om te evalueren van de aanbeveling.

## <a name="recommendation-types"></a>Aanbeveling typen

Op dit moment alleen *Create Index* aanbevelingen worden ondersteund.

### <a name="create-index-recommendations"></a>Aanbevelingen van Index maken

*Index maken* aanbevelingen voorgesteld nieuwe indexen om de meest uitgevoerd of tijdrovende query's in de werkbelasting te versnellen. Dit type aanbeveling is vereist [Query Store](concepts-query-store.md) zijn ingeschakeld. Query Store querygegevens verzamelt en voorziet in de gedetailleerde query-runtime en de frequentie statistieken die de analyse gebruikt voor het maken van de aanbeveling.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [bewaking en afstemming](concepts-monitoring.md) in Azure Database voor MariaDB.