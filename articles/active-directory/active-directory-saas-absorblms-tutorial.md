---
title: 'Zelfstudie: Azure Active Directory-integratie met kunnen LMS | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en LMS opvangen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jeedes
ms.openlocfilehash: 4231b5e72ea0852de344e7484bdf20997243cee6
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Zelfstudie: Azure Active Directory-integratie met LMS opnemen

In deze zelfstudie leert u hoe LMS kunnen integreren met Azure Active Directory (Azure AD).

LMS kunnen integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot LMS opvangen heeft.
- U kunt uw gebruikers automatisch naar LMS (via eenmalige aanmelding) kunnen zich met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie, de Azure-portal beheren.

Als u meer weten over software als een dienst (SaaS)-app-integratie met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie met LMS kunnen configureren, moet u de volgende items:

- Een Azure AD-abonnement
- Een LMS kunnen eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> U wordt aangeraden een productie-omgeving voor deze zelfstudie niet gebruiken.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

* LMS opnemen uit de galerie toevoegen
* Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-absorb-lms-from-the-gallery"></a>LMS opnemen uit de galerie toevoegen
Voor het configureren van de integratie van LMS opnemen in Azure AD LMS kunnen uit de galerie te toevoegt aan de lijst met beheerde SaaS-apps.

Als u wilt toevoegen LMS opnemen uit de galerie, het volgende doen:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Ga naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![Het deelvenster Enterprise-toepassingen][2]
    
