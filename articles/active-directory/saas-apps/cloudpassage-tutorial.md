---
title: 'Zelfstudie: Azure Active Directory-integratie met CloudPassage | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en CloudPassage.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: bfe1f14e-74e4-4680-ac9e-f7355e1c94cc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: d8bddc309db3ede67586a2067fcf57fbede7d6b3
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55152787"
---
# <a name="tutorial-azure-active-directory-integration-with-cloudpassage"></a>Zelfstudie: Azure Active Directory-integratie met CloudPassage

In deze zelfstudie leert u hoe u CloudPassage integreren met Azure Active Directory (Azure AD).

CloudPassage integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot CloudPassage heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij CloudPassage (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met CloudPassage, moet u de volgende items:

- Een Azure AD-abonnement
- Een CloudPassage eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. CloudPassage uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-cloudpassage-from-the-gallery"></a>CloudPassage uit de galerie toe te voegen
Voor het configureren van de integratie van CloudPassage in Azure AD, moet u CloudPassage uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen CloudPassage uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **CloudPassage**.

    ![Het maken van een Azure AD-testgebruiker](./media/cloudpassage-tutorial/tutorial_cloudpassage_search.png)

1. Selecteer in het deelvenster resultaten **CloudPassage**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/cloudpassage-tutorial/tutorial_cloudpassage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met CloudPassage op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in CloudPassage is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in CloudPassage tot stand worden gebracht.

In CloudPassage, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met CloudPassage, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker CloudPassage](#creating-a-cloudpassage-test-user)**  : als u wilt een equivalent van Britta Simon in CloudPassage die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing CloudPassage.

**Voor het configureren van Azure AD eenmalige aanmelding met CloudPassage, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **CloudPassage** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/cloudpassage-tutorial/tutorial_cloudpassage_samlbase.png)

1. Op de **CloudPassage domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/cloudpassage-tutorial/tutorial_cloudpassage_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://portal.cloudpassage.com/saml/init/accountid`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://portal.cloudpassage.com/saml/consume/accountid`. U kunt de waarde voor dit kenmerk opvragen door te klikken op **SSO ondersteuningsdocumentatie** in de **instellingen voor eenmalige aanmelding** sectie van uw CloudPassage-portal.

    ![Eenmalige aanmelding configureren](./media/cloudpassage-tutorial/tutorial_cloudpassage_05.png)
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke antwoord-URL en de aanmeldings-URL. Neem contact op met [CloudPassage Client ondersteuningsteam](https://www.cloudpassage.com/company/contact/) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/cloudpassage-tutorial/tutorial_cloudpassage_certificate.png) 

1. Uw toepassing CloudPassage wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermopname ziet u een voorbeeld hiervan.
   
   ![Eenmalige aanmelding configureren](./media/cloudpassage-tutorial/tutorial_cloudpassage_25.png) 

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:

    | Naam kenmerk | Waarde kenmerk |
    | --- | --- |
    | firstname |user.givenname |
    | lastname |user.surname |
    | e-mail |user.mail |
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/cloudpassage-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/cloudpassage-tutorial/tutorial_attribute_05.png)
    
    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/cloudpassage-tutorial/tutorial_general_400.png)
    
1. Op de **CloudPassage configuratie** sectie, klikt u op **configureren CloudPassage** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/cloudpassage-tutorial/tutorial_cloudpassage_configure.png) 

1. In een ander browservenster aanmelden voor uw bedrijf CloudPassage site als administrator.

1. Klik in het menu aan de bovenkant op **instellingen**, en klik vervolgens op **Sitebeheer**. 
   
    ![Eenmalige aanmelding configureren][12]

1. Klik op de **verificatie-instellingen** tabblad. 
   
    ![Eenmalige aanmelding configureren][13]

1. In de **instellingen voor eenmalige aanmelding** sectie, voert u de volgende stappen uit: 
   
    ![Eenmalige aanmelding configureren][14]

    a. Selecteer **één inschakelen sign-on(SSO) (Setup-documentatie, eenmalige aanmelding)** selectievakje.
    
    b. Plakken **SAML entiteit-ID** in de **URL voor SAML-verlener** tekstvak.
  
    c. Plakken **Single Sign-On Service URL voor SAML** in de **eindpunt-URL voor SAML** tekstvak.
  
    d. Plakken **afmelding URL** in de **afmelden landingspagina** tekstvak.
  
    e. Open het gedownloade certificaat in Kladblok, Kopieer de inhoud van het gedownloade certificaat in het Klembord en plak deze in de **x 509-certificaat** tekstvak.
  
    f. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/cloudpassage-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/cloudpassage-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/cloudpassage-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/cloudpassage-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-cloudpassage-test-user"></a>Het maken van een testgebruiker CloudPassage

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in CloudPassage.

**Voor het maken van een gebruiker met de naam van Britta Simon in CloudPassage, moet u de volgende stappen uitvoeren:**

1. Aanmelding bij uw **CloudPassage** bedrijf site als beheerder. 

1. Klik in de werkbalk bovenaan op **instellingen**, en klik vervolgens op **Sitebeheer**. 
   
   ![Het maken van een testgebruiker CloudPassage][22] 

1. Klik op de **gebruikers** tabblad en klik vervolgens op **Add New User**. 
   
   ![Het maken van een testgebruiker CloudPassage][23]

1. In de **Add New User** sectie, voert u de volgende stappen uit: 
   
   ![Het maken van een testgebruiker CloudPassage][24]
    
    a. In de **voornaam** tekstvak typt Julia. 
  
    b. In de **achternaam** tekstvak typt Simon.
  
    c. In de **gebruikersnaam** tekstvak de **e** tekstvak en de **e-mailadres opnieuw** tekstvak, typ de gebruikersnaam van Julia in Azure AD.
  
    d. Als **toegangstype**, selecteer **Halo Portal toegang inschakelen**.
  
    e. Klik op **Add**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan CloudPassage.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan CloudPassage toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **CloudPassage**.

    ![Eenmalige aanmelding configureren](./media/cloudpassage-tutorial/tutorial_cloudpassage_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van de configuratie van uw Azure AD-eenmalige aanmelding via het toegangsvenster.

Wanneer u op de tegel CloudPassage in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing CloudPassage.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/cloudpassage-tutorial/tutorial_general_04.png
[12]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_10.png
[22]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_17.png

[100]: ./media/cloudpassage-tutorial/tutorial_general_100.png

[200]: ./media/cloudpassage-tutorial/tutorial_general_200.png
[201]: ./media/cloudpassage-tutorial/tutorial_general_201.png
[202]: ./media/cloudpassage-tutorial/tutorial_general_202.png
[203]: ./media/cloudpassage-tutorial/tutorial_general_203.png

