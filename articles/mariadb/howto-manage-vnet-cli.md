---
title: Maken en beheren van Azure Database voor MariaDB-VNet-service-eindpunten en regels met behulp van Azure CLI | Microsoft Docs
description: In dit artikel wordt beschreven hoe u maken en beheren van Azure Database voor MariaDB-VNet-service-eindpunten en regels met behulp van Azure CLI-opdrachtregel.
services: mariaDB
author: ajlam
ms.author: andrela
editor: jasonwhowell
ms.service: mariadb
ms.devlang: azure-cli
ms.topic: article
ms.date: 11/20/2018
ms.openlocfilehash: 7f39eb2b3020016d1fb14e8c611ae00dfab07c11
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52336794"
---
# <a name="create-and-manage-azure-database-for-mariadb-vnet-service-endpoints-using-azure-cli"></a>Maken en beheren van Azure Database voor MariaDB-VNet-service-eindpunten met behulp van Azure CLI

> [!IMPORTANT]
> De VNet-service-eindpunten-functie is beschikbaar als openbare preview.

Virtueel netwerk (VNet) services-eindpunten en regels uitbreiden privé-adresruimte van een Virtueelnetwerk met uw Azure Database voor MariaDB-server. Met behulp van handige opdrachten van Azure-opdrachtregelinterface (CLI), kunt u maken, bijwerken, verwijderen, lijst en VNet-service-eindpunten en regels voor het beheren van uw server weergeven. Zie voor een overzicht van Azure Database voor MariaDB-VNet-service-eindpunten, met inbegrip van beperkingen, [Azure Database voor MariaDB Server VNet service-eindpunten](concepts-data-access-security-vnet.md). VNet-service-eindpunten zijn beschikbaar in alle ondersteunde regio's voor Azure Database voor MariaDB.

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- Installeer [de Azure CLI](/cli/azure/install-azure-cli) of de Azure Cloud Shell gebruiken in de browser.
- Een [Azure Database voor MariaDB-server en database](quickstart-create-mariadb-server-database-using-azure-cli.md).

> [!NOTE]
> Ondersteuning voor VNet-service-eindpunten is alleen voor algemeen gebruik en geoptimaliseerd voor geheugen-servers.

## <a name="configure-vnet-service-endpoints"></a>VNet-service-eindpunten configureren
De [az network vnet](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest) opdrachten worden gebruikt om virtuele netwerken te configureren.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer de opdracht `az --version` uit om de geïnstalleerde versie te zien. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren]( /cli/azure/install-azure-cli). 

Als u de CLI lokaal uitvoert, moet u zich aanmelden bij uw account met behulp van de opdracht [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest). Let op de **id**-eigenschap van de opdrachtuitvoer voor de naam van het desbetreffende abonnement.
```azurecli-interactive
az login
```

Als u meerdere abonnementen hebt, kiest u het juiste abonnement waarin de resource moet worden gefactureerd. Selecteer de specifieke abonnements-id in uw account met de opdracht [az account set](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-set). Gebruik de **id**-eigenschap uit de **az login**-uitvoer voor uw abonnement in de tijdelijke aanduiding voor de abonnement-id.

- Het account moet de vereiste machtigingen om het maken van een virtueel netwerk en de service-eindpunt hebben.

Service-eindpunten kunnen afzonderlijk worden geconfigureerd op de virtuele netwerken, door een gebruiker met schrijftoegang tot het virtuele netwerk.

Voor het Azure-serviceresources aan een VNet, moet de gebruiker machtiging voor 'Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/' voor de subnetten die worden toegevoegd. Deze machtiging is standaard opgenomen in de ingebouwde service-beheerdersrollen en kan worden gewijzigd door aangepaste rollen te maken.

Meer informatie over [ingebouwde rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) en het toewijzen van specifieke machtigingen voor [aangepaste rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).

VNets en Azure-serviceresources kunnen in hetzelfde abonnement of in verschillende abonnementen zitten. Als het VNet en Azure-serviceresources in verschillende abonnementen zitten, moeten de resources onder dezelfde Active Directory (AD)-tenant.

