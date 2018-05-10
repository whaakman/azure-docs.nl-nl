---
title: De firewall configureren voor Azure Blockchain Workbench SQL DB
description: Informatie over het configureren van de Azure Blockchain Workbench SQL DB-firewall.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/4/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: dc22f212c014ab1d6622eff3491d669b21ca6f47
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="configure-the-azure-blockchain-workbench-database-firewall"></a>De database Azure Blockchain Workbench firewall configureren

In dit artikel laat zien hoe u een firewallregel met de Azure portal configureren. Firewall-regels kunnen externe clients of toepassingen verbinding maken met uw Azure Blockchain Workbench-database.

## <a name="connect-to-the-blockchain-workbench-database"></a>Verbinding maken met de database Blockchain Workbench

Verbinding maken met de database waar u een regel configureren:

1. Meld u aan bij de Azure-Portal met een account met **eigenaar** machtigingen voor de bronnen Azure Blockchain Workbench.
2. Kies in het navigatiedeelvenster links **resourcegroepen**.
3. Kies de naam van de resourcegroep voor uw implementatie-Blockchain Workbench.
4. Selecteer **Type** sorteer de lijst van resources en klik vervolgens op uw **SQL server**.
5. Het voorbeeld van een resource in de volgende schermafbeelding ziet u twee databases: *master* en *lsgn sdk*. Configureren van de firewallregel op *lsgn sdk*.

![Lijst Blockchain Workbench resources](media/blockchain-workbench-database-firewall/list-database-resources.png)

## <a name="create-a-database-firewall-rule"></a>Een firewallregel voor database maken

Een firewallregel maken:

1. Kies de koppeling naar de database 'lsgn sdk'.
2. Selecteer op de menubalk **serverfirewall ingesteld**.

   ![Serverfirewall instellen](media/blockchain-workbench-database-firewall/configure-server-firewall.png)

3. Maken van een regel voor uw organisatie:

   * Voer een **REGELNAAM**
   * Geef een IP-adres voor de **eerste IP-** van het adresbereik
   * Geef een IP-adres voor de **END-IP** van het adresbereik

   ![Firewallregel maken](media/blockchain-workbench-database-firewall/create-firewall-rule.png)

    > [!NOTE]
    > Als u alleen het IP-adres van uw computer toevoegen wilt, kiest u **+ toevoegen van client-IP-**.
        
1. Voor het opslaan van uw firewallconfiguratie, selecteer **opslaan**.
2. Het IP-adresbereik dat u hebt geconfigureerd voor de database door verbinding te maken van een toepassing of het hulpprogramma testen. Bijvoorbeeld: SQL Server Management Studio.

## <a name="next-steps"></a>Volgende stappen

* [Databaseweergaven in Azure Blockchain Workbench](blockchain-workbench-database-views.md)