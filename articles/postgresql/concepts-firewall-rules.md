---
title: Azure Database for PostgreSQL-Server firewall-regels
description: Dit artikel beschrijft de firewallregels voor uw Azure Database for PostgreSQL-server.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 793a68ee829d87a8433d5bdd77fe7b43e2f3af19
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548830"
---
# <a name="azure-database-for-postgresql-server-firewall-rules"></a>Azure Database for PostgreSQL-Server firewall-regels
Azure Database for PostgreSQL-Server-firewall voorkomt alle toegang tot uw databaseserver, totdat u opgeeft welke computers zijn gemachtigd. De firewall verleent toegang tot de server op basis van het oorspronkelijke IP-adres van elke aanvraag.
U configureert de firewall door firewallregels te maken die bereiken opgeven van acceptabele IP-adressen. U kunt firewallregels op serverniveau maken.

**Firewall-regels:** Deze regels zorgen ervoor dat clients toegang krijgen tot uw hele Azure Database for PostgreSQL-Server, dat wil zeggen, alle databases binnen dezelfde logische server. Firewallregels op serverniveau kunnen worden geconfigureerd met behulp van de Azure-portal of met behulp van Azure CLI-opdrachten. Voor het maken van firewallregels op serverniveau, moet u eigenaar van het abonnement of een bijdrager aan het abonnement.

## <a name="firewall-overview"></a>Firewalloverzicht
Alle databasetoegang tot uw Azure Database for PostgreSQL-server is standaard geblokkeerd door de firewall. Als u wilt beginnen met behulp van de server vanaf een andere computer, moet u een of meer firewallregels op serverniveau regels voor toegang tot uw server op te geven. Gebruik de firewallregels om op te geven welke IP-adresbereiken van Internet om toe te staan. Toegang tot de Azure portal-website zelf wordt niet beïnvloed door de firewall-regels.
Verbindingspogingen van Internet en Azure moeten eerst de firewall passeren voordat ze de PostgreSQL-Database kunnen bereiken zoals wordt weergegeven in het volgende diagram:

![Voorbeeld van de stroom van de werking van de firewall](media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>Verbinding maken vanuit internet
Firewallregels op serverniveau zijn van toepassing op alle databases op dezelfde Azure Database for PostgreSQL-server. Als het IP-adres van de aanvraag binnen een van de bereiken ligt die zijn opgegeven in de firewallregels op serverniveau, wordt de verbinding toegestaan.
Als het IP-adres van de aanvraag niet binnen de bereiken die in de firewallregels op serverniveau zijn opgegeven, mislukt de verbindingsaanvraag.
Bijvoorbeeld, als uw toepassing verbinding met JDBC-stuurprogramma voor PostgreSQL maakt, u deze fout kan optreden wanneer de verbinding wordt geblokkeerd door de firewall verbinding proberen te maken.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: Onherstelbare fout: Er is geen pg\_hba.conf-vermelding voor de host ": 123.45.67.890", gebruiker "adminuser', database 'postgresql", SSL

## <a name="connecting-from-azure"></a>Verbinding maken vanuit Azure
Als u wilt toestaan dat toepassingen vanuit Azure verbinding kunnen maken met uw Azure Database for PostgreSQL-server, moeten de Azure-verbindingen zijn ingeschakeld. Bijvoorbeeld als host voor een Azure Web Apps-toepassing of een toepassing die wordt uitgevoerd in een Azure-VM, of verbinding te maken van een Azure Data Factory data management gateway. De resources hoeft te worden in het hetzelfde Virtueelnetwerk (VNet) of de resourcegroep voor de firewallregel om in te schakelen deze verbindingen. Wanneer een toepassing vanuit Azure probeert verbinding te maken met uw databaseserver, verifieert de firewall of Azure-verbindingen zijn toegestaan. Er zijn een aantal methoden voor het inschakelen van deze typen verbindingen. Een firewallinstelling waarvan het begin- en eindadres gelijk zijn aan 0.0.0.0 geeft aan dat deze verbindingen zijn toegestaan. U kunt ook instellen de **toegang tot Azure-services toestaan** optie naar **ON** in de portal van de **verbindingsbeveiliging** deelvenster en druk op **Opslaan**. Als de verbindingspoging niet is toegestaan, wordt in de aanvraag de Azure Database for PostgreSQL-server niet bereiken.

> [!IMPORTANT]
> Met deze optie configureert u de firewall zo dat alle verbindingen vanuit Azure zijn toegestaan, inclusief verbindingen vanuit de abonnementen van andere klanten. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.
> 

![Toegang tot Azure-services toestaan in de portal configureren](media/concepts-firewall-rules/allow-azure-services.png)

## <a name="programmatically-managing-firewall-rules"></a>Firewallregels programmatisch beheren
Naast de Azure portal, firewall-regels, worden beheerd via een programma met behulp van Azure CLI.
Zie ook [maken en beheren van Azure Database voor PostgreSQL-firewallregels met behulp van Azure CLI](howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-the-database-server-firewall"></a>Het oplossen van de firewall van de database-server
De volgende punten overwegen wanneer u toegang tot de Microsoft Azure-Database voor PostgreSQL-Server-service niet meer werkt zoals verwacht:

* **Wijzigingen aan de acceptatielijst zijn nog niet doorgevoerd:** Mogelijk zijn er maar liefst vijf minuten vertraging voor wijzigingen in de Azure Database for PostgreSQL-Server firewall-configuratie te activeren.

* **De aanmelding is niet gemachtigd of is een onjuist wachtwoord gebruikt:** Als een aanmelding heeft geen machtigingen voor de Azure Database for PostgreSQL-server of het wachtwoord onjuist is, wordt de verbinding met de Azure Database for PostgreSQL-server is geweigerd. Het maken van een firewallinstelling biedt alleen clients met een kans om verbinding te maken met uw server. elke client moet nog steeds de benodigde beveiligingsreferenties opgeven.

Bijvoorbeeld kan met behulp van een JDBC-client, de volgende fout optreden.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: Onherstelbare fout: wachtwoordverificatie is mislukt voor gebruiker "uwgebruikersnaam"

* **Dynamische IP-adres:** Als u een internetverbinding met dynamische IP-adressering hebt en u problemen ondervindt bij het ophalen van via de firewall, kan u een van de volgende oplossingen proberen:

* Vraag uw Internetproviderverbindingen (ISP) voor het IP-adresbereik is toegewezen aan uw clientcomputers die toegang hebben tot de Azure Database for PostgreSQL-Server en vervolgens als een firewall-regel toevoegen het IP-adresbereik.

* Ophalen van statische IP-adressen in plaats daarvan voor uw clientcomputers en voeg vervolgens het statische IP-adres toe als een firewall-regel.

## <a name="next-steps"></a>Volgende stappen
Voor artikelen over het maken van firewallregels op serverniveau en databaseniveau, Zie:
* [Maken en beheren van Azure Database voor PostgreSQL-firewallregels met behulp van de Azure portal](howto-manage-firewall-using-portal.md)
* [Maken en beheren van Azure-Database voor firewallregels PostgreSQL met Azure CLI](howto-manage-firewall-using-cli.md)
