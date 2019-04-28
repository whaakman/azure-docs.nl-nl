---
title: Een Azure Data Catalog maken
description: Een snelstartgids voor het maken van een Azure Data Catalog.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: quickstart
ms.date: 04/05/2019
ms.openlocfilehash: f00e9eaf56f3973b357792a8d1923a4b5998e0a2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61003268"
---
# <a name="quickstart-create-an-azure-data-catalog"></a>Quickstart: Een Azure Data Catalog maken

Azure Data Catalog is een volledig beheerde cloudservice die als een registratie- en detectiesysteem voor gegevensassets van ondernemingen fungeert. Zie [Wat is Azure Data Catalog?](overview.md) voor een gedetailleerd overzicht.

In deze snelstartgids helpt u aan de slag met het maken van een Azure Data Catalog.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u wilt beginnen, moet u beschikken over:

* Een [Microsoft Azure](https://azure.microsoft.com/) abonnement.
* U moet beschikken over uw eigen [Azure Active Directory-tenant](../active-directory/fundamentals/active-directory-access-create-new-tenant.md).

Als u Data Catalog instelt, moet u de eigenaar of mede-eigenaar van een Azure-abonnement zijn.

## <a name="create-a-data-catalog"></a>Een catalogus met gegevens maken

U kunt per organisatie (Azure Active Directory-domein) slechts één gegevenscatalogus inrichten. Dus als de eigenaar of mede-eigenaar van een Azure-abonnement die deel uitmaakt van deze Azure Active Directory-domein is al een catalogus worden gemaakt, kan niet maakt u een catalogus opnieuw, zelfs als u meerdere Azure-abonnementen hebt. Als u wilt testen of er al een gegevenscatalogus is gemaakt door een gebruiker in uw Azure Active Directory-domein, gaat u naar de [startpagina van Azure Data Catalog](http://azuredatacatalog.com) en controleert u of u de catalogus ziet. Als er al een catalogus voor u is gemaakt, slaat u de volgende procedure over en gaat u naar de volgende sectie.

1. Ga naar de [Azure-portal](https://portal.azure.com) > **een resource maken** en selecteer **Data Catalog**.

    ![Maken van Data Catalog](media/data-catalog-get-started/data-catalog-create.png)

2. Geef een **naam** voor de data catalog, de **abonnement** u wilt gebruiken, de **locatie** voor de catalogus, en de **prijscategorie**. Selecteer vervolgens **Maken**.

3. Ga naar de [startpagina van Azure Data Catalog](http://azuredatacatalog.com) en klik op **Gegevens publiceren**.

   ![Azure Data Catalog - de knop Gegevens publiceren](media/data-catalog-get-started/data-catalog-publish-data.png)

   U krijgt ook naar de startpagina van de catalogus met gegevens uit de [Data Catalog-servicepagina](https://azure.microsoft.com/services/data-catalog) hiervoor **aan de slag**.

   ![Azure Data Catalog - marketingstartpagina](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)

4. Ga naar de **instellingen** pagina.

    ![Azure Data Catalog - gegevenscatalogus inrichten](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)

5. Vouw **prijzen** en controleer of uw Azure Data Catalog **edition** (gratis of Standard).

    ![Azure Data Catalog - editie selecteren](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)

6. Als u ervoor kiest *Standard* editie als uw prijscategorie die u kunt uitvouwen **beveiligingsgroepen** en Active Directory-beveiligingsgroepen voor toegang tot Data Catalog en automatische aanpassing van de inschakelen toestemming inschakelen facturering.

    ![Azure Data Catalog-beveiligingsgroepen](media/data-catalog-get-started/data-catalog-standard-security-groups.png)

7. Vouw **Catalogusgebruikers** uit en klik op **Toevoegen** om gebruikers toe te voegen aan de gegevenscatalogus. U bent automatisch toegevoegd aan deze groep.

    ![Azure Data Catalog - gebruikers](media/data-catalog-get-started/data-catalog-add-catalog-user.png)

8. Als u ervoor kiest *Standard* editie als uw prijscategorie die u kunt uitvouwen **Woordenlijstbeheerders** en klikt u op **toevoegen** verklarende woordenlijst beheerder gebruikers toe te voegen. U bent automatisch toegevoegd aan deze groep.

    ![Azure Data Catalog-verklarende woordenlijst voor beheerders](media/data-catalog-get-started/data-catalog-standard-glossary-admin.png)

9. Vouw **Catalogusbeheerders** uit en klik op **Toevoegen** om meer beheerders toe te voegen aan de gegevenscatalogus. U bent automatisch toegevoegd aan deze groep.

    ![Azure Data Catalog - beheerders](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)

10. Vouw **Portal titel** en toevoegen van aanvullende tekst die moet worden weergegeven in de titel van de portal.

    ![Titel van Azure Data Catalog-Portal](media/data-catalog-get-started/data-catalog-portal-title.png)

11. Nadat u hebt voltooid, de **instellingen** pagina, naast gaat u naar de **publiceren** pagina.

    ![Azure Data Catalog - gemaakt](media/data-catalog-get-started/data-catalog-created.png)

## <a name="find-a-data-catalog-in-the-azure-portal"></a>Zoeken naar een gegevenscatalogus in de Azure Portal

1. Ga op een afzonderlijk tabblad in de webbrowser of in een afzonderlijke geopend browservenster naar de [Azure Portal](https://portal.azure.com) en meld u aan met hetzelfde account dat u in de vorige stap hebt gebruikt voor het maken van de gegevenscatalogus.

2. Selecteer **alle services** en klik vervolgens op **Data Catalog**.

    ![Azure Data Catalog-bladeren door Azure](media/data-catalog-get-started/data-catalog-browse-azure-portal.png)

    U ziet de gegevenscatalogus die u hebt gemaakt.

    ![Azure Data Catalog - catalogus in lijst bekijken](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)

3. Klik op de catalogus die u hebt gemaakt. U ziet de blade **Gegevenscatalogus** in de portal.

   ![Azure Data Catalog - blade in portal](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)

4. U kunt de eigenschappen van de gegevenscatalogus bekijken en bijwerken. Klik bijvoorbeeld op **Prijscategorie** en wijzig de editie.

    ![Azure Data Catalog - prijscategorie](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd over het maken van een Azure Data Catalog voor uw organisatie. U kunt nu gegevensbronnen registreren in uw data catalog.

> [!div class="nextstepaction"]
> [Gegevensbronnen registreren in Azure Data Catalog](data-catalog-how-to-register.md)
