---
title: Maken en beheren van de firewallregels MySQL in Azure-Database voor MySQL | Microsoft Docs
description: Maken en beheren van Azure-Database voor firewallregels MySQL met de Azure portal
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/20/2018
ms.openlocfilehash: 3ab65ad99b3219060bb044b0e6b84edf3f1737e0
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-portal"></a>Maken en beheren van Azure-Database voor firewallregels MySQL met behulp van de Azure-portal
Firewallregels op serverniveau kunnen beheerders toegang krijgen tot een Azure-Database voor MySQL-Server uit een opgegeven IP-adres of een bereik met IP-adressen. 

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Een serverfirewallregel maken in Azure Portal

1. Op de pagina MySQL-server onder de instellingen voor kop, klikt u op **verbindingsbeveiliging** de pagina beveiliging van de verbinding voor de Azure-Database voor MySQL openen.

   ![Azure-portal - Klik op de beveiliging van de verbinding](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Klik op **Mijn IP toevoegen** op de werkbalk. Dit maakt automatisch een firewallregel met het openbare IP-adres van uw computer als waargenomen door het Azure-systeem.

   ![Azure-portal - Klik op toevoegen Mijn IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Controleer of uw IP-adres voor het opslaan van de configuratie. In sommige gevallen is verschilt het IP-adres dat door de Azure-portal waargenomen van het IP-adres gebruikt bij het openen van het internet en de Azure-servers. Daarom moet u mogelijk het eerste IP- en eind-IP-ervoor zorgen dat de regel werkt zoals verwacht wijzigen.

   Gebruik een zoekmachine of andere online hulpprogramma om te controleren van uw eigen IP-adres. Zoek bijvoorbeeld naar 'Wat is Mijn IP-adres'. 

   ![Bing voor Mijn IP](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Voeg aanvullende adresbereiken. U kunt één IP-adres of een bereik aan adressen opgeven in de firewallregels voor de Azure-Database voor MySQL. Als u beperken van de regel met een enkel IP-adres wilt, typt u hetzelfde adres in de eerste IP- en eind-IP-velden. De firewall te openen, kunnen beheerders, gebruikers en de toepassing toegang heeft tot een database op de MySQL-server waartoe ze geldige referenties hebben.

   ![Azure portal - firewall-regels ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Klik op **opslaan** op de werkbalk om op te slaan deze firewallregel op serverniveau. Wacht totdat de bevestiging dat de update voor de firewallregels geslaagd is.

   ![Azure-portal - Klik op Opslaan](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Verbinding maken vanuit Azure
Azure-verbindingen moeten worden ingeschakeld zodat toepassingen van Azure te verbinden met uw Azure-Database voor de MySQL-server. Bijvoorbeeld als host voor een Web-Apps van Azure-toepassing of een toepassing die wordt uitgevoerd in een Azure VM of verbinding te maken uit een Azure Data Factory data management gateway. De resources hoeft niet in hetzelfde virtuele netwerk (VNET) of resourcegroep voor de firewallregel inschakelen van deze verbindingen zijn. Wanneer een toepassing vanuit Azure probeert verbinding te maken met uw databaseserver, verifieert de firewall of Azure-verbindingen zijn toegestaan. Er zijn een aantal methoden aan de hand van deze typen verbindingen. Een firewallinstelling waarvan het begin- en eindadres gelijk zijn aan 0.0.0.0 geeft aan dat deze verbindingen zijn toegestaan. U kunt ook instellen de **toegang tot Azure-services toestaan** optie naar **ON** in de portal van de **verbindingsbeveiliging** deelvenster en drukt **Opslaan**. Als de verbindingspoging is niet toegestaan, wordt in de aanvraag de Azure-Database voor de MySQL-server niet bereiken.

> [!IMPORTANT]
> Met deze optie configureert u de firewall zo dat alle verbindingen vanuit Azure zijn toegestaan, inclusief verbindingen vanuit de abonnementen van andere klanten. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.
> 

## <a name="manage-existing-server-level-firewall-rules-by-using-the-azure-portal"></a>Bestaande serverniveau firewallregels beheren met behulp van de Azure-portal
Herhaal de stappen voor het beheren van de firewall-regels.
* Als u wilt toevoegen in de huidige computer, klikt u op **+ Mijn IP toevoegen**. Klik op **Opslaan** om de wijzigingen op te slaan.
* Extra IP-adressen, typt u de **REGELNAAM**, **eerste IP-**, en **END-IP**. Klik op **Opslaan** om de wijzigingen op te slaan.
* Een bestaande regel te wijzigen, klikt u op een van de velden in de regel en vervolgens wijzigen. Klik op **Opslaan** om de wijzigingen op te slaan.
* Als u wilt een bestaande regel verwijderen, klik op het weglatingsteken [...] en klik vervolgens op **verwijderen**. Klik op **Opslaan** om de wijzigingen op te slaan.


## <a name="next-steps"></a>Volgende stappen
- Op deze manier kunt script naar [maken en beheren van Azure-Database voor firewallregels MySQL met Azure CLI](howto-manage-firewall-using-cli.md).
- Voor hulp bij het verbinding maken met een Azure-Database voor de MySQL-server, Zie [verbindingsbibliotheken voor Azure-Database voor MySQL](./concepts-connection-libraries.md)
