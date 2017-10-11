---
title: Maken en beheren van Azure-Database voor firewallregels PostgreSQL met de Azure portal | Microsoft Docs
description: Maken en beheren van Azure-Database voor firewallregels PostgreSQL met de Azure portal
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 20ac1392949a6f604e68d984cb50273b61051037
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-the-azure-portal"></a>Maken en beheren van Azure-Database voor firewallregels PostgreSQL met de Azure portal
Firewallregels op serverniveau kunnen beheerders toegang krijgen tot een Azure-Database voor PostgreSQL-Server uit een opgegeven IP-adres of een bereik van IP-adressen. 

## <a name="prerequisites"></a>Vereisten
Stap in deze handleiding instructies, wilt u het volgende nodig:
- Een server [PostgreSQL een Azure-Database gemaakt](quickstart-create-server-database-portal.md)

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Een serverfirewallregel maken in Azure Portal
1. Op de blade PostgreSQL server onder de instellingen voor kop, klikt u op **verbindingsbeveiliging** de verbindingsbeveiliging blade voor de Azure-Database voor PostgreSQL openen.

  ![Azure-portal - Klik op de beveiliging van de verbinding](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Klik op **Mijn IP toevoegen** op de werkbalk. Hiermee maakt u een regel automatisch met het IP-adres van uw computer, zoals waargenomen door het Azure-systeem.

  ![Azure-portal - Klik op toevoegen Mijn IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Controleer of uw IP-adres voor het opslaan van de configuratie. In sommige gevallen is verschilt het IP-adres dat door de Azure-portal waargenomen van het IP-adres gebruikt bij het openen van het internet en de Azure-servers. Daarom moet u mogelijk het eerste IP- en eind-IP-ervoor zorgen dat de regel werkt zoals verwacht wijzigen.
Gebruik een zoekmachine of andere online hulpprogramma om te controleren van uw eigen IP-adres (bijvoorbeeld Bing zoeken 'Wat is Mijn IP').

  ![Bing zoeken naar wat Mijn IP is](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Voeg aanvullende adresbereiken. U kunt één IP-adres of een bereik aan adressen opgeven in de regels voor de Azure-Database voor PostgreSQL-firewall. Als u beperken van de regel toe aan een enkel IP-adres wilt, typt u hetzelfde adres in het veld voor het eerste IP- en End IP-adres. De firewall te openen, kunnen beheerders en gebruikers om aan te melden met een database op de PostgreSQL-server waarmee ze geldige referenties bevatten.

  ![Azure portal - firewall-regels ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Klik op **opslaan** op de werkbalk om op te slaan deze firewallregel op serverniveau. Wacht totdat de bevestiging dat de update voor de firewallregels geslaagd is.

  ![Azure-portal - Klik op Opslaan](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)


## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Bestaande firewallregels op serverniveau beheren via Azure Portal
Herhaal de stappen voor het beheren van de firewall-regels.
* Als u wilt toevoegen in de huidige computer, klik op de knop + **Mijn IP toevoegen**. Klik op **Opslaan** om de wijzigingen op te slaan.
* As u extra IP-adressen wilt toevoegen, typt u de Regelnaam, het Eerste IP-adres en het Laatste IP-adres in. Klik op **Opslaan** om de wijzigingen op te slaan.
* Als u een bestaande regel wilt wijzigen, klikt u op een willekeurig veld in de regel om dit aan te passen. Klik op **Opslaan** om de wijzigingen op te slaan.
* Klik op het weglatingsteken [...] en klik op verwijderen verwijderen de regel voor het verwijderen van een bestaande regel. Klik op **Opslaan** om de wijzigingen op te slaan.

## <a name="next-steps"></a>Volgende stappen
- Op deze manier kunt script naar [maken en beheren van Azure-Database voor firewallregels PostgreSQL met Azure CLI](howto-manage-firewall-using-cli.md)
- Zie voor meer informatie in de verbinding te maken met een Azure-Database voor PostgreSQL server [verbindingsbibliotheken voor Azure-Database voor PostgreSQL](concepts-connection-libraries.md)
