---
title: 'Zelfstudie: Azure Active Directory-integratie met FreshDesk | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en FreshDesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 232058fc35c8206c4d8f4e990d1aea5d3d9a5a69
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Zelfstudie: Azure Active Directory-integratie met FreshDesk

In deze zelfstudie leert u hoe FreshDesk integreren met Azure Active Directory (Azure AD).

FreshDesk integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot FreshDesk heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij FreshDesk (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure Management portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met FreshDesk, moet u de volgende items:

- Een Azure AD-abonnement
- Een FreshDesk eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- U moet uw productieomgeving niet gebruiken tenzij dit noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. FreshDesk uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-freshdesk-from-the-gallery"></a>FreshDesk uit de galerie toevoegen
Voor het configureren van de integratie van FreshDesk in Azure AD, moet u FreshDesk uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen FreshDesk uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Management Portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **FreshDesk**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_search.png)

5. Selecteer in het deelvenster resultaten **FreshDesk**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie configureert en test eenmalige aanmelding Azure AD met FreshDesk op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in FreshDesk is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in FreshDesk tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in FreshDesk.

Om te configureren en testen van Azure AD eenmalige aanmelding met FreshDesk, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker FreshDesk](#creating-a-freshdesk-test-user)**  - FreshDesk die is gekoppeld aan de Azure AD-representatie van haar van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure-beheerportal en eenmalige aanmelding in uw toepassing FreshDesk configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met FreshDesk, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal op de **FreshDesk** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster als **modus** Selecteer **op basis van SAML aanmelding** eenmalige aanmelding inschakelen op.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

3. Op de **FreshDesk domein en de URL's** sectie, voer de **aanmeldings-URL** als: `https://<tenant-name>.freshdesk.com` of een andere waarde Freshdesk is voorgesteld.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_url.png)

    > [!NOTE] 
    > Houd er rekening mee dat dit geen de feitelijke waarde is. U moet de waarde wordt bijgewerkt met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [FreshDesk Client ondersteuningsteam](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) deze waarde op te halen.  

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat** en sla het certificaat op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-freshdesk-tutorial/tutorial_general_400.png)

6. Op de **FreshDesk configuratie** sectie, klikt u op **FreshDesk configureren** om configureren aanmelding venster te openen. Kopieer de SAML Single Sign-On Service-URL en Sign-Out URL uit de **Naslaggids** sectie.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_configure.png)

7. In een ander browservenster, meld u bij uw bedrijf Freshdesk site als beheerder.

8. Klik in het menu bovenaan op **Admin**.
   
   ![Beheerder](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Admin")

9. In de **algemene instellingen** tabblad **beveiliging**.
   
   ![Beveiliging](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "beveiliging")

10. In de **beveiliging** sectie, voert u de volgende stappen uit:
   
    ![Eenmalige aanmelding](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "eenmalige aanmelding")
   
    a. Voor **eenmalige aanmelding op (SSO)**, selecteer **op**.

    b. Selecteer **SAML SSO**.

    c. Type de **SAML Single Sign-On Service-URL** u hebt gekopieerd uit de Azure-portal in de **aanmeldings-URL van SAML** textbox.

    d. Type de **Sign-Out URL** u hebt gekopieerd uit de Azure-portal in de **afmelding URL** textbox.

    e. Kopieer de **vingerafdruk** waarde uit het gedownloade certificaat vanuit Azure-portal en plak deze in de **beveiliging certificaat vingerafdruk** textbox.  
 
    >[!TIP]
    >Zie voor meer informatie [hoe op te halen van een certificaat vingerafdrukwaarde](http://youtu.be/YKQF266SAxI). 
    
    f. Klik op **Opslaan**.


### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in Azure Management portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure Management portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_01.png) 

2. Ga naar **gebruikers en groepen** en klik op **alle gebruikers** om de lijst met gebruikers weer te geven.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_02.png) 

3. Klik aan de bovenkant van het dialoogvenster **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-freshdesk-test-user"></a>Een testgebruiker FreshDesk maken

Om in te schakelen gebruikers van Azure AD aan te melden bij FreshDesk, moeten ze worden ingericht in FreshDesk.  
In het geval van FreshDesk is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Freshdesk** tenant.
2. Klik in het menu bovenaan op **Admin**.
   
   ![Beheerder](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Admin")

3. In de **algemene instellingen** tabblad **Agents**.
   
   ![Agents](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Agents")

4. Klik op **nieuwe Agent**.
   
    ![De nieuwe Agent](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "nieuwe-Agent")

5. Voer de volgende stappen uit in het dialoogvenster gegevens van agents:
   
   ![Agentgegevens](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "agentgegevens")
   
   a. In de **volledige naam** textbox, typ de naam van de Azure AD-account dat u inrichten wilt.

   b. In de **e** textbox type de Azure AD-e-mailadres van de Azure AD-account dat u inrichten wilt.

   c. In de **titel** textbox, typ de titel van de Azure AD-account dat u inrichten wilt.

   d. Selecteer **Agents rol**, en klik vervolgens op **toewijzen**.
       
   e. Klik op **Opslaan**.     
   
    >[!NOTE]
    >De accounthouder Azure AD ontvangt een e-mailbericht een koppeling om te bevestigen van het account bevat voordat deze wordt geactiveerd. 
    > 
    
    >[!NOTE]
    >U kunt andere Freshdesk gebruiker account hulpmiddelen voor het maken of API's die is geleverd door Freshdesk aan inrichten AAD-gebruikersaccounts. naar FreshDesk.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang te verlenen aan vak.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen FreshDesk, moet u de volgende stappen uitvoeren:**

1. In de Azure-beheerportal, opent u de weergave toepassingen en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **FreshDesk**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel FreshDesk in het deelvenster toegang, krijgt u de aanmeldingspagina te verkrijgen aangemeld bij uw toepassing FreshDesk.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_203.png