3. Als u wilt een toepassing toevoegen, selecteert u de **nieuwe toepassing** knop.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **kunnen LMS**, selecteer **LMS kunnen** in Configuratiescherm leiden, en selecteer vervolgens de **toevoegen** knop.

    ![LMS opnemen in de lijst met resultaten](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met kunnen LMS op basis van een testgebruiker Britta Simon aangeroepen.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker van de bijbehorende equivalent kunnen LMS is in Azure AD. Met andere woorden, moet u een koppeling relatie tussen een gebruiker in Azure AD en de bijbehorende gebruiker in LMS kunnen maken.

U de relatie van deze koppeling tot stand brengen door toe te wijzen de *gebruikersnaam* waarde in Azure AD als de *gebruikersnaam* waarde in LMS opvangen.

Configureren en testen van Azure AD eenmalige aanmelding met LMS vangen, de bouwstenen in de volgende vijf secties voltooien.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing LMS kunnen configureren.

Voor het configureren van Azure AD eenmalige aanmelding met LMS vangen, het volgende doen:

1. In de Azure-portal op de **kunnen LMS** toepassing Integratiepagina **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. In de **eenmalige aanmelding** het dialoogvenster de **modus** de optie **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. In de **kunnen LMS domein en URL's** sectie, het volgende doen:

    ![LMS domein en de URL's eenmalige aanmelding informatie opnemen](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_url.png)

    a. In de **id** typt u een URL die u gebruikt de volgende syntaxis: `https://<subdomain>.myabsorb.com/Account/SAML`.

    b. In de **antwoord-URL** typt u een URL die u gebruikt de volgende syntaxis: `https://<subdomain>.myabsorb.com/Account/SAML`.
     
    > [!NOTE] 
    > Deze URL's zijn niet de werkelijke waarden. Deze bijwerken met de werkelijke id en de antwoord-URL's. Deze waarden, neem contact op met de [LMS kunnen client-ondersteuningsteam](https://www.absorblms.com/support). 

4. In de **SAML-certificaat voor ondertekening van** sectie in het **downloaden** kolom, selecteer **Metadata XML**, en sla het bestand met metagegevens op uw computer.

    ![De koppeling ondertekenen certificaat downloaden](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_certificate.png) 

5. Selecteer **Opslaan**.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-absorblms-tutorial/tutorial_general_400.png)
    
6. In de **LMS configuratie kunnen** sectie **kunnen LMS configureren** openen **eenmalige aanmelding configureren** venster en kopieer de **Sign-Out URL** in de **Naslaggids punt.**

    ![Het deelvenster kunnen LMS configuratie](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_configure.png) 

7. In een nieuw browservenster aanmelden bij uw bedrijf kunnen LMS site als beheerder.

8. Selecteer de **Account** knop in de rechterbovenhoek. 

    ![De knop Account](./media/active-directory-saas-absorblms-tutorial/1.png)

9. Selecteer in het deelvenster Account **instellingen voor de Gebruikersportal**.

    ![De instellingen voor Portal-koppeling](./media/active-directory-saas-absorblms-tutorial/2.png)
    
10. Selecteer de tab **Gebruikers**.

    ![Het tabblad gebruikers](./media/active-directory-saas-absorblms-tutorial/3.png)

11. Ga als volgt te werk op de configuratiepagina voor eenmalige aanmelding:

    ![De configuratiepagina voor eenmalige aanmelding](./media/active-directory-saas-absorblms-tutorial/4.png)

    a. In de **modus** de optie **identiteit Provider geïnitieerd**.

    b. Open in Kladblok het certificaat dat u hebt gedownload van de Azure-portal. Verwijder de **---BEGIN CERTIFICATE---** en **---EINDCERTIFICAAT---** labels. Klik in de **sleutel** vak, plak de resterende inhoud.
    
    c. In de **Id-eigenschap** Selecteer het kenmerk dat u hebt geconfigureerd als de gebruikers-id in Azure AD. Bijvoorbeeld, als *userPrincipalName* is geselecteerd in Azure AD, selecteer **gebruikersnaam**.

    d. In de **aanmeldings-URL** vak, plak de **toegangs-URL van gebruiker** van de toepassing **eigenschappen** pagina van de Azure-portal.

    e. In de **afmelding URL**, plak de **Sign-Out URL** waarde die u hebt gekopieerd uit de **eenmalige aanmelding configureren** venster van de Azure portal.

12. Wisselknop **alleen toestaan SSO aanmelding** naar **op**.

    ![De wisselknop alleen toestaan SSO-aanmelding](./media/active-directory-saas-absorblms-tutorial/5.png)

13. Selecteer **opslaan.**

> [!TIP]
> Vindt u een beknopte versie van deze instructies in de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app. Na het toevoegen van de app vanuit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. Zie voor meer informatie [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u testgebruiker Britta Simon in de Azure portal.

![Een Azure AD-testgebruiker maken][100]

Als u wilt een testgebruiker maken in Azure AD, het volgende doen:

1. Selecteer in de Azure-portal in het linkerdeelvenster **Azure Active Directory**.

    ![De Azure Active Directory-knop](./media/active-directory-saas-absorblms-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, selecteer **gebruikers en groepen** > **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-absorblms-tutorial/create_aaduser_02.png) 

3. Selecteer aan de bovenkant van het dialoogvenster **toevoegen**.
 
    ![De knop toevoegen](./media/active-directory-saas-absorblms-tutorial/create_aaduser_03.png) 

4. In de **gebruiker** dialoogvenster de volgende handelingen uit:
 
    ![Het dialoogvenster gebruiker](./media/active-directory-saas-absorblms-tutorial/create_aaduser_04.png) 

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** textbox, typt u het e-mailadres Britta Simon.

    c. Selecteer de **wachtwoord weergeven** uit en noteer de waarde in de **wachtwoord** vak.

    d. Selecteer **Maken**.

### <a name="create-an-absorb-lms-test-user"></a>Een testgebruiker LMS kunnen maken

Voor Azure AD-gebruikers aan te melden bij LMS opnemen, moeten ze worden ingesteld in LMS opvangen.  

Setup is voor LMS kunnen een handmatige taak.

Als u een gebruikersaccount instelt, het volgende doen:

1. Meld u aan bij uw bedrijf kunnen LMS site als beheerder.

2. Selecteer in het linkerdeelvenster **gebruikers**.

    ![De koppeling kunnen LMS gebruikers](./media/active-directory-saas-absorblms-tutorial/absorblms_users.png)

3. In de **gebruikers** deelvenster **gebruikers**.

    ![De koppeling gebruikers](./media/active-directory-saas-absorblms-tutorial/absorblms_userssub.png)

4. In de **nieuwe toevoegen** vervolgkeuzelijst, selecteer **gebruiker**.

    ![De nieuwe toevoegen vervolgkeuzelijst](./media/active-directory-saas-absorblms-tutorial/absorblms_createuser.png)

5. Op de **gebruiker toevoegen** pagina, de volgende handelingen uit:

    ![De pagina gebruiker toevoegen](./media/active-directory-saas-absorblms-tutorial/user.png)

    a. In de **voornaam** , zoals de voornaam, typ **Britta**.

    b. In de **achternaam** typt u de achternaam, zoals **Simon**.
    
    c. In de **gebruikersnaam** typt u een volledige naam, zoals **Britta Simon**.

    d. In de **wachtwoord** vak Britta Simon van wachtwoord.

    e. In de **wachtwoord bevestigen** vak, typ nogmaals het wachtwoord.
    
    f. Stel de **Is actief** in-of uitschakelen op **Active**.  

6. Selecteer **opslaan.**
 
### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u gebruiker Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan LMS kunnen.

![Toewijzen van de gebruikersrol][200]

Gebruiker Britta Simon om aan te wijzen LMS vangen, het volgende doen:

1. In de Azure portal, opent u de weergave van toepassingen, Ga naar de directoryweergave en selecteer vervolgens **bedrijfstoepassingen** > **alle toepassingen**.

    ![De koppeling 'Alle toepassingen'][201] 

2. In de **toepassingen** selecteert **kunnen LMS**.

    ![De koppeling LMS opnemen in de lijst met toepassingen](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_app.png) 

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

4. Selecteer **toevoegen** en klikt u op de **toevoegen toewijzing** deelvenster **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** het dialoogvenster de **gebruikers** selecteert **Britta Simon**.

6. In de **gebruikers en groepen** selecteert u de **Selecteer** knop.

7. In de **toevoegen toewijzing** selecteert u de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

In het deelvenster toegang selecteren de **kunnen LMS** tegel automatisch aangemeld bij uw toepassing LMS opvangen. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_203.png

