---
title: 'Zelfstudie: Azure Active Directory-integratie met voorzien CX Suite | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en CX Suite voorzien.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5f4b7830-6186-4d17-b77b-504d4192bfde
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: jeedes
ms.openlocfilehash: 54dd577e3e3cc1650e4384e3b7f4eda581fe0fa1
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55191139"
---
# <a name="tutorial-azure-active-directory-integration-with-foresee-cx-suite"></a>Zelfstudie: Azure Active Directory-integratie met CX Suite voorzien

In deze zelfstudie leert u hoe u om te voorzien CX Suite integreren met Azure Active Directory (Azure AD).

Met CX Suite voorzien integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot CX Suite voorzien heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij CX Suite voorzien (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met CX Suite voorzien, moet u de volgende items:

- Een Azure AD-abonnement
- Een voorzien CX Suite eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. CX Suite voorzien in de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-foresee-cx-suite-from-the-gallery"></a>CX Suite voorzien in de galerie toevoegen
Voor het configureren van de integratie van CX Suite voorzien in Azure AD, moet u voorzien CX Suite uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen CX Suite voorzien in de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **voorzien CX Suite**, selecteer **voorzien CX Suite** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Met CX Suite voorzien in de lijst met resultaten](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met voorzien CX Suite op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in voorzien CX Suite is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in voorzien CX Suite tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met CX Suite voorzien, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker voorzien CX Suite](#create-a-foresee-cx-suite-test-user)**  : als u wilt een equivalent van Britta Simon in voorzien CX Suite die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing voorzien CX-Suite.

**Voor het configureren van Azure AD eenmalige aanmelding met CX Suite voorzien, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **voorzien CX Suite** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_samlbase.png)

1. Op de **voorzien CX Suite domein en URL's** sectie, hebt u **Service Provider-bestand met metagegevens**, voer de volgende stappen uit:

    ![Met CX Suite domein en URL's één aanmelding informatie voorzien](./media/foreseecxsuite-tutorial/upload.png)

    a. Klik op **Metagegevensbestand uploaden**.

    ![Met CX Suite domein en URL's één aanmelding informatie voorzien](./media/foreseecxsuite-tutorial/tutorial_foreseen_uploadconfig.png)

    b. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.

    c. Na voltooiing van het uploaden van **Service Provider-bestand met metagegevens** de **id** waarde get automatisch ingevuld **voorzien CX Suite domein en URL's** sectie tekstvak, zoals hieronder weergegeven:

    ![Met CX Suite domein en URL's één aanmelding informatie voorzien](./media/foreseecxsuite-tutorial/urlupload.png)

1. Als u geen **Service Provider-bestand met metagegevens**, voer de volgende stappen uit:

    ![Met CX Suite domein en URL's één aanmelding informatie voorzien](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_url.png)

    a. In de **aanmeldings-URL** tekstvak typt u de URL: `https://cxsuite.foresee.com/`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://www.okta.com/saml2/service-provider/<UniqueID>`

    > [!NOTE]
    > De id-waarde is niet echt. Werk deze waarde bij met de werkelijke id. Neem contact op met [voorzien CX Suite Client ondersteuningsteam](mailto:support@foresee.com) deze waarde op te halen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_certificate.png)

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/foreseecxsuite-tutorial/tutorial_general_400.png)

1. Het configureren van eenmalige aanmelding op **voorzien CX Suite** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [voorzien CX Suite ondersteuningsteam](mailto:support@foresee.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/foreseecxsuite-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/foreseecxsuite-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/foreseecxsuite-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/foreseecxsuite-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-foresee-cx-suite-test-user"></a>Maak een testgebruiker voorzien CX-Suite

In deze sectie maakt u een gebruiker met de naam van Britta Simon in CX Suite voorzien. Werken met [voorzien CX Suite ondersteuningsteam](mailto:support@foresee.com) de gebruikers of het domein dat nodig is om te worden opgenomen in het platform, CX Suite voorzien in de whitelist toe te voegen. Als het domein is toegevoegd door het team, krijgen automatisch gebruikers ingericht voor het platform CX Suite voorzien. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen om te voorzien CX Suite.

![De de gebruikersrol toewijzen][200]

**Als u wilt toewijzen Britta Simon voorzien CX-Suite, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

1. Selecteer in de lijst met toepassingen, **voorzien CX Suite**.

    ![De koppeling CX Suite voorzien in de lijst met toepassingen](./media/foreseecxsuite-tutorial/tutorial_foreseecxsuite_app.png)

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel CX Suite voorzien in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing voorzien CX-Suite.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/foreseecxsuite-tutorial/tutorial_general_01.png
[2]: ./media/foreseecxsuite-tutorial/tutorial_general_02.png
[3]: ./media/foreseecxsuite-tutorial/tutorial_general_03.png
[4]: ./media/foreseecxsuite-tutorial/tutorial_general_04.png

[100]: ./media/foreseecxsuite-tutorial/tutorial_general_100.png

[200]: ./media/foreseecxsuite-tutorial/tutorial_general_200.png
[201]: ./media/foreseecxsuite-tutorial/tutorial_general_201.png
[202]: ./media/foreseecxsuite-tutorial/tutorial_general_202.png
[203]: ./media/foreseecxsuite-tutorial/tutorial_general_203.png

