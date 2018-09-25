---
title: Azure Database voor MariaDB-gegevens in replicatie opgeslagen Procedures
description: Dit artikel bevat alle opgeslagen procedures gebruikt voor replicatie van gegevens in.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: reference
ms.date: 09/24/2018
ms.openlocfilehash: 87c6fa783964c019841810ec38f342a5a44c0ee3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46959084"
---
# <a name="azure-database-for-mariadb-data-in-replication-stored-procedures"></a>Azure Database voor MariaDB-gegevens in replicatie opgeslagen Procedures

Gegevens in replicatie kunt u gegevens synchroniseren met een MariaDB-server die on-premises worden uitgevoerd in virtuele machines of database-services die worden gehost door andere cloudproviders in de Azure Database voor MariaDB-service.

De volgende opgeslagen procedures worden gebruikt om te stellen of verwijderen van gegevens in replicatie tussen een model en de replica.

|**Naam van opgeslagen Procedure**|**Invoerparameters**|**Output-Parameters**|**Gebruik Opmerking**|
|-----|-----|-----|-----|
|*MySQL.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|N/A|In de context van het CA-certificaat in de parameter master_ssl_ca doorgeven om over te dragen gegevens met SSL-modus. </br><br>In een lege tekenreeks in de parameter master_ssl_ca doorgeven om over te dragen gegevens zonder SSL.|
|*MySQL.az_replication _starten*|N/A|N/A|Replicatie is gestart.|
|*MySQL.az_replication _stop*|N/A|N/A|Replicatie beëindigen.|
|*MySQL.az_replication _remove_master*|N/A|N/A|Hiermee verwijdert u de replicatierelatie tussen de hoofd- en replica.|
|*MySQL.az_replication_skip_counter*|N/A|N/A|Hiermee slaat u een fout bij de replicatie.|

Als u gegevens in replicatie tussen een hoofd- en replica in een Azure Database voor MariaDB instelt, raadpleegt u [het configureren van replicatie van gegevens in](howto-data-in-replication.md).