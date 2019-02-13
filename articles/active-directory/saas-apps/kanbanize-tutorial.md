---
title: 'Zelfstudie: Azure Active Directory-integratie met Kanbanize | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Kanbanize.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b436d2f6-bfa5-43fd-a8f9-d2144dc25669
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 22c136225e5a8526afd482e5ef8400198947422f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56199677"
---
# <a name="tutorial-azure-active-directory-integration-with-kanbanize"></a>Zelfstudie: Azure Active Directory-integratie met Kanbanize

In deze zelfstudie leert u hoe u Kanbanize integreren met Azure Active Directory (Azure AD).

Kanbanize integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Kanbanize heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Kanbanize (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Kanbanize, moet u de volgende items:

- Een Azure AD-abonnement
- Een Kanbanize eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Kanbanize uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-kanbanize-from-the-gallery"></a>Kanbanize uit de galerie toe te voegen
Voor het configureren van de integratie van Kanbanize in Azure AD, moet u Kanbanize uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Kanbanize uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Kanbanize**, selecteer **Kanbanize** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Kanbanize in de lijst met resultaten](./media/kanbanize-tutorial/tutorial_kanbanize_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Kanbanize op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Kanbanize is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Kanbanize tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Kanbanize, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker Kanbanize](#create-a-kanbanize-test-user)**  : als u wilt een equivalent van Britta Simon in Kanbanize die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Kanbanize.

**Voor het configureren van Azure AD eenmalige aanmelding met Kanbanize, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Kanbanize** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/kanbanize-tutorial/tutorial_kanbanize_samlbase.png)

3. Op de **Kanbanize domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Kanbanize domein en URL's, eenmalige aanmelding informatie](./media/kanbanize-tutorial/tutorial_kanbanize_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<subdomain>.kanbanize.com/`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<subdomain>.kanbanize.com/saml/acs`

    c. Controleer **geavanceerde URL-instellingen weergeven**.

    d.  In de **Relaystatus** tekstvak, een URL typen: `/ctrl_login/saml_login`

    e. Als u wilt configureren van de toepassing in **SP** modus gestart **aanmeldings-URL** tekstvak typt u een URL met behulp van het volgende patroon: `https://<subdomain>.kanbanize.com`
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [Kanbanize Client ondersteuningsteam](mailto:support@ms.kanbanize.com) om deze waarden te verkrijgen. 

5. Kanbanize toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermopname ziet u een voorbeeld hiervan. De standaardwaarde van **gebruikers-id** is **user.userprincipalname** maar Kanbanize wordt verwacht dat deze optie om te worden toegewezen met de e-mailadres van de gebruiker. Hiervoor kunt u **user.mail** kenmerk in de lijst of gebruik de waarde van het juiste kenmerk op basis van de organisatieconfiguratie van uw
    
    ![Eenmalige aanmelding configureren](./media/kanbanize-tutorial/tutorial_Kanbanize_attribute.png)

6. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/kanbanize-tutorial/tutorial_kanbanize_certificate.png) 

7. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/kanbanize-tutorial/tutorial_general_400.png)
    
8. Op de **Kanbanize configuratie** sectie, klikt u op **configureren Kanbanize** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Kanbanize configuratie](./media/kanbanize-tutorial/tutorial_kanbanize_configure.png)

9. In een ander browservenster, meld u aan bij Kanbanize als een beveiligingsbeheerder. 

10. Ga naar rechtsboven in de pagina, klikt u op **instellingen** logo.

    ![Instellingen voor Kanbanize](./media/kanbanize-tutorial/tutorial_kanbanize_set.png)

11. Klik op de pagina van het deelvenster Beheer menu aan de linkerkant **integraties** en schakel vervolgens **Single Sign-On**. 

    ![Kanbanize integraties](./media/kanbanize-tutorial/tutorial_kanbanize_admin.png)

12. Integraties sectie, klikt u op **configureren** openen **Single Sign-On integratie** pagina.

    ![Kanbanize config](./media/kanbanize-tutorial/tutorial_kanbanize_config.png)

13. Op de **Single Sign-On integratie** pagina onder **configuraties**, voer de volgende stappen uit:

    ![Kanbanize integraties](./media/kanbanize-tutorial/tutorial_kanbanize_save.png)

    a. In de **Idp entiteit-Id** tekstvak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal.

    b. In de **Idp-eindpunt voor aanmelding** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    c. In de **Idp Afmeldingseindpunt** tekstvak, plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.

    d. In **kenmerknaam voor e-mailbericht** tekstvak, geef deze waarde `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    e. In **kenmerknaam voornaam** tekstvak, geef deze waarde `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`

    f. In **kenmerknaam achternaam** tekstvak, geef deze waarde `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` 
    > [!Note]
    > U kunt deze waarden kunt krijgen door een combinatie van waarden van naamruimte en de naam van het bijbehorende kenmerk uit de sectie van de gebruiker kenmerken in Azure portal.

    g. In Kladblok, opent u het base-64 gecodeerde certificaat dat u hebt gedownload vanuit Azure portal, Kopieer de inhoud (zonder de begin- en markeringen) en plak deze in de **Idp X.509-certificaat** vak.

    h. Controleer **aanmelding inschakelen met eenmalige aanmelding en Kanbanize**.
    
    i. Klik op **instellingen opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/kanbanize-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/kanbanize-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/kanbanize-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/kanbanize-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-kanbanize-test-user"></a>Maak een testgebruiker Kanbanize

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Kanbanize. Kanbanize biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Kanbanize als deze nog niet bestaat.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Kanbanize Client ondersteuningsteam](mailto:support@ms.kanbanize.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Kanbanize.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Kanbanize toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Kanbanize**.

    ![De koppeling Kanbanize in de lijst met toepassingen](./media/kanbanize-tutorial/tutorial_kanbanize_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Kanbanize in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Kanbanize.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/kanbanize-tutorial/tutorial_general_01.png
[2]: ./media/kanbanize-tutorial/tutorial_general_02.png
[3]: ./media/kanbanize-tutorial/tutorial_general_03.png
[4]: ./media/kanbanize-tutorial/tutorial_general_04.png

[100]: ./media/kanbanize-tutorial/tutorial_general_100.png

[200]: ./media/kanbanize-tutorial/tutorial_general_200.png
[201]: ./media/kanbanize-tutorial/tutorial_general_201.png
[202]: ./media/kanbanize-tutorial/tutorial_general_202.png
[203]: ./media/kanbanize-tutorial/tutorial_general_203.png

