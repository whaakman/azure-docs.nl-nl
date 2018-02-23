---
title: 'Zelfstudie: Azure Active Directory-integratie met beheerservices aanwezigheid | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en aanwezigheid Management Services.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1f56e612-728b-4203-a545-a81dc5efda00
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: jeedes
ms.openlocfilehash: 1fcbbabe80c3ff4b5a18904637cb227499da6829
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="tutorial-azure-active-directory-integration-with-attendance-management-services"></a>Zelfstudie: Azure Active Directory-integratie met beheerservices aanwezigheid

In deze zelfstudie leert u hoe aanwezigheid Management Services integreren met Azure Active Directory (Azure AD).

Aanwezigheid Management Services integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot aanwezigheid Management Services heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij aanwezigheid Management Services (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met aanwezigheid Management Services, moet u de volgende items:

- Een Azure AD-abonnement
- Een aanwezigheid beheerservices eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Aanwezigheid Management-Services uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-attendance-management-services-from-the-gallery"></a>Aanwezigheid Management-Services uit de galerie toevoegen
Voor het configureren van de integratie van aanwezigheid Management-Services in Azure AD, moet u aanwezigheid Management-Services uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen aanwezigheid Management-Services uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **aanwezigheid beheerservices**, selecteer **aanwezigheid Management Services** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Aanwezigheid Management-Services in de lijst met resultaten](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met aanwezigheid Management Services op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in aanwezigheid Management-Services in Azure AD voor een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in aanwezigheid beheerservices tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met aanwezigheid Management Services, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker aanwezigheid beheerservices](#create-an-attendance-management-service-test-user)**  - aanwezigheid beheerservices die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw aanwezigheid Management Services-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met aanwezigheid Management Services, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **aanwezigheid beheerservices** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_samlbase.png)

3. Op de **URL's en Services van aanwezigheid beheerdomein** sectie, voert u de volgende stappen uit:

    ![URL's en Services van aanwezigheid beheerdomein eenmalige aanmelding informatie](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://id.obc.jp/<tenant information >/`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://id.obc.jp/<tenant information >/`

    > [!NOTE]
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [aanwezigheid Management Services Client ondersteuningsteam](http://www.obcnet.jp/) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_400.png)

6. Op de **aanwezigheid Management Services Configuration** sectie, klikt u op **aanwezigheid Management-Services configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Configuratie van de aanwezigheid Management Services](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_configure.png) 

7. In een ander browservenster aanmelding bij uw bedrijf aanwezigheid beheerservices site als administrator.

8. Klik op **SAML-verificatie** onder de **management Beveiligingssectie**.

    ![Configuratie van de aanwezigheid Management Services](./media/active-directory-saas-attendancemanagementservices-tutorial/user1.png)

9. De volgende stappen uitvoeren:

    ![Configuratie van de aanwezigheid Management Services](./media/active-directory-saas-attendancemanagementservices-tutorial/user2.png)

    a. Selecteer **gebruik SAML-verificatie**.

    b. In de **id** textbox, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal. 

    c. In de **eindpunt-URL voor webverificatie** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.

    d. Klik op **selecteert u een bestand** voor het uploaden van het certificaat dat u hebt gedownload van Azure AD.

    e. Selecteer **uitschakelen wachtwoordverificatie**.

    f. Klik op **registratie**

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt! Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-attendancemanagementservices-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-attendancemanagementservices-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-attendancemanagementservices-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-attendancemanagementservices-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-attendance-management-services-test-user"></a>Een testgebruiker aanwezigheid Management Services maken

Om Azure AD-gebruikers zich aanmelden bij aanwezigheid Management Services, moeten ze worden ingericht in aanwezigheid Management Services. In het geval van aanwezigheid Management Services is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf aanwezigheid beheerservices site als beheerder.

2. Klik op **Gebruikersbeheer** onder de **management Beveiligingssectie**.

    ![Werknemer toevoegen](./media/active-directory-saas-attendancemanagementservices-tutorial/user5.png)

3. Klik op **nieuwe regels aanmelding**.

    ![Werknemer toevoegen](./media/active-directory-saas-attendancemanagementservices-tutorial/user3.png)

4. In de **OBCiD informatie** sectie, voert u de volgende stappen uit:

    ![Werknemer toevoegen](./media/active-directory-saas-attendancemanagementservices-tutorial/user4.png)

    a. In de **OBCiD** textbox, typ het e-mailadres van gebruiker, zoals  **BrittaSimon@contoso.com** .

    b. In de **wachtwoord** textbox, typt u het wachtwoord van gebruiker.

    c. Klik op **registratie**


### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan aanwezigheid Management Services.

![Toewijzen van de gebruikersrol][200] 

**Als u wilt toewijzen Britta Simon en aanwezigheid Management-Services, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **aanwezigheid beheerservices**.

    ![De koppeling aanwezigheid Management-Services in de lijst met toepassingen](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de aanwezigheid Management Services-tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw aanwezigheid Management Services-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_203.png

