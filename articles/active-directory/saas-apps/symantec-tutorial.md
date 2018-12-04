---
title: 'Zelfstudie: Azure Active Directory-integratie met Symantec Web Security Service (WSS) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Symantec Web Security Service (WSS).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: b933bc5f5ecb39c3462e4e9bd300f1e07fd718c0
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52838768"
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a>Zelfstudie: Azure Active Directory-integratie met Symantec Web Security Service (WSS)

In deze zelfstudie leert u hoe u uw account Symantec Web Security Service (WSS) integreren met uw Azure Active Directory (Azure AD)-account zodat WSS kunt verifiëren van een eindgebruiker die is ingericht in de Azure AD met behulp van SAML-verificatie en afdwingen van de gebruiker of regels voor beleid op abonnementsniveau.

Symantec Web Security Service (WSS) integreren met Azure AD biedt u de volgende voordelen:

- Alle gebruikers en groepen die worden gebruikt door uw WSS-account van uw Azure AD-portal beheren. 

- De gebruikers om zich te authentiseren in WSS met behulp van hun Azure AD-referenties toestaan.

- Inschakelen van de naleving van de gebruiker en groep niveau beleidsregels die zijn gedefinieerd in uw WSS-account.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Symantec Web Security Service (WSS), moet u de volgende items:

- Een Azure AD-abonnement
- Een Symantec Web Security Service (WSS)-account

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met een WSS-account dat momenteel wordt gebruikt voor productie-doel.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet de WSS-account dat momenteel wordt gebruikt voor productie-doel voor deze test als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie configureert u uw Azure AD om in te schakelen single sign-on bij WSS met behulp van de referenties van de gebruiker gedefinieerd in uw Azure AD-account.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van de Symantec Web Security Service (WSS)-app uit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-symantec-web-security-service-wss-from-the-gallery"></a>Symantec Web Security Service (WSS) uit de galerie toe te voegen
Voor het configureren van de integratie van Symantec Web Security Service (WSS) in Azure AD, moet u Symantec Web Security Service (WSS) uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Symantec Web Security Service (WSS) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **Symantec Web Security Service (WSS)**, selecteer **Symantec Web Security Service (WSS)** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![Symantec Web Security Service (WSS) in de lijst met resultaten](./media/symantec-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Symantec Web Security Service (WSS) hebt op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Symantec Web Security Service (WSS) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Symantec Web Security Service (WSS) tot stand worden gebracht.

In Symantec Web Security Service (WSS), wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Symantec Web Security Service (WSS), moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker Symantec Web Security Service (WSS)](#create-a-symantec-web-security-service-wss-test-user)**  - hebben een equivalent van Britta Simon in Symantec Web Security Service (WSS) die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Symantec Web Security Service (WSS).

**Voor het configureren van Azure AD eenmalige aanmelding met Symantec Web Security Service (WSS), moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Symantec Web Security Service (WSS)** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/symantec-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

1. Op de **Symantec Web Security Service (WSS)-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Symantec Web Security Service (WSS)-domein en URL's, eenmalige aanmelding informatie](./media/symantec-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    a. In de **id** tekstvak typt u de URL: `https://saml.threatpulse.net:8443/saml/saml_realm`

    b. In de **antwoord-URL** tekstvak typt u de URL: `https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`

    > [!NOTE]
    > Neem contact op met de [Symantec Web Security Service (WSS) Client-ondersteuningsteam](https://www.symantec.com/contact-us) als de waarden voor de **id** en **antwoord-URL** werken niet voor een of andere reden.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/symantec-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/symantec-tutorial/tutorial_general_400.png)
    
1. Raadpleeg de online WSS-documentatie voor het configureren van eenmalige aanmelding aan van de Symantec Web Security Service (WSS). De gedownloade **Metadata XML** bestand moet worden geïmporteerd in de WSS-portal. Neem contact op met de [Symantec Web Security Service (WSS) ondersteuningsteam](https://www.symantec.com/contact-us) als u hulp nodig hebt bij de configuratie op de WSS-portal.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/symantec-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/symantec-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/symantec-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/symantec-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-symantec-web-security-service-wss-test-user"></a>Maak een testgebruiker Symantec Web Security Service (WSS)

In deze sectie maakt u een gebruiker met de naam Britta Simon in Symantec Web Security Service (WSS). De bijbehorende end-gebruikersnaam kan handmatig worden gemaakt in de WSS-portal of u kunt wachten op de gebruikers/groepen ingericht in de Azure AD worden gesynchroniseerd met de portal WSS na een paar minuten (ongeveer 15 minuten). Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. Het openbare IP-adres van de eindgebruiker-machine die wordt gebruikt om te bladeren websites moet ook worden ingericht in de portal voor Symantec Web Security Service (WSS).

> [!NOTE]
> Neem [Klik hier](https://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) om op te halen van de computer het openbare IP-adres.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Symantec Web Security Service (WSS).

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Symantec Web Security Service (WSS), moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Symantec Web Security Service (WSS)**.

    ![De koppeling Symantec Web Security Service (WSS) in de lijst met toepassingen](./media/symantec-tutorial/tutorial_symantecwebsecurityservicewss_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie test u de functionaliteit van eenmalige aanmelding nu dat u uw WSS-account voor het gebruik van uw Azure AD voor SAML-verificatie hebt geconfigureerd.

Nadat u hebt uw webbrowser naar de proxy het verkeer naar WSS, geconfigureerd wanneer u uw webbrowser Open en probeert om te bladeren naar een site, wordt u omgeleid naar de pagina voor Azure. Geef de referenties van de eindgebruiker test die is ingericht in de Azure AD (dat wil zeggen, BrittaSimon) en het wachtwoord dat is gekoppeld. Eenmaal is geverifieerd, moet u mogelijk zijn om te navigeren naar de website die u hebt gekozen. U moet een beleidsregel maken aan de WSS BrittaSimon van bladeren naar een bepaalde site en vervolgens u de pagina van WSS blokkeren ziet wanneer u probeert om te bladeren naar die site als gebruiker BrittaSimon blokkeren.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/symantec-tutorial/tutorial_general_01.png
[2]: ./media/symantec-tutorial/tutorial_general_02.png
[3]: ./media/symantec-tutorial/tutorial_general_03.png
[4]: ./media/symantec-tutorial/tutorial_general_04.png

[100]: ./media/symantec-tutorial/tutorial_general_100.png

[200]: ./media/symantec-tutorial/tutorial_general_200.png
[201]: ./media/symantec-tutorial/tutorial_general_201.png
[202]: ./media/symantec-tutorial/tutorial_general_202.png
[203]: ./media/symantec-tutorial/tutorial_general_203.png

