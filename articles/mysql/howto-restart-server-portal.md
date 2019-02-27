---
title: Opnieuw opstarten van Azure Database voor MySQL-server met behulp van Azure portal
description: Dit artikel wordt beschreven hoe u een Azure Database for MySQL-server met behulp van de Azure-Portal opnieuw kunt starten.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 2/7/2019
ms.openlocfilehash: 6cf6679dc6398b112ffc964f50986b2ab30aba47
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56882481"
---
# <a name="restart-azure-database-for-mysql-server-using-azure-portal"></a>Opnieuw opstarten van Azure Database voor MySQL-server met behulp van Azure portal
In dit onderwerp wordt beschreven hoe u een Azure Database for MySQL-server opnieuw kunt starten. Mogelijk moet u de server voor onderhoudsredenen, waardoor een korte onderbreking als de server de bewerking voert opnieuw.

De server opnieuw opstarten wordt geblokkeerd als de service bezet is. De service kan bijvoorbeeld een eerder gevraagde bewerking, zoals schalen vCores verwerkt.

De tijd die nodig is om te voltooien van opnieuw opstarten, is afhankelijk van het herstelproces voor MySQL. Als u wilt verlagen voor het opstarten, wordt u aangeraden dat als u de hoeveelheid activiteit die plaatsvindt op de server voorafgaand aan het opnieuw opstarten kan.

## <a name="prerequisites"></a>Vereisten
Voor deze handleiding, hebt u het volgende nodig:
- Een [Azure Database for MySQL-server en database](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Server opnieuw moet worden opgestart

De MySQL-server start opnieuw op de volgende stappen uit:

1. Selecteer uw Azure Database voor MySQL-server in de Azure-portal.

2. Op de werkbalk van de server **overzicht** pagina, klikt u op **opnieuw**.

   ![Azure Database for MySQL - overzicht - uit-knop](./media/howto-restart-server-portal/2-server.png)

3. Klik op **Ja** om te bevestigen van de server opnieuw wordt opgestart.

   ![Azure Database voor MySQL - opnieuw starten bevestigen](./media/howto-restart-server-portal/3-restart-confirm.png)

4. U ziet dat de status van de server is gewijzigd in 'Opnieuw opstarten'.

   ![Azure Database voor MySQL - status opnieuw starten](./media/howto-restart-server-portal/4-restarting-status.png)

5. Controleer of de server opnieuw opstarten is voltooid.

   ![Azure Database voor MySQL - opnieuw opstarten geslaagd](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Volgende stappen

[Snelstart: Azure Database for MySQL-server met behulp van Azure portal maken](./quickstart-create-mysql-server-database-using-azure-portal.md)