---
title: Maken en beheren van Azure-Database voor de MySQL-server met Azure portal
description: Dit artikel wordt beschreven hoe u kunt snel een nieuwe Azure-Database voor de MySQL-server maken en beheren van de server met de Azure-Portal.
services: mysql
author: ajlam
ms.author: nolanwu
editor: jasonwhowell
manager: kfile
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 0e274c0ada3de5e9000ae41516e5b9b67ef1490b
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Maken en beheren van Azure-Database voor de MySQL-server met Azure portal
Dit onderwerp wordt beschreven hoe u snel een nieuwe Azure-Database voor de MySQL-server kunt maken. Het bevat ook informatie over het beheren van de server met behulp van de Azure-portal. Serverbeheer omvat server-gegevens en het wachtwoord opnieuw instellen, resources schalen en verwijderen van de server-databases weer te geven.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken
Volg deze stappen voor het maken van een Azure-Database voor de MySQL-server met de naam 'mydemoserver'.

1. Klik op de **maken van een resource** knop zich in de linkerbovenhoek van de Azure portal.

2. Selecteer op de pagina nieuwe **Databases**, en selecteer vervolgens op de pagina Databases **Azure Database voor MySQL**.

    > Een Azure-Database voor de MySQL-server is gemaakt met een gedefinieerde set [berekenings- en](./concepts-pricing-tiers.md) resources. De database is gemaakt vanuit een Azure-resourcegroep en in een Azure-Database voor de MySQL-server.

   ![Maak nieuwe-server](./media/howto-create-manage-server-portal/create-new-server.png)

3. De Azure-Database voor MySQL formulier invullen met behulp van de volgende informatie:

    | **Formulierveld** | **Beschrijving van veld** |
    |----------------|-----------------------|
    | *Servernaam* | mydemoserver (servernaam is globally unique identifier) |
    | *Abonnement* | mysubscription (Selecteer in de vervolgkeuzelijst) |
    | *Resourcegroep* | myresourcegroup (een nieuwe resourcegroep maken of een bestaande gebruiken) |
    | *Bron selecteren* | Lege (een lege MySQL-server maken) |
    | *Aanmeldgegevens van serverbeheerder* | myadmin (stel accountnaam van beheerder in) |
    | *Wachtwoord* | de account beheerderswachtwoord instellen |
    | *Wachtwoord bevestigen* | bevestig wachtwoord voor beheerdersaccount |
    | *Locatie* | Zuidoost-Azië (Selecteer tussen Noord-Europa en VS-West) |
    | *Versie* | 5.7 (Kies Azure Database voor MySQL server-versie) |

4. Klik op **prijscategorie** om op te geven van het serviceniveau voor prijscategorie en prestatieniveau voor uw nieuwe server. Selecteer de **algemeen** tabblad. *Gen 4*, *2 vCores*, *5 GB* en *7 dagen* zijn de standaardwaarden voor **Bewerking voor compute**, **vCore**, **Opslag** en **Bewaarperiode voor back-up**. U kunt deze schuifregelaars laten zoals ze zijn. Als u serverback-ups in geografisch redundante opslag wilt inschakelen, selecteert u **Geografisch redundant** in het gedeelte **Redundantieopties voor back-up**.

   ![Maak-server--prijscategorie](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5. Klik op **Maken** om de server in te richten. De inrichting duurt een paar minuten.

    > Selecteer de **vastmaken aan dashboard** optie voor het toestaan van eenvoudige bijhouden van uw implementaties.

## <a name="update-an-azure-database-for-mysql-server"></a>Een Azure-Database voor de MySQL-server bijwerken
Nadat de nieuwe server is ingericht, beschikt de gebruiker over verschillende opties voor het configureren van de bestaande server, inclusief het administratorwachtwoord opnieuw instellen en schalen van de server omhoog of omlaag door vCore of opslag te wijzigen.

### <a name="change-the-administrator-user-password"></a>Het beheerderswachtwoord voor de gebruiker te wijzigen
1. Van de server **overzicht**, klikt u op **wachtwoord opnieuw instellen** om het wachtwoord opnieuw instellen van venster weer te geven.

   ![overview](./media/howto-create-manage-server-portal/overview.png)

2. Een nieuw wachtwoord invoeren en Bevestig het wachtwoord in het venster zoals wordt weergegeven:

   ![reset-password](./media/howto-create-manage-server-portal/reset-password.png)

3. Klik op **OK** om op te slaan van het nieuwe wachtwoord.

### <a name="scale-vcores-updown"></a>VCores schaal omhoog/omlaag

1. Klik op **prijscategorie**, dat zich bevindt onder **instellingen**.

2. Wijzig de **vCore** instellen met de schuifregelaar op de gewenste waarde.

    ![scale-compute](./media/howto-create-manage-server-portal/scale-compute.png)

3. Klik op **OK** om de wijzigingen op te slaan.

### <a name="scale-storage-up"></a>Opslag van schaal

1. Klik op **prijscategorie**, dat zich bevindt onder **instellingen**.

2. Wijzig de **opslag** instellen met de schuifregelaar op de gewenste waarde.

    ![Scale-opslag](./media/howto-create-manage-server-portal/scale-storage.png)

3. Klik op **OK** om de wijzigingen op te slaan.

## <a name="delete-an-azure-database-for-mysql-server"></a>Een Azure-Database voor de MySQL-server verwijderen

1. Van de server **overzicht**, klikt u op de **verwijderen** te openen verwijderen bevestigingsprompt weer te geven.

    ![verwijderen](./media/howto-create-manage-server-portal/delete.png)

2. Typ de naam van de server in het invoervak voor dubbele bevestiging.

    ![confirm-delete](./media/howto-create-manage-server-portal/confirm.png)

3. Klik op de **verwijderen** knop om te bevestigen verwijderen van de server. Wachten op de pop 'is verwijderd MySQL server' tot in de meldingsbalk weergegeven.

## <a name="list-the-azure-database-for-mysql-databases"></a>Lijst van de Azure-Database voor de MySQL-databases
Van de server **overzicht**, schuif omlaag totdat u de database onder tegel ziet. Alle databases in de server worden weergegeven in de tabel.

   ![weergeven-databases](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>Details weergeven van een Azure-Database voor de MySQL-server
Klik op **eigenschappen**, dat zich bevindt onder **instellingen** om gedetailleerde informatie over de server weer te geven.

![properties](./media/howto-create-manage-server-portal/properties.png)

## <a name="next-steps"></a>Volgende stappen

[Snelstartgids: Azure-Database maken voor de MySQL-server met Azure portal](./quickstart-create-mysql-server-database-using-azure-portal.md)