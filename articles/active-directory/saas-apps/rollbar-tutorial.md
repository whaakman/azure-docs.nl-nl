---
title: 'Zelfstudie: Azure Active Directory-integratie met Rollbar | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Rollbar.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 57537e54-9388-4272-a610-805ce45a451f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/04/2017
ms.author: jeedes
ms.openlocfilehash: 6b1bc9b0eaf7ff94a2ba51a521ba6fb75cef13f9
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041837"
---
# <a name="tutorial-azure-active-directory-integration-with-rollbar"></a>Zelfstudie: Azure Active Directory-integratie met Rollbar

In deze zelfstudie leert u hoe u Rollbar integreren met Azure Active Directory (Azure AD).

Rollbar integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Rollbar heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Rollbar (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Rollbar, moet u de volgende items:

- Een Azure AD-abonnement
- Een Rollbar eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Rollbar uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-rollbar-from-the-gallery"></a>Rollbar uit de galerie toe te voegen
Voor het configureren van de integratie van Rollbar in Azure AD, moet u Rollbar uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Rollbar uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Rollbar**, selecteer **Rollbar** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Rollbar in de lijst met resultaten](./media/rollbar-tutorial/tutorial_rollbar_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Rollbar op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Rollbar is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Rollbar tot stand worden gebracht.

In Rollbar, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Rollbar, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Rollbar](#create-a-rollbar-test-user)**  : als u wilt een equivalent van Britta Simon in Rollbar die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Rollbar.

**Voor het configureren van Azure AD eenmalige aanmelding met Rollbar, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Rollbar** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/rollbar-tutorial/tutorial_rollbar_samlbase.png)

3. Op de **Rollbar domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Rollbar domein en URL's, eenmalige aanmelding informatie](./media/rollbar-tutorial/tutorial_rollbar_url.png)

    a. In de **id** tekstvak typt u de URL: `https://saml.rollbar.com`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://rollbar.com/<accountname>/saml/sso/azure/`

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Rollbar domein en URL's, eenmalige aanmelding informatie](./media/rollbar-tutorial/tutorial_rollbar_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://rollbar.com/<accountname>/saml/login/azure/`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke antwoord-URL en de aanmeldings-URL. Neem contact op met [Rollbar Client ondersteuningsteam](mailto:support@rollbar.com) om deze waarden te verkrijgen. 

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/rollbar-tutorial/tutorial_rollbar_certificate.png) 

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/rollbar-tutorial/tutorial_general_400.png)
    
7. In een ander browservenster aanmelden bij uw bedrijf Rollbar site als beheerder.

8. Klik op de **profielinstellingen** in de rechterbovenhoek en vervolgens op **accountnaam instellingen**.
    
    ![Configuratie](./media/rollbar-tutorial/general.png)

9. Klik op **id-Provider** onder beveiliging.

    ![Configuratie](./media/rollbar-tutorial/configure1.png)

10. In de **SAML-identiteitsprovider** sectie, voert u de volgende stappen uit:
    
    ![Configuratie](./media/rollbar-tutorial/configure2.png)

    a. Selecteer **AZURE** uit de **SAML-identiteitsprovider** vervolgkeuzelijst.

    b. Open het bestand met metagegevens in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **SAML-metagegevens** tekstvak.

    c. Klik op **Opslaan**.

11. Na het opslaan te klikken op de knop, het scherm worden als volgt:
    
    ![Configuratie](./media/rollbar-tutorial/configure3.png)
    > [!NOTE] 
    > Als u wilt de volgende stap hebt voltooid, moet u eerst toevoegen zelf als een gebruiker naar de Rollbar-app in Azure.
    a. Als u wilt dat alle gebruikers moeten verifiëren via Azure, en klik vervolgens op **aanmelden via uw id-provider** opnieuw te verifiëren via Azure.  

    b.  Zodra u bent weer terug naar het scherm, selecteert u de **vereisen aanmelding via SAML-identiteitsprovider** selectievakje.

    b. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/rollbar-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/rollbar-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/rollbar-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/rollbar-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-rollbar-test-user"></a>Maak een testgebruiker Rollbar

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Rollbar, moeten ze worden ingericht voor Rollbar. In het geval van Rollbar is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Rollbar site aan als beheerder.

2. Klik op de **profielinstellingen** in de rechterbovenhoek en vervolgens op **accountnaam instellingen**.

    ![Gebruiker](./media/rollbar-tutorial/general.png)

3. Klik op **gebruikers**.
    
    ![Werknemer toevoegen](./media/rollbar-tutorial/user1.png)

4. Klik op **uitnodigen teamleden**.

    ![Anderen uitnodigen](./media/rollbar-tutorial/user2.png)

5. Voer de naam van gebruiker, zoals in het tekstvak **brittasimon@contoso.com** en klik op **toevoegen/uitnodiging**.

    ![Anderen uitnodigen](./media/rollbar-tutorial/user3.png)

6. Gebruiker ontvangt een uitnodiging voor en na accepteren komt gemaakt in het systeem.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Rollbar.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Rollbar toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Rollbar**.

    ![De koppeling Rollbar in de lijst met toepassingen](./media/rollbar-tutorial/tutorial_rollbar_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Rollbar in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Rollbar.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/rollbar-tutorial/tutorial_general_01.png
[2]: ./media/rollbar-tutorial/tutorial_general_02.png
[3]: ./media/rollbar-tutorial/tutorial_general_03.png
[4]: ./media/rollbar-tutorial/tutorial_general_04.png

[100]: ./media/rollbar-tutorial/tutorial_general_100.png

[200]: ./media/rollbar-tutorial/tutorial_general_200.png
[201]: ./media/rollbar-tutorial/tutorial_general_201.png
[202]: ./media/rollbar-tutorial/tutorial_general_202.png
[203]: ./media/rollbar-tutorial/tutorial_general_203.png

