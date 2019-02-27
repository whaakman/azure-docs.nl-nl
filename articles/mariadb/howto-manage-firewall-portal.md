---
title: Maken en beheren van MariaDB firewall-regels in Azure Database voor MariaDB
description: Maken en beheren van Azure Database voor MariaDB-firewallregels met behulp van de Azure portal
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 3fccb50e3447eabc15cbef89ab7655b3f1d1f4ac
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56876803"
---
# <a name="create-and-manage-azure-database-for-mariadb-firewall-rules-by-using-the-azure-portal"></a>Maken en beheren van Azure Database voor MariaDB-firewallregels met behulp van Azure portal
Firewallregels op serverniveau kunnen beheerders toegang tot een Azure-Database voor MariaDB-Server vanaf een opgegeven IP-adres of een bereik van IP-adressen. 

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Een serverfirewallregel maken in Azure Portal

1. Op de pagina MariaDB-server onder instellingen kop, klikt u op **verbindingsbeveiliging** op de pagina verbindingsbeveiliging openen voor de Azure Database voor MariaDB.

   ![Azure-portal: klik op de beveiliging van de verbinding](./media/howto-manage-firewall-portal/1-connection-security.png)

2. Klik op **Mijn IP toevoegen** op de werkbalk. Dit maakt automatisch een firewall-regel met het openbare IP-adres van uw computer, zoals waargenomen door de Azure-systeem.

   ![Azure-portal: klik op Mijn IP toevoegen](./media/howto-manage-firewall-portal/2-add-my-ip.png)

3. Controleer of uw IP-adres voor het opslaan van de configuratie. In sommige gevallen is het IP-adres dat door Azure-portal waargenomen wijkt af van het IP-adres dat is gebruikt bij het openen van het internet en Azure-servers. In dat geval moet u ervoor zorgen dat de regel werkt zoals verwacht het eerste IP- en eind-IP-wijzigen.

   Gebruik een zoekmachine of andere online hulpprogramma om te controleren of uw eigen IP-adres. Zoek bijvoorbeeld naar 'Wat is Mijn IP-adres'.

4. Voeg aanvullende adresbereiken. In de firewallregels voor de Azure Database voor MariaDB, kunt u één IP-adres of een reeks adressen. Als u beperken van de regel moet één IP-adres wilt, typt u hetzelfde adres in het eerste IP- en eind-IP-velden. De firewall opent, kunnen beheerders, gebruikers en toepassingen voor toegang tot elke database op de MariaDB-server waarvoor ze geldige aanmeldgegevens hebben.

   ![Azure portal - firewall-regels](./media/howto-manage-firewall-portal/4-specify-addresses.png)

5. Klik op **opslaan** op de werkbalk om op te slaan deze firewallregel op serverniveau. Wachten op de bevestiging dat de update van de firewall-regels voltooid is.

   ![Azure-portal: klik op Opslaan](./media/howto-manage-firewall-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Verbinding maken vanuit Azure
Opdat toepassingen vanuit Azure verbinding maken met uw Azure Database voor MariaDB-server, moeten de Azure-verbindingen zijn ingeschakeld. Bijvoorbeeld als host voor een Azure Web Apps-toepassing of een toepassing die wordt uitgevoerd in een Azure-VM, of verbinding te maken van een Azure Data Factory data management gateway. De resources hoeft te worden in het hetzelfde Virtueelnetwerk (VNet) of de resourcegroep voor de firewallregel om in te schakelen deze verbindingen. Wanneer een toepassing vanuit Azure probeert verbinding te maken met uw databaseserver, verifieert de firewall of Azure-verbindingen zijn toegestaan. Er zijn een aantal methoden voor het inschakelen van deze typen verbindingen. Een firewallinstelling waarvan het begin- en eindadres gelijk zijn aan 0.0.0.0 geeft aan dat deze verbindingen zijn toegestaan. U kunt ook instellen de **toegang tot Azure-services toestaan** optie naar **ON** in de portal van de **verbindingsbeveiliging** deelvenster en klik op **Opslaan**. Als de verbindingspoging niet is toegestaan, wordt in de aanvraag de Azure Database voor MariaDB-server niet bereiken.

> [!IMPORTANT]
> Met deze optie configureert u de firewall zo dat alle verbindingen vanuit Azure zijn toegestaan, inclusief verbindingen vanuit de abonnementen van andere klanten. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.
> 

## <a name="manage-existing-firewall-rules-in-the-azure-portal"></a>Bestaande firewallregels in Azure portal beheren
Herhaal de stappen voor het beheren van de firewall-regels.
* De huidige computer toevoegen, klikt u op **+ Mijn IP toevoegen**. Klik op **Opslaan** om de wijzigingen op te slaan.
* Als u wilt toevoegen van extra IP-adressen, typt u in de **REGELNAAM**, **eerste IP**, en **END-IP**. Klik op **Opslaan** om de wijzigingen op te slaan.
* Klik op een van de velden in de regel voor het wijzigen van een bestaande regel en wijzig vervolgens. Klik op **Opslaan** om de wijzigingen op te slaan.
* Als u wilt een bestaande regel verwijderen, klikt u op het weglatingsteken [...] en klik vervolgens op **verwijderen**. Klik op **Opslaan** om de wijzigingen op te slaan.

<!--
## Next steps
 - Similarly, you can script to [Create and manage Azure Database for MariaDB firewall rules using Azure CLI](howto-manage-firewall-using-cli.md).
- For help in connecting to an Azure Database for MariaDB server, see [Connection libraries for Azure Database for MariaDB](./concepts-connection-libraries.md) -->
