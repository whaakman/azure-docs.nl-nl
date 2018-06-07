---
title: 'Zelfstudie: Azure Active Directory-integratie met voorzien CX Suite | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en CX-Suite voorzien.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5f4b7830-6186-4d17-b77b-504d4192bfde
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: jeedes
ms.openlocfilehash: 62ce16969f31a8909218495e3f130bc6713622dc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654882"
---
# <a name="tutorial-azure-active-directory-integration-with-foresee-cx-suite"></a>Zelfstudie: Azure Active Directory-integratie met voorzien CX-Suite

In deze zelfstudie leert u hoe voorzien CX Suite integreren met Azure Active Directory (Azure AD).

Voorzien CX Suite integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot CX Suite voorzien heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij CX-Suite voorzien (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met CX-Suite voorzien, moet u de volgende items:

- Een Azure AD-abonnement
- Een voorzien CX Suite eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. CX Suite voorzien in de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-foresee-cx-suite-from-the-gallery"></a>CX Suite voorzien in de galerie toevoegen
Voor het configureren van de integratie van CX Suite voorzien in Azure AD, moet u voorzien CX Suite uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen CX Suite voorzien in de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **voorzien CX Suite**, selecteer **voorzien CX Suite** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![CX Suite voorzien in de lijst met resultaten](./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_foreseecxsuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met voorzien CX Suite op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in CX Suite voorzien in Azure AD voor een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in voorzien CX Suite tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met CX-Suite voorzien, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker voorzien CX Suite](#create-a-foresee-cx-suite-test-user)**  - voorzien CX-pakket dat is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing CX Suite voorzien.

**Voor het configureren van Azure AD eenmalige aanmelding met CX-Suite voorzien, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **voorzien CX Suite** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_foreseecxsuite_samlbase.png)

3. Op de **voorzien CX Suite domein en URL's** sectie, hebt u **metagegevensbestand van de serviceprovider**, voer de volgende stappen uit:

    ![URL's en CX Suite domein één aanmelding informatie voorzien](./media/active-directory-saas-foreseecxsuite-tutorial/upload.png)

    a. Klik op **uploaden metagegevensbestand**.

    ![URL's en CX Suite domein één aanmelding informatie voorzien](./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_foreseen_uploadconfig.png)

    b. Klik op **map logo** het metagegevensbestand selecteren en op **uploaden**.

    c. Na voltooiing van het uploaden van **metagegevensbestand van de serviceprovider** de **id** waarde get automatisch ingevuld in **voorzien CX Suite domein en URL's** sectie TextBox zoals hieronder wordt weergegeven:

    ![URL's en CX Suite domein één aanmelding informatie voorzien](./media/active-directory-saas-foreseecxsuite-tutorial/urlupload.png)

4. Als u geen **metagegevensbestand van de serviceprovider**, voer de volgende stappen uit:

    ![URL's en CX Suite domein één aanmelding informatie voorzien](./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_foreseecxsuite_url.png)

    a. In de **aanmeldings-URL** textbox, typ de URL: `https://cxsuite.foresee.com/`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://www.okta.com/saml2/service-provider/<UniqueID>`

    > [!NOTE]
    > De id-waarde is geen echte. Deze waarde door de werkelijke ID bijwerken. Neem contact op met [voorzien CX Suite Client ondersteuningsteam](mailto:support@foresee.com) deze waarde op te halen.

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_foreseecxsuite_certificate.png)

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_general_400.png)

7. Eenmalige aanmelding configureren op **voorzien CX Suite** zijde, moet u de gedownloade verzenden **Metadata XML** naar [voorzien CX Suite ondersteuningsteam](mailto:support@foresee.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-foreseecxsuite-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-foreseecxsuite-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-foreseecxsuite-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-foreseecxsuite-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-foresee-cx-suite-test-user"></a>Maak een testgebruiker voorzien CX-Suite

In deze sectie maakt u Britta Simon aangeroepen in CX Suite voorzien van een gebruiker. Werken met [voorzien CX Suite ondersteuningsteam](mailto:support@foresee.com) om toe te voegen de gebruikers of het domein die nodig is om de wilt plaatsen van het platform CX Suite voorzien. Als het domein is toegevoegd door het team, krijgen automatisch gebruikers ingericht voor het platform CX Suite voorzien. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon toegang verlenen aan voorzien CX Suite gebruikt Azure eenmalige aanmelding.

![Toewijzen van de gebruikersrol][200]

**Als u wilt toewijzen Britta Simon CX Suite voorzien, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **voorzien CX Suite**.

    ![De koppeling CX Suite voorzien in de lijst met toepassingen](./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_foreseecxsuite_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel CX Suite voorzien in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing CX Suite voorzien.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-foreseecxsuite-tutorial/tutorial_general_203.png

