---
title: 'Snelstart: een Azure Database for MySQL met behulp van een eenvoudige Azure CLI-opdracht - az mysql van (preview) maken'
description: Quick Start voor het maken van Azure Database voor MySQL-server met behulp van Azure CLI (opdrachtregelinterface) van de opdracht.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 3/18/2019
ms.custom: mvc
ms.openlocfilehash: aa0d2a9e990faa8d99355744824f34e26aeb519e
ms.sourcegitcommit: f68b0e128f0478444740172f54e92b453df696be
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58137909"
---
# <a name="quickstart-create-an-azure-database-for-mysql-using-a-simple-azure-cli-command---az-mysql-up-preview"></a>Quickstart: Maak een Azure Database voor MySQL met behulp van een eenvoudige Azure CLI-opdracht - az mysql van (preview)

> [!IMPORTANT]
> De [az mysql van](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-up) Azure CLI-opdracht is beschikbaar als preview.

Azure Database voor MySQL is een beheerde service waarmee u MySQL-databases met hoge beschikbaarheid in de cloud kunt uitvoeren, beheren en schalen. De Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Deze snelstartgids leest u hoe u de [az mysql van](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-up) opdracht voor het maken van een Azure Database for MySQL-server met de Azure CLI. Naast het maken van de server, de `az mysql up` opdracht maakt u een voorbeelddatabase, een hoofdgebruiker zijn in de database, opent u de firewall voor Azure-services en maakt standaard firewall-regels voor de clientcomputer. Hiermee kunt u het ontwikkelproces versnellen.

## <a name="prerequisites"></a>Vereisten

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

In dit artikel is vereist dat u van Azure CLI versie 2.0 of hoger uitvoert lokaal. Voer de opdracht `az --version` uit om de geïnstalleerde versie te zien. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

U moet zich aanmelden bij uw account met de [az login](/cli/azure/authenticate-azure-cli?view=interactive-log-in) opdracht. Let op de **id**-eigenschap van de opdrachtuitvoer voor de naam van het desbetreffende abonnement.

```azurecli
az login
```

Als u meerdere abonnementen hebt, kiest u het juiste abonnement waarin de resource moet worden gefactureerd. Selecteer de specifieke abonnements-id in uw account met de opdracht [az account set](/cli/azure/account). Vervang de **abonnements-id** eigenschap uit de **az login** uitvoer voor uw abonnement in de tijdelijke aanduiding voor abonnement-id.

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-an-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken

Voor het gebruik van de opdrachten, installeert de [db-up](/cli/azure/ext/db-up) extensie. Als een fout wordt geretourneerd, controleert u of dat u de nieuwste versie van de Azure CLI hebt geïnstalleerd. Zie [Azure CLI installeren](/cli/azure/install-azure-cli).

```azurecli
az extension add --name db-up
```

Maak een Azure Database for MySQL-server met de volgende opdracht:

```azurecli
az mysql up
```

De server is gemaakt met de volgende standaardwaarden (tenzij u handmatig deze overschrijft):

**Instelling** | **Standaardwaarde** | **Beschrijving**
---|---|---
servernaam | Door het systeem gegenereerde | Een unieke naam ter identificatie van de Azure Database voor MySQL-server.
resource-group | Door het systeem gegenereerde | Een nieuwe Azure-resourcegroep.
sku-name | GP_Gen5_2 | De naam van de SKU. Volgt de verkorte notatie voor conventie {prijscategorie}\_{compute-generatie}\_{vCores}. De standaardwaarde is een Gen5 voor algemeen gebruik-server met 2 vCores. Zie onze [pagina met prijzen](https://azure.microsoft.com/pricing/details/mysql/) voor meer informatie over de lagen.
backup-retention | 7 | Hoe lang een back-up moet worden bewaard. De eenheid is dagen.
geo-redundant-backup | Uitgeschakeld | Of geografisch redundante back-ups moeten worden ingeschakeld voor deze server.
location | westus2 | De Azure-locatie voor de server.
ssl-enforcement | Uitgeschakeld | Of ssl moet worden ingeschakeld voor deze server.
storage-size | 5120 | De opslagcapaciteit van de server (eenheid is MB).
versie | 5.7 | De hoofdversie van MySQL.
admin-user | Door het systeem gegenereerde | De gebruikersnaam voor aanmelding als beheerder.
admin-password | Door het systeem gegenereerde | Het wachtwoord van het beheerdersaccount.

> [!NOTE]
> Voor meer informatie over de `az mysql up` opdracht en de aanvullende parameters opgeeft, Zie de [documentatie van Azure CLI](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-up).

Als uw server is gemaakt, beschikt u met de volgende instellingen:

- Een firewall-regel met de naam 'devbox' wordt gemaakt. De Azure CLI voor het detecteren van het IP-adres van de machine probeert de `az mysql up` opdracht uitvoeren vanaf en witte lijsten dat IP-adres is.
- 'Toegang tot Azure-services toestaan' is ingesteld op aan. Deze instelling configureert u de firewall van de server om clientverbindingen van alle Azure-resources, met inbegrip van resources in uw abonnement niet te accepteren.
- De `wait_timeout` parameter is ingesteld op 8 uur
- Een lege database met de naam 'sampledb' wordt gemaakt
- Een nieuwe gebruiker met de naam "root" met machtigingen om 'sampledb' wordt gemaakt

> [!NOTE]
> Azure Database voor MySQL communiceren via poort 3306. Wanneer u verbinding maakt vanuit een bedrijfsnetwerk, is uitgaand verkeer via poort 3306 mogelijk niet toegestaan door de firewall van uw netwerk. Uw IT-afdeling poort 3306 verbinding maken met uw server openen hebben.

## <a name="get-the-connection-information"></a>De verbindingsgegevens ophalen

Na de `az mysql up` opdracht is voltooid, wordt een lijst met tekenreeksen voor databaseverbindingen voor populaire programmeertalen geretourneerd naar u. Deze tekenreeksen voor databaseverbindingen zijn vooraf geconfigureerd met de specifieke kenmerken van uw zojuist gemaakte Azure-Database voor MySQL-server.

U kunt de [az mysql show-connection-string](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-show-connection-string) opdracht om deze tekenreeksen voor databaseverbindingen opnieuw weer te geven.

## <a name="clean-up-resources"></a>Resources opschonen

Opschonen van alle resources die u hebt gemaakt in de Quick Start met de volgende opdracht. Met deze opdracht verwijdert u de Azure Database for MySQL-server en de resourcegroep.

```azurecli
az mysql down --delete-group
```

Als u alleen de zojuist gemaakte server verwijderen wilt, kunt u uitvoeren [az mysql omlaag](/cli/azure/ext/db-up/mysql#ext-db-up-az-mysql-down) opdracht.

```azurecli
az mysql down
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een MySQL-database ontwerpen met Azure CLI](./tutorial-design-database-using-cli.md)
