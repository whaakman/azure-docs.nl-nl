---
title: 'Zelfstudie: uw eerste toegang-pakket maken in Azure AD-recht beheer (Preview) - Azure Active Directory'
description: Stapsgewijze zelfstudie voor het maken van uw eerste pakket van de toegang in Azure Active Directory waar u recht op management (Preview).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.subservice: compliance
ms.date: 04/27/2019
ms.author: rolyon
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 354af736d5896214848205f41e429d9bf2c49863
ms.sourcegitcommit: 8a681ba0aaba07965a2adba84a8407282b5762b2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64873495"
---
# <a name="tutorial-create-your-first-access-package-in-azure-ad-entitlement-management-preview"></a>Zelfstudie: Maak uw eerste toegang-pakket in beheer van de rechten van een Azure AD (Preview)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) waar u recht op management is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Beheer van toegang tot alle resources werknemers nodig, zoals groepen, toepassingen en websites, is een belangrijke functie voor organisaties. U wilt verlenen werknemers het juiste niveau van toegang, ze moeten productief en hun toegang verwijderen wanneer deze niet meer nodig is.

In deze zelfstudie werkt u voor de Woodgrove Bank als IT-beheerder. U hebt gevraagd om een pakket van resources voor een webproject dat interne gebruikers aanvraag Self-servicegebruikers kunnen te maken. Vraag goedkeuring van aanvragen en de toegang van gebruiker verloopt na 30 dagen. Voor deze zelfstudie de web-project-resources zijn alleen lid zijn van één groep, maar wordt een verzameling van groepen, toepassingen of SharePoint Online-sites.

