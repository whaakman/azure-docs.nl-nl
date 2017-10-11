---
title: 'Zelfstudie: Azure Active Directory-integratie met adaptieve Suite | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en adaptieve Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 5d7ba2f4c7d814e3aaa1bf804ddc5030380ccb2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Zelfstudie: Azure Active Directory-integratie met adaptieve Suite

In deze zelfstudie leert u hoe adaptieve Suite integreren met Azure Active Directory (Azure AD).

Adaptieve Suite integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot adaptieve Suite heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij adaptieve Suite (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met adaptieve Suite, moet u de volgende items:

- Een Azure AD-abonnement
- Een adaptieve Suite eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Adaptieve Suite uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-adaptive-suite-from-the-gallery"></a>Adaptieve Suite uit de galerie toevoegen
Voor het configureren van de integratie van adaptieve Suite in Azure AD, moet u adaptieve Suite uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen adaptieve Suite uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **adaptieve Suite**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_search.png)

5. Selecteer in het deelvenster resultaten **adaptieve Suite**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met adaptieve Suite op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in adaptieve Suite is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in adaptieve Suite tot stand worden gebracht.

Wijs in adaptieve Suite de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met adaptieve Suite, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker adaptieve Suite](#creating-an-adaptive-suite-test-user)**  - adaptieve Suite die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing adaptieve Suite.

**Voor het configureren van eenmalige aanmelding Azure AD met adaptieve Suite, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **adaptieve Suite** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_samlbase.png)

3. Op de **adaptieve Suite domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    >[!NOTE]
    > U kunt deze waarde niet ophalen uit de adaptieve Suite **SAML SSO instellingen** pagina.
    >  

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_400.png)

6. Op de **geavanceerde configuratie van de Suite** sectie, klikt u op **adaptieve Suite configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_configure.png) 

7. In een ander browservenster, meld u aan bij uw bedrijf adaptieve Suite site als beheerder.

8. Ga naar **Admin**.
   
    ![Beheerder](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")

9. In de **gebruikers en rollen** sectie, klikt u op **SAML SSO-instellingen beheren**.
   
    ![SAML SSO-instellingen beheren](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "SAML SSO-instellingen beheren")

10. Op de **SAML SSO instellingen** pagina, voert u de volgende stappen uit:
   
    ![Instellingen voor eenmalige aanmelding SAML](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "SAML SSO-instellingen")

    a. In de **identiteit providernaam** textbox, typ een naam voor uw configuratie.
    
    b. Plak de **SAML entiteit-ID** waarde opgehaald uit Azure-portal in de **identiteitsprovider entiteit-ID** textbox.
  
    c. Plak de **SAML Single Sign-On Service-URL** waarde opgehaald uit Azure-portal in de **identiteitsprovider SSO-URL** textbox.
  
    d. Plak de **SAML Single Sign-On Service-URL** waarde opgehaald uit Azure-portal in de **aangepaste afmelding URL** textbox.
  
    e. Als u wilt uw gedownloade certificaat uploaden, klikt u op **bestand kiezen**.
  
    f. Selecteer het volgende voor:
    * **Gebruikers-id van SAML**, selecteer **adaptieve Insights gebruiker gebruikersnaam**.
    * **Locatie van het SAML-id**, selecteer **gebruikers-id in NameID van onderwerp**.
    * **SAML NameID indeling**, selecteer **e-mailadres**.
    * **SAML inschakelen**, selecteer **toestaan SAML SSO en direct adaptieve Insights aanmelden**.
    
    g. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-adaptive-suite-test-user"></a>Maken van een testgebruiker adaptieve Suite

Om Azure AD-gebruikers zich aanmelden bij adaptieve Suite, moeten ze worden ingericht in adaptieve Suite.  

* In het geval van adaptieve Suite is inrichting een handmatige taak.

**Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:** 

1. Meld u aan bij uw **adaptieve Suite** bedrijf site als beheerder.
2. Ga naar **Admin**.
   
   ![Beheerder](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")
3. In de **gebruikers en rollen** sectie, klikt u op **gebruiker toevoegen**.
   
   ![Gebruiker toevoegen](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "gebruiker toevoegen")
4. In de **nieuwe gebruiker** sectie, voert u de volgende stappen uit:
   
   ![Indienen](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "verzenden")   

   a. Typ de **naam**, **aanmelding**, **e**, **wachtwoord** van een geldige Azure Active Directory-gebruiker die u inrichten in de bijbehorende tekstvakken wilt.
  
   b. Selecteer een **rol**.
  
   c. Klik op **indienen**.

>[!NOTE]
>U kunt andere adaptieve Suite gebruiker account hulpmiddelen voor het maken of API's geleverd door adaptieve Suite aan inrichten AAD-gebruikersaccounts.
>  

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon toegang verlenen aan adaptieve Suite gebruikt Azure eenmalige aanmelding.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon adaptieve Suite, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **adaptieve Suite**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Microsoft Azure AD Single Sign-On configuratie met behulp van het toegangsvenster.

Als u op de tegel adaptieve Suite in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing adaptieve Suite.


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_203.png

