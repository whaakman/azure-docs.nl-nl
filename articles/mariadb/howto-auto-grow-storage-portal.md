---
title: Automatisch vergroten opslag in Azure Database voor MariaDB met behulp van de Azure portal
description: Dit artikel wordt beschreven hoe u kunt automatische groei van opslag voor Azure Database voor MariaDB met behulp van Azure portal
author: ambhatna
ms.author: ambhatna
ms.service: mariadb
ms.topic: conceptual
ms.date: 5/29/2019
ms.openlocfilehash: bb3291b66776a5f0f6be16069b2d6a999b2d1f32
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676876"
---
# <a name="auto-grow-storage-in-azure-database-for-mariadb-using-the-azure-portal"></a>Automatisch vergroten opslag in Azure Database voor MariaDB met behulp van de Azure portal
Dit artikel wordt beschreven hoe u een Azure Database voor MariaDB serveropslag om te groeien zonder gevolgen voor de werkbelasting kunt configureren.

Wanneer een server van de toegewezen opslag bereikt, wordt de server wordt gemarkeerd als alleen-lezen. Als u inschakelt opslag automatisch vergroten, de opslag van de server wordt verhoogd om de groeiende gegevens mogelijk te maken. Voor servers met minder dan 100 GB ingerichte opslag, wordt de ingerichte opslaggrootte verhoogd met 5 GB zodra de vrije opslagruimte lager dan de hoogste waarde van 1 GB of 10% van de ingerichte opslag is. Voor servers met meer dan 100 GB van de ingerichte opslag, wordt de ingerichte opslaggrootte verhoogd van 5% wanneer de vrije opslagruimte lager dan 5% van de ingerichte opslaggrootte is. Maximale opslag beperkt als opgegeven [hier](https://docs.microsoft.com/azure/mariadb/concepts-pricing-tiers#storage) toepassen.

## <a name="prerequisites"></a>Vereisten
Voor deze handleiding, hebt u het volgende nodig:
- Een [Azure Database voor MariaDB-server](./quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="enable-storage-auto-grow"></a>Inschakelen van storage automatisch vergroten 

Volg deze stappen om in te stellen MariaDB server opslag automatisch vergroten:

1. In de [Azure-portal](https://portal.azure.com/), selecteer uw Azure Database voor MariaDB-server.

2. Op de pagina van de server MariaDB onder **instellingen** kop, klikt u op **prijscategorie** openen van de pagina met prijzen laag.

3. Selecteer in de sectie automatische groei **Ja** om in te schakelen opslag automatisch laten groeien.

    ![Azure Database voor MariaDB - Settings_Pricing_tier - Auto-groei](./media/howto-auto-grow-storage-portal/3-auto-grow.png)

4. Klik op **OK** om de wijzigingen op te slaan.

5. Een melding wordt bevestigen voor automatisch vergroten is ingeschakeld.

    ![Azure Database voor MariaDB - automatische groei geslaagd](./media/howto-auto-grow-storage-portal/5-auto-grow-successful.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [over het maken van waarschuwingen over metrische gegevens](howto-alert-metric.md).
