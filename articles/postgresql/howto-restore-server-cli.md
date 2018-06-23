---
title: Het back-up en herstellen van een server in Azure-Database voor PostgreSQL | Microsoft Docs
description: Informatie over het back-up en herstellen van een server in Azure-Database voor PostgreSQL met behulp van de Azure CLI.
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 04/01/2018
ms.openlocfilehash: 4e745a5de8000e0f26491c9f4f236f7f8a735ae9
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36319638"
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-postgresql-using-the-azure-cli"></a>Het back-up en herstellen van een server in Azure-Database voor PostgreSQL met de Azure CLI

## <a name="backup-happens-automatically"></a>Back-up automatisch wordt uitgevoerd
Azure-Database voor PostgreSQL-servers zijn back-up periodiek Restore-functies inschakelen. Gebruik van deze functie kunt u de server en alle bijbehorende databases herstellen naar een eerdere point-in-time, op een nieuwe server.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze handleiding instructies, hebt u het volgende nodig:
- Een [Azure Database voor PostgreSQL-server en database](quickstart-create-server-database-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> Deze handleiding instructies vereist het gebruik van Azure CLI versie 2.0 of hoger. Controleer de versie, bij de opdrachtprompt Azure CLI Voer `az --version`. Als u wilt installeren of upgraden, Zie [2.0 voor Azure CLI installeren]( /cli/azure/install-azure-cli).

## <a name="set-backup-configuration"></a>Back-upconfiguratie instellen

U de keuze tussen uw server configureren voor lokaal redundante back-ups of geografisch redundante back-ups op de server is gemaakt. 

> [!NOTE]
> Nadat een server is gemaakt, wordt het soort redundantie dat zo is, kan geografisch redundante tegenover lokaal redundant kan niet worden overgeschakeld.
>

Tijdens het maken van een server via de `az postgres server create` opdracht, de `--geo-redundant-backup` parameter besluit uw back-up redundantie-optie. Als `Enabled`, geografisch redundante back-ups worden gemaakt. Of als `Disabled` lokaal redundante back-ups worden gemaakt. 

De back-up bewaarperiode is ingesteld door de parameter `--backup-retention-days`. 

Zie voor meer informatie over het instellen van deze waarden tijdens het maken, de [Azure-Database voor PostgreSQL server CLI Quick Start](quickstart-create-server-database-azure-cli.md).

De bewaarperiode voor back-up van een server kan als volgt worden gewijzigd:

```azurecli-interactive
az postgres server update --name mydemoserver --resource-group myresourcegroup --backup-retention-days 10
```

Het vorige voorbeeld verandert de back-up bewaarperiode van mydemoserver in 10 dagen.

De back-up bewaarperiode bepaalt hoe ver terug in tijd die een punt in tijd terugzetbewerking kan worden opgehaald, omdat deze gebaseerd op de back-ups beschikbaar. Punt in tijd restore wordt verder beschreven in de volgende sectie.

## <a name="server-point-in-time-restore"></a>Punt in tijd herstellen van de server
U kunt de server naar een eerder tijdstip herstellen. De herstelde gegevens gekopieerd naar een nieuwe server en de bestaande server is ongewijzigd worden gelaten. Bijvoorbeeld, als een tabel wordt per ongeluk verwijderd op twaalf uur 's middags vandaag de dag, kunt u herstellen op de tijd net vóór twaalf uur 's middags. Vervolgens kunt u de ontbrekende tabel en gegevens ophalen uit de herstelde kopie van de server. 

De Azure CLI gebruiken voor het herstellen van de server [az postgres server terugzetten](/cli/azure/postgres/server#az_postgres_server_restore) opdracht.

### <a name="run-the-restore-command"></a>Voer de opdracht herstellen

Voer de volgende opdracht voor het herstellen van de server bij de Azure CLI-opdrachtprompt:

```azurecli-interactive
az postgres server restore --resource-group myresourcegroup --name mydemoserver-restored --restore-point-in-time 2018-03-13T13:59:00Z --source-server mydemoserver
```

De `az postgres server restore` opdracht worden de volgende parameters:
| Instelling | Voorgestelde waarde | Beschrijving  |
| --- | --- | --- |
| resource-group |  myResourceGroup |  De resourcegroep waar de bronserver bestaat.  |
| naam | mydemoserver-restored | De naam van de nieuwe server die door de opdracht restore is gemaakt. |
| restore-point-in-time | 2018-03-13T13:59:00Z | Selecteer een punt in tijd om naar te herstellen. Deze datum en tijd moet binnen de back-upretentieperiode van de bronserver vallen. Gebruik de ISO8601-indeling voor datum en tijd. Bijvoorbeeld, kunt u uw eigen lokale tijdzone, zoals `2018-03-13T05:59:00-08:00`. U kunt ook de Zulu UTC-notatie, bijvoorbeeld gebruiken `2018-03-13T13:59:00Z`. |
| source-server | mydemoserver | De naam of ID van de bronserver voor het herstellen. |

Wanneer u een server naar een eerder tijdstip herstellen, wordt een nieuwe server gemaakt. De oorspronkelijke server en de databases van het opgegeven punt in tijd worden gekopieerd naar de nieuwe server.

De locatie en de prijscategorie laag waarden voor de herstelde server blijven hetzelfde als de oorspronkelijke server. 

Nadat het herstelproces is voltooid, zoek de nieuwe server en controleer of dat de gegevens is hersteld, zoals verwacht.

## <a name="geo-restore"></a>Geo-herstel
Als u uw server voor geografisch redundante back-ups geconfigureerd, kan een nieuwe server worden gemaakt vanuit de back-up van een bestaande server. Deze nieuwe server kan worden gemaakt in elke regio dat Azure-Database voor PostgreSQL beschikbaar is.  

Gebruik voor het maken van een server die geografisch redundante back-up met de Azure CLI `az postgres server georestore` opdracht.

> [!NOTE]
> Wanneer een server voor het eerst wordt gemaakt kunt deze mogelijk niet onmiddellijk beschikbaar voor de geo-herstel. Het duurt enkele uren voor de benodigde metagegevens worden ingevuld.
>

Voer de volgende opdracht op geo terugzetten van de server bij de Azure CLI-opdrachtprompt:

```azurecli-interactive
az postgres server georestore --resource-group myresourcegroup --name mydemoserver-georestored --source-server mydemoserver --location eastus --sku-name GP_Gen4_8 
```
Deze opdracht maakt u een nieuwe server met de naam *mydemoserver georestored* in VS-Oost die tot behoren *myresourcegroup*. Het is een voor algemene doeleinden, Gen 4-server met 8 vCores. De server wordt gemaakt van de geografisch redundante back-up van *mydemoserver*, dit is ook in de resourcegroep *myresourcegroup*

Als u maken van de nieuwe server in een andere resourcegroep van de bestaande server vervolgens wilt in de `--source-server` parameter spitst u de naam van de server zoals in het volgende voorbeeld:

```azurecli-interactive
az postgres server georestore --resource-group newresourcegroup --name mydemoserver-georestored --source-server "/subscriptions/$<subscription ID>/resourceGroups/$<resource group ID>/providers/Microsoft.DBforPostgreSQL/servers/mydemoserver" --location eastus --sku-name GP_Gen4_8

```

De `az postgres server georestore` opdracht requies de volgende parameters:
| Instelling | Voorgestelde waarde | Beschrijving  |
| --- | --- | --- |
|resource-group| myResourceGroup | De naam van de resourcegroep de nieuwe server deel van uitmaakt.|
|naam | mydemoserver georestored | De naam van de nieuwe server. |
|source-server | mydemoserver | De naam van de bestaande server waarvan back-ups redundante geo worden gebruikt. |
|locatie | eastus | De locatie van de nieuwe server. |
|SKU-naam| GP_Gen4_8 | Deze parameter stelt de prijscategorie laag, compute genereren en aantal vCores van de nieuwe server. GP_Gen4_8 toegewezen aan een voor algemene doeleinden, Gen 4-server met 8 vCores.|


>[!Important]
>Wanneer u een nieuwe server geo herstellen, neemt de dezelfde opslaggrootte en prijscategorie als de bronserver. Deze waarden kunnen niet worden gewijzigd tijdens het maken van. Nadat de nieuwe server is gemaakt, kan de opslagruimte worden uitgebreid.

Nadat het herstelproces is voltooid, zoek de nieuwe server en controleer of dat de gegevens is hersteld, zoals verwacht.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de service [back-ups](concepts-backup.md).
- Meer informatie over [bedrijfscontinuïteit](concepts-business-continuity.md) opties.
