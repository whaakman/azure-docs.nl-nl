---
title: 'Zelfstudie: Azure Active Directory-integratie met XaitPorter | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en XaitPorter.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d33c7cb7-0550-425b-882a-619a713a71b7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6cd4f9e561a55c010273b48fb4232348baad3582
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56200935"
---
# <a name="tutorial-azure-active-directory-integration-with-xaitporter"></a>Zelfstudie: Azure Active Directory-integratie met XaitPorter

In deze zelfstudie leert u hoe u XaitPorter integreren met Azure Active Directory (Azure AD).

XaitPorter integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot XaitPorter heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij XaitPorter (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met XaitPorter, moet u de volgende items:

- Een Azure AD-abonnement
- Een XaitPorter eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. XaitPorter uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-xaitporter-from-the-gallery"></a>XaitPorter uit de galerie toe te voegen
Voor het configureren van de integratie van XaitPorter in Azure AD, moet u XaitPorter uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen XaitPorter uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **XaitPorter**, selecteer **XaitPorter** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![XaitPorter in de lijst met resultaten](./media/xaitporter-tutorial/tutorial_xaitporter_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met XaitPorter op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in XaitPorter is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in XaitPorter tot stand worden gebracht.

In XaitPorter, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met XaitPorter, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker XaitPorter](#create-a-xaitporter-test-user)**  : als u wilt een equivalent van Britta Simon in XaitPorter die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing XaitPorter.

**Voor het configureren van Azure AD eenmalige aanmelding met XaitPorter, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **XaitPorter** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/xaitporter-tutorial/tutorial_xaitporter_samlbase.png)

1. Op de **XaitPorter domein en URL's** sectie, voert u de volgende stappen uit:

    ![XaitPorter domein en URL's, eenmalige aanmelding informatie](./media/xaitporter-tutorial/tutorial_xaitporter_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<subdomain>.xaitporter.com/saml/login`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<subdomain>.xaitporter.com`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [XaitPorter Client ondersteuningsteam](https://www.xait.com/support/) om deze waarden te verkrijgen.
     
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op de knop kopiëren om te kopiëren **App-Url voor federatieve metagegevens** en plak deze in Kladblok. 

    ![De link om het certificaat te downloaden](./media/xaitporter-tutorial/tutorial_xaitporter_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/xaitporter-tutorial/tutorial_general_400.png)

1. Geef de **IP-adres** of de **App-Url voor federatieve metagegevens** naar de [SmartRecruiters ondersteuningsteam](https://www.smartrecruiters.com/about-us/contact-us/), zodat XaitPorter kunt controleren of het IP-adres dat bereikbaar is vanaf is uw Het exemplaar van de XaitPorter whitelist bij hun kant configureren. 

1. In een ander browservenster aanmelden bij uw bedrijf XaitPorter site als beheerder.

1. Klik op **Admin**.

    ![Eenmalige aanmelding configureren](./media/xaitporter-tutorial/user1.png)

1. Selecteer **beheren Single Sign-On** uit de **Setup van System** vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/xaitporter-tutorial/user2.png)

1. In de **beheren van EENMALIGE aanmelding** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/xaitporter-tutorial/user3.png)

    a. Selecteer **verificatie met eenmalige aanmelding inschakelen**.

    b. In **instellingen van de identiteit** tekstvak plakken **App-Url voor federatieve metagegevens** die u hebt gekopieerd uit de Azure-portal en op **ophalen**.

    c. Selecteer **Autocreation van gebruikers inschakelen**.

    d. Klik op **OK**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/xaitporter-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/xaitporter-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/xaitporter-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/xaitporter-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-xaitporter-test-user"></a>Maak een testgebruiker XaitPorter

In deze sectie maakt u een gebruiker met de naam van Britta Simon in XaitPorter. Werken met [XaitPorter Client ondersteuningsteam](https://www.xait.com/support/) om toe te voegen de gebruikers in het XaitPorter-platform. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan XaitPorter.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan XaitPorter toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **XaitPorter**.

    ![De koppeling XaitPorter in de lijst met toepassingen](./media/xaitporter-tutorial/tutorial_xaitporter_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel XaitPorter in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing XaitPorter.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/xaitporter-tutorial/tutorial_general_01.png
[2]: ./media/xaitporter-tutorial/tutorial_general_02.png
[3]: ./media/xaitporter-tutorial/tutorial_general_03.png
[4]: ./media/xaitporter-tutorial/tutorial_general_04.png

[100]: ./media/xaitporter-tutorial/tutorial_general_100.png

[200]: ./media/xaitporter-tutorial/tutorial_general_200.png
[201]: ./media/xaitporter-tutorial/tutorial_general_201.png
[202]: ./media/xaitporter-tutorial/tutorial_general_202.png
[203]: ./media/xaitporter-tutorial/tutorial_general_203.png

