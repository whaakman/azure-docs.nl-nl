---
title: 'Zelfstudie: Azure Active Directory-integratie met O.C. Tan - AppreciateHub | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en O.C. Tan - AppreciateHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: dee8fbca-0b60-4a21-8917-1fb6919de5a0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 57838d5f5a49045138ce9adbdcf7855aeab783e8
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56172112"
---
# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a>Zelfstudie: Azure Active Directory-integratie met O.C. Tanner - AppreciateHub

In deze zelfstudie leert u hoe u O.C. integreren Tan - AppreciateHub met Azure Active Directory (Azure AD).

Integratie van O.C. Tan - AppreciateHub met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot O.C. heeft Tanner - AppreciateHub
- U kunt uw gebruikers automatisch ophalen aangemeld bij O.C. inschakelen Tan - AppreciateHub (Single Sign-On) met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie met O.C. configureren Tan - AppreciateHub, moet u de volgende items:

- Een Azure AD-abonnement
- A O.C. Tan - AppreciateHub eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. O.C. toevoegen Tan - AppreciateHub uit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-oc-tanner---appreciatehub-from-the-gallery"></a>O.C. toevoegen Tan - AppreciateHub uit de galerie
De integratie van O.C. configureren Tan - AppreciateHub in Azure AD, moet u O.C. toevoegen Tan - AppreciateHub uit de galerie aan de lijst met beheerde SaaS-apps.

**Om toe te voegen O.C. Tan - AppreciateHub uit de galerie, de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **O.C. Tanner - AppreciateHub**.

    ![Het maken van een Azure AD-testgebruiker](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_search.png)

1. Selecteer in het deelvenster resultaten **O.C. Tan - AppreciateHub**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met O.C. Tan - AppreciateHub op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de equivalente-gebruiker in O.C. Tan - AppreciateHub is aan een gebruiker in Azure AD. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in O.C. Tan - AppreciateHub moet tot stand worden gebracht.

In O.C. Tan - AppreciateHub, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met O.C. Tan - AppreciateHub, die u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een O.C. Tan - AppreciateHub testgebruiker](#creating-a-oc-tanner---appreciatehub-test-user)**  : als u wilt een equivalent van Britta Simon in O.C. hebben Tan - AppreciateHub die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw O.C. Tan - AppreciateHub toepassing.

**Het configureren van Azure AD eenmalige aanmelding met O.C. Tan - AppreciateHub, voer de volgende stappen uit:**

1. In de Azure-portal op de **O.C. Tan - AppreciateHub** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_samlbase.png)

1. Op de **O.C. Tan - AppreciateHub domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_url.png)

    a. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<companyname>.octanner.net/sp/ACS.saml2`

    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke antwoord-URL. Neem contact op met [O.C. Tan - ondersteuningsteam AppreciateHub](mailto:sso@octanner.com) deze waarde op te halen.

    b. Open het bestand met metagegevens via de volgende koppeling: [ https://fed.appreciatehub.com/fed/sp/metadata ](https://fed.appreciatehub.com/fed/sp/metadata).
   
    c. Zoek de **md:AssertionConsumerService** knooppunt. 
   
    d. Kopieer de waarde van de **locatie** kenmerk. 
   
    ![App-instellingen configureren][12]
   
    e. In de **aanmelding URL** tekstvak voorbij de waarde die u in de vorige stap hebt verkregen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/oc-tanner-tutorial/tutorial_general_400.png)

1. Het configureren van eenmalige aanmelding op **O.C. Tan - AppreciateHub** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [O.C. Tan - ondersteuningsteam AppreciateHub](mailto:sso@octanner.com).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/oc-tanner-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/oc-tanner-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/oc-tanner-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/oc-tanner-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a>Het maken van een O.C. Tan - AppreciateHub testgebruiker

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in O.C. Tan - AppreciateHub.

**Het maken van een gebruiker met de naam van Britta Simon in O.C. Tan - AppreciateHub, voer de volgende stappen uit:**

Vraag uw [O.C. Tan - ondersteuningsteam AppreciateHub](mailto:sso@octanner.com) te maken van een gebruiker die als kenmerk van nameID dezelfde waarde als de gebruikersnaam van Britta Simon in Azure AD heeft.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan O.C. Tan - AppreciateHub.

![Gebruiker toewijzen][200] 

**Britta Simon toewijzen aan O.C. Tan - AppreciateHub, voer de volgende stappen uit:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **O.C. Tanner - AppreciateHub**.

    ![Eenmalige aanmelding configureren](./media/oc-tanner-tutorial/tutorial_octannerappreciatehub_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.  
Wanneer u klikt op de O.C. Tan - AppreciateHub tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw O.C. Tan - AppreciateHub toepassing.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/oc-tanner-tutorial/tutorial_general_04.png

[12]: ./media/oc-tanner-tutorial/tutorial_octanner_08.png

[100]: ./media/oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/oc-tanner-tutorial/tutorial_general_202.png
[203]: ./media/oc-tanner-tutorial/tutorial_general_203.png

