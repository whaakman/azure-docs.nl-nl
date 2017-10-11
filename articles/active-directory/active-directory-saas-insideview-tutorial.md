---
title: 'Zelfstudie: Azure Active Directory-integratie met InsideView | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en InsideView.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: f2b0a1d4bc44f8d0cd57c61e2d78950cb6a99854
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a>Zelfstudie: Azure Active Directory-integratie met InsideView

In deze zelfstudie leert u hoe InsideView integreren met Azure Active Directory (Azure AD).

InsideView integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot InsideView heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij InsideView (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met InsideView, moet u de volgende items:

- Een Azure AD-abonnement
- Een InsideView eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. InsideView uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-insideview-from-the-gallery"></a>InsideView uit de galerie toevoegen
Voor het configureren van de integratie van InsideView in naar Azure AD, moet u InsideView uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen InsideView uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **InsideView**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_search.png)

5. Selecteer in het deelvenster resultaten **InsideView**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met InsideView op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in InsideView is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in InsideView tot stand worden gebracht.

Wijs in InsideView, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met InsideView, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker InsideView](#creating-a-insideview-test-user)**  - InsideView die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing InsideView configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met InsideView, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **InsideView** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_samlbase.png)

3. Op de **InsideView domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_url.png)
    
    In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://my.insideview.com/iv/<STS Name>/login.iv`

    > [!NOTE] 
    > Deze waarde is geen echte. Deze waarde bijwerken met de werkelijke antwoord-URL. Neem contact op met [InsideView ondersteuningsteam ](mailto:support@insideview.com) deze waarde op te halen.
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Raw)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-insideview-tutorial/tutorial_general_400.png)

6. Op de **InsideView configuratie** sectie, klikt u op **configureren InsideView** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_configure.png) 

7. In een ander browservenster, meld u aan bij uw bedrijf InsideView site als beheerder.

8. Klik in de werkbalk bovenaan op **Admin**, **SingleSignOn instellingen**, en klik vervolgens op **SAML toevoegen**.
   
   ![SAML voor eenmalige aanmelding instellingen](./media/active-directory-saas-insideview-tutorial/ic794135.png "SAML voor eenmalige aanmelding instellingen")

9. In de **toevoegen van een nieuwe SAML** sectie, voert u de volgende stappen uit:

    ![Voeg een nieuwe SAML](./media/active-directory-saas-insideview-tutorial/ic794136.png "toevoegen van een nieuwe SAML")
   
    a. In de **STS Name** textbox, typ een naam voor uw configuratie.

    b. In **WS-SamlP-Fed ongevraagde eindpunt** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.
    
    c. Open uw base-64 gecodeerde certificaat dat u vanuit Azure-portal hebt gedownload, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **STS Certificate** textbox.

    d. In de **Crm-Id Gebruikerskoppeling** textbox type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
        
    e. In de **Crm e toewijzing** textbox type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    f. In de **Crm voornaam toewijzing** textbox type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
    
    g. In de **Crm lastName toewijzing** textbox type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.  

    h. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 
 
### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-insideview-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-insideview-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-insideview-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-insideview-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-insideview-test-user"></a>Een testgebruiker InsideView maken

Om Azure AD-gebruikers zich aanmelden bij InsideView, moeten ze worden ingericht op InsideView. In het geval van InsideView is inrichting een handmatige taak.

Als u gebruikers of contactpersonen in InsideView hebt gemaakt, neem contact op met [InsideView ondersteuningsteam](mailto:support@insideview.com).

>[!NOTE]
>U kunt andere InsideView gebruiker account hulpmiddelen voor het maken of API's die is geleverd door InsideView voor het inrichten van Azure AD-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan InsideView.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen InsideView, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **InsideView**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel InsideView in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing InsideView.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_203.png

