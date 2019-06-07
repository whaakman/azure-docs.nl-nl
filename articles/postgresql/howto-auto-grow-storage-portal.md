---
title: Automatisch vergroten opslag met behulp van de Azure-portal in Azure Database voor PostgreSQL - één Server
description: Dit artikel wordt beschreven hoe u automatisch kunt inschakelen groeien storage met behulp van de Azure-portal in Azure Database voor PostegreSQL - één Server
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/29/2019
ms.openlocfilehash: 9f88fe3e8a30dd80c5331bd6c8ea7aec401c6fb9
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676756"
---
# <a name="auto-grow-storage-using-the-azure-portal-in-azure-database-for-postgresql---single-server"></a>Automatisch vergroten opslag met behulp van de Azure-portal in Azure Database voor PostgreSQL - één Server
Dit artikel wordt beschreven hoe u een Azure Database for PostgreSQL-server-opslag om te groeien zonder gevolgen voor de werkbelasting kunt configureren.

Wanneer een server van de toegewezen opslag bereikt, wordt de server wordt gemarkeerd als alleen-lezen. Als u inschakelt opslag automatisch vergroten, de opslag van de server wordt verhoogd om de groeiende gegevens mogelijk te maken. Voor servers met minder dan 100 GB ingerichte opslag, wordt de ingerichte opslaggrootte verhoogd met 5 GB zodra de vrije opslagruimte lager dan de hoogste waarde van 1 GB of 10% van de ingerichte opslag is. Voor servers met meer dan 100 GB van de ingerichte opslag, wordt de ingerichte opslaggrootte verhoogd van 5% wanneer de vrije opslagruimte lager dan 5% van de ingerichte opslaggrootte is. Maximale opslag beperkt als opgegeven [hier](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers#storage) toepassen.

## <a name="prerequisites"></a>Vereisten
Voor deze handleiding, hebt u het volgende nodig:
- Een [Azure Database for PostgreSQL-server](quickstart-create-server-database-portal.md)

## <a name="enable-storage-auto-grow"></a>Inschakelen van storage automatisch vergroten 

Volg deze stappen voor het instellen van de PostgreSQL server opslag automatisch vergroten:

1. In de [Azure-portal](https://portal.azure.com/), selecteer uw Azure Database for PostgreSQL-server.

2. Klik op de pagina van de PostgreSQL-server onder **instellingen**, klikt u op **prijscategorie** openen van de pagina met prijzen laag.

3. In de **automatische groei** sectie, selecteer **Ja** om in te schakelen opslag automatisch laten groeien.

    ![Azure Database for PostgreSQL - Settings_Pricing_tier - Auto-groei](./media/howto-auto-grow-storage-portal/3-auto-grow.png)

4. Klik op **OK** om de wijzigingen op te slaan.

5. Een melding wordt bevestigen voor automatisch vergroten is ingeschakeld.

    ![Azure Database voor PostgreSQL - automatische groei geslaagd](./media/howto-auto-grow-storage-portal/5-auto-grow-successful.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [over het maken van waarschuwingen over metrische gegevens](howto-alert-on-metric.md).
