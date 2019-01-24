---
title: 'Zelfstudie: Azure Active Directory-integratie met ScaleX Enterprise | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ScaleX Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: fb01aec04af10d3e413213dc9821a27b42a0a9d2
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54822974"
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met ScaleX Enterprise

In deze zelfstudie leert u hoe u ScaleX Enterprise integreren met Azure Active Directory (Azure AD).

ScaleX Enterprise integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ScaleX Enterprise heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij ScaleX Enterprise (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, raadpleegt u. Wat is toegang tot toepassingen en eenmalige aanmelding met [Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ScaleX Enterprise, moet u de volgende items:

- Een Azure AD-abonnement
- Een ScaleX Enterprise eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet uw productieomgeving, tenzij dit noodzakelijk is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ScaleX Enterprise uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-scalex-enterprise-from-the-gallery"></a>ScaleX Enterprise uit de galerie toe te voegen
Voor het configureren van de integratie van ScaleX Enterprise in Azure AD, moet u ScaleX Enterprise uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ScaleX Enterprise uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **ScaleX Enterprise**.

    ![Het maken van een Azure AD-testgebruiker](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

1. Selecteer in het deelvenster resultaten **ScaleX Enterprise**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met ScaleX Enterprise op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de onderneming ScaleX is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de onderneming ScaleX tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in ScaleX onderneming.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met ScaleX Enterprise, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker ScaleX Enterprise](#creating-a-scalex-enterprise-test-user)**  : als u wilt een equivalent van Britta Simon in ScaleX-onderneming die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing ScaleX Enterprise.

**Voor het configureren van Azure AD eenmalige aanmelding met ScaleX Enterprise, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ScaleX Enterprise** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

1. Op de **ScaleX Enterprise domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    a. In de **id** tekstvak typt u de waarde met behulp van het volgende patroon: `https://platform.rescale.com/saml2/<company id>/`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://platform.rescale.com/saml2/<company id>/acs/`

1. Controleer **geavanceerde URL-instellingen weergeven**, als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    In de **aanmeldings-URL** tekstvak typt u de waarde met behulp van het volgende patroon: `https://platform.rescale.com/saml2/<company id>/sso/`
     
    > [!NOTE] 
    > Dit zijn niet de werkelijke waarden. Werk deze waarden met de werkelijke-id, de antwoord-URL of aanmeldings-URL. Neem contact op met [ScaleX Enterprise Client-ondersteuningsteam](https://info.rescale.com/contact_sales) om deze waarden te verkrijgen. 

1. Uw toepassing ScaleX wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u het aangepaste kenmerktoewijzingen aan de configuratie van de SAML-token kenmerken wijzigen. Klik op **weergeven en bewerk alle andere gebruikerskenmerken** selectievakje in om te openen van de aangepaste kenmerken van instellingen.

    ![Eenmalige aanmelding configureren](./media/scalexenterprise-tutorial/scalex_attributes.png)
    
    a. Klik met de rechtermuisknop op het kenmerk **naam** en klikt u op verwijderen.

    ![Eenmalige aanmelding configureren](./media/scalexenterprise-tutorial/delete_attribute_name.png)

    b. Klik op **emailaddress** kenmerk om het kenmerk bewerken-venster te openen. Wijzig de waarde van **user.mail** naar **user.userprincipalname** en klik op Ok.

    ![Eenmalige aanmelding configureren](./media/scalexenterprise-tutorial/edit_email_attribute.png) 
    
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/scalexenterprise-tutorial/tutorial_general_400.png)
    
1. Op de **ScaleX Ondernemingsconfiguratie** sectie, klikt u op **configureren ScaleX Enterprise** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID** en **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

1. Het configureren van eenmalige aanmelding op **ScaleX Enterprise** side, meld u aan bij de website van het bedrijf ScaleX Enterprise als beheerder.

1. Klik in het menu in de rechterbovenhoek en selecteer **Contoso beheer**.

    > [!NOTE] 
    > Contoso is slechts een voorbeeld. Dit moet de werkelijke naam van uw bedrijf. 

    ![Eenmalige aanmelding configureren](./media/scalexenterprise-tutorial/Test_Admin.png) 

1. Selecteer **integraties** in het bovenste menu en selecteer **Single Sign-On**.

    ![Eenmalige aanmelding configureren](./media/scalexenterprise-tutorial/admin_sso.png) 

1. Vul het formulier als volgt:

    ![Eenmalige aanmelding configureren](./media/scalexenterprise-tutorial/scalex_admin_save.png) 
    
    a. Selecteer **'Een gebruiker die zich kan verifiëren met eenmalige aanmelding maken'.**

    b. **Service Provider saml**: Plak de waarde ***urn: oasis: namen: tc: SAML:2.0:nameid-indeling: permanente***

    c. **Naam van id-Provider e-veld in de ACS-antwoord**: Plak de waarde `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. **ID-Provider EntityDescriptor entiteit-ID:** Plak de **SAML entiteit-ID** waarde opgehaald uit de Azure-portal.

    e. **URL van SingleSignOnService de id-Provider:** Plak de **Single Sign-On Service URL voor SAML** vanuit Azure portal.

    f. **Provider van openbare X509 identiteitscertificaat:** Open de X509 certificaat gedownload vanuit de Azure in Kladblok en plak de inhoud in dit vak. Controleer of er zijn dat geen regeleinden in het midden van de certificaatinhoud.
    
    g. Schakel de selectievakjes in de volgende: **NameID ingeschakeld, coderen en meld u aan AuthnRequests.**

    h. Klik op **Update SSO-instellingen** de instellingen op te slaan.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/scalexenterprise-tutorial/create_aaduser_01.png) 

1. Ga naar **gebruikers en groepen** en klikt u op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Het maken van een Azure AD-testgebruiker](./media/scalexenterprise-tutorial/create_aaduser_02.png) 

1. Aan de bovenkant van het dialoogvenster, klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/scalexenterprise-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/scalexenterprise-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-scalex-enterprise-test-user"></a>Het maken van een testgebruiker ScaleX Enterprise

Om Azure AD-gebruikers zich aanmelden bij ScaleX Enterprise, moeten ze worden ingericht met ScaleX Enterprise. In het geval van ScaleX Enterprise, een automatische taak inrichten is en er zijn geen handmatige stappen vereist. Elke gebruiker die kan worden geverifieerd met referenties voor eenmalige aanmelding wordt automatisch aan de ScaleX worden ingericht.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door gebruikerstoegang verlenen tot ScaleX Enterprise.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar ScaleX Enterprise, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **ScaleX Enterprise**.

    ![Eenmalige aanmelding configureren](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Klik op de tegel ScaleX Enterprise in het toegangsvenster, u wordt u automatisch aangemeld bij uw toepassing ScaleX Enterprise. Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.


## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/scalexenterprise-tutorial/tutorial_general_203.png

