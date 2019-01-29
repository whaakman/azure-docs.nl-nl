---
title: 'Zelfstudie: Azure Active Directory-integratie met EFI digitale winkel | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en EFI digitale winkel.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 33c148fc-d490-4bb9-90c1-d5933679ce4e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: b93e63acbad5a43b898e80dc706dfe17837f0183
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55155354"
---
# <a name="tutorial-azure-active-directory-integration-with-efi-digital-storefront"></a>Zelfstudie: Azure Active Directory-integratie met EFI digitale winkel

In deze zelfstudie leert u hoe u de digitale winkel EFI integreren met Azure Active Directory (Azure AD).

Digitale winkel EFI integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot EFI digitale winkel heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij de digitale winkel EFI (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met EFI digitale winkel, moet u de volgende items:

- Een Azure AD-abonnement
- Een digitale winkel EFI eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Digitale winkel EFI uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-efi-digital-storefront-from-the-gallery"></a>Digitale winkel EFI uit de galerie toe te voegen
Voor het configureren van de integratie van digitale winkel EFI in Azure AD, moet u EFI digitale winkel uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen EFI digitale winkel uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **EFI digitale winkel**, selecteer **EFI digitale winkel** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![EFI digitale winkel in de lijst met resultaten](./media/efidigitalstorefront-tutorial/tutorial_efidigital_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met EFI digitale winkel op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in EFI digitale winkel is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in EFI digitale winkel tot stand worden gebracht.

Wijs in EFI digitale winkel, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met EFI digitale winkel, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker EFI digitale winkel](#create-a-efi-digital-storefront-test-user)**  : als u wilt een equivalent van Britta Simon in EFI digitale winkel die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw digitale winkel EFI-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met EFI digitale winkel, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **EFI digitale winkel** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/efidigitalstorefront-tutorial/tutorial_efidigital_samlbase.png)

1. Op de **EFI digitale winkel domein en URL's** sectie, voert u de volgende stappen uit:

    ![EFI digitale winkel domein en URL's, eenmalige aanmelding informatie](./media/efidigitalstorefront-tutorial/tutorial_efidigital_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<companyname>.myprintdesk.net/DSF`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<companyname>.myprintdesk.net/DSF/asp4/`

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/efidigitalstorefront-tutorial/tutorial_efidigital_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/efidigitalstorefront-tutorial/tutorial_general_400.png)

1. Het configureren van eenmalige aanmelding op **EFI digitale winkel** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [digitale winkel EFI-ondersteuningsteam](https://www.efi.com/products/productivity-software/ecommerce-web-to-print/efi-digital-storefront/support/). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/efidigitalstorefront-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/efidigitalstorefront-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/efidigitalstorefront-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/efidigitalstorefront-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-efi-digital-storefront-test-user"></a>Maak een testgebruiker EFI digitale winkel

In deze sectie maakt u een gebruiker met de naam van Britta Simon in EFI digitale winkel. Werken met [digitale winkel EFI-ondersteuningsteam](https://www.efi.com/products/productivity-software/ecommerce-web-to-print/efi-digital-storefront/support/) om toe te voegen de gebruikers in het digitale winkel EFI-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan digitale winkel EFI.

![De de gebruikersrol toewijzen][200] 

**Britta Simon om aan te wijzen EFI digitale winkel, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **EFI digitale winkel**.

    ![De digitale winkel EFI-koppeling in de lijst met toepassingen](./media/efidigitalstorefront-tutorial/tutorial_efidigital_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel EFI digitale winkel in het toegangsvenster, u moet u automatisch aangemeld bij uw digitale winkel EFI-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/efidigitalstorefront-tutorial/tutorial_general_01.png
[2]: ./media/efidigitalstorefront-tutorial/tutorial_general_02.png
[3]: ./media/efidigitalstorefront-tutorial/tutorial_general_03.png
[4]: ./media/efidigitalstorefront-tutorial/tutorial_general_04.png

[100]: ./media/efidigitalstorefront-tutorial/tutorial_general_100.png

[200]: ./media/efidigitalstorefront-tutorial/tutorial_general_200.png
[201]: ./media/efidigitalstorefront-tutorial/tutorial_general_201.png
[202]: ./media/efidigitalstorefront-tutorial/tutorial_general_202.png
[203]: ./media/efidigitalstorefront-tutorial/tutorial_general_203.png

