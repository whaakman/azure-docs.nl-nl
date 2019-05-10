---
title: Configureer het eindpunt voor openbare - Azure SQL Database managed instance | Microsoft Docs
description: Informatie over het configureren van een openbaar eindpunt voor het beheerde exemplaar
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 05/07/2019
ms.openlocfilehash: d3e68a5287e59c576f85491e6e5eba33fac080ca
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65465185"
---
# <a name="configure-public-endpoint-in-azure-sql-database-managed-instance"></a>Openbare eindpunt in beheerd exemplaar voor Azure SQL Database configureren

Openbaar eindpunt voor een [beheerd exemplaar](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index) kunt u toegang tot gegevens in uw beheerde exemplaar van buiten de [virtueel netwerk](../virtual-network/virtual-networks-overview.md). U zijn kunt toegang tot uw beheerde exemplaar van meerdere tenants Azure-services, zoals Power BI, Azure App Service of een on-premises netwerk. Via het openbare eindpunt op een beheerd exemplaar hoeft u niet te gebruiken een VPN, die kan helpen te voorkomen dat problemen met VPN-doorvoer.

In dit artikel leert u hoe u:

> [!div class="checklist"]
> - Openbare eindpunt voor uw beheerde exemplaar in de Azure-portal inschakelen
> - Openbare eindpunt voor uw beheerde exemplaar met behulp van PowerShell inschakelen
> - Configureren van uw beheerde exemplaar voor de netwerkbeveiligingsgroep voor verkeer naar het openbare eindpunt beheerd exemplaar
> - De verbindingsreeks voor beheerd exemplaar openbaar eindpunt verkrijgen

## <a name="permissions"></a>Machtigingen

Vanwege de vertrouwelijkheid van gegevens die zich in een beheerd exemplaar, vereist de configuratie om in te schakelen beheerd exemplaar openbaar eindpunt een proces in twee stappen. Dit beveiligingsmaatregel voldoet aan de scheiding van functies (beplanten):

