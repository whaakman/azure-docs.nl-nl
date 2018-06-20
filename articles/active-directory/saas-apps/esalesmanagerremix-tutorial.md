---
title: 'Zelfstudie: Azure Active Directory-integratie met E Sales Manager Remix | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en E Sales Manager Remix.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 89b5022c-0d5b-4103-9877-ddd32b6e1c02
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: 07cbc047179c3de19d6f648d0342d47970c81dbc
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231369"
---
# <a name="integrate-azure-active-directory-with-e-sales-manager-remix"></a>Azure Active Directory integreren met E Verkoopmanager Remix

In deze zelfstudie leert u het integreren van Azure Active Directory (Azure AD) met E Sales Manager Remix.

Door Azure AD integreren met het E-Sales Manager Remix, profiteren van de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot E Sales Manager Remix heeft.
- U kunt uw gebruikers te verkrijgen ondertekend automatisch E Sales Manager Remix (eenmalige aanmelding of SSO) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Azure AD om integratie te configureren met E Sales Manager Remix, moet u de volgende items:

- Een Azure AD-abonnement
- Een abonnement E Sales Manager Remix SSO-ingeschakeld

> [!NOTE]
> Wanneer u de stappen in deze zelfstudie hebt getest, raden we aan dat u doen *niet* gebruik van een productieomgeving.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. 

Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

* Toe te voegen E Sales Manager Remix uit de galerie
* Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-e-sales-manager-remix-from-the-gallery"></a>E Sales Manager Remix uit de galerie toevoegen
Als u wilt de integratie van Azure AD configureren met E Sales Manager Remix, E Sales Manager Remix uit de galerie toevoegt aan de lijst met beheerde SaaS-apps als volgt:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**. 

    ![De Azure Active Directory-knop][1]

2. Selecteer **bedrijfstoepassingen** > **alle toepassingen**.

    ![Het venster 'Bedrijfstoepassingen'][2]
    
