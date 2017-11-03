---
title: Maken en beheren van Azure-Database voor de MySQL-server met Azure portal | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt snel een nieuwe Azure-Database voor de MySQL-server maken en beheren van de server met de Azure-Portal.
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 6e9c541aac1241b6af0e4a58f5591d46f9a98c40
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Maken en beheren van Azure-Database voor de MySQL-server met Azure portal
Dit onderwerp wordt beschreven hoe u snel een nieuwe Azure-Database voor de MySQL-server kunt maken. Het bevat ook informatie over het beheren van de server met behulp van de Azure-portal. Serverbeheer omvat server-gegevens en het wachtwoord opnieuw instellen en het verwijderen van de server-databases weer te geven.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken
Volg deze stappen voor het maken van een Azure-Database voor de MySQL-server met de naam 'mysqlserver4demo'.

1. Klik op de **nieuw** knop zich in de linkerbovenhoek van de Azure portal.

2. Selecteer op de pagina nieuwe **Databases**, en selecteer vervolgens op de pagina Databases **Azure Database voor MySQL**.

    > Een Azure-Database voor de MySQL-server is gemaakt met een gedefinieerde set [berekenings- en](./concepts-compute-unit-and-storage.md) resources. De database is gemaakt vanuit een Azure-resourcegroep en in een Azure-Database voor de MySQL-server.

   ![Maak nieuwe-server](./media/howto-create-manage-server-portal/create-new-server.png)

3. De Azure-Database voor MySQL formulier invullen met behulp van de volgende informatie:

    | **Formulierveld** | **Beschrijving van veld** |
    |----------------|-----------------------|
    | *Servernaam* | Azure-mysql (servernaam is globally unique identifier) |
    | *Abonnement* | MySQLaaS (Selecteer in de vervolgkeuzelijst) |
    | *Resourcegroep* | MyResource (een nieuwe resourcegroep maken of een bestaande gebruiken) |
    | *Aanmeldgegevens van serverbeheerder* | myadmin (stel accountnaam van beheerder in) |
    | *Wachtwoord* | beheerderswachtwoord voor account Setup |
    | *Wachtwoord bevestigen* | bevestig wachtwoord voor beheerdersaccount |
    | *Locatie* | Noord-Europa (Selecteer tussen Noord-Europa en VS-West) |
    | *Versie* | 5.6 (Kies Azure Database voor MySQL server-versie) |

4. Klik op **prijscategorie** om op te geven van het serviceniveau voor prijscategorie en prestatieniveau voor uw nieuwe server. COMPUTE-eenheid kan worden geconfigureerd tussen 50 en 100 in basisstaffel tussen 100 en 200 in Standard-laag en opslag op basis van het opgenomen bedrag kan worden toegevoegd. Voor deze handleiding procedure kiezen we 50 Compute-eenheid en 50 GB. Klik op **OK** opslaan van uw selectie.

   ![Maak-server--prijscategorie](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5. Klik op **Maken** om de server in te richten. De inrichting duurt een paar minuten.

    > Selecteer de **vastmaken aan dashboard** optie voor het toestaan van eenvoudige bijhouden van uw implementaties.
    > [!NOTE]
    > Hoewel in de basisstaffel maximaal 1000 GB en 10.000 GB in Standard-laag wordt ondersteund voor opslag, voor de openbare Preview is maximale opslagcapaciteit tijdelijk beperkt tot 1000 GB.</Include>

## <a name="update-an-azure-database-for-mysql-server"></a>Een Azure-Database voor de MySQL-server bijwerken
Nadat de nieuwe server is ingericht, de gebruiker heeft twee opties voor het bewerken van een bestaande server: administratorwachtwoord opnieuw instellen of de server omhoog of omlaag schalen door het wijzigen van de compute-eenheden.

### <a name="change-the-administrator-user-password"></a>Het beheerderswachtwoord voor de gebruiker te wijzigen
1. Op de server **overzicht** blade, klikt u op **wachtwoord opnieuw instellen** voor het vullen van de invoer en de bevestiging venster van een wachtwoord.

2. Nieuw wachtwoord invoeren en Bevestig het wachtwoord in het venster zoals wordt weergegeven:

   ![Wachtwoord opnieuw instellen](./media/howto-create-manage-server-portal/reset-password.png)

3. Klik op **OK** om op te slaan van het nieuwe wachtwoord.

### <a name="scale-updown-by-changing-compute-units"></a>Omhoog/omlaag schalen door het wijzigen van de Compute-eenheden

1. Op de serverblade onder **instellingen**, klikt u op **prijscategorie** de prijzen laag om blade te openen voor de Azure-Database voor de MySQL-server.

2. Ga als volgt stap 4 in **maken van een Azure-Database voor MySQL server** Compute eenheden in de dezelfde prijscategorie te wijzigen.

## <a name="delete-an-azure-database-for-mysql-server"></a>Een Azure-Database voor de MySQL-server verwijderen

1. Op de server **overzicht** blade, klikt u op de **verwijderen** opdrachtknop om de blade van de bevestiging verwijderen te openen.

2. Typ de juiste servernaam in het invoervak van de blade voor dubbele bevestiging.

3. Klik op de **verwijderen** knop Bevestig de actie verwijderen en vervolgens wacht u voor het succes' verwijderen' worden weergegeven op de meldingsbalk weergegeven.

## <a name="list-the-azure-database-for-mysql-databases"></a>Lijst van de Azure-Database voor de MySQL-databases
Op de server **overzicht** blade Schuif omlaag totdat u de database onder tegel ziet. Alle databases worden vermeld in de tabel. Klik op de **verwijderen** opdrachtknop om de blade van de bevestiging verwijderen te openen.

   ![weergeven-databases](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>Details weergeven van een Azure-Database voor de MySQL-server
Op de serverblade onder **instellingen**, klikt u op **eigenschappen** openen de **eigenschappen** blade en klik vervolgens weergave alle gedetailleerde informatie over de server.

## <a name="next-steps"></a>Volgende stappen

[Snelstartgids: Azure-Database maken voor de MySQL-server met Azure portal](./quickstart-create-mysql-server-database-using-azure-portal.md)