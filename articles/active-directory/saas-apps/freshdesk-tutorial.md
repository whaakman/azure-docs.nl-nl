---
title: 'Zelfstudie: Azure Active Directory-integratie met FreshDesk | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en FreshDesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: d0fbed347805a581fb66e0218290993817277214
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39428329"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Zelfstudie: Azure Active Directory-integratie met FreshDesk

In deze zelfstudie leert u hoe u FreshDesk integreren met Azure Active Directory (Azure AD).

FreshDesk integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot FreshDesk heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij FreshDesk (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - de Azure Management portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met FreshDesk, moet u de volgende items:

- Een Azure AD-abonnement
- Een FreshDesk eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productie-omgeving, niet gebruiken als dit nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. FreshDesk uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-freshdesk-from-the-gallery"></a>FreshDesk uit de galerie toe te voegen
Voor het configureren van de integratie van FreshDesk in Azure AD, moet u FreshDesk uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen FreshDesk uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **FreshDesk**.

    ![Het maken van een Azure AD-testgebruiker](./media/freshdesk-tutorial/tutorial_freshdesk_search.png)

1. Selecteer in het deelvenster resultaten **FreshDesk**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met FreshDesk op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in FreshDesk is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in FreshDesk tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in FreshDesk.

Om te configureren en testen van Azure AD eenmalige aanmelding met FreshDesk, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker FreshDesk](#creating-a-freshdesk-test-user)**  : als u wilt een equivalent van Britta Simon in FreshDesk die is gekoppeld aan de Azure AD-weergave van haar hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure Management portal inschakelen en configureren van eenmalige aanmelding in uw FreshDesk-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met FreshDesk, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, op de **FreshDesk** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **SAML gebaseerde aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

1. Op de **FreshDesk-domein en URL's** sectie, voer de **aanmeldings-URL** als: `https://<tenant-name>.freshdesk.com` of een andere waarde Freshdesk is voorgesteld.

    ![Eenmalige aanmelding configureren](./media/freshdesk-tutorial/tutorial_freshdesk_url.png)

    > [!NOTE] 
    > Houd er rekening mee dat dit niet de werkelijke waarde is. U moet de waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [FreshDesk-Client-ondersteuningsteam](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) deze waarde op te halen.  

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat** en sla het certificaat op uw computer.

    ![Eenmalige aanmelding configureren](./media/freshdesk-tutorial/tutorial_freshdesk_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/freshdesk-tutorial/tutorial_general_400.png)

1. Op de **FreshDesk configuratie** sectie, klikt u op **FreshDesk configureren** om configureren aanmeldings-venster te openen. Kopieer de SAML Single Sign-On Service-URL en de URL van de afmelding van de **Naslaggids** sectie.

    ![Eenmalige aanmelding configureren](./media/freshdesk-tutorial/tutorial_freshdesk_configure.png)

1. Meld u in een ander browservenster in uw bedrijf Freshdesk site als beheerder.

1. Klik in het menu aan de bovenkant op **Admin**.
   
   ![Beheerder](./media/freshdesk-tutorial/IC776768.png "Admin")

1. In de **algemene instellingen** tabblad **Security**.
   
   ![Beveiliging](./media/freshdesk-tutorial/IC776769.png "beveiliging")

1. In de **Security** sectie, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding](./media/freshdesk-tutorial/IC776770.png "eenmalige aanmelding")
   
    a. Voor **eenmalige aanmelding (SSO)**, selecteer **op**.

    b. Selecteer **SAML SSO**.

    c. Type de **Single Sign-On Service URL voor SAML** u hebt gekopieerd vanuit Azure portal in de **aanmeldings-URL voor SAML** tekstvak.

    d. Type de **afmelding URL** u hebt gekopieerd vanuit Azure portal in de **afmeldings-URL van** tekstvak.

    e. Kopieer de **vingerafdruk** waarde uit het gedownloade certificaat vanuit Azure portal en plak deze in de **Security certificaat vingerafdruk** tekstvak.  
 
    >[!TIP]
    >Zie voor meer informatie, [hoe u voor het ophalen van een certificaat vingerafdruk-waarde](http://youtu.be/YKQF266SAxI). 
    
    f. Klik op **Opslaan**.


### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in de Azure Management portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/freshdesk-tutorial/create_aaduser_01.png) 

1. Ga naar **gebruikers en groepen** en klikt u op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Het maken van een Azure AD-testgebruiker](./media/freshdesk-tutorial/create_aaduser_02.png) 

1. Aan de bovenkant van het dialoogvenster klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/freshdesk-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/freshdesk-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-freshdesk-test-user"></a>Het maken van een testgebruiker FreshDesk

Om in te schakelen in Azure AD-gebruikers zich aanmelden bij FreshDesk, moeten ze worden ingericht in FreshDesk.  
In het geval van FreshDesk is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Freshdesk** tenant.
1. Klik in het menu aan de bovenkant op **Admin**.
   
   ![Beheerder](./media/freshdesk-tutorial/IC776772.png "Admin")

1. In de **algemene instellingen** tabblad **Agents**.
   
   ![Agents](./media/freshdesk-tutorial/IC776773.png "Agents")

1. Klik op **nieuwe Agent**.
   
    ![Nieuwe Agent](./media/freshdesk-tutorial/IC776774.png "nieuwe-Agent")

1. In het dialoogvenster voor informatie over agents, moet u de volgende stappen uitvoeren:
   
   ![Agentgegevens](./media/freshdesk-tutorial/IC776775.png "agentgegevens")
   
   a. In de **volledige naam** tekstvak, typ de naam van de Azure AD-account dat u inrichten wilt.

   b. In de **e** tekstvak, typ de Azure AD e-mailadres van de Azure AD-account dat u inrichten wilt.

   c. In de **titel** tekstvak typt u de titel van de Azure AD-account dat u inrichten wilt.

   d. Selecteer **Agents rol**, en klik vervolgens op **toewijzen**.
       
   e. Klik op **Opslaan**.     
   
    >[!NOTE]
    >De houder van Azure AD-account ontvangt een e-mailbericht een koppeling om te bevestigen dat het account bevat voordat deze wordt geactiveerd. 
    > 
    
    >[!NOTE]
    >U kunt alle andere Freshdesk gebruiker-account maken van hulpprogramma's of API's geleverd door Freshdesk aan inrichten AAD-gebruikersaccounts. naar freshdesk kopiëren.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding in haar door toegang te verlenen aan vak.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar freshdesk kopiëren, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-beheerportal, en vervolgens gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **FreshDesk**.

    ![Eenmalige aanmelding configureren](./media/freshdesk-tutorial/tutorial_freshdesk_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u de FreshDesk-tegel in het toegangsvenster klikt, krijgt u rechtstreeks aanmeldingspagina aan u aangemeld bij uw FreshDesk-toepassing.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/freshdesk-tutorial/tutorial_general_203.png

