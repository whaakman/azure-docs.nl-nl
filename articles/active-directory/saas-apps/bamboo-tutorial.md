---
title: 'Zelfstudie: Azure Active Directory-integratie met SAML SSO voor Bamboe resolutie GmbH | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAML SSO voor Bamboe resolutie GmbH.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: f00160c7-f4cc-43bf-af18-f04168d3767c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: jeedes
ms.openlocfilehash: 027f5a9f02a0580fce61091e8be9ece9069fb34f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55156170"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bamboo-by-resolution-gmbh"></a>Zelfstudie: Azure Active Directory-integratie met SAML SSO voor Bamboe resolutie GmbH

In deze zelfstudie leert u over het integreren van SAML SSO voor Bamboe resolutie GmbH met Azure Active Directory (Azure AD).

Integratie van SAML SSO voor Bamboe resolutie GmbH met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SAML SSO voor Bamboe resolutie GmbH heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SAML SSO voor Bamboe door resolutie GmbH (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAML SSO voor Bamboe resolutie GmbH, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAML SSO voor Bamboe resolutie GmbH eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van SAML SSO voor Bamboe resolutie GmbH uit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-saml-sso-for-bamboo-by-resolution-gmbh-from-the-gallery"></a>Toevoegen van SAML SSO voor Bamboe resolutie GmbH uit de galerie
Voor het configureren van de integratie van SAML SSO voor Bamboe resolutie GmbH in Azure AD, moet u de SAML SSO voor Bamboe resolutie GmbH uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**SAML SSO voor Bamboe met resolutie GmbH uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **SAML SSO voor Bamboe resolutie GmbH**, selecteer **SAML SSO voor Bamboe resolutie GmbH** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![SAML SSO voor Bamboe door oplossing GmbH in de lijst met resultaten](./media/bamboo-tutorial/tutorial_bamboo_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SAML SSO voor Bamboe door resolutie die GmbH op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de equivalente gebruiker in de SAML SSO voor Bamboe resolutie GmbH aan een gebruiker in Azure AD is. Met andere woorden, een koppeling de relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de SAML SSO voor Bamboe resolutie GmbH moet tot stand worden gebracht.

In de SAML SSO voor Bamboe resolutie GmbH, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met SAML SSO voor Bamboe resolutie GmbH, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maken van een SAML SSO voor Bamboe door resolutie GmbH testgebruiker](#create-a-saml-sso-for-bamboo-by-resolution-gmbh-test-user)**  : als u wilt een equivalent van Britta Simon hebben in de SAML SSO voor Bamboe resolutie GmbH die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en eenmalige aanmelding in de SAML SSO voor Bamboe configureren door de resolutie GmbH toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met SAML SSO voor Bamboe resolutie GmbH, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SAML SSO voor Bamboe resolutie GmbH** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/bamboo-tutorial/tutorial_bamboo_samlbase.png)

1. Op de **SAML SSO voor Bamboe resolutie GmbH domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in de modus voor IDP gestart:

    ![SAML SSO voor Bamboe resolutie GmbH domein en URL's, eenmalige aanmelding informatie](./media/bamboo-tutorial/tutorial_bamboo_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<server-base-url>/plugins/servlet/samlsso`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<server-base-url>/plugins/servlet/samlsso`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![SAML SSO voor Bamboe resolutie GmbH domein en URL's, eenmalige aanmelding informatie](./media/bamboo-tutorial/tutorial_bamboo_url1.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [SAML SSO voor Bamboe resolutie GmbH Client ondersteuningsteam](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/bamboo-tutorial/tutorial_bamboo_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/bamboo-tutorial/tutorial_general_400.png)

1. Aanmelding bij de SAML SSO voor Bamboe door resolutie GmbH bedrijf site als administrator.

1. Aan de rechterkant van de belangrijkste werkbalk, klikt u op **instellingen** > **invoegtoepassingen**.

    ![De instellingen](./media/bamboo-tutorial/tutorial_bamboo_setings.png)

1. Ga naar de sectie beveiliging, klikt u op **SAML SingleSignOn** op de menubalk.

    ![De Samlsingle](./media/bamboo-tutorial/tutorial_bamboo_samlsingle.png)

1. Op de **SAML SIngleSignOn invoegtoepassing configuratiepagina**, klikt u op **toevoegen idp**. 

    ![De id-provider toevoegen](./media/bamboo-tutorial/tutorial_bamboo_addidp.png)

1. Op de **kiest u de SAML-identiteitsprovider** pagina, de volgende stappen uitvoeren:

    ![De id-provider](./media/bamboo-tutorial/tutorial_bamboo_identityprovider.png)

    a. Selecteer **Idp Type** als **AZURE AD**.

    b. In de **naam** tekstvak typt u de naam.

    c. In de **beschrijving** tekstvak typt u de beschrijving.

    d. Klik op **volgende**.

1. Op de **id-providerconfiguratie** pagina op **volgende**.

    ![De configuratie van de identiteit](./media/bamboo-tutorial/tutorial_bamboo_identityconfig.png)

1.  Op de **metagegevens importeren SAML-id-provider** pagina, klikt u op **bestand laden** het uploaden van de **METADATA XML** -bestand dat u hebt gedownload vanuit Azure Portal.

    ![De idpmetadata](./media/bamboo-tutorial/tutorial_bamboo_idpmetadata.png)

1. Klik op **volgende**.

1. Klik op **Instellingen opslaan**.

    ![Het opslaan](./media/bamboo-tutorial/tutorial_bamboo_save.png)
    
> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/bamboo-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/bamboo-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/bamboo-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/bamboo-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-saml-sso-for-bamboo-by-resolution-gmbh-test-user"></a>Een SAML SSO voor Bamboe door resolutie GmbH testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in SAML SSO voor Bamboe resolutie GmbH. SAML SSO voor Bamboe resolutie GmbH biedt ondersteuning voor just-in-time inrichting en ook gebruikers kunnen handmatig worden gemaakt, neem contact op met [SAML SSO voor Bamboe resolutie GmbH Client ondersteuningsteam](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support) volgens uw behoeften.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SAML SSO voor Bamboe resolutie GmbH.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon toewijzen aan SAML SSO voor Bamboe met resolutie GmbH, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **SAML SSO voor Bamboe resolutie GmbH**.

    ![De SAML SSO voor Bamboe door resolutie GmbH koppeling in de lijst met toepassingen](./media/bamboo-tutorial/tutorial_bamboo_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u de SAML SSO voor Bamboe door resolutie GmbH tegel in het toegangsvenster, u moet u automatisch aangemeld bij de SAML SSO voor Bamboe resolutie GmbH toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/bamboo-tutorial/tutorial_general_01.png
[2]: ./media/bamboo-tutorial/tutorial_general_02.png
[3]: ./media/bamboo-tutorial/tutorial_general_03.png
[4]: ./media/bamboo-tutorial/tutorial_general_04.png

[100]: ./media/bamboo-tutorial/tutorial_general_100.png

[200]: ./media/bamboo-tutorial/tutorial_general_200.png
[201]: ./media/bamboo-tutorial/tutorial_general_201.png
[202]: ./media/bamboo-tutorial/tutorial_general_202.png
[203]: ./media/bamboo-tutorial/tutorial_general_203.png

