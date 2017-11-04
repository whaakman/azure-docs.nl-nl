---
title: Azure-Database voor firewallregels voor server MySQL | Microsoft Docs
description: Beschrijft de firewallregels voor uw Azure-Database voor de MySQL-server.
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/20/2017
ms.openlocfilehash: aea561b526d6f3f818fd75771dd8c65c9f25051a
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2017
---
# <a name="azure-database-for-mysql-server-firewall-rules"></a>Azure voor firewallregels voor server MySQL-Database
Firewalls verhinderen alle toegang met de databaseserver tot u opgeven welke computers over machtigingen beschikken. De firewall verleent toegang tot de server op basis van het oorspronkelijke IP-adres van elke aanvraag.

Maak voor het configureren van een firewall, firewallregels die acceptabele IP-adresbereiken specificeren. U kunt firewallregels op serverniveau maken.

**Firewall-regels:** deze regels kunnen clients toegang tot uw volledige Azure-Database voor de server, MySQL, dat wil zeggen, alle databases binnen dezelfde logische server. Firewallregels op serverniveau kunnen worden geconfigureerd met behulp van de Azure-portal of Azure CLI-opdrachten. U moet eigenaar van het abonnement of een bijdrager abonnement zijn voor het maken van de firewallregels op serverniveau.

## <a name="firewall-overview"></a>Firewalloverzicht
Alle databasetoegang tot uw Azure-Database MySQL-server is standaard geblokkeerd door de firewall. Om te beginnen met behulp van uw server uit een andere computer, moet u een of meer serverniveau firewallregels voor toegang tot uw server opgeven. Gebruik de firewallregels om op te geven welke IP-adresbereiken van het Internet om toe te staan. Toegang tot de Azure portal-website zelf wordt niet beïnvloed door de firewall-regels.

Verbindingspogingen van Internet en Azure moeten eerst doorgeven via de firewall voordat ze uw Azure-Database voor de MySQL-database bereiken kunnen, zoals wordt weergegeven in het volgende diagram:

![Voorbeeld van de stroom van de werking van de firewall](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>Verbinding maken vanuit internet
Firewallregels op serverniveau gelden voor alle databases op de Azure-Database voor de MySQL-server.

Als het IP-adres van de aanvraag binnen een van de bereiken die is opgegeven in de firewallregels op serverniveau, wordt de verbinding verleend.

Als het IP-adres van de aanvraag buiten de bereiken die in de database- of server niveau firewallregels zijn opgegeven is, mislukt de verbindingsaanvraag.

## <a name="programmatically-managing-firewall-rules"></a>Firewallregels programmatisch beheren
Naast de Azure portal worden firewallregels programmatisch beheerd met behulp van de Azure CLI. Zie ook [maken en beheren van Azure-Database voor firewallregels MySQL met Azure CLI](./howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-the-database-firewall"></a>Problemen met de databasefirewall oplossen
Houd rekening met de volgende punten wanneer toegang tot de Microsoft Azure-Database voor MySQL server-service niet meer werkt zoals verwacht:

* **Wijzigingen aan de lijst zijn niet van kracht:** mogelijk zijn er zoveel een vertraging vijf minuten voor in de Azure-Database voor de firewallconfiguratie MySQL-Server wijzigingen te activeren.

* **De aanmelding is niet gemachtigd of een onjuist wachtwoord is gebruikt:** als een aanmelding heeft geen machtigingen voor de Azure-Database voor de MySQL-server of het wachtwoord onjuist is, de verbinding met de Azure-Database voor de MySQL-server is geweigerd. Door een firewallinstellingen te maken, krijgen clients alleen de mogelijkheid om te proberen verbinding te maken met de server. Elke client moet alsnog de benodigde beveiligingsreferenties opgeven.

* **Dynamische IP-adres:** als u een internetverbinding met dynamische IP-adressering hebt en u problemen ondervindt bij het ophalen van via de firewall, kunt u een van de volgende oplossingen proberen:

* Vraag uw serviceprovider (ISP) om het IP-adresbereik is toegewezen aan de clientcomputers die toegang hebben tot de Azure-Database voor de MySQL-server en vervolgens het IP-adresbereik toevoegen als een firewallregel.

* Vraag in plaats daarvan statische IP-adressen voor uw clientcomputers en voeg de IP-adressen als firewallregels toe.

## <a name="next-steps"></a>Volgende stappen

[Maken en beheren van Azure-Database voor firewallregels MySQL met de Azure portal](./howto-manage-firewall-using-portal.md)
[maken en beheren van Azure-Database voor firewallregels MySQL met Azure CLI](./howto-manage-firewall-using-cli.md)
