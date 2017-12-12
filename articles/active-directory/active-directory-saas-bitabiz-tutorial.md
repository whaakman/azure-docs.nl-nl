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
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.openlocfilehash: 6265f11216aaa75a2dd0dbbf572ecbd913135458
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bitabiz"></a>Zelfstudie: Azure Active Directory-integratie met BitaBIZ

In deze zelfstudie leert u hoe BitaBIZ integreren met Azure Active Directory (Azure AD).

BitaBIZ integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot BitaBIZ heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij BitaBIZ (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met BitaBIZ, moet u de volgende items:

- Een Azure AD-abonnement
- Een BitaBIZ eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. BitaBIZ uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-bitabiz-from-the-gallery"></a>BitaBIZ uit de galerie toevoegen
Voor het configureren van de integratie van BitaBIZ in Azure AD, moet u BitaBIZ uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen BitaBIZ uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **BitaBIZ**, selecteer **BitaBIZ** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![BitaBIZ in de lijst met resultaten](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met BitaBIZ op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in BitaBIZ is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in BitaBIZ tot stand worden gebracht.

Wijs in BitaBIZ, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met BitaBIZ, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker BitaBIZ](#create-a-bitabiz-test-user)**  - BitaBIZ die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing BitaBIZ configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met BitaBIZ, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **BitaBIZ** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_samlbase.png)

3. Op de **BitaBIZ domein en de URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in de IDP geïnitieerd modus:

    ![URL's en BitaBIZ domein eenmalige aanmelding informatie](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_url.png)

    In de **id** textbox, typ een URL met het volgende patroon volgen:`https://www.bitabiz.com/<instanceId>`

    > [!NOTE] 
    > De waarde in de bovenstaande URL is voor alleen demonstratie. Werk de waarde met de werkelijke id, die verderop in de zelfstudie wordt uitgelegd.

4. Controleer **weergeven geavanceerde instellingen voor URL** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en BitaBIZ domein eenmalige aanmelding informatie](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_url1.png)

    In de **aanmeldings-URL** textbox, typ de URL:`https://www.bitabiz.com/dashboard`

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_certificate.png) 

6. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-bitabiz-tutorial/tutorial_general_400.png)
    
7. Op de **BitaBIZ configuratie** sectie, klikt u op **configureren BitaBIZ** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![BitaBIZ configuratie](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_configure.png) 

8. In een ander browservenster aanmelding bij uw tenant BitaBIZ als beheerder.

9. Klik op **SETUP ADMIN**.

    ![BitaBIZ configuratie](./media/active-directory-saas-bitabiz-tutorial/settings1.png)

10. Klik op **Microsoft integraties** onder **waarde toevoegen** sectie.

    ![BitaBIZ configuratie](./media/active-directory-saas-bitabiz-tutorial/settings2.png)

11. Blader omlaag naar de sectie **Microsoft Azure AD (inschakelen voor eenmalige aanmelding)** en voert u de volgende stappen:

    ![BitaBIZ configuratie](./media/active-directory-saas-bitabiz-tutorial/settings3.png)

    a. Kopieer de waarde van de **entiteit-ID ('id' in Azure AD)** textbox en plak deze in de **id** textbox op de **BitaBIZ domein en de URL's** sectie in Azure-portal. 
    
    b. In de **Azure AD Single Sign-On Service-URL** textbox plakken **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal.
    
    c. In de **entiteit-ID van Azure AD SAML** textbox plakken **SAML entiteit-ID**, die u hebt gekopieerd vanuit Azure-portal.

    d. Open uw gedownloade **Certificate(Base64)** bestand in Kladblok, Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **Azure AD certificaat voor ondertekening (Base64-gecodeerd)** textbox.

    e. Toevoegen van uw zakelijke e-maildomein dat wil zeggen mycompany.com in naam **domeinnaam** textbox SSO toewijzen aan de gebruikers in uw bedrijf met dit e-maildomein (niet verplicht).
    
    f. Markeer **SSO ingeschakeld** de BitaBIZ-account.
    
    g. Klik op **Azure AD-configuratie op te slaan** opslaan en activeer de SSO-configuratie.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-bitabiz-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-bitabiz-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-bitabiz-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-bitabiz-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-bitabiz-test-user"></a>Een testgebruiker BitaBIZ maken

Om Azure AD-gebruikers zich aanmelden bij BitaBIZ, moeten ze worden ingericht in BitaBIZ.  
In het geval van BitaBIZ is inrichting een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf BitaBIZ site als beheerder.

2. Klik op **SETUP ADMIN**.

    ![BitaBIZ gebruiker toevoegen](./media/active-directory-saas-bitabiz-tutorial/settings1.png)

3. Klik op **gebruikers toevoegen** onder **organisatie** sectie.

    ![BitaBIZ gebruiker toevoegen](./media/active-directory-saas-bitabiz-tutorial/user1.png)

4. Klik op **toevoegen nieuwe werknemer**.

    ![BitaBIZ gebruiker toevoegen](./media/active-directory-saas-bitabiz-tutorial/user2.png)

5. Op de **'Nieuwe werknemer toevoegen'** dialoogvenster pagina, voert u de volgende stappen uit:

    ![BitaBIZ gebruiker toevoegen](./media/active-directory-saas-bitabiz-tutorial/user3.png)

    a. In de **voornaam** textbox, de eerste naam van gebruiker zoals Britta.

    b. In de **achternaam** textbox, typt u de achternaam van de gebruiker zoals Simon.

    c. In de **e** textbox, typ het e-mailadres van gebruiker, zoals Brittasimon@contoso.com.

    d. Selecteer een datum in **datum van arbeid**.

    e. Er zijn andere niet-verplichte gebruikerskenmerken die kunnen worden ingesteld voor de gebruiker. Raadpleeg de [werknemer Setup Doc](https://help.bitabiz.dk/manage-or-set-up-your-account/on-boarding-employees/new-employee) voor meer informatie.    
    
    f. Klik op **opslaan werknemer**.
    
    > [!NOTE]
    > De houder van Azure Active Directory-account ontvangt een e-mailbericht en volgt een koppeling om hun account te bevestigen voordat deze geactiveerd wordt.
    
### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan BitaBIZ.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen BitaBIZ, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **BitaBIZ**.

    ![De koppeling BitaBIZ in de lijst met toepassingen](./media/active-directory-saas-bitabiz-tutorial/tutorial_bitabiz_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel BitaBIZ in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing BitaBIZ.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bitabiz-tutorial/tutorial_general_203.png

