---
title: 'Zelfstudie: Azure Active Directory-integratie met Zscaler particuliere Access-beheerder | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zscaler particuliere Access-beheerder.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c87392a7-e7fe-4cdc-a8e6-afe1ed975172
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: jeedes
ms.openlocfilehash: 0be7f71f5995be269bb630491003678e0640699b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39055165"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-administrator"></a>Zelfstudie: Azure Active Directory-integratie met Zscaler persoonlijke toegang beheerder

In deze zelfstudie leert u hoe u Zscaler persoonlijke toegang beheerder integreren met Azure Active Directory (Azure AD).

Zscaler persoonlijke toegang beheerder integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die naar Zscaler persoonlijke toegang beheerder toegang heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld Zscaler persoonlijke toegang beheerder (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zscaler particuliere Access-beheerder, moet u de volgende items:

- Een Azure AD-abonnement
- Een beheerder van de Zscaler persoonlijke toegang eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Beheerder van de Zscaler persoonlijke toegang toe te voegen uit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zscaler-private-access-administrator-from-the-gallery"></a>Beheerder van de Zscaler persoonlijke toegang toe te voegen uit de galerie
Voor het configureren van de integratie van de Zscaler particuliere Access-beheerder in Azure AD, moet u Zscaler persoonlijke toegang beheerder uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Zscaler particuliere Access-beheerder van de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak typt, **Zscaler persoonlijke toegang beheerder**, selecteer **Zscaler particuliere Access-beheerder** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![Beheerder van de Zscaler persoonlijke toegang in de lijst met resultaten](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Zscaler particuliere beheerder toegang op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de gebruiker equivalent in Zscaler persoonlijke toegang beheerder is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker beheerder in Zscaler persoonlijke toegang tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Zscaler particuliere Access-beheerder, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Zscaler persoonlijke toegang beheerder](#create-a-zscaler-private-access-administrator-test-user)**  - hebben een equivalent van Britta Simon in Zscaler persoonlijke toegang die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Zscaler particuliere Access-beheerder.

**Voor het configureren van Azure AD eenmalige aanmelding met Zscaler particuliere Access-beheerder, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Zscaler persoonlijke toegang beheerder** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_samlbase.png)

3. Op de **Zscaler persoonlijke toegang beheerder domein en URL's** sectie als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Zscaler persoonlijke toegang beheerder domein en URL's eenmalige aanmelding informatie](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.private.zscaler.com/auth/metadata`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.private.zscaler.com/auth/sso`

    c. Controleer **geavanceerde URL-instellingen weergeven**

    d. In de **RelayState** tekstvak, een waarde: `idpadminsso`

4.  Als u wilt configureren van de toepassing in **SP** gestart modus de volgende stappen uitvoeren:

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.private.zscaler.com/auth/sso`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [Zscaler persoonlijke toegang beheerder ondersteuningsteam](https://help.zscaler.com/zpa-submit-ticket) om deze waarden te verkrijgen.
 
5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_certificate.png) 

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_400.png)

7. In een ander browservenster, aanmelding Zscaler persoonlijke toegang beheerder als beheerder.

8. Klik op de bovenkant op **beheer** en navigeer naar **verificatie** sectie Klik **IdP-configuratie**.

    ![Beheerder van de Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

9. Klik in de rechterbovenhoek, **IdP-configuratie toevoegen**. 

    ![Addidp Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addpidp.png)

10. Op de **IdP-configuratie toevoegen** pagina de volgende stappen uitvoeren:
 
    ![Idpselect Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_idpselect.png)

    a. Klik op **bestand selecteren** voor het uploaden van het gedownloade bestand met metagegevens van Azure AD in de **IdP metagegevens bestand uploaden** veld.

    b. Is het ingesteld op de **IdP metagegevens** van Azure AD en alle informatie van de velden gevuld, zoals hieronder wordt weergegeven.

    ![Idpconfig Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/idpconfig.png)

    c. Selecteer **eenmalige aanmelding** als **beheerder**.

    d. Selecteer uw domein uit **domeinen** veld.
    
    e. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-zscaler-private-access-administrator-test-user"></a>Maak een testgebruiker Zscaler persoonlijke toegang beheerder

Als u wilt dat Azure AD-gebruikers zich aanmelden voor persoonlijke Zscaler Access-beheerder, als ze in Zscaler persoonlijke toegang beheerder worden ingericht. In het geval van Zscaler persoonlijke toegang Administrator inrichten is een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Zscaler persoonlijke toegang beheerder site aan als beheerder.

2. Klik op de bovenkant op **beheer** en navigeer naar **verificatie** sectie Klik **IdP-configuratie**.

    ![Beheerder van de Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

3. Klik op **beheerders** vanaf de linkerkant van het menu.

    ![Beheerder van de Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_adminstrator.png)

4. Klik in de rechterbovenhoek, **beheerder toevoegen**:

    ![Beheerder van de Zscaler persoonlijke toegang beheerder toevoegen](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addadmin.png)

5. In de **beheerder toevoegen** pagina, voert u de volgende stappen uit:

    ![Gebruikersbeheerder Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_useradmin.png)

    a. In de **gebruikersnaam** tekstvak, voer het e-mailadres van gebruiker, zoals **BrittaSimon@contoso.com**.

    b. In de **wachtwoord** tekstvak typt u het wachtwoord.

    c. In de **wachtwoord bevestigen** tekstvak typt u het wachtwoord.

    d. Selecteer **rol** als **Zscaler persoonlijke toegang beheerder**.

    e. In de **e** tekstvak, voer het e-mailadres van gebruiker, zoals **BrittaSimon@contoso.com**.

    f. In de **Phone** tekstvak typt u het telefoonnummer.

    g. In de **tijdzone** textbox, selecteert u de tijdzone.

    h. Klik op **Opslaan**.  

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Zscaler persoonlijke toegang beheerder.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Zscaler persoonlijke toegang beheerder, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Zscaler persoonlijke toegang beheerder**.

    ![De koppeling Zscaler particuliere Access-beheerder in de lijst met toepassingen](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Zscaler particuliere Access-beheerder in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Zscaler particuliere Access-beheerder.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_01.png
[2]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_02.png
[3]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_03.png
[4]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_04.png

[100]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_100.png

[200]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_200.png
[201]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_201.png
[202]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_202.png
[203]: ./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_203.png

