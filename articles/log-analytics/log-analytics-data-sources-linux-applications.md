---
title: Verzamelen van de toepassingsprestaties Linux in OMS Log Analytics | Microsoft Docs
description: Dit artikel bevat informatie voor het configureren van de OMS-Agent voor Linux voor het verzamelen van prestatiemeteritems voor MySQL en Apache HTTP-Server.
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 04ea6f728e59ec8b47e54fe45e1adc6cbbfb85ff
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="collect-performance-counters-for-linux-applications-in-log-analytics"></a>Verzamelen van prestatiemeteritems voor Linux-toepassingen in Log Analytics 
Dit artikel bevat informatie voor het configureren van de [OMS-Agent voor Linux](https://github.com/Microsoft/OMS-Agent-for-Linux) voor het verzamelen van prestatiemeteritems voor specifieke toepassingen.  De toepassingen die zijn opgenomen in dit artikel zijn:  

- [MySQL](#MySQL)
- [Apache HTTP-Server](#apache-http-server)

## <a name="mysql"></a>MySQL
Als MySQL-Server of MariaDB Server wordt gedetecteerd op de computer wanneer de OMS-agent is geïnstalleerd, wordt een provider voor de MySQL-Server voor prestatiebewaking automatisch geïnstalleerd. Deze provider verbinding maakt met de lokale MySQL/MariaDB-server om prestatiestatistieken weer te geven. MySQL-gebruikersreferenties moeten worden geconfigureerd zodat de provider toegang heeft tot de MySQL-Server.

### <a name="configure-mysql-credentials"></a>MySQL-referenties configureren
De provider MySQL OMI moet de gebruiker vooraf geconfigureerde MySQL en MySQL-clientbibliotheken query's op de prestaties en de statusgegevens van de MySQL-exemplaar geïnstalleerd.  Deze referenties worden opgeslagen in een verificatiebestand dat opgeslagen op de Linux-agent.  Het verificatiebestand specificeert welke bind-adres en poort van het exemplaar MySQL luistert op en wat referenties om te gebruiken voor het verzamelen van metrische gegevens.  

Tijdens de installatie van de OMS-Agent voor Linux de MySQL OMI provider scant MySQL my.cnf configuratiebestanden (standaardlocaties) voor bind-adres en poort en de MySQL OMI gedeeltelijk verificatiebestand ingesteld.

De MySQL-verificatie-bestand is opgeslagen op `/var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth`.


### <a name="authentication-file-format"></a>Verificatie-bestandsindeling
Hieronder vindt u de indeling voor het bestand MySQL OMI-verificatie

    [Port]=[Bind-Address], [username], [Base64 encoded Password]
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    AutoUpdate=[true|false]

De vermeldingen in het verificatiebestand worden in de volgende tabel beschreven.

| Eigenschap | Beschrijving |
|:--|:--|
| Poort | Hiermee geeft u de huidige poort luistert het MySQL-exemplaar op. Poort 0 geeft aan dat de volgende eigenschappen worden gebruikt voor het standaardexemplaar. |
| BIND-adres| Huidige MySQL bind-adres. |
| gebruikersnaam| MySQL-gebruiker gebruikt om te gebruiken voor het bewaken van de MySQL-server-exemplaar. |
| Base64-gecodeerd wachtwoord| Wachtwoord van de MySQL bewaking gebruiker gecodeerd in Base64. |
| Automatisch bijwerken| Geeft aan of opnieuw scannen op wijzigingen in het bestand my.cnf en het bestand MySQL OMI verificatie overschrijven wanneer de MySQL OMI-Provider is bijgewerkt. |

### <a name="default-instance"></a>Standaard-instantie
Het bestand MySQL OMI-verificatie kunt definiëren een standaard exemplaar en het poortnummer nummer zodat meerdere exemplaren van MySQL op een Linux-host eenvoudiger beheren.  Het standaardexemplaar wordt aangeduid door een instantie met poort 0. Alle extra exemplaren worden overgenomen eigenschappen instellen van het standaardexemplaar, tenzij ze verschillende waarden opgeven. Bijvoorbeeld, als MySQL exemplaar luisteren op poort '3308' wordt toegevoegd, wordt het standaardexemplaar bind-adres, gebruikersnaam en wachtwoord Base64-gecodeerd worden gebruikt om te controleren van het exemplaar op 3308 luistert. Als het exemplaar op 3308 is gebonden aan een ander adres en dezelfde MySQL gebruikersnaam en wachtwoord twee gebruikt alleen de binding-adres nodig is en de overige eigenschappen worden overgenomen.

De volgende tabel bevat voorbeeld exemplaar instellingen 

| Beschrijving | File |
|:--|:--|
| Standaardexemplaar en -exemplaar met poort 3308. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=, ,`<br>`AutoUpdate=true` |
| Standaardexemplaar en -exemplaar met poort 3308 en andere gebruikersnaam en wachtwoord. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=127.0.1.1, myuser2,cGluaGVhZA==`<br>`AutoUpdate=true` |


### <a name="mysql-omi-authentication-file-program"></a>Programma voor verificatie bestand MySQL OMI
Is opgenomen met de installatie van de provider MySQL OMI een MySQL OMI verificatie programma dat kan worden gebruikt voor het bewerken van het bestand MySQL OMI verificatie. Het programma voor verificatie-bestand vindt op de volgende locatie.

    /opt/microsoft/mysql-cimprov/bin/mycimprovauth

> [!NOTE]
> Het referentiebestand moet worden gelezen door de omsagent-account. Met de opdracht mycimprovauth als omsgent wordt aanbevolen.

De volgende tabel biedt details over de syntaxis voor het gebruik van mycimprovauth.

| Bewerking | Voorbeeld | Beschrijving
|:--|:--|:--|
| AutoUpdate * false\|waar * | mycimprovauth autoupdate false | Stelt de verificatiebestand al dan niet automatisch bijgewerkt op opnieuw opstarten of bijwerken. |
| standaard *gebruikersnaamwachtwoord bind-adres* | mycimprovauth standaard 127.0.0.1 hoofdmap pwd | Hiermee stelt u het standaardexemplaar in de MySQL OMI verificatiebestand.<br>Het wachtwoordveld moet worden ingevoerd als tekst zonder opmaak: het wachtwoord in het bestand MySQL OMI-verificatie wordt Base 64 gecodeerde. |
| Verwijder * default\|port_num * | mycimprovauth 3308 | Verwijdert het opgegeven exemplaar van een standaard of door poortnummer. |
| Help | mycimprov help | Afdrukken een lijst met opdrachten te gebruiken. |
| afdrukken | mycimprov afdrukken | Afdrukken een gemakkelijk te lezen MySQL OMI verificatiebestand. |
| bijwerken van port_num *gebruikersnaamwachtwoord bind-adres* | mycimprov update 3307 127.0.0.1 hoofdmap pwd | Het opgegeven exemplaar van updates of het exemplaar wordt toegevoegd als deze niet bestaat. |

De volgende voorbeeldopdrachten wordt een standaard-gebruikersaccount voor de MySQL-server op localhost definiëren.  Het wachtwoordveld moet worden ingevoerd als tekst zonder opmaak: het wachtwoord in het bestand MySQL OMI-verificatie worden Base 64 gecodeerde

    sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>'
    sudo /opt/omi/bin/service_control restart

### <a name="database-permissions-required-for-mysql-performance-counters"></a>Databasemachtigingen vereist voor de MySQL-prestatiemeteritems
De gebruiker MySQL vereist toegang tot de volgende query's voor het verzamelen van prestatiegegevens MySQL-Server. 

    SHOW GLOBAL STATUS;
    SHOW GLOBAL VARIABLES:


De gebruiker MySQL vereist ook Selecteer toegang tot de volgende standaardtabellen.

- INFORMATION_SCHEMA
- MySQL. 

Deze rechten kunnen worden verleend met de volgende grant-opdrachten.

    GRANT SELECT ON information_schema.* TO ‘monuser’@’localhost’;
    GRANT SELECT ON mysql.* TO ‘monuser’@’localhost’;


> [!NOTE]
> Om machtigingen te verlenen aan een MySQL hebben bewaking gebruiker de gebruiker verlenen de bevoegdheid 'GRANT optie', evenals de bevoegdheid wordt verleend.

### <a name="define-performance-counters"></a>Prestatiemeteritems definiëren

Zodra u de OMS-Agent voor Linux gegevens verzenden naar logboekanalyse configureert, moet u de prestatiemeteritems voor het verzamelen van configureren.  Gebruik de procedure in [Windows en Linux prestaties gegevensbronnen in logboekanalyse](log-analytics-data-sources-windows-events.md) met de items in de volgende tabel.

| Objectnaam | Naam van het meteritem |
|:--|:--|
| MySQL-database | Schijfruimte in Bytes |
| MySQL-database | Tabellen |
| MySQL-Server | Pct afgebroken verbinding |
| MySQL-Server | Verbinding gebruik Pct |
| MySQL-Server | Schijfruimtegebruik in Bytes |
| MySQL-Server | Volledige tabel Scan Pct |
| MySQL-Server | InnoDB buffergroep bereikt Pct |
| MySQL-Server | InnoDB groep gebruik bufferpercentage |
| MySQL-Server | InnoDB groep gebruik bufferpercentage |
| MySQL-Server | Sleutel Trefferfrequentie Pct |
| MySQL-Server | Cache voor gebruik Pct |
| MySQL-Server | Cache voor Write-Pct |
| MySQL-Server | Query-Cache treffers Pct |
| MySQL-Server | Query-Cache Prunes Pct |
| MySQL-Server | Query-Cache gebruiken Pct |
| MySQL-Server | Trefferfrequentie in tabel Pct |
| MySQL-Server | Tabel Cache gebruik Pct |
| MySQL-Server | Tabel vergrendelen conflicten Pct |

## <a name="apache-http-server"></a>Apache HTTP-Server 
Als de Apache HTTP-Server op de computer wordt gedetecteerd wanneer de bundel omsagent is geïnstalleerd, wordt een provider voor de Apache HTTP-Server voor prestatiebewaking automatisch geïnstalleerd. Deze provider is afhankelijk van een Apache-module om prestatiegegevens toegang te krijgen in de Apache HTTP-Server moet worden geladen. De module kan worden geladen met de volgende opdracht:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

Als u wilt verwijderen (Unload) van de Apache-bewakingsmodule, voer de volgende opdracht:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```

### <a name="define-performance-counters"></a>Prestatiemeteritems definiëren

Zodra u de OMS-Agent voor Linux gegevens verzenden naar logboekanalyse configureert, moet u de prestatiemeteritems voor het verzamelen van configureren.  Gebruik de procedure in [Windows en Linux prestaties gegevensbronnen in logboekanalyse](log-analytics-data-sources-windows-events.md) met de items in de volgende tabel.

| Objectnaam | Naam van het meteritem |
|:--|:--|
| Apache HTTP-Server | Bezet werknemers |
| Apache HTTP-Server | Niet-actieve werknemers |
| Apache HTTP-Server | PCT bezet werknemers |
| Apache HTTP-Server | Totaal aantal Pct CPU |
| Apache virtuele Host | Fouten per minuut - Client |
| Apache virtuele Host | Fouten per minuut - Server |
| Apache virtuele Host | KB per aanvraag |
| Apache virtuele Host | Aanvragen KB per seconde |
| Apache virtuele Host | Aanvragen per seconde |



## <a name="next-steps"></a>Volgende stappen
* [Verzamelen van prestatiemeteritems](log-analytics-data-sources-performance-counters.md) van Linux-agents.
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om de gegevens verzameld van gegevensbronnen en oplossingen te analyseren. 