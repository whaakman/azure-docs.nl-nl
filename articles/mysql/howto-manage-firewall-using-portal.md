---
title: Maken en beheren van de firewallregels voor MySQL in Azure Database voor MySQL
description: Maken en beheren van Azure Database voor MySQL-firewallregels met behulp van de Azure portal
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: b55f8d44d12fa503a17218db4a5166819024f1aa
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56872893"
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-portal"></a>Maken en beheren van Azure Database voor MySQL-firewallregels met behulp van Azure portal
Firewallregels op serverniveau kunnen beheerders toegang tot een Azure-Database voor MySQL-Server vanaf een opgegeven IP-adres of een bereik van IP-adressen. 

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Een serverfirewallregel maken in Azure Portal

1. Op de pagina van de MySQL-server onder instellingen kop, klikt u op **verbindingsbeveiliging** op de pagina verbindingsbeveiliging openen voor de Azure Database voor MySQL.

   ![Azure-portal: klik op de beveiliging van de verbinding](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Klik op **Mijn IP toevoegen** op de werkbalk. Dit maakt automatisch een firewall-regel met het openbare IP-adres van uw computer, zoals waargenomen door de Azure-systeem.

   ![Azure-portal: klik op Mijn IP toevoegen](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Controleer of uw IP-adres voor het opslaan van de configuratie. In sommige gevallen is het IP-adres dat door Azure-portal waargenomen wijkt af van het IP-adres dat is gebruikt bij het openen van het internet en Azure-servers. In dat geval moet u ervoor zorgen dat de regel werkt zoals verwacht het eerste IP- en eind-IP-wijzigen.

   Gebruik een zoekmachine of andere online hulpprogramma om te controleren of uw eigen IP-adres. Zoek bijvoorbeeld naar 'Wat is Mijn IP-adres'. 

   ![Bing voor wat Mijn IP is](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Voeg aanvullende adresbereiken. In de firewallregels voor de Azure Database for MySQL, kunt u één IP-adres of een reeks adressen. Als u beperken van de regel moet één IP-adres wilt, typt u hetzelfde adres in het eerste IP- en eind-IP-velden. De firewall opent, kunnen beheerders, gebruikers en toepassingen voor toegang tot elke database op de MySQL-server waarvoor ze geldige aanmeldgegevens hebben.

   ![Azure portal - firewall-regels](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Klik op **opslaan** op de werkbalk om op te slaan deze firewallregel op serverniveau. Wachten op de bevestiging dat de update van de firewall-regels voltooid is.

   ![Azure-portal: klik op Opslaan](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Verbinding maken vanuit Azure
Als u wilt toestaan dat toepassingen vanuit Azure verbinding kunnen maken met uw Azure Database voor MySQL-server, moeten de Azure-verbindingen zijn ingeschakeld. Bijvoorbeeld als host voor een Azure Web Apps-toepassing of een toepassing die wordt uitgevoerd in een Azure-VM, of verbinding te maken van een Azure Data Factory data management gateway. De resources hoeft te worden in het hetzelfde Virtueelnetwerk (VNet) of de resourcegroep voor de firewallregel om in te schakelen deze verbindingen. Wanneer een toepassing vanuit Azure probeert verbinding te maken met uw databaseserver, verifieert de firewall of Azure-verbindingen zijn toegestaan. Er zijn een aantal methoden voor het inschakelen van deze typen verbindingen. Een firewallinstelling waarvan het begin- en eindadres gelijk zijn aan 0.0.0.0 geeft aan dat deze verbindingen zijn toegestaan. U kunt ook instellen de **toegang tot Azure-services toestaan** optie naar **ON** in de portal van de **verbindingsbeveiliging** deelvenster en druk op **Opslaan**. Als de verbindingspoging niet is toegestaan, wordt in de aanvraag de Azure Database for MySQL-server niet bereiken.

> [!IMPORTANT]
> Met deze optie configureert u de firewall zo dat alle verbindingen vanuit Azure zijn toegestaan, inclusief verbindingen vanuit de abonnementen van andere klanten. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.
> 

## <a name="manage-existing-server-level-firewall-rules-by-using-the-azure-portal"></a>Bestaande firewallregels op serverniveau beheren met behulp van de Azure-portal
Herhaal de stappen voor het beheren van de firewall-regels.
* De huidige computer toevoegen, klikt u op **+ Mijn IP toevoegen**. Klik op **Opslaan** om de wijzigingen op te slaan.
* Als u wilt toevoegen van extra IP-adressen, typt u in de **REGELNAAM**, **eerste IP**, en **END-IP**. Klik op **Opslaan** om de wijzigingen op te slaan.
* Klik op een van de velden in de regel voor het wijzigen van een bestaande regel en wijzig vervolgens. Klik op **Opslaan** om de wijzigingen op te slaan.
* Als u wilt een bestaande regel verwijderen, klikt u op het weglatingsteken [...] en klik vervolgens op **verwijderen**. Klik op **Opslaan** om de wijzigingen op te slaan.


## <a name="next-steps"></a>Volgende stappen
- Op dezelfde manier, u kunt een script op [maken en beheren van Azure Database voor MySQL-firewallregels met behulp van Azure CLI](howto-manage-firewall-using-cli.md).
- Zie voor hulp bij het verbinding maken met een Azure Database for MySQL-server, [verbindingsbibliotheken voor Azure Database for MySQL](./concepts-connection-libraries.md)
