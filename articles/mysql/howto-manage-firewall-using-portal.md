---
title: Maken en beheren van Azure-Database voor firewallregels MySQL met de Azure portal | Microsoft Docs
description: Maken en beheren van Azure-Database voor firewallregels MySQL met de Azure portal
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 0604b29fcd9849545886a783ae5bbb2cbb72f2ce
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-portal"></a>Maken en beheren van Azure-Database voor firewallregels MySQL met behulp van de Azure-portal
Firewallregels op serverniveau kunnen beheerders toegang krijgen tot een Azure-Database voor MySQL-Server uit een opgegeven IP-adres of een bereik met IP-adressen. 

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Een serverfirewallregel maken in Azure Portal

1. Op de blade MySQL-server onder de instellingen voor kop, klikt u op **verbindingsbeveiliging** de verbindingsbeveiliging blade voor de Azure-Database voor MySQL openen.

   ![Azure-portal - Klik op de beveiliging van de verbinding](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Klik op **Mijn IP toevoegen** op de werkbalk om een regel maken met het IP-adres van uw computer, zoals waargenomen door het Azure-systeem.

   ![Azure-portal - Klik op toevoegen Mijn IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Controleer of uw IP-adres voor het opslaan van de configuratie. In sommige gevallen is verschilt het IP-adres dat door de Azure-portal waargenomen van het IP-adres gebruikt bij het openen van het internet en de Azure-servers. Daarom moet u mogelijk het eerste IP- en eind-IP-ervoor zorgen dat de regel werkt zoals verwacht wijzigen.

   Gebruik een zoekmachine of andere online hulpprogramma om te controleren van uw eigen IP-adres (Zoek bijvoorbeeld naar 'Wat is Mijn IP-adres').

   ![Bing voor Mijn IP](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Voeg aanvullende adresbereiken. U kunt één IP-adres of een bereik aan adressen opgeven in de regels voor de Azure-Database voor de MySQL-firewall. Als u beperken van de regel met een enkel IP-adres wilt, typt u hetzelfde adres in de eerste IP- en eind-IP-velden. De firewall te openen, kunnen beheerders en gebruikers toegang krijgen tot een database op de MySQL-server waartoe ze geldige referenties hebben.

   ![Azure portal - firewall-regels ](./media/howto-manage-firewall-using-portal/5-specify-addresses.png)


5. Klik op **opslaan** op de werkbalk om op te slaan deze firewallregel op serverniveau. Wacht totdat de bevestiging dat de update voor de firewallregels geslaagd is.

   ![Azure-portal - Klik op Opslaan](./media/howto-manage-firewall-using-portal/4-save-firewall-rule.png)

## <a name="manage-existing-server-level-firewall-rules-by-using-the-azure-portal"></a>Bestaande serverniveau firewallregels beheren met behulp van de Azure-portal
Herhaal de stappen voor het beheren van de firewall-regels.
* Als u wilt toevoegen in de huidige computer, klikt u op **+ Mijn IP toevoegen**.
* Extra IP-adressen, typt u de **REGELNAAM**, **eerste IP-**, en **END-IP**.
* Een bestaande regel te wijzigen, klikt u op een van de velden in de regel en vervolgens wijzigen.
* Als u wilt een bestaande regel verwijderen, klik op het weglatingsteken [...] en klik vervolgens op **verwijderen**.
* Klik op **Opslaan** om de wijzigingen op te slaan.

## <a name="next-steps"></a>Volgende stappen
- Voor hulp bij het verbinding maken met een Azure-Database voor de MySQL-server, Zie [verbindingsbibliotheken voor Azure-Database voor MySQL](./concepts-connection-libraries.md)
