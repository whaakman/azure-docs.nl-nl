---
title: 'Zelfstudie: Azure Active Directory-integratie met Andromeda | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Andromeda.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7a142c86-ca0c-4915-b1d8-124c08c3e3d8
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: jeedes
ms.openlocfilehash: 047e1ea6a474d95c57ffc2bdff5ad8a5c45e0d36
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048562"
---
# <a name="tutorial-azure-active-directory-integration-with-andromeda"></a>Zelfstudie: Azure Active Directory-integratie met Andromeda

In deze zelfstudie leert u hoe u Andromeda integreren met Azure Active Directory (Azure AD).

Andromeda integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Andromeda heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Andromeda (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Andromeda, moet u de volgende items:

- Een Azure AD-abonnement
- Een Andromeda eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Andromeda uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-andromeda-from-the-gallery"></a>Andromeda uit de galerie toe te voegen
Voor het configureren van de integratie van Andromeda in Azure AD, moet u Andromeda uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Andromeda uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Andromeda**, selecteer **Andromeda** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Andromeda in de lijst met resultaten](./media/andromedascm-tutorial/tutorial_andromedascm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Andromeda op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Andromeda is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Andromeda tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Andromeda, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Andromeda](#create-an-andromeda-test-user)**  : als u wilt een equivalent van Britta Simon in Andromeda die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Andromeda.

**Voor het configureren van Azure AD eenmalige aanmelding met Andromeda, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Andromeda** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/andromedascm-tutorial/tutorial_andromedascm_samlbase.png)

3. Op de **Andromeda domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Andromeda domein en URL's, eenmalige aanmelding informatie](./media/andromedascm-tutorial/tutorial_andromedascm_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<tenantURL>.ngcxpress.com/`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<tenantURL>.ngcxpress.com/SAMLConsumer.aspx`

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Andromeda domein en URL's, eenmalige aanmelding informatie](./media/andromedascm-tutorial/tutorial_andromedascm_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<tenantURL>.ngcxpress.com/SAMLLogon.aspx`
     
    > [!NOTE] 
    > De bovenstaande waarde is geen echte waarde. U kunt de waarde wordt bijgewerkt met de werkelijke-id, de antwoord-URL en aanmeldings-URL die later in de zelfstudie wordt uitgelegd.

5. Andromeda toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Single Sign-On attb configureren](./media/andromedascm-tutorial/tutorial_andromedascm_attribute.png)

    > [!Important]
    > Wissen van de naamruimte-definities tijdens deze instellen.
    
6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | -------------- | -------------------- |    
    | rol        | Specifieke App-rol |
    | type        | Type toevoegen |
    | Bedrijf       | Bedrijfsnaam    |

    > [!NOTE]
    > Er zijn geen echte waarden. Deze waarden zijn alleen voor de demo-doel, gebruik de functies van uw organisatie.

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Configureren van eenmalige aanmelding toevoegen](./media/andromedascm-tutorial/tutorial_attribute_04.png)

    ![Single Sign-On Addattb configureren](./media/andromedascm-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.
    
    e. Klik op **OK**.

7. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/andromedascm-tutorial/tutorial_andromedascm_certificate.png) 

8. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/andromedascm-tutorial/tutorial_general_400.png)
    
9. Op de **Andromeda configuratie** sectie, klikt u op **configureren Andromeda** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Andromeda configuratie](./media/andromedascm-tutorial/tutorial_andromedascm_configure.png)

10. Aanmelding bij uw bedrijf Andromeda site als administrator.

11. Klik boven aan de menubalk op **Admin** en navigeer naar **beheer**.

    ![Andromeda beheerder](./media/andromedascm-tutorial/tutorial_andromedascm_admin.png)

12. Aan de linkerkant van de werkbalk onder **Interfaces** sectie, klikt u op **SAML-configuratie**.

    ![Andromeda saml](./media/andromedascm-tutorial/tutorial_andromedascm_saml.png)

13. Op de **SAML-configuratie** pagina sectie, voert u de volgende stappen uit:

    ![Andromeda config](./media/andromedascm-tutorial/tutorial_andromedascm_config.png)

    a. Controleer **inschakelen van eenmalige aanmelding met SAML**.

    b. Onder **Andromeda informatie** sectie, Kopieer de **SP identiteit** waarde en plak deze in de **id** tekstvak van **Andromeda domein en URL's** sectie.

    c. Kopiëren de **consument URL** waarde en plak deze in de **antwoord-URL** tekstvak van **Andromeda domein en URL's** sectie.

    d. Kopieer de **aanmeldings-URL** waarde en plak deze in de **aanmeldings-URL** tekstvak van **Andromeda domein en URL's** sectie.

    e. Onder **SAML-identiteitsprovider** sectie, typt u de naam van uw id-provider.

    f. In de **eenmalige aanmelding op eindpunt** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.

    g. Open het gedownloade **Base64-gecodeerd certificaat** vanuit Azure portal in Kladblok, plak deze in de **X 509 certificaat** tekstvak.
    
    h. De volgende kenmerken met de betreffende waarde te vergemakkelijken SSO-aanmelding van Azure AD worden toegewezen. De **gebruikers-ID** kenmerk is vereist voor het aanmelden. Voor het inrichten, **e**, **bedrijf**, **UserType**, en **rol** zijn vereist. In deze sectie definiëren we kenmerken toewijzen (naam en de waarden) die gerelateerd aan die zijn gedefinieerd in Azure portal

    ![Andromeda attbmap](./media/andromedascm-tutorial/tutorial_andromedascm_attbmap.png)

    i. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/andromedascm-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/andromedascm-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/andromedascm-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/andromedascm-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-andromeda-test-user"></a>Maak een testgebruiker Andromeda

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Andromeda. Andromeda biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Andromeda als deze nog niet bestaat.

>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Andromeda Client ondersteuningsteam](https://www.ngcsoftware.com/support/).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Andromeda.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Andromeda toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Andromeda**.

    ![De koppeling Andromeda in de lijst met toepassingen](./media/andromedascm-tutorial/tutorial_andromedascm_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Andromeda in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Andromeda.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/andromedascm-tutorial/tutorial_general_01.png
[2]: ./media/andromedascm-tutorial/tutorial_general_02.png
[3]: ./media/andromedascm-tutorial/tutorial_general_03.png
[4]: ./media/andromedascm-tutorial/tutorial_general_04.png

[100]: ./media/andromedascm-tutorial/tutorial_general_100.png

[200]: ./media/andromedascm-tutorial/tutorial_general_200.png
[201]: ./media/andromedascm-tutorial/tutorial_general_201.png
[202]: ./media/andromedascm-tutorial/tutorial_general_202.png
[203]: ./media/andromedascm-tutorial/tutorial_general_203.png
