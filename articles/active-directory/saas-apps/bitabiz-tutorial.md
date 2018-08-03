---
title: 'Zelfstudie: Azure Active Directory-integratie met BitaBIZ | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en BitaBIZ.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 1a51e677-c62b-4aee-9c61-56926aaaa899
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.openlocfilehash: 2a05a4f1b9162a69e074bf6243236df48c8ce536
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39429659"
---
# <a name="tutorial-azure-active-directory-integration-with-bitabiz"></a>Zelfstudie: Azure Active Directory-integratie met BitaBIZ

In deze zelfstudie leert u hoe u BitaBIZ integreren met Azure Active Directory (Azure AD).

BitaBIZ integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot BitaBIZ heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij BitaBIZ (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met BitaBIZ, moet u de volgende items:

- Een Azure AD-abonnement
- Een BitaBIZ eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. BitaBIZ uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-bitabiz-from-the-gallery"></a>BitaBIZ uit de galerie toe te voegen
Voor het configureren van de integratie van BitaBIZ in Azure AD, moet u BitaBIZ uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen BitaBIZ uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **BitaBIZ**, selecteer **BitaBIZ** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![BitaBIZ in de lijst met resultaten](./media/bitabiz-tutorial/tutorial_bitabiz_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met BitaBIZ op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in BitaBIZ is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in BitaBIZ tot stand worden gebracht.

In BitaBIZ, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met BitaBIZ, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker BitaBIZ](#create-a-bitabiz-test-user)**  : als u wilt een equivalent van Britta Simon in BitaBIZ die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing BitaBIZ.

**Voor het configureren van Azure AD eenmalige aanmelding met BitaBIZ, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **BitaBIZ** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/bitabiz-tutorial/tutorial_bitabiz_samlbase.png)

1. Op de **BitaBIZ domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in de modus voor IDP gestart:

    ![BitaBIZ domein en URL's, eenmalige aanmelding informatie](./media/bitabiz-tutorial/tutorial_bitabiz_url.png)

    In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://www.bitabiz.com/<instanceId>`

    > [!NOTE] 
    > De waarde in de bovenstaande URL is voor een demonstratie alleen. Werk de waarde bij met de werkelijke id, die later in de zelfstudie wordt uitgelegd.

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![BitaBIZ domein en URL's, eenmalige aanmelding informatie](./media/bitabiz-tutorial/tutorial_bitabiz_url1.png)

    In de **aanmeldings-URL** tekstvak typt u de URL: `https://www.bitabiz.com/dashboard`

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/bitabiz-tutorial/tutorial_bitabiz_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/bitabiz-tutorial/tutorial_general_400.png)
    
1. Op de **BitaBIZ configuratie** sectie, klikt u op **configureren BitaBIZ** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![BitaBIZ configuratie](./media/bitabiz-tutorial/tutorial_bitabiz_configure.png) 

1. In een ander browservenster aanmelden voor uw tenant BitaBIZ als beheerder.

1. Klik op **SETUP ADMIN**.

    ![BitaBIZ configuratie](./media/bitabiz-tutorial/settings1.png)

1. Klik op **Microsoft integraties** onder **meerwaarde** sectie.

    ![BitaBIZ configuratie](./media/bitabiz-tutorial/settings2.png)

1. Schuif omlaag naar het gedeelte **Microsoft Azure AD (inschakelen voor eenmalige aanmelding)** en voert u de volgende stappen uit:

    ![BitaBIZ configuratie](./media/bitabiz-tutorial/settings3.png)

    a. Kopieer de waarde van de **entiteit-ID ('id' in Azure AD)** tekstvak en plak deze in de **id** tekstvak op de **BitaBIZ domein en URL's** sectie in Azure portal. 
    
    b. In de **Azure AD-aanmelding Service-URL met eenmalige** tekstvak plakken **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.
    
    c. In de **entiteit-ID voor Azure AD SAML** tekstvak plakken **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure portal.

    d. Open uw gedownloade **Certificate(Base64)** -bestand in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **Azure AD-handtekeningcertificaat (Base64-gecodeerd)** tekstvak.

    e. Toevoegen van uw zakelijke e-maildomein dat wil zeggen, mycompany.com in naam **domeinnaam** tekstvak SSO toewijzen aan de gebruikers in uw bedrijf met dit e-maildomein (niet verplicht).
    
    f. Mark **eenmalige aanmelding ingeschakeld** de BitaBIZ-account.
    
    g. Klik op **Azure AD-configuratie op te slaan** opslaan en activeer de SSO-configuratie.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/bitabiz-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/bitabiz-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/bitabiz-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/bitabiz-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-bitabiz-test-user"></a>Maak een testgebruiker BitaBIZ

Als u wilt dat Azure AD-gebruikers zich aanmelden bij BitaBIZ, moeten ze worden ingericht voor BitaBIZ.  
In het geval van BitaBIZ is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf BitaBIZ site aan als beheerder.

1. Klik op **SETUP ADMIN**.

    ![BitaBIZ gebruiker toevoegen](./media/bitabiz-tutorial/settings1.png)

1. Klik op **gebruikers toevoegen** onder **organisatie** sectie.

    ![BitaBIZ gebruiker toevoegen](./media/bitabiz-tutorial/user1.png)

1. Klik op **toevoegen nieuwe werknemer**.

    ![BitaBIZ gebruiker toevoegen](./media/bitabiz-tutorial/user2.png)

1. Op de **'Een nieuwe werknemer toevoegen'** dialoogvenster pagina, voert u de volgende stappen uit:

    ![BitaBIZ gebruiker toevoegen](./media/bitabiz-tutorial/user3.png)

    a. In de **voornaam** tekstvak, de eerste naam van gebruiker, zoals Julia.

    b. In de **achternaam** tekstvak typt u de achternaam van de gebruiker, zoals Simon.

    c. In de **e** tekstvak, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    d. Selecteer een datum in **datum dienst**.

    e. Er zijn andere niet-verplichte gebruikerskenmerken die kunnen worden ingesteld voor de gebruiker. Raadpleeg de [werknemer Setup Doc](https://help.bitabiz.dk/manage-or-set-up-your-account/on-boarding-employees/new-employee) voor meer informatie.    
    
    f. Klik op **opslaan werknemer**.
    
    > [!NOTE]
    > De houder van Azure Active Directory-account ontvangt een e-mailbericht en volgt een koppeling om te bevestigen van hun account voordat deze geactiveerd wordt.
    
### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan BitaBIZ.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan BitaBIZ toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **BitaBIZ**.

    ![De koppeling BitaBIZ in de lijst met toepassingen](./media/bitabiz-tutorial/tutorial_bitabiz_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel BitaBIZ in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing BitaBIZ.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/bitabiz-tutorial/tutorial_general_01.png
[2]: ./media/bitabiz-tutorial/tutorial_general_02.png
[3]: ./media/bitabiz-tutorial/tutorial_general_03.png
[4]: ./media/bitabiz-tutorial/tutorial_general_04.png

[100]: ./media/bitabiz-tutorial/tutorial_general_100.png

[200]: ./media/bitabiz-tutorial/tutorial_general_200.png
[201]: ./media/bitabiz-tutorial/tutorial_general_201.png
[202]: ./media/bitabiz-tutorial/tutorial_general_202.png
[203]: ./media/bitabiz-tutorial/tutorial_general_203.png

