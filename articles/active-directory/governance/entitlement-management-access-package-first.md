---
title: 'Zelf studie: uw eerste toegangs pakket maken in azure AD rechten beheer (preview)-Azure Active Directory'
description: Stapsgewijze zelf studie voor het maken van uw eerste toegangs pakket in Azure Active Directory rechten beheer (preview).
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.subservice: compliance
ms.date: 07/23/2019
ms.author: ajburnle
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1688651466ba6748e1254c9d33bb24435602868b
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489174"
---
# <a name="tutorial-create-your-first-access-package-in-azure-ad-entitlement-management-preview"></a>Zelfstudie: Uw eerste toegangs pakket maken in het beheer van rechten van Azure AD (preview-versie)

> [!IMPORTANT]
> Azure Active Directory (Azure AD)-rechts beheer is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Het beheren van de toegang tot alle resources die werk nemers nodig hebben, zoals groepen, toepassingen en sites, is een belang rijke functie voor organisaties. U wilt werk nemers het juiste toegangs niveau geven om productief te zijn en hun toegang te verwijderen wanneer deze niet meer nodig is.

In deze zelf studie werkt u voor de Woodgrove Bank als IT-beheerder. U wordt gevraagd om een pakket met resources te maken voor een webproject dat interne gebruikers een self-service aanvraag kunnen indienen. Aanvragen vereisen goed keuring en de gebruikers toegang verloopt na 30 dagen. Voor deze zelf studie zijn de resources van het webproject slechts lid van één groep, maar dit kan een verzameling van groepen, toepassingen of share point online-sites zijn.

