---
title: Aanbevelingen in Azure Database for PostgreSQL voor prestaties
description: Dit artikel beschrijft de aanbevelingen voor prestaties een in Azure Database voor PostgreSQL krijgen kunt.
services: postgresql
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 25ab2f735cfd4b0870729cb86992665fa8984580
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46976373"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql"></a>Aanbevelingen in Azure Database for PostgreSQL voor prestaties

**Is van toepassing op:** Azure Database for PostgreSQL 9.6 en 10

> [!IMPORTANT]
> Aanbevelingen voor prestaties is in openbare Preview.

De functie voor aanbevelingen voor prestaties geeft de bovenste indexen die kunnen worden gemaakt in uw Azure Database for PostgreSQL-server om prestaties te verbeteren. Om te produceren indexaanbevelingen, wordt de functie rekening gehouden met verschillende kenmerken van de database, met inbegrip van het schema en de werkbelasting, zoals gerapporteerd door de Query Store. Na de implementatie van elke aanbeveling prestaties, moeten klanten prestaties voor het evalueren van de impact van deze wijzigingen testen. 

## <a name="permissions"></a>Machtigingen
**De eigenaar van** of **Inzender** machtigingen vereist voor het uitvoeren van analyses met de functie voor aanbevelingen voor prestaties.

## <a name="performance-recommendations"></a>Aanbevelingen voor prestaties
De [aanbevelingen voor prestaties](concepts-performance-recommendations.md) functie analyseert werkbelastingen op de server voor het identificeren van indexen met de mogelijkheid om prestaties te verbeteren.

Open **aanbevelingen voor prestaties** uit de **ondersteuning en probleemoplossing** sectie van de in de menubalk op de Azure portal-pagina voor uw PostgreSQL-server.

![Aanbevelingen voor prestaties landingspagina](./media/concepts-performance-recommendations/performance-recommendations-landing-page.png)

Selecteer **analyseren** en kiest u een database. Hiermee wordt begonnen met de analyse. Dit kan enkele minuten duren, afhankelijk van uw workload. Zodra de analyse is voltooid, wordt er een melding in de portal.

De **aanbevelingen voor prestaties** venster ziet u een lijst met aanbevelingen als deze zijn gevonden. Een aanbeveling ziet u informatie over de desbetreffende **Database**, **tabel**, **kolom**, en **indexgrootte**.

![Nieuwe pagina voor prestatie-aanbevelingen](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Voor het implementeren van de aanbeveling, Kopieer de querytekst en uitvoeren vanaf de client naar keuze.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [bewaking en afstemming](concepts-monitoring.md) in Azure Database for PostgreSQL.

