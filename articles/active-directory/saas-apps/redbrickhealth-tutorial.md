---
title: 'Zelfstudie: Azure Active Directory-integratie met RedBrick Health | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en RedBrick Health.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26290c65-9aa3-42ab-8ba5-901b14dc8e73
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: jeedes
ms.openlocfilehash: bccc7abed9a86bcba74a5d994664a20313f3282a
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52833802"
---
# <a name="tutorial-azure-active-directory-integration-with-redbrick-health"></a>Zelfstudie: Azure Active Directory-integratie met RedBrick Health

In deze zelfstudie leert u hoe u RedBrick Health integreert met Azure Active Directory (Azure AD).

RedBrick Health integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot RedBrick Health heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij RedBrick Health (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met RedBrick Health, moet u de volgende items:

- Een Azure AD-abonnement
- Een RedBrick Health eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen RedBrick wordt de status van de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-redbrick-health-from-the-gallery"></a>Toe te voegen RedBrick wordt de status van de galerie
Voor het configureren van de integratie van RedBrick Health in Azure AD, moet u RedBrick status uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen RedBrick status uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **RedBrick Health**, selecteer **RedBrick Health** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![RedBrick Health in de lijst met resultaten](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met RedBrick Health op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in RedBrick Health is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in RedBrick Health tot stand worden gebracht.

In de gezondheid van RedBrick, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en Azure AD eenmalige aanmelding met RedBrick Health test, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker RedBrick Health](#create-a-redbrick-health-test-user)**  : als u wilt een equivalent van Britta Simon in RedBrick status die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing RedBrick Health.

**Voor het configureren van Azure AD eenmalige aanmelding met RedBrick Health, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **RedBrick Health** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_samlbase.png)

1. Op de **RedBrick Health domein en URL's** sectie, voert u de volgende stappen uit:

    ![RedBrick Health domein en URL's eenmalige aanmelding informatie](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_url.png)

    a. In de **id** tekstvak, een URL typen: `https://www.redbrickhealth.com`
    
    b. In de **antwoord-URL** tekstvak, een URL typen: `https://sso-intg.redbrickhealth.com/sp/ACS.saml2`
    
    Voor productie-omgeving: `https://sso.redbrickhealth.com/sp/ACS.saml2`

    c. Klik op **geavanceerde URL-instellingen weergeven**.
    
    ![RedBrick Health domein en URL's eenmalige aanmelding informatie](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_url1.png)

    d. In de **Relaystatus** tekstvak, een URL met behulp van het volgende patroon: `https://api-sso2.redbricktest.com/identity/sso/nbound?target=https://vanity9-sso2.redbrickdev.com/portal&connection=<companyname>conn1`
    
    > [!NOTE] 
    > Relaystatus waarde is niet echt. Deze waarde bijwerken met de huidige status van de Relay. Neem contact op met [RedBrick Health ondersteuningsteam](https://home.redbrickhealth.com/contact/) deze waarde op te halen.

1. De toepassing RedBrick Health wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. Deze claims specifieke-klant bent en is afhankelijk van uw behoeften. Volgende optioneel claims voorbeeld worden alleen die u kunt configureren voor uw toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie.

    ![Eenmalige aanmelding configureren](./media/redbrickhealth-tutorial/attribute.png)

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:

    | Naam kenmerk | Waarde kenmerk |
    | ---------------| ----------------|
    | Principal-naam | ********** |
    | client-id | ********** |
    | deelnemer-id | ********** |
    
    > [!NOTE]
    > Deze waarden zijn voor verwijzing doel alleen. U moet de kenmerken aan de hand van de behoefte van uw organisatie definiëren. Neem contact op met [RedBrick Health ondersteuningsteam](https://home.redbrickhealth.com/contact/) voor meer informatie over de vereiste claims.
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.
    
    ![Eenmalige aanmelding configureren](./media/redbrickhealth-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/redbrickhealth-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.
    
    e. Klik op **OK**.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/redbrickhealth-tutorial/tutorial_general_400.png)

1. Op de **RedBrick Statusconfiguratie** sectie, klikt u op **RedBrick Health configureren** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID** uit de **Naslaggids sectie.**

    ![Configuratie van redBrick Health](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_configure.png) 

1. Het configureren van eenmalige aanmelding op **RedBrick Health** zijde, moet u voor het verzenden van de gedownloade **Certificate(Base64)** en **SAML entiteit-ID** naar [RedBrick Health ondersteuning voor team](https://home.redbrickhealth.com/contact/). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/redbrickhealth-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/redbrickhealth-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/redbrickhealth-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/redbrickhealth-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-a-redbrick-health-test-user"></a>Maak een testgebruiker RedBrick Health

In deze sectie maakt u een gebruiker met de naam van Britta Simon in RedBrick Health. Werken met [RedBrick Health ondersteuningsteam](https://home.redbrickhealth.com/contact/) om toe te voegen de gebruikers in het RedBrick Health-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot de status van RedBrick.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon RedBrick status, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **RedBrick Health**.

    ![De RedBrick status koppeling in de lijst met toepassingen](./media/redbrickhealth-tutorial/tutorial_redbrickhealth_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel RedBrick status in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing RedBrick Health.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/redbrickhealth-tutorial/tutorial_general_01.png
[2]: ./media/redbrickhealth-tutorial/tutorial_general_02.png
[3]: ./media/redbrickhealth-tutorial/tutorial_general_03.png
[4]: ./media/redbrickhealth-tutorial/tutorial_general_04.png

[100]: ./media/redbrickhealth-tutorial/tutorial_general_100.png

[200]: ./media/redbrickhealth-tutorial/tutorial_general_200.png
[201]: ./media/redbrickhealth-tutorial/tutorial_general_201.png
[202]: ./media/redbrickhealth-tutorial/tutorial_general_202.png
[203]: ./media/redbrickhealth-tutorial/tutorial_general_203.png

