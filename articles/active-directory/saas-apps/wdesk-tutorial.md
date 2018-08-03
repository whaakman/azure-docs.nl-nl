---
title: 'Zelfstudie: Azure Active Directory-integratie met Wdesk | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Wdesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 9ff4d38240bf44cdb3112730b6f6962feed09a6a
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444433"
---
# <a name="tutorial-azure-active-directory-integration-with-wdesk"></a>Zelfstudie: Azure Active Directory-integratie met Wdesk

In deze zelfstudie leert u hoe u Wdesk integreren met Azure Active Directory (Azure AD).

Wdesk integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Wdesk heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Wdesk (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, raadpleegt u. [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Wdesk, moet u de volgende items:

- Een Azure AD-abonnement
- Een Wdesk eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Wdesk uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-wdesk-from-the-gallery"></a>Wdesk uit de galerie toe te voegen
Voor het configureren van de integratie van Wdesk in Azure AD, moet u Wdesk uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Wdesk uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **Wdesk**.

    ![Het maken van een Azure AD-testgebruiker](./media/wdesk-tutorial/tutorial_wdesk_search.png)

1. Selecteer in het deelvenster resultaten **Wdesk**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/wdesk-tutorial/tutorial_wdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Wdesk op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Wdesk is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Wdesk tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Wdesk.

Om te configureren en testen van Azure AD eenmalige aanmelding met Wdesk, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Wdesk](#creating-a-wdesk-test-user)**  : als u wilt een equivalent van Britta Simon in Wdesk die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Wdesk.

**Voor het configureren van Azure AD eenmalige aanmelding met Wdesk, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Wdesk** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_samlbase.png)

1. Op de **Wdesk domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP** gestart modus de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`

1. Controleer **geavanceerde URL-instellingen weergeven**. Als u wilt configureren van de toepassing in **SP** gestart modus, de volgende stap uitvoeren:

    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. U krijgt deze waarden van WDesk portal bij het configureren van de eenmalige aanmelding. 
  
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_general_400.png)
    
1. In een ander browservenster, meld u aan bij Wdesk als een beveiligingsbeheerder.

1. In de onderaan, links, klikt u op **Admin** en kies **accountbeheerder**:
 
     ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

1. Navigeer in Wdesk Admin, naar **Security**, klikt u vervolgens **SAML** > **SAML-instellingen**:

    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

1. Onder **algemene instellingen**, Controleer de **SAML eenmalige aanmelding inschakelen**:

    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

1. Onder **Details van Service Provider**, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      a. Kopieer de **aanmeldings-URL** en plak deze in **aanmeldings-Url** tekstvak in Azure portal.
   
      b. Kopieer de **metagegevens-Url** en plak deze in **id** tekstvak in Azure portal.
       
      c. Kopieer de **consument url** en plak deze in **antwoord-Url** tekstvak in Azure portal.
   
      d. Klik op **opslaan** in Azure portal de wijzigingen op te slaan.      

1. Klik op **IdP-instellingen configureren** openen **IdP-instellingen bewerken** dialoogvenster. Klik op **bestand kiezen** vinden de **Metadata.xml** bestand dat u hebt opgeslagen in Azure portal, klikt u vervolgens uploaden.
    
    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
1. Klik op **wijzigingen opslaan**.

    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/wdesk-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/wdesk-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/wdesk-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/wdesk-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-wdesk-test-user"></a>Het maken van een testgebruiker Wdesk

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Wdesk, moeten ze worden ingericht voor Wdesk. In Wdesk is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij Wdesk als een beveiligingsbeheerder.
1. Navigeer naar **Admin** > **Admin Account**.

     ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

1. Klik op **leden** onder **mensen**.

1. Klik nu op **Add Member** openen **Add Member** in het dialoogvenster. 
   
    ![Het maken van een Azure AD-testgebruiker](./media/wdesk-tutorial/createuser1.png)  

1. In **gebruiker** tekst voert u de gebruikersnaam van gebruiker, zoals **brittasimon@contoso.com** en klikt u op **doorgaan** knop.

    ![Het maken van een Azure AD-testgebruiker](./media/wdesk-tutorial/createuser3.png)

1.  Geef de details, zoals hieronder weergegeven:
  
    ![Het maken van een Azure AD-testgebruiker](./media/wdesk-tutorial/createuser4.png)
 
    a. In **e** tekst vak, voer het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.

    b. In **voornaam** tekst voert u de voornaam van de gebruiker, zoals **Julia**.

    c. In **achternaam** tekst voert u de achternaam van de gebruiker, zoals **Simon**.

1. Klik op **lid opslaan** knop.  

    ![Het maken van een Azure AD-testgebruiker](./media/wdesk-tutorial/createuser5.png)

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Wdesk.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Wdesk toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Wdesk**.

    ![Eenmalige aanmelding configureren](./media/wdesk-tutorial/tutorial_wdesk_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Wdesk in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Wdesk.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/wdesk-tutorial/tutorial_general_01.png
[2]: ./media/wdesk-tutorial/tutorial_general_02.png
[3]: ./media/wdesk-tutorial/tutorial_general_03.png
[4]: ./media/wdesk-tutorial/tutorial_general_04.png

[100]: ./media/wdesk-tutorial/tutorial_general_100.png

[200]: ./media/wdesk-tutorial/tutorial_general_200.png
[201]: ./media/wdesk-tutorial/tutorial_general_201.png
[202]: ./media/wdesk-tutorial/tutorial_general_202.png
[203]: ./media/wdesk-tutorial/tutorial_general_203.png

