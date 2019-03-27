---
title: Aanbevelingen in Azure Database for PostgreSQL voor prestaties
description: Dit artikel beschrijft de functie prestaties aanbeveling in Azure Database voor PostgreSQL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/26/2018
ms.openlocfilehash: d8edbc2847c06e95e658a1324f2e85f1758e60be
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58487937"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql"></a>Aanbevelingen in Azure Database for PostgreSQL voor prestaties

**Van toepassing op:** Azure Database for PostgreSQL 9.6 en 10

De functie voor aanbevelingen voor prestaties geeft de bovenste indexen die kunnen worden gemaakt in uw Azure Database for PostgreSQL-server om prestaties te verbeteren. Om te produceren indexaanbevelingen, wordt de functie rekening gehouden met verschillende kenmerken van de database, met inbegrip van het schema en de werkbelasting, zoals gerapporteerd door de Query Store. Na de implementatie van elke aanbeveling prestaties, moeten klanten prestaties voor het evalueren van de impact van deze wijzigingen testen. 

## <a name="permissions"></a>Machtigingen
De machtigingen van **Eigenaar** of **Inzender** zijn vereist voor het uitvoeren van analyses met de functie Prestatieaanbevelingen.

## <a name="performance-recommendations"></a>Aanbevelingen voor prestaties
De functie [Prestatieaanbevelingen](concepts-performance-recommendations.md) analyseert workloads op de server om indexen te analyseren met de mogelijkheid om prestaties te verbeteren.

Open **Prestatieaanbevelingen** in de sectie **Ondersteuning en probleemoplossing** van de menubalk op de Azure Portal-pagina voor uw PostgreSQL-server.

![Landingspagina van Prestatieaanbevelingen](./media/concepts-performance-recommendations/performance-recommendations-landing-page.png)

Selecteer **Analyseren** en kies een database. De analyse wordt hiermee gestart. Dit kan enkele minuten duren, afhankelijk van uw workload. Wanneer de analyse is voltooid, verschijnt er een melding in de portal.

Het venster **Prestatieaanbevelingen** toont een lijst met aanbevelingen als deze zijn gevonden. Een aanbeveling bevat informatie over de relevante **Database**, **Tabel**, **Kolom** en **Indexgrootte**.

![Nieuwe pagina voor prestatie-aanbevelingen](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Als u de aanbeveling wilt implementeren, kopieert u de querytekst en voert u deze uit vanaf de gewenste client.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [bewaking en afstemming](concepts-monitoring.md) in Azure Database for PostgreSQL.

