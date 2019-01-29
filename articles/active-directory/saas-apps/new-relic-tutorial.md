---
title: 'Zelfstudie: Azure Active Directory-integratie met New Relic | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en New Relic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: jeedes
ms.openlocfilehash: a60360d8268d43d920ad878d372014fa6d9cc875
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55176859"
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Zelfstudie: Azure Active Directory-integratie met New Relic

In deze zelfstudie leert u hoe u New Relic integreren met Azure Active Directory (Azure AD).

New Relic integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de New Relic heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij New Relic (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met New Relic, moet u de volgende items:

- Een Azure AD-abonnement
- Een New Relic eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. New Relic uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-new-relic-from-the-gallery"></a>New Relic uit de galerie toe te voegen
Voor het configureren van de integratie van New Relic in Azure AD, moet u New Relic uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen de New Relic uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **New Relic**, selecteer **New Relic** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![New Relic in de lijst met resultaten](./media/new-relic-tutorial/tutorial_new-relic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met New Relic op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in New Relic is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in New Relic tot stand worden gebracht.

In de New Relic, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met New Relic, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker New Relic](#create-a-new-relic-test-user)**  : als u wilt een equivalent van Britta Simon in New Relic die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw New Relic-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met New Relic, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **New Relic** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/new-relic-tutorial/tutorial_new-relic_samlbase.png)

1. Op de **nieuwe Relic domein en URL's** sectie, voert u de volgende stappen uit:

    ![Nieuwe Relic domein en URL's, eenmalige aanmelding informatie](./media/new-relic-tutorial/tutorial_new-relic_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://rpm.newrelic.com/accounts/{acc_id}/sso/saml/login` -Zorg ervoor dat u vervangen door uw eigen New Relic-Account-ID.

    b. In de **id** tekstvak typt u de waarde: `rpm.newrelic.com`

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/new-relic-tutorial/tutorial_new-relic_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/new-relic-tutorial/tutorial_general_400.png)

1. Op de **nieuwe Relic configuratie** sectie, klikt u op **New Relic configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Nieuwe Relic-configuratie](./media/new-relic-tutorial/tutorial_new-relic_configure.png) 

1. In een ander browservenster aanmelden bij uw **New Relic** bedrijf site als administrator.

1. Klik in het menu aan de bovenkant op **Accountinstellingen**.
   
    ![Account Settings](./media/new-relic-tutorial/ic797036.png "Account Settings")

1. Klik op de **beveiligings- en authenticatieservices** tabblad en klik vervolgens op de **eenmalige aanmelding** tabblad.
   
    ![Single Sign-On](./media/new-relic-tutorial/ic797037.png "Single Sign-On")

1. Voer de volgende stappen uit op de pagina van het dialoogvenster SAML:
   
    ![SAML](./media/new-relic-tutorial/ic797038.png "SAML")
   
   a. Klik op **bestand kiezen** om uw gedownloade Azure Active Directory-certificaat te uploaden.

   b. In de **externe aanmeldings-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.
   
   c. In de **afmelden aanvoer URL** tekstvak, plak de waarde van **afmelding URL**, die u hebt gekopieerd vanuit Azure portal.

   d. Klik op **mijn wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/new-relic-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/new-relic-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/new-relic-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/new-relic-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-new-relic-test-user"></a>Maak een testgebruiker New Relic

Als u wilt dat Azure Active Directory-gebruikers zich aanmelden bij de New Relic, moeten ze worden ingericht in New Relic. In het geval van New Relic is inrichten een handmatige taak.

**Voor het inrichten van een New Relic-gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **New Relic** bedrijf site als administrator.

1. Klik in het menu aan de bovenkant op **Accountinstellingen**.
   
    ![Account Settings](./media/new-relic-tutorial/ic797040.png "Account Settings")

1. In de **Account** deelvenster aan de linkerkant te klikken op **samenvatting**, en klik vervolgens op **gebruiker toevoegen**.
   
    ![Account Settings](./media/new-relic-tutorial/ic797041.png "Account Settings")

1. Op de **actieve gebruikers** dialoogvenster, voer de volgende stappen uit:
   
    ![Actieve gebruikers](./media/new-relic-tutorial/ic797042.png "actieve gebruikers")
   
    a. In de **e** tekstvak typt u het e-mailadres van een geldige Azure Active Directory-gebruiker die u inrichten wilt.

    b. Als **rol** Selecteer **gebruiker**.

    c. Klik op **deze gebruiker toevoegen**.

>[!NOTE]
>U kunt alle andere New Relic gebruiker-account maken van hulpprogramma's of API's geleverd door New Relic aan inrichten AAD-gebruikersaccounts.
> 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding door toegang te verlenen aan de New Relic gebruiken.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon op New Relic, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **New Relic**.

    ![De New Relic-koppeling in de lijst met toepassingen](./media/new-relic-tutorial/tutorial_new-relic_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de New Relic-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw New Relic-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/new-relic-tutorial/tutorial_general_01.png
[2]: ./media/new-relic-tutorial/tutorial_general_02.png
[3]: ./media/new-relic-tutorial/tutorial_general_03.png
[4]: ./media/new-relic-tutorial/tutorial_general_04.png

[100]: ./media/new-relic-tutorial/tutorial_general_100.png

[200]: ./media/new-relic-tutorial/tutorial_general_200.png
[201]: ./media/new-relic-tutorial/tutorial_general_201.png
[202]: ./media/new-relic-tutorial/tutorial_general_202.png
[203]: ./media/new-relic-tutorial/tutorial_general_203.png

