---
title: 'Zelfstudie: Azure Active Directory-integratie met SAML SSO voor Bitbucket resolutie GmbH | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAML SSO voor Bitbucket resolutie GmbH.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: fc947df1-f24e-43ae-9a34-518293583d69
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: jeedes
ms.openlocfilehash: 21b6adb98fdb973b75aa1d6db519777bab730d73
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048120"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bitbucket-by-resolution-gmbh"></a>Zelfstudie: Azure Active Directory-integratie met SAML SSO voor Bitbucket resolutie GmbH

In deze zelfstudie leert u over het integreren van SAML SSO voor Bitbucket resolutie GmbH met Azure Active Directory (Azure AD).

Integratie van SAML SSO voor Bitbucket resolutie GmbH met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SAML SSO voor Bitbucket resolutie GmbH heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SAML SSO voor Bitbucket door resolutie GmbH (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAML SSO voor Bitbucket resolutie GmbH, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAML SSO voor Bitbucket resolutie GmbH eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van SAML SSO voor Bitbucket resolutie GmbH uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-saml-sso-for-bitbucket-by-resolution-gmbh-from-the-gallery"></a>Toevoegen van SAML SSO voor Bitbucket resolutie GmbH uit de galerie
Voor het configureren van de integratie van SAML SSO voor Bitbucket resolutie GmbH in Azure AD, moet u de SAML SSO voor Bitbucket resolutie GmbH uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**SAML SSO voor Bitbucket met resolutie GmbH uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **SAML SSO voor Bitbucket resolutie GmbH**, selecteer **SAML SSO voor Bitbucket resolutie GmbH** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![SAML SSO voor Bitbucket door oplossing GmbH in de lijst met resultaten](./media/bitbucket-tutorial/tutorial_bitbucket_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SAML SSO voor Bitbucket door resolutie die GmbH op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de equivalente gebruiker in de SAML SSO voor Bitbucket resolutie GmbH aan een gebruiker in Azure AD is. Met andere woorden, een koppeling de relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de SAML SSO voor Bitbucket resolutie GmbH moet tot stand worden gebracht.

In de SAML SSO voor Bitbucket resolutie GmbH, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met SAML SSO voor Bitbucket resolutie GmbH, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een SAML SSO voor Bitbucket door resolutie GmbH testgebruiker](#create-a-saml-sso-for-bitbucket-by-resolution-gmbh-test-user)**  : als u wilt een equivalent van Britta Simon hebben in de SAML SSO voor Bitbucket resolutie GmbH die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en eenmalige aanmelding in de SAML SSO voor Bitbucket configureren door de resolutie GmbH toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met SAML SSO voor Bitbucket resolutie GmbH, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SAML SSO voor Bitbucket resolutie GmbH** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/bitbucket-tutorial/tutorial_bitbucket_samlbase.png)

3. Op de **SAML SSO voor Bitbucket resolutie GmbH domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in de modus voor IDP gestart:

    ![SAML SSO voor Bitbucket resolutie GmbH domein en URL's, eenmalige aanmelding informatie](./media/bitbucket-tutorial/tutorial_bitbucket_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<server-base-url>/plugins/servlet/samlsso`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<server-base-url>/plugins/servlet/samlsso`

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![SAML SSO voor Bitbucket resolutie GmbH domein en URL's, eenmalige aanmelding informatie](./media/bitbucket-tutorial/tutorial_bitbucket_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [SAML SSO voor Bitbucket resolutie GmbH Client ondersteuningsteam](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bitbucket/server/support) om deze waarden te verkrijgen. 

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/bitbucket-tutorial/tutorial_bitbucket_certificate.png) 

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/bitbucket-tutorial/tutorial_general_400.png)
    
7. Aanmelding bij de SAML SSO voor Bitbucket door resolutie GmbH bedrijf site als administrator.

8. Aan de rechterkant van de belangrijkste werkbalk, klikt u op **instellingen**.

9. Ga naar accountsectie, klikt u op **SAML SingleSignOn** op de menubalk.

    ![De Samlsingle](./media/bitbucket-tutorial/tutorial_bitbucket_samlsingle.png)

10. Op de **SAML SIngleSignOn invoegtoepassing configuratiepagina**, klikt u op **toevoegen idp**. 

    ![De id-provider toevoegen](./media/bitbucket-tutorial/tutorial_bitbucket_addidp.png)

11. Op de **kiest u de SAML-identiteitsprovider** pagina, de volgende stappen uitvoeren:

    ![De id-provider](./media/bitbucket-tutorial/tutorial_bitbucket_identityprovider.png)

    a. Selecteer **Idp Type** als **AZURE AD**.

    b. In de **naam** tekstvak typt u de naam.

    c. In de **beschrijving** tekstvak typt u de beschrijving.

    d. Klik op **Volgende**.

12. Op de **Identity provider configuratiepagina**, klikt u op **volgende**.

    ![De configuratie van de identiteit](./media/bitbucket-tutorial/tutorial_bitbucket_identityconfig.png)

13.  Op de **metagegevens importeren SAML-id-provider** pagina, klikt u op **bestand laden** het uploaden van de **METADATA XML** -bestand dat u hebt gedownload vanuit Azure portal.

    ![De idpmetadata](./media/bitbucket-tutorial/tutorial_bitbucket_idpmetadata.png)
    
14. Klik op **Volgende**.

15. Klik op **instellingen opslaan**.

    ![Het opslaan](./media/bitbucket-tutorial/tutorial_bitbucket_save.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/bitbucket-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/bitbucket-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/bitbucket-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/bitbucket-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-saml-sso-for-bitbucket-by-resolution-gmbh-test-user"></a>Een SAML SSO voor Bitbucket door resolutie GmbH testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in SAML SSO voor Bitbucket resolutie GmbH. SAML SSO voor Bitbucket resolutie GmbH biedt ondersteuning voor just-in-time inrichting en ook gebruikers kunnen handmatig worden gemaakt, neem contact op met [SAML SSO voor Bitbucket resolutie GmbH Client ondersteuningsteam](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bitbucket/server/support) volgens uw behoeften.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SAML SSO voor Bitbucket resolutie GmbH.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon toewijzen aan SAML SSO voor Bitbucket met resolutie GmbH, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **SAML SSO voor Bitbucket resolutie GmbH**.

    ![De SAML SSO voor Bitbucket door resolutie GmbH koppeling in de lijst met toepassingen](./media/bitbucket-tutorial/tutorial_bitbucket_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u de SAML SSO voor Bitbucket op door het probleem zou moeten GmbH tegel in het toegangsvenster, u moet u automatisch aangemeld bij de SAML SSO voor Bitbucket resolutie GmbH toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bitbucket-tutorial/tutorial_general_01.png
[2]: ./media/bitbucket-tutorial/tutorial_general_02.png
[3]: ./media/bitbucket-tutorial/tutorial_general_03.png
[4]: ./media/bitbucket-tutorial/tutorial_general_04.png

[100]: ./media/bitbucket-tutorial/tutorial_general_100.png

[200]: ./media/bitbucket-tutorial/tutorial_general_200.png
[201]: ./media/bitbucket-tutorial/tutorial_general_201.png
[202]: ./media/bitbucket-tutorial/tutorial_general_202.png
[203]: ./media/bitbucket-tutorial/tutorial_general_203.png

