---
title: 'Zelfstudie: Azure Active Directory-integratie met afbeelding Relay | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Image-Relay.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 326f12ff3923855b09462d4909d37638d4010b48
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435006"
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a>Zelfstudie: Azure Active Directory-integratie met afbeelding Relay

In deze zelfstudie leert u hoe u Image Relay integreren met Azure Active Directory (Azure AD).

Afbeelding Relay integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Relay-installatiekopie heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij de Relay-installatiekopie (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met de Relay-installatiekopie, moet u de volgende items:

- Een Azure AD-abonnement
- Een afbeelding Relay eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Afbeelding Relay uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-image-relay-from-the-gallery"></a>Afbeelding Relay uit de galerie toe te voegen
Voor het configureren van de integratie van afbeelding Relay in Azure AD, moet u de Relay-installatiekopie uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen de Relay-installatiekopie uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **installatiekopie Relay**.

    ![Het maken van een Azure AD-testgebruiker](./media/imagerelay-tutorial/tutorial_imagerelay_search.png)

1. Selecteer in het deelvenster resultaten **installatiekopie Relay**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/imagerelay-tutorial/tutorial_imagerelay_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en Azure AD eenmalige aanmelding testen met afbeelding Relay op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de afbeelding Relay is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de Relay-installatiekopie tot stand worden gebracht.

In afbeelding Relay, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en Azure AD eenmalige aanmelding testen met afbeelding Relay, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een installatiekopie Relay testgebruiker](#creating-an-image-relay-test-user)**  : als u wilt een equivalent van Britta Simon in installatiekopie Relay die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw installatiekopie Relay-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met de Relay-installatiekopie, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **installatiekopie Relay** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_samlbase.png)

1. Op de **installatiekopie Relay domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.imagerelay.com/`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.imagerelay.com/sso/metadata`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [installatiekopie Relay Client ondersteuningsteam](http://support.imagerelay.com/) om deze waarden te verkrijgen. 
 


1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_general_400.png)

1. Op de **Relay-configuratie van installatiekopie** sectie, klikt u op **installatiekopie Relay configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding Service-URL en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_configure.png) 

1. In een ander browservenster aanmelden bij uw bedrijf installatiekopie Relay site als beheerder.

1. Klik in de werkbalk bovenaan op de **gebruikers en machtigingen** werkbelasting.
   
    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_06.png) 

1. Klik op **nieuwe machtigingenset maken**.
   
    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_08.png)

1. In de **één teken op instellingen** werkbelasting, selecteer de **deze groep kunnen alleen aanmelden via eenmalige aanmelding** selectievakje en klik vervolgens op **opslaan**.
   
    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_09.png) 

1. Ga naar **Accountinstellingen**.
   
    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_10.png) 

1. Ga naar de **één teken op instellingen** werkbelasting.
    
     ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_11.png)

1. Op de **SAML-instellingen** dialoogvenster, voer de volgende stappen uit:
    
    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_12.png)
    
    a. In **aanmeldings-URL** tekstvak, plak de waarde van **Single Sign-On Service URL** die u hebt gekopieerd vanuit Azure portal.

    b. In **afmeldings-URL van** tekstvak, plak de waarde van **Service-URL voor eenmalige afmelding** die u hebt gekopieerd vanuit Azure portal.

    c. Als **indeling van de Id**, selecteer **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: emailAddress**.

    d. Als **Binding opties voor het aanvragen van de serviceprovider (installatiekopie-Relay)**, selecteer **POST Binding**.

    e. Onder **x.509 certificaat**, klikt u op **updatecertificaat**.

    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. Open het gedownloade certificaat in Kladblok, Kopieer de inhoud en plak deze in het tekstvak x.509-certificaat.

    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. In **JIT Gebruikersinrichting** sectie, selecteer de **inrichten van gebruikers met JIT inschakelen**.

    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. Selecteer de machtigingsgroep (bijvoorbeeld **SSO Basic**) die zich alleen via eenmalige aanmelding is toegestaan.

    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_20.png)

    i. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/imagerelay-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/imagerelay-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/imagerelay-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/imagerelay-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-image-relay-test-user"></a>Het maken van een testgebruiker installatiekopie Relay

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in installatiekopie Relay.

**Voor het maken van een gebruiker met de naam van Britta Simon in installatiekopie Relay, moet u de volgende stappen uitvoeren:**

1. Aanmelding bij uw bedrijf installatiekopie Relay site als beheerder.

1. Ga naar **gebruikers en machtigingen** en selecteer **SSO-gebruiker maken**.
   
    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_21.png) 

1. Voer de **e**, **voornaam**, **achternaam**, en **bedrijf** van de gebruiker die u wilt inrichten en selecteer de machtigingsgroep (voor voorbeeld SSO Basic) dat is de groep die alleen via eenmalige aanmelding kunt aanmelden.
   
    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_22.png) 

1. Klik op **Create**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan installatiekopie Relay.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar de Relay-installatiekopie, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **installatiekopie Relay**.

    ![Eenmalige aanmelding configureren](./media/imagerelay-tutorial/tutorial_imagerelay_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.    

Wanneer u op de installatiekopie van Relay-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing de Relay-installatiekopie.


## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/imagerelay-tutorial/tutorial_general_04.png


[100]: ./media/imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/imagerelay-tutorial/tutorial_general_201.png
[202]: ./media/imagerelay-tutorial/tutorial_general_202.png
[203]: ./media/imagerelay-tutorial/tutorial_general_203.png

