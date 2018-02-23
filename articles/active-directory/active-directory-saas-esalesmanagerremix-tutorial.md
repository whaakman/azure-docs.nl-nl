---
title: 'Zelfstudie: Azure Active Directory-integratie met E Sales Manager Remix | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en E Sales Manager Remix.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 89b5022c-0d5b-4103-9877-ddd32b6e1c02
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeedes
ms.openlocfilehash: 56c2860b6aeac9fdc66f2b1fdd1ed9126bf77d3f
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2018
---
# <a name="tutorial-azure-active-directory-integration-with-e-sales-manager-remix"></a>Zelfstudie: Azure Active Directory-integratie met E Sales Manager Remix

In deze zelfstudie leert u hoe E Sales Manager Remix integreren met Azure Active Directory (Azure AD).

E Sales Manager Remix integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot E Sales Manager Remix heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij E Sales Manager Remix (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD om integratie te configureren met E Sales Manager Remix, moet u de volgende items:

- Een Azure AD-abonnement
- Een E-Sales Manager Remix eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen E Sales Manager Remix uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-e-sales-manager-remix-from-the-gallery"></a>Toe te voegen E Sales Manager Remix uit de galerie
Voor het configureren van de integratie van IE Sales Manager Remix in Azure AD, moet u E Sales Manager Remix toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen E Sales Manager Remix uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **E Sales Manager Remix**, selecteer **E Sales Manager Remix** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![E Sales Manager Remix in de lijst met resultaten](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met E Sales Manager Remix op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in IE Sales Manager Remix is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in IE Sales Manager Remix tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met E Sales Manager Remix, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker E Sales Manager Remix](#create-an-e-sales-manager-remix-test-user)**  - met een exemplaar van Britta Simon E Sales Manager Remix die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing E Sales Manager Remix configureren.

**Eenmalige aanmelding Azure AD configureren met E Sales Manager Remix, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **E Sales Manager Remix** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_samlbase.png)

3. Op de **E Sales Manager Remix domein en de URL's** sectie, voert u de volgende stappen uit:

    ![E Sales Manager Remix domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<Server-Based-URL>/<sub-domain>/esales-pc`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://<Server-Based-URL>/<sub-domain>/`

    c. Kopieer de **id** waarde in Kladblok. Verderop in deze zelfstudie wordt u id-waarde.
    
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [E Sales Manager Remix Client ondersteuningsteam](mailto:esupport@softbrain.co.jp) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_certificate.png) 

5. Selecteer **weergeven en bewerken van alle andere gebruikerskenmerken** en klik op **emailaddress** kenmerk.
    
    ![E Verkoopmanager Remix configuratie](./media/active-directory-saas-esalesmanagerremix-tutorial/configure1.png)

6. Kopieer de **Namespace** en **naam** waarde van de claim van het tekstvak. De waarde in het volgende patroon - genereren `<Namespace>/<Name>`. U gebruikt deze waarde verderop in deze zelfstudie.

    ![E Verkoopmanager Remix configuratie](./media/active-directory-saas-esalesmanagerremix-tutorial/configure2.png)

7. Op de **E Sales Manager Remix configuratie** sectie, klikt u op **configureren IE Sales Manager Remix** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out URL's en SAML Single Sign-On Service** van de **Naslaggids punt.**

    ![E Verkoopmanager Remix configuratie](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_configure.png) 

8. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_400.png)

9. Meld u aan op uw toepassing E Sales Manager Remix als beheerder.

10. Selecteer **Menu naar beheerder** in het menu rechtsboven.

    ![E Verkoopmanager Remix configuratie](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

11. Selecteer **systeeminstellingen**>**samenwerking met extern systeem**

    ![E Verkoopmanager Remix configuratie](./media/active-directory-saas-esalesmanagerremix-tutorial/configure5.png)
    
12. Selecteer **SAML**

    ![E Verkoopmanager Remix configuratie](./media/active-directory-saas-esalesmanagerremix-tutorial/configure6.png)

13. In de **SAML-verificatie-instelling** sectie, voert u de volgende stappen uit:

    ![E Verkoopmanager Remix configuratie](./media/active-directory-saas-esalesmanagerremix-tutorial/configure3.png)
    
    a. Selecteer **PC-versie**
    
    b. Selecteer **e** item uit de vervolgkeuzelijst in de samenwerking sectie.

    c. Plak in het tekstvak voor de samenwerking item, de **claimwaarde** die u hebt gekopieerd uit de Azure portal eenledige `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. In de **verlener (entiteit-ID)** textbox, plak de **id** waarde die u hebt gekopieerd vanuit de Azure-portal van **E Sales Manager Remix domein en de URL's** sectie.

    e. Voor het uploaden van uw gedownloade **certificaat** Selecteer in de Azure-portal **selectie bestand**.

    f. In **ID-provider aanmeldings-URL** textbox plakken **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.

    g. In **identiteit Provider afmelding URL** textbox plakken **Sign-Out URL** waarde die u hebt gekopieerd vanuit Azure-portal.

    h. Klik op **voltooid instellen**

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-esalesmanagerremix-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-e-sales-manager-remix-test-user"></a>Een testgebruiker E Sales Manager Remix maken

1. Meld u aan op uw toepassing E Sales Manager Remix als beheerder.

2. Selecteer **Menu naar beheerder** in het menu rechtsboven.

    ![E Verkoopmanager Remix configuratie](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

3. Selecteer **uw bedrijfsinstellingen**>**onderhoud van de afdelingen en werknemers** en selecteer **werknemers geregistreerd**.

    ![De gebruiker](./media/active-directory-saas-esalesmanagerremix-tutorial/user1.png)

4. In de **nieuwe registratie van de werknemer** sectie, voert u de volgende stappen uit:
    
    ![De gebruiker](./media/active-directory-saas-esalesmanagerremix-tutorial/user2.png)

    a. In de **naam van de werknemer** textbox, typ de naam van gebruiker zoals Britta.

    b. Vul alle verplichte velden voor de respectieve met de gebruikersgegevens.
    
    c. Als u SAML inschakelt, de beheerder kan niet worden als u wilt zich aanmelden vanaf het aanmeldingsscherm, dus verlenen beheerder aanmeldingsbevoegdheden voor de gebruiker door te selecteren de **aanmeldgegevens van serverbeheerder**

    d. Klik op **registratie**

5. In de toekomst, als u zich aanmelden als beheerder wilt, meld u aan met de gebruiker die de beheerder is gemachtigd en op **Menu naar beheerder** in het menu rechtsboven.

    ![De gebruiker](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang aan E Sales Manager Remix.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen E Sales Manager Remix, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **E Sales Manager Remix**.

    ![De koppeling E Sales Manager Remix in de lijst met toepassingen](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel E Sales Manager Remix in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing E Sales Manager Remix.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_203.png

