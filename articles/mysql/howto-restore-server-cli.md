---
title: Het back-up en herstellen van een server in Azure Database for MySQL
description: Leer hoe u back-up en herstellen van een server in Azure Database for MySQL met behulp van de Azure CLI.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: article
ms.date: 04/01/2018
ms.openlocfilehash: f4253d4259d66b0c5746ef61cfc3cf4f4f2caad3
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39448900"
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-mysql-using-the-azure-cli"></a>Het back-up en herstellen van een server in Azure Database voor MySQL met behulp van de Azure CLI

## <a name="backup-happens-automatically"></a>Back-up wordt automatisch uitgevoerd
Azure Database for MySQL-servers voor back-ups regelmatig Restore-functies inschakelen. Met deze functie kan u de server en alle bijbehorende databases herstellen naar een eerder punt-in-time, op een nieuwe server.

## <a name="prerequisites"></a>Vereisten
Voor deze handleiding, hebt u het volgende nodig:
- Een [Azure Database for MySQL-server en database](quickstart-create-mysql-server-database-using-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> In deze gebruiksaanwijzing vereist het gebruik van Azure CLI versie 2.0 of hoger. Als u wilt controleren welke versie, bij de Azure CLI-opdrachtprompt, voer `az --version`. Als u wilt installeren of upgraden, Zie [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="set-backup-configuration"></a>Configuratie van de back-up

U de keuze tussen het configureren van uw server voor back-ups van lokaal redundante of geografisch redundante back-ups op de server is gemaakt. 

> [!NOTE]
> Nadat een server is gemaakt, het soort redundantie dat zo is, kan geografisch redundant en lokaal redundant, kan niet worden overgeschakeld.
>

Tijdens het maken van een server via de `az mysql server create` opdracht, de `--geo-redundant-backup` parameter besluit uw back-up-optie voor redundantie. Als `Enabled`, geografisch redundante back-ups worden gemaakt. Of als `Disabled` lokaal redundante back-ups worden gemaakt. 

De bewaarperiode voor back-up is ingesteld door de parameter `--backup-retention`. 

Zie voor meer informatie over het instellen van deze waarden tijdens het maken, de [Azure Database for MySQL-server de Snelstartgids CLI](quickstart-create-mysql-server-database-using-azure-cli.md).

De bewaarperiode voor back-up van een server kan als volgt worden gewijzigd:

```azurecli-interactive
az mysql server update --name mydemoserver --resource-group myresourcegroup --backup-retention 10
```

Het vorige voorbeeld wijzigt de bewaarperiode voor back-up van mydemoserver en 10 dagen.

De bewaarperiode voor back-up bepaalt hoe ver terug in de tijd die een point-in-time-restore kan worden opgehaald, omdat deze gebaseerd op de back-ups beschikbaar. Point-in-time-restore is beschreven in de volgende sectie.

## <a name="server-point-in-time-restore"></a>Herstellen van de server point-in-time
De server kunt u herstellen naar een eerder tijdstip. De herstelde gegevens worden gekopieerd naar een nieuwe server en de bestaande server is, blijven ongewijzigd. Bijvoorbeeld, als een tabel per ongeluk is verwijderd op twaalf uur 's middags vandaag, kunt u herstellen tot het moment waarop net vóór twaalf uur 's middags. U kunt vervolgens de ontbrekende tabel en de gegevens ophalen uit de herstelde kopie van de server. 

De Azure CLI gebruiken voor het herstellen van de server, [az mysql server restore](/cli/azure/mysql/server#az-mysql-server-restore) opdracht.

### <a name="run-the-restore-command"></a>Voer de opdracht herstellen

Voer de volgende opdracht voor het herstellen van de server, op de Azure CLI-opdrachtprompt:

```azurecli-interactive
az mysql server restore --resource-group myresourcegroup --name mydemoserver-restored --restore-point-in-time 2018-03-13T13:59:00Z --source-server mydemoserver
```

De `az mysql server restore` opdracht moet de volgende parameters:
| Instelling | Voorgestelde waarde | Beschrijving  |
| --- | --- | --- |
| resource-group |  myResourceGroup |  De resourcegroep waarin de bronserver bestaat.  |
| naam | mydemoserver-restored | De naam van de nieuwe server die door de opdracht restore is gemaakt. |
| restore-point-in-time | 2018-03-13T13:59:00Z | Selecteer een punt in tijd om naar te herstellen. Deze datum en tijd moet binnen de back-upretentieperiode van de bronserver vallen. Gebruik de ISO8601 datum en tijd-indeling. Bijvoorbeeld, kunt u uw eigen lokale tijdzone, zoals `2018-03-13T05:59:00-08:00`. U kunt ook de UTC Zulu-notatie, bijvoorbeeld gebruiken `2018-03-13T13:59:00Z`. |
| source-server | mydemoserver | De naam of ID van de bronserver voor het herstellen. |

Wanneer u een server naar een eerder tijdstip herstelt, wordt een nieuwe server gemaakt. De oorspronkelijke server en de bijbehorende databases uit het opgegeven tijdstip worden gekopieerd naar de nieuwe server.

De locatie en prijscategorie van de herstelde server blijven gelijk zijn aan de oorspronkelijke server. 

Nadat het herstelproces is voltooid, Ga naar de nieuwe server en controleer of dat de gegevens is hersteld zoals verwacht.

## <a name="geo-restore"></a>Geo-herstel
Als u uw server voor geografisch redundante back-ups hebt geconfigureerd, kan een nieuwe server worden gemaakt van de back-up van de bestaande server. Deze nieuwe server kan worden gemaakt in andere regio's dat Azure Database for MySQL beschikbaar is.  

De Azure CLI gebruiken voor het maken van een server met behulp van een redundante back-up voor geografische `az mysql server georestore` opdracht.

> [!NOTE]
> Als u een server maakt eerst kan het niet onmiddellijk beschikbaar voor geo-herstel zijn. Het duurt een paar uur voor de metagegevens die nodig zijn om te worden ingevuld.
>

Voer de volgende opdracht voor geo-herstel de-server, op de Azure CLI-opdrachtprompt:

```azurecli-interactive
az mysql server georestore --resource-group myresourcegroup --name mydemoserver-georestored --source-server mydemoserver --location eastus --sku-name GP_Gen4_8 
```
Deze opdracht maakt u een nieuwe server met de naam *mydemoserver georestored* in VS-Oost die tot behoren *myresourcegroup*. Het is een algemeen gebruik, Gen 4-server met 8 vCores. De server is gemaakt op basis van de geografisch redundante back-up van *mydemoserver*, die ook in de resourcegroep is *myresourcegroup*

Als u maken van de nieuwe server in een andere resourcegroep van de bestaande server, klikt u vervolgens wilt de `--source-server` parameter zou u in aanmerking komt de servernaam zoals in het volgende voorbeeld:

```azurecli-interactive
az mysql server georestore --resource-group newresourcegroup --name mydemoserver-georestored --source-server "/subscriptions/$<subscription ID>/resourceGroups/$<resource group ID>/providers/Microsoft.DBforMySQL/servers/mydemoserver" --location eastus --sku-name GP_Gen4_8

```

De `az mysql server georestore` opdracht requies de volgende parameters:
| Instelling | Voorgestelde waarde | Beschrijving  |
| --- | --- | --- |
|resource-group| myResourceGroup | De naam van de resourcegroep waarin de nieuwe server deel van uitmaakt.|
|naam | mydemoserver-georestored | De naam van de nieuwe server. |
|source-server | mydemoserver | De naam van de bestaande server waarvan back-ups redundante geo worden gebruikt. |
|location | eastus | De locatie van de nieuwe server. |
|SKU-naam| GP_Gen4_8 | Deze parameter stelt de prijzen laag, bewerking voor compute en het aantal vCores van de nieuwe server. GP_Gen4_8 toegewezen aan een algemeen gebruik, Gen 4-server met 8 vCores.|


>[!Important]
>Wanneer het maken van een nieuwe server door een geo-herstel, neemt de dezelfde opslaggrootte en de prijscategorie als de bronserver. Deze waarden kunnen niet worden gewijzigd tijdens het maken van. Nadat de nieuwe server is gemaakt, kan de opslaggrootte worden opgeschaald.

Nadat het herstelproces is voltooid, Ga naar de nieuwe server en controleer of dat de gegevens is hersteld zoals verwacht.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over van de service [back-ups](concepts-backup.md).
- Meer informatie over [bedrijfscontinuïteit](concepts-business-continuity.md) opties.
