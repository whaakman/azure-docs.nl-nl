---
title: 'Zelfstudie: Azure Active Directory-integratie met Spotinst | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Spotinst.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2f6dbd70-c2db-4ae9-99ee-976c3090d214
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2018
ms.author: jeedes
ms.openlocfilehash: 7edf1cbc5cc351e25a9ae7b319768376ea9968a3
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39265767"
---
# <a name="tutorial-azure-active-directory-integration-with-spotinst"></a>Zelfstudie: Azure Active Directory-integratie met Spotinst

In deze zelfstudie leert u hoe u Spotinst integreren met Azure Active Directory (Azure AD).

Spotinst integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Spotinst heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Spotinst (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Spotinst, moet u de volgende items:

- Een Azure AD-abonnement
- Een Spotinst eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Spotinst uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-spotinst-from-the-gallery"></a>Spotinst uit de galerie toe te voegen
Voor het configureren van de integratie van Spotinst in Azure AD, moet u Spotinst uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Spotinst uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Spotinst**, selecteer **Spotinst** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Spotinst in de lijst met resultaten](./media/spotinst-tutorial/tutorial_spotinst_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Spotinst op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Spotinst is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Spotinst tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Spotinst, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Spotinst](#create-a-spotinst-test-user)**  : als u wilt een equivalent van Britta Simon in Spotinst die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Spotinst.

**Voor het configureren van Azure AD eenmalige aanmelding met Spotinst, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Spotinst** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/spotinst-tutorial/tutorial_spotinst_samlbase.png)

3. Op de **Spotinst domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in de modus voor IDP gestart:

    ![Spotinst domein en URL's, eenmalige aanmelding informatie](./media/spotinst-tutorial/tutorial_spotinst_url1.png)

    a. Controleer **geavanceerde URL-instellingen weergeven**.

    b. In de **Relaystatus** tekstvak, een waarde: `<ID>`

    c. Als u wilt configureren van de toepassing in **SP** modus gestart de **aanmeldings-URL** tekstvak typt u de URL: `https://console.spotinst.com`

    > [!NOTE]
    > De status van de Relay-waarde is niet echt. U kunt de status van de Relay-waarde wordt bijgewerkt met de werkelijke status van de Relay-waarde, die later in de zelfstudie wordt uitgelegd.

4. Spotinst toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.

    ![Eenmalige aanmelding configureren](./media/spotinst-tutorial/tutorial_Spotinst_attribute.png)

5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:

    | Naam kenmerk | Waarde kenmerk |
    | ---------------| --------------- |
    | Email | User.mail |
    | Voornaam | User.givenName |
    | LastName | User.surname |
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/spotinst-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/spotinst-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Laat de **Namespace** leeg.

    e. Klik op **Ok**

6. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/spotinst-tutorial/tutorial_spotinst_certificate.png) 

7. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/spotinst-tutorial/tutorial_general_400.png)

8. In een ander browservenster, meld u aan bij Spotinst als een beveiligingsbeheerder.

9. Klik op de **Gebruikerspictogram** op de rechtsboven in het scherm en klik op **instellingen**.

    ![Instellingen voor Spotinst](./media/spotinst-tutorial/tutorial_spotinst_settings.png)

10. Klik op de **SECURITY** tabblad in de rechterbovenhoek en selecteer vervolgens **id-Providers** en voer de volgende stappen uit:

    ![Spotinst beveiliging](./media/spotinst-tutorial/tutorial_spotinst_security.png)

    a. Kopieer de **Relaystatus** voor uw exemplaar van de waarde en plak deze in **Relaystatus** -tekstvak in **Spotinst domein en URL's** sectie in Azure portal.

    b. Klik op **Bladeren** voor het uploaden van het metagegevens-xml-bestand dat u hebt gedownload vanuit Azure portal

    c. Klik op **OPSLAAN**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/spotinst-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/spotinst-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/spotinst-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/spotinst-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-spotinst-test-user"></a>Maak een testgebruiker Spotinst

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Spotinst.

1. Als u hebt geconfigureerd dat de toepassing in de **SP** intiated modus, voert u de volgende stappen uit:

   a. In een ander browservenster, meld u aan bij Spotinst als een beveiligingsbeheerder.

   b. Klik op de **Gebruikerspictogram** op de rechtsboven in het scherm en klik op **instellingen**.

    ![Instellingen voor Spotinst](./media/spotinst-tutorial/tutorial_spotinst_settings.png)

    c. Klik op **gebruikers** en selecteer **gebruiker toevoegen**.

    ![Instellingen voor Spotinst](./media/spotinst-tutorial/adduser1.png)

    d. In de sectie van de gebruiker toevoegen de volgende stappen uitvoeren:

    ![Instellingen voor Spotinst](./media/spotinst-tutorial/adduser2.png)

    * In de **volledige naam** tekstvak, voer de volledige naam van gebruiker, zoals **BrittaSimon**.

    * In de **e** tekstvak, voer het e-mailadres van de gebruiker, zoals **brittasimon@contoso.com**.

    * Selecteer uw organisatie-specifieke details voor de **organisatie rol, Accountrol en Accounts**.

2. Als u hebt geconfigureerd dat de toepassing in de **IDP** intiated modus, er is geen actie-item voor u in deze sectie. Spotinst biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Spotinst als deze nog niet bestaat.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Spotinst.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Spotinst toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Spotinst**.

    ![De koppeling Spotinst in de lijst met toepassingen](./media/spotinst-tutorial/tutorial_spotinst_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Spotinst in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Spotinst.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/spotinst-tutorial/tutorial_general_01.png
[2]: ./media/spotinst-tutorial/tutorial_general_02.png
[3]: ./media/spotinst-tutorial/tutorial_general_03.png
[4]: ./media/spotinst-tutorial/tutorial_general_04.png

[100]: ./media/spotinst-tutorial/tutorial_general_100.png

[200]: ./media/spotinst-tutorial/tutorial_general_200.png
[201]: ./media/spotinst-tutorial/tutorial_general_201.png
[202]: ./media/spotinst-tutorial/tutorial_general_202.png
[203]: ./media/spotinst-tutorial/tutorial_general_203.png

