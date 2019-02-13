---
title: 'Zelfstudie: Azure Active Directory-integratie met SD elementen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory- en SD-elementen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c09947e6d34c5314e8ed4bc2744f07b199278a6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56188100"
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a>Zelfstudie: Azure Active Directory-integratie met SD-elementen

In deze zelfstudie leert u hoe u naar de SD-elementen worden geïntegreerd met Azure Active Directory (Azure AD).

SD elementen integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de SD-elementen heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij SD-elementen (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SD-elementen, hebt u de volgende items nodig:

- Een Azure AD-abonnement
- Een SD-elementen eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SD-elementen toe te voegen uit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sd-elements-from-the-gallery"></a>SD-elementen toe te voegen uit de galerie
Voor het configureren van de integratie van SD-elementen in Azure AD, moet u SD elementen uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SD elementen uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **SD elementen**.

    ![Het maken van een Azure AD-testgebruiker](./media/sd-elements-tutorial/tutorial_sdelements_search.png)

1. Selecteer in het deelvenster resultaten **SD elementen**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/sd-elements-tutorial/tutorial_sdelements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SD-elementen op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent SD-elementen in Azure AD aan een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker SD-elementen tot stand worden gebracht.

In de SD-elementen, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met SD-elementen, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker SD elementen](#creating-a-sd-elements-test-user)**  : als u wilt een equivalent van Britta Simon in SD-elementen die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing SD-elementen.

**Voor het configureren van Azure AD eenmalige aanmelding met SD-elementen, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SD elementen** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/sd-elements-tutorial/tutorial_sdelements_samlbase.png)

1. Op de **SD elementen domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/sd-elements-tutorial/tutorial_sdelements_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<tenantname>.sdelements.com/sso/saml2/metadata`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<tenantname>.sdelements.com/sso/saml2/acs/`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id en antwoord-URL. Neem contact op met [SD elementen ondersteuningsteam](mailto:support@sdelements.com) om deze waarden te verkrijgen.

1. Elementen van de SD-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **"Gebruikerskenmerk"** tabblad van de toepassing. In de volgende schermopname ziet u een voorbeeld hiervan.

    ![Eenmalige aanmelding configureren](./media/sd-elements-tutorial/tutorial_sdelements_attribute.png)

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en voer de volgende stappen uit: 

    | Naam kenmerk | Waarde kenmerk |
    | --- | --- |
    | e-mail |user.mail |
    | firstname |user.givenname |
    | lastname |user.surname |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/sd-elements-tutorial/tutorial_officespace_04.png)

    ![Eenmalige aanmelding configureren](./media/sd-elements-tutorial/tutorial_officespace_05.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Klik op **OK**.
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/sd-elements-tutorial/tutorial_sdelements_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/sd-elements-tutorial/tutorial_general_400.png)

1. Op de **SD elementen configuratie** sectie, klikt u op **SD-elementen configureren** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/sd-elements-tutorial/tutorial_sdelements_configure.png)

1. Als u eenmalige aanmelding ingeschakeld, neem contact op met uw [SD elementen ondersteuningsteam](mailto:support@sdelements.com) en hen voorzien van het bestand gedownloade certificaat. 

1. In een ander browservenster aanmelden voor uw tenant SD-elementen als beheerder.

1. Klik in het menu aan de bovenkant op **System**, en vervolgens **Single Sign-on**. 
   
    ![Eenmalige aanmelding configureren](./media/sd-elements-tutorial/tutorial_sd-elements_09.png) 

1. Op de **instellingen voor eenmalige aanmelding** dialoogvenster, voer de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    a. Als **SSO Type**, selecteer **SAML**.
   
    b. In de **Identity Provider entiteit-ID** tekstvak, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal. 
   
    c. In de **Identity Provider Single Sign-On-Service** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal. 
   
    d. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/sd-elements-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/sd-elements-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/sd-elements-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/sd-elements-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-sd-elements-test-user"></a>Het maken van een testgebruiker SD-elementen

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon SD-elementen. In het geval van SD-elementen is het maken van gebruikers van de SD-elementen een handmatige taak.

**Als u wilt Britta Simon SD-elementen maken, moet u de volgende stappen uitvoeren:**

1. In een browservenster aanmelding in voor uw bedrijf SD elementen site als beheerder.

1. Klik in het menu aan de bovenkant op **Gebruikersbeheer**, en vervolgens **gebruikers**.
   
    ![Het maken van een testgebruiker SD-elementen](./media/sd-elements-tutorial/tutorial_sd-elements_11.png) 

1. Klik op **Add New User**.
   
    ![Het maken van een testgebruiker SD-elementen](./media/sd-elements-tutorial/tutorial_sd-elements_12.png)
 
1. Op de **Add New User** dialoogvenster, voer de volgende stappen uit:
   
    ![Het maken van een testgebruiker SD-elementen](./media/sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
    a. In de **e** tekstvak, voer het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.
   
    b. In de **voornaam** tekstvak, geef de voornaam van de gebruiker, zoals **Julia**.
   
    c. In de **achternaam** tekstvak, geef de achternaam van de gebruiker, zoals **Simon**.
   
    d. Als **rol**, selecteer **gebruiker**. 
   
    e. Klik op **gebruiker maken**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SD-elementen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon SD-elementen, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **SD elementen**.

    ![Eenmalige aanmelding configureren](./media/sd-elements-tutorial/tutorial_sdelements_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.
  
Wanneer u op de tegel SD-elementen in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing SD-elementen.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/sd-elements-tutorial/tutorial_general_04.png

[100]: ./media/sd-elements-tutorial/tutorial_general_100.png

[200]: ./media/sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/sd-elements-tutorial/tutorial_general_201.png
[202]: ./media/sd-elements-tutorial/tutorial_general_202.png
[203]: ./media/sd-elements-tutorial/tutorial_general_203.png

