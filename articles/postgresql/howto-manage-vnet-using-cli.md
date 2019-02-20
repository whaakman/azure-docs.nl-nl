---
title: Maken en beheren van Azure Database voor PostgreSQL-VNet-service-eindpunten en regels met behulp van Azure CLI
description: In dit artikel wordt beschreven hoe u maken en beheren van Azure Database voor PostgreSQL-VNet-service-eindpunten en regels met behulp van Azure CLI-opdrachtregel.
author: mbolz
ms.author: mbolz
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 10/23/2018
ms.openlocfilehash: 67ca81e385bbb7aaddddc6af9aa9bcbd45005e20
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56416731"
---
# <a name="create-and-manage-azure-database-for-postgresql-vnet-service-endpoints-using-azure-cli"></a>Maken en beheren van Azure Database voor PostgreSQL-VNet-service-eindpunten met behulp van Azure CLI
Virtueel netwerk (VNet) services-eindpunten en regels uitbreiden privé-adresruimte van een Virtueelnetwerk met uw Azure Database for PostgreSQL-server. Met behulp van handige opdrachten van Azure-opdrachtregelinterface (CLI), kunt u maken, bijwerken, verwijderen, lijst en VNet-service-eindpunten en regels voor het beheren van uw server weergeven. Zie voor een overzicht van Azure Database voor PostgreSQL-VNet-service-eindpunten, met inbegrip van beperkingen, [Azure Database for PostgreSQL-Server VNet-service-eindpunten](concepts-data-access-and-security-vnet.md). VNet-service-eindpunten zijn beschikbaar in alle ondersteunde regio's voor Azure Database for PostgreSQL.

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- Installeer [de Azure CLI](/cli/azure/install-azure-cli) of de Azure Cloud Shell gebruiken in de browser.
- Een [Azure Database for PostgreSQL-server en database](quickstart-create-server-database-azure-cli.md).

> [!NOTE]
> Ondersteuning voor VNet-service-eindpunten is alleen voor algemeen gebruik en geoptimaliseerd voor geheugen-servers.
> In het geval van VNet-peering wordt als verkeer via een gemeenschappelijke VNet-Gateway met service-eindpunten stroomt en moet worden overgebracht naar de peer, maak een ACL/VNet-regel voor het toestaan van Azure Virtual Machines in het VNet-Gateway voor toegang tot de Azure Database for PostgreSQL-server.

## <a name="configure-vnet-service-endpoints-for-azure-database-for-postgresql"></a>Vnet-service-eindpunten configureren voor Azure Database for PostgreSQL
De [az network vnet](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest) opdrachten worden gebruikt om virtuele netwerken te configureren.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer de opdracht `az --version` uit om de geïnstalleerde versie te zien. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 

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
> Het is raadzaam om te lezen in dit artikel over de service-eindpunt configuraties en overwegingen voordat u het voorbeeldscript uitvoert of het configureren van service-eindpunten. **Virtual Network service-eindpunt:** Een [service-eindpunt voor Virtueelnetwerk](../virtual-network/virtual-network-service-endpoints-overview.md) is een subnet met eigenschappen die een of meer formele Azure-service typenamen bevatten. VNet-services-eindpunten gebruikt u de naam van de service type **Microsoft.Sql**, die verwijst naar de Azure-service met de naam SQL-Database. Deze servicetag geldt ook voor de Azure SQL Database, Azure Database for PostgreSQL en MySQL-services. Het is belangrijk te weten bij het toepassen van de **Microsoft.Sql** servicetag naar een VNet-service-eindpunt configureert het verkeer van de service-eindpunt voor alle Azure-Database-services, met inbegrip van Azure SQL Database, Azure Database for PostgreSQL en Azure Database for MySQL-servers op het subnet. 
> 

### <a name="sample-script-to-create-an-azure-database-for-postgresql-database-create-a-vnet-vnet-service-endpoint-and-secure-the-server-to-the-subnet-with-a-vnet-rule"></a>Voorbeeld van een script voor het maken van een Azure Database for PostgreSQL-database, een VNet, VNet-service-eindpunt maken en beveiligen van de server met het subnet met een VNet-regel
Wijzig in dit voorbeeldscript de gemarkeerde regels om de gebruikersnaam en het wachtwoord van de beheerder aan te passen. Vervang de abonnement-id die wordt gebruikt de `az account set --subscription` opdracht met uw eigen abonnement-id.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/create-postgresql-server.sh?highlight=5,20 "Create an Azure Database for PostgreSQL, VNet, VNet service endpoint, and VNet rule.")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie
Na het uitvoeren van het voorbeeldscript kan de volgende opdracht worden gebruikt om de resourcegroep en alle resources die er aan zijn gekoppeld te verwijderen.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/delete-postgresql.sh "Delete the resource group.")]
