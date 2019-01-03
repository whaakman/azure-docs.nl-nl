---
title: Serverparameters configureren in Azure Database voor MySQL
description: In dit artikel wordt beschreven hoe u MySQL-serverparameters configureren in Azure Database voor MySQL met behulp van de Azure-portal.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: 103e09a0e2b9dd409fa2ddaff1c5311ef9936d22
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53538495"
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Parameters van de server configureren in Azure Database voor MySQL met behulp van Azure portal

Azure Database for MySQL ondersteunt de configuratie van bepaalde parameters van de server. In dit artikel wordt beschreven hoe u deze parameters configureren met behulp van de Azure-portal. Niet alle parameters van de server kunnen worden aangepast.

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Navigeer naar de Parameters van de Server in Azure portal

1. Meld u aan bij Azure portal en zoek uw Azure Database voor MySQL-server.
2. Onder de **instellingen** sectie, klikt u op **serverparameters** openen van de pagina de parameters voor de Azure Database for MySQL-server.
![Pagina van de parameters voor Azure portal-server](./media/howto-server-parameters/auzre-portal-server-parameters.png)
3. Zoek alle instellingen die u nodig hebt om aan te passen. Controleer de **beschrijving** kolom om te begrijpen van het doel en de toegestane waarden.
![Vervolgkeuzelijst omlaag opsommen](./media/howto-server-parameters/3-toggle_parameter.png)
4. Klik op **opslaan** uw wijzigingen op te slaan.
![Opslaan of wijzigingen negeren](./media/howto-server-parameters/4-save_parameters.png)
5. Als u nieuwe waarden voor de parameters hebt opgeslagen, kunt u altijd terugkeren alles weer op de standaardwaarden hiervoor **alle standaardinstellingen opnieuw instellen**.
![Alle standaardinstellingen opnieuw instellen](./media/howto-server-parameters/5-reset_parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Lijst met parameters van de server kunnen worden geconfigureerd

De lijst met ondersteunde serverparameters groeit voortdurend. Gebruik het tabblad van de parameters-server in Azure portal naar de definitie en parameters van de server op basis van de toepassingsvereisten van uw configureren.

## <a name="non-configurable-server-parameters"></a>Niet-configureerbare serverparameters

InnoDB-buffergroep en maximum aantal verbindingen zijn niet kunnen worden geconfigureerd en gekoppeld aan uw [prijscategorie](concepts-service-tiers.md).

|**Prijscategorie**| **Bewerking voor COMPUTE**|**vCore(s)**|**InnoDB buffergroep (MB)**| **Maximum aantal verbindingen**|
|---|---|---|---|--|
|Basic| Gen 4| 1| 960| 50|
|Basic| Gen 4| 2| 2560| 100|
|Basic| Gen 5| 1| 960| 50|
|Basic| Gen 5| 2| 2560| 100|
|Algemeen doel| Gen 4| 2| 3584| 300|
|Algemeen doel| Gen 4| 4| 7680| 625|
|Algemeen doel| Gen 4| 8| 15360| 1250|
|Algemeen doel| Gen 4| 16| 31232| 2500|
|Algemeen doel| Gen 4| 32| 62976| 5000|
|Algemeen doel| Gen 5| 2| 3584| 300|
|Algemeen doel| Gen 5| 4| 7680| 625|
|Algemeen doel| Gen 5| 8| 15360| 1250|
|Algemeen doel| Gen 5| 16| 31232| 2500|
|Algemeen doel| Gen 5| 32| 62976| 5000|
|Algemeen doel| Gen 5| 64| 125952| 10.000|
|Geoptimaliseerd geheugen| Gen 5| 2| 7168| 600|
|Geoptimaliseerd geheugen| Gen 5| 4| 15360| 1250|
|Geoptimaliseerd geheugen| Gen 5| 8| 30720| 2500|
|Geoptimaliseerd geheugen| Gen 5| 16| 62464| 5000|
|Geoptimaliseerd geheugen| Gen 5| 32| 125952| 10.000|

Deze extra server-parameters zijn niet kunnen worden geconfigureerd in het systeem:

|**Parameter**|**Vaste waarde**|
| :------------------------ | :-------- |
|innodb_file_per_table in Basic-laag|UIT|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512 MB|

Andere parameters van de server die hier niet worden weergegeven zijn ingesteld op de MySQL standaardwaarden out-of-box voor versies [5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) en [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html).

## <a name="working-with-the-time-zone-parameter"></a>Werken met de parameter tijdzone

### <a name="populating-the-time-zone-tables"></a>Invullen van de tijdzone-tabellen

De tijdzone-tabellen op de server kunnen worden gevuld door het aanroepen van de `az_load_timezone` opgeslagen procedure van een hulpprogramma zoals de MySQL-opdrachtregel of MySQL Workbench.

> [!NOTE]
> Als u werkt met de `az_load_timezone` opdracht via MySQL Workbench, moet u de veilige modus eerst uitschakelen met behulp van `SET SQL_SAFE_UPDATES=0;`.

```sql
CALL mysql.az_load_timezone();
```

Als u wilt weergeven van waarden van de beschikbare tijd zone, moet u de volgende opdracht uitvoeren:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>De globale niveau tijdzone instellen

De globale niveau tijdzone kan worden ingesteld van de **serverparameters** pagina in de Azure portal. De onderstaande stelt de globale tijdzone op de waarde "VS / Stille Oceaan '.

![De tijdzoneparameter instellen](./media/howto-server-parameters/timezone.png)

### <a name="setting-the-session-level-time-zone"></a>De sessie niveau tijdzone instellen

De sessie niveau time zone kan worden ingesteld door het uitvoeren van de `SET time_zone` opdracht uit vanaf een hulpprogramma zoals de MySQL-opdrachtregel of MySQL Workbench. In het volgende voorbeeld wordt de tijdzone ingesteld op de **VS / Stille Oceaan** tijdzone.

```sql
SET time_zone = 'US/Pacific';
```

Raadpleeg de MySQL-documentatie voor [datum- en tijdfuncties](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_convert-tz).

## <a name="next-steps"></a>Volgende stappen

- [Verbindingsbibliotheken voor Azure Database for MySQL](concepts-connection-libraries.md).
