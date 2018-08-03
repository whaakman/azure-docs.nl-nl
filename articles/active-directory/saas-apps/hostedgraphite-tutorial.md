---
title: 'Zelfstudie: Azure Active Directory-integratie met de grafiek die wordt gehost | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de grafiek die wordt gehost.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: a1ac4d7f-d079-4f3c-b6da-0f520d427ceb
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: c64e54b80b6295358036d054af14ebe85432d3f6
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39437376"
---
# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a>Zelfstudie: Azure Active Directory-integratie met de grafiek die wordt gehost

In deze zelfstudie leert u over het integreren van de grafiek die worden gehost met Azure Active Directory (Azure AD).

Integratie van de grafiek die wordt gehost in Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de grafiek die wordt gehost heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij de grafiek die wordt gehost (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met de grafiek die wordt gehost, moet u de volgende items:

- Een Azure AD-abonnement
- Een grafiek die wordt gehost eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Grafiek die wordt gehost in de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-hosted-graphite-from-the-gallery"></a>Grafiek die wordt gehost in de galerie toevoegen
Voor het configureren van de integratie van de grafiek die wordt gehost in Azure AD, moet u de grafiek die wordt gehost in de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen die wordt gehost grafiek uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **grafiek die wordt gehost**.

    ![Het maken van een Azure AD-testgebruiker](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_search.png)

1. Selecteer in het deelvenster resultaten **grafiek die wordt gehost**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met gehost grafiek op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de grafiek die wordt gehost in Azure AD aan een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de grafiek die wordt gehost tot stand worden gebracht.

In de grafiek die wordt gehost, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met de grafiek die wordt gehost, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een grafiek die wordt gehost testgebruiker](#creating-a-hosted-graphite-test-user)**  : als u wilt een equivalent van Britta Simon hebben in de grafiek die worden gehost die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing grafiek die wordt gehost.

**Voor het configureren van Azure AD eenmalige aanmelding met de grafiek die wordt gehost, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **grafiek die wordt gehost** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_samlbase.png)

1. Op de **gehoste grafiek domeinen en URL's** sectie, als u wilt configureren van de toepassing in **IDP gestart door modus**, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://www.hostedgraphite.com/metadata/<user id>`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://www.hostedgraphite.com/complete/saml/<user id>`

1. Op de **gehoste grafiek domeinen en URL's** sectie, als u wilt configureren van de toepassing in **SP geïnitieerde modus**, voer de volgende stappen uit:
   
    ![Eenmalige aanmelding configureren](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_10.png)
  
    a. Klik op de **geavanceerde URL-instellingen weergeven** optie

    b. In de **aanmelding URL** tekstvak, een URL met behulp van het volgende patroon: `https://www.hostedgraphite.com/login/saml/<user id>/`   

    > [!NOTE] 
    > Houd er rekening mee dat deze niet de werkelijke waarden zijn. U hebt deze waarden bijwerken met de werkelijke-id, de antwoord-URL en meld u op URL. Als u deze waarden, gaat u naar toegang setup op uw toepassing aan clientzijde of neem contact op met SAML -> [grafiek die wordt gehost ondersteuningsteam](mailto:help@hostedgraphite.com).
    >
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/hostedgraphite-tutorial/tutorial_general_400.png)

1. Op de **gehost grafiek configuratie** sectie, klikt u op **configureren die worden gehost grafiek** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_configure.png) 

1. Aanmelding bij uw tenant grafiek die wordt gehost als beheerder.

1. Ga naar de **SAML-installatiepagina** in de zijbalk (**toegang SAML-instellingen ->**).
   
    ![Configureren van eenmalige aanmelding op App-zijde](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

1. Controleer of deze URL's overeenkomen met uw configuratie uitgevoerd op de **gehoste grafiek domeinen en URL's** sectie van de Azure portal.
   
    ![Configureren van eenmalige aanmelding op App-zijde](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

1. In **entiteit of verlener-ID** en **aanmeldings-URL voor eenmalige aanmelding** tekstvakken, plak de waarde van **SAML entiteit-ID** en **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal. 
   
    ![Configureren van eenmalige aanmelding op App-zijde](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)
   

1. Selecteer "**alleen-lezen**' als **standaard gebruikersrol**.
    
    ![Configureren van eenmalige aanmelding op App-zijde](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

1. Uw base-64 gecodeerde certificaat openen in Kladblok gedownload vanuit Azure portal, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **X.509-certificaat** tekstvak.
    
    ![Configureren van eenmalige aanmelding op App-zijde](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

1. Klik op **opslaan** knop.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/hostedgraphite-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/hostedgraphite-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/hostedgraphite-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/hostedgraphite-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-hosted-graphite-test-user"></a>Het maken van een testgebruiker grafiek die wordt gehost

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in de grafiek die wordt gehost. Gehoste grafiek biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker wordt gemaakt tijdens een poging tot toegang tot de grafiek die wordt gehost als deze nog niet bestaat.

>[!NOTE]
>Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met het ondersteuningsteam van de grafiek die wordt gehost via <mailto:help@hostedgraphite.com>. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan de grafiek die wordt gehost.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon op grafiek die wordt gehost, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **grafiek die wordt gehost**.

    ![Eenmalige aanmelding configureren](./media/hostedgraphite-tutorial/tutorial_hostedgraphite_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van de configuratie van uw Azure AD-eenmalige aanmelding via het toegangsvenster.

Wanneer u op de tegel grafiek die wordt gehost in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing grafiek die wordt gehost.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/hostedgraphite-tutorial/tutorial_general_01.png
[2]: ./media/hostedgraphite-tutorial/tutorial_general_02.png
[3]: ./media/hostedgraphite-tutorial/tutorial_general_03.png
[4]: ./media/hostedgraphite-tutorial/tutorial_general_04.png

[100]: ./media/hostedgraphite-tutorial/tutorial_general_100.png

[200]: ./media/hostedgraphite-tutorial/tutorial_general_200.png
[201]: ./media/hostedgraphite-tutorial/tutorial_general_201.png
[202]: ./media/hostedgraphite-tutorial/tutorial_general_202.png
[203]: ./media/hostedgraphite-tutorial/tutorial_general_203.png

