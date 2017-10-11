---
title: 'Zelfstudie: Azure Active Directory-integratie met LearnUpon | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en LearnUpon.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: b6ac8acc244e9029be01ede5e0865c280171217d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Zelfstudie: Azure Active Directory-integratie met LearnUpon

In deze zelfstudie leert u hoe LearnUpon integreren met Azure Active Directory (Azure AD).

LearnUpon integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot LearnUpon heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij LearnUpon (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met LearnUpon, moet u de volgende items:

- Een Azure AD-abonnement
- Een LearnUpon eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. LearnUpon uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-learnupon-from-the-gallery"></a>LearnUpon uit de galerie toevoegen
Voor het configureren van de integratie van LearnUpon in Azure AD, moet u LearnUpon uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen LearnUpon uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **LearnUpon**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_search.png)

5. Selecteer in het deelvenster resultaten **LearnUpon**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met LearnUpon op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in LearnUpon is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in LearnUpon tot stand worden gebracht.

Wijs in LearnUpon, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met LearnUpon, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker LearnUpon](#creating-a-learnupon-test-user)**  - LearnUpon die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing LearnUpon configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met LearnUpon, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **LearnUpon** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_samlbase.png)

3. Op de **LearnUpon domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_url.png)

    In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<companyname>.learnupon.com/saml/consumer`

    > [!NOTE] 
    > Houd er rekening mee dat dit geen de feitelijke waarde is. u moet deze waarde bijwerken met de werkelijke antwoord-URL. Deze waarde Contact op te halen [LearnUpon ondersteuningsteam](https://www.learnupon.com/features/support/).



4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Raw)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_general_400.png)

6. Op de **LearnUpon configuratie** sectie, klikt u op **configureren LearnUpon** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_configure.png) 

7. Open een ander browserexemplaar en meld u aan bij LearnUpon met een administratoraccount. 

8. Klik op de **instellingen** tabblad.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png)

9. Klik op **eenmalige aanmelding - SAML**, en klik vervolgens op **algemene instellingen** om SAML-instellingen te configureren.
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 

10. In de **algemene instellingen** sectie, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png)  
  
    a. Selecteer **ingeschakeld**.

    b. Selecteer **versie** als **2.0**.

    c. Selecteer **overslaan voorwaarden** als **Nee**.

    d. In de **SAML-Token boeken param naam** textbox, de naam van de aanvraag post-parameter voor de URL van de consument SAML hierboven genoemde type met de SAML-verklaring worden geverifieerd en geverifieerd - bijvoorbeeld **SAMLResponse** .

    e. In de **indeling van de id** textbox type de waarde die waar in uw SAML-verklaring (e-mailadres) van de gebruikers-id aangeeft bevindt zich - bijvoorbeeld **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: emailAddress**.
  
    f. In de **Providerlocatie identificeren** textbox, typ de waarde die waar de gebruikers worden verzonden aangeeft als ze op het pictogram van uw geüploade van het scherm van uw Azure-portal aanmelding klikt.
  
    g. In de **afmelden URL** textbox, plak de **Sign-Out URL** die u hebt gekopieerd uit de Azure portal.
    
    h. Klik op **vinger afdrukken bestellen beheren**, en vervolgens de vingerafdruk van het gedownloade certificaat te uploaden.

11. Klik op **gebruikersinstellingen**, en voer de volgende stappen uit:
   
     ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png)  
 
    a. In de **voornaam id indeling** textbox type de waarde die kan worden weergegeven wanneer u in uw SAML-verklaring de voornaam van de gebruikers zich - voorbeeld bevindt: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
  
    b. In de **laatste naamnotatie id** textbox type de waarde die kan worden weergegeven wanneer u in uw SAML-verklaring de achternaam van de gebruikers zich - voorbeeld bevindt: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname** .

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-learnupon-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-learnupon-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-learnupon-test-user"></a>Een testgebruiker LearnUpon maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in LearnUpon genoemd. LearnUpon ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker wordt gemaakt tijdens een poging tot toegang tot LearnUpon als deze nog niet bestaat. [Configureren van Azure AD voor eenmalige aanmelding](#configuring-azure-ad-single-single-sign-on).

>[!NOTE]
>Als u een gebruiker handmatig maken wilt, moet u contact op met [LearnUpon ondersteuningsteam](https://www.learnupon.com/features/support/). 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan LearnUpon.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen LearnUpon, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **LearnUpon**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel LearnUpon in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing LearnUpon.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png

