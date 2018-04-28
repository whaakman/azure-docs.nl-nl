---
title: 'Zelfstudie: Azure Active Directory-integratie met Clever | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Clever.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 069ff13a-310e-4366-a147-d6ec5cca12a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: b7529b0942cd86b0d9e657d8d0f61313aa7f0a66
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-azure-active-directory-integration-with-clever"></a>Zelfstudie: Azure Active Directory-integratie met Clever

In deze zelfstudie leert u hoe Clever integreren met Azure Active Directory (Azure AD).

Clever integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Clever heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Clever (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Clever, moet u de volgende items:

- Een Azure AD-abonnement
- Een slimme eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Clever uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-clever-from-the-gallery"></a>Clever uit de galerie toevoegen
Voor het configureren van de integratie van Clever in Azure AD, moet u Clever uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Clever uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Clever**, selecteer **Clever** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Slimme in de lijst met resultaten](./media/active-directory-saas-clever-tutorial/tutorial_clever_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Clever op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Clever is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Clever tot stand worden gebracht.

Wijs in Clever, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Clever, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een slimme testgebruiker](#create-a-clever-test-user)**  - Clever die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing slimme configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Clever, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Clever** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-clever-tutorial/tutorial_clever_samlbase.png)

3. Op de **slimme domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Slimme domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-clever-tutorial/tutorial_clever_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://clever.com/in/<companyname>`

    b. In de **id** textbox, typ de URL: `https://clever.com/oauth/saml/metadata.xml`

    > [!NOTE]
    > Eenmalige aanmelding URL-waarde is geen echte. Deze waarde bijwerken met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [slimme Client ondersteuningsteam](https://clever.com/about/contact/) deze waarde op te halen.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-clever-tutorial/tutorial_metadataurl.png)

5. De slimme toepassing de SAML-asserties verwacht in een specifieke indeling waarvoor u het toevoegen van aangepast kenmerktoewijzingen aan uw **SAML-Token kenmerken** configuratie.

    De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-clever-tutorial/tutorial_clever_07.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding hierboven en voer de volgende stappen uit:
    
    | Naam kenmerk  | Waarde kenmerk |
    | --------------- | -------------------- |
    | clever.Teacher.credentials.district_username|User.userPrincipalName|
    | clever.student.credentials.district_username| User.userPrincipalName |
    | Voornaam  | User.givenName |
    | Achternaam  | User.surname |

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-clever-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-clever-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** textbox leeg.
    
    d. Klik op **OK**.
    
7. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-clever-tutorial/tutorial_general_400.png)

8. In een ander browservenster, meld u aan bij uw site slimme bedrijf als beheerder.

9. Klik in de werkbalk op **directe aanmelding**.

    ![Directe aanmelding](./media/active-directory-saas-clever-tutorial/ic798984.png "directe aanmelding")

    > [!NOTE]
    > Voordat u eenmalige aanmelding testen kunt, hebt u contact opnemen met [slimme Client ondersteuningsteam](https://clever.com/about/contact/) Office 365 eenmalige aanmelding inschakelen in de back-end.

10. Op de **directe aanmelding** pagina, voert u de volgende stappen uit:
    
      ![Directe aanmelding](./media/active-directory-saas-clever-tutorial/ic798985.png "directe aanmelding")
    
      a. Typ de **aanmeldings-URL**.
    
      >[!NOTE]
      >De **aanmeldings-URL** is een aangepaste waarde. Neem contact op met [slimme Client ondersteuningsteam](https://clever.com/about/contact/) deze waarde op te halen.
    
      b. Als **identiteitsbeheersysteem**, selecteer **ADFS**.

      c. In de **metagegevens-URL** textbox plakken **App-Url voor federatieve metagegevens** waarde die u hebt gekopieerd uit de Azure portal.
    
      d. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-clever-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-clever-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-clever-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-clever-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-clever-test-user"></a>Een slimme testgebruiker maken

Om Azure AD-gebruikers zich aanmelden bij Clever, moeten ze worden ingericht in Clever.

In geval van een Clever, werken met [slimme Client ondersteuningsteam](https://clever.com/about/contact/) toevoegen van de gebruikers in de slimme platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

>[!NOTE]
>U kunt een andere gebruiker slimme account hulpmiddelen voor het maken of API's die is geleverd door Clever voor het inrichten van Azure AD-gebruikersaccounts.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Clever.

![Toewijzen van de gebruikersrol][200]

**Britta Simon om aan te wijzen Clever, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **Clever**.

    ![De Clever koppelen in de lijst met toepassingen](./media/active-directory-saas-clever-tutorial/tutorial_clever_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Wanneer u klikt op de slimme tegel in het deelvenster toegang u moet ophalen automatisch aangemeld bij uw slimme toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clever-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clever-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clever-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clever-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clever-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clever-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clever-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clever-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clever-tutorial/tutorial_general_203.png