![Overzicht van scenario's](./media/entitlement-management-access-package-first/elm-scenario-overview.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een toegangs pakket maken met een groep als een resource
> * Een fiatteur aanwijzen
> * Laat zien hoe een interne gebruiker het toegangs pakket kan aanvragen
> * De toegangs aanvraag goed keuren

Als u geen Azure AD Premium P2 of Enterprise Mobility + Security E5-licentie hebt, maakt u een gratis [proef versie van Enterprise Mobility + Security E5](https://signup.microsoft.com/Signup?OfferId=87dd2714-d452-48a0-a809-d2f58c4f68b7&ali=1).

## <a name="prerequisites"></a>Vereisten

Als u Azure AD-rechts beheer (preview) wilt gebruiken, hebt u een van de volgende licenties nodig:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5-licentie

## <a name="step-1-set-up-users-and-group"></a>Stap 1: Gebruikers en groepen instellen

Een resource directory bevat een of meer resources om te delen. In deze stap maakt u een groep met de naam **technische groep** in de map Woodgrove Bank die de doel resource is voor het beheer van rechten. U hebt ook een interne aanvrager ingesteld.

**Vereiste rol:** Globale beheerder of gebruikers beheerder

![Gebruikers en groepen maken](./media/entitlement-management-access-package-first/elm-users-groups.png)

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als globale beheerder of gebruikers beheerder.  

1. Klik in de linkernavigatiebalk op **Azure Active Directory**.

1. De volgende twee gebruikers maken of configureren. U kunt deze namen of andere namen gebruiken. **Admin1** kan de gebruiker zijn met wie u momenteel bent aangemeld.

    | Name | Maprol | Description |
    | --- | --- | --- |
    | **Admin1** | Globale beheerder<br/>-of-<br/>Beperkte beheerder (gebruikers beheerder) | Beheerder en goed keurder |
    | **Requestor1** | Gebruiker | Interne aanvrager |

    Voor deze zelf studie zijn de beheerder en goed keurder dezelfde persoon, maar u wijst meestal een of meer personen aan die goed keurders zijn.

1. Maak een Azure AD-beveiligings groep met de naam **technische groep** met het lidmaatschaps type **toegewezen**.

    Deze groep wordt de doel resource voor het beheer van rechten. De groep moet leeg zijn om te starten.

## <a name="step-2-create-an-access-package"></a>Stap 2: Een toegangs pakket maken

Een *toegangs pakket* is een bundel van alle resources die een gebruiker nodig heeft om te werken aan een project of om de taak uit te voeren. Toegangs pakketten worden gedefinieerd in containers die *catalogi*worden genoemd. In deze stap maakt u een **toegangs pakket** voor het web-project in de **algemene** catalogus.

**Vereiste rol:** Globale beheerder of gebruikers beheerder

![Een toegangs pakket maken](./media/entitlement-management-access-package-first/elm-access-package.png)

1. Klik in de Azure Portal in het navigatie venster links op **Azure Active Directory**.

1. Klik in het linkermenu op **Identity governance**

1. Klik in het linkermenu op **toegangs pakketten**.  Als u de **toegang geweigerd**ziet, moet u ervoor zorgen dat er een Azure AD Premium P2-licentie aanwezig is in die map.

1. Klik op **nieuw toegangs pakket**.

    ![Het beheer van rechten in het Azure Portal](./media/entitlement-management-access-package-first/access-packages-list.png)

1. Op het tabblad **basis beginselen** typt u het toegangs pakket naam **web project** en beschrijving **voor het technisch webproject**.

1. Laat de vervolg keuzelijst **catalogus** ingesteld op **Algemeen**.

    ![Nieuw toegangs pakket-tabblad basis](./media/entitlement-management-access-package-first/basics.png)

1. Klik op **volgende** om het tabblad **resource rollen** te openen.

    Op dit tabblad selecteert u de machtigingen die u wilt toevoegen in het toegangs pakket.

1. Klik op **groepen**.

1. Zoek en selecteer in het deel venster groepen selecteren de groep van **technische groepen** die u eerder hebt gemaakt.

    Standaard ziet u groepen binnen en buiten de **algemene** catalogus. Wanneer u een groep buiten de **algemene** catalogus selecteert, wordt deze toegevoegd aan de **algemene** catalogus.

    ![Nieuw toegangs pakket-tabblad Resource rollen](./media/entitlement-management-access-package-first/resource-roles-select-groups.png)

1. Klik op **selecteren** om de groep toe te voegen aan de lijst.

1. Selecteer **lid**in de vervolg keuzelijst **rol** .

    ![Nieuw toegangs pakket-tabblad Resource rollen](./media/entitlement-management-access-package-first/resource-roles.png)

1. Klik op **volgende** om het tabblad **beleid** te openen.

1. Stel de schakel optie **eerste beleid maken** in op **later**.

    U maakt het beleid in de volgende sectie.

    ![Nieuw toegangs pakket-tabblad beleid](./media/entitlement-management-access-package-first/policy.png)

1. Klik op **volgende** om het tabblad **controleren + maken** te openen.

    ![Nieuw toegangs pakket-tabblad controleren + maken](./media/entitlement-management-access-package-first/review-create.png)

1. Controleer de instellingen van het toegangs pakket en klik vervolgens op **maken**.

    Mogelijk wordt er een bericht weer gegeven dat het toegangs pakket niet zichtbaar is voor gebruikers omdat de catalogus nog niet is ingeschakeld.

    ![Nieuw toegangs pakket-niet zichtbaar bericht](./media/entitlement-management-access-package-first/not-visible.png)

1. Klik op **OK**.

    Na enkele ogen blikken ziet u een melding dat het toegangs pakket is gemaakt.

## <a name="step-3-create-a-policy"></a>Stap 3: Een beleid maken

Een *beleid* definieert de regels of Guardrails voor toegang tot een toegangs pakket. In deze stap maakt u een beleid waarmee een specifieke gebruiker in de resource directory het toegangs pakket kan aanvragen. U kunt ook opgeven dat aanvragen moeten worden goedgekeurd en wie de goed keurder wordt.

![Een toegangs pakket beleid maken](./media/entitlement-management-access-package-first/elm-access-package-policy.png)

**Vereiste rol:** Globale beheerder of gebruikers beheerder

1. Klik in het **pakket web project Access**in het menu links op **beleids regels**.

    ![Lijst met beleids regels voor toegang tot pakketten](./media/entitlement-management-access-package-first/policies-list.png)

1. Klik op **beleid toevoegen** om beleid maken te openen.

1. Typ de naam **intern beleid** en beschrijving van de aanvrager **kunnen gebruikers in deze map toegang tot de resources van het webproject aanvragen**.

1. Klik in de sectie **gebruikers die toegang kunnen aanvragen** , op **voor gebruikers in uw directory**.

    ![Beleid maken](./media/entitlement-management-access-package-first/policy-create.png)

1. Schuif omlaag naar de sectie **gebruikers en groepen selecteren** en klik op **gebruikers en groepen toevoegen**.

1. Selecteer in het deel venster gebruikers en groepen selecteren de **Requestor1** -gebruiker die u eerder hebt gemaakt en klik vervolgens op **selecteren**.

1. Stel in het gedeelte **aanvraag** **goed keuring vereisen** in op **Ja**.

1. Klik in de sectie **goed keurders selecteren** op **goed keurders toevoegen**.

1. Selecteer in het deel venster fiatteurs selecteren de **admin1** die u eerder hebt gemaakt en klik vervolgens op **selecteren**.

    Voor deze zelf studie zijn de beheerder en goed keurder dezelfde persoon, maar u kunt een andere persoon aanwijzen als fiatteur.

1. Stel in de sectie **verloop** het **toegangs pakket** in op **aantal dagen**.

1. Instellen van **toegang verloopt na** tot **30** dagen.

1. Klik op **Ja**om **beleid in te scha kelen**.

    ![Beleids instellingen maken](./media/entitlement-management-access-package-first/policy-create-settings.png)

1. Klik op **maken** om het **interne beleid**voor de aanvrager te maken.

1. Klik in het menu links van het pakket web project Access op **overzicht**.

1. Kopieer de **koppeling naar de portal van mijn toegang**.

    U gebruikt deze koppeling voor de volgende stap.

    ![Overzicht van toegangs pakketten-koppeling naar mijn Access-Portal](./media/entitlement-management-shared/my-access-portal-link.png)

## <a name="step-4-request-access"></a>Stap 4: Toegang aanvragen

In deze stap voert u de stappen uit als **interne aanvrager** en vraagt u toegang tot het toegangs pakket. Aanvragers verzenden hun aanvragen met behulp van een site met de naam mijn toegangs Portal. De portal van mijn toegang kan aanvragers aanvragen voor toegangs pakketten verzenden, de toegangs pakketten waartoe ze al toegang hebben, en hun aanvraag geschiedenis weer geven.

**Vereiste rol:** Interne aanvrager

1. Meld u af bij de Azure Portal.

1. Navigeer in een nieuw browser venster naar de Portal koppeling van mijn toegang die u in de vorige stap hebt gekopieerd.

1. Meld u aan bij de portal My Access als **Requestor1**.

    Het **toegangs pakket voor het web-project wordt**weer geven.

1. Als dat nodig is, klikt u in de kolom **Beschrijving** op de pijl om de details van het toegangs pakket weer te geven.

    ![Mijn Access-Portal-toegangs pakketten](./media/entitlement-management-shared/my-access-access-packages.png)

1. Klik op het vinkje om het pakket te selecteren.

1. Klik op **toegang aanvragen** om het deel venster toegang aanvragen te openen.

1. Typ in het vak **zakelijke rechtvaardiging** de reden **voor het werken met**het webproject.

1. Stel de **aanvraag voor een specifieke periode** in op **Ja**.

1. Stel de **begin datum** in op vandaag en **eind datum** op morgen.

    ![Mijn Access-portal-toegang aanvragen](./media/entitlement-management-shared/my-access-request-access.png)

1. Klik op **Indienen**.

1. Klik in het menu links op **aanvraag geschiedenis** om te controleren of uw aanvraag is verzonden.

## <a name="step-5-approve-access-request"></a>Stap 5: Toegangs aanvraag goed keuren

In deze stap meldt u zich aan als de gebruiker van de **goed keurder** en keurt u de toegangs aanvraag voor de interne aanvrager goed. Fiatteurs gebruiken dezelfde mijn toegangs portal als aanvragers om aanvragen te verzenden. Met de portal mijn toegang kunnen goed keurders wachtende goed keuringen weer geven en aanvragen goed keuren of weigeren.

**Vereiste rol:** Goedkeurder

1. Meld u af bij de portal van mijn toegang.

1. Meld u aan bij de [Portal My Access](https://myaccess.microsoft.com) als **admin1**.

1. Klik in het menu links op **goed keuringen**.

1. Zoek op het tabblad **in behandeling** **Requestor1**.

    Als u de aanvraag van Requestor1 niet ziet, wacht u een paar minuten en probeert u het opnieuw.

1. Klik op de koppeling **weer geven** om het deel venster toegangs aanvraag te openen.

1. Klik op **goed keuren**.

1. Typ in het vak **reden** de reden **voor het goed keuren van de toegang voor**het webproject.

    ![Mijn toegangs Portal-toegangs aanvraag](./media/entitlement-management-shared/my-access-approve-request.png)

1. Klik op **verzenden** om uw beslissing in te dienen.

    Er wordt een bericht weer gegeven dat het goed is goedgekeurd.

## <a name="step-6-validate-that-access-has-been-assigned"></a>Stap 6: Controleren of de toegang is toegewezen

Nu u de toegangs aanvraag hebt goedgekeurd, bevestigt u in deze stap dat de **interne aanvrager** het toegangs pakket heeft toegewezen en dat ze nu lid zijn van de groep **technische groepen** .

**Vereiste rol:** Globale beheerder of gebruikers beheerder

1. Meld u af bij de portal van mijn toegang.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als de **admin1**.

1. Klik op **Azure Active Directory** en klik vervolgens op **Identity governance**.

1. Klik in het linkermenu op **toegangs pakketten**.

1. Zoek en klik op **web project Access package**.

1. Klik in het menu links op **aanvragen**.

    U ziet Requestor1 en het interne beleid van de aanvrager met de status **bezorgd**.

1. Klik op de aanvraag om de details van de aanvraag weer te geven.

    ![Toegangs pakket-aanvraag Details](./media/entitlement-management-access-package-first/request-details.png)

1. Klik in de linkernavigatiebalk op **Azure Active Directory**.

1. Klik op **groepen** en open de groep **technische groep** .

1. Klik op **leden**.

    U ziet **Requestor1** vermeld als een lid.

    ![Leden van technische groep](./media/entitlement-management-access-package-first/group-members.png)

## <a name="step-7-clean-up-resources"></a>Stap 7: Resources opschonen

In deze stap verwijdert u de wijzigingen die u hebt aangebracht en verwijdert u het toegangs pakket voor het toegangs pakket voor het **Web** .

**Vereiste rol:**  Globale beheerder of gebruikers beheerder

1. Klik in de Azure Portal op **Azure Active Directory** en klik vervolgens op **Identity governance**.

1. Open het **toegangs pakket**voor het web-project.

1. Klik op **toewijzingen**.

1. Klik voor **Requestor1**op het weglatings teken ( **...** ) en klik vervolgens op **toegang verwijderen**.

    De status wordt gewijzigd van bezorgd in verlopen.

1. Klik op **beleids regels**.

1. Klik voor **intern beleid**van de aanvrager op het weglatings teken ( **...** ) en klik vervolgens op **verwijderen**.

1. Klik op **resource rollen**.

1. Klik voor **technische groep**op het weglatings teken ( **...** ) en klik vervolgens op **resource functie verwijderen**.

1. Open de lijst met toegangs pakketten.

1. Klik voor **Access project web project**op het weglatings teken ( **...** ) en klik vervolgens op **verwijderen**.

1. In Azure Active Directory verwijdert u de gebruikers die u hebt gemaakt, zoals **Requestor1** en **admin1**.

1. De groep **technische groepen** verwijderen.

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over veelvoorkomende scenario stappen in het beheer van rechten.
> [!div class="nextstepaction"]
> [Algemene scenario's](entitlement-management-scenarios.md)
