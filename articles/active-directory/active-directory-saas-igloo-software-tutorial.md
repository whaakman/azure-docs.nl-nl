---
title: 'Zelfstudie: Azure Active Directory-integratie met Igloo Software | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Igloo Software.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 8fc957a6a54f9f3a640147f453ebefcb8aac9d4c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Zelfstudie: Azure Active Directory-integratie met Igloo Software

In deze zelfstudie leert u hoe Igloo Software integreren met Azure Active Directory (Azure AD).

Igloo Software integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Igloo Software heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Igloo Software (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Igloo Software, moet u de volgende items:

- Een Azure AD-abonnement
- Een Igloo Software eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Igloo Software uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-igloo-software-from-the-gallery"></a>Igloo Software uit de galerie toevoegen
Voor het configureren van de integratie van Igloo Software in Azure AD, moet u Igloo Software uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Igloo Software uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Igloo Software**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_search.png)

5. Selecteer in het deelvenster resultaten **Igloo Software**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met Igloo Software op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Igloo Software is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Igloo Software worden gemaakt.

Wijs in Igloo Software, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Igloo Software, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Igloo Software](#creating-an-igloo-software-test-user)**  - Igloo-Software die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Igloo Software.

**Voor het configureren van Azure AD eenmalige aanmelding met Igloo Software, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Igloo Software** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_samlbase.png)

3. Op de **Igloo Software domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_url.png)
    
    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<company name>.igloocommmunities.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<company name>.igloocommmunities.com/saml.digest`

    c. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<company name>.igloocommmunities.com/saml.digest`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [Igloo Software Client ondersteuningsteam](https://www.igloosoftware.com/services/support) ophalen van deze waarden. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-igloo-software-tutorial/tutorial_general_400.png)
    
6. Op de **Igloo softwareconfiguratie** sectie, klikt u op **Igloo Software configureren** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out URL's en SAML Single Sign-On Service** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_configure.png) 

7. In een ander browservenster, meld u aan bij uw bedrijf Software Igloo site als beheerder.

8. Ga naar de **Configuratiescherm**.
   
     ![Het Configuratiescherm](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "Configuratiescherm")

9. In de **lidmaatschap** tabblad **aanmelding In instellingen**.
   
    ![Meld u aan instellingen](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "instellingen aanmelden")

10. Klik in de sectie SAML-configuratie op **SAML-verificatie configureren**.
   
    ![De SAML-configuratie](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "SAML-configuratie")
   
11. In de **algemene configuratie** sectie, voert u de volgende stappen uit:
   
    ![Algemene configuratie](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "algemene configuratie")

    a. In de **verbindingsnaam** textbox, typ een naam voor uw configuratie.
   
    b. In de **IdP aanmeldings-URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.
   
    c. In de **IdP afmelding URL** textbox, plak de waarde van **Sign-Out URL** die u hebt gekopieerd vanuit Azure-portal.
    
    d. Selecteer **afmelding antwoord en Type van de HTTP-aanvraag** als **POST**.
   
    e. Open uw **base 64-** in Kladblok een gecodeerd certificaat gedownload vanuit Azure-portal, de inhoud van het kopiëren naar het Klembord en plakt u deze naar de **openbaar certificaat** textbox.
    
12. In de **respons en de configuratie van verificatie**, voer de volgende stappen uit:
    
    ![Respons en de configuratie van verificatie](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "respons en de configuratie van verificatie")
  
      a. Als **identiteitsprovider**, selecteer **Microsoft ADFS**.
      
      b. Als **id van het Type**, selecteer **e-mailadres**. 

      c. In de **e kenmerk** textbox type **emailaddress**.

      d. In de **voornaam kenmerk** textbox type **givenname**.

      e. In de **laatste naamkenmerk** textbox type **achternaam**.

13. Voer de volgende stappen uit om de configuratie te voltooien:
    
    ![Het maken van de gebruiker op aanmelden](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "aanmaken op aanmelding gebruiker") 

     a. Als **aanmaken op aanmelding gebruiker**, selecteer **Maak een nieuwe gebruiker in uw site wanneer ze zich aanmelden**.

     b. Als **aanmelden instellingen**, selecteer **gebruik SAML-knop op het scherm 'Aanmelden'**.

     c. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-igloo-software-test-user"></a>Een testgebruiker Igloo Software maken

Er is geen actie-item voor gebruikers inrichten op Igloo Software configuratie.  

Wanneer een toegewezen gebruiker probeert zich aanmelden bij Igloo Software in het deelvenster toegang, wordt er Igloo Software controleert of de gebruiker bestaat.  Als er nog geen gebruikersaccount beschikbaar is, wordt het automatisch gemaakt door Igloo Software.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Igloo Software.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Igloo Software, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Igloo Software**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Igloo Software in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Igloo Software.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_203.png

