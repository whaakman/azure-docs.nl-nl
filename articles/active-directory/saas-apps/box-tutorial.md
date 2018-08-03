---
title: 'Zelfstudie: Azure Active Directory-integratie met Box | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Box.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3b565c8d-35e2-482a-b2f4-bf8fd7d8731f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: f5aa724e9848c9794eef093aef15b0aaed9cae97
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435757"
---
# <a name="integrate-azure-active-directory-with-box"></a>Azure Active Directory integreren met Box

In deze zelfstudie leert u hoe u Azure Active Directory (Azure AD) integreren met Box.

Azure AD integreren met Box, krijgt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Box heeft.
- U kunt uw gebruikers krijgen automatisch aangemeld met Box (eenmalige aanmelding of SSO) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over SaaS-app-integratie met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Box, moet u de volgende items:

- Een Azure AD-abonnement
- Een abonnement in het SSO-ingeschakeld

> [!NOTE]
> Wanneer u de stappen in deze zelfstudie hebt getest, raden we aan dat u doen *niet* een productie-omgeving gebruiken.

Als u wilt testen van de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. 

Het scenario dat wordt beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Vak uit de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-box-from-the-gallery"></a>Vak uit de galerie toevoegen
Voor het configureren van de integratie van Azure AD met Box, vak uit de galerie toevoegt aan uw lijst met beheerde SaaS-apps door het volgende te doen:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster **Azure Active Directory**. 

    ![De Azure Active Directory-knop][1]

1. Selecteer **bedrijfstoepassingen** > **alle toepassingen**.

    ![Het venster 'Enterprise Application'][2]
    
1. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop aan de bovenkant van het venster.

    ![De 'nieuwe toepassing"knop][3]

1. Typ in het zoekvak **vak**, selecteer **vak** in de lijst met resultaten en selecteer vervolgens **toevoegen**.

    ![Het vak in de lijst met resultaten](./media/box-tutorial/tutorial_box_search.png)
### <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Box, op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD de Box-gebruiker en het bijbehorende equivalent identificatie in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en dezelfde gebruiker in Box worden ingesteld.

Toewijzen als het selectievakje in voor het maken van de relatie koppeling *gebruikersnaam* de waarde van de *gebruikersnaam* in Azure AD.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Box, voltooi de bouwstenen die u in de volgende vijf secties.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw Box-toepassing door de volgende te doen:

1. In de Azure-portal in de **vak** venster van de integratie van toepassing, selecteer **eenmalige aanmelding**.

    ![De koppeling "Single sign-on"][4]

1. In de **eenmalige aanmelding** venster in de **modus voor één aanmelding** Schakel **SAML gebaseerde aanmelding**.
 
    ![Het venster "Single sign-on"](./media/box-tutorial/tutorial_box_samlbase.png)

1. Onder **in het domein en URL's**, doet u het volgende:

    !['Vak domein en URL's ' eenmalige aanmelding informatie](./media/box-tutorial/url3.png)

    a. In de **aanmeldings-URL** vak, typ een URL in de volgende indeling: *https://\<subdomein >. box.com*.

    b. In de **id** tekstvak, type **box.net**.
     
    > [!NOTE] 
    > De bovenstaande waarden zijn niet echt. Deze bijwerken met de werkelijke aanmeldings-URL en -id. De waarden, neem contact op met de [vak client ondersteuningsteam](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire). 

1. Onder **SAML-handtekeningcertificaat**, selecteer **Metadata XML**, en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/box-tutorial/tutorial_box_certificate.png) 

1. Selecteer **Opslaan**.

    ![Configureren van eenmalige aanmelding opslaan](./media/box-tutorial/tutorial_general_400.png)
    
1. Volg de procedure in voor meer informatie over het configureren van eenmalige aanmelding voor uw toepassing [een SSO instellen op uw eigen](https://community.box.com/t5/How-to-Guides-for-Admins/Setting-Up-Single-Sign-On-SSO-for-your-Enterprise/ta-p/1263#ssoonyourown).

> [!NOTE] 
> Als u niet de instellingen voor eenmalige aanmelding voor uw Box-account inschakelen, moet u mogelijk contact op met de [vak client ondersteuningsteam](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) en geeft u het gedownloade XML-bestand.

> [!TIP]
> Als u de app instelt, vindt u een beknopte versie van de voorgaande instructies in de [Azure-portal](https://portal.azure.com). Nadat u hebt toegevoegd in de app de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad, en vervolgens toegang tot de ingesloten in de documentatie bij de **configuratie** sectie aan de onderkant. Zie voor meer informatie over de functie ingesloten documentatie [documentatie over Azure AD embedded]( https://go.microsoft.com/fwlink/?linkid=845985).
>

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

In deze sectie maakt maken u testgebruiker Britta Simon in Azure portal door het volgende te doen:

![Maak een testgebruiker Azure AD][100]

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**.

    ![De Azure Active Directory-koppeling](./media/box-tutorial/create_aaduser_01.png)

1. Selecteer voor een lijst van de huidige gebruikers, **gebruikers en groepen** > **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/box-tutorial/create_aaduser_02.png)

1. Aan de bovenkant van de **alle gebruikers** venster **toevoegen**.

    ![De knop toevoegen](./media/box-tutorial/create_aaduser_03.png)

    De **gebruiker** venster wordt geopend.

1. In de **gebruiker** venster de volgende handelingen uit:

    ![Het venster van de gebruiker](./media/box-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-box-test-user"></a>Maak een testgebruiker vak

In deze sectie maakt u testgebruiker Britta Simon in Box. Vak biedt ondersteuning voor just-in-time inrichting, dat standaard is ingeschakeld. Als een gebruiker nog niet bestaat, wordt een nieuw bestand wordt gemaakt wanneer u probeert te krijgen tot de Box. Er is geen actie vereist van u te maken van de gebruiker.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u gebruiker Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan vak. Om dit te doen, het volgende doen:

![De de gebruikersrol toewijzen][200]

1. Open in de Azure-portal, de **toepassingen** weergeven, gaat u naar de **Directory** weergeven en selecteer vervolgens **bedrijfstoepassingen** > **alle toepassingen**.

    ![De 'Zakelijke toepassingen' en 'Alle Application' koppelingen][201] 

1. In de **toepassingen** in de lijst met **vak**.

    ![De koppeling](./media/box-tutorial/tutorial_box_app.png)  

1. Selecteer in het linkerdeelvenster **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Selecteer **toevoegen** en klikt u op de **toevoegen toewijzing** venster **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

1. In de **gebruikers en groepen** venster in de **gebruikers** in de lijst met **Britta Simon**.

1. Selecteer de **Selecteer** knop.

1. In de **toevoegen toewijzing** venster **toewijzen**.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u selecteert de **vak** tegel in het toegangsvenster, u opent de aanmeldingspagina voor aanmelden bij uw Box-toepassing.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Inrichten van gebruikers configureren](box-userprovisioning-tutorial.md)



<!--Image references-->

[1]: ./media/box-tutorial/tutorial_general_01.png
[2]: ./media/box-tutorial/tutorial_general_02.png
[3]: ./media/box-tutorial/tutorial_general_03.png
[4]: ./media/box-tutorial/tutorial_general_04.png

[100]: ./media/box-tutorial/tutorial_general_100.png

[200]: ./media/box-tutorial/tutorial_general_200.png
[201]: ./media/box-tutorial/tutorial_general_201.png
[202]: ./media/box-tutorial/tutorial_general_202.png
[203]: ./media/box-tutorial/tutorial_general_203.png

