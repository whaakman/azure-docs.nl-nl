---
title: 'Zelfstudie: Azure Active Directory-integratie met Zscaler persoonlijke toegang Administrator | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zscaler particuliere Access-beheerder.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c87392a7-e7fe-4cdc-a8e6-afe1ed975172
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: jeedes
ms.openlocfilehash: 6eca534f933742e4797ca164637e7d536d194ba3
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55181500"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-administrator"></a>Zelfstudie: Azure Active Directory-integratie met Zscaler persoonlijke toegang beheerder

In deze zelfstudie leert u hoe u Zscaler persoonlijke toegang beheerder integreren met Azure Active Directory (Azure AD).

Zscaler persoonlijke toegang beheerder integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die naar Zscaler persoonlijke toegang beheerder toegang heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld Zscaler persoonlijke toegang beheerder (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zscaler particuliere Access-beheerder, moet u de volgende items:

- Een Azure AD-abonnement
- Een beheerder van de Zscaler persoonlijke toegang eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Beheerder van de Zscaler persoonlijke toegang toe te voegen uit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zscaler-private-access-administrator-from-the-gallery"></a>Beheerder van de Zscaler persoonlijke toegang toe te voegen uit de galerie
Voor het configureren van de integratie van de Zscaler particuliere Access-beheerder in Azure AD, moet u Zscaler persoonlijke toegang beheerder uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Zscaler particuliere Access-beheerder van de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak typt, **Zscaler persoonlijke toegang beheerder**, selecteer **Zscaler particuliere Access-beheerder** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![Beheerder van de Zscaler persoonlijke toegang in de lijst met resultaten](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Zscaler particuliere beheerder toegang op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de gebruiker equivalent in Zscaler persoonlijke toegang beheerder is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker beheerder in Zscaler persoonlijke toegang tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Zscaler particuliere Access-beheerder, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Zscaler persoonlijke toegang beheerder](#create-a-zscaler-private-access-administrator-test-user)**  - hebben een equivalent van Britta Simon in Zscaler persoonlijke toegang die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Zscaler particuliere Access-beheerder.

**Voor het configureren van Azure AD eenmalige aanmelding met Zscaler particuliere Access-beheerder, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Zscaler persoonlijke toegang beheerder** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_samlbase.png)

1. Op de **Zscaler persoonlijke toegang beheerder domein en URL's** sectie als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Zscaler persoonlijke toegang beheerder domein en URL's eenmalige aanmelding informatie](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<subdomain>.private.zscaler.com/auth/metadata`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<subdomain>.private.zscaler.com/auth/sso`

    c. Controleer **geavanceerde URL-instellingen weergeven**

    d. In de **RelayState** tekstvak, een waarde: `idpadminsso`

1.  Als u wilt configureren van de toepassing in **SP** gestart modus de volgende stappen uitvoeren:

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<subdomain>.private.zscaler.com/auth/sso`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke-id, de antwoord-URL en de aanmeldings-URL. Neem contact op met [Zscaler persoonlijke toegang beheerder ondersteuningsteam](https://help.zscaler.com/zpa-submit-ticket) om deze waarden te verkrijgen.
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/zscalerprivateaccessadministrator-tutorial/tutorial_general_400.png)

1. In een ander browservenster, aanmelding Zscaler persoonlijke toegang beheerder als beheerder.

1. Klik op de bovenkant op **beheer** en navigeer naar **verificatie** sectie Klik **IdP-configuratie**.

    ![Beheerder van de Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

1. Klik in de rechterbovenhoek, **IdP-configuratie toevoegen**. 

    ![Addidp Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addpidp.png)

1. Op de **IdP-configuratie toevoegen** pagina de volgende stappen uitvoeren:
 
    ![Idpselect Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_idpselect.png)

    a. Klik op **bestand selecteren** voor het uploaden van het gedownloade bestand met metagegevens van Azure AD in de **IdP metagegevens bestand uploaden** veld.

    b. Is het ingesteld op de **IdP metagegevens** van Azure AD en alle informatie van de velden gevuld, zoals hieronder wordt weergegeven.

    ![Idpconfig Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/idpconfig.png)

    c. Selecteer **eenmalige aanmelding** als **beheerder**.

    d. Selecteer uw domein uit **domeinen** veld.
    
    e. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/zscalerprivateaccessadministrator-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-zscaler-private-access-administrator-test-user"></a>Maak een testgebruiker Zscaler persoonlijke toegang beheerder

Als u wilt dat Azure AD-gebruikers zich aanmelden voor persoonlijke Zscaler Access-beheerder, als ze in Zscaler persoonlijke toegang beheerder worden ingericht. In het geval van Zscaler persoonlijke toegang Administrator inrichten is een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw bedrijf Zscaler persoonlijke toegang beheerder site aan als beheerder.

1. Klik op de bovenkant op **beheer** en navigeer naar **verificatie** sectie Klik **IdP-configuratie**.

    ![Beheerder van de Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

1. Klik op **beheerders** vanaf de linkerkant van het menu.

    ![Beheerder van de Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_adminstrator.png)

1. Klik in de rechterbovenhoek, **beheerder toevoegen**:

    ![Beheerder van de Zscaler persoonlijke toegang beheerder toevoegen](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addadmin.png)

1. In de **beheerder toevoegen** pagina, voert u de volgende stappen uit:

    ![Gebruikersbeheerder Zscaler persoonlijke toegang beheerder](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_useradmin.png)

    a. In de **gebruikersnaam** tekstvak, voer het e-mailadres van gebruiker, zoals **BrittaSimon@contoso.com**.

    b. In de **wachtwoord** tekstvak typt u het wachtwoord.

    c. In de **wachtwoord bevestigen** tekstvak typt u het wachtwoord.

    d. Selecteer **rol** als **Zscaler persoonlijke toegang beheerder**.

    e. Voer in het tekstvak **Email** het e-mailadres van de gebruiker in, bijvoorbeeld **BrittaSimon@contoso.com**.

    f. In de **Phone** tekstvak typt u het telefoonnummer.

    g. In de **tijdzone** textbox, selecteert u de tijdzone.

    h. Klik op **Opslaan**.  

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Zscaler persoonlijke toegang beheerder.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Zscaler persoonlijke toegang beheerder, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Zscaler persoonlijke toegang beheerder**.

    ![De koppeling Zscaler particuliere Access-beheerder in de lijst met toepassingen](./media/zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Zscaler particuliere Access-beheerder in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Zscaler particuliere Access-beheerder.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



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

