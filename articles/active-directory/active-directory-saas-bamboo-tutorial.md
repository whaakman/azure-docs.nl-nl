---
title: 'Zelfstudie: Azure Active Directory-integratie met SAML SSO voor Bamboe door resolutie GmbH | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAML SSO voor Bamboe door resolutie GmbH.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: f00160c7-f4cc-43bf-af18-f04168d3767c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: jeedes
ms.openlocfilehash: c677161defb04091566ae608c3edfb9f25af801b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bamboo-by-resolution-gmbh"></a>Zelfstudie: Azure Active Directory-integratie met SAML SSO voor Bamboe door resolutie GmbH

In deze zelfstudie leert u het integreren van SAML SSO voor Bamboe door resolutie GmbH met Azure Active Directory (Azure AD).

Integratie van SAML SSO voor Bamboe door resolutie GmbH met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SAML SSO voor Bamboe door resolutie GmbH heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SAML SSO voor Bamboe door resolutie GmbH (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAML SSO voor Bamboe door resolutie GmbH, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAML SSO voor Bamboe door resolutie GmbH eenmalige aanmelding voor ingeschakelde abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAML SSO voor Bamboe door resolutie GmbH uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-saml-sso-for-bamboo-by-resolution-gmbh-from-the-gallery"></a>SAML SSO voor Bamboe door resolutie GmbH uit de galerie toevoegen
Voor het configureren van de integratie van SAML SSO voor Bamboe door resolutie GmbH in Azure AD, moet u SAML SSO voor Bamboe door resolutie GmbH uit de galerie toevoegt aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SAML SSO voor Bamboe resolutie GmbH uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **SAML SSO voor Bamboe door resolutie GmbH**, selecteer **SAML SSO voor Bamboe door resolutie GmbH** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![SAML SSO voor Bamboe door oplossing GmbH in de lijst met resultaten](./media/active-directory-saas-bamboo-tutorial/tutorial_bamboo_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met SAML SSO voor Bamboe door resolutie die GmbH op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de equivalente gebruiker in SAML SSO voor Bamboe door resolutie GmbH aan een gebruiker in Azure AD is. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in SAML SSO voor Bamboe door resolutie GmbH moet tot stand worden gebracht.

Wijs in SAML SSO voor Bamboe door resolutie GmbH, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SAML SSO voor Bamboe door resolutie GmbH, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een SAML SSO voor Bamboe door resolutie GmbH testgebruiker](#create-a-saml-sso-for-bamboo-by-resolution-gmbh-test-user)**  - bevatten een equivalent van Britta Simon SAML SSO voor Bamboe door resolutie GmbH die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en configureer eenmalige aanmelding in uw SAML SSO voor Bamboe resolutie GmbH toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met SAML SSO voor Bamboe door resolutie GmbH, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SAML SSO voor Bamboe door resolutie GmbH** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-bamboo-tutorial/tutorial_bamboo_samlbase.png)

3. Op de **SAML SSO voor Bamboe door resolutie GmbH domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in de IDP geïnitieerd modus:

    ![SAML SSO voor Bamboe door resolutie GmbH domein en één URL's aanmelding informatie](./media/active-directory-saas-bamboo-tutorial/tutorial_bamboo_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<server-base-url>/plugins/servlet/samlsso`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<server-base-url>/plugins/servlet/samlsso`

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![SAML SSO voor Bamboe door resolutie GmbH domein en één URL's aanmelding informatie](./media/active-directory-saas-bamboo-tutorial/tutorial_bamboo_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [SAML SSO voor Bamboe door resolutie GmbH Client ondersteuningsteam](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) ophalen van deze waarden. 

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-bamboo-tutorial/tutorial_bamboo_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-bamboo-tutorial/tutorial_general_400.png)

7. Aanmelding bij uw SAML SSO voor Bamboe door resolutie GmbH bedrijf site als administrator.

8. Klik aan de rechterkant van de hoofdwerkbalk **instellingen** > **invoegtoepassingen**.

    ![De instellingen](./media/active-directory-saas-bamboo-tutorial/tutorial_bamboo_setings.png)

9. Ga naar de sectie beveiliging, klikt u op **SAML SingleSignOn** op de menubalk.

    ![De Samlsingle](./media/active-directory-saas-bamboo-tutorial/tutorial_bamboo_samlsingle.png)

10. Op de **SAML SIngleSignOn invoegtoepassing configuratiepagina**, klikt u op **idp toevoegen**. 

    ![De idp toevoegen](./media/active-directory-saas-bamboo-tutorial/tutorial_bamboo_addidp.png)

11. Op de **Kies uw SAML-identiteitsprovider** pagina de volgende stappen uitvoeren:

    ![De id-provider](./media/active-directory-saas-bamboo-tutorial/tutorial_bamboo_identityprovider.png)

    a. Selecteer **Idp Type** als **AZURE AD**.

    b. In de **naam** textbox, typ de naam.

    c. In de **beschrijving** textbox, typt u de beschrijving.

    d. Klik op **Volgende**.

12. Op de **identiteit providerconfiguratie** pagina op **volgende**.

    ![De configuratie van de identiteit](./media/active-directory-saas-bamboo-tutorial/tutorial_bamboo_identityconfig.png)

13.  Op de **SAML Idp-metagegevens importeren** pagina, klikt u op **bestand laden** voor het uploaden van de **METADATA XML** -bestand dat u hebt gedownload vanuit Azure Portal.

    ![De idpmetadata](./media/active-directory-saas-bamboo-tutorial/tutorial_bamboo_idpmetadata.png)

14. Klik op **Volgende**.

15. Klik op **instellingen opslaan**.

    ![Het opslaan](./media/active-directory-saas-bamboo-tutorial/tutorial_bamboo_save.png)
    
> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-bamboo-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-bamboo-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-bamboo-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-bamboo-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-saml-sso-for-bamboo-by-resolution-gmbh-test-user"></a>Maken van een SAML SSO voor Bamboe door resolutie GmbH testgebruiker

Het doel van deze sectie is het maken van een gebruiker Britta Simon in SAML SSO voor Bamboe aangeroepen door de resolutie GmbH. SAML SSO voor Bamboe door resolutie GmbH ondersteuning biedt voor just-in-time-inrichting en ook gebruikers kunnen handmatig worden gemaakt, neem contact op met [SAML SSO voor Bamboe door resolutie GmbH Client ondersteuningsteam](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) volgens uw vereisten.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon SAML SSO voor Bamboe toegang verlenen door resolutie GmbH gebruikt Azure eenmalige aanmelding.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen SAML SSO voor Bamboe door resolutie GmbH, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SAML SSO voor Bamboe door resolutie GmbH**.

    ![De SAML SSO voor Bamboe door resolutie GmbH koppeling in de lijst met toepassingen](./media/active-directory-saas-bamboo-tutorial/tutorial_bamboo_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op het SAML SSO voor Bamboe door resolutie GmbH tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw SAML SSO voor Bamboe door resolutie GmbH toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bamboo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bamboo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bamboo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bamboo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bamboo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bamboo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bamboo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bamboo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bamboo-tutorial/tutorial_general_203.png

