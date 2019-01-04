---
title: Maken en beheren van Azure Database voor MySQL-server met behulp van Azure portal
description: Dit artikel wordt beschreven hoe u snel een nieuwe Azure-Database voor MySQL-server maken en beheren van de server met behulp van de Azure-Portal.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 39ffe1b71fb002658110c63c825e0515f38e2b9e
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53537321"
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Maken en beheren van Azure Database voor MySQL-server met behulp van Azure portal
In dit onderwerp wordt beschreven hoe u snel een nieuwe Azure-Database voor MySQL-server kunt maken. Het bevat ook informatie over het beheren van de server met behulp van de Azure-portal. Serverbeheer bevat Serverdetails van de en databases, het wachtwoord opnieuw instellen, het schalen van resources en het verwijderen van de server weergeven.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken
Volg deze stappen voor het maken van een Azure Database for MySQL-server met de naam "mydemoserver."

1. Klik op de **een resource maken** knop zich in de linkerbovenhoek van Azure portal.

2. Selecteer op de pagina nieuwe **Databases**, en selecteer vervolgens op de pagina Databases, **Azure Database for MySQL**.

    > Een Azure Database for MySQL-server wordt gemaakt met een gedefinieerde set [reken- en](./concepts-pricing-tiers.md) resources. De database wordt gemaakt binnen een Azure-resourcegroep en in een Azure Database for MySQL-server.

   ![maken-nieuwe-server](./media/howto-create-manage-server-portal/create-new-server.png)

3. Vul de Azure Database for MySQL-formulier in met behulp van de volgende informatie:

    | **Formulierveld** | **Beschrijving van veld** |
    |----------------|-----------------------|
    | *Servernaam* | mydemoserver (servernaam is globaal uniek) |
    | *Abonnement* | mysubscription (Selecteer in de vervolgkeuzelijst) |
    | *Resourcegroep* | myresourcegroup (Maak een nieuwe resourcegroep of gebruik een bestaande resourcegroep) |
    | *Bron selecteren* | Leeg (Maak een lege MySQL-server) |
    | *Aanmeldgegevens van serverbeheerder* | myadmin (stel accountnaam van beheerder in) |
    | *Wachtwoord* | wachtwoord voor beheerdersaccount instellen |
    | *Wachtwoord bevestigen* | bevestig wachtwoord voor beheerdersaccount |
    | *Locatie* | Zuidoost-Azië (selecteren tussen Noord-Europa en VS-West) |
    | *Versie* | 5.7 (Kies Azure Database voor MySQL-server-versie) |

4. Klik op **prijscategorie** om op te geven van het serviceniveau voor servicelaag en het prestatieniveau voor uw nieuwe server. Selecteer de **algemeen** tabblad. *Gen 4*, *2 vCores*, *5 GB* en *7 dagen* zijn de standaardwaarden voor **Bewerking voor compute**, **vCore**, **Opslag** en **Bewaarperiode voor back-up**. U kunt deze schuifregelaars laten zoals ze zijn. Als u serverback-ups in geografisch redundante opslag wilt inschakelen, selecteert u **Geografisch redundant** in het gedeelte **Redundantieopties voor back-up**.

   ![maken-server-prijscategorie](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5. Klik op **Maken** om de server in te richten. De inrichting duurt een paar minuten.

    > Selecteer de **vastmaken aan dashboard** optie voor het eenvoudig bijhouden van uw implementaties.

## <a name="update-an-azure-database-for-mysql-server"></a>Een Azure Database for MySQL-server bijwerken
Nadat de nieuwe server is ingericht, beschikt de gebruiker over verschillende opties voor het configureren van de bestaande server, met inbegrip van het administratorwachtwoord opnieuw instellen en de server omhoog of omlaag schalen door vCore of opslag te wijzigen.

### <a name="change-the-administrator-user-password"></a>Het beheerderswachtwoord van de gebruiker wijzigen
1. Van de server **overzicht**, klikt u op **wachtwoord opnieuw instellen** om het wachtwoord opnieuw instellen van venster weer te geven.

   ![overview](./media/howto-create-manage-server-portal/overview.png)

2. Voer een nieuw wachtwoord in en Bevestig het wachtwoord in het venster zoals wordt weergegeven:

   ![wachtwoord opnieuw instellen](./media/howto-create-manage-server-portal/reset-password.png)

3. Klik op **OK** om op te slaan van het nieuwe wachtwoord.

### <a name="scale-vcores-updown"></a>Schaal vCores omhoog/omlaag

1. Klik op **prijscategorie**, dat zich bevindt onder **instellingen**.

2. Wijzig de **vCore** instellen met de schuifregelaar verplaatsen naar de gewenste waarde.

    ![schaal compute](./media/howto-create-manage-server-portal/scale-compute.png)

3. Klik op **OK** om de wijzigingen op te slaan.

### <a name="scale-storage-up"></a>Schaal opslag omhoog

1. Klik op **prijscategorie**, dat zich bevindt onder **instellingen**.

2. Wijzig de **opslag** instellen met de schuifregelaar verplaatsen naar de gewenste waarde.

    ![schaalbare cloudopslag](./media/howto-create-manage-server-portal/scale-storage.png)

3. Klik op **OK** om de wijzigingen op te slaan.

## <a name="delete-an-azure-database-for-mysql-server"></a>Een Azure Database for MySQL-server verwijderen

1. Van de server **overzicht**, klikt u op de **verwijderen** knop voor het openen van de bevestigingsprompt verwijderen.

    ![delete](./media/howto-create-manage-server-portal/delete.png)

2. Typ de naam van de server in het invoervak om dubbele bevestiging.

    ![verwijderen bevestigen](./media/howto-create-manage-server-portal/confirm.png)

3. Klik op de **verwijderen** knop om te bevestigen van verwijderen van de server. Wachten op de pop 'MySQL-server is verwijderd' tot in de meldingsbalk weergegeven.

## <a name="list-the-azure-database-for-mysql-databases"></a>Lijst van de Azure Database for MySQL-databases
Van de server **overzicht**, schuif omlaag totdat u de database aan de onderkant tegel ziet. Alle databases in de server worden weergegeven in de tabel.

   ![show-databases](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>Details van een Azure Database for MySQL-server weergeven
Klik op **eigenschappen**, dat zich bevindt onder **instellingen** om gedetailleerde informatie over de server weer te geven.

![properties](./media/howto-create-manage-server-portal/properties.png)

## <a name="next-steps"></a>Volgende stappen

[Snelstart: Azure Database for MySQL-server met behulp van Azure portal maken](./quickstart-create-mysql-server-database-using-azure-portal.md)