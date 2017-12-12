---
title: 'Zelfstudie: Azure Active Directory-integratie met Zoho | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zoho.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: 51a10a4d687a0d218b5439ebf5aeb1d0895c6161
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zoho"></a>Zelfstudie: Azure Active Directory-integratie met Zoho

In deze zelfstudie leert u hoe Zoho integreren met Azure Active Directory (Azure AD).

Zoho integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Zoho heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Zoho (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zoho, moet u de volgende items:

- Een Azure AD-abonnement
- Een Zoho eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Zoho uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zoho-from-the-gallery"></a>Zoho uit de galerie toevoegen
Voor het configureren van de integratie van Zoho in Azure AD, moet u Zoho uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Zoho uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Zoho**, selecteer **Zoho** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Zoho in de lijst met resultaten](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Zoho op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Zoho is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Zoho tot stand worden gebracht.

Wijs in Zoho, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Zoho, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Zoho](#create-a-zoho-test-user)**  - Zoho die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Zoho configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Zoho, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Zoho** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_samlbase.png)

3. Op de **Zoho domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en Zoho domein eenmalige aanmelding informatie](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<company name>.zohomail.com`

    > [!NOTE] 
    > Deze waarde is geen echte. Deze waarde bijwerken met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [Zoho Client ondersteuningsteam](https://www.zoho.com/mail/contact.html) deze waarde op te halen. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_400.png)

6. Op de **Zoho configuratie** sectie, klikt u op **configureren Zoho** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, de URL wachtwoord wijzigen en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Zoho configuratie](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_configure.png) 

7. In een ander browservenster, meld u bij uw bedrijf Zoho Mail site als beheerder.

8. Ga naar de **Configuratiescherm**.
   
    ![Het Configuratiescherm](./media/active-directory-saas-zoho-mail-tutorial/ic789607.png "Configuratiescherm")

9. Klik op de **SAML-verificatie** tabblad.
   
    ![SAML-verificatie](./media/active-directory-saas-zoho-mail-tutorial/ic789608.png "SAML-verificatie")

10. In de **SAML verificatiegegevens** sectie, voert u de volgende stappen uit:
   
    ![Details van SAML-verificatie](./media/active-directory-saas-zoho-mail-tutorial/ic789609.png "Details van SAML-verificatie")
   
    a. In de **aanmeldings-URL** textbox plakken **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.
   
    b. In de **afmelding URL** textbox plakken **Sign-Out URL** die u hebt gekopieerd vanuit Azure-portal.
   
    c. In de **URL van wijzigen wachtwoord** textbox plakken **URL van wijzigen wachtwoord** die u hebt gekopieerd vanuit Azure-portal.
       
    d. Open uw base-64 gecodeerde certificaat gedownload vanuit Azure-portal in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **PublicKey** textbox.
   
    e. Als **algoritme**, selecteer **RSA**.
   
    f. Klik op **OK**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-zoho-test-user"></a>Een testgebruiker Zoho maken

Om in te schakelen gebruikers van Azure AD aan te melden bij Zoho Mail, moeten ze worden ingericht in Zoho Mail. In het geval van Zoho Mail is inrichting een handmatige taak.

> [!NOTE]
> U kunt andere Zoho afdruk gebruiker account hulpmiddelen voor het maken of API's geleverd door Zoho E-mail aan inrichten AAD-gebruikersaccounts.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:

1. Meld u aan bij uw **Zoho Mail** bedrijf site als beheerder.

2. Ga naar **Configuratiescherm \> e & Docs**.

3. Ga naar **Gebruikersdetails \> gebruiker toevoegen**.
   
    ![Gebruiker toevoegen](./media/active-directory-saas-zoho-mail-tutorial/ic789611.png "gebruiker toevoegen")

4. Op de **gebruikers toevoegen** dialoogvenster de volgende stappen uitvoeren:
   
    ![Gebruiker toevoegen](./media/active-directory-saas-zoho-mail-tutorial/ic789612.png "gebruiker toevoegen")
   
    a. In de **voornaam** textbox type de voornaam van de gebruiker, zoals **Britta**.

    b. In de **achternaam** textbox type de naam van de laatste gebruiker, zoals **Simon**.

    c. In de **E-mail-ID** textbox type de e-mail-id van gebruiker, zoals  **brittasimon@contoso.com** .

    d. In de **wachtwoord** textbox, voer het wachtwoord van gebruiker.
   
    e. Klik op **OK**.  
      
    > [!NOTE]
    > De houder van Azure Active Directory-account ontvangt een e-mailbericht met een koppeling naar het account te bevestigen voordat deze geactiveerd wordt.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Zoho.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Zoho, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Zoho**.

    ![De koppeling Zoho in de lijst met toepassingen](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Zoho in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Zoho.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_203.png

