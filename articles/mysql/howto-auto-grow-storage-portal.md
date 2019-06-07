---
title: Automatisch vergroten opslag in Azure Database voor MySQL met behulp van de Azure-portal
description: Dit artikel wordt beschreven hoe u automatisch kunt inschakelen toenemen opslag voor Azure Database voor MySQL met behulp van Azure portal
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: conceptual
ms.date: 5/29/2019
ms.openlocfilehash: 5343475f38dd5389d6b0e266ff7167925cd38d71
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676786"
---
# <a name="auto-grow-storage-in-azure-database-for-mysql-using-the-azure-portal"></a>Automatisch vergroten opslag in Azure Database voor MySQL met behulp van de Azure-portal
Dit artikel wordt beschreven hoe u een Azure Database for MySQL-server-opslag om te groeien zonder gevolgen voor de werkbelasting kunt configureren.

Wanneer een server van de toegewezen opslag bereikt, wordt de server wordt gemarkeerd als alleen-lezen. Als u inschakelt opslag automatisch vergroten, de opslag van de server wordt verhoogd om de groeiende gegevens mogelijk te maken. Voor servers met minder dan 100 GB ingerichte opslag, wordt de ingerichte opslaggrootte verhoogd met 5 GB zodra de vrije opslagruimte lager dan de hoogste waarde van 1 GB of 10% van de ingerichte opslag is. Voor servers met meer dan 100 GB van de ingerichte opslag, wordt de ingerichte opslaggrootte verhoogd van 5% wanneer de vrije opslagruimte lager dan 5% van de ingerichte opslaggrootte is. Maximale opslag beperkt als opgegeven [hier](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers#storage) toepassen.

## <a name="prerequisites"></a>Vereisten
Voor deze handleiding, hebt u het volgende nodig:
- Een [Azure Database for MySQL-server](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="enable-storage-auto-grow"></a>Inschakelen van storage automatisch vergroten 

Volg deze stappen om in te stellen van MySQL server opslag automatisch vergroten:

1. In de [Azure-portal](https://portal.azure.com/), selecteert u uw Azure Database voor MySQL-server.

2. Klik op de pagina van de MySQL-server onder **instellingen** kop, klikt u op **prijscategorie** om de prijzen laag-pagina te openen.

3. Selecteer in de sectie automatische groei **Ja** om in te schakelen opslag automatisch laten groeien.

    ![Azure Database for MySQL - Settings_Pricing_tier - Auto-groei](./media/howto-auto-grow-storage-portal/3-auto-grow.png)

4. Klik op **OK** om de wijzigingen op te slaan.

5. Een melding wordt bevestigen voor automatisch vergroten is ingeschakeld.

    ![Azure Database voor MySQL - automatische groei geslaagd](./media/howto-auto-grow-storage-portal/5-auto-grow-success.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [over het maken van waarschuwingen over metrische gegevens](howto-alert-on-metric.md).
