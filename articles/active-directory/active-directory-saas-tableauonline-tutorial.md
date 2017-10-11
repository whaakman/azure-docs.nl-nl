---
title: 'Zelfstudie: Azure Active Directory-integratie met Tableau Online | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding van Azure Active Directory en Tableau Online.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 443fab1198a91a4d5749e6421f7b8603fc75a81e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Zelfstudie: Azure Active Directory-integratie met Tableau Online

In deze zelfstudie leert u hoe Tableau Online integreren met Azure Active Directory (Azure AD).

Online Tableau integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Tableau Online heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Tableau Online (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Tableau Online, moet u de volgende items:

- Een Azure AD-abonnement
- Een Tableau Online eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van Online Tableau uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-tableau-online-from-the-gallery"></a>Toevoegen van Online Tableau uit de galerie
Voor het configureren van de integratie van Tableau Online in Azure AD, moet u Tableau Online uit de galerie toevoegt aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen in de galerie Tableau Online, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Tableau Online**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_search.png)

5. Selecteer in het deelvenster resultaten **Tableau Online**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Tableau Online op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Tableau Online is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker Tableau online worden gemaakt.

In Online Tableau, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Tableau Online, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een Online Tableau testgebruiker](#creating-a-tableau-online-test-user)**  - bevatten een equivalent van Britta Simon Tableau Online die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Tableau Online configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Tableau Online, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Tableau Online** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_samlbase.png)

3. Op de **Tableau Online domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_url.png)
    
    a. In de **aanmeldings-URL** textbox, typ de URL:`https://sso.online.tableau.com`

    b. In de **id** textbox, typ de URL:`https://sso.online.tableau.com/public/sp/<instancename>`

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_general_400.png)

6. In een ander browservenster aanmelding bij uw Tableau Online-toepassing. Ga naar **instellingen** en vervolgens **verificatie**.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)
    
7. Om in te schakelen SAML, onder **verificatietypen** sectie. Controleer de **eenmalige aanmelding met SAML** selectievakje.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

8. Schuif omlaag totdat **metagegevensbestand importeren in Tableau Online** sectie.  Klik op Bladeren en importeer het metagegevensbestand die u hebt gedownload van Azure AD. Klik vervolgens op **toepassen**.
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

9. In de **overeen met asserties** sectie, voert u de bijbehorende naam van de verklaring id-Provider voor **e-mailadres**, **voornaam**, en **achternaam**. Deze gegevens ophalen uit Azure AD: 
  
    a. Ga in de Azure-portal op de **Tableau Online** pagina van de integratie van toepassingen.
    
    b. Selecteer in de kenmerkensectie de **'weergeven en bewerken van alle andere gebruikerskenmerken '** selectievakje. 
    
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/attributesection.png)
      
    c. Kopieer de naamruimtewaarde voor deze kenmerken: givenname, e-mail- en achternaam met behulp van de volgende stappen uit:

   ![Azure AD voor eenmalige aanmelding](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)
    
    d. Klik op **user.givenname** waarde 
    
    e. Kopieer de waarde van de **naamruimte** textbox.

   ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/attributesection2.png)

    f. Als u wilt kopiëren van de namesapce stappen waarden voor de e-mail- en achternaam de voorgaande.

    g. Overschakelen naar de Tableau on line toepassing en stel vervolgens de **Tableau Online kenmerken** sectie als volgt:
     * E-mailadres: **mail** of **userprincipalname**
     * Voornaam: **givenname**
     * Achternaam: **achternaam**
   
   ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-tableau-online-test-user"></a>Maken van een Online Tableau testgebruiker

In deze sectie maakt maken u een gebruiker die is aangeroepen terwijl Britta Simon Tableau Online.

1. Op **Tableau Online**, klikt u op **instellingen** en vervolgens **verificatie** sectie. Schuif omlaag naar **gebruikers selecteren** sectie. Klik op **gebruikers toevoegen** en vervolgens **Voer e-mailadressen**.
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. Selecteer **toevoegen van gebruikers voor eenmalige aanmelding (SSO) verificatie**. In de **e-mailadressen opgeven** textbox toevoegenbritta.simon@contoso.com
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)
3. Klik op **Create**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding toegang verleent tot Tableau Online.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Tableau Online, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Tableau Online**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD SSO-configuratie met behulp van het toegangsvenster.

Als u op de tegel Tableau Online in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw Tableau Online-toepassing.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png

