---
title: 'Zelfstudie: Azure Active Directory-integratie met Intralinks | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Intralinks.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 147f2bf9-166b-402e-adc4-4b19dd336883
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: fbe3ceb3d26de90bd46ee02263d09c822ee90c29
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868844"
---
# <a name="tutorial-azure-active-directory-integration-with-intralinks"></a>Zelfstudie: Azure Active Directory-integratie met Intralinks

In deze zelfstudie leert u hoe u Intralinks integreren met Azure Active Directory (Azure AD).

Intralinks integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Intralinks heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Intralinks (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Intralinks, moet u de volgende items:

- Een Azure AD-abonnement
- Een Intralinks eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Intralinks uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-intralinks-from-the-gallery"></a>Intralinks uit de galerie toe te voegen
Voor het configureren van de integratie van Intralinks in Azure AD, moet u Intralinks uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Intralinks uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Intralinks**.

    ![Het maken van een Azure AD-testgebruiker](./media/intralinks-tutorial/tutorial_intralinks_search.png)

5. Selecteer in het deelvenster resultaten **Intralinks**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/intralinks-tutorial/tutorial_intralinks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Intralinks op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Intralinks is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Intralinks tot stand worden gebracht.

In Intralinks, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Intralinks, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Intralinks](#creating-an-intralinks-test-user)**  : als u wilt een equivalent van Britta Simon in Intralinks die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Intralinks.

**Voor het configureren van Azure AD eenmalige aanmelding met Intralinks, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Intralinks** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/intralinks-tutorial/tutorial_intralinks_samlbase.png)

3. Op de **Intralinks domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/intralinks-tutorial/tutorial_intralinks_url.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon:  `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`

    > [!NOTE] 
    > Deze waarde is niet echt. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [Intralinks Client ondersteuningsteam](https://www.intralinks.com/contact-1) deze waarde op te halen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/intralinks-tutorial/tutorial_intralinks_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/intralinks-tutorial/tutorial_general_400.png)

6. Het configureren van eenmalige aanmelding op **Intralinks** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** [Intralinks ondersteuningsteam](https://www.intralinks.com/contact-1). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/intralinks-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/intralinks-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/intralinks-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/intralinks-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-intralinks-test-user"></a>Het maken van een testgebruiker Intralinks

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Intralinks. Neem contact op met [Intralinks ondersteuningsteam](https://www.intralinks.com/contact-1) om toe te voegen de gebruikers in het Intralinks-platform.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Intralinks.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Intralinks toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Intralinks**.

    ![Eenmalige aanmelding configureren](./media/intralinks-tutorial/tutorial_intralinks_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="add-intralinks-via-or-elite-application"></a>Intralinks VIA of Elite toepassing toevoegen

Intralinks maakt gebruik van de dezelfde identity-platform van eenmalige aanmelding voor alle andere Intralinks toepassingen met uitzondering van Deal Nexus-toepassing. Dus als u van plan bent te gebruiken een andere Intralinks-toepassing hebt eerst u voor het configureren van eenmalige aanmelding voor één primaire Intralinks toepassing met behulp van de procedure die hierboven worden beschreven.

Daarna voert u de onderstaande procedure voor het toevoegen van een andere Intralinks-toepassing in uw tenant die u kunt gebruikmaken van deze primaire toepassing voor eenmalige aanmelding. 

>[!NOTE]
>Deze functie is alleen beschikbaar voor klanten van Azure AD Premium-SKU en niet beschikbaar voor klanten van gratis of Basic SKU.

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]


2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Intralinks**.

    ![Het maken van een Azure AD-testgebruiker](./media/intralinks-tutorial/tutorial_intralinks_search.png)

5. Op **app Intralinks toevoegen** de volgende stappen uitvoeren:

    ![Intralinks VIA of Elite toepassing toe te voegen](./media/intralinks-tutorial/tutorial_intralinks_addapp.png)

    a. In **naam** tekstvak, voer juiste naam van de toepassing bijvoorbeeld **Intralinks Elite**.

    b. Klik op **toevoegen** knop.

6.  In de Azure-portal op de **Intralinks** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

7. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **gekoppelde Sign-on**.
 
    ![Eenmalige aanmelding configureren](./media/intralinks-tutorial/tutorial_intralinks_linkedsignon.png)

8. Om de URL SP geïnitieerde eenmalige aanmelding vanaf ophalen [Intralinks team](https://www.intralinks.com/contact-1) voor de andere Intralinks-toepassing en voer deze in **configureren aanmeldings-URL** zoals hieronder wordt weergegeven. 
    
     ![Eenmalige aanmelding configureren](./media/intralinks-tutorial/tutorial_intralinks_customappurl.png)
    
     Typ in het teken op URL-tekstvak, de URL die wordt gebruikt door uw gebruikers aan te melden bij uw Intralinks-toepassing met behulp van het volgende patroon:
   
    `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`

9. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/intralinks-tutorial/tutorial_general_400.png)

10. De toepassing toewijzen aan gebruikers of groepen zoals wordt weergegeven in de sectie  **[toewijzen aan de gebruiker van de test Azure AD](#assigning-the-azure-ad-test-user)**.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Intralinks in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Intralinks.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/intralinks-tutorial/tutorial_general_01.png
[2]: ./media/intralinks-tutorial/tutorial_general_02.png
[3]: ./media/intralinks-tutorial/tutorial_general_03.png
[4]: ./media/intralinks-tutorial/tutorial_general_04.png

[100]: ./media/intralinks-tutorial/tutorial_general_100.png

[200]: ./media/intralinks-tutorial/tutorial_general_200.png
[201]: ./media/intralinks-tutorial/tutorial_general_201.png
[202]: ./media/intralinks-tutorial/tutorial_general_202.png
[203]: ./media/intralinks-tutorial/tutorial_general_203.png

