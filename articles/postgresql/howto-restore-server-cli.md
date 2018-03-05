---
title: Het back-up en herstellen van een server in Azure-Database voor PostgreSQL
description: Informatie over het back-up en herstellen van een server in Azure-Database voor PostgreSQL met behulp van de Azure CLI.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 69dfde7e54a271caabc6d0909565165fb219c7f2
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql-by-using-the-azure-cli"></a>Het back-up en herstellen van een server in Azure-Database voor PostgreSQL met behulp van de Azure CLI

Gebruik Azure voor PostgreSQL-Database naar een server-database herstellen naar een eerdere datum die uit 7 tot 35 dagen omvat.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze handleiding instructies, hebt u het volgende nodig:
- Een [Azure Database voor PostgreSQL-server en database](quickstart-create-server-database-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]


> [!IMPORTANT]
> Als u installeert en de Azure CLI lokaal gebruiken, is deze handleiding instructies vereist dat u Azure CLI versie 2.0 of hoger. Controleer de versie, bij de opdrachtprompt Azure CLI Voer `az --version`. Als u wilt installeren of upgraden, Zie [2.0 voor Azure CLI installeren]( /cli/azure/install-azure-cli).

## <a name="backup-happens-automatically"></a>Back-up automatisch wordt uitgevoerd
Wanneer u Azure-Database voor PostgreSQL, wordt de database-service automatisch een back-up van de service om de 5 minuten. 

Basic-laag zijn de back-ups beschikbaar voor 7 dagen. Standard-laag zijn de back-ups beschikbaar voor 35 dagen. Zie voor meer informatie [Azure Database voor PostgreSQL Prijscategorieën](concepts-pricing-tiers.md).

Met deze functie voor automatische back-up kunt u de server en de databases herstellen naar een eerdere datum of tijdstip voor herstel.

## <a name="restore-a-database-to-a-previous-point-in-time-by-using-the-azure-cli"></a>Een database naar een eerder tijdstip herstellen met behulp van de Azure CLI
Gebruik Azure Database voor PostgreSQL de server naar een eerder tijdstip herstellen. De herstelde gegevens gekopieerd naar een nieuwe server en de bestaande server is ongewijzigd worden gelaten. Bijvoorbeeld, als een tabel wordt per ongeluk verwijderd op twaalf uur 's middags vandaag de dag, kunt u herstellen op de tijd net vóór twaalf uur 's middags. Vervolgens kunt u de ontbrekende tabel en gegevens ophalen uit de herstelde kopie van de server. 

De Azure CLI gebruiken voor het herstellen van de server [az postgres server terugzetten](/cli/azure/postgres/server#az_postgres_server_restore) opdracht.

### <a name="run-the-restore-command"></a>Voer de opdracht herstellen

Voer de volgende opdracht voor het herstellen van de server bij de Azure CLI-opdrachtprompt:

```azurecli-interactive
az postgres server restore --resource-group myresourcegroup --server mydemoserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mydemoserver
```

De `az postgres server restore` opdracht worden de volgende parameters:
| Instelling | Voorgestelde waarde | Beschrijving  |
| --- | --- | --- |
| resource-group |  myResourceGroup |  De resourcegroep waar de bronserver bestaat.  |
| naam | mydemoserver-restored | De naam van de nieuwe server die door de opdracht restore wordt gemaakt. |
| herstel punt in tijd | 2017-04-13T13:59:00Z | Selecteer een punt in tijd om naar te herstellen. Deze datum en tijd moet binnen de back-up bewaarperiode van de bronserver. Gebruik de ISO8601-indeling voor datum en tijd. Bijvoorbeeld, kunt u uw eigen lokale tijdzone, zoals `2017-04-13T05:59:00-08:00`. U kunt ook de Zulu UTC-notatie, bijvoorbeeld gebruiken `2017-04-13T13:59:00Z`. |
| bron-server | mydemoserver | De naam of ID van de bronserver in om te herstellen. |

Wanneer u een server naar een eerder tijdstip herstellen, wordt een nieuwe server gemaakt. De oorspronkelijke server en de databases van het opgegeven punt in tijd worden gekopieerd naar de nieuwe server.

De locatie en de prijscategorie laag waarden voor de herstelde server blijven hetzelfde als de oorspronkelijke server. 

De `az postgres server restore` opdracht synchroon is. Nadat de server is hersteld, kunt u deze opnieuw aan het proces herhaalt voor een ander punt in tijd. 

Nadat het herstelproces is voltooid, zoek de nieuwe server en controleer of dat de gegevens is hersteld, zoals verwacht.

## <a name="next-steps"></a>Volgende stappen
[Verbindingsbibliotheken voor Azure-Database voor PostgreSQL](concepts-connection-libraries.md)
