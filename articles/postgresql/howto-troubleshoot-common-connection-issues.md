---
title: Oplossen van verbindingsproblemen met Azure Database for PostgreSQL | Microsoft Docs
description: Informatie over het oplossen van verbindingsproblemen met Azure Database voor PostgreSQL.
keywords: postgresql-verbinding, verbindingsreeks, problemen met de netwerkverbinding, tijdelijke fout, -verbindingsfout
services: postgresql
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/09/2018
ms.openlocfilehash: 67383db4bd1d57d194e10de2dc1964532b3619a4
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53160808"
---
# <a name="troubleshoot-connection-issues-to-azure-database-for-postgresql"></a>Oplossen van verbindingsproblemen met Azure Database for PostgreSQL

Problemen met de verbinding kunnen worden veroorzaakt door een aantal dingen, met inbegrip van:

* Firewallinstellingen
* Verbindingstime-out
* Onjuiste aanmeldingsgegevens
* Maximumlimiet bereikt in een Azure Database for PostgreSQL-resources
* Problemen met de infrastructuur van de service
* Onderhoud wordt uitgevoerd in de service
* De compute-toewijzing van de server wordt door het schalen van het aantal vCores of verplaatsen naar een andere service-laag gewijzigd

Verbindingsproblemen met Azure Database for PostgreSQL kunnen over het algemeen als volgt worden ingedeeld:

* Tijdelijke fouten (tijdelijke of onregelmatige)
* Permanente of niet-tijdelijke fouten (fouten regelmatig terugkerende)

## <a name="troubleshoot-transient-errors"></a>Tijdelijke fouten oplossen

Tijdelijke fouten optreden bij het onderhoud is uitgevoerd, het systeem er een fout optreedt bij de hardware of software of wijzigen van de laag vCores of service van uw server. De Azure Database for PostgreSQL-service heeft ingebouwde hoge beschikbaarheid en is ontworpen om dit soort problemen automatisch oplossen. Echter, uw toepassing verliest de verbinding met de server voor een korte periode van meestal minder dan 60 seconden maximaal. Sommige gebeurtenissen kunnen van tijd tot tijd beperken, zoals wanneer een grote transactie zorgt ervoor het herstel van een langlopende dat langer duren.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Stappen voor het oplossen van problemen met de tijdelijke netwerkverbinding

1. Controleer de [servicedashboard van Microsoft Azure](https://azure.microsoft.com/status) bekende storingen die zijn opgetreden tijdens de periode waarin de fouten zijn gemeld door de toepassing.
2. Toepassingen die verbinding met een cloudservice maken zoals Azure Database for PostgreSQL moet verwachten van tijdelijke fouten en implementeren van logica voor het afhandelen van deze fouten in plaats van deze als toepassingsfouten aan gebruikers zichtbaar te maken voor opnieuw proberen. Beoordeling [afhandeling van tijdelijke connectiviteitsfouten voor Azure Database voor PostgreSQL](concepts-connectivity.md) voor best practices en ontwerprichtlijnen voor het afhandelen van tijdelijke fouten.
3. Als een server de resourcelimieten nadert, fouten kunnen lijkt het alsof tijdelijk verbindingsprobleem. Zie [beperkingen in Azure Database for PostgreSQL](concepts-limits.md).
4. Als u problemen met de netwerkverbinding gaan, of als de duur die uw toepassing de fout optreedt groter is dan 60 seconden, of als er meerdere exemplaren van de fout in een bepaalde dag een ondersteuningsaanvraag indienen voor Azure-bestand door te selecteren **ondersteuning krijgen voor**op de [ondersteuning voor Azure](https://azure.microsoft.com/support/options) site.

## <a name="troubleshoot-persistent-errors"></a>Permanente fouten oplossen

Als de toepassing niet permanent verbinding maken met Azure Database for PostgreSQL, duidt dit meestal op een probleem met een van de volgende:

* Server firewall-configuratie: Zorg ervoor dat de Azure Database for PostgreSQL-server-firewall is geconfigureerd voor verbindingen van uw client, met inbegrip van de WAP-servers en gateways.
* Firewall-configuratie van client: De firewall op de client moet toestaan verbindingen met uw database-server. IP-adressen en poorten van de server die is niet mogelijk om moeten worden toegestaan en de toepassingsnamen zoals PostgreSQL in sommige firewalls.
* Gebruikersfout: U hebt verbindingsparameters, zoals de naam van de server in de verbindingsreeks of een ontbrekend verkeerd *@servername* -achtervoegsel in de naam van de gebruiker.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Stappen voor het oplossen van problemen met de permanente netwerkverbinding

1. Instellen van [firewall-regels](howto-manage-firewall-using-portal.md) waarmee de client-IP-adres. Voor tijdelijke alleen voor testdoeleinden, stelt u een firewallregel met behulp van 0.0.0.0 als de IP-beginadres en 255.255.255.255 als het laatste IP-adres gebruiken. Hiermee opent u de server aan alle IP-adressen. Als dit het probleem met de netwerkverbinding is opgelost, verwijdert u deze regel en een firewallregel voor een op de juiste wijze beperkt IP-adres of adresbereik maken.
2. Zorg ervoor dat poort 3306 geopend voor uitgaande verbindingen is op alle firewalls tussen de client en het internet.
3. Controleer of de verbindingsreeks en andere instellingen.
4. Controleer de status van de service in het dashboard. Als u denkt er is een regionale onderbreking dat, Zie [overzicht van bedrijfscontinuïteit met Azure Database for PostgreSQL](concepts-business-continuity.md) voor stappen om te herstellen naar een nieuwe regio.

## <a name="next-steps"></a>Volgende stappen

* [Afhandeling van tijdelijke connectiviteitsfouten voor Azure Database for PostgreSQL](concepts-connectivity.md)
