---
title: 'Zelfstudie: Azure Active Directory-integratie met Workteam | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Workteam.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 41df17a1-ba69-414f-8ec3-11079b030df6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a7cd986544dfb1472f5cc8a013fec951dca42a59
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57898649"
---
# <a name="tutorial-azure-active-directory-integration-with-workteam"></a>Zelfstudie: Azure Active Directory-integratie met Workteam

In deze zelfstudie leert u hoe u Workteam integreren met Azure Active Directory (Azure AD).

Workteam integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Workteam heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Workteam (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Workteam, moet u de volgende items:

- Een Azure AD-abonnement
- Een Workteam eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Workteam uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-workteam-from-the-gallery"></a>Workteam uit de galerie toe te voegen
Voor het configureren van de integratie van Workteam in Azure AD, moet u Workteam uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Workteam uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Workteam**, selecteer **Workteam** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Workteam in de lijst met resultaten](./media/workteam-tutorial/tutorial_workteam_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Workteam op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Workteam is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Workteam tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Workteam, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker Workteam](#create-a-workteam-test-user)**  : als u wilt een equivalent van Britta Simon in Workteam die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Workteam.

**Voor het configureren van Azure AD eenmalige aanmelding met Workteam, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Workteam** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/workteam-tutorial/tutorial_workteam_samlbase.png)

3. Op de **Workteam domein en URL's** sectie, de gebruiker beschikt niet over de stappen uitvoeren omdat de toepassing is al vooraf geïntegreerd met Azure.

    ![Workteam domein en URL's, eenmalige aanmelding informatie](./media/workteam-tutorial/tutorial_workteam_url.png)

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Workteam domein en URL's, eenmalige aanmelding informatie](./media/workteam-tutorial/tutorial_workteam_url1.png)

    Typ een URL in het tekstvak **Aanmeldings-URL**: `https://app.workte.am`
     
5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/workteam-tutorial/tutorial_workteam_certificate.png) 

6. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/workteam-tutorial/tutorial_general_400.png)
    
7. Op de **Workteam configuratie** sectie, klikt u op **configureren Workteam** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Workteam configuratie](./media/workteam-tutorial/tutorial_workteam_configure.png) 

8. In een ander browservenster aanmelden bij Workteam als een beveiligingsbeheerder.

9. Klik in de rechterbovenhoek op **logo profiel** en klik vervolgens op **organisatie-instellingen**. 

    ![Instellingen voor Workteam](./media/workteam-tutorial/tutorial_workteam_settings.png)

10. Onder **verificatie** sectie, klikt u op **instellingen logo**.

     ![Workteam azure](./media/workteam-tutorial/tutorial_workteam_azure.png)

11. Op de **SAML-instellingen** pagina, voert u de volgende stappen uit:

     ![Workteam saml](./media/workteam-tutorial/tutorial_workteam_saml.png)

    a. Selecteer **SAML IdP** als **AD Azure**.

    b. In de **Single Sign-On Service URL voor SAML** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    c. In de **SAML entiteit-ID** tekstvak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal.

    d. Open in Kladblok het **base-64 gecodeerde certificaat** die u hebt gedownload van de Azure-portal, Kopieer de inhoud en plak deze in de **SAML-handtekeningcertificaat (Base64)** vak.

    e. Klik op **OK**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/workteam-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/workteam-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/workteam-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/workteam-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-workteam-test-user"></a>Maak een testgebruiker Workteam

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij Workteam, moeten ze worden ingericht voor Workteam. In Workteam is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u als een beveiligingsbeheerder Workteam bij.

2. In het midden boven in de **organisatie-instellingen** pagina, klikt u op **gebruikers** en klik vervolgens op **nieuwe gebruiker**.

    ![Workteam gebruiker](./media/workteam-tutorial/tutorial_workteam_user.png)

3. Op de **nieuwe werknemer** pagina, voert u de volgende stappen uit:

    ![Workteam nieuwegebruiker](./media/workteam-tutorial/tutorial_workteam_newuser.png)

    a. In de **naam** tekst voert u de voornaam van de gebruiker, zoals **Brittasimon**.

    b. In **e** tekst vak, voer het e-mailadres van gebruiker, zoals **Brittasimon\@contoso.com**.

    c. Klik op **OK**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Workteam.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Workteam toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Workteam**.

    ![De koppeling Workteam in de lijst met toepassingen](./media/workteam-tutorial/tutorial_workteam_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Workteam in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Workteam.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/workteam-tutorial/tutorial_general_01.png
[2]: ./media/workteam-tutorial/tutorial_general_02.png
[3]: ./media/workteam-tutorial/tutorial_general_03.png
[4]: ./media/workteam-tutorial/tutorial_general_04.png

[100]: ./media/workteam-tutorial/tutorial_general_100.png

[200]: ./media/workteam-tutorial/tutorial_general_200.png
[201]: ./media/workteam-tutorial/tutorial_general_201.png
[202]: ./media/workteam-tutorial/tutorial_general_202.png
[203]: ./media/workteam-tutorial/tutorial_general_203.png

