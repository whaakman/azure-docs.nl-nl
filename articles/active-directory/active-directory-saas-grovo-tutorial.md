---
title: 'Zelfstudie: Azure Active Directory-integratie met Grovo | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Grovo.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 399cecc3-aa62-4914-8b6c-5a35289820c1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2018
ms.author: jeedes
ms.openlocfilehash: e4bb050e96d3e8d9da4666f5418ac3e444f6212d
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="tutorial-azure-active-directory-integration-with-grovo"></a>Zelfstudie: Azure Active Directory-integratie met Grovo

In deze zelfstudie leert u hoe Grovo integreren met Azure Active Directory (Azure AD).

Grovo integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Grovo heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Grovo (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Grovo, moet u de volgende items:

- Een Azure AD-abonnement
- Een Grovo eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Grovo uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-grovo-from-the-gallery"></a>Grovo uit de galerie toevoegen
Voor het configureren van de integratie van Grovo in Azure AD, moet u Grovo uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Grovo uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **Grovo**, selecteer **Grovo** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Grovo in de lijst met resultaten](./media/active-directory-saas-grovo-tutorial/tutorial_grovo_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met Grovo op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Grovo is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Grovo tot stand worden gebracht.

Wijs in Grovo, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Grovo, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Grovo](#create-a-grovo-test-user)**  - Grovo die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Grovo configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Grovo, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Grovo** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-grovo-tutorial/tutorial_grovo_samlbase.png)

3. Op de **Grovo domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren, de toepassing in **IDP** modus gestart:

    ![URL's en Grovo domein eenmalige aanmelding informatie](./media/active-directory-saas-grovo-tutorial/tutorial_grovo_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<subdomain>.grovo.com/sso/saml2/metadata`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<subdomain>.grovo.com/sso/saml2/saml-assertion`

4. Controleer **weergeven geavanceerde instellingen voor URL**, voer de volgende stap:

    ![URL's en Grovo domein eenmalige aanmelding informatie](./media/active-directory-saas-grovo-tutorial/tutorial_grovo_url1.png)

    a. In de **Relay-status** textbox, typ een URL met het volgende patroon volgen:`https://<subdomain>.grovo.com`

    b. Als u wilt configureren van de toepassing in **SP** geïnitieerd modus, voer de volgende stappen uit:

    ![URL's en Grovo domein eenmalige aanmelding informatie](./media/active-directory-saas-grovo-tutorial/tutorial_grovo_url2.png)
    
    In de **aanmelden URL** textbox, typ een URL met het volgende patroon volgen:`https://<subdomain>.grovo.com/sso/saml2/saml-assertion`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke-id en antwoord-URL met eenmalige URL en de Relay-status. Neem contact op met [Grovo ondersteuningsteam](https://www.grovo.com/contact-us) ophalen van deze waarden.
 
5. De SAML-asserties verwacht Grovo toepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt beheren de waarden van deze kenmerken van de '**gebruikerskenmerken**' sectie op de pagina van de toepassing-integratie. Wijs **gebruikers-id** met **user.mail** en andere kenmerken configureren, zoals weergegeven in onderstaande schermafbeelding.
    
    ![Single Sign-On attb configureren](./media/active-directory-saas-grovo-tutorial/tutorial_grovo_attribute.png)
    
6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en de volgende stappen uitvoeren:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | Voornaam          | user.givenname |
    | Achternaam           | User.surname |
    | E-mailadres       | User.mail    |
    | Werknemer-id          | user.employeeid |

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren toevoegen](./media/active-directory-saas-grovo-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding Addattb configureren](./media/active-directory-saas-grovo-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.
    
    e. Klik op **OK**.


7. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-grovo-tutorial/tutorial_grovo_certificate.png) 

8. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-grovo-tutorial/tutorial_general_400.png)

9. Op de **Grovo configuratie** sectie, klikt u op **configureren Grovo** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Grovo configuratie](./media/active-directory-saas-grovo-tutorial/tutorial_grovo_configure.png) 

10. In een ander browservenster, meld u aan bij Grovo als Administrator.

11. Ga naar **Admin** > **integraties**.
 
    ![Grovo configuratie](./media/active-directory-saas-grovo-tutorial/tutorial_grovo_admin.png) 

12. Klik op **instellen** onder **SP geïnitieerd SAML 2.0** sectie.

    ![Grovo configuratie](./media/active-directory-saas-grovo-tutorial/tutorial_grovo_setup.png)

13. In **SP geïnitieerd SAML 2.0** pop-upvenster de volgende stappen uitvoeren:

    ![Grovo configuratie](./media/active-directory-saas-grovo-tutorial/tutorial_grovo_saml.png)

    a. In de **entiteit-id** textbox, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal.

    b. In de **eenmalige aanmelding bij het service-eindpunt** textbox, plak de waarde van **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.

    c. Selecteer **eenmalige aanmelding bij het service-eindpuntbinding** als `urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect`.
    
    d. Open de gedownloade **met Base64 versleuteld certificaat** vanuit Azure-portal in Kladblok, plak deze in de **openbare sleutel** textbox.

    e. Klik op **Volgende**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-grovo-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-grovo-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-grovo-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-grovo-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-grovo-test-user"></a>Een testgebruiker Grovo maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in Grovo genoemd. Grovo ondersteunt just-in-time-inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Grovo als deze nog niet bestaat.
>[!Note]
>Als u wilt een gebruiker handmatig maken, neem contact op met [Grovo ondersteuningsteam](https://www.grovo.com/contact-us).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Grovo.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen Grovo, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Grovo**.

    ![De koppeling Grovo in de lijst met toepassingen](./media/active-directory-saas-grovo-tutorial/tutorial_grovo_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Grovo in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Grovo.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-grovo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-grovo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-grovo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-grovo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-grovo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-grovo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-grovo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-grovo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-grovo-tutorial/tutorial_general_203.png

