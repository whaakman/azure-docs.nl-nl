---
title: Het back-up en herstellen van een server in Azure-Database voor MySQL | Microsoft Docs
description: Informatie over het back-up en herstellen van een server in Azure-Database voor MySQL met behulp van de Azure CLI.
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 11/28/2017
ms.openlocfilehash: 44b3c68b8df4006d3fe087e5ad4118d7616d3d9a
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-mysql-by-using-the-azure-cli"></a>Het back-up en herstellen van een server in Azure-Database voor MySQL met behulp van de Azure CLI

Gebruik Azure voor MySQL-Database naar een server-database herstellen naar een eerdere datum die uit 7 tot 35 dagen omvat.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze handleiding instructies, hebt u het volgende nodig:
- Een [Azure-Database voor MySQL-server en database](quickstart-create-mysql-server-database-using-azure-portal.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Als u installeert en de Azure CLI lokaal gebruiken, is deze handleiding instructies vereist dat u Azure CLI versie 2.0 of hoger. Controleer de versie, bij de opdrachtprompt Azure CLI Voer `az --version`. Als u wilt installeren of upgraden, Zie [2.0 voor Azure CLI installeren]( /cli/azure/install-azure-cli).

## <a name="backup-happens-automatically"></a>Back-up automatisch wordt uitgevoerd
Wanneer u Azure-Database voor MySQL, wordt de database-service automatisch een back-up van de service om de 5 minuten. 

Basic-laag zijn de back-ups beschikbaar voor 7 dagen. Standard-laag zijn de back-ups beschikbaar voor 35 dagen. Zie voor meer informatie [Azure Database voor MySQL Prijscategorieën](concepts-service-tiers.md).

Met deze functie voor automatische back-up kunt u de server en de databases herstellen naar een eerdere datum of tijdstip voor herstel.

## <a name="restore-a-database-to-a-previous-point-in-time-by-using-the-azure-cli"></a>Een database naar een eerder tijdstip herstellen met behulp van de Azure CLI
Gebruik Azure voor MySQL-Database naar de server naar een eerder tijdstip herstellen. De herstelde gegevens gekopieerd naar een nieuwe server en de bestaande server is ongewijzigd worden gelaten. Bijvoorbeeld, als een tabel wordt per ongeluk verwijderd op twaalf uur 's middags vandaag de dag, kunt u herstellen op de tijd net vóór twaalf uur 's middags. Vervolgens kunt u de ontbrekende tabel en gegevens ophalen uit de herstelde kopie van de server. 

De Azure CLI gebruiken voor het herstellen van de server [az mysql server terugzetten](/cli/azure/mysql/server#az_mysql_server_restore) opdracht.

### <a name="run-the-restore-command"></a>Voer de opdracht herstellen

Voer de volgende opdracht voor het herstellen van de server bij de Azure CLI-opdrachtprompt:

```azurecli-interactive
az mysql server restore --resource-group myResourceGroup --name myserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server myserver4demo
```

De `az mysql server restore` opdracht worden de volgende parameters:
| Instelling | Voorgestelde waarde | Beschrijving  |
| --- | --- | --- |
| resourcegroep | myResourceGroup |  De resourcegroep waar de bronserver bestaat.  |
| naam | MijnServer hersteld | De naam van de nieuwe server die door de opdracht restore wordt gemaakt. |
| herstel punt in tijd | 2017-04-13T13:59:00Z | Selecteer een punt in tijd om naar te herstellen. Deze datum en tijd moet binnen de back-up bewaarperiode van de bronserver. Gebruik de ISO8601-indeling voor datum en tijd. Bijvoorbeeld, kunt u uw eigen lokale tijdzone, zoals `2017-04-13T05:59:00-08:00`. U kunt ook de Zulu UTC-notatie, bijvoorbeeld gebruiken `2017-04-13T13:59:00Z`. |
| bron-server | myserver4demo | De naam of ID van de bronserver in om te herstellen. |

Wanneer u een server naar een eerder tijdstip herstellen, wordt een nieuwe server gemaakt. De oorspronkelijke server en de databases van het opgegeven punt in tijd worden gekopieerd naar de nieuwe server.

De locatie en de prijscategorie laag waarden voor de herstelde server blijven hetzelfde als de oorspronkelijke server. 

De `az mysql server restore` opdracht synchroon is. Nadat de server is hersteld, kunt u deze opnieuw aan het proces herhaalt voor een ander punt in tijd. 

Nadat het herstelproces is voltooid, zoek de nieuwe server en controleer of dat de gegevens is hersteld, zoals verwacht.

## <a name="next-steps"></a>Volgende stappen
[Verbindingsbibliotheken voor Azure-Database voor MySQL](concepts-connection-libraries.md)
