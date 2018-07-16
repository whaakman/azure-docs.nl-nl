---
title: 'Zelfstudie: Azure Active Directory-integratie met Apex-Portal | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Apex-Portal.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: db204a46-6460-4ace-bdbb-4353846723ad
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2017
ms.author: jeedes
ms.openlocfilehash: 4c267313e4851e621b57aa1d2bddc73118405776
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054230"
---
# <a name="tutorial-azure-active-directory-integration-with-apex-portal"></a>Zelfstudie: Azure Active Directory-integratie met het toppunt van Portal

In deze zelfstudie leert u hoe u het toppunt van Portal integreren met Azure Active Directory (Azure AD).

Apex Portal integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Apex-Portal heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij de Portal Apex (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Apex-Portal, moet u de volgende items:

- Een Azure AD-abonnement
- Een Portal voor het toppunt van eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Apex Portal uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-apex-portal-from-the-gallery"></a>Apex Portal uit de galerie toe te voegen
Voor het configureren van de integratie van Apex Portal in Azure AD, moet u het toppunt van Portal vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Apex Portal vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Apex Portal**, selecteer **Apex Portal** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![APEX-Portal in de lijst met resultaten](./media/apexportal-tutorial/tutorial_apexonline_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Apex-Portal op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de Apex-Portal is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in het toppunt van Portal tot stand worden gebracht.

Wijs in het toppunt van Portal, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Apex-Portal, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Apex Portal](#create-an-apex-online-test-user)**  : als u wilt een equivalent van Britta Simon hebben in de Apex-Portal die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Apex-Portal.

**Voor het configureren van Azure AD eenmalige aanmelding met Apex-Portal, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Apex Portal** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/apexportal-tutorial/tutorial_apexonline_samlbase.png)

3. Op de **Apex Portal domein en URL's** sectie, voert u de volgende stappen uit:

    ![APEX Portal domein en URL's, eenmalige aanmelding informatie](./media/apexportal-tutorial/tutorial_apexonline_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<customer name>.apexanalytix.com/saml/sso.aspx`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<customer name>.apexanalytix.com/saml/sso.aspx`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke id en de antwoord-URL. Neem contact op met [Apex Portal ondersteuningsteam](mailto:support@apexanalytix.com) om deze waarden te verkrijgen.
 
4. De toepassing Apex Portal wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. 

    ![Eenmalige aanmelding configureren](./media/apexportal-tutorial/attribute.png)

5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de voorgaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | VOORNAAM | User.givenName |
    | ACHTERNAAM | User.surname |
    | E-MAIL | User.mail |    

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/apexportal-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/apexportal-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

6. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/apexportal-tutorial/tutorial_apexonline_certificate.png) 

7. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/apexportal-tutorial/tutorial_general_400.png)

8. Het configureren van eenmalige aanmelding op **Apex Portal** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [Apex Portal ondersteuningsteam](mailto:support@apexanalytix.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/apexportal-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/apexportal-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/apexportal-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/apexportal-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
  
### <a name="create-an-apex-portal-test-user"></a>Maak een testgebruiker Apex-Portal

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in de Apex-Portal. APEX-Portal biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot het toppunt van Portal als deze nog niet bestaat.
 
> [!NOTE]
> Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met de [Apex Portal ondersteuningsteam](mailto:support@apexanalytix.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot het toppunt van Portal.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon Apex-Portal, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Apex Portal**.

    ![De Apex-Portal-koppeling in de lijst met toepassingen](./media/apexportal-tutorial/tutorial_apexonline_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Apex Portal in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Apex-Portal.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/apexportal-tutorial/tutorial_general_01.png
[2]: ./media/apexportal-tutorial/tutorial_general_02.png
[3]: ./media/apexportal-tutorial/tutorial_general_03.png
[4]: ./media/apexportal-tutorial/tutorial_general_04.png

[100]: ./media/apexportal-tutorial/tutorial_general_100.png

[200]: ./media/apexportal-tutorial/tutorial_general_200.png
[201]: ./media/apexportal-tutorial/tutorial_general_201.png
[202]: ./media/apexportal-tutorial/tutorial_general_202.png
[203]: ./media/apexportal-tutorial/tutorial_general_203.png

