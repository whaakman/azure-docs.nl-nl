---
title: 'Zelfstudie: Azure Active Directory-integratie met Work.com | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Work.com.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 7cfec8e9ac12d43095483696a15c0580776d3114
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>Zelfstudie: Azure Active Directory-integratie met Work.com

In deze zelfstudie leert u hoe Work.com integreren met Azure Active Directory (Azure AD).

Work.com integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Work.com heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Work.com (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Work.com, moet u de volgende items:

- Een Azure AD-abonnement
- Een Work.com eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Work.com uit de galerie toevoegen
2. Configureren en testen eenmalige aanmelding Azure AD

## <a name="add-workcom-from-the-gallery"></a>Work.com uit de galerie toevoegen
Voor het configureren van de integratie van Work.com in Azure AD, moet u Work.com uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Work.com uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Work.com**, selecteer **Work.com** Klik vanuit het deelvenster resultaten **toevoegen** om toe te voegen van de toepassing.

    ![Uit de galerie toevoegen](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie configureert en test eenmalige aanmelding Azure AD met Work.com op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Work.com is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Work.com tot stand worden gebracht.

Wijs in Work.com, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Work.com, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Work.com](#create-a-workcom-test-user)**  - Work.com die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Work.com configureren.

>[!NOTE]
>Voor het configureren van eenmalige aanmelding, moet u een aangepaste domeinnaam van Work.com nog instellen. U moet ten minste een domeinnaam definiëren, testen van uw domeinnaam en deze implementeren voor uw hele organisatie.

**Voor het configureren van Azure AD eenmalige aanmelding met Work.com, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Work.com** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Op basis van SAML eenmalige aanmelding](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_samlbase.png)

3. Op de **Work.com domein en de URL's** sectie, voert u het volgende:

    ![Sectie Work.com domein en URL 's](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_url.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`http://<companyname>.my.salesforce.com`

    > [!NOTE] 
    > Deze waarde is geen echte. Deze waarde bijwerken met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [Work.com Client ondersteuningsteam](https://help.salesforce.com/articleView?id=000159855&type=3) deze waarde op te halen. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Certificaat voor ondertekening van SAML-sectie](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_certificate.png) 

5. Klik op **opslaan** knop.

    ![De knop Opslaan](./media/active-directory-saas-work-com-tutorial/tutorial_general_400.png)

6. Op de **Work.com configuratie** sectie, klikt u op **configureren Work.com** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![De configuratiesectie Work.com](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_configure.png) 
7. Aanmelden bij uw tenant Work.com als administrator.

8. Ga naar **Setup**.
   
    ![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")

9. In het navigatiedeelvenster links in de **beheren** sectie, klikt u op **domeinbeheer** Vouw de bijbehorende sectie en klik vervolgens op **mijn domein** openen van de **Mijn domein** pagina. 
   
    ![Mijn domein](./media/active-directory-saas-work-com-tutorial/ic767825.png "mijn domein")

10. Om te controleren of uw domein correct is ingesteld, zorg ervoor dat deze wordt '**stap 4 geïmplementeerd naar gebruikers**' en bekijk uw '**mijn domeininstellingen**'.
   
    ![Domein geïmplementeerd voor gebruiker](./media/active-directory-saas-work-com-tutorial/ic784377.png "domein geïmplementeerd voor gebruiker")

11. Aanmelden bij uw tenant Work.com.

12. Ga naar **Setup**.
    
    ![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")

13. Vouw de **beveiligingsmechanismen** menu en klik vervolgens op **instellingen voor eenmalige aanmelding**.
    
    ![Eenmalige aanmelding instellingen](./media/active-directory-saas-work-com-tutorial/ic794113.png "eenmalige aanmelding-instellingen")

14. Op de **instellingen voor eenmalige aanmelding** dialoogvenster pagina, voert u de volgende stappen uit:
    
    ![SAML ingeschakeld](./media/active-directory-saas-work-com-tutorial/ic781026.png "SAML ingeschakeld")
    
    a. Selecteer **SAML ingeschakeld**.
    
    b. Klik op **Nieuw**.

15. In de **SAML Single Sign-On-instellingen** sectie, voert u de volgende stappen uit:
    
    ![Afzonderlijke SAML aanmelding instelling](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML Single Sign-On-instelling")
    
    a. In de **naam** textbox, typ een naam voor uw configuratie.  
       
    > [!NOTE]
    > Om een waarde voor **naam** automatisch invullen de **API-naam** textbox.
    
    b. In **verlener** textbox, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure-portal.
    
    c. Als u wilt uploaden het gedownloade certificaat vanuit Azure-portal, klikt u op **Bladeren**.
    
    d. In de **entiteit-Id** textbox type `https://salesforce-work.com`.
    
    e. Als **SAML identiteitstype**, selecteer **Assertion bevat de Federation-ID van het gebruikersobject**.
    
    f. Als **SAML identiteit locatie**, selecteer **identiteit is in het element NameIdentfier van het onderwerp overzicht**.
    
    g. In **identiteit Provider aanmeldings-URL** textbox, plak de waarde van **SAML Single Sign-On Service-URL** die u hebt gekopieerd vanuit Azure-portal.

    h. In **identiteit Provider afmelding URL** textbox, plak de waarde van **Sign-Out URL** die u hebt gekopieerd vanuit Azure-portal.
    
    ik. Als **Provider geïnitieerd aanvragen servicebinding**, selecteer **HTTP Post**.
    
    j. Klik op **Opslaan**.

16. Klik in de klassieke portal van Work.com in het navigatiedeelvenster links op **domeinbeheer** Vouw de bijbehorende sectie en klik vervolgens op **mijn domein** openen de **mijn domein** pagina. 
    
    ![Mijn domein](./media/active-directory-saas-work-com-tutorial/ic794115.png "mijn domein")

17. Op de **mijn domein** pagina in de **aanmelding pagina huisstijl** sectie, klikt u op **bewerken**.
    
    ![Aanmeldingspagina huisstijl](./media/active-directory-saas-work-com-tutorial/ic767826.png "aanmeldingspagina huisstijl")

14. Op de **aanmelding pagina huisstijl** pagina in de **verificatieservice** sectie, de naam van uw **SAML SSO instellingen** wordt weergegeven. Selecteer deze en klik vervolgens op **opslaan**.
    
    ![Aanmeldingspagina huisstijl](./media/active-directory-saas-work-com-tutorial/ic784366.png "aanmeldingspagina huisstijl")

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-work-com-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Gebruikers en groepen -> alle gebruikers](./media/active-directory-saas-work-com-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Toevoegen](./media/active-directory-saas-work-com-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Dialoogvenster op de gebruikerspagina](./media/active-directory-saas-work-com-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-workcom-test-user"></a>Een testgebruiker Work.com maken
Voor Azure Active Directory-gebruikers moeten kunnen aanmelden, moeten ze worden ingericht op Work.com. In het geval van Work.com is inrichting een handmatige taak.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren voor gebruikers inrichten, moet u de volgende stappen uitvoeren:
1. Meld u op met uw bedrijf Work.com site als een beheerder.

2. Ga naar **Setup**.
   
    ![Setup](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")
3. Ga naar **gebruikers beheren \> gebruikers**.
   
    ![Gebruikers beheren](./media/active-directory-saas-work-com-tutorial/IC784369.png "gebruikers beheren")

4. Klik op **nieuwe gebruiker**.
   
    ![Alle gebruikers](./media/active-directory-saas-work-com-tutorial/IC794117.png "alle gebruikers")

5. Uitvoeren in de sectie gebruikers bewerken in de volgende stappen in de kenmerken van een geldig Azure AD-account die u inrichten in de bijbehorende tekstvakken wilt:
   
    ![Gebruiker bewerken](./media/active-directory-saas-work-com-tutorial/ic794118.png "gebruiker bewerken")
   
    a. In de **voornaam** textbox type de **voornaam** van de gebruiker **Britta**.
    
    b. In de **achternaam** textbox type de **achternaam** van de gebruiker **Simon**.
    
    c. In de **Alias** textbox type de **naam** van de gebruiker **BrittaS**.
    
    d. In de **e** textbox type de **e-mailadres** van gebruiker  **Brittasimon@contoso.com** .
    
    e. In de **gebruikersnaam** textbox, typ een naam van gebruiker, zoals  **Brittasimon@contoso.com** .
    
    f. In de **bijnaam** textbox, typ een **bijnaam** van gebruiker **Simon**.
    
    g. Selecteer **rol**, **gebruikerslicentie**, en **profiel**.
    
    h. Klik op **Opslaan**.  
      
    > [!NOTE]
    > De accounthouder Azure AD ontvangt een e-mailbericht met inbegrip van een koppeling om te bevestigen van het account voordat deze geactiveerd wordt.
    > 
    > 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Work.com.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Work.com, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Work.com**.

    ![Work.com in de lijst van app](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Work.com in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Work.com.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_203.png