- Inschakelen van openbare eindpunt op een beheerd exemplaar moet worden uitgevoerd door de beheerder beheerd exemplaar. De beheerder van het beheerde exemplaar is te vinden in **overzicht** pagina van uw SQL managed instance-bron.
- Toestaan van verkeer dat gebruikmaakt van een netwerkbeveiligingsgroep die moet worden uitgevoerd door de beheerder van een netwerk. Zie voor meer informatie, [network security-groepsmachtigingen](../virtual-network/manage-network-security-group.md#permissions).

## <a name="enabling-public-endpoint-for-a-managed-instance-in-the-azure-portal"></a>Openbaar eindpunt voor een beheerd exemplaar in de Azure-portal inschakelen

1. Starten van de Azure portal op <https://portal.azure.com/.>
1. Open de resourcegroep met het beheerde exemplaar en selecteer de **SQL beheerd exemplaar** dat u configureren van openbare eindpunt wilt op.
1. Op de **Security** instellingen, selecteer de **virtueel netwerk** tabblad.
1. Selecteer in de configuratiepagina van virtueel netwerk **inschakelen** en vervolgens de **opslaan** pictogram van de configuratie bij te werken.

![mi-vnet-config.png](media/sql-database-managed-instance-public-endpoint-configure/mi-vnet-config.png)

## <a name="enabling-public-endpoint-for-a-managed-instance-using-powershell"></a>Inschakelen van openbare eindpunt voor een beheerd exemplaar met behulp van PowerShell

### <a name="enable-public-endpoint"></a>Openbare eindpunt inschakelen

Voer de volgende PowerShell-opdrachten. Vervang **abonnement-id** met uw abonnements-ID. Vervang ook **rg-name** met de resourcegroep voor uw beheerde exemplaar en vervang **mi-name** met de naam van uw beheerde exemplaar.

```powershell
Install-Module -Name Az

Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount

# Use your subscription ID in place of subscription-id below

Select-AzSubscription -SubscriptionId {subscription-id}

# Replace rg-name with the resource group for your managed instance, and replace mi-name with the name of your managed instance

$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}

$mi = $mi | Set-AzSqlInstance -PublicDataEndpointEnabled $true -force
```

### <a name="disable-public-endpoint"></a>Openbaar eindpunt uitschakelen

Als u wilt het openbare eindpunt met behulp van PowerShell uitschakelen, u zou de volgende opdracht uitvoeren (en vergeet ook niet de NSG voor de binnenkomende poort 3342 sluiten als u deze geconfigureerd hebt):

```powershell
Set-AzSqlInstance -PublicDataEndpointEnabled $false -force
```

## <a name="allow-public-endpoint-traffic-on-the-network-security-group"></a>Sta openbaar eindpunt-verkeer op de netwerkbeveiligingsgroep

1. Als u de configuratiepagina van het beheerde exemplaar nog geopend hebt, gaat u naar de **overzicht** tabblad. Anders gaat u terug naar uw **SQL beheerd exemplaar** resource. Selecteer de **virtueel netwerk/subnet** koppeling u naar de configuratiepagina van de virtuele-netwerk gaat.

    ![mi-overview.png](media/sql-database-managed-instance-public-endpoint-configure/mi-overview.png)

1. Selecteer de **subnetten** tabblad in het deelvenster links configuratie van uw virtuele netwerk, en noteer de **BEVEILIGINGSGROEP** voor uw beheerde exemplaar.

    ![mi-vnet-subnet.png](media/sql-database-managed-instance-public-endpoint-configure/mi-vnet-subnet.png)

1. Ga terug naar de resourcegroep waarin uw beheerde exemplaar. U ziet de **netwerkbeveiligingsgroep** naam hierboven vermeld. Selecteer de naam om te gaan naar de configuratiepagina van network security group.

1. Selecteer de **inkomende beveiligingsregels** tabblad, en **toevoegen** een regel met hogere prioriteit dan de **deny_all_inbound** regel met de volgende instellingen: </br> </br>

    |Instelling  |Voorgestelde waarde  |Description  |
    |---------|---------|---------|
    |**Bron**     |IP-adres of servicetag         |<ul><li>Voor Azure-services zoals Power BI, selecteert u de Tag Azure Cloud Service</li> <li>Gebruik NAT IP-adres voor uw computer of virtuele machine van Azure</li></ul> |
    |**Poortbereiken van bron**     |*         |Laat deze * (alle) zoals bronpoorten meestal dynamisch toegewezen en als dergelijke onvoorspelbare zijn |
    |**Destination**     |Elk         |Als alle verkeer toe te staan in het subnet van het beheerde exemplaar bestemming verlaten |
    |**Poortbereiken van doel**     |3342         |Bereik doelpoort voor 3342, dit de openbare TDS-eindpunt voor beheerd exemplaar is |
    |**Protocol**     |TCP         |Beheerd exemplaar maakt gebruik van TCP-protocol voor TDS |
    |**Actie**     |Toestaan         |Binnenkomend verkeer naar beheerd exemplaar via het openbare eindpunt toestaan |
    |**prioriteit**     |1300         |Zorg ervoor dat deze regel is een hogere prioriteit dan de **deny_all_inbound** regel |

    ![mi-nsg-rules.png](media/sql-database-managed-instance-public-endpoint-configure/mi-nsg-rules.png)

    > [!NOTE]
    > Poort 3342 wordt gebruikt voor het openbare eindpunt verbindingen met beheerd exemplaar, en op dit moment niet worden gewijzigd.

## <a name="obtaining-the-managed-instance-public-endpoint-connection-string"></a>Het ophalen van de verbindingsreeks voor beheerd exemplaar openbaar eindpunt

1. Navigeer naar de configuratiepagina van de SQL-beheerd exemplaar dat is ingeschakeld voor het openbare eindpunt. Selecteer de **verbindingsreeksen** tabblad onder de **instellingen** configuratie.
1. Houd er rekening mee dat de host-naam van het openbare eindpunt is beschikbaar in de indeling < mi_name >. **openbare**. < dns_zone >. database.windows.net en dat de poort die wordt gebruikt voor de verbinding 3342 is.

    ![mi-public-endpoint-conn-string.png](media/sql-database-managed-instance-public-endpoint-configure/mi-public-endpoint-conn-string.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [met behulp van Azure SQL-Database beheerd exemplaar veilig met een openbaar eindpunt](sql-database-managed-instance-public-endpoint-securely.md).