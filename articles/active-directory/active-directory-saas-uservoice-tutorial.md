---
title: 'Zelfstudie: Azure Active Directory-integratie met UserVoice | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en UserVoice.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: fcfda1c2ecb162fb93b70574a18bd745b72ee4db
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Zelfstudie: Azure Active Directory-integratie met UserVoice

In deze zelfstudie leert u hoe UserVoice integreren met Azure Active Directory (Azure AD).

UserVoice integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot UserVoice heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij UserVoice (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met UserVoice, moet u de volgende items:

- Een Azure AD-abonnement
- Een UserVoice eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. UserVoice uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-uservoice-from-the-gallery"></a>UserVoice uit de galerie toevoegen
Voor het configureren van de integratie van UserVoice in Azure AD, moet u UserVoice uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen UserVoice uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **UserVoice**, selecteer **UserVoice** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![UserVoice in de lijst met resultaten](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met UserVoice op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in UserVoice is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de verwante UserVoice-gebruiker worden gemaakt.

Wijs in UserVoice, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met UserVoice, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker UserVoice](#create-a-uservoice-test-user)**  - UserVoice die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw UserVoice-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met UserVoice, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **UserVoice** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_samlbase.png)

3. Op de **UserVoice domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's voor UserVoice-domein en één aanmelding informatie](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<tenantname>.UserVoice.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<tenantname>.UserVoice.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [UserVoice Client ondersteuningsteam](https://www.uservoice.com/) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, Kopieer de **VINGERAFDRUK** waarde van het certificaat.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-uservoice-tutorial/tutorial_general_400.png)

6. Op de **UserVoice configuratie** sectie, klikt u op **configureren UserVoice** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out URL's, en SAML Single Sign-On Service** van de **Naslaggids punt.**

    ![UserVoice-configuratie](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_configure.png) 

7. In een ander browservenster, meld u aan bij uw bedrijf UserVoice site als beheerder.

8. Klik in de werkbalk bovenaan op **instellingen**, en selecteer vervolgens **webportal** in het menu.
   
    ![Sectie met App-zijde](./media/active-directory-saas-uservoice-tutorial/ic777519.png "instellingen")

9. Op de **webportal** tabblad, in de **gebruikersverificatie** sectie, klikt u op **bewerken** openen de **gebruikersverificatie bewerken** dialoogvenster pagina.
   
    ![Web-portal tabblad](./media/active-directory-saas-uservoice-tutorial/ic777520.png "Web-portal")

10. Op de **gebruikersverificatie bewerken** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Verificatie van gebruikers bewerken](./media/active-directory-saas-uservoice-tutorial/ic777521.png "gebruikersverificatie bewerken")
   
    a. Klik op **eenmalige aanmelding (SSO)**.
 
    b. Plakken de **SAML Single Sign-On Service-URL** waarde, die u hebt gekopieerd vanuit de Azure-portal in de **extern aanmelden SSO** textbox.

    c. Plak de **Sign-Out URL** waarde, die u hebt gekopieerd vanuit de Azure-portal in de **SSO externe Sign-Out textbox**.
 
    d. Plak de **vingerafdruk** waarde, die u hebt gekopieerd vanuit Azure-portal in de **huidige vingerafdruk van certificaat SHA1** textbox.
    
    e. Klik op **verificatie-instellingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-uservoice-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-uservoice-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-uservoice-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-uservoice-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-uservoice-test-user"></a>Maak een testgebruiker UserVoice

Om Azure AD-gebruikers zich aanmelden bij UserVoice, moeten ze worden ingericht in UserVoice. In het geval van UserVoice is inrichting een handmatige taak.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:
1. Meld u aan bij uw **UserVoice** tenant.

2. Ga naar **instellingen**.
   
    ![Instellingen](./media/active-directory-saas-uservoice-tutorial/ic777811.png "instellingen")

3. Klik op **algemene**.

4. Klik op **Agents en machtigingen**.
   
    ![Agents en machtigingen](./media/active-directory-saas-uservoice-tutorial/ic777812.png "Agents en machtigingen")

5. Klik op **toevoegen admins**.
   
    ![Toevoegen van beheerders](./media/active-directory-saas-uservoice-tutorial/ic777813.png "admins toevoegen")

6. Op de **uitnodigen admins** dialoogvenster de volgende stappen uitvoeren:
   
    ![Beheerders uitnodigen](./media/active-directory-saas-uservoice-tutorial/ic777814.png "admins uitnodigen")
   
    a. Typ in het tekstvak voor de e-mailberichten, het e-mailadres van het account dat u wilt inrichten, en klik vervolgens op **toevoegen**.
   
    b. Klik op **uitnodigen**.

> [!NOTE]
> U kunt andere UserVoice gebruiker account hulpmiddelen voor het maken of API's die is geleverd door UserVoice aan inrichten AAD-gebruikersaccounts.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan UserVoice.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen UserVoice, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **UserVoice**.

    ![De UserVoice-koppeling in de lijst met toepassingen](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel UserVoice in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw UserVoice-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_203.png

