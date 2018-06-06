---
title: Maken en beheren van Azure-Database voor de service-eindpunten MySQL VNet en regels die gebruikmaken van Azure CLI | Microsoft Docs
description: In dit artikel wordt beschreven hoe maken en beheren van Azure-Database voor de service-eindpunten MySQL VNet en regels met behulp van Azure CLI-opdrachtregel.
services: mysql
author: mbolz
ms.author: mbolz
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/01/2018
ms.openlocfilehash: fd8b21d1273b1bd02b0a949894be53cdc4a5c3c0
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34736589"
---
# <a name="create-and-manage-azure-database-for-mysql-vnet-service-endpoints-using-azure-cli"></a>Maken en beheren van Azure-Database voor service-eindpunten MySQL VNet met Azure CLI
Virtueel netwerk (VNet) services-eindpunten en de regels van de persoonlijke adresruimte van een virtueel netwerk uitbreiden naar uw Azure-Database voor de MySQL-server. U handige Azure-opdrachtregelinterface (CLI)-opdrachten gebruikt, kunt u maken, bijwerken, verwijderen, weergeven en service-eindpunten VNet en regels voor het beheren van uw server weergeven. Zie voor een overzicht van Azure-Database voor MySQL VNet service-eindpunten, met inbegrip van beperkingen, [Azure Database voor de service-eindpunten MySQL Server VNet](concepts-data-access-and-security-vnet.md). VNet-service-eindpunten zijn beschikbaar in public preview in alle ondersteunde regio's voor Azure-Database voor MySQL.

## <a name="prerequisites"></a>Vereisten
Stap in deze handleiding instructies, wilt u het volgende nodig:
- Installeer [Azure CLI 2.0](/cli/azure/install-azure-cli) opdrachtregelprogramma of gebruik de Azure-Cloud-Shell in de browser.
- Een [Azure-Database voor MySQL-server en database](quickstart-create-mysql-server-database-using-azure-cli.md).

> [!NOTE]
> Ondersteuning voor VNet-service-eindpunten is alleen voor algemene doeleinden en geoptimaliseerd voor geheugen-servers.

## <a name="configure-vnet-service-endpoints-for-azure-database-for-mysql"></a>Vnet-service-eindpunten configureren voor Azure-Database voor MySQL
De [az network vnet](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest) opdrachten worden gebruikt voor het configureren van virtuele netwerken.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer de opdracht `az --version` uit om de geïnstalleerde versie te zien. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

Als u de CLI lokaal uitvoert, moet u zich aanmelden bij uw account met behulp van de opdracht [az login](/cli/azure/authenticate-azure-cli?view=interactive-log-in). Let op de **id**-eigenschap van de opdrachtuitvoer voor de naam van het desbetreffende abonnement.
```azurecli-interactive
az login
```

Installeren van de CLI-extensie voor Azure-Database voor MySQL VNet service-eindpunten met behulp van de `az extension add --name rdbms-vnet` opdracht. 
```azurecli-interactive
az extension add --name rdbms-vnet
```

Voer de `az extension list` om te controleren of de installatie van de CLI-extensie.
```azurecli-interactive
az extension list
```
De uitvoer van de opdracht geeft een lijst met alle geïnstalleerde uitbreidingen. De Azure-Database voor MySQL CLI-extensie is:

 {"extensionType": "whl", "name": 'rdbms-vnet', 'versie': "10.0.0"}

> [!NOTE]
> Verwijderen van de CLI-extensie uitgevoerd de `az extension remove -n rdbms-vnet` opdracht. 

Als u meerdere abonnementen hebt, kiest u het juiste abonnement waarin de resource moet worden gefactureerd. Selecteer de specifieke abonnements-id in uw account met de opdracht [az account set](/cli/azure/account#az_account_set). Gebruik de **id**-eigenschap uit de **az login**-uitvoer voor uw abonnement in de tijdelijke aanduiding voor de abonnement-id.

- Het account moet de vereiste machtigingen om het maken van een virtueel netwerk en de service-eindpunt hebben.

Service-eindpunten kunnen afzonderlijk worden geconfigureerd op de virtuele netwerken, door een gebruiker met schrijftoegang tot het virtuele netwerk.

Als u Azure-serviceresources naar een VNet wilt beveiligen, moet u machtigingen hebben voor Microsoft.Network/JoinServicetoaSubnet voor de subnetten die worden toegevoegd. Deze machtiging is standaard opgenomen in de ingebouwde service-beheerdersrollen en kan worden gewijzigd door aangepaste rollen te maken.

Meer informatie over [ingebouwde rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) en het toewijzen van specifieke machtigingen voor [aangepaste rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).

VNets en Azure-serviceresources kunnen in hetzelfde abonnement of in verschillende abonnementen zitten. Als de bronnen voor VNet en Azure-service zich in verschillende abonnementen behoren, de resources moeten onder dezelfde tenant Active Directory (AD), op het moment van deze preview.

> [!IMPORTANT]
> Het is raadzaam Lees dit artikel over service-eindpuntconfiguraties en overwegingen voordat u het voorbeeldscript of service-eindpunten configureren. **Virtueel netwerk service-eindpunt:** A [Virtual Network service-eindpunt](../virtual-network/virtual-network-service-endpoints-overview.md) een subnet waarvan de eigenschapwaarden een of meer namen van formele Azure-service bevatten. VNet-services-eindpunten gebruiken de naam van de service type **Microsoft.Sql**, die verwijst naar de Azure-service met de naam SQL-Database. Deze servicetag geldt ook voor de Azure SQL Database, Azure-Database voor PostgreSQL en MySQL-services. Het is belangrijk te weten bij het toepassen van de **Microsoft.Sql** service-tag met de service-eindpunt van een VNet configureert het netwerkverkeer voor de service-eindpunt voor alle Azure-Database-services, met inbegrip van Azure SQL Database, Azure-Database voor PostgreSQL en Azure-Database voor de MySQL-servers op het subnet. 
> 

### <a name="sample-script-to-create-an-azure-database-for-mysql-database-create-a-vnet-vnet-service-endpoint-and-secure-the-server-to-the-subnet-with-a-vnet-rule"></a>Voorbeeldscript te maken van een Azure-Database voor de MySQL-database, maakt u een VNet, VNet service-eindpunt en de server met het subnet met een VNet-regel beveiligen
Wijzig in dit voorbeeldscript de gemarkeerde regels om de gebruikersnaam en het wachtwoord van de beheerder aan te passen. Vervang de abonnements-id gebruikt in de `az account set --subscription` opdracht met de id van uw eigen abonnement.
[!code-azurecli-interactive[main](../../cli_scripts/mysql/create-mysql-server-vnet/create-mysql-server.sh?highlight=5,20 "Create an Azure Database for MySQL, VNet, VNet service endpoint, and VNet rule.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.
[!code-azurecli-interactive[main](../../cli_scripts/mysql/create-mysql-server-vnet/delete-mysql.sh "Delete the resource group.")]
