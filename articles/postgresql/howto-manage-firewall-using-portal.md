---
title: Maken en beheren van firewallregels in Azure-Database voor PostgreSQL
description: Maken en beheren van Azure-Database voor firewallregels PostgreSQL met de Azure portal
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: bef927cff49d957728a2a12362786d48d60e61b7
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-the-azure-portal"></a>Maken en beheren van Azure-Database voor firewallregels PostgreSQL met de Azure portal
Firewallregels op serverniveau kunnen beheerders toegang krijgen tot een Azure-Database voor PostgreSQL-Server uit een opgegeven IP-adres of een bereik van IP-adressen. 

## <a name="prerequisites"></a>Vereisten
Stap in deze handleiding instructies, wilt u het volgende nodig:
- Een server [PostgreSQL een Azure-Database gemaakt](quickstart-create-server-database-portal.md)

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Een serverfirewallregel maken in Azure Portal
1. Op de pagina PostgreSQL-server onder de instellingen voor kop, klikt u op **verbindingsbeveiliging** de beveiligingspagina verbinding voor de Azure-Database voor PostgreSQL openen.

  ![Azure-portal - Klik op de beveiliging van de verbinding](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Klik op **Mijn IP toevoegen** op de werkbalk. Dit maakt automatisch een firewallregel met het openbare IP-adres van uw computer als waargenomen door het Azure-systeem.

  ![Azure-portal - Klik op toevoegen Mijn IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Controleer of uw IP-adres voor het opslaan van de configuratie. In sommige gevallen is verschilt het IP-adres dat door de Azure-portal waargenomen van het IP-adres gebruikt bij het openen van het internet en de Azure-servers. Daarom moet u mogelijk het eerste IP- en eind-IP-ervoor zorgen dat de regel werkt zoals verwacht wijzigen.
Gebruik een zoekmachine of andere online hulpprogramma om te controleren van uw eigen IP-adres. Bijvoorbeeld, zoek naar 'Wat is Mijn IP'.

  ![Bing zoeken naar wat Mijn IP is](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Voeg aanvullende adresbereiken. U kunt één IP-adres of een bereik aan adressen opgeven in de firewallregels voor de Azure-Database voor PostgreSQL. Als u beperken van de regel met een enkel IP-adres wilt, typt u hetzelfde adres in het veld voor het eerste IP- en End IP-adres. De firewall te openen, kunnen beheerders, gebruikers en toepassingen aan te melden met een database op de PostgreSQL-server waarmee ze geldige referenties bevatten.

  ![Azure portal - firewall-regels ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Klik op **opslaan** op de werkbalk om op te slaan deze firewallregel op serverniveau. Wacht totdat de bevestiging dat de update voor de firewallregels geslaagd is.

  ![Azure-portal - Klik op Opslaan](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Verbinding maken vanuit Azure
Zodat toepassingen van Azure te verbinden met uw Azure-Database voor PostgreSQL-server moeten de Azure-verbindingen zijn ingeschakeld. Bijvoorbeeld als host voor een Web-Apps van Azure-toepassing of een toepassing die wordt uitgevoerd in een Azure VM of verbinding te maken uit een Azure Data Factory data management gateway. De resources hoeft niet te worden in het hetzelfde virtuele netwerk (VNet) of de resourcegroep voor de firewallregel inschakelen van deze verbindingen. Wanneer een toepassing vanuit Azure probeert verbinding te maken met uw databaseserver, verifieert de firewall of Azure-verbindingen zijn toegestaan. Er zijn een aantal methoden aan de hand van deze typen verbindingen. Een firewallinstelling waarvan het begin- en eindadres gelijk zijn aan 0.0.0.0 geeft aan dat deze verbindingen zijn toegestaan. U kunt ook instellen de **toegang tot Azure-services toestaan** optie naar **ON** in de portal van de **verbindingsbeveiliging** deelvenster en drukt **Opslaan**. Als de verbindingspoging is niet toegestaan, wordt in de aanvraag de Azure-Database voor PostgreSQL-server niet bereiken.

> [!IMPORTANT]
> Met deze optie configureert u de firewall zo dat alle verbindingen vanuit Azure zijn toegestaan, inclusief verbindingen vanuit de abonnementen van andere klanten. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.
> 

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Bestaande firewallregels op serverniveau beheren via Azure Portal
Herhaal de stappen voor het beheren van de firewall-regels.
* Als u wilt toevoegen in de huidige computer, klik op de knop + **Mijn IP toevoegen**. Klik op **Opslaan** om de wijzigingen op te slaan.
* As u extra IP-adressen wilt toevoegen, typt u de Regelnaam, het Eerste IP-adres en het Laatste IP-adres in. Klik op **Opslaan** om de wijzigingen op te slaan.
* Als u een bestaande regel wilt wijzigen, klikt u op een willekeurig veld in de regel om dit aan te passen. Klik op **Opslaan** om de wijzigingen op te slaan.
* Verwijder een bestaande regel, klik op het weglatingsteken [...] te klikken en **verwijderen** om de regel te verwijderen. Klik op **Opslaan** om de wijzigingen op te slaan.

## <a name="next-steps"></a>Volgende stappen
- Op deze manier kunt script naar [maken en beheren van Azure-Database voor firewallregels PostgreSQL met Azure CLI](howto-manage-firewall-using-cli.md).
- Zie voor meer informatie in de verbinding te maken met een Azure-Database voor PostgreSQL server [verbindingsbibliotheken voor Azure-Database voor PostgreSQL](concepts-connection-libraries.md).
