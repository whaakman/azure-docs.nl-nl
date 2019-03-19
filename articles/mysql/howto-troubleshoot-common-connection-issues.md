---
title: Oplossen van verbindingsproblemen met Azure Database for MySQL | Microsoft Docs
description: Informatie over het oplossen van verbindingsproblemen met Azure Database voor MySQL.
keywords: MySQL-verbinding, verbindingsreeks, problemen met de netwerkverbinding, tijdelijke fout, -verbindingsfout
author: jan-eng
ms.author: janeng
ms.service: mysql
ms.topic: conceptual
ms.date: 11/09/2018
ms.openlocfilehash: faf378a81a6db24acc676bed82fe495cfb108612
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57855517"
---
# <a name="troubleshoot-connection-issues-to-azure-database-for-mysql"></a>Oplossen van verbindingsproblemen met Azure Database for MySQL

Problemen met de verbinding kunnen worden veroorzaakt door een aantal dingen, met inbegrip van:

* Firewallinstellingen
* Verbindingstime-out
* Onjuiste aanmeldingsgegevens
* Maximumlimiet bereikt in een Azure Database for MySQL-resources
* Problemen met de infrastructuur van de service
* Onderhoud wordt uitgevoerd in de service
* De compute-toewijzing van de server wordt door het schalen van het aantal vCores of verplaatsen naar een andere service-laag gewijzigd

Verbindingsproblemen met Azure Database voor MySQL kunnen over het algemeen als volgt worden ingedeeld:

* Tijdelijke fouten (tijdelijke of onregelmatige)
* Permanente of niet-tijdelijke fouten (fouten regelmatig terugkerende)

## <a name="troubleshoot-transient-errors"></a>Tijdelijke fouten oplossen

Tijdelijke fouten optreden bij het onderhoud is uitgevoerd, het systeem er een fout optreedt bij de hardware of software of wijzigen van de laag vCores of service van uw server. De Azure Database for MySQL-service heeft ingebouwde hoge beschikbaarheid en is ontworpen om dit soort problemen automatisch oplossen. Echter, uw toepassing verliest de verbinding met de server voor een korte periode van meestal minder dan 60 seconden maximaal. Sommige gebeurtenissen kunnen van tijd tot tijd beperken, zoals wanneer een grote transactie zorgt ervoor het herstel van een langlopende dat langer duren.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Stappen voor het oplossen van problemen met de tijdelijke netwerkverbinding

1. Controleer de [servicedashboard van Microsoft Azure](https://azure.microsoft.com/status) bekende storingen die zijn opgetreden tijdens de periode waarin de fouten zijn gemeld door de toepassing.
2. Toepassingen die verbinding met een cloudservice maken zoals Azure Database for MySQL moet verwachten van tijdelijke fouten en implementeren van logica voor het afhandelen van deze fouten in plaats van deze als toepassingsfouten aan gebruikers zichtbaar te maken voor opnieuw proberen. Beoordeling [afhandeling van tijdelijke connectiviteitsfouten voor Azure Database voor MySQL](concepts-connectivity.md) voor best practices en ontwerprichtlijnen voor het afhandelen van tijdelijke fouten.
3. Als een server de resourcelimieten nadert, fouten kunnen lijkt het alsof tijdelijk verbindingsprobleem. Zie [beperkingen in Azure Database for MySQL](concepts-limits.md).
4. Als u problemen met de netwerkverbinding gaan, of als de duur die uw toepassing de fout optreedt groter is dan 60 seconden, of als er meerdere exemplaren van de fout in een bepaalde dag een ondersteuningsaanvraag indienen voor Azure-bestand door te selecteren **ondersteuning krijgen voor**op de [ondersteuning voor Azure](https://azure.microsoft.com/support/options) site.

## <a name="troubleshoot-persistent-errors"></a>Permanente fouten oplossen

Als de toepassing niet permanent verbinding maken met Azure Database for MySQL, duidt dit meestal op een probleem met een van de volgende:

* Server firewall-configuratie: Zorg ervoor dat de Azure Database for MySQL serverfirewall is geconfigureerd voor verbindingen van uw client, met inbegrip van de WAP-servers en gateways.
* Firewall-configuratie van client: De firewall op de client moet toestaan verbindingen met uw database-server. IP-adressen en poorten van de server die is niet mogelijk om moeten worden toegestaan en de toepassingsnamen zoals MySQL in sommige firewalls.
* Gebruikersfout: U hebt verbindingsparameters, zoals de naam van de server in de verbindingsreeks of een ontbrekend verkeerd  *\@servername* -achtervoegsel in de naam van de gebruiker.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Stappen voor het oplossen van problemen met de permanente netwerkverbinding

1. Instellen van [firewall-regels](howto-manage-firewall-using-portal.md) waarmee de client-IP-adres. Voor tijdelijke alleen voor testdoeleinden, stelt u een firewallregel met behulp van 0.0.0.0 als de IP-beginadres en 255.255.255.255 als het laatste IP-adres gebruiken. Hiermee opent u de server aan alle IP-adressen. Als dit het probleem met de netwerkverbinding is opgelost, verwijdert u deze regel en een firewallregel voor een op de juiste wijze beperkt IP-adres of adresbereik maken.
2. Zorg ervoor dat poort 3306 geopend voor uitgaande verbindingen is op alle firewalls tussen de client en het internet.
3. Controleer of de verbindingsreeks en andere instellingen. Beoordeling [toepassingen met Azure Database voor MySQL koppelen](howto-connection-string.md).
4. Controleer de status van de service in het dashboard. Als u denkt er is een regionale onderbreking dat, Zie [overzicht van bedrijfscontinuïteit met Azure Database for MySQL](concepts-business-continuity.md) voor stappen om te herstellen naar een nieuwe regio.

## <a name="next-steps"></a>Volgende stappen

* [Afhandeling van tijdelijke connectiviteitsfouten voor Azure Database for MySQL](concepts-connectivity.md)
