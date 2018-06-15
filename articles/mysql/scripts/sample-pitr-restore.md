---
title: 'Azure CLI-script: een Azure Database for MySQL-server herstellen naar een eerder tijdstip'
description: Dit CLI-voorbeeldscript herstelt een Azure Database for MySQL-server naar een eerder tijdstip.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 02/28/2018
ms.openlocfilehash: bea3a8057e9b99dc5847e23541d41c5c68393d48
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266208"
---
# <a name="restore-an-azure-database-for-mysql-server-using-azure-cli"></a>Een Azure Database for MySQL-server herstellen met behulp van Azure CLI
Met dit CLI-voorbeeldscript wordt één Azure Database for MySQL-server hersteld naar een eerder tijdstip.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit voorbeeld gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeldscript
Wijzig in dit voorbeeldscript de gemarkeerde regels om de gebruikersnaam en het wachtwoord van de beheerder aan te passen. Vervang de abonnement-ID die wordt gebruikt in de az monitor opdrachten door uw eigen abonnement-ID.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/backup-restore-pitr/backup-restore.sh?highlight=18-19 "Restore Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/backup-restore-pitr/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Uitleg van het script
In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| **Opdracht** | **Opmerkingen** |
|---|---|
| [az group create](/cli/azure/group#create) | Hiermee maakt u een resourcegroep waarin alle resources worden opgeslagen. |
| [az mysql server create](/cli/azure/mysql/server#create) | Hiermee wordt een MySQL-server gemaakt waar de databases worden gehost. |
| [az mysql server restore](/cli/azure/mysql/server#restore) | Een server herstellen uit een back-up. |
| [az group delete](/cli/azure/group#delete) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen
- Lees de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.
- Meer scripts om te proberen: [Azure CLI-voorbeelden voor Azure Database for MySQL](../sample-scripts-azure-cli.md)
