---
title: 'Zelfstudie: Azure Active Directory-integratie met Velpic SAML | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Velpic SAML.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: eb59c61f89b3b23ce8b0a393ee1e0e842826e90b
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811788"
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a>Zelfstudie: Azure Active Directory-integratie met Velpic SAML

In deze zelfstudie leert u hoe u Velpic SAML integreren met Azure Active Directory (Azure AD).

Velpic SAML integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Velpic SAML heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Velpic SAML (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - de Azure Management portal beheren

Zie [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Velpic SAML, moet u de volgende items:

- Een Azure AD-abonnement
- Een Velpic SAML eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- U moet uw productie-omgeving, niet gebruiken als dit nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Velpic SAML vanuit de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-velpic-saml-from-the-gallery"></a>Velpic SAML vanuit de galerie toevoegen
Voor het configureren van de integratie van Velpic SAML in Azure AD, moet u Velpic SAML vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Velpic SAML vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Velpic SAML**.

    ![Het maken van een Azure AD-testgebruiker](./media/velpicsaml-tutorial/tutorial_velpicsaml_search.png)

1. Selecteer in het deelvenster resultaten **Velpic SAML**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert u en test Azure AD eenmalige aanmelding met Velpic SAML op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Velpic SAML is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Velpic SAML tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Velpic SAML.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Velpic SAML, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Velpic SAML](#creating-a-velpic-saml-test-user)**  : als u wilt een equivalent van Britta Simon in Velpic SAML die is gekoppeld aan de Azure AD-weergave van haar hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure Management portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Velpic SAML.

**Voor het configureren van Azure AD eenmalige aanmelding met Velpic SAML, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, op de **Velpic SAML** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **SAML gebaseerde aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

1. Geef de details in de **Velpic SAML-domein en URL's** sectie -

    ![Eenmalige aanmelding configureren](./media/velpicsaml-tutorial/tutorial_velpicsaml_url.png)

    a. In de **aanmeldings-URL** tekstvak typt u de waarde als: `https://<sub-domain>.velpicsaml.net`

    b. In de **id** tekstvak, plak de **'Enkel aanmeldings-URL'** waarde `https://auth.velpic.com/saml/v2/<entity-id>/login`
    
    > [!NOTE]
    > Houd er rekening mee dat de aanmeldings-URL wordt geleverd door het Velpic SAML-team en id-waarde beschikbaar is bij het configureren van de invoegtoepassing SSO Velpic SAML aan. U moet deze waarde van de toepassingspagina Velpic SAML kopiëren en plak deze hier.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/velpicsaml-tutorial/tutorial_general_400.png)

1. Klik in de sectie SAML-configuratie van Velpic Velpic SAML configureren om configureren aanmeldings-venster te openen. Kopieer de SAML-entiteit-ID uit de sectie ter referentie.

1. Meld u in een ander browservenster in uw bedrijf Velpic SAML site als beheerder.

1. Klik op **beheren** tabblad en Ga naar **integratie** sectie waar u nodig hebt op **invoegtoepassingen** om te maken van nieuwe invoegtoepassing voor aanmelding bij.

    ![Invoegtoepassing](./media/velpicsaml-tutorial/velpic_1.png)

1. Klik op de **'Add invoegtoepassing'** knop.
    
    ![Invoegtoepassing](./media/velpicsaml-tutorial/velpic_2.png)

1. Klik op de **SAML** tegel op de pagina toevoegen-invoegtoepassing.
    
    ![Invoegtoepassing](./media/velpicsaml-tutorial/velpic_3.png)

1. Voer de naam van de nieuwe SAML-invoegtoepassing en klik op de **'Toevoegen'** knop.

    ![Invoegtoepassing](./media/velpicsaml-tutorial/velpic_4.png)

1. Voer de gegevens als volgt:

    ![Invoegtoepassing](./media/velpicsaml-tutorial/velpic_5.png)

    a. In de **naam** tekstvak, typ de naam van SAML-invoegtoepassing.

    b. In de **URL-verlener** tekstvak, plak de **SAML entiteit-ID** u gekopieerd uit de **aanmelding configureren** venster van de Azure-portal.

    c. In de **Provider metagegevens Config** uploaden van de Metadata-XML-bestand dat u hebt gedownload van Azure portal.

    d. U kunt er ook voor kiezen om in te schakelen van SAML just-in-time inrichting door in te schakelen de **'Automatisch maken van nieuwe gebruikers'** selectievakje. Als een gebruiker niet in Velpic bestaat en deze markering niet is ingeschakeld, mislukt de aanmelding van Azure. Als de vlag is ingeschakeld. de gebruiker automatisch worden ingericht in Velpic op het moment van de aanmelding. 

    e. Kopieer de **eenmalige aanmelding op URL** uit de tekst vak en plak deze in Azure portal.
    
    f. Klik op **Opslaan**.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in de Azure Management portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/velpicsaml-tutorial/create_aaduser_01.png) 

1. Ga naar **gebruikers en groepen** en klikt u op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Het maken van een Azure AD-testgebruiker](./media/velpicsaml-tutorial/create_aaduser_02.png) 

1. Aan de bovenkant van het dialoogvenster klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/velpicsaml-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/velpicsaml-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-velpic-saml-test-user"></a>Het maken van een testgebruiker Velpic SAML

Deze stap is doorgaans niet vereist als de toepassing just-in-tijd van gebruikersinrichting ondersteunt. Als het automatisch inrichten van gebruikers niet is ingeschakeld kan vervolgens handmatig maken worden uitgevoerd zoals hieronder wordt beschreven.

Meld u aan bij uw site van het bedrijf Velpic SAML als beheerder en voert u de volgende stappen uit:
    
1. Klik op het tabblad beheren en gaat u naar de sectie gebruikers, en klik vervolgens op de knop Nieuwe gebruikers toe te voegen.

    ![Gebruiker toevoegen](./media/velpicsaml-tutorial/velpic_7.png)

1. Op de **'Nieuwe gebruiker maken'** dialoogvenster pagina, de volgende stappen uitvoeren.

    ![user](./media/velpicsaml-tutorial/velpic_8.png)
    
    a. In de **voornaam** tekstvak, de eerste naam van Britta Simon.

    b. In de **achternaam** tekstvak typt u de achternaam van Britta Simon.

    c. In de **gebruikersnaam** tekstvak, typ de naam van de gebruiker van Britta Simon.

    d. Typ in het tekstvak **Email** het e-mailadres van het account van Britta Simon.

    e. De rest van de informatie is optioneel en u deze indien nodig kunt invullen.
    
    f. Klik op **OPSLAAN**.  

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot Velpic SAML.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan Velpic SAML, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-beheerportal, en vervolgens gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Velpic SAML**.

    ![Eenmalige aanmelding configureren](./media/velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

1. Als u de Velpic SAML-tegel in het toegangsvenster klikt, krijgt u de aanmeldingspagina van Velpic SAML-toepassing. U ziet de **'Aanmelden met Azure AD'** knop op de aanmeldingspagina.

    ![Invoegtoepassing](./media/velpicsaml-tutorial/velpic_6.png)

1. Klik op de **'Aanmelden met Azure AD'** knop aanmelden bij Velpic met behulp van uw Azure AD-account.


## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/velpicsaml-tutorial/tutorial_general_203.png