![Overzicht van scenario's](./media/entitlement-management-access-package-first/elm-scenario-overview.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een access-pakket met een groep als een resource maken
> * Een fiatteur toewijzen
> * Laten zien hoe een interne gebruiker het pakket toegang kunt aanvragen
> * De aanvraag goedkeuren

Als u een Azure AD Premium P2 of Enterprise Mobility + Security E5-licentie hebt, maakt u een gratis [Enterprise Mobility + Security E5-proefversie](https://signup.microsoft.com/Signup?OfferId=87dd2714-d452-48a0-a809-d2f58c4f68b7&ali=1).

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van beheer van de rechten van een Azure AD (Preview), moet u een van de volgende licenties hebben:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5 license

## <a name="step-1-set-up-users-and-group"></a>Stap 1: Instellen van gebruikers en groepen

Een resource-map heeft een of meer resources te delen. In deze stap maakt u een groep met de naam **groep Engineering** in de Woodgrove Bank-map die de doelresource voor het beheer van rechten. U instellen een interne aanvrager ook.

**Vereiste rol:** Globale beheerder of Gebruikerbeheerder

![Gebruikers en groepen maken](./media/entitlement-management-access-package-first/elm-users-groups.png)

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als een globale beheerder of Gebruikerbeheerder.  

1. Klik in het linkernavigatievenster op **Azure Active Directory**.

1. Maken of configureren van de volgende twee gebruikers. U kunt deze namen of verschillende namen gebruiken. **Admin1** mag de gebruiker die u momenteel bent aangemeld.

    | Name | Maprol | Description |
    | --- | --- | --- |
    | **Admin1** | Globale beheerder<br/>-of-<br/>Beperkte beheerder (Gebruikersbeheerder) | De beheerder en de fiatteur |
    | **Requestor1** | Gebruiker | Interne aanvrager |

    Voor deze zelfstudie de beheerder en de fiatteur dezelfde persoon zijn, maar u een of meer mensen goedkeurders worden meestal aan te wijzen.

1. Een Azure AD-beveiligingsgroep maken met de naam groep **groep Engineering** met een lidmaatschapstype van **toegewezen**.

    Deze groep worden de doelresource voor het beheer van rechten. De groep mag niet leeg zijn van de leden te starten.

## <a name="step-2-create-an-access-package"></a>Stap 2: Een pakket toegang maken

Een *toegang pakket* is een samenstelling van alle resources die een gebruiker nodig heeft om te werken aan een project of hun taak uit te voeren. Toegang tot pakketten zijn gedefinieerd in containers met de naam *catalogi*. In deze stap maakt u een **Web project toegang pakket** in de **algemene** catalogus.

**Vereiste rol:** Globale beheerder of Gebruikerbeheerder

![Een pakket toegang maken](./media/entitlement-management-access-package-first/elm-access-package.png)

1. Klik in de Azure-portal, in het linker navigatiegedeelte op **Azure Active Directory**.

1. Klik in het menu links op **Identiteitsbestuur**

1. Klik in het menu links op **toegang tot pakketten**.  Als u ziet **toegang is geweigerd**, zorg ervoor dat een Azure AD Premium P2-licentie aanwezig zijn in die map.

1. Klik op **nieuwe toegang-pakket**.

    ![Beheer van rechten in de Azure-portal](./media/entitlement-management-access-package-first/access-packages-list.png)

1. Op de **basisbeginselen** tabblad, typt u de naam van de **Web project toegang pakket** en beschrijving **toegang-pakket voor de Engineering-webproject**.

1. Laat de **catalogus** vervolgkeuzelijst ingesteld op **algemene**.

    ![Nieuwe toegang-pakket - tabblad van de basisbeginselen](./media/entitlement-management-access-package-first/basics.png)

1. Klik op **volgende** openen de **resourcerollen** tabblad.

    Op dit tabblad selecteert u de machtigingen om op te nemen in het pakket voor toegang.

1. Klik op **groepen**.

1. Zoek in het deelvenster groepen selecteren en selecteer de **groep Engineering** groep die u eerder hebt gemaakt.

    Standaard ziet u groepen binnen en buiten de **algemene** catalogus. Wanneer u een groep buiten de **algemene** -catalogus, wordt deze toegevoegd aan de **algemene** catalogus.

    ![Nieuwe toegang-pakket - tabblad van Resource-rollen](./media/entitlement-management-access-package-first/resource-roles-select-groups.png)

1. Klik op **Selecteer** aan de groep toevoegen aan de lijst.

1. In de **rol** vervolgkeuzelijst, selecteer **lid**.

    ![Nieuwe toegang-pakket - tabblad van Resource-rollen](./media/entitlement-management-access-package-first/resource-roles.png)

1. Klik op **volgende** openen de **beleid** tabblad.

1. Stel de **eerste beleid maken** overzet naar **Later**.

    In de volgende sectie maakt u het beleid.

    ![Nieuwe toegang-pakket - tabblad beleid](./media/entitlement-management-access-package-first/policy.png)

1. Klik op **volgende** openen de **revisie + maken** tabblad.

    ![Nieuwe toegang-pakket - beoordelen en tabblad maken](./media/entitlement-management-access-package-first/review-create.png)

1. Controleer de instellingen voor toegang tot pakketten en klik op **maken**.

    U ziet een bericht dat het pakket toegang niet zichtbaar voor gebruikers zijn omdat de catalogus nog niet is ingeschakeld.

    ![Nieuwe toegang-pakket - bericht niet zichtbaar](./media/entitlement-management-access-package-first/not-visible.png)

1. Klik op **OK**.

    Na enkele ogenblikken ziet u een melding of het pakket toegang is gemaakt.

## <a name="step-3-create-a-policy"></a>Stap 3: Een beleid maken

Een *beleid* definieert de regels of guardrails voor toegang tot een package toegang. In deze stap maakt u een beleid waarmee een specifieke gebruiker in de map resource om aan te vragen van de access-pakket. U ook opgeven dat aanvragen moeten worden goedgekeurd en die de fiatteur.

![Een pakket toegangsbeleid maken](./media/entitlement-management-access-package-first/elm-access-package-policy.png)

**Vereiste rol:** Globale beheerder of Gebruikerbeheerder

1. In de **Web project toegang pakket**, klik in het menu links op **beleid**.

    ![Lijst met beleidsregels voor toegang-pakket](./media/entitlement-management-access-package-first/policies-list.png)

1. Klik op **beleid toevoegen** beleid maken te openen.

1. Typ de naam **interne aanvrager beleid** en beschrijving **kunnen gebruikers in deze map voor het aanvragen van toegang tot resources van de project web**.

1. In de **gebruikers die toegang kunnen vragen** sectie, klikt u op **voor gebruikers in uw directory**.

    ![Beleid maken](./media/entitlement-management-access-package-first/policy-create.png)

1. Schuif omlaag naar de **gebruikers en groepen selecteren** sectie en klikt u op **gebruikers en groepen toevoegen**.

1. Selecteer in de geselecteerde gebruikers en groepen deelvenster de **Requestor1** gebruiker die u eerder hebt gemaakt en klik vervolgens op **Selecteer**.

1. In de **aanvragen** sectie, stelt u de **goedkeuring vereisen** naar **Ja**.

1. In de **fiatteurs selecteren** sectie, klikt u op **fiatteurs toevoegen**.

1. Selecteer in het deelvenster Selecteer goedkeurders de **Admin1** u eerder hebt gemaakt en klik vervolgens op **Selecteer**.

    Voor deze zelfstudie de beheerder en de fiatteur dezelfde persoon zijn, maar u kunt een andere persoon opgeven als een goedkeurder.

1. In de **verlopen** sectie, stelt u de **toegang pakket verloopt** naar **aantal dagen**.

1. Stel **toegang verloopt na** naar **30** dagen.

1. Voor **beleid inschakelen**, klikt u op **Ja**.

    ![Beleidsinstellingen maken](./media/entitlement-management-access-package-first/policy-create-settings.png)

1. Klik op **maken** maken de **interne aanvrager beleid**.

1. Klik in het menu links van het pakket Web project toegang op **overzicht**.

1. Kopieer de **koppeling naar de mijn toegang**.

    U gebruikt deze koppeling voor de volgende stap.

    ![Overzicht van Access-pakket - koppeling naar de mijn toegang](./media/entitlement-management-shared/my-access-portal-link.png)

## <a name="step-4-request-access"></a>Stap 4: Toegang aanvragen

In deze stap maakt u te werk als de **interne aanvrager** en toegang aanvragen tot het pakket toegang. Aanvragers indienen hun met behulp van een site met de naam van de portal mijn toegang aanvragen. De portal mijn toegang kunt aanvragers te verzenden aanvragen voor toegang tot pakketten, raadpleegt u de toegang tot pakketten ze al hebben toegang tot en hun aanvraaggeschiedenis weergeven.

**Vereiste rol:** Interne aanvrager

1. Afmelden bij de Azure-portal.

1. Navigeer in een nieuw browservenster naar de portal My Access-koppeling die u in de vorige stap hebt gekopieerd.

1. Aanmelden bij de portal mijn toegang als **Requestor1**.

    U ziet de **Web project toegang pakket**.

1. Indien nodig, in de **beschrijving** kolom, klikt u op de pijl om weer te geven details over het pakket toegang.

    ![Mijn toegangsportal - toegang tot pakketten](./media/entitlement-management-shared/my-access-access-packages.png)

1. Klik op het vinkje om te selecteren van het pakket.

1. Klik op **toegang aanvragen** om de aanvraag voor toegang tot deelvenster te openen.

1. In de **zakelijke reden** typt u de reden van de **werkt aan webproject**.

1. Stel de **aanvragen voor die specifieke periode** overzet naar **Ja**.

1. Stel de **begindatum** op vandaag en **einddatum** aan morgen.

    ![Mijn toegangsportal - aanvraag voor toegang](./media/entitlement-management-shared/my-access-request-access.png)

1. Klik op **Submit**

1. Klik in het menu links op **geschiedenis van de aanvraag** om te controleren of dat uw aanvraag is verzonden.

## <a name="step-5-approve-access-request"></a>Stap 5: Aanvraag voor toegang tot goedkeuren

In deze stap maakt u zich aanmelden als de **goedkeurder** gebruiker en de aanvraag voor de interne aanvrager goed te keuren. Fiatteurs gebruiken dezelfde portal mijn toegang zoals aanvragers gebruiken voor het indienen van aanvragen. Met behulp van de portal My Access, kunnen goedkeurders aangevraagde goedkeuringen bekijken en goedkeuren of afwijzen aanvragen.

**Vereiste rol:** Goedkeurder

1. Afmelden bij de portal mijn toegang.

1. Aanmelden bij de [mijn toegangsportal](https://myaccess.microsoft.com) als **Admin1**.

1. Klik in het menu links op **goedkeuringen**.

1. Op de **in behandeling** tabblad, zoeken **Requestor1**.

    Als u de aanvraag van Requestor1 niet ziet, wacht een paar minuten en probeer het opnieuw.

1. Klik op de **weergave** koppeling om de toegang tot aanvraag deelvenster te openen.

1. Klik op **goedkeuren**.

1. In de **reden** typt u de reden **toegang voor web-project goedgekeurd**.

    ![Mijn toegangsportal - aanvraag voor toegang tot](./media/entitlement-management-shared/my-access-approve-request.png)

1. Klik op **indienen** om in te dienen uw beslissing.

    U ziet een bericht met succes is goedgekeurd.

## <a name="step-6-validate-that-access-has-been-assigned"></a>Stap 6: Valideren dat toegang is toegewezen

Nu dat u hebt goedgekeurd de toegangsaanvraag in deze stap maakt u bevestigen dat de **interne aanvrager** aan is toegewezen voor toegang tot de pakket- en ze zijn nu lid is van de **groep Engineering** groep.

**Vereiste rol:** Globale beheerder of Gebruikerbeheerder

1. Afmelden bij de portal mijn toegang.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als de **Admin1**.

1. Klik op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Klik in het menu links op **toegang tot pakketten**.

1. Zoek en klik op **Web project toegang pakket**.

1. Klik in het menu links op **aanvragen**.

    U ziet Requestor1 en het beleid voor interne aanvrager met de status van **geleverd**.

1. Klik op de aanvraag om de aanvraagdetails te bekijken.

    ![Toegang tot package - details van de aanvraag](./media/entitlement-management-access-package-first/request-details.png)

1. Klik in het linkernavigatievenster op **Azure Active Directory**.

1. Klik op **groepen** en open de **groep Engineering** groep.

1. Klik op **leden**.

    U ziet **Requestor1** vermeld als een lid.

    ![Leden van de groep engineering](./media/entitlement-management-access-package-first/group-members.png)

## <a name="step-7-clean-up-resources"></a>Stap 7: Resources opschonen

In deze stap maakt u de wijzigingen verwijderen en verwijdert de **Web project toegang pakket** toegang-pakket.

**Vereiste rol:**  Globale beheerder of Gebruikerbeheerder

1. Klik in de Azure-portal op **Azure Active Directory** en klik vervolgens op **Identiteitsbestuur**.

1. Open **Web project toegang pakket**.

1. Klik op **toewijzingen**.

1. Voor **Requestor1**, klik op het weglatingsteken (**...** ) en klik vervolgens op **toegang verwijderen**.

    De status wordt gewijzigd van geleverd om te verlopen.

1. Klik op **beleid**.

1. Voor **interne aanvrager beleid**, klik op het weglatingsteken (**...** ) en klik vervolgens op **verwijderen**.

1. Klik op **resourcerollen**.

1. Voor **groep Engineering**, klik op het weglatingsteken (**...** ) en klik vervolgens op **verwijderen resourcerol**.

1. Open de lijst met toegang tot pakketten.

1. Voor **webproject project toegang**, klik op het weglatingsteken (**...** ) en klik vervolgens op **verwijderen**.

1. In Azure Active Directory, verwijdert u alle gebruikers die u hebt gemaakt, zoals **Requestor1** en **Admin1**.

1. Verwijder de **groep Engineering** groep.

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over veelvoorkomende scenariostappen voor het beheer van rechten.
> [!div class="nextstepaction"]
> [Algemene scenario's](entitlement-management-scenarios.md)
