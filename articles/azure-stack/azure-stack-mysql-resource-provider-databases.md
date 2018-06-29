---
title: Met behulp van databases die worden geleverd door de RP MySQL-Adapter op AzureStack | Microsoft Docs
description: Het maken en beheren van MySQL-databases die zijn ingericht met behulp van de Resourceprovider voor MySQL-Adapter
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
ms.date: 06/26/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: fdadc29aa1d25e90afe088053ae4fe2139fa7f16
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37031821"
---
# <a name="create-mysql-databases"></a>MySQL-databases maken

U kunt maken en beheren van selfservice-databases in de gebruikersportal. Een Azure-Stack-gebruiker moet een abonnement met een aanbieding met de MySQL-database-service.

## <a name="test-your-deployment-by-creating-a-mysql-database"></a>Uw implementatie testen door een MySQL-database te maken

1. Aanmelden bij de gebruikersportal van Azure-Stack.
2. Selecteer **+ nieuw** > **gegevens en opslag** > **MySQL-Database** > **toevoegen**.
3. Onder **MySQL-Database maken**, voer de naam van de Database en de overige instellingen te configureren zoals vereist voor uw omgeving.

    ![Een test MySQL-database maken](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Onder **Create Database**, selecteer **SKU**. Onder **selecteert u een MySQL-SKU**, kies de SKU voor uw database.

    ![Selecteer een MySQL-SKU](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png)

    >[!Note]
    >Als u hosting-servers worden toegevoegd aan Azure-Stack, waaraan ze een SKU zijn toegewezen. Databases zijn gemaakt in de groep servers in een SKU hosten.

5. Onder **aanmelding**, selecteer ***vereiste instellingen configureren***.
6. Onder **selecteert u een aanmelding**, kunt u ervoor kiezen een bestaande aanmelding of selecteren **+ Maak een nieuwe aanmelding** voor het instellen van een nieuwe aanmelding.  Voer een **databaseaanmelding** naam en **wachtwoord**, en selecteer vervolgens **OK**.

    ![Maak een nieuwe databaseaanmelding](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    >[!NOTE]
    >De lengte van de aanmeldingsnaam voor de Database mag maximaal 32 tekens MySQL 5.7. In eerdere versies, mag deze 16 tekens niet overschrijden.

7. Selecteer **maken** voltooid instellen van de database.

Nadat de database is geïmplementeerd, dient u de **verbindingsreeks** onder **Essentials**. U kunt deze tekenreeks gebruiken in elke toepassing die u moet toegang tot de MySQL-database.

![De verbindingsreeks ophalen voor de MySQL-database](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

## <a name="update-the-administrative-password"></a>Het beheerderswachtwoord bijwerken

U kunt het wachtwoord wijzigen door deze te wijzigen op het exemplaar van de server MySQL.

1. Selecteer **SERVERVIRTUALISATIE** > **MySQL-Servers die als host fungeert**. Selecteer de hostserver.
2. Onder **instellingen**, selecteer **wachtwoord**.
3. Onder **wachtwoord**, voer het nieuwe wachtwoord en selecteer vervolgens **opslaan**.

![Het beheerderswachtwoord bijwerken](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="next-steps"></a>Volgende stappen

[Update de MySQL-resourceprovider](azure-stack-mysql-resource-provider-update.md)
