---
title: Verzamelen van prestaties van de Linux-toepassingen in Log Analytics | Microsoft Docs
description: Dit artikel bevat informatie voor het configureren van de Log Analytics-agent voor Linux voor het verzamelen van prestatiemeteritems voor MySQL en Apache HTTP-Server.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: bf14e06f52f1b5a32ea3922083cc1f9bdbfb2aae
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54104842"
---
# <a name="collect-performance-counters-for-linux-applications-in-log-analytics"></a>Verzamelen van prestatiemeteritems voor Linux-toepassingen in Log Analytics 
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]
Dit artikel bevat informatie voor het configureren van de [Log Analytics-agent voor Linux](https://github.com/Microsoft/OMS-Agent-for-Linux) voor het verzamelen van prestatiemeteritems voor specifieke toepassingen in Log Analytics.  De toepassingen die zijn opgenomen in dit artikel zijn:  

- [MySQL](#MySQL)
- [Apache HTTP-Server](#apache-http-server)

## <a name="mysql"></a>MySQL
Als de MySQL-Server of MariaDB-Server wordt gedetecteerd op de computer wanneer de Log Analytics-agent is geïnstalleerd, wordt een provider voor MySQL-Server voor prestatiebewaking automatisch geïnstalleerd. Deze provider maakt verbinding met de lokale MySQL/MariaDB-server om beschikbaar te stellen van statistieken over de prestaties. De referenties van de MySQL-gebruiker moeten worden geconfigureerd zodat de provider toegang heeft tot de MySQL-Server.

### <a name="configure-mysql-credentials"></a>MySQL-referenties configureren
De MySQL OMI-provider vereist dat een vooraf geconfigureerde MySQL-gebruiker en MySQL-clientbibliotheken geïnstalleerd om een query uitvoeren op de prestaties en statusgegevens van de MySQL-exemplaar.  Deze referenties worden opgeslagen in een verificatiebestand dat opgeslagen op de Linux-agent.  Het verificatiebestand geeft aan welke bind-adres en poort van de MySQL-exemplaar luistert op en welke referenties te gebruiken voor het verzamelen van metrische gegevens.  

Tijdens de installatie van de Log Analytics-agent voor Linux de OMI MySQL provider scant MySQL my.cnf-configuratiebestanden (standaardlocaties) voor binding-adres en poort en het verificatiebestand MySQL OMI gedeeltelijk instellen.

De MySQL-verificatie-bestand is opgeslagen op `/var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth`.


### <a name="authentication-file-format"></a>Verificatie-bestandsindeling
Hieronder volgt de indeling voor het verificatiebestand MySQL OMI

    [Port]=[Bind-Address], [username], [Base64 encoded Password]
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    AutoUpdate=[true|false]

De vermeldingen in het verificatiebestand worden beschreven in de volgende tabel.

| Eigenschap | Description |
|:--|:--|
| Poort | Hiermee geeft u de huidige poort op de MySQL-exemplaar luistert. Poort 0 geeft aan dat de volgende eigenschappen worden gebruikt voor het standaardexemplaar. |
| Binding-adres| Huidige MySQL-bind-adres. |
| gebruikersnaam| De MySQL-gebruiker is gebruikt om te gebruiken voor het bewaken van de MySQL-server-exemplaar. |
| Base64-gecodeerd wachtwoord| Het wachtwoord van de MySQL-gebruiker bewaking gecodeerd in Base64. |
| Automatisch bijwerken| Hiermee geeft u op of u wilt opnieuw scannen op wijzigingen in het bestand my.cnf en het verificatiebestand van MySQL OMI overschreven wanneer de MySQL-OMI-Provider is bijgewerkt. |

### <a name="default-instance"></a>Standaardexemplaar
Het verificatiebestand MySQL OMI kunt een standaard exemplaar en het poortnummer dat voor het beheer van meerdere exemplaren van MySQL op een Linux-host eenvoudiger definiëren.  Het standaardexemplaar wordt aangeduid met een exemplaar met poort 0. Alle extra exemplaren wordt nemen de eigenschappen instellen vanuit het standaardexemplaar, tenzij ze verschillende waarden opgeven. Bijvoorbeeld, als MySQL-exemplaar luistert op poort '3308' wordt toegevoegd, wordt het standaardexemplaar bind-adres, gebruikersnaam en wachtwoord van de met Base64 gecodeerde worden gebruikt om te controleren van het exemplaar 3308 luisteren. Als het exemplaar op 3308 is gebonden aan een ander adres en dezelfde MySQL gebruikersnaam en wachtwoord twee gebruikt alleen de binding-adres is vereist en de andere eigenschappen, worden overgenomen.

De volgende tabel bevat instellingen voor voorbeeld-exemplaar 

| Description | File |
|:--|:--|
| Standaardexemplaar en -exemplaar met poort 3308. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=, ,`<br>`AutoUpdate=true` |
| Standaardexemplaar en -exemplaar met poort 3308 en andere gebruikersnaam en wachtwoord. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=127.0.1.1, myuser2,cGluaGVhZA==`<br>`AutoUpdate=true` |


### <a name="mysql-omi-authentication-file-program"></a>Programma voor MySQL OMI verificatie-bestand
Inbegrepen bij de installatie van de MySQL OMI-provider is een MySQL OMI verificatie bestand-programma dat kan worden gebruikt voor het bewerken van het verificatiebestand van MySQL OMI. Het programma voor verificatie-bestand kan worden gevonden op de volgende locatie.

    /opt/microsoft/mysql-cimprov/bin/mycimprovauth

> [!NOTE]
> Het bestand moet worden gelezen door de omsagent-account. Met de opdracht mycimprovauth als omsgent wordt aanbevolen.

De volgende tabel bevat details over de syntaxis voor het gebruik van mycimprovauth.

| Bewerking | Voorbeeld | Description
|:--|:--|:--|
| AutoUpdate *false of true* | mycimprovauth autoupdate false | Stelt het verificatiebestand wel of niet automatisch bijgewerkt op opnieuw starten of bijwerken. |
| standaard *bind-address gebruikersnaam wachtwoord* | mycimprovauth standaard 127.0.0.1 hoofdmap pwd | Hiermee stelt u het standaardexemplaar plaatst in het OMI MySQL verificatiebestand.<br>Het wachtwoordveld moet worden ingevoerd als tekst zonder opmaak: het wachtwoord in het verificatiebestand MySQL OMI is Base-64 gecodeerd. |
| Verwijder *standaard of port_num* | mycimprovauth 3308 | Hiermee verwijdert het opgegeven exemplaar van een standaard of door poortnummer. |
| Help | mycimprov help | Een lijst met opdrachten voor het gebruik van afdrukken. |
| Afdrukken | mycimprov afdrukken | Afdrukken van een gemakkelijk leesbare MySQL OMI verificatiebestand. |
| bijwerken van port_num *bind-address gebruikersnaam wachtwoord* | mycimprov update 3307 127.0.0.1 hoofdmap pwd | Het opgegeven exemplaar van updates of het exemplaar wordt toegevoegd als deze niet bestaat. |

De volgende voorbeeldopdrachten wordt een standaard-gebruikersaccount voor de MySQL-server op localhost definiëren.  Het wachtwoordveld moet worden ingevoerd als tekst zonder opmaak: het wachtwoord in het verificatiebestand MySQL OMI is Base-64 gecodeerd

    sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>'
    sudo /opt/omi/bin/service_control restart

### <a name="database-permissions-required-for-mysql-performance-counters"></a>Databasemachtigingen die vereist zijn voor MySQL-prestatiemeteritems
De MySQL-gebruiker is vereist voor toegang tot de volgende query's voor het verzamelen van prestatiegegevens van de MySQL-Server. 

    SHOW GLOBAL STATUS;
    SHOW GLOBAL VARIABLES:


De MySQL-gebruiker moet ook SELECT-toegang tot de volgende standaardtabellen.

- information_schema
- MySQL. 

Deze rechten kunnen worden verleend door het uitvoeren van de volgende opdrachten voor verlenen.

    GRANT SELECT ON information_schema.* TO ‘monuser’@’localhost’;
    GRANT SELECT ON mysql.* TO ‘monuser’@’localhost’;


> [!NOTE]
> Om machtigingen te verlenen aan een MySQL moet bewaking gebruiker de verlenen gebruiker de bevoegdheid 'Verlenen optie', evenals de bevoegdheid wordt verleend.

### <a name="define-performance-counters"></a>Prestatiemeteritems definiëren

Nadat u de Log Analytics-agent voor Linux om gegevens te verzenden naar Log Analytics configureren, moet u de prestatiemeteritems voor het verzamelen van configureren.  Gebruik de procedure in [Windows en Linux-gegevensbronnen van de prestaties die u in Log Analytics](data-sources-performance-counters.md) met de items in de volgende tabel.

| Objectnaam | Naam van teller |
|:--|:--|
| MySQL-database | Schijfruimte in Bytes |
| MySQL-database | Tabellen |
| MySQL-server | Afgebroken verbinding Pct |
| MySQL-server | Pct voor verbinding gebruiken |
| MySQL-server | Gebruik van de schijf in Bytes |
| MySQL-server | Volledige tabel Scan Pct |
| MySQL-server | InnoDB buffergroep bereikt Pct |
| MySQL-server | InnoDB Pool gebruik bufferpercentage |
| MySQL-server | InnoDB Pool gebruik bufferpercentage |
| MySQL-server | Belangrijkste cachetreffers Pct |
| MySQL-server | Belangrijkste Cache gebruik Pct |
| MySQL-server | Cache voor Write Pct |
| MySQL-server | Query Cache treffers Pct |
| MySQL-server | Query Cache Prunes Pct |
| MySQL-server | Query-Cache gebruiken Pct |
| MySQL-server | Tabel cachetreffers Pct |
| MySQL-server | Tabel Cache gebruik Pct |
| MySQL-server | Tabel vergrendelen conflicten Pct |

## <a name="apache-http-server"></a>Apache HTTP-Server 
Als Apache HTTP-Server op de computer wordt gedetecteerd wanneer de omsagent-bundel is geïnstalleerd, wordt een provider voor de Apache HTTP Server voor prestatiebewaking automatisch geïnstalleerd. Deze provider, is afhankelijk van een Apache-module die moet worden geladen in de Apache HTTP-Server voor toegang tot prestatiegegevens. De module kan worden geladen met de volgende opdracht:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

Als u wilt verwijderen van de Apache-bewakingsmodule, voer de volgende opdracht:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```

### <a name="define-performance-counters"></a>Prestatiemeteritems definiëren

Nadat u de Log Analytics-agent voor Linux om gegevens te verzenden naar Log Analytics configureren, moet u de prestatiemeteritems voor het verzamelen van configureren.  Gebruik de procedure in [Windows en Linux-gegevensbronnen van de prestaties die u in Log Analytics](data-sources-performance-counters.md) met de items in de volgende tabel.

| Objectnaam | Naam van teller |
|:--|:--|
| Apache HTTP-Server | Werknemers bezet |
| Apache HTTP-Server | Niet-actieve werknemers |
| Apache HTTP-Server | PCT bezet werknemers |
| Apache HTTP-Server | Totaal aantal Pct CPU |
| Virtuele Host van Apache | Fouten per minuut - Client |
| Virtuele Host van Apache | Fouten per minuut - Server |
| Virtuele Host van Apache | KB per aanvraag |
| Virtuele Host van Apache | Aanvragen KB per seconde |
| Virtuele Host van Apache | Aanvragen per seconde |



## <a name="next-steps"></a>Volgende stappen
* [Verzamelen van prestatiemeteritems](data-sources-performance-counters.md) van Linux-agents.
* Meer informatie over [query's bijgehouden](../../log-analytics/log-analytics-queries.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren. 
