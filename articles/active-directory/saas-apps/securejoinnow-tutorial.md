---
title: 'Zelfstudie: Azure Active Directory-integratie met SecureW2 JoinNow-Connector | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SecureW2 JoinNow Connector.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2445b3af-f827-40de-9097-6f5c933d0f53
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2018
ms.author: jeedes
ms.openlocfilehash: 283f8c935556006a21812578d0638b72adb6eed0
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37908229"
---
# <a name="tutorial-azure-active-directory-integration-with-securew2-joinnow-connector"></a>Zelfstudie: Azure Active Directory-integratie met SecureW2 JoinNow-Connector

In deze zelfstudie leert u hoe u SecureW2 JoinNow Connector integreren met Azure Active Directory (Azure AD).

SecureW2 JoinNow Connector integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SecureW2 JoinNow Connector heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SecureW2 JoinNow Connector (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SecureW2 JoinNow Connector, moet u de volgende items:

- Een Azure AD-abonnement
- Een SecureW2 JoinNow Connector eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SecureW2 JoinNow Connector uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-securew2-joinnow-connector-from-the-gallery"></a>SecureW2 JoinNow Connector uit de galerie toe te voegen
Voor het configureren van de integratie van SecureW2 JoinNow Connector in Azure AD, moet u SecureW2 JoinNow Connector uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SecureW2 JoinNow Connector uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **SecureW2 JoinNow Connector**, selecteer **SecureW2 JoinNow Connector** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SecureW2 JoinNow Connector in de lijst met resultaten](./media/securejoinnow-tutorial/tutorial_securejoinnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met SecureW2 JoinNow Connector op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in SecureW2 JoinNow Connector is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SecureW2 JoinNow Connector tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met SecureW2 JoinNow Connector, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker SecureW2 JoinNow Connector](#create-a-securew2-joinnow-connector-test-user)**  : als u wilt een equivalent van Britta Simon in SecureW2 JoinNow Connector die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing SecureW2 JoinNow Connector.

**Voor het configureren van Azure AD eenmalige aanmelding met SecureW2 JoinNow Connector, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SecureW2 JoinNow Connector** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/securejoinnow-tutorial/tutorial_securejoinnow_samlbase.png)

3. Op de **SecureW2 JoinNow Connector domein en URL's** sectie, voert u de volgende stappen uit:

    ![SecureW2 JoinNow Connector domein en URL's, eenmalige aanmelding informatie](./media/securejoinnow-tutorial/tutorial_securejoinnow_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<organization-identifier>-auth.securew2.com/auth/saml/SSO`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<organization-identifier>-auth.securew2.com/auth/saml`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [SecureW2 JoinNow Connector Client-ondersteuningsteam](mailto:support@securew2.com) om deze waarden te verkrijgen. 

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/securejoinnow-tutorial/tutorial_securejoinnow_certificate.png) 

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/securejoinnow-tutorial/tutorial_general_400.png)

6. Het configureren van eenmalige aanmelding op **SecureW2 JoinNow Connector** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [SecureW2 JoinNow Connector ondersteuningsteam](mailto:support@securew2.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/securejoinnow-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/securejoinnow-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/securejoinnow-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/securejoinnow-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-securew2-joinnow-connector-test-user"></a>Maak een testgebruiker SecureW2 JoinNow-Connector

In deze sectie maakt u een gebruiker met de naam van Britta Simon in SecureW2 JoinNow-Connector. Werken met [SecureW2 JoinNow Connector Client-ondersteuningsteam](mailto:support@securew2.com) om toe te voegen de gebruikers in het platform SecureW2 JoinNow Connector. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan SecureW2 JoinNow Connector.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar SecureW2 JoinNow-Connector, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **SecureW2 JoinNow Connector**.

    ![De koppeling SecureW2 JoinNow Connector in de lijst met toepassingen](./media/securejoinnow-tutorial/tutorial_securejoinnow_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

**Als u wilt testen van de toepassing, moet u de volgende stappen uitvoeren:** 

a. Open de SecureW2 JoinNow Connector-client, selecteer het juiste apparaat uit de lijst en klik op **aanmelden** knop.

b. De standaardbrowser wordt geopend en u moet zich opnieuw gerichte naar de Azure-portal voor verificatie.

c. Op een geslaagde verificatie moet worden geretourneerd naar de oorspronkelijke landingspagina van SecureW2 JoinNow-Connector.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/securejoinnow-tutorial/tutorial_general_01.png
[2]: ./media/securejoinnow-tutorial/tutorial_general_02.png
[3]: ./media/securejoinnow-tutorial/tutorial_general_03.png
[4]: ./media/securejoinnow-tutorial/tutorial_general_04.png

[100]: ./media/securejoinnow-tutorial/tutorial_general_100.png

[200]: ./media/securejoinnow-tutorial/tutorial_general_200.png
[201]: ./media/securejoinnow-tutorial/tutorial_general_201.png
[202]: ./media/securejoinnow-tutorial/tutorial_general_202.png
[203]: ./media/securejoinnow-tutorial/tutorial_general_203.png

