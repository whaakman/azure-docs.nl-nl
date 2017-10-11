---
title: 'Zelfstudie: Azure Active Directory-integratie met werkplek door Facebook | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en werkplek met Facebook.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 1590a66f215f0c093d24ff602c0ad951ba1e1eea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Zelfstudie: Azure Active Directory-integratie met werkplek door Facebook

In deze zelfstudie leert u hoe werkplek door Facebook integreren met Azure Active Directory (Azure AD).

Werkplek door Facebook integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de werkplek door Facebook heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij werkplek door Facebook (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met werkplek door Facebook, moet u de volgende items:

- Een Azure AD-abonnement
- Een werkplek met eenmalige aanmelding Facebook abonnement ingeschakeld

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Werkplek door Facebook uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-workplace-by-facebook-from-the-gallery"></a>Werkplek door Facebook uit de galerie toevoegen
Voor het configureren van de integratie van werkplek door Facebook in Azure AD, moet u werkplek door Facebook uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen werkplek door Facebook uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **werkplek door Facebook**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

5. Selecteer in het deelvenster resultaten **werkplek door Facebook**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met werkplek door Facebook op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in werkplek door Facebook is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gebruiker op de werkplek door Facebook tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in werkplek met Facebook.

Om te configureren en testen van Azure AD eenmalige aanmelding met werkplek door Facebook, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren van Herauthenticatie frequentie](#configuring-reauthentication-frequency)**  - werkplek vraagt om een SAML-controle configureren.
3. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
4. **[Maken van een werkplek door Facebook testgebruiker](#creating-a-workplace-by-facebook-test-user)**  - werkplek door Facebook die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
5. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
6. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren op uw werkplek met Facebook-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met werkplek door Facebook, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **werkplek door Facebook** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. Op de **werkplek Facebook-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://<instancename>.facebook.com`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://www.facebook.com/company/<instancename>`

    > [!NOTE] 
    > Deze waarden zijn niet de werkelijke. Deze waarden bijwerken met het werkelijke aanmeldings-URL en de id. Neem contact op met [werkplek door Facebook Client-ondersteuningsteam](https://workplace.fb.com/faq/) ophalen van deze waarden. 

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_400.png)

6. Op de **werkplek door Facebook configuratie** sectie, klikt u op **werkplek configureren door Facebook** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-workplacebyfacebook-tutorial/config.png) 

7. In een ander browservenster, meld u aan bij uw werkplek door Facebook bedrijf site als beheerder.
  
   > [!NOTE] 
   > Als onderdeel van het SAML-verificatieproces mag werkplek queryreeksen van de grootte van maximaal 2,5 kB gebruiken om de parameters doorgeven aan Azure AD.

8. In de **bedrijf Dashboard**, gaat u naar de **verificatie** tabblad.

9. Onder **SAML-verificatie**, selecteer **eenmalige aanmelding alleen** uit de vervolgkeuzelijst.

10. Voer de waarden die zijn gekopieerd uit **werkplek door Facebook-configuratie** sectie van de Azure portal naar de bijbehorende velden:

    *   In **SAML URL** textbox, plak de waarde van **Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.
    *   In **URL-verlener SAML textbox**, plak de waarde van **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal.
    *   In **SAML afmelding omleiden** (optioneel), plak de waarde van **Sign-Out URL**, die u hebt gekopieerd vanuit Azure-portal.
    *   Open uw **base-64 gecodeerde certificaat** in Kladblok van Azure portal hebt gedownload, kopieert u de inhoud ervan naar het Klembord en plakt u deze naar de **SAML certificaat** textbox.

11. Mogelijk moet u de doelgroep URL invoeren, de URL van de geadresseerde, en de URL van de ACS (Assertion Consumer-Service) die worden vermeld onder de **SAML-configuratie** sectie.

12. Ga naar de onderkant van de sectie en klik op de **Test SSO** knop. Dit resulteert in een pop-upvenster wordt weergegeven met Azure AD-aanmeldingspagina weergegeven. Geef uw referenties in als normaal om te verifiëren. 

    **Voor probleemoplossing:** Zorg ervoor dat het e-mailadres dat wordt geretourneerd back vanuit Azure AD is hetzelfde als het werkplekaccount dat u bent aangemeld.

13. Zodra de test is voltooid, Ga naar de onderkant van de pagina en klik op de **opslaan** knop.

14. Alle gebruikers met behulp van werkplek wordt nu weergegeven met Azure AD-aanmeldingspagina voor verificatie.

15. **SAML afmelding omleiden (optioneel)** - 

    U kunt desgewenst een SAML afmelding-Url, die kan worden gebruikt om te verwijzen op de pagina voor Azure AD-Meld u af te configureren. Als deze instelling is ingeschakeld en geconfigureerd, wordt de gebruiker niet langer worden omgeleid naar de pagina Werkplek afmelden. In plaats daarvan wordt de gebruiker wordt omgeleid naar de url die is toegevoegd in de instelling voor het omleiden van SAML-afmelden.


> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="configuring-reauthentication-frequency"></a>Herauthenticatie frequentie configureren

U kunt configureren werkplek om aan te vragen om een SAML-controle elke dag, drie dagen, week, twee weken, maanden of nooit.

> [!NOTE] 
>De minimumwaarde voor de SAML-controle voor mobiele toepassingen is ingesteld op één week.

U kunt ook een SAML opnieuw instellen voor alle gebruikers met behulp van de knop afdwingen: vereisen SAML-verificatie voor alle gebruikers nu.


### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-workplace-by-facebook-test-user"></a>Maken van een werkplek door Facebook testgebruiker

In deze sectie wordt een gebruiker met de naam Britta Simon in werkplek gemaakt door Facebook. Werkplek door Facebook ondersteunt just-in-time-inrichting, die standaard is ingeschakeld.

Er is geen actie voor u in deze sectie. Als een gebruiker niet op de werkplek door Facebook bestaat, wordt een nieuw gemaakt wanneer u probeert te openen, werkplek met Facebook.

>[!Note]
>Als u wilt een gebruiker handmatig maken, neem contact op met [werkplek door ondersteuningsteam Facebook-Client](https://workplace.fb.com/faq/)

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang te verlenen aan een werkplek met Facebook.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon werkplek door Facebook, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **werkplek door Facebook**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Gebruikers inrichten configureren](active-directory-saas-workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_203.png

