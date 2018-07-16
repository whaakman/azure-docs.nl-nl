---
title: 'Zelfstudie: Azure Active Directory-integratie met absorberen LMS | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en absorberen LMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jeedes
ms.openlocfilehash: 066ae92056e4b80b6627b371d6ebeb3235b2781d
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043775"
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Zelfstudie: Azure Active Directory-integratie met absorberen LMS

In deze zelfstudie leert u hoe u absorberen LMS integreren met Azure Active Directory (Azure AD).

Absorberen LMS integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot absorberen LMS heeft.
- U kunt uw gebruikers automatisch te absorberen LMS (via eenmalige aanmelding) aanmelden met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Als u meer weten over de software als een service (SaaS)-app-integratie met Azure AD wilt, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met absorberen LMS, moet u de volgende items:

- Een Azure AD-abonnement
- Een LMS absorberen eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> U wordt aangeraden niet met behulp van een productie-omgeving voor deze zelfstudie.

Als u wilt testen van de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

* Absorberen LMS uit de galerie toe te voegen
* Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-absorb-lms-from-the-gallery"></a>Absorberen LMS uit de galerie toevoegen
Voor het configureren van de integratie van absorberen LMS in Azure AD, absorberen LMS uit de galerie te toevoegt aan uw lijst met beheerde SaaS-apps.

Als u wilt toevoegen absorberen LMS uit de galerie, het volgende doen:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Ga naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![Het deelvenster met Enterprise-toepassingen][2]
    
