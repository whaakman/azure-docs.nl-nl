---
title: Met behulp van databases die worden geleverd door de MySQL-Adapter Relying Party op AzureStack | Microsoft Docs
description: Over het maken en beheren van de MySQL-databases die zijn ingericht met behulp van de Resourceprovider van MySQL-Adapter
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 10/16/2018
ms.openlocfilehash: 6eaba728b794c0102ec4e28791b218efa28b51b5
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56160759"
---
# <a name="create-mysql-databases"></a>MySQL-databases maken
Een Azure Stack-gebruiker, geabonneerd op een aanbieding met de MySQL-database-service kunt maken en beheren van selfservice MySQL-databases in de gebruikersportal.

## <a name="create-a-mysql-database"></a>Een MySQL-database maken

1. Meld u aan bij de gebruikersportal van Azure Stack.
2. Selecteer **+ een resource maken** > **gegevens en opslag** > **MySQL-Database** > **toevoegen**.
3. Onder **MySQL-Database maken**, voer de naam van de Database en configureer de andere instellingen zoals vereist voor uw omgeving.

    ![Een test-MySQL-database maken](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Onder **Create Database**, selecteer **SKU**. Onder **selecteert u een MySQL-SKU**, kies de SKU voor uw database.

    ![Selecteer een MySQL-SKU](./media/azure-stack-mysql-rp-deploy/mysql-select-sku.png)

    >[!Note]
    >Als hosting-servers worden toegevoegd aan Azure Stack, krijgt deze een SKU. Databases worden gemaakt in de groep van het host-servers in een SKU.

5. Onder **aanmelding**, selecteer ***vereiste instellingen configureren***.
6. Onder **selecteert u een aanmelding**, u kunt een bestaande aanmelding kiezen of selecteer **en maak een nieuwe aanmelding** voor het instellen van een nieuwe aanmelding.  Voer een **databaseaanmelding** naam en **wachtwoord**, en selecteer vervolgens **OK**.

    ![Maak een nieuwe databaseaanmelding](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    >[!NOTE]
    >De lengte van de naam van de Database kan niet groter zijn dan 32 tekens in MySQL 5.7. Het kan niet meer dan 16 tekens in eerdere versies.

7. Selecteer **maken** instellen van de database voltooid.

Nadat de database is geïmplementeerd, noteer de **Connection String** onder **Essentials**. U kunt deze tekenreeks gebruiken in alle toepassingen die toegang nodig heeft tot de MySQL-database.

![De verbindingsreeks ophalen voor de MySQL-database](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

## <a name="update-the-administrative-password"></a>Het beheerprogramma wachtwoord bijwerken

U kunt het wachtwoord wijzigen door deze te wijzigen in de MySQL-server-exemplaar.

1. Selecteer **BEHEERDERSRESOURCES** > **MySQL-Servers die als host fungeert**. Selecteer de host-server.
2. Onder **instellingen**, selecteer **wachtwoord**.
3. Onder **wachtwoord**, voer het nieuwe wachtwoord en selecteer vervolgens **opslaan**.

![Het beheerderswachtwoord bijwerken](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="next-steps"></a>Volgende stappen

[De MySQL-resourceprovider bijwerken](azure-stack-mysql-resource-provider-update.md)
