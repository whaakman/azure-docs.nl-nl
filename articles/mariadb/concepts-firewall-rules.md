---
title: Azure Database voor MariaDB server firewall-regels
description: Beschrijft de firewallregels voor uw Azure Database voor MariaDB-server.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 6fb9099ebfe884fc6eee58882ee23e46ba550e13
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53546245"
---
# <a name="azure-database-for-mariadb-server-firewall-rules"></a>Azure Database voor MariaDB server firewall-regels
Totdat u opgeeft welke computers zijn gemachtigd, verhinderen firewalls alle toegang tot uw database-server. De firewall verleent toegang tot de server op basis van het oorspronkelijke IP-adres van elke aanvraag.

Voor het configureren van een firewall, firewallregels die bereiken van acceptabele IP-adressen opgeven te maken. U kunt firewallregels op serverniveau maken.

**Firewall-regels:** Deze regels zorgen ervoor dat clients toegang krijgen tot uw gehele Azure-Database voor MariaDB-server, dat wil zeggen, alle databases binnen dezelfde logische server. Firewallregels op serverniveau kunnen worden geconfigureerd met behulp van de Azure portal of Azure CLI-opdrachten. Voor het maken van firewallregels op serverniveau, moet u eigenaar van het abonnement of een bijdrager aan het abonnement.

## <a name="firewall-overview"></a>Firewalloverzicht
Alle databasetoegang tot uw Azure Database voor MariaDB-server is standaard geblokkeerd door de firewall. Als u wilt beginnen met behulp van de server vanaf een andere computer, moet u een of meer firewallregels op serverniveau regels voor toegang tot uw server op te geven. Gebruik de firewallregels om op te geven welke IP-adresbereiken van Internet om toe te staan. Toegang tot de Azure portal-website zelf wordt niet beïnvloed door de firewall-regels.

Verbindingspogingen van Internet en Azure moeten eerst de firewall passeren voordat ze kunnen zich bij uw Azure Database voor MariaDB-database, zoals wordt weergegeven in het volgende diagram:

![Voorbeeld van de stroom van de werking van de firewall](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>Verbinding maken vanuit internet
Firewallregels op serverniveau zijn van toepassing op alle databases op de Azure Database voor MariaDB-server.

Als het IP-adres van de aanvraag binnen een van de bereiken die is opgegeven in de firewallregels op serverniveau, wordt de verbinding toegestaan.

Als het IP-adres van de aanvraag buiten het bereik dat is opgegeven in een van de firewallregels op databaseniveau of op serverniveau is, klikt u vervolgens de verbindingsaanvraag is mislukt.

## <a name="connecting-from-azure"></a>Verbinding maken vanuit Azure
Opdat toepassingen vanuit Azure verbinding maken met uw Azure Database voor MariaDB-server, moeten de Azure-verbindingen zijn ingeschakeld. Bijvoorbeeld als host voor een Azure Web Apps-toepassing of een toepassing die wordt uitgevoerd in een Azure-VM, of verbinding te maken van een Azure Data Factory data management gateway. De resources hoeft te worden in het hetzelfde Virtueelnetwerk (VNet) of de resourcegroep voor de firewallregel om in te schakelen deze verbindingen. Wanneer een toepassing vanuit Azure probeert verbinding te maken met uw databaseserver, verifieert de firewall of Azure-verbindingen zijn toegestaan. Er zijn een aantal methoden voor het inschakelen van deze typen verbindingen. Een firewallinstelling waarvan het begin- en eindadres gelijk zijn aan 0.0.0.0 geeft aan dat deze verbindingen zijn toegestaan. U kunt ook instellen de **toegang tot Azure-services toestaan** optie naar **ON** in de portal van de **verbindingsbeveiliging** deelvenster en druk op **Opslaan**. Als de verbindingspoging niet is toegestaan, wordt in de aanvraag de Azure Database voor MariaDB-server niet bereiken.

> [!IMPORTANT]
> Met deze optie configureert u de firewall zo dat alle verbindingen vanuit Azure zijn toegestaan, inclusief verbindingen vanuit de abonnementen van andere klanten. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.
> 

![Toegang tot Azure-services toestaan in de portal configureren](./media/concepts-firewall-rules/allow-azure-services.png)

## <a name="programmatically-managing-firewall-rules"></a>Firewallregels programmatisch beheren
Naast de Azure portal, kunnen de firewallregels programmatisch worden beheerd met behulp van de Azure CLI. 

<!--See also [Create and manage Azure Database for MariaDB firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md)-->

## <a name="troubleshooting-the-database-firewall"></a>Problemen met de databasefirewall oplossen
De volgende punten overwegen wanneer u toegang tot de Microsoft Azure-Database voor MariaDB server-service niet werkt zoals verwacht:

* **Wijzigingen aan de acceptatielijst zijn nog niet doorgevoerd:** Mogelijk zijn er maar liefst vijf minuten vertraging voor wijzigingen in de Azure Database voor MariaDB Server firewall-configuratie te activeren.

* **De aanmelding is niet gemachtigd of is een onjuist wachtwoord gebruikt:** Als een aanmelding heeft geen machtigingen voor de Azure Database voor MariaDB-server of het wachtwoord onjuist is, wordt de verbinding met de Azure Database voor MariaDB-server is geweigerd. Door een firewallinstellingen te maken, krijgen clients alleen de mogelijkheid om te proberen verbinding te maken met de server. Elke client moet alsnog de benodigde beveiligingsreferenties opgeven.

* **Dynamische IP-adres:** Als u een internetverbinding met dynamische IP-adressering hebt en u problemen ondervindt bij het ophalen van via de firewall, kunt u een van de volgende oplossingen proberen:

* Vraag uw Internetproviderverbindingen (ISP) voor het IP-adresbereik is toegewezen aan uw clientcomputers die toegang hebben tot de Azure Database voor MariaDB-server en vervolgens als een firewall-regel toevoegen het IP-adresbereik.

* Vraag in plaats daarvan statische IP-adressen voor uw clientcomputers en voeg de IP-adressen als firewallregels toe.

## <a name="next-steps"></a>Volgende stappen
- [Maken en beheren van Azure Database voor MariaDB-firewallregels met behulp van de Azure portal](./howto-manage-firewall-portal.md)

<!--
- [Create and manage Azure Database for MariaDB firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md) -->
