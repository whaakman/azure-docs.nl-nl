---
title: 'Zelfstudie: Azure Active Directory-integratie met Trisotech digitale Enterprise Server | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Trisotech digitale Enterprise-Server.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6d54d20c-eca1-4fa6-b56a-4c3ed0593db0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c4ed44bff002ed8422f199c24a566fe6ff606af8
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56190650"
---
# <a name="tutorial-azure-active-directory-integration-with-trisotech-digital-enterprise-server"></a>Zelfstudie: Azure Active Directory-integratie met Trisotech digitale Enterprise-Server

In deze zelfstudie leert u hoe u Trisotech digitale Enterprise Server integreren met Azure Active Directory (Azure AD).

Trisotech digitale Enterprise Server integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Trisotech digitale Enterprise-Server heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Trisotech digitale Enterprise Server (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Trisotech digitale Enterprise-Server, moet u de volgende items:

- Een Azure AD-abonnement
- Een digitale bedrijfsserver Trisotech eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Trisotech digitale Enterprise Server uit de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-trisotech-digital-enterprise-server-from-the-gallery"></a>Trisotech digitale Enterprise Server uit de galerie toevoegen
Voor het configureren van de integratie van Trisotech digitale Enterprise Server in Azure AD, moet u Trisotech digitale Enterprise Server uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Trisotech digitale Enterprise Server uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Trisotech digitale Enterprise Server**, selecteer **Trisotech digitale Enterprise Server** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen de de toepassing.

    ![Trisotech digitale Enterprise Server in de lijst met resultaten](./media/trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Trisotech digitale Enterprise-Server op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Trisotech digitale Enterprise Server in Azure AD aan een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Trisotech digitale Enterprise Server tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Trisotech digitale Enterprise-Server, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Trisotech digitale Enterprise Server](#create-a-trisotech-digital-enterprise-server-test-user)**  : als u wilt een equivalent van Britta Simon hebben in het digitale Enterprise Server Trisotech, die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Trisotech digitale Enterprise-Server.

**Voor het configureren van Azure AD eenmalige aanmelding met Trisotech digitale Enterprise-Server, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Trisotech digitale Enterprise Server** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_samlbase.png)

1. Op de **Trisotech digitale Enterprise serverdomein en URL's** sectie, voert u de volgende stappen uit:

    ![Trisotech digitale Enterprise serverdomein en URL's, eenmalige aanmelding informatie](./media/trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<companyname>.trisotech.com`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<companyname>.trisotech.com`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [Trisotech digitale Enterprise Server Client ondersteuningsteam](mailto:support@trisotech.com) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok. 

    ![De link om het certificaat te downloaden](./media/trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_certificate.png)

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_400.png)

1. In een ander browservenster aanmelden bij uw bedrijf digitale Enterprise-serverconfiguratie Trisotech site als beheerder.

1. Klik op de **menupictogram** en selecteer vervolgens **beheer**.

    ![Eenmalige aanmelding configureren](./media/trisotechdigitalenterpriseserver-tutorial/user1.png)

1. Selecteer **gebruiker Provider**.

    ![Eenmalige aanmelding configureren](./media/trisotechdigitalenterpriseserver-tutorial/user2.png)

1. In de **Provider gebruikersconfiguraties** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/trisotechdigitalenterpriseserver-tutorial/user3.png)

    a. Selecteer **beveiligd Assertion Markup Language 2 (SAML-2)** in de vervolgkeuzelijst in de **verificatiemethode**.

    b. In de **metagegevens-URL** tekstvak, plak de **App-Url voor federatieve metagegevens** waarde die u hebt gekopieerd, vormen de Azure-portal.

    c. In de **toepassings-ID** tekstvak, voer de URL in met behulp van het volgende patroon: `https://<companyname>.trisotech.com`.

    d. Klik op **Opslaan**.

    e. Voer de naam van het domein in de **toegestaan domeinen (leeg betekent dat iedereen)** tekstvak, deze automatisch wordt toegewezen licenties voor gebruikers die overeenkomen met de domeinen toegestaan

    f. Klik op **Opslaan**.

 ### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/trisotechdigitalenterpriseserver-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/trisotechdigitalenterpriseserver-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/trisotechdigitalenterpriseserver-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/trisotechdigitalenterpriseserver-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-trisotech-digital-enterprise-server-test-user"></a>Maak een testgebruiker Trisotech digitale Enterprise-Server

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Trisotech digitale Enterprise-Server. Trisotech digitale Enterprise Server biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Trisotech digitale Enterprise-Server als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Trisotech digitale Enterprise Server-ondersteuningsteam](mailto:support@trisotech.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Trisotech digitale Enterprise Server.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon Trisotech digitale Enterprise-Server, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Trisotech digitale Enterprise Server**.

    ![De koppeling Trisotech digitale Enterprise Server in de lijst met toepassingen](./media/trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Trisotech digitale Enterprise Server in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Trisotech digitale Enterprise-Server.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_01.png
[2]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_02.png
[3]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_03.png
[4]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_04.png

[100]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_100.png

[200]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_200.png
[201]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_201.png
[202]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_202.png
[203]: ./media/trisotechdigitalenterpriseserver-tutorial/tutorial_general_203.png

