---
title: Azure Database for MySQL-gegevens in replicatie opgeslagen Procedures
description: Dit artikel bevat alle opgeslagen procedures gebruikt voor replicatie van gegevens in.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 08/31/2018
ms.openlocfilehash: fb1a1b31d90df0022e5973de3ae2f55fb4c36701
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43665943"
---
# <a name="azure-database-for-mysql-data-in-replication-stored-procedures"></a>Azure Database for MySQL-gegevens in replicatie opgeslagen Procedures

Gegevens in replicatie kunt u gegevens synchroniseren met een MySQL-server die on-premises worden uitgevoerd in virtuele machines of database-services die worden gehost door andere cloudproviders in de Azure Database for MySQL-service.

De volgende opgeslagen procedures worden gebruikt om te stellen of verwijderen van gegevens in replicatie tussen een model en de replica.

|**Naam van opgeslagen Procedure**|**Invoerparameters**|**Output-Parameters**|**Gebruik Opmerking**|
|-----|-----|-----|-----|
|*MySQL.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|N/A|In de context van het CA-certificaat in de parameter master_ssl_ca doorgeven om over te dragen gegevens met SSL-modus. </br><br>In een lege tekenreeks in de parameter master_ssl_ca doorgeven om over te dragen gegevens zonder SSL.|
|*MySQL.az_replication _starten*|N/A|N/A|Replicatie is gestart.|
|*MySQL.az_replication _stop*|N/A|N/A|Replicatie beëindigen.|
|*MySQL.az_replication _remove_master*|N/A|N/A|Hiermee verwijdert u de replicatierelatie tussen de hoofd- en replica.|
|*MySQL.az_replication_skip_counter*|N/A|N/A|Hiermee slaat u een fout bij de replicatie.|

Als u gegevens in replicatie tussen een hoofd- en replica in een Azure Database for MySQL instelt, raadpleegt u [het configureren van replicatie van gegevens in](howto-data-in-replication.md).
