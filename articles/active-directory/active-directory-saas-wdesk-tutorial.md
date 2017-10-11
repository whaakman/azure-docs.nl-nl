---
title: 'Zelfstudie: Azure Active Directory-integratie met Wdesk | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Wdesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 37660b80cfb01d6a3105aea5ce248f1e03c46695
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wdesk"></a>Zelfstudie: Azure Active Directory-integratie met Wdesk

In deze zelfstudie leert u hoe Wdesk integreren met Azure Active Directory (Azure AD).

Wdesk integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Wdesk heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Wdesk (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, zien. [Wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Wdesk, moet u de volgende items:

- Een Azure AD-abonnement
- Een Wdesk eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Wdesk uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-wdesk-from-the-gallery"></a>Wdesk uit de galerie toevoegen
Voor het configureren van de integratie van Wdesk in Azure AD, moet u Wdesk uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Wdesk uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Wdesk**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_search.png)

5. Selecteer in het deelvenster resultaten **Wdesk**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Wdesk op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Wdesk is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Wdesk tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Wdesk.

Om te configureren en testen van Azure AD eenmalige aanmelding met Wdesk, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Wdesk](#creating-a-wdesk-test-user)**  - Wdesk die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Wdesk configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Wdesk, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Wdesk** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_samlbase.png)

3. Op de **Wdesk domein en de URL's** sectie als u wilt configureren van de toepassing in **IDP** geïnitieerd modus de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`

4. Controleer **weergeven geavanceerde instellingen voor URL**. Als u wilt configureren van de toepassing in **SP** geïnitieerd modus, voer de volgende stap:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url1.png)

    In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke id, antwoord-URL en aanmeldings-URL. U krijgt deze waarden van de portal WDesk bij het configureren van de SSO. 
  
5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-wdesk-tutorial/tutorial_general_400.png)
    
7. In een ander browservenster, meld u aan bij Wdesk als een beveiligingsbeheerder.

8. In de onderste links, klikt u op **Admin** en kies **accountbeheerder**:
 
     ![Eenmalige aanmelding configureren](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

9. Navigeer in Wdesk Admin, naar **beveiliging**, klikt u vervolgens **SAML** > **SAML instellingen**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

10. Onder **algemene instellingen**, Controleer de **SAML eenmalige aanmelding inschakelen**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

11. Onder **Details van Service Provider**, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      a. Kopieer de **aanmeldings-URL** en plak deze in **aanmeldings-Url** textbox in Azure portal.
   
      b. Kopieer de **metagegevens-Url** en plak deze in **id** textbox in Azure portal.
       
      c. Kopieer de **Consumer url** en plak deze in **antwoord-Url** textbox in Azure portal.
   
      d. Klik op **opslaan** in Azure portal de wijzigingen wilt opslaan.      

12. Klik op **IdP-instellingen configureren** openen **IdP-instellingen bewerken** dialoogvenster. Klik op **bestand kiezen** vinden de **Metadata.xml** opgeslagen vanuit Azure-portal-bestand uploaden.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
13. Klik op **wijzigingen opslaan**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wdesk-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wdesk-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wdesk-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wdesk-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-wdesk-test-user"></a>Een testgebruiker Wdesk maken

Om Azure AD-gebruikers zich aanmelden bij Wdesk, moeten ze worden ingericht in Wdesk. In Wdesk is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan als een beveiligingsbeheerder Wdesk bij.
2. Navigeer naar **Admin** > **Admin-Account**.

     ![Eenmalige aanmelding configureren](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. Klik op **leden** onder **mensen**.

4. Klik nu op **lid toevoegen** openen **lid toevoegen** in het dialoogvenster. 
   
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wdesk-tutorial/createuser1.png)  

5. In **gebruiker** tekst en voer de gebruikersnaam van de gebruiker zoals  **brittasimon@contoso.com**  en klik op **doorgaan** knop.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wdesk-tutorial/createuser3.png)

6.  Voer de gegevens, zoals hieronder wordt weergegeven:
  
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wdesk-tutorial/createuser4.png)
 
    a. In **e** tekst en voer het e-mailadres van de gebruiker zoals  **brittasimon@contoso.com** .

    b. In **voornaam** tekst en voer de voornaam van de gebruiker zoals **Britta**.

    c. In **achternaam** tekst en voer de achternaam van de gebruiker zoals **Simon**.

7. Klik op **lid opslaan** knop.  

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-wdesk-tutorial/createuser5.png)

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Wdesk.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Wdesk, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Wdesk**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Wdesk in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Wdesk.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_203.png

