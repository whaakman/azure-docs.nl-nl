---
title: 'Zelfstudie: Azure Active Directory-integratie met E Sales Manager Remix | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory- en E Sales Manager Remix.
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
ms.openlocfilehash: 235ba5fd1365ed8b400edce3db22420369540cce
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39052666"
---
# <a name="integrate-azure-active-directory-with-e-sales-manager-remix"></a>Azure Active Directory integreren met E Verkoopmanager Remix

In deze zelfstudie leert u hoe u Azure Active Directory (Azure AD) met E Sales Manager Remix integreren.

Door te integreren in Azure AD E Sales Manager Remix, krijgt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot E Sales Manager Remix heeft.
- U kunt uw gebruikers krijgen aangemeld automatisch bij E Sales Manager Remix (eenmalige aanmelding of SSO) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met E Sales Manager Remix, moet u de volgende items:

- Een Azure AD-abonnement
- Een abonnement E Sales Manager Remix SSO is ingeschakeld

> [!NOTE]
> Wanneer u de stappen in deze zelfstudie hebt getest, raden we aan dat u doen *niet* een productie-omgeving gebruiken.

Als u wilt testen van de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. 

Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

* Toe te voegen E Sales Manager Remix uit de galerie
* Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-e-sales-manager-remix-from-the-gallery"></a>E Sales Manager Remix uit de galerie toevoegen
Als u wilt de integratie van Azure AD configureren met E Sales Manager Remix, E Sales Manager Remix uit de galerie toevoegt aan uw lijst met beheerde SaaS-apps door het volgende te doen:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**. 

    ![De Azure Active Directory-knop][1]

