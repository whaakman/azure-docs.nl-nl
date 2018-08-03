---
title: 'Zelfstudie: Azure Active Directory-integratie met herkennen | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en herkennen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: b2d5acfcb722845d7f346668597c073319f273f0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39448781"
---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Zelfstudie: Azure Active Directory-integratie met herkennen

In deze zelfstudie leert u hoe u kunt herkennen integreren met Azure Active Directory (Azure AD).

Herkennen integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot herkennen heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij herkennen (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met herkennen, moet u de volgende items:

- Een Azure AD-abonnement
- Een herkennen eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u een proefversie van één maand hier downloaden: [proefversie aanbieding](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Herkennen in de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-recognize-from-the-gallery"></a>Herkennen in de galerie toevoegen
Voor het configureren van de integratie van herkennen in Azure AD, moet u herkennen in de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voor het herkennen van de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **herkennen**.

    ![Het maken van een Azure AD-testgebruiker](./media/recognize-tutorial/tutorial_recognize_search.png)

1. Selecteer in het deelvenster resultaten **herkennen**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/recognize-tutorial/tutorial_recognize_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met herkennen op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in herkennen is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in herkennen tot stand worden gebracht.

In herkennen, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met herkennen, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker herkennen](#creating-a-recognize-test-user)**  : als u wilt een equivalent van Britta Simon in herkennen die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing herkennen.

**Voor het configureren van Azure AD eenmalige aanmelding met herkennen, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **herkennen** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/recognize-tutorial/tutorial_recognize_samlbase.png)

1. Op de **herkennen domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/recognize-tutorial/tutorial_recognize_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://recognizeapp.com/<your-domain>/saml/sso`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://recognizeapp.com/<your-domain>`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [Client herkent ondersteuningsteam](mailto:support@recognizeapp.com) aanmeldings-URL en u id-waarde kunt krijgen door het openen van de Service Provider metagegevens-URL uit de sectie SSO-instellingen die later in de zelfstudie wordt uitgelegd. . 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/recognize-tutorial/tutorial_recognize_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/recognize-tutorial/tutorial_general_400.png)

1. Op de **herkent Configuration** sectie, klikt u op **configureren herkennen** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/recognize-tutorial/tutorial_recognize_configure.png) 

1. In een ander browservenster aanmelden voor uw tenant herkennen als een beheerder.

1. Klik in de rechterbovenhoek op **Menu**. Ga naar **beheerder van de bedrijfsportal**.
   
    ![Configureren van eenmalige aanmelding op App-zijde](./media/recognize-tutorial/tutorial_recognize_000.png)

1. Klik in het linkernavigatiedeelvenster op **instellingen**.
   
    ![Configureren van eenmalige aanmelding op App-zijde](./media/recognize-tutorial/tutorial_recognize_001.png)

1. Voer de volgende stappen uit op **SSO-instellingen** sectie.
   
    ![Configureren van eenmalige aanmelding op App-zijde](./media/recognize-tutorial/tutorial_recognize_002.png)
    
    a. Als **SSO inschakelen**, selecteer **ON**.

    b. In de **IDP entiteit-ID** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.
    
    c. In de **doel-url voor eenmalige aanmelding** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.
    
    d. In de **Slo-doel-url** tekstvak, plak de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal. 
    
    e. Open uw gedownloade **certificaat (Base64)** -bestand in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **certificaat** tekstvak.
    
    f. Klik op de **instellingen opslaan** knop. 

1. Naast de **SSO-instellingen** sectie, Kopieer de URL onder **Service Provider metagegevens-url**.
   
    ![Configureren van eenmalige aanmelding op App-zijde](./media/recognize-tutorial/tutorial_recognize_003.png)

1. Open de **metagegevens-URL-koppeling** onder een lege browser voor het downloaden van het document met metagegevens. Vervolgens kopieert u de value(entityID) EntityDescriptor uit het bestand en plak deze in **id** -tekstvak in **herkennen domein en URL's sectie** in Azure portal.
    
    ![Configureren van eenmalige aanmelding op App-zijde](./media/recognize-tutorial/tutorial_recognize_004.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/recognize-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/recognize-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/recognize-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/recognize-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-recognize-test-user"></a>Het maken van een testgebruiker herkennen

Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij herkennen, moeten ze worden ingericht voor herkennen. In het geval van herkennen is inrichten een handmatige taak.

Deze app biedt geen ondersteuning voor SCIM wordt ingericht, maar heeft een andere gebruiker synchronisatie die gebruikers inricht. 

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u in uw bedrijf herkennen site als beheerder.

1. Klik in de rechterbovenhoek op **Menu**. Ga naar **beheerder van de bedrijfsportal**.

1. Klik in het linkernavigatiedeelvenster op **instellingen**.

1. Voer de volgende stappen uit op **gebruiker synchronisatie** sectie.
   
   ![Nieuwe gebruiker](./media/recognize-tutorial/tutorial_recognize_005.png "nieuwe gebruiker")
   
   a. Als **synchronisatie ingeschakeld**, selecteer **ON**.
   
   b. Als **kiezen synchronisatie provider**, selecteer **Microsoft / Office 365**.
   
   c. Klik op **gebruiker synchronisatie uitvoeren**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen om te herkennen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon te herkennen, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **herkennen**.

    ![Eenmalige aanmelding configureren](./media/recognize-tutorial/tutorial_recognize_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.

Wanneer u op de tegel herkennen in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing herkennen. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/recognize-tutorial/tutorial_general_01.png
[2]: ./media/recognize-tutorial/tutorial_general_02.png
[3]: ./media/recognize-tutorial/tutorial_general_03.png
[4]: ./media/recognize-tutorial/tutorial_general_04.png

[100]: ./media/recognize-tutorial/tutorial_general_100.png

[200]: ./media/recognize-tutorial/tutorial_general_200.png
[201]: ./media/recognize-tutorial/tutorial_general_201.png
[202]: ./media/recognize-tutorial/tutorial_general_202.png
[203]: ./media/recognize-tutorial/tutorial_general_203.png