3. Als u wilt een toepassing hebt toegevoegd, selecteert u de **nieuwe toepassing** knop.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **absorberen LMS**, selecteer **absorberen LMS** in deelvenster resulteren, en selecteer vervolgens de **toevoegen** knop.

    ![LMS opnemen in de lijst met resultaten](./media/absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met absorberen LMS op basis van een testgebruiker Britta Simon genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker absorberen LMS-equivalent is in Azure AD. Met andere woorden, moet u een koppeling relatie tussen een gebruiker in Azure AD en de bijbehorende gebruiker in LMS kunnen maken.

U deze relatie koppeling tot stand brengen door toe te wijzen de *gebruikersnaam* waarde in Azure AD als de *gebruikersnaam* waarde in absorberen LMS.

Om te configureren en testen van Azure AD eenmalige aanmelding met absorberen LMS, voltooi de bouwstenen die u in de volgende vijf secties.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw absorberen LMS-toepassing.

Voor het configureren van Azure AD eenmalige aanmelding met absorberen LMS, het volgende doen:

1. In de Azure-portal op de **absorberen LMS** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. In de **eenmalige aanmelding** In het dialoogvenster de **modus** Schakel **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. In de **absorberen LMS-domein en URL's** sectie, doet u het volgende:

    ![Absorberen LMS-domein en URL's één aanmeldings-informatie](./media/absorblms-tutorial/tutorial_absorblms_url.png)

    a. In de **id** typt u een URL die gebruikmaakt van de volgende syntaxis: `https://<subdomain>.myabsorb.com/Account/SAML`.

    b. In de **antwoord-URL** typt u een URL die gebruikmaakt van de volgende syntaxis: `https://<subdomain>.myabsorb.com/Account/SAML`.
     
    > [!NOTE] 
    > Deze URL's zijn niet de werkelijke waarden. Deze bijwerken met de werkelijke id en de antwoord-URL's. Als u wilt deze waarden verkrijgt, neem contact op met de [absorberen LMS-client-ondersteuningsteam](https://www.absorblms.com/support). 

4. In de **SAML-handtekeningcertificaat** sectie in de **downloaden** kolom, selecteer **Metadata XML**, en sla het bestand met metagegevens op uw computer.

    ![De koppeling ondertekenen certificaat downloaden](./media/absorblms-tutorial/tutorial_absorblms_certificate.png) 

5. Selecteer **Opslaan**.

    ![Configureren van eenmalige aanmelding opslaan](./media/absorblms-tutorial/tutorial_general_400.png)
    
6. In de **LMS-configuratie absorberen** sectie, selecteer **absorberen LMS configureren** openen **aanmelding configureren** venster en kopieer de **afmelding URL** in de **Naslaggids sectie.**

    ![Het deelvenster absorberen LMS-configuratie](./media/absorblms-tutorial/tutorial_absorblms_configure.png) 

7. In een nieuw browservenster aanmelden bij uw bedrijf absorberen LMS site als beheerder.

8. Selecteer de **Account** bovenaan op de knop rechts. 

    ![De knop Account](./media/absorblms-tutorial/1.png)

9. Selecteer in het deelvenster Account **instellingen voor de Gebruikersportal**.

    ![De instellingen voor de Gebruikersportal-koppeling](./media/absorblms-tutorial/2.png)
    
10. Selecteer de tab **Gebruikers**.

    ![Het tabblad gebruikers](./media/absorblms-tutorial/3.png)

11. Op de pagina configuratie voor eenmalige aanmelding in het volgende doen:

    ![De pagina configuratie voor eenmalige aanmelding](./media/absorblms-tutorial/4.png)

    a. In de **modus** Schakel **Identity Provider gestart**.

    b. Open in Kladblok het certificaat dat u hebt gedownload van de Azure-portal. Verwijder de **---BEGIN CERTIFICATE---** en **---END CERTIFICATE---** tags. Klik in de **sleutel** vak, plak de resterende inhoud.
    
    c. In de **Id-eigenschap** vak, selecteert u het kenmerk dat u hebt geconfigureerd als de gebruikers-id in Azure AD. Bijvoorbeeld, als *userPrincipalName* is geselecteerd in Azure AD, selecteer **gebruikersnaam**.

    d. In de **aanmeldings-URL** vak, plak de **URL van gebruikerstoegang** uit van de toepassing **eigenschappen** pagina van de Azure-portal.

    e. In de **afmeldings-URL van**, plak de **afmelding URL** waarde die u hebt gekopieerd uit de **aanmelding configureren** venster van de Azure-portal.

12. In-/ uitschakelen **ervoor zorgen dat alleen SSO aanmelding** naar **op**.

    ![De wisselknop alleen toestaan SSO-aanmelding](./media/absorblms-tutorial/5.png)

13. Selecteer **opslaan.**

> [!TIP]
> U vindt een beknopte versie van deze instructies in de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app. Nadat u de app vanuit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie aan de onderkant. Zie voor meer informatie, [documentatie over Azure AD embedded]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

In deze sectie maakt u testgebruiker Britta Simon in Azure portal.

![Maak een testgebruiker Azure AD][100]

Als u wilt een testgebruiker maken in Azure AD, het volgende doen:

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**.

    ![De Azure Active Directory-knop](./media/absorblms-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, selecteert u **gebruikers en groepen** > **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/absorblms-tutorial/create_aaduser_02.png) 

3. Aan de bovenkant van het dialoogvenster, selecteer **toevoegen**.
 
    ![De knop toevoegen](./media/absorblms-tutorial/create_aaduser_03.png) 

4. In de **gebruiker** dialoogvenster de volgende handelingen uit:
 
    ![Het dialoogvenster gebruiker](./media/absorblms-tutorial/create_aaduser_04.png) 

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak typt u het e-mailadres van Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde in de **wachtwoord** vak.

    d. Selecteer **Maken**.

### <a name="create-an-absorb-lms-test-user"></a>Maak een testgebruiker absorberen LMS

Voor Azure AD-gebruikers zich aanmelden bij absorberen LMS, moeten ze worden ingesteld in absorberen LMS.  

Setup is een handmatige taak voor absorberen LMS.

Als u een account instelt, het volgende doen:

1. Meld u aan uw bedrijf absorberen LMS site als een beheerder.

2. Selecteer in het linkerdeelvenster **gebruikers**.

    ![De koppeling absorberen LMS-gebruikers](./media/absorblms-tutorial/absorblms_users.png)

3. In de **gebruikers** venster **gebruikers**.

    ![De koppeling gebruikers](./media/absorblms-tutorial/absorblms_userssub.png)

4. In de **nieuwe toevoegen** vervolgkeuzelijst, selecteer **gebruiker**.

    ![De nieuwe toevoegen vervolgkeuzelijst](./media/absorblms-tutorial/absorblms_createuser.png)

5. Op de **gebruiker toevoegen** pagina, de volgende handelingen uit:

    ![De pagina gebruiker toevoegen](./media/absorblms-tutorial/user.png)

    a. In de **voornaam** vak, typ de voornaam, zoals **Julia**.

    b. In de **achternaam** vak, typ de naam van de laatste, zoals **Simon**.
    
    c. In de **gebruikersnaam** typt u een volledige naam, zoals **Britta Simon**.

    d. In de **wachtwoord** Britta Simon van wachtwoord typt.

    e. In de **wachtwoord bevestigen** vak, typ nogmaals het wachtwoord.
    
    f. Stel de **Is actief** overzet naar **Active**.  

6. Selecteer **opslaan.**
 
### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u gebruiker Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan absorberen LMS.

![De de gebruikersrol toewijzen][200]

Gebruiker Britta Simon om aan te wijzen absorberen LMS, het volgende doen:

1. In de Azure-portal, opent u de weergave van toepassingen, gaat u naar de directoryweergave en selecteer vervolgens **bedrijfstoepassingen** > **alle toepassingen**.

    ![De koppeling 'Alle Application'][201] 

2. In de **toepassingen** in de lijst met **absorberen LMS**.

    ![De absorberen LMS-koppeling in de lijst met toepassingen](./media/absorblms-tutorial/tutorial_absorblms_app.png) 

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

4. Selecteer **toevoegen** en klikt u op de **toevoegen toewijzing** venster **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** in het dialoogvenster de **gebruikers** in de lijst met **Britta Simon**.

6. In de **gebruikers en groepen** in het dialoogvenster, selecteer de **Selecteer** knop.

7. In de **toevoegen toewijzing** in het dialoogvenster, selecteer de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

In het toegangsvenster, selecteren de **absorberen LMS** tegel automatisch aangemeld bij uw absorberen LMS-toepassing. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/absorblms-tutorial/tutorial_general_01.png
[2]: ./media/absorblms-tutorial/tutorial_general_02.png
[3]: ./media/absorblms-tutorial/tutorial_general_03.png
[4]: ./media/absorblms-tutorial/tutorial_general_04.png

[100]: ./media/absorblms-tutorial/tutorial_general_100.png

[200]: ./media/absorblms-tutorial/tutorial_general_200.png
[201]: ./media/absorblms-tutorial/tutorial_general_201.png
[202]: ./media/absorblms-tutorial/tutorial_general_202.png
[203]: ./media/absorblms-tutorial/tutorial_general_203.png
