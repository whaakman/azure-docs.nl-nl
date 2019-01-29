---
title: 'Zelfstudie: Azure Active Directory-integratie met Igloo Software | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Igloo Software.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: a062cd7c56e3915d0b065474879ce679238c3779
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55187535"
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Zelfstudie: Azure Active Directory-integratie met Igloo Software

In deze zelfstudie leert u hoe u Igloo om Software te integreren met Azure Active Directory (Azure AD).

Igloo Software integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Igloo Software heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Igloo Software (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Igloo Software, moet u de volgende items:

- Een Azure AD-abonnement
- Een Igloo Software eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Igloo Software uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-igloo-software-from-the-gallery"></a>Igloo Software uit de galerie toe te voegen
Voor het configureren van de integratie van Igloo Software in Azure AD, moet u Igloo Software uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Igloo Software uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Igloo Software**.

    ![Het maken van een Azure AD-testgebruiker](./media/igloo-software-tutorial/tutorial_igloosoftware_search.png)

1. Selecteer in het deelvenster resultaten **Igloo Software**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/igloo-software-tutorial/tutorial_igloosoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Igloo-Software op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Igloo Software is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Software Igloo tot stand worden gebracht.

Wijs in Igloo Software, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en Azure AD eenmalige aanmelding met Igloo Software testen, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Igloo Software](#creating-an-igloo-software-test-user)**  : als u wilt een equivalent van Britta Simon in Igloo-Software die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Igloo Software.

**Voor het configureren van Azure AD eenmalige aanmelding met Igloo Software, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Igloo Software** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/igloo-software-tutorial/tutorial_igloosoftware_samlbase.png)

1. Op de **Igloo Software domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/igloo-software-tutorial/tutorial_igloosoftware_url.png)
    
    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<company name>.igloocommmunities.com`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<company name>.igloocommmunities.com/saml.digest`

    c. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<company name>.igloocommmunities.com/saml.digest`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. Neem contact op met [Igloo softwareclient ondersteuningsteam](https://www.igloosoftware.com/services/support) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/igloo-software-tutorial/tutorial_igloosoftware_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/igloo-software-tutorial/tutorial_general_400.png)
    
1. Op de **Igloo softwareconfiguratie** sectie, klikt u op **Igloo Software configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding URL's en SAML Single Sign-On Service** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/igloo-software-tutorial/tutorial_igloosoftware_configure.png) 

1. In een ander browservenster aanmelden bij uw bedrijf Igloo Software site als beheerder.

1. Ga naar de **Configuratiescherm**.
   
     ![Configuratiescherm](./media/igloo-software-tutorial/ic799949.png "Configuratiescherm")

1. In de **lidmaatschap** tabblad **Sign In instellingen**.
   
    ![Meld u aan instellingen](./media/igloo-software-tutorial/ic783968.png "Aanmeldinstellingen")

1. Klik in de sectie SAML-configuratie **SAML-verificatie configureren**.
   
    ![SAML-configuratie](./media/igloo-software-tutorial/ic783969.png "SAML-configuratie")
   
1. In de **algemene configuratie** sectie, voert u de volgende stappen uit:
   
    ![Algemene configuratie](./media/igloo-software-tutorial/ic783970.png "algemene configuratie")

    a. In de **verbindingsnaam** tekstvak typt u een aangepaste naam voor uw configuratie.
   
    b. In de **IdP aanmeldings-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.
   
    c. In de **afmeldings-URL van id-provider** tekstvak, plak de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal.
    
    d. Selecteer **afmelden antwoord en Type van de HTTP-aanvraag** als **POST**.
   
    e. Open uw **base-64** gecodeerd certificaat in Kladblok gedownload vanuit Azure portal, de inhoud van het kopiëren naar het Klembord en plakt u deze naar de **openbaar certificaat** tekstvak.
    
1. In de **antwoord en verificatieconfiguratie**, voer de volgende stappen uit:
    
    ![Antwoord en verificatieconfiguratie](./media/igloo-software-tutorial/IC783971.png "antwoord en Authentication-configuratie")
  
      a. Als **id-Provider**, selecteer **Microsoft ADFS**.
      
      b. Als **ID-Type**, selecteer **e-mailadres**. 

      c. In de **e kenmerk** tekstvak, type **emailaddress**.

      d. In de **voornaam kenmerk** tekstvak, type **givenname**.

      e. In de **laatste naamkenmerk** tekstvak, type **achternaam**.

1. Voer de volgende stappen uit om de configuratie te voltooien:
    
    ![Het maken van de gebruiker op aanmelden](./media/igloo-software-tutorial/IC783972.png "maken door gebruiker op aanmelden") 

     a. Als **maken door gebruiker op aanmelden**, selecteer **een nieuwe gebruiker in uw site maken wanneer ze zich aanmelden**.

     b. Als **Aanmeldinstellingen**, selecteer **gebruik SAML-knop op het scherm 'Aanmelden'**.

     c. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/igloo-software-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/igloo-software-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/igloo-software-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/igloo-software-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-igloo-software-test-user"></a>Het maken van een testgebruiker Igloo Software

Er is geen actie-item voor u het inrichten van gebruikers naar Software Igloo configureren.  

Wanneer een toegewezen gebruiker wil zich aanmelden bij Igloo Software via het toegangsvenster, controleert Igloo Software of de gebruiker bestaat.  Als er nog geen gebruikersaccount beschikbaar is, wordt deze automatisch gemaakt door Igloo Software.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot Igloo Software.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon Igloo software, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Igloo Software**.

    ![Eenmalige aanmelding configureren](./media/igloo-software-tutorial/tutorial_igloosoftware_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Igloo Software in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Igloo Software.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/igloo-software-tutorial/tutorial_general_01.png
[2]: ./media/igloo-software-tutorial/tutorial_general_02.png
[3]: ./media/igloo-software-tutorial/tutorial_general_03.png
[4]: ./media/igloo-software-tutorial/tutorial_general_04.png

[100]: ./media/igloo-software-tutorial/tutorial_general_100.png

[200]: ./media/igloo-software-tutorial/tutorial_general_200.png
[201]: ./media/igloo-software-tutorial/tutorial_general_201.png
[202]: ./media/igloo-software-tutorial/tutorial_general_202.png
[203]: ./media/igloo-software-tutorial/tutorial_general_203.png

