---
title: 'Zelfstudie: Azure Active Directory-integratie met Sprinklr | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Sprinklr.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: b5f240091cf5ce9331ada818c218db04f508dbc8
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50241800"
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Zelfstudie: Azure Active Directory-integratie met Sprinklr

In deze zelfstudie leert u hoe u Sprinklr integreren met Azure Active Directory (Azure AD).

Sprinklr integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Sprinklr heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Sprinklr (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Sprinklr, moet u de volgende items:

- Een Azure AD-abonnement
- Een Sprinklr eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Sprinklr uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-sprinklr-from-the-gallery"></a>Sprinklr uit de galerie toe te voegen
Voor het configureren van de integratie van Sprinklr in Azure AD, moet u Sprinklr uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Sprinklr uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **Sprinklr**.

    ![Het maken van een Azure AD-testgebruiker](./media/sprinklr-tutorial/tutorial_sprinklr_search.png)

1. Selecteer in het deelvenster resultaten **Sprinklr**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/sprinklr-tutorial/tutorial_sprinklr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Sprinklr op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Sprinklr is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Sprinklr tot stand worden gebracht.

In Sprinklr, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Sprinklr, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Sprinklr](#creating-a-sprinklr-test-user)**  : als u wilt een equivalent van Britta Simon in Sprinklr die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Sprinklr.

**Voor het configureren van Azure AD eenmalige aanmelding met Sprinklr, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Sprinklr** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/sprinklr-tutorial/tutorial_sprinklr_samlbase.png)

1. Op de **Sprinklr domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/sprinklr-tutorial/tutorial_sprinklr_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.sprinklr.com`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.sprinklr.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL en -id. Neem contact op met [Sprinklr Client ondersteuningsteam](https://www.sprinklr.com/contact-us/) om deze waarden te verkrijgen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/sprinklr-tutorial/tutorial_sprinklr_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/sprinklr-tutorial/tutorial_general_400.png)

1. Op de **Sprinklr configuratie** sectie, klikt u op **configureren Sprinklr** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

1. In een ander browservenster aanmelden bij uw bedrijf Sprinklr site als beheerder.

1. Ga naar **beheer \> instellingen**.
   
    ![Beheer](./media/sprinklr-tutorial/ic782907.png "beheer")

1. Ga naar **beheren Partner \> eenmalige aanmelding** op in het linkerdeelvenster.
   
    ![Partner beheren](./media/sprinklr-tutorial/ic782908.png "Partner beheren")

1. Klik op **+ invoegtoepassingen voor eenmalige aanmelding**.
   
    ![Eenmalige aanmeldingen](./media/sprinklr-tutorial/ic782909.png "eenmalige aanmeldingen")

1. Op de **voor eenmalige aanmelding** pagina, voert u de volgende stappen uit:
   
    ![Eenmalige aanmeldingen](./media/sprinklr-tutorial/ic782910.png "eenmalige aanmeldingen")

    a. In de **naam** tekstvak, typ een naam voor uw configuratie (bijvoorbeeld: *WAADSSOTest*).

    b. Selecteer **ingeschakeld**.

    c. Selecteer **nieuwe SSO-certificaat gebruiken**.
             
    e. Het base-64 gecodeerde certificaat openen in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **Provider identiteitscertificaat** tekstvak.

    f. Plak de **SAML entiteit-ID** waarde die u hebt gekopieerd vanuit Azure Portal in de **entiteit-Id** tekstvak.

    g. Plak de **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit Azure Portal in de **aanmeldings-URL van id-Provider** tekstvak.

    h. Plak de **afmelding URL** waarde die u hebt gekopieerd vanuit Azure Portal in de **afmeldings-URL van id-Provider** tekstvak.
     
    i. Als **SAML-ID gebruikerstype**, selecteer **verklaring bevat de gebruikersnaam van gebruiker sprinklr.com**.

    j. Als **SAML-ID gebruikerslocatie**, selecteer **gebruikers-ID is in het element met naam-id van het onderwerp overzicht**.

    k. Klik op **Opslaan**.
       
    ![SAML](./media/sprinklr-tutorial/ic782911.png "SAML")

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/sprinklr-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/sprinklr-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/sprinklr-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/sprinklr-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-sprinklr-test-user"></a>Het maken van een testgebruiker Sprinklr

1. Meld u aan bij uw bedrijf Sprinklr site aan als beheerder.

1. Ga naar **beheer \> instellingen**.
   
    ![Beheer](./media/sprinklr-tutorial/ic782907.png "beheer")

1. Ga naar **Client beheren \> gebruikers** in het linkerdeelvenster.
   
    ![Instellingen voor](./media/sprinklr-tutorial/ic782914.png "instellingen")

1. Klik op **gebruiker toevoegen**.
   
    ![Instellingen voor](./media/sprinklr-tutorial/ic782915.png "instellingen")

1. Op de **bewerken gebruiker** dialoogvenster, voer de volgende stappen uit:
   
    ![Gebruiker bewerken](./media/sprinklr-tutorial/ic782916.png "gebruiker bewerken") 

    a. In de **e**, **voornaam** en **achternaam** tekstvakken, typt u de gegevens van een Azure AD-gebruikersaccount die u inrichten wilt.

    b. Selecteer **wachtwoord uitgeschakeld**.

    c. Selecteer **taal**.

    d. Selecteer **gebruikerstype**.

    e. Klik op **Update**.
   
     >[!IMPORTANT]
     >**Wachtwoord uitgeschakeld** zodat een gebruiker zich aanmelden via een id-provider moet worden geselecteerd. 
     
1. Ga naar **rol**, en voer de volgende stappen uit:
   
    ![Functies van partners](./media/sprinklr-tutorial/ic782917.png "rollen van partners")

    a. Uit de **Global** in de lijst met **alle\_machtigingen**.  

    b. Klik op **Update**.

>[!NOTE]
>U kunt alle andere Sprinklr gebruiker-account maken van hulpprogramma's of API's geleverd door Sprinklr voor het inrichten van gebruikersaccounts van de Azure AD. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Sprinklr.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Sprinklr toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Sprinklr**.

    ![Eenmalige aanmelding configureren](./media/sprinklr-tutorial/tutorial_sprinklr_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Sprinklr in het toegangsvenster, krijgt u automatisch aangemeld bij uw toepassing Sprinklr Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/sprinklr-tutorial/tutorial_general_01.png
[2]: ./media/sprinklr-tutorial/tutorial_general_02.png
[3]: ./media/sprinklr-tutorial/tutorial_general_03.png
[4]: ./media/sprinklr-tutorial/tutorial_general_04.png

[100]: ./media/sprinklr-tutorial/tutorial_general_100.png

[200]: ./media/sprinklr-tutorial/tutorial_general_200.png
[201]: ./media/sprinklr-tutorial/tutorial_general_201.png
[202]: ./media/sprinklr-tutorial/tutorial_general_202.png
[203]: ./media/sprinklr-tutorial/tutorial_general_203.png