> [!IMPORTANT]
> Het is raadzaam om te lezen in dit artikel over de service-eindpunt configuraties en overwegingen voor het configureren van service-eindpunten. **Service-eindpunt voor virtueel netwerk:** A [service-eindpunt voor Virtueelnetwerk](../virtual-network/virtual-network-service-endpoints-overview.md) is een subnet met eigenschappen die een of meer formele Azure-service typenamen bevatten. VNet-services-eindpunten gebruikt u de naam van de service type **Microsoft.Sql**, die verwijst naar de Azure-service met de naam SQL-Database. Deze servicetag geldt ook voor de Azure SQL Database, Azure Database voor MariaDB, PostgreSQL en MySQL-services. Het is belangrijk te weten bij het toepassen van de **Microsoft.Sql** servicetag naar een VNet-service-eindpunt configureert het verkeer van de service-eindpunt voor alle Azure-Database-services, met inbegrip van Azure SQL Database, Azure Database for PostgreSQL, Azure Database voor MariaDB en Azure Database for MySQL-servers op het subnet.

### <a name="sample-script"></a>Voorbeeldscript

Met dit voorbeeldscript wordt gebruikt voor het maken van een Azure Database voor MariaDB-server, een VNet, VNet-service-eindpunt maken en beveiligen van de server met het subnet met een VNet-regel. In dit voorbeeldscript wijzigen de gebruikersnaam van beheerder en het wachtwoord. Vervang de abonnement-id die wordt gebruikt de `az account set --subscription` opdracht met uw eigen abonnement-id.

```azurecli-interactive
# To find the name of an Azure region in the CLI run this command: az account list-locations
# Substitute  <subscription id> with your identifier
az account set --subscription <subscription id>

# Create a resource group
az group create \
--name myresourcegroup \
--location westus

# Create a MariaDB server in the resource group
# Name of a server maps to DNS name and is thus required to be globally unique in Azure.
# Substitute the <server_admin_password> with your own value.
az mariadb server create \
--name mydemoserver \
--resource-group myresourcegroup \
--location westus \
--admin-user mylogin \
--admin-password <server_admin_password> \
--sku-name GP_Gen5_2

# Get available service endpoints for Azure region output is JSON
# Use the command below to get the list of services supported for endpoints, for an Azure region, say "westus".
az network vnet list-endpoint-services \
-l westus

# Add Azure SQL service endpoint to a subnet *mySubnet* while creating the virtual network *myVNet* output is JSON
az network vnet create \
-g myresourcegroup \
-n myVNet \
--address-prefixes 10.0.0.0/16 \
-l westus

# Creates the service endpoint
az network vnet subnet create \
-g myresourcegroup \
-n mySubnet \
--vnet-name myVNet \
--address-prefix 10.0.1.0/24 \
--service-endpoints Microsoft.SQL

# View service endpoints configured on a subnet
az network vnet subnet show \
-g myresourcegroup \
-n mySubnet \
--vnet-name myVNet

# Create a VNet rule on the sever to secure it to the subnet Note: resource group (-g) parameter is where the database exists. VNet resource group if different should be specified using subnet id (URI) instead of subnet, VNet pair.
az mariadb server vnet-rule create \
-n myRule \
-g myresourcegroup \
-s mydemoserver \
--vnet-name myVNet \
--subnet mySubnet
```

<!-- 
In this sample script, change the highlighted lines to customize the admin username and password. Replace the SubscriptionID used in the `az account set --subscription` command with your own subscription identifier.
[!code-azurecli-interactive[main](../../cli_scripts/mariadb/create-mysql-server-vnet/create-mysql-server.sh?highlight=5,20 "Create an Azure Database for MariaDB, VNet, VNet service endpoint, and VNet rule.")]
-->

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.

```azurecli-interactive
az group delete --name myresourcegroup
```


<!--
[!code-azurecli-interactive[main](../../cli_scripts/mysql/create-mysql-server-vnet/delete-mysql.sh "Delete the resource group.")]
-->