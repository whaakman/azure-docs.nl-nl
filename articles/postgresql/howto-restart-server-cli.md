---
title: Opnieuw opstarten van Azure Database for PostgreSQL-server met behulp van Azure CLI
description: Dit artikel wordt beschreven hoe u een Azure Database for PostgreSQL-server met de Azure CLI opnieuw kunt starten
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 3/28/2019
ms.openlocfilehash: 51b3011c040db8576c13868f9fac26cb1e431515
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58623286"
---
# <a name="restart-azure-database-for-postgresql-server-using-the-azure-cli"></a>Opnieuw opstarten van Azure Database for PostgreSQL-server met de Azure CLI
In dit onderwerp wordt beschreven hoe u een Azure Database for PostgreSQL-server opnieuw kunt starten. Mogelijk moet u de server voor onderhoudsredenen, waardoor een korte onderbreking als de server de bewerking voert opnieuw.

De server opnieuw opstarten wordt geblokkeerd als de service bezet is. De service kan bijvoorbeeld een eerder gevraagde bewerking, zoals schalen vCores verwerkt.
 
De tijd die nodig is om te voltooien van opnieuw opstarten, is afhankelijk van het herstelproces PostgreSQL. Als u wilt verlagen voor het opstarten, wordt u aangeraden dat als u de hoeveelheid activiteit die plaatsvindt op de server voorafgaand aan het opnieuw opstarten kan.

## <a name="prerequisites"></a>Vereisten
Voor deze handleiding, hebt u het volgende nodig:
- Een [Azure Database for PostgreSQL-server](quickstart-create-server-up-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> In deze gebruiksaanwijzing vereist het gebruik van Azure CLI versie 2.0 of hoger. Als u wilt controleren welke versie, bij de Azure CLI-opdrachtprompt, voer `az --version`. Als u wilt installeren of upgraden, Zie [Azure CLI installeren]( /cli/azure/install-azure-cli).


## <a name="restart-the-server"></a>Start de server opnieuw

Start opnieuw op de server met de volgende opdracht:

```azurecli-interactive
az postgres server restart --name mydemoserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [hoe u parameters instelt in Azure Database for PostgreSQL](howto-configure-server-parameters-using-cli.md)