---
title: 'Zelfstudie: Azure Active Directory-integratie met Mimecast-beheerconsole | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Mimecast-beheerconsole.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: ce4142c5b4a20886a94c87699f262f7238fc2cb4
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438566"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Zelfstudie: Azure Active Directory-integratie met Mimecast-beheerconsole

In deze zelfstudie leert u hoe u Mimecast-beheerconsole integreren met Azure Active Directory (Azure AD).

Beheerconsole Mimecast integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Mimecast-beheerconsole heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Mimecast-beheerconsole (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Mimecast-beheerconsole, moet u de volgende items:

- Een Azure AD-abonnement
- Een Mimecast-beheerconsole eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Mimecast-beheerconsole uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-mimecast-admin-console-from-the-gallery"></a>Mimecast-beheerconsole uit de galerie toe te voegen
Voor het configureren van de integratie van Mimecast-beheerconsole in Azure AD, moet u Mimecast-beheerconsole uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Mimecast-beheerconsole uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **Mimecast-beheerconsole**, selecteer **Mimecast-beheerconsole** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Mimecast-beheerconsole in de lijst met resultaten](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Mimecast-beheerconsole op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de beheerconsole Mimecast is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de beheerconsole Mimecast tot stand worden gebracht.

In de beheerconsole Mimecast, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Mimecast-beheerconsole, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker Mimecast-beheerconsole](#create-a-mimecast-admin-console-test-user)**  : als u wilt een equivalent van Britta Simon in Mimecast-beheerconsole die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Mimecast-beheerconsole.

**Voor het configureren van Azure AD eenmalige aanmelding met Mimecast-beheerconsole, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Mimecast-beheerconsole** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_samlbase.png)

1. Op de **Mimecast Admin Consoledomein en URL's** sectie, voert u de volgende stappen uit:

    ![Mimecast Admin Consoledomein en URL's, eenmalige aanmelding informatie](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_url.png)

    In de **aanmeldings-URL** tekstvak typt u de URL:
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > De aanmeldings-URL is regiospecifiek.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/mimecast-admin-console-tutorial/tutorial_general_400.png)

1. Op de **Mimecast Console beheerconfiguratie** sectie, klikt u op **configureren Mimecast-beheerconsole** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![De configuratie van de Mimecast Administrator-Console](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_configure.png) 

1. Meld u in een ander browservenster in de beheerconsole Mimecast als beheerder.

1. Ga naar **Services \> toepassing**.

    ![Services](./media/mimecast-admin-console-tutorial/ic794998.png "Services")

1. Klik op **verificatie profielen**.

    ![Verificatie-profielen](./media/mimecast-admin-console-tutorial/ic794999.png "verificatie-profielen")
    
1. Klik op **nieuwe verificatie-profiel**.

    ![Nieuwe verificatie-profielen](./media/mimecast-admin-console-tutorial/ic795000.png "nieuwe verificatie-profielen")

1. In de **verificatie profiel** sectie, voert u de volgende stappen uit:

    ![Verificatie-profiel](./media/mimecast-admin-console-tutorial/ic795015.png "verificatie-profiel")
    
    a. In de **beschrijving** tekstvak, typ een naam voor uw configuratie.
    
    b. Selecteer **afdwingen van SAML-verificatie voor de beheerconsole Mimecast**.
    
    c. Als **Provider**, selecteer **Azure Active Directory**.
    
    d. Plakken **SAML entiteit-ID**, die u hebt gekopieerd vanuit de Azure portal in de **URL-verlener** tekstvak.
    
    e. Plakken **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit de Azure portal in de **aanmeldings-URL** tekstvak.

    f. Plakken **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit de Azure portal in de **afmeldings-URL van** tekstvak.
    
    >[!NOTE]
    >De aanmeldings-URL-waarde en de waarde van de afmeldings-URL zijn voor de beheerconsole Mimecast hetzelfde.
    
    g. Open uw Base64-certificaat gedownload vanuit Azure portal in Kladblok, verwijdert u de eerste regel ("*--*') en de laatste regel ("*--*"), Kopieer de resterende inhoud van deze in uw Klembord en plakt u deze naar de **id-Provider-certificaat (metagegevens)** tekstvak.
    
    h. Selecteer **eenmalige aanmelding op toestaan**.
    
    i. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985) 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/mimecast-admin-console-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/mimecast-admin-console-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/mimecast-admin-console-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/mimecast-admin-console-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-mimecast-admin-console-test-user"></a>Maak een testgebruiker Mimecast-beheerconsole

Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij Mimecast-beheerconsole, moeten ze worden ingericht voor Mimecast-beheerconsole. In het geval van Mimecast-beheerconsole is inrichten een handmatige taak.

* U moet een domein registreren voordat u gebruikers kunt maken.

**Als u wilt inrichten van gebruikers configureren, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Mimecast-beheerconsole** als administrator.
1. Ga naar **mappen \> interne**.
   
   ![Mappen](./media/mimecast-admin-console-tutorial/ic795003.png "mappen")
1. Klik op **nieuw domein registreren**.
   
   ![Nieuwe domein registreren](./media/mimecast-admin-console-tutorial/ic795004.png "nieuw domein registreren")
1. Nadat het nieuwe domein is gemaakt, klikt u op **nieuw adres**.
   
   ![Nieuw adres](./media/mimecast-admin-console-tutorial/ic795005.png "nieuw adres")
1. In het dialoogvenster Nieuw adres, moet u de volgende stappen uitvoeren:
   
   ![Sla](./media/mimecast-admin-console-tutorial/ic795006.png "opslaan")
   
   a. Type de **e-mailadres**, **globale naam**, **wachtwoord**, en **wachtwoord bevestigen** kenmerken van een geldige Azure AD account dat u wilt in te richten in de bijbehorende tekstvakken.

   b. Klik op **Opslaan**.

>[!NOTE]
>U kunt een andere Mimecast-beheerconsole gebruiker-account maken-hulpprogramma's of API's die worden geleverd door Mimecast-beheerconsole gebruiken voor het inrichten van gebruikersaccounts van de Azure AD. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot Mimecast-beheerconsole.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Mimecast-beheerconsole, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Mimecast-beheerconsole**.

    ![De koppeling Mimecast-beheerconsole in de lijst met toepassingen](./media/mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Mimecast-beheerconsole in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Mimecast-beheerconsole.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/mimecast-admin-console-tutorial/tutorial_general_01.png
[2]: ./media/mimecast-admin-console-tutorial/tutorial_general_02.png
[3]: ./media/mimecast-admin-console-tutorial/tutorial_general_03.png
[4]: ./media/mimecast-admin-console-tutorial/tutorial_general_04.png

[100]: ./media/mimecast-admin-console-tutorial/tutorial_general_100.png

[200]: ./media/mimecast-admin-console-tutorial/tutorial_general_200.png
[201]: ./media/mimecast-admin-console-tutorial/tutorial_general_201.png
[202]: ./media/mimecast-admin-console-tutorial/tutorial_general_202.png
[203]: ./media/mimecast-admin-console-tutorial/tutorial_general_203.png

