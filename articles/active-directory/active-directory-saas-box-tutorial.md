---
title: 'Zelfstudie: Azure Active Directory integreren met vak | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en vak.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3b565c8d-35e2-482a-b2f4-bf8fd7d8731f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/8/2017
ms.author: jeedes
ms.openlocfilehash: 638ae63057df00375b05a58e3ceab510e2a608de
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="integrate-azure-active-directory-with-box"></a>Azure Active Directory integreren met het selectievakje

In deze zelfstudie leert u het integreren van Azure Active Directory (Azure AD) met Box.

Door Azure AD integreren met Box, profiteren van de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot vak heeft.
- U kunt uw gebruikers krijgen automatisch aangemeld aan vak (eenmalige aanmelding of SSO) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over de integratie van SaaS-app met Azure AD [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Box, moet u de volgende items:

- Een Azure AD-abonnement
- Een abonnement vak SSO-ingeschakeld

> [!NOTE]
> Wanneer u de stappen in deze zelfstudie hebt getest, raden we aan dat u doen *niet* gebruik van een productieomgeving.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. 

Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Vak uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-box-from-the-gallery"></a>Het selectievakje uit de galerie toevoegen
Voor het configureren van de integratie van Azure AD met Box, het selectievakje uit de galerie toevoegt aan de lijst met beheerde SaaS-apps als volgt:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**. 

    ![De Azure Active Directory-knop][1]

2. Selecteer **bedrijfstoepassingen** > **alle toepassingen**.

    ![Het venster 'Bedrijfstoepassingen'][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop bovenaan in het venster.

    !['Nieuwe application' knop][3]

4. Typ in het zoekvak **vak**, selecteer **vak** in de lijst met resultaten en selecteer vervolgens **toevoegen**.

    ![Vak in de lijst met resultaten](./media/active-directory-saas-box-tutorial/tutorial_box_search.png)
### <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met vak op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD identificeren van de gebruiker het selectievakje en het bijbehorende equivalent in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en dezelfde gebruiker in het vak worden ingesteld.

Voor het opzetten van de relatie koppeling toewijzen als het selectievakje *gebruikersnaam* de waarde van de *gebruikersnaam* in Azure AD.

Als u wilt configureren en Azure AD eenmalige aanmelding met vak testen, voltooi de bouwstenen in de volgende vijf secties.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing vak als volgt:

1. In de Azure-portal in de **vak** toepassing integratie venster Selecteer **eenmalige aanmelding**.

    ![De koppeling "Single sign-on"][4]

2. In de **eenmalige aanmelding** venster in de **modus voor één aanmelding** de optie **op basis van SAML aanmelding**.
 
    ![Het venster "Single sign-on"](./media/active-directory-saas-box-tutorial/tutorial_box_samlbase.png)

3. Onder **in het domein en de URL's**, het volgende doen:

    !['Vak domein en de URL's ' eenmalige aanmelding informatie](./media/active-directory-saas-box-tutorial/url3.png)

    a. In de **aanmeldings-URL** vak, typt u een URL in de volgende indeling: *https://\<subdomein >. box.com*.

    b. In de **id** textbox type **box.net**.
     
    > [!NOTE] 
    > De voorgaande waarden zijn niet echt. Deze bijwerken met de werkelijke aanmeldings-URL en de id. De waarden, neem contact op met de [vak client ondersteuningsteam](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire). 

4. Onder **SAML-certificaat voor ondertekening van**, selecteer **Metadata XML**, en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-box-tutorial/tutorial_box_certificate.png) 

5. Selecteer **Opslaan**.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-box-tutorial/tutorial_general_400.png)
    
6. Volg de procedure in eenmalige aanmelding configureren voor uw toepassing [instellen van eenmalige aanmelding op uw eigen](https://community.box.com/t5/How-to-Guides-for-Admins/Setting-Up-Single-Sign-On-SSO-for-your-Enterprise/ta-p/1263#ssoonyourown).

> [!NOTE] 
> Als u niet de instellingen voor eenmalige aanmelding voor uw account vak inschakelen, moet u mogelijk contact opnemen met de [vak client ondersteuningsteam](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) en geef het gedownloade XML-bestand.

> [!TIP]
> Als u de app instelt, vindt u een beknopte versie van de voorgaande instructies in de [Azure-portal](https://portal.azure.com). Nadat u hebt toegevoegd in de app de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en vervolgens naar de Embedded-documentatie in de **configuratie** sectie onderaan. Zie voor meer informatie over de functie ingesloten documentatie [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).
>

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie kunt u testgebruiker Britta Simon in de Azure portal maken door het volgende te doen:

![Een Azure AD-testgebruiker maken][100]

1. Selecteer in de Azure-portal in het linkerdeelvenster **Azure Active Directory**.

    ![De Azure Active Directory-koppeling](./media/active-directory-saas-box-tutorial/create_aaduser_01.png)

2. Selecteer voor een lijst met actieve gebruikers, **gebruikers en groepen** > **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-box-tutorial/create_aaduser_02.png)

3. Aan de bovenkant van de **alle gebruikers** Selecteer **toevoegen**.

    ![De knop toevoegen](./media/active-directory-saas-box-tutorial/create_aaduser_03.png)

    De **gebruiker** venster wordt geopend.

4. In de **gebruiker** venster de volgende handelingen uit:

    ![Het venster gebruiker](./media/active-directory-saas-box-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-box-test-user"></a>Maken van een gebruiker vak testen

In deze sectie maakt u testgebruiker Britta Simon in vak. Vak ondersteunt just-in-time-inrichting, die standaard is ingeschakeld. Als een gebruiker niet al bestaat, wordt een nieuw wordt gemaakt wanneer u probeert te openen vak. Er is geen actie vereist van kunt maken van de gebruiker.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u gebruiker Britta Simon gebruikt Azure eenmalige aanmelding toegang te verlenen aan vak. Ga als volgt te werk om dit te doen:

![Toewijzen van de gebruikersrol][200]

1. Open in de Azure-portal, de **toepassingen** weergeven, gaat u naar de **Directory** weergeven en selecteer vervolgens **bedrijfstoepassingen** > **alle toepassingen**.

    ![De 'Bedrijfstoepassingen' en 'Alle toepassingen' koppelingen][201] 

2. In de **toepassingen** selecteert **vak**.

    ![De koppeling](./media/active-directory-saas-box-tutorial/tutorial_box_app.png)  

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer **toevoegen** en klikt u op de **toevoegen toewijzing** deelvenster **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** venster in de **gebruikers** selecteert **Britta Simon**.

6. Selecteer de **Selecteer** knop.

7. In de **toevoegen toewijzing** Selecteer **toewijzen**.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

Wanneer u selecteert de **vak** tegel in het deelvenster toegang u opent de aanmeldingspagina voor het aanmelden bij uw toepassing vak.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Gebruikers inrichten configureren](active-directory-saas-box-userprovisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-box-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-box-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-box-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-box-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-box-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-box-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-box-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-box-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-box-tutorial/tutorial_general_203.png

