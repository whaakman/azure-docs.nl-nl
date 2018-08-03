---
title: 'Zelfstudie: Azure Active Directory-integratie met Autotask werkplek | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Autotask werkplek.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a9a7ff71-c389-4169-aafd-d7a505244797
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: cc1ee04c9d614e895c4e8a021564e9b9405fa8c0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438957"
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-workplace"></a>Zelfstudie: Azure Active Directory-integratie met Autotask werkplek

In deze zelfstudie leert u hoe werkplek Autotask integreren met Azure Active Directory (Azure AD).

Autotask werkplek integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Autotask werkplek heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Autotask werkplek (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Autotask werkplek, moet u de volgende items:

- Een Azure AD-abonnement
- Een Autotask werkplek eenmalige aanmelding ingeschakeld abonnement
- U moet een beheerder of super-beheerder in de werkplek.
- U moet een administrator-account hebben in de Azure AD.
- De gebruikers die gebruikmaken van deze functie wordt accounts binnen werkplek en de Azure AD moeten hebben en moeten overeenkomen met hun e-mailadressen voor beide.

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Autotask werkplek toevoegen vanuit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-autotask-workplace-from-the-gallery"></a>Autotask werkplek toevoegen vanuit de galerie
Voor het configureren van de integratie van Autotask werkplek in Azure AD, moet u Autotask werkplek toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt Autotask werkplek toevoegen vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **Autotask werkplek**, selecteer **Autotask werkplek** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Autotask werkplek in de lijst met resultaten](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Autotask werkplek op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Autotask werkplek is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Autotask werkplek tot stand worden gebracht.

In de Autotask werkplek, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Autotask werkplek, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker Autotask werkplek](#create-an-autotask-workplace-test-user)**  : als u wilt een equivalent van Britta Simon in Autotask werkplek die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Autotask werkplek.

**Voor het configureren van Azure AD eenmalige aanmelding met Autotask werkplek, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Autotask werkplek** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_samlbase.png)

1. Als u wilt configureren van de toepassing in **IDP** gestart modus, voert u de volgende stappen uit op de **Autotask werkplek domein en URL's** sectie:

    ![Autotask werkplek domein en URL's eenmalige aanmelding-informatie voor id-provider](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`

1. Als u wilt configureren van de toepassing in **SP** gestart modus selectievakje **geavanceerde URL-instellingen weergeven** en voer de volgende stappen uit:

    ![Autotask werkplek domein en URL's één aanmeldings-informatie voor SP](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.awp.autotask.net/loginsso`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [Autotask werkplek Client ondersteuningsteam](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) om deze waarden te verkrijgen. 

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/autotaskworkplace-tutorial/tutorial_general_400.png)

1. In een ander browservenster, moet u zich aanmelden bij werkplek Online met behulp van de administratorreferenties.

    >[!Note]
    >Bij het configureren van de id-provider, moet een subdomein worden opgegeven. Om te bevestigen van het juiste subdomein, meld u aan bij de Online werkplek. Wanneer u bent aangemeld, maak notitie naar het subdomein in de URL.
    >Het subdomein is het gedeelte tussen de 'https://' en '.awp.autotask.net/' en moet VS, eu, ca of Australië.

1. Ga naar **configuratie** > **Single Sign-On** en voer de volgende stappen uit:

    ![Autotask Single Sign-on-configuratie](./media/autotaskworkplace-tutorial/tutorial_autotaskssoconfig1.png)
 
    a. Selecteer de **XML-bestand met metagegevens** optie en upload het **Metadata XML** gedownload vanuit Azure portal.

    b. Klik op **SSO inschakelen**.
    
    ![Configuratie goedkeuren Autotask voor eenmalige aanmelding](./media/autotaskworkplace-tutorial/tutorial_autotaskssoconfig2.png)

    c. Selecteer de **ik bevestig deze informatie juist is en ik vertrouw deze IdP** selectievakje.

    d. Klik op **goedkeuren**.
     
>[!Note]
>Als u hulp bij het configureren van Autotask werkplek nodig hebt, raadpleegt u [deze pagina](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) hulp met uw werkplek te krijgen.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/autotaskworkplace-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/autotaskworkplace-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/autotaskworkplace-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/autotaskworkplace-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-an-autotask-workplace-test-user"></a>Maak een testgebruiker Autotask werkplek

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Autotask. Neem contact op met [Autotask werkplek ondersteuningsteam](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) om toe te voegen de gebruikers in het platform Autotask werkplek.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan de werkplek Autotask.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon Autotask werkplek koppelen, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Autotask werkplek**.

    ![De koppeling Autotask werkplek in de lijst met toepassingen](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Autotask werkplek in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Autotask werkplek.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)


<!--Image references-->

[1]: ./media/autotaskworkplace-tutorial/tutorial_general_01.png
[2]: ./media/autotaskworkplace-tutorial/tutorial_general_02.png
[3]: ./media/autotaskworkplace-tutorial/tutorial_general_03.png
[4]: ./media/autotaskworkplace-tutorial/tutorial_general_04.png

[100]: ./media/autotaskworkplace-tutorial/tutorial_general_100.png

[200]: ./media/autotaskworkplace-tutorial/tutorial_general_200.png
[201]: ./media/autotaskworkplace-tutorial/tutorial_general_201.png
[202]: ./media/autotaskworkplace-tutorial/tutorial_general_202.png
[203]: ./media/autotaskworkplace-tutorial/tutorial_general_203.png

