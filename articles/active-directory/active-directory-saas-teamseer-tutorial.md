---
title: 'Zelfstudie: Azure Active Directory-integratie met TeamSeer | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TeamSeer.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 2a5e8f6d1443681c43db95da5cef0b7f2ef92291
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Zelfstudie: Azure Active Directory-integratie met TeamSeer

In deze zelfstudie leert u hoe TeamSeer integreren met Azure Active Directory (Azure AD).

TeamSeer integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TeamSeer heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij TeamSeer (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TeamSeer, moet u de volgende items:

- Een Azure AD-abonnement
- Een TeamSeer eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TeamSeer uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-teamseer-from-the-gallery"></a>TeamSeer uit de galerie toevoegen
Voor het configureren van de integratie van TeamSeer in naar Azure AD, moet u TeamSeer uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen TeamSeer uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **TeamSeer**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_search.png)

5. Selecteer in het deelvenster resultaten **TeamSeer**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met TeamSeer op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in TeamSeer is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in TeamSeer tot stand worden gebracht.

Wijs in TeamSeer, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TeamSeer, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker TeamSeer](#creating-a-teamseer-test-user)**  - TeamSeer die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing TeamSeer configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met TeamSeer, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **TeamSeer** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_samlbase.png)

3. Op de **TeamSeer domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_url.png)

     In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://www.teamseer.com/<companyid>`

    > [!NOTE] 
    > De waarde is geen echte. Werk de waarde met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [TeamSeer Client ondersteuningsteam](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) de waarde op te halen. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-teamseer-tutorial/tutorial_general_400.png)

6. Op de **TeamSeer configuratie** sectie, klikt u op **configureren TeamSeer** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_configure.png)

7. In een ander browservenster, meld u aan bij uw bedrijf TeamSeer site als beheerder.

8. Ga naar **HR Admin**.
   
    ![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR-beheerder")

9. Klik op **Setup**.
   
    ![Setup](./media/active-directory-saas-teamseer-tutorial/ic789635.png "Setup")

10. Klik op **SAML provider details instellen**.
   
    ![Instellingen voor SAML](./media/active-directory-saas-teamseer-tutorial/ic789636.png "SAML-instellingen")

11. In het gedeelte details van SAML-provider de volgende stappen uitvoeren:
   
    ![Instellingen voor SAML](./media/active-directory-saas-teamseer-tutorial/ic789637.png "SAML-instellingen")   

    a. Plak de **Single Sign-On Service-URL** waarde in voor de **URL** textbox.
          
    b. De base-64 gecodeerde certificaat openen in Kladblok, Kopieer de inhoud van in naar het Klembord en plakt u deze naar de **IdP openbaar certificaat** textbox.

12. Voor het voltooien van de configuratie van de SAML-provider, moet u de volgende stappen uitvoeren:
    
    ![Instellingen voor SAML](./media/active-directory-saas-teamseer-tutorial/ic789638.png "SAML-instellingen") 

    a. In de **e-mailadressen testen**, typ de e-mailadres van de testgebruiker. 
  
    b. In de **verlener** textbox, typ de URL van de verlener van de serviceprovider. 
  
    c. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-teamseer-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-teamseer-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-teamseer-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-teamseer-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-teamseer-test-user"></a>Een testgebruiker TeamSeer maken

Om Azure AD-gebruikers zich aanmelden bij TeamSeer, moeten ze worden ingericht op ShiftPlanning. In het geval van TeamSeer is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **TeamSeer** bedrijf site als beheerder.

2. De volgende stappen uitvoeren:
   
    ![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR-beheerder")  
 
    a. Ga naar **HR Admin \> gebruikers**.
  
    b. Klik op **voert u de wizard nieuwe gebruiker**.

3. In de **Gebruikersdetails** sectie, voert u de volgende stappen uit:
   
    ![Details van gebruiker](./media/active-directory-saas-teamseer-tutorial/ic789641.png "Gebruikersdetails")

    a. Typ de **voornaam**, **achternaam**, **gebruikersnaam (e-mailadres)** van een geldige AAD-account dat u inrichten wilt de bijbehorende tekstvakken.
  
    b. Klik op **Volgende**.

4. Volg de aanwijzingen op het scherm instructies voor het toevoegen van een nieuwe gebruiker en klik op **voltooien**.

>[!NOTE]
>U kunt andere TeamSeer gebruiker account hulpmiddelen voor het maken of API's die is geleverd door TeamSeer voor het inrichten van Azure AD-gebruikersaccounts. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan TeamSeer.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen TeamSeer, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **TeamSeer**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_203.png