3. Selecteer om een nieuwe toepassing toe **nieuwe toepassing** aan de bovenkant van het venster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **E Sales Manager Remix**, selecteer **E Sales Manager Remix** in de lijst met resultaten en selecteer vervolgens **toevoegen**.

    ![E Sales Manager Remix in de lijst met resultaten](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met E Sales Manager Remix, op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD identificeren van de gebruiker E Sales Manager Remix en het bijbehorende equivalent in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en dezelfde gebruiker in IE Sales Manager Remix worden ingesteld.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met E Sales Manager Remix, voert u de bouwstenen in de volgende vijf secties:

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing E Sales Manager Remix als volgt:

1. In de Azure-portal op de **E Sales Manager Remix** toepassing Integratiepagina **eenmalige aanmelding**.

    ![De koppeling "Single sign-on"][4]

2. In de **eenmalige aanmelding** venster in de **modus voor één aanmelding** de optie **op basis van SAML aanmelding**.
 
    ![Het venster "Single sign-on"](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_samlbase.png)

3. Onder **E Sales Manager Remix domein en de URL's**, het volgende doen:

    ![E Sales Manager Remix domein en de URL's van eenmalige aanmelding informatie](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_url.png)

    a. In de **aanmeldings-URL** vak, typt u een URL in de volgende indeling: *https://\<Server-basis-URL > /\<subdomein > / esales pc*.

    b. In de **id** vak, typt u een URL in de volgende indeling: *https://\<Server-basis-URL > /\<subdomein > /*.

    c. Opmerking de **id** voor later gebruik in deze zelfstudie.
    
    > [!NOTE] 
    > De voorgaande waarden zijn niet echt. Deze bijwerken met de werkelijke aanmelden URL en de id. De waarden, neem contact op met [E Sales Manager Remix Client ondersteuningsteam](mailto:esupport@softbrain.co.jp).

4. Onder **SAML-certificaat voor ondertekening van**, selecteer **certificaat (Base64)**, en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat (Base64)](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_certificate.png) 

5. Selecteer de **weergeven en bewerken van alle andere gebruikerskenmerken** selectievakje en selecteer vervolgens de **emailaddress** kenmerk.
    
    ![Het venster gebruikerskenmerken](./media/esalesmanagerremix-tutorial/configure1.png)

    De **kenmerk bewerken** venster wordt geopend.

6. Kopieer de **Namespace** en **naam** waarden. De waarde in het patroon genereren  *\<Namespace > /\<Name >*, en op te slaan voor later gebruik in deze zelfstudie.

    ![Het venster kenmerk bewerken](./media/esalesmanagerremix-tutorial/configure2.png)

7. Onder **E Sales Manager Remix configuratie**, selecteer **configureren IE Sales Manager Remix**.

    ![E Verkoopmanager Remix configuratie](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_configure.png) 

    De **eenmalige aanmelding configureren** venster wordt geopend.

8. In de **Naslaggids** sectie, Kopieer de URL voor afmelden en de SAML service voor eenmalige aanmelding.

9. Selecteer **Opslaan**.

    ![De knop Opslaan](./media/esalesmanagerremix-tutorial/tutorial_general_400.png)

10. Aanmelden bij uw toepassing E Sales Manager Remix als beheerder.

11. Selecteer in de rechterbovenhoek, **Menu naar beheerder**.

    ![De opdracht 'Beheerder Menu naar'](./media/esalesmanagerremix-tutorial/configure4.png)

12. Selecteer in het linkerdeelvenster **systeeminstellingen** > **samenwerking met extern systeem**.

    ![Instellingen voor de ' systeem ' en 'Samenwerking met extern systeem' koppelingen](./media/esalesmanagerremix-tutorial/configure5.png)
    
13. In de **samenwerking met extern systeem** Selecteer **SAML**.

    ![Het venster 'Samenwerking met extern systeem'](./media/esalesmanagerremix-tutorial/configure6.png)

14. Onder **SAML-verificatie-instelling**, het volgende doen:

    ![De sectie 'SAML-verificatie-instelling'](./media/esalesmanagerremix-tutorial/configure3.png)
    
    a. Selecteer de **PC-versie** selectievakje.
    
    b. In de **samenwerking item** sectie in de vervolgkeuzelijst selecteren **e**.

    c. In de **samenwerking item** vak, plak de claimwaarde die u eerder hebt gekopieerd vanuit de Azure-portal (dat wil zeggen, **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**).

    d. In de **verlener (entiteit-ID)** vak, plak de id-waarde die u eerder hebt gekopieerd uit de **E Sales Manager Remix domein en de URL's** sectie van de Azure-portal.

    e. Als u wilt uploaden uw gedownloade certificaat van de Azure-portal, selecteer **selectie bestand**.

    f. In de **ID-provider aanmeldings-URL** vak, plak de URL van het SAML-service voor eenmalige aanmelding die u eerder in de Azure-portal hebt gekopieerd.

    g. In **identiteit Provider afmelding URL** vak, plak de afmelden URL-waarde die u eerder in de Azure-portal hebt gekopieerd.

    h. Selecteer **instelling voltooid**.

> [!TIP]
> Als u de app instelt, vindt u een beknopte versie van de voorgaande instructies in de [Azure-portal](https://portal.azure.com). Nadat u hebt toegevoegd in de app de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en vervolgens naar de Embedded-documentatie in de **configuratie** sectie onderaan. Zie voor meer informatie over de functie ingesloten documentatie [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie kunt u testgebruiker Britta Simon in de Azure portal maken door het volgende te doen:

![Een Azure AD-testgebruiker maken][100]

1. Selecteer in de Azure-portal in het linkerdeelvenster **Azure Active Directory**.

    ![De Azure Active Directory-koppeling](./media/paloaltoadmin-tutorial/create_aaduser_01.png)

2. Selecteer voor een lijst met actieve gebruikers, **gebruikers en groepen** > **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/paloaltoadmin-tutorial/create_aaduser_02.png)

3. Aan de bovenkant van de **alle gebruikers** Selecteer **toevoegen**.

    ![De knop toevoegen](./media/paloaltoadmin-tutorial/create_aaduser_03.png)
    
    De **gebruiker** venster wordt geopend.

4. In de **gebruiker** venster de volgende handelingen uit:

    ![Het venster gebruiker](./media/paloaltoadmin-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** uit en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-an-e-sales-manager-remix-test-user"></a>Een testgebruiker E Sales Manager Remix maken

1. Meld u aan op uw toepassing E Sales Manager Remix als beheerder.

2. Selecteer **Menu naar beheerder** in het menu rechtsboven.

    ![E Verkoopmanager Remix configuratie](./media/esalesmanagerremix-tutorial/configure4.png)

3. Selecteer **van uw bedrijf instellingen** > **onderhoud van de afdelingen en werknemers**, en selecteer vervolgens **werknemers geregistreerd**.

    ![Het tabblad 'Werknemers geregistreerd'](./media/esalesmanagerremix-tutorial/user1.png)

4. In de **nieuwe registratie van de werknemer** sectie, het volgende doen:
    
    ![De 'nieuwe werknemer '-inschrijving sectie](./media/esalesmanagerremix-tutorial/user2.png)

    a. In de **naam van de werknemer** typt u de naam van de gebruiker (bijvoorbeeld **Britta**).

    b. Vul de overige vereiste velden.
    
    c. Als u SAML inschakelt, kunnen de beheerder zich kan aanmelden van de aanmeldingspagina. Verleen aanmelden beheerdersrechten voor de gebruiker door te selecteren de **aanmeldgegevens van serverbeheerder** selectievakje.

    d. Selecteer **registratie**.

5. In de toekomst, als u wilt aanmelden als beheerder, meld u aan als de gebruiker die beheerdersrechten heeft en selecteer vervolgens in de rechterbovenhoek, **Menu naar beheerder**.

    ![De opdracht 'Beheerder Menu naar'](./media/esalesmanagerremix-tutorial/configure4.png)

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u gebruiker Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang aan E Sales Manager Remix. Ga als volgt te werk om dit te doen: 

![Toewijzen van de gebruikersrol][200] 

1. Open in de Azure-portal, de **toepassingen** weergeven, gaat u naar de **Directory** weergeven en selecteer vervolgens **bedrijfstoepassingen** > **alle toepassingen**.

    ![De 'Bedrijfstoepassingen' en 'Alle toepassingen' koppelingen][201] 

2. In de **toepassingen** selecteert **E Sales Manager Remix**.

    ![De koppeling E Sales Manager Remix](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_app.png)  

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer **toevoegen** en klikt u op de **toevoegen toewijzing** deelvenster **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** venster in de **gebruikers** selecteert **Britta Simon**.

6. Selecteer de **Selecteer** knop.

7. In de **toevoegen toewijzing** Selecteer **toewijzen**.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

Wanneer u de tegel E Sales Manager Remix in het deelvenster toegang selecteert, moet u zijn aangemeld automatisch aan uw toepassing E Sales Manager Remix.

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/esalesmanagerremix-tutorial/tutorial_general_01.png
[2]: ./media/esalesmanagerremix-tutorial/tutorial_general_02.png
[3]: ./media/esalesmanagerremix-tutorial/tutorial_general_03.png
[4]: ./media/esalesmanagerremix-tutorial/tutorial_general_04.png

[100]: ./media/esalesmanagerremix-tutorial/tutorial_general_100.png

[200]: ./media/esalesmanagerremix-tutorial/tutorial_general_200.png
[201]: ./media/esalesmanagerremix-tutorial/tutorial_general_201.png
[202]: ./media/esalesmanagerremix-tutorial/tutorial_general_202.png
[203]: ./media/esalesmanagerremix-tutorial/tutorial_general_203.png

