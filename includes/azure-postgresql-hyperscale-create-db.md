---
title: bestand opnemen
description: bestand opnemen
author: jonels-msft
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: include
ms.date: 05/14/2019
ms.author: jonels
ms.custom: include file
ms.openlocfilehash: c07e352288d7dc1d0bf198fd74c8baaded3a2d23
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65791332"
---
Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-database-for-postgresql---hyperscale-citus"></a>Maak een Azure Database for PostgreSQL - grootschalige (Citus)

Volg deze stappen voor het maken van een Azure Database voor PostgreSQL-server:
1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Selecteer **Databases** op de pagina **Nieuw** en selecteer **Azure Database voor PostgreSQL** op de pagina **Databases**.
3. Voor de Implementatieoptie, klikt u op de **maken** knop onder **servergroep grootschalige (Citus) - voorbeeld.**
4. Vul het formulier voor de gegevens van de nieuwe server als volgt in:
   - Resourcegroep: klik op de **nieuw** koppeling onder het tekstvak voor dit veld. Voer een naam zoals **myresourcegroup**.
   - Naam van servergroep: Voer een unieke naam voor de nieuwe server-groep, die ook worden gebruikt voor een subdomein van de server.
   - Gebruikersnaam van beheerder: momenteel is de waarde is vereist **citus**, en kan niet worden gewijzigd.
   - Wachtwoord: moet ten minste acht tekens lang zijn en tekens bevatten uit drie van de volgende categorieën: Nederlandse hoofdletters letters, Nederlandse kleine letters, cijfers (0-9) en niet-alfanumerieke tekens (!, $, #, %, enzovoort.)
   - Locatie: Gebruik de locatie die zich het dichtst bij uw gebruikers zodat ze de snelst mogelijke toegang tot de gegevens.

   > [!IMPORTANT]
   > Wachtwoord van de serverbeheerder die u hier opgeeft, is vereist voor aanmelding bij de server en de bijbehorende databases. Onthoud of noteer deze informatie voor later gebruik.

5. Klik op **configureren servergroep**. Laat de instellingen in die sectie ongewijzigd en klik op **opslaan**.
6. Klik op **revisie + maken** en vervolgens **maken** voor het inrichten van de server. De inrichting duurt een paar minuten.
7. De pagina wordt omgeleid voor het controleren van de implementatie. Wanneer de live status verandert van **uw implementatie wordt uitgevoerd** naar **uw implementatie is voltooid**, klikt u op de **uitvoer** menu-item aan de linkerkant van de pagina.
8. De uitvoer-pagina bevat een hostnaam coördinator met een knop naast het om de waarde naar het Klembord kopiëren. Noteer deze informatie voor later gebruik.

## <a name="configure-a-server-level-firewall-rule"></a>Een serverfirewallregel configureren

De Azure Database voor PostgreSQL – grootschalige (Citus) (preview)-service wordt gebruikt een firewall op serverniveau. Standaard de firewall voorkomt dat externe toepassingen en hulpprogramma's verbinding maken met het coördinatorknooppunt en alle databases op. Er moet een regel voor het openen van de firewall voor een specifiek IP-adresbereik toevoegen.

1. Uit de **uitvoer** sectie waarnaar u de hostnaam van het knooppunt coordinator eerder hebt gekopieerd, klik op terug naar de **overzicht** menu-item.

2. Zoek de naam van de servergroep van uw implementatie en klik erop. (De naam van de server wordt *niet* een achtervoegsel hebben. Items met namen hebben die eindigt op, bijvoorbeeld '-c ","-w0 ', of '-w1 ' zijn niet de server-groep.)

3. Klik op **Firewall** onder **Security** in het linkermenu.

4. Klik op de koppeling **+ firewallregel toevoegen voor de huidige IP-adres van client**.

5. Ten slotte klikt u op de **opslaan** knop.

   > [!NOTE]
   > De Azure PostgreSQL-server communiceert via poort 5432. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 5432 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u geen verbinding maken met uw Azure SQL Database-server, tenzij de IT-afdeling poort 5432 openstelt.
   >

## <a name="connect-to-the-database-using-psql"></a>Verbinding maken met de database met behulp van psql

Wanneer u uw Azure Database for PostgreSQL-server maakt, een standaarddatabase met de naam **citus** wordt gemaakt. Voor verbinding met uw databaseserver, moet u een verbindingsreeks en het beheerderswachtwoord.

1. Haal de verbindingsreeks. Klik in de pagina van de server-groep op de **verbindingsreeksen** menu-item. (Is onder **instellingen**.) De tekenreeks die is gemarkeerd als  **C++ (libpq)**. Dit is van het formulier:

   ```
   host=hostname.postgres.database.azure.com port=5432 dbname=citus user=citus password={your_password} sslmode=require
   ```

   Kopieer de tekenreeks. U moet vervangen door "{uw\_wachtwoord} ' met het beheerderswachtwoord dat u eerder hebt gekozen. Het systeem niet uw wachtwoord als leesbare tekst opgeslagen en dus niet weergeven voor u in de verbindingsreeks.

2. Open een terminalvenster op uw lokale computer.

3. Bij de prompt verbinding met uw Azure Database for PostgreSQL-server met de [psql](https://www.postgresql.org/docs/current/app-psql.html) hulpprogramma. Door uw verbindingsreeks door te geven tussen aanhalingstekens, wordt ervoor dat uw wachtwoord bevat:
   ```bash
   psql "{connection_string}"
   ```

   Bijvoorbeeld, de volgende opdracht maakt verbinding met het coördinatorknooppunt van de servergroep **mydemoserver**:

   ```bash
   psql "host=mydemoserver-c.postgres.database.azure.com port=5432 dbname=citus user=citus password={your_password} sslmode=require"
   ```
