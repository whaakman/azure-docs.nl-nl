---
title: 'Zelfstudie: Azure Active Directory-integratie met Picturepark | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Picturepark.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: jeedes
ms.openlocfilehash: 1c009aa1fdd3140a4466cf762b6c9687e74ce4c7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Zelfstudie: Azure Active Directory-integratie met Picturepark

In deze zelfstudie leert u hoe Picturepark integreren met Azure Active Directory (Azure AD).

Picturepark integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Picturepark heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Picturepark (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Picturepark, moet u de volgende items:

- Een Azure AD-abonnement
- Een Picturepark eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Picturepark uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-picturepark-from-the-gallery"></a>Picturepark uit de galerie toevoegen
Voor het configureren van de integratie van Picturepark in Azure AD, moet u Picturepark uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Picturepark uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Picturepark**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_search.png)

5. Selecteer in het deelvenster resultaten **Picturepark**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met Picturepark op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Picturepark is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Picturepark tot stand worden gebracht.

Wijs in Picturepark, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Picturepark, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Picturepark](#creating-a-picturepark-test-user)**  - Picturepark die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Picturepark configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Picturepark, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Picturepark** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_samlbase.png)

3. Op de **Picturepark domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.picturepark.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: 
    
    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [Picturepark Client ondersteuningsteam](https://picturepark.com/about/contact/) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, Kopieer de **VINGERAFDRUK** waarde van het certificaat.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-picturepark-tutorial/tutorial_general_400.png)

6. Op de **Picturepark configuratie** sectie, klikt u op **configureren Picturepark** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_configure.png) 

7. In een ander browservenster, meld u bij uw bedrijf Picturepark site als beheerder.

8. Klik in de werkbalk bovenaan op **Systeembeheer**, en klik vervolgens op **beheerconsole**.
   
    ![Beheerconsole](./media/active-directory-saas-picturepark-tutorial/ic795062.png "-beheerconsole")

9. Klik op **verificatie**, en klik vervolgens op **identiteitsproviders**.
   
    ![Verificatie](./media/active-directory-saas-picturepark-tutorial/ic795063.png "verificatie")

10. In de **identiteit providerconfiguratie** sectie, voert u de volgende stappen uit:
   
    ![Configuratie van de provider identiteit](./media/active-directory-saas-picturepark-tutorial/ic795064.png "providerconfiguratie identiteit")
   
    a. Klik op **Add**.
  
    b. Typ een naam voor uw configuratie.
   
    c. Selecteer **ingesteld als standaard**.
   
    d. In **verlener URI** textbox, plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.
   
    e. In **vertrouwde verlener miniatuur afdrukken** textbox, plak de waarde van **vingerafdruk** die u hebt gekopieerd uit **certificaat voor ondertekening van SAML** sectie. 

11. Klik op **JoinDefaultUsersGroup**.

12. Om in te stellen de **Emailaddress** kenmerk in de **Claim** textbox type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` en klik op **opslaan**.

      ![Configuratie](./media/active-directory-saas-picturepark-tutorial/ic795065.png "configuratie")

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-picturepark-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-picturepark-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-picturepark-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-picturepark-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-picturepark-test-user"></a>Een testgebruiker Picturepark maken

Om in te schakelen gebruikers van Azure AD aan te melden bij Picturepark, moeten ze worden ingericht in Picturepark. In het geval van Picturepark is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Picturepark** tenant.

2. Klik in de werkbalk bovenaan op **Systeembeheer**, en klik vervolgens op **gebruikers**.
   
    ![Gebruikers](./media/active-directory-saas-picturepark-tutorial/ic795067.png "gebruikers")

3. In de **overzicht van gebruikers** tabblad **nieuw**.
   
    ![Gebruikersbeheer](./media/active-directory-saas-picturepark-tutorial/ic795068.png "Gebruikersbeheer")

4. Op de **gebruiker maken** dialoogvenster de volgende stappen uit een geldige Azure Active Directory-gebruiker u wilt inrichten:
   
    ![Gebruiker maken](./media/active-directory-saas-picturepark-tutorial/ic795069.png "gebruiker maken")
   
    a. In de **e-mailadres** textbox type de **e-mailadres** van de gebruiker  **BrittaSimon@contoso.com** .  
   
    b. In de **wachtwoord** en **wachtwoord bevestigen** tekstvakken, typ de **wachtwoord** van BrittaSimon. 
   
    c. In de **voornaam** textbox type de **voornaam** van de gebruiker **Britta**. 
   
    d. In de **achternaam** textbox type de **achternaam** van de gebruiker **Simon**.
   
    e. In de **bedrijf** textbox type de **bedrijfsnaam** van de gebruiker. 
   
    f. In de **land** textbox, selecteer de **land** van de gebruiker.
  
    g. In de **ZIP** textbox type de **postcode** van de plaats.
   
    h. In de **stad** textbox type de **plaatsnaam** van de gebruiker.

    ik. Selecteer een **taal**.
   
    j. Klik op **Create**.

>[!NOTE]
>U kunt andere Picturepark gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Picturepark voor het inrichten van Azure AD-gebruikersaccounts.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Picturepark.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Picturepark, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Picturepark**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Picturepark in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Picturepark. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_203.png

