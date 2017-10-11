---
title: 'Zelfstudie: Azure Active Directory-integratie met Jobscience | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Jobscience.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 77282dcc-bbe2-4728-953d-adb4ab6a713b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 66bec35a8f17482433dbf02827b90620d1cff378
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Zelfstudie: Azure Active Directory-integratie met Jobscience

In deze zelfstudie leert u hoe Jobscience integreren met Azure Active Directory (Azure AD).

Jobscience integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Jobscience heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Jobscience (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Jobscience, moet u de volgende items:

- Een Azure AD-abonnement
- Een Jobscience eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Jobscience uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-jobscience-from-the-gallery"></a>Jobscience uit de galerie toevoegen
Voor het configureren van de integratie van Jobscience in Azure AD, moet u Jobscience uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Jobscience uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Jobscience**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_search.png)

5. Selecteer in het deelvenster resultaten **Jobscience**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Jobscience op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Jobscience is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Jobscience tot stand worden gebracht.

Wijs in Jobscience, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Jobscience, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Jobscience](#creating-a-jobscience-test-user)**  - Jobscience die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Jobscience configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Jobscience, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Jobscience** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_samlbase.png)

3. Op de **Jobscience domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_url.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`http://<company name>.my.salesforce.com`
    
    > [!NOTE] 
    > Deze waarde is geen echte. Deze waarde bijwerken met de werkelijke URL voor eenmalige aanmelding. Deze waarde krijgen door [Jobscience Client ondersteuningsteam](https://www.jobscience.com/support) of van het profiel voor eenmalige aanmelding wordt u maken die later in de zelfstudie wordt uitgelegd. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-jobscience-tutorial/tutorial_general_400.png)

6. Op de **Jobscience configuratie** sectie, klikt u op **configureren Jobscience** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_configure.png) 

7. Meld u aan bij uw bedrijf Jobscience site als beheerder.

8. Ga naar **Setup**.
   
   ![Setup](./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")

9. In het navigatiedeelvenster links in de **beheren** sectie, klikt u op **domeinbeheer** Vouw de bijbehorende sectie en klik vervolgens op **mijn domein** openen van de **Mijn domein** pagina. 
   
   ![Mijn domein](./media/active-directory-saas-jobscience-tutorial/ic767825.png "mijn domein")

10. Om te controleren of uw domein correct is ingesteld, zorg ervoor dat deze wordt '**stap 4 geïmplementeerd naar gebruikers**' en bekijk uw '**mijn domeininstellingen**'.

    ![Domein geïmplementeerd voor gebruiker](./media/active-directory-saas-jobscience-tutorial/ic784377.png "domein geïmplementeerd voor gebruiker")

11. Klik op de site van het bedrijf Jobscience **beveiligingsmechanismen**, en klik vervolgens op **instellingen voor eenmalige aanmelding**.
    
    ![Beveiligingsmechanismen](./media/active-directory-saas-jobscience-tutorial/ic784364.png "beveiligingsmechanismen")

12. In de **instellingen voor eenmalige aanmelding** sectie, voert u de volgende stappen uit:
    
    ![Eenmalige aanmelding instellingen](./media/active-directory-saas-jobscience-tutorial/ic781026.png "eenmalige aanmelding-instellingen")
    
    a. Selecteer **SAML ingeschakeld**.

    b. Klik op **Nieuw**.

13. Op de **SAML Single Sign-On instelling bewerken** dialoogvenster de volgende stappen uitvoeren:
    
    ![Afzonderlijke SAML aanmelding instelling](./media/active-directory-saas-jobscience-tutorial/ic784365.png "SAML Single Sign-On-instelling")
    
    a. In de **naam** textbox, typ een naam voor uw configuratie.

    b. In **verlener** textbox, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal.

    c. In de **entiteit-Id** textbox type`https://salesforce-jobscience.com`

    d. Klik op **Bladeren** om uw Azure AD-certificaat te uploaden.

    e. Als **SAML identiteitstype**, selecteer **Assertion bevat de Federation-ID van het gebruikersobject**.

    f. Als **SAML identiteit locatie**, selecteer **identiteit is in het element NameIdentfier van het onderwerp overzicht**.

    g. In **identiteit Provider aanmeldings-URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.

    h. In **identiteit Provider afmelding URL** textbox, plak de waarde van **Sign-Out URL**, die u hebt gekopieerd vanuit Azure-portal.

    ik. Klik op **Opslaan**.

14. In het navigatiedeelvenster links in de **beheren** sectie, klikt u op **domeinbeheer** Vouw de bijbehorende sectie en klik vervolgens op **mijn domein** openen van de **Mijn domein** pagina. 
    
    ![Mijn domein](./media/active-directory-saas-jobscience-tutorial/ic767825.png "mijn domein")

15. Op de **mijn domein** pagina in de **aanmelding pagina huisstijl** sectie, klikt u op **bewerken**.
    
    ![Aanmeldingspagina huisstijl](./media/active-directory-saas-jobscience-tutorial/ic767826.png "aanmeldingspagina huisstijl")

16. Op de **aanmelding pagina huisstijl** pagina in de **verificatieservice** sectie, de naam van uw **SAML SSO instellingen** wordt weergegeven. Selecteer deze en klik vervolgens op **opslaan**.
    
    ![Aanmeldingspagina huisstijl](./media/active-directory-saas-jobscience-tutorial/ic784366.png "aanmeldingspagina huisstijl")

17. Gestart om op te halen van de Serviceprovider voor eenmalige aanmeldings-URL wordt geklikt op de **instellingen voor eenmalige aanmelding** in de **beveiligingsmechanismen** sectie menu.

    ![Beveiligingsmechanismen](./media/active-directory-saas-jobscience-tutorial/ic784368.png "beveiligingsmechanismen")
    
    Klik op de SSO-profiel dat u hebt gemaakt in de bovenstaande stap. Deze pagina bevat de eenmalige aanmelding op de URL voor uw bedrijf (bijvoorbeeld [https://companyname.my.salesforce.com?so=companyid](https://companyname.my.salesforce.com?so=companyid).    

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-jobscience-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-jobscience-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-jobscience-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-jobscience-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-jobscience-test-user"></a>Een testgebruiker Jobscience maken

Om Azure AD-gebruikers zich aanmelden bij Jobscience inschakelt, moeten ze worden ingericht in Jobscience. In het geval van Jobscience is inrichting een handmatige taak.

>[!NOTE]
>U kunt andere Jobscience gebruiker account hulpmiddelen voor het maken of API's geleverd door Jobscience om in te richten Azure Active Directory-gebruikersaccounts.
>  
        
**Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Jobscience** bedrijf site als administrator.

2. Ga naar instellingen.
   
   ![Setup](./media/active-directory-saas-jobscience-tutorial/ic784358.png "Setup")
3. Ga naar **gebruikers beheren \> gebruikers**.
   
   ![Gebruikers](./media/active-directory-saas-jobscience-tutorial/ic784369.png "gebruikers")
4. Klik op **nieuwe gebruiker**.
   
   ![Alle gebruikers](./media/active-directory-saas-jobscience-tutorial/ic784370.png "alle gebruikers")
5. Op de **-gebruiker bewerken** dialoogvenster de volgende stappen uitvoeren:
   
   ![Gebruiker bewerken](./media/active-directory-saas-jobscience-tutorial/ic784371.png "gebruiker bewerken")
   
   a. In de **voornaam** textbox, typ een naam van de eerste van de gebruiker zoals Britta.
   
   b. In de **achternaam** textbox, typt u de achternaam van de gebruiker zoals Simon.
   
   c. In de **Alias** textbox, typt u een aliasnaam van de gebruiker zoals brittas.

   d. In de **e** textbox, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

   e. In de **gebruikersnaam** textbox, typ een naam van gebruiker, zoals Brittasimon@contoso.com.

   f. In de **bijnaam** textbox, typ een naam nick van gebruiker zoals Simon.

   g. Klik op **Opslaan**.

    
> [!NOTE]
> De houder van Azure Active Directory-account ontvangt een e-mailbericht en volgt een koppeling om hun account te bevestigen voordat deze geactiveerd wordt.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Jobscience.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Jobscience, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Jobscience**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Jobscience in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Jobscience.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_203.png

