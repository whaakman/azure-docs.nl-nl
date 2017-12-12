---
title: 'Zelfstudie: Azure Active Directory-integratie met iLMS | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en iLMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: 2764a109e92d3eabf2b7064ce7cd2e428256c8b8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a>Zelfstudie: Azure Active Directory-integratie met iLMS

In deze zelfstudie leert u hoe iLMS integreren met Azure Active Directory (Azure AD).

ILMS integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot iLMS heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij iLMS (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met iLMS, moet u de volgende items:

- Een Azure AD-abonnement
- Een iLMS eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ILMS uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-ilms-from-the-gallery"></a>ILMS uit de galerie toevoegen
Voor het configureren van de integratie van iLMS in Azure AD, moet u iLMS uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen iLMS uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **iLMS**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_search.png)

5. Selecteer in het deelvenster resultaten **iLMS**, klikt u vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met iLMS op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in iLMS is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in iLMS tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in iLMS.

Om te configureren en testen van Azure AD eenmalige aanmelding met iLMS, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker iLMS](#creating-an-ilms-test-user)**  : een equivalent van Britta Simon in iLMS die is gekoppeld aan de Azure AD-representatie van haar hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing iLMS configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met iLMS, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **iLMS** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_samlbase.png)

3. Op de **iLMS domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url.png)

    a. In de **id** textbox, plak de **id** waarde u van kopiëren **serviceprovider** sectie van SAML-instellingen in de beheerportal iLMS.

    b. In de **antwoord-URL** textbox, plak de **eindpunt (URL)** waarde u van kopiëren **serviceprovider** sectie van SAML-instellingen in het volgende patroon iLMS-beheerportal`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

    >[!Note]
    >Deze '123456' is een van de Voorbeeldwaarde van id.

4. Controleer **weergeven geavanceerde instellingen voor URL**, als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url1.png)

    In de **aanmeldings-URL** textbox, plak de **eindpunt (URL)** waarde u van kopiëren **serviceprovider** sectie van SAML-instellingen in de beheerportal iLMS als`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`     

5. Om in te schakelen JIT-inrichting, verwacht iLMS toepassing de SAML-asserties in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken van beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ilms-tutorial/4.png)
    
    Maak **afdeling, regio** en **deling** kenmerken en voegt u de naam van deze kenmerken in iLMS. Alle deze kenmerken die hierboven zijn vereist.  

    > [!NOTE] 
    > U moet inschakelen **Un-recognized-gebruikersaccount maken** in iLMS deze kenmerken toewijzen. Volg de instructies [hier](http://support.inspiredelearning.com/customer/portal/articles/2204526) voor een beter beeld van de configuratie van de kenmerken.

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding hierboven en voer de volgende stappen uit:
    
    | Kenmerknaam | Waarde kenmerk |
    | ---------------| --------------- |    
    | deling | User.Department |
    | regio | User.state |
    | Afdeling | User.jobtitle |

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_04.png)

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_05.png)
    
    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **Ok**

7. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_certificate.png) 

8. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-iLMS-tutorial/tutorial_general_400.png)

9. In een ander browservenster geopend, moet u zich aanmelden bij uw **iLMS-beheerportal** als beheerder.

10. Klik op **SSO:SAML** onder **instellingen** tabblad SAML-instellingen openen en voer de volgende stappen uit:
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ilms-tutorial/1.png) 

    a. Vouw de **serviceprovider** sectie en kopieer de **id** en **eindpunt (URL)** waarde.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ilms-tutorial/2.png) 

    b. Onder **identiteitsprovider** sectie, klikt u op **metagegevens importeren**.
    
    c. Selecteer de **metagegevens** bestand gedownload van Azure-Portal **certificaat voor ondertekening van SAML** sectie.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig1.png) 

    d. Als u inschakelen wilt voor het maken van accounts iLMS voor ongedaan maken inrichting JIT-gebruikers herkennen, volgt u onderstaande stappen te volgen:
        
       - Controleer **Account maken voor niet-herkende gebruiker**.
       
       ![Eenmalige aanmelding configureren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig2.png)

       -  De kenmerken in Azure AD met de kenmerken in iLMS toewijzen. Geef de naam van de kenmerken of de standaardwaarde in de kenmerkkolom.

    e. Ga naar **bedrijfsregels** tabblad en voer de volgende stappen uit: 
        
       ![Eenmalige aanmelding configureren](./media/active-directory-saas-ilms-tutorial/5.png)

       - Controleer **Un-recognized regio's maken, afdelingen en afdelingen** regio's, afdelingen en afdelingen die nog niet aanwezig op het moment van eenmalige aanmelding maken.
        
       - Controleer **Update gebruiker profiel tijdens aanmelden** om op te geven of profiel van de gebruiker is bijgewerkt met elke eenmalige aanmelding. 
        
       - Als de **'Update lege waarden voor niet verplichte velden in gebruikersprofiel'** optie is ingeschakeld, optionele profiel velden leeg bij het aanmelden wordt zijn ook voor zorgen dat het iLMS gebruikersprofiel bevat lege waarden voor deze velden.
        
       - Controleer **fout meldingse-mail verzenden** en voer het e-mailadres van de gebruiker waarop u wilt ontvangen van de e-mailmelding fout.

11. Klik op **opslaan** knop de instellingen op te slaan.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ilms-tutorial/save.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
    
### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-ilms-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klik op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-ilms-tutorial/create_aaduser_02.png) 

3. Klik aan de bovenkant van het dialoogvenster **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-ilms-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-ilms-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-ilms-test-user"></a>Een testgebruiker iLMS maken

Toepassing ondersteunt Just in time gebruikers inrichten en na verificatie gebruikers automatisch in de toepassing gemaakt worden. JIT werkt als u hebt geklikt op de **Un-recognized-gebruikersaccount maken** selectievakje tijdens SAML-configuratie-instelling op iLMS-beheerportal.

Als u een gebruiker handmatig maken wilt, volgt u onderstaande stappen te volgen:

1. Meld u aan bij uw bedrijf iLMS site als beheerder.

2. Klik op **'Gebruiker registreren'** onder **gebruikers** tabblad openen **gebruiker registreren** pagina. 
   
   ![Werknemer toevoegen](./media/active-directory-saas-ilms-tutorial/3.png)

3. Op de **'Gebruiker registreren'** pagina, de volgende stappen uitvoeren.

    ![Werknemer toevoegen](./media/active-directory-saas-ilms-tutorial/create_testuser_add.png)

    a. In de **voornaam** textbox Britta de eerste typenaam.
   
    b. In de **achternaam** textbox, typ de achternaam Simon.

    c. In de **E-mail-ID** textbox, typ de e-mailadres van Britta Simon account.

    d. In de **regio** vervolgkeuzelijst, selecteer de waarde voor de regio.

    e. In de **deling** vervolgkeuzelijst, selecteer de waarde voor verdeling.

    f. In de **afdeling** vervolgkeuzelijst, selecteer de waarde voor afdeling.

    g. Klik op **Opslaan**.

    > [!NOTE] 
    > U kunt registratie-e-mail naar gebruiker verzenden door te selecteren **registratie E-mail verzenden** selectievakje.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen tot iLMS.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen iLMS, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **iLMS**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel iLMS in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing iLMS.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_203.png

