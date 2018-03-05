---
title: Parameters van de Server in Azure-Database configureren voor MySQL
description: In dit artikel wordt beschreven hoe serverparameters MySQL in Azure-Database configureren voor MySQL met de Azure portal.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: b3510c616d2a9ba66cb83cb998c42e03fdbb0f2b
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Parameters van de server in Azure-Database configureren voor MySQL met behulp van de Azure-portal

Azure MySQL-Database ondersteunt de configuratie van bepaalde parameters van de server. In dit artikel wordt beschreven hoe deze parameters configureren met behulp van de Azure-portal. Niet alle parameters van de server kunnen worden aangepast. 

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Navigeer naar de Parameters van de Server op Azure-portal
1. Meld u aan bij de Azure portal en zoek uw Azure-Database voor de MySQL-server.
2. Onder de **instellingen** sectie, klikt u op **serverparameters** de pagina de parameters voor de Azure-Database voor MySQL openen.
![Pagina van de parameters voor de server voor Azure portal](./media/howto-server-parameters/auzre-portal-server-parameters.png)
3. Zoek alle instellingen die u wilt aanpassen. Controleer de **beschrijving** kolom om te begrijpen van het doel en de toegestane waarden. 
![Verwijder omlaag opsommen](./media/howto-server-parameters/3-toggle_parameter.png)
4. Klik op **opslaan** uw wijzigingen op te slaan.
![Opslaan of wijzigingen negeren](./media/howto-server-parameters/4-save_parameters.png)
5. Als u nieuwe waarden voor de parameters hebt opgeslagen, u kunt altijd terugkeren alles terug naar de standaardwaarden door te selecteren **opnieuw instellen van alle standaardwaarden**.
![Alles standaardwaarden herstellen](./media/howto-server-parameters/5-reset_parameters.png)


## <a name="list-of-configurable-server-parameters"></a>Lijst met parameters van de server worden geconfigureerd

De lijst met ondersteunde serverparameters groeit voortdurend. Gebruik het tabblad van de parameters-server in Azure-portal voor de definitie en parameters van de server op basis van de toepassingsvereisten van uw configureren. 

## <a name="nonconfigurable-server-parameters"></a>Serverparameters Nonconfigurable
De buffergroep InnoDB en maximum aantal verbindingen zijn niet configureerbaar en gekoppeld aan uw [prijscategorie](concepts-service-tiers.md). 

|**Prijscategorie**| **COMPUTE generatie**|**vCore(s)**|**InnoDB Buffer Pool (MB)**| **Max Connections**|
|---|---|---|---|--|
|Basic| Gen 4| 1| 1024| 50 |
|Basic| Gen 4| 2| 2560| 100 |
|Basic| Gen 5| 1| 1024| 50 |
|Basic| Gen 5| 2| 2560| 100 |
|Algemeen doel| Gen 4| 2| 2560| 200|
|Algemeen doel| Gen 4| 4| 5120| 400|
|Algemeen doel| Gen 4| 8| 10240| 800|
|Algemeen doel| Gen 4| 16| 20480| 1600|
|Algemeen doel| Gen 4| 32| 40960| 3200|
|Algemeen doel| Gen 5| 2| 2560| 200|
|Algemeen doel| Gen 5| 4| 5120| 400|
|Algemeen doel| Gen 5| 8| 10240| 800|
|Algemeen doel| Gen 5| 16| 20480| 1600|
|Algemeen doel| Gen 5| 32| 40960| 3200|
|Geoptimaliseerd geheugen| Gen 5| 2| 7168| 600|
|Geoptimaliseerd geheugen| Gen 5| 4| 15360| 1250|
|Geoptimaliseerd geheugen| Gen 5| 8| 30720| 2500|
|Geoptimaliseerd geheugen| Gen 5| 16| 62464| 5000|
|Geoptimaliseerd geheugen| Gen 5| 32| 125952| 10.000| 

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