2. Selecteer **bedrijfstoepassingen** > **alle toepassingen**.

    ![Het venster 'Enterprise Application'][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing** aan de bovenkant van het venster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **E Sales Manager Remix**, selecteer **E Sales Manager Remix** in de lijst met resultaten en selecteer vervolgens **toevoegen**.

    ![E Sales Manager Remix in de lijst met resultaten](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met E Sales Manager Remix, op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD identificatie van de gebruiker E Sales Manager Remix en het bijbehorende equivalent in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en dezelfde gebruiker in E Sales Manager Remix worden ingesteld.

Als u wilt configureren en Azure AD eenmalige aanmelding met E Sales Manager Remix testen, voert u de bouwstenen die u in de volgende vijf secties:

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing E Sales Manager Remix door het volgende te doen:

1. In de Azure-portal op de **E Sales Manager Remix** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![De koppeling "Single sign-on"][4]

2. In de **eenmalige aanmelding** venster in de **modus voor één aanmelding** Schakel **SAML gebaseerde aanmelding**.
 
    ![Het venster "Single sign-on"](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_samlbase.png)

3. Onder **E Sales Manager Remix domein en URL's**, doet u het volgende:

    ![E Sales Manager Remix domein en URL's, eenmalige aanmelding informatie](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_url.png)

    a. In de **aanmeldings-URL** vak, typ een URL in de volgende indeling: *https://\<Server-basis-URL > /\<subdomein > / esales-pc*.

    b. In de **id** vak, typ een URL in de volgende indeling: *https://\<Server-basis-URL > /\<subdomein > /*.

    c. Houd er rekening mee de **id** waarde voor later gebruik in deze zelfstudie.
    
    > [!NOTE] 
    > De bovenstaande waarden zijn niet echt. Deze bijwerken met de werkelijke URL voor aanmelden en -id. De waarden, neem contact op met [E Sales Manager Remix Client ondersteuningsteam](mailto:esupport@softbrain.co.jp).

4. Onder **SAML-handtekeningcertificaat**, selecteer **certificaat (Base64)**, en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat (Base64)](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_certificate.png) 

5. Selecteer de **weergeven en bewerken van alle andere gebruikerskenmerken** selectievakje en selecteer vervolgens de **emailaddress** kenmerk.
    
    ![Het venster gebruikerskenmerken](./media/esalesmanagerremix-tutorial/configure1.png)

    De **kenmerk bewerken** venster wordt geopend.

6. Kopieer de **Namespace** en **naam** waarden. De waarde in het patroon genereren  *\<Namespace > /\<naam >*, en bewaar het voor later gebruik in deze zelfstudie.

    ![Het venster kenmerk bewerken](./media/esalesmanagerremix-tutorial/configure2.png)

7. Onder **E Sales Manager Remix configuratie**, selecteer **configureren E Sales Manager Remix**.

    ![E Verkoopmanager Remix configuratie](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_configure.png) 

    De **aanmelding configureren** venster wordt geopend.

8. In de **Naslaggids** sectie, Kopieer de URL voor afmelden en de URL voor SAML-service voor eenmalige aanmelding.

9. Selecteer **Opslaan**.

    ![De knop Opslaan](./media/esalesmanagerremix-tutorial/tutorial_general_400.png)

10. Meld u aan uw toepassing E Sales Manager Remix als beheerder.

11. Selecteer in de rechterbovenhoek, **beheerder Menu**.

    ![De opdracht "Beheerder Menu naar"](./media/esalesmanagerremix-tutorial/configure4.png)

12. Selecteer in het linkerdeelvenster **systeeminstellingen** > **samenwerking met het externe systeem**.

    ![De 'Instellingen' en "Samenwerking met het externe systeem" koppelingen](./media/esalesmanagerremix-tutorial/configure5.png)
    
13. In de **samenwerking met het externe systeem** venster **SAML**.

    ![Het venster "Samenwerking met het externe systeem"](./media/esalesmanagerremix-tutorial/configure6.png)

14. Onder **SAML-verificatie instelling**, doet u het volgende:

    ![De sectie "SAML-verificatie instellen"](./media/esalesmanagerremix-tutorial/configure3.png)
    
    a. Selecteer de **PC-versie** selectievakje.
    
    b. In de **samenwerking item** sectie in de vervolgkeuzelijst, selecteer **e**.

    c. In de **samenwerking item** vak, plak de waarde van de claim die u eerder hebt gekopieerd vanuit Azure portal (dat wil zeggen, **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**).

    d. In de **verlener (entiteits-ID)** vak, plak de id-waarde die u eerder hebt gekopieerd uit de **E Sales Manager Remix domein en URL's** sectie van de Azure portal.

    e. Als u wilt uploaden uw gedownloade certificaat vanuit de Azure-portal, selecteert u **bestandsselectie**.

    f. In de **aanmeldings-URL van ID-provider** vak, plak de URL voor SAML-service voor eenmalige aanmelding die u eerder in de Azure-portal hebt gekopieerd.

    g. In **afmeldings-URL van id-Provider** vak, plak de afmeld-URL-waarde die u eerder in de Azure-portal hebt gekopieerd.

    h. Selecteer **instellen voltooid**.

> [!TIP]
> Als u de app instelt, vindt u een beknopte versie van de voorgaande instructies in de [Azure-portal](https://portal.azure.com). Nadat u hebt toegevoegd in de app de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad, en vervolgens toegang tot de ingesloten in de documentatie bij de **configuratie** sectie aan de onderkant. Zie voor meer informatie over de functie ingesloten documentatie [documentatie over Azure AD embedded]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

In deze sectie maakt maken u testgebruiker Britta Simon in Azure portal door het volgende te doen:

![Maak een testgebruiker Azure AD][100]

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**.

    ![De Azure Active Directory-koppeling](./media/paloaltoadmin-tutorial/create_aaduser_01.png)

2. Selecteer voor een lijst van de huidige gebruikers, **gebruikers en groepen** > **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/paloaltoadmin-tutorial/create_aaduser_02.png)

3. Aan de bovenkant van de **alle gebruikers** venster **toevoegen**.

    ![De knop toevoegen](./media/paloaltoadmin-tutorial/create_aaduser_03.png)
    
    De **gebruiker** venster wordt geopend.

4. In de **gebruiker** venster de volgende handelingen uit:

    ![Het venster van de gebruiker](./media/paloaltoadmin-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-an-e-sales-manager-remix-test-user"></a>Maak een testgebruiker E Sales Manager Remix

1. Meld u aan bij uw toepassing E Sales Manager Remix als beheerder.

2. Selecteer **beheerder Menu** in het menu rechtsboven.

    ![E Verkoopmanager Remix configuratie](./media/esalesmanagerremix-tutorial/configure4.png)

3. Selecteer **van uw bedrijf instellingen** > **onderhoud van afdelingen en werknemers**, en selecteer vervolgens **werknemers geregistreerd**.

    ![Het tabblad 'Werknemers registered'](./media/esalesmanagerremix-tutorial/user1.png)

4. In de **registratie van nieuwe werknemers** sectie, doet u het volgende:
    
    ![De 'nieuwe werknemers '-inschrijving sectie](./media/esalesmanagerremix-tutorial/user2.png)

    a. In de **naam van de werknemer** typt u de naam van de gebruiker (bijvoorbeeld **Julia**).

    b. Voltooi de resterende velden vereist.
    
    c. Als u SAML inschakelt, kan niet de beheerder zich aanmelden vanaf de aanmeldingspagina. Beheerder aanmelden bevoegdheden verlenen aan de gebruiker door te selecteren de **beheerdersaanmelding bij** selectievakje.

    d. Selecteer **registratie**.

5. In de toekomst, als u wilt aanmelden als beheerder, meld u aan als de gebruiker die beheerdersmachtigingen en selecteer vervolgens in de rechterbovenhoek, **beheerder Menu**.

    ![De opdracht "Beheerder Menu naar"](./media/esalesmanagerremix-tutorial/configure4.png)

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u gebruiker Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan E Sales Manager Remix. Om dit te doen, het volgende doen: 

![De de gebruikersrol toewijzen][200] 

1. Open in de Azure-portal, de **toepassingen** weergeven, gaat u naar de **Directory** weergeven en selecteer vervolgens **bedrijfstoepassingen** > **alle toepassingen**.

    ![De 'Zakelijke toepassingen' en 'Alle Application' koppelingen][201] 

2. In de **toepassingen** in de lijst met **E Sales Manager Remix**.

    ![De koppeling E Sales Manager Remix](./media/esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_app.png)  

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer **toevoegen** en klikt u op de **toevoegen toewijzing** venster **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** venster in de **gebruikers** in de lijst met **Britta Simon**.

6. Selecteer de **Selecteer** knop.

7. In de **toevoegen toewijzing** venster **toewijzen**.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de tegel E Sales Manager Remix in het toegangsvenster selecteert, moet u zijn aangemeld automatisch aan uw toepassing E Sales Manager Remix.

Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

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

