---
title: Parameters van de Server in Azure-Database configureren voor MySQL | Microsoft Docs
description: In dit artikel wordt beschreven hoe serverparameters MySQL in Azure-Database configureren voor MySQL met de Azure portal.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/25/2018
ms.openlocfilehash: 59eeed42356a276c259bd8da55890b7ada67d729
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Parameters van de server in Azure-Database configureren voor MySQL met behulp van de Azure-portal

Azure MySQL-Database ondersteunt de configuratie van bepaalde parameters van de server. In dit artikel wordt beschreven hoe deze parameters configureren met behulp van de Azure-portal. Niet alle parameters van de server kunnen worden aangepast. 

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Navigeer naar de Parameters van de Server op Azure-portal
1. Meld u aan bij de Azure portal en zoek uw Azure-Database voor de MySQL-server.
2. Onder de **instellingen** sectie, klikt u op **serverparameters** de pagina de parameters voor de Azure-Database voor MySQL openen.
3. Zoek alle instellingen die u wilt aanpassen. Controleer de **beschrijving** kolom om te begrijpen van het doel en de toegestane waarden. 
4. Klik op **opslaan** uw wijzigingen op te slaan.

![Pagina van de parameters voor de server voor Azure portal](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Lijst met parameters van de server worden geconfigureerd

De lijst met ondersteunde serverparameters groeit voortdurend. Gebruik het tabblad van de parameters-server in Azure-portal voor de definitie en parameters van de server op basis van de toepassingsvereisten van uw configureren. 

## <a name="nonconfigurable-server-parameters"></a>Serverparameters Nonconfigurable
De buffergroep InnoDB en maximum aantal verbindingen zijn niet configureerbaar en gekoppeld aan uw [prijscategorie](concepts-service-tiers.md). 

| **Prijscategorie** | **InnoDB Buffer Pool (MB)** | **Max Connections** |
| :------------------------ | :-------- | :----------- |
| Basic 50 | 1024 | 50 | 
| Basic 100  | 2560 | 100 | 
| Standaard 100 | 2560 | 200 | 
| Standard 200 | 5120 | 400 | 
| Standard 400 | 10240 | 800 | 
| Standard 800 | 20480 | 1600 |

Deze extra server-parameters zijn niet geconfigureerd in het systeem:

|**Parameter**|**Vaste waarde**|
| :------------------------ | :-------- |
|innodb_file_per_table in basisstaffel|UIT|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512 MB|

Andere parameters van de server die hier niet worden weergegeven zijn ingesteld op MySQL out of box standaardwaarden voor versies [5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) en [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html).

## <a name="next-steps"></a>Volgende stappen
- [Verbindingsbibliotheken voor Azure-Database voor MySQL](concepts-connection-libraries.md).
