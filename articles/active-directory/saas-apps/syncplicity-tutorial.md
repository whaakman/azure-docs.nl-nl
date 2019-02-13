---
title: 'Zelfstudie: Azure Active Directory-integratie met Syncplicity | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Syncplicity.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3df48fd42ca998e2a64f2fbe685047da16a8bae7
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56193476"
---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a>Zelfstudie: Azure Active Directory-integratie met Syncplicity

In deze zelfstudie leert u hoe u Syncplicity integreren met Azure Active Directory (Azure AD).

Syncplicity integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Syncplicity heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Syncplicity (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Syncplicity, moet u de volgende items:

- Een Azure AD-abonnement
- Een Syncplicity eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Syncplicity uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-syncplicity-from-the-gallery"></a>Syncplicity uit de galerie toe te voegen
Voor het configureren van de integratie van Syncplicity in Azure AD, moet u Syncplicity uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Syncplicity uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Syncplicity**.

    ![Het maken van een Azure AD-testgebruiker](./media/syncplicity-tutorial/tutorial_syncplicity_search.png)

1. Selecteer in het deelvenster resultaten **Syncplicity**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/syncplicity-tutorial/tutorial_syncplicity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Syncplicity op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Syncplicity is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Syncplicity tot stand worden gebracht.

In Syncplicity, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Syncplicity, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Syncplicity](#creating-a-syncplicity-test-user)**  : als u wilt een equivalent van Britta Simon in Syncplicity die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Syncplicity.

**Voor het configureren van Azure AD eenmalige aanmelding met Syncplicity, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Syncplicity** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/syncplicity-tutorial/tutorial_syncplicity_samlbase.png)

1. Op de **Syncplicity domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/syncplicity-tutorial/tutorial_syncplicity_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<companyname>.syncplicity.com`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<companyname>.syncplicity.com/sp`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [Syncplicity Client ondersteuningsteam](https://www.syncplicity.com/contact-us) om deze waarden te verkrijgen. 
 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/syncplicity-tutorial/tutorial_syncplicity_certificate.png) 

  
1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/syncplicity-tutorial/tutorial_general_400.png)

1. Op de **Syncplicity configuratie** sectie, klikt u op **configureren Syncplicity** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/syncplicity-tutorial/tutorial_syncplicity_configure.png) 

1. Aanmelden bij uw **Syncplicity** tenant.

1. Klik in het menu aan de bovenkant op **admin**, selecteer **instellingen**, en klik vervolgens op **aangepast domein en eenmalige aanmelding**.
   
    ![Syncplicity](./media/syncplicity-tutorial/ic769545.png "Syncplicity")

1. Op de **eenmalige aanmelding (SSO)** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Single Sign-On \(eenmalige aanmelding\)](./media/syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")   

    a. In de **Custom Domain** tekstvak typt u de naam van uw domein.
  
    b. Selecteer **ingeschakeld** als **Single Sign-On Status**.

    c. In de **entiteit-Id** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.

    d. In de **aanmelden pagina-URL** tekstvak, plak de **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.

    e. In de **afmeldings-URL van pagina** tekstvak, plak de **afmelding URL** die u hebt gekopieerd vanuit Azure portal.

    f. In **Provider identiteitscertificaat**, klikt u op **bestand kiezen**, en upload het certificaat dat u hebt gedownload vanuit Azure portal. 

    g. Klik op **WIJZIGINGEN OPSLAAN**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/syncplicity-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/syncplicity-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/syncplicity-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/syncplicity-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-syncplicity-test-user"></a>Het maken van een testgebruiker Syncplicity
Voor AAD-gebruikers kunnen zich aanmelden, moeten ze worden ingericht tot Syncplicity toepassing. In deze sectie wordt beschreven hoe u AAD-gebruikersaccounts maken in Syncplicity.

**Voor het inrichten van een gebruikersaccount Syncplicity, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Syncplicity** tenant (bijvoorbeeld: `https://company.Syncplicity.com`).

1. Klik op **admin** en selecteer **gebruikersaccounts**.

1. Klik op **toevoegen van een gebruiker**.
   
    ![Gebruikers beheren](./media/syncplicity-tutorial/ic769764.png "gebruikers beheren")

1. Type de **e-adressen** van een AAD-account dat u inrichten wilt, selecteer **gebruiker** als **rol**, en klik vervolgens op **volgende**.
   
    ![Accountgegevens](./media/syncplicity-tutorial/ic769765.png "accountgegevens")
   
    >[!NOTE]
    >De houder van AAD-account ontvangt een e-mail met inbegrip van een koppeling om te bevestigen en activeren van het account. 
    > 

1. Selecteer een groep in uw bedrijf dat de nieuwe gebruiker moet lid zijn van, en klik vervolgens op **volgende**.
   
    ![Groepslidmaatschap](./media/syncplicity-tutorial/ic769772.png "groepslidmaatschap")
   
    >[!NOTE]
    >Als er geen groepen die worden vermeld zijn, klikt u op **volgende**. 
    > 

1. Selecteer de mappen die u wilt onder Syncplicity van beheer op de computer van de gebruiker, en klik vervolgens op **volgende**.
   
    ![Syncplicity mappen](./media/syncplicity-tutorial/ic769773.png "Syncplicity mappen")

>[!NOTE]
>U kunt alle andere Syncplicity gebruiker-account maken van hulpprogramma's of API's geleverd door Syncplicity aan inrichten AAD-gebruikersaccounts. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Syncplicity.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Syncplicity toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Syncplicity**.

    ![Eenmalige aanmelding configureren](./media/syncplicity-tutorial/tutorial_syncplicity_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.

Wanneer u op de tegel Syncplicity in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Syncplicity.
## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/syncplicity-tutorial/tutorial_general_01.png
[2]: ./media/syncplicity-tutorial/tutorial_general_02.png
[3]: ./media/syncplicity-tutorial/tutorial_general_03.png
[4]: ./media/syncplicity-tutorial/tutorial_general_04.png

[100]: ./media/syncplicity-tutorial/tutorial_general_100.png

[200]: ./media/syncplicity-tutorial/tutorial_general_200.png
[201]: ./media/syncplicity-tutorial/tutorial_general_201.png
[202]: ./media/syncplicity-tutorial/tutorial_general_202.png
[203]: ./media/syncplicity-tutorial/tutorial_general_203.png

