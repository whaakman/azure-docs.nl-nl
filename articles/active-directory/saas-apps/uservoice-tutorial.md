---
title: 'Zelfstudie: Azure Active Directory-integratie met UserVoice | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en UserVoice.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: f8a30ba7eee7653e4a098df706450c6a4328cce9
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54819064"
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Zelfstudie: Azure Active Directory-integratie met UserVoice

In deze zelfstudie leert u hoe u UserVoice integreren met Azure Active Directory (Azure AD).

UserVoice integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot UserVoice heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij UserVoice (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met UserVoice, moet u de volgende items:

- Een Azure AD-abonnement
- Een UserVoice eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. UserVoice uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-uservoice-from-the-gallery"></a>UserVoice uit de galerie toe te voegen
Voor het configureren van de integratie van UserVoice in Azure AD, moet u UserVoice uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen UserVoice uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **UserVoice**, selecteer **UserVoice** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![UserVoice in de lijst met resultaten](./media/uservoice-tutorial/tutorial_uservoice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met UserVoice op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de gebruiker equivalent in UserVoice wordt aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in UserVoice tot stand worden gebracht.

In UserVoice, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met UserVoice, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker UserVoice](#create-a-uservoice-test-user)**  : als u wilt een equivalent van Britta Simon in UserVoice die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw UserVoice-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met UserVoice, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **UserVoice** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/uservoice-tutorial/tutorial_uservoice_samlbase.png)

1. Op de **UserVoice-domein en URL's** sectie, voert u de volgende stappen uit:

    ![UserVoice-domein en URL's, eenmalige aanmelding informatie](./media/uservoice-tutorial/tutorial_uservoice_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<tenantname>.UserVoice.com`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<tenantname>.UserVoice.com`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [UserVoice-Client-ondersteuningsteam](https://www.uservoice.com/) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, Kopieer de **VINGERAFDRUK** waarde van het certificaat.

    ![De link om het certificaat te downloaden](./media/uservoice-tutorial/tutorial_uservoice_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/uservoice-tutorial/tutorial_general_400.png)

1. Op de **UserVoice configuratie** sectie, klikt u op **configureren UserVoice** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![UserVoice-configuratie](./media/uservoice-tutorial/tutorial_uservoice_configure.png) 

1. In een ander browservenster aanmelden bij uw UserVoice-site bedrijf als beheerder.

1. Klik in de werkbalk bovenaan op **instellingen**, en selecteer vervolgens **webportal** in het menu.
   
    ![De sectie instellingen-App aan](./media/uservoice-tutorial/ic777519.png "instellingen")

1. Op de **webportal** tabblad, in de **gebruikersverificatie** sectie, klikt u op **bewerken** openen de **gebruikersverificatie bewerken** dialoogvenster pagina.
   
    ![Webportal tabblad](./media/uservoice-tutorial/ic777520.png "webportal")

1. Op de **gebruikersverificatie bewerken** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Verificatie van de gebruiker bewerken](./media/uservoice-tutorial/ic777521.png "verificatie van de gebruiker bewerken")
   
    a. Klik op **Single Sign-On (SSO)**.
 
    b. Plakken de **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit de Azure portal in de **extern aanmelden SSO** tekstvak.

    c. Plakken de **afmelding URL** waarde die u hebt gekopieerd vanuit de Azure portal in de **SSO externe afmelding tekstvak**.
 
    d. Plak de **vingerafdruk** waarde die u hebt gekopieerd vanuit Azure portal in de **huidige certificaat SHA1-vingerafdruk** tekstvak.
    
    e. Klik op **verificatie-instellingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/uservoice-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/uservoice-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/uservoice-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/uservoice-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-uservoice-test-user"></a>Maak een testgebruiker UserVoice

Als u wilt dat Azure AD-gebruikers zich aanmelden met UserVoice, moeten ze worden ingericht in UserVoice. In het geval van UserVoice is inrichten een handmatige taak.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Voer de volgende stappen uit als u een gebruikersaccount wilt inrichten:
1. Meld u aan bij uw **UserVoice** tenant.

1. Ga naar **instellingen**.
   
    ![Settings](./media/uservoice-tutorial/ic777811.png "Settings")

1. Klik op **algemene**.

1. Klik op **Agents en machtigingen**.
   
    ![Agents en machtigingen](./media/uservoice-tutorial/ic777812.png "Agents en machtigingen")

1. Klik op **beheerders toevoegen**.
   
    ![Beheerders toevoegen](./media/uservoice-tutorial/ic777813.png "beheerders toevoegen")

1. Op de **uitnodigen beheerders** dialoogvenster, voer de volgende stappen uit:
   
    ![Beheerders uitnodigen](./media/uservoice-tutorial/ic777814.png "beheerders uitnodigen")
   
    a. Typ het e-mailadres van het account dat u wilt inrichten, en klik vervolgens op in het tekstvak e-mailberichten **toevoegen**.
   
    b. Klik op **uitnodigen**.

> [!NOTE]
> U kunt alle andere UserVoice gebruiker-account maken van hulpprogramma's of API's geleverd door UserVoice aan inrichten AAD-gebruikersaccounts.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan UserVoice.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon met UserVoice, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **UserVoice**.

    ![De UserVoice-koppeling in de lijst met toepassingen](./media/uservoice-tutorial/tutorial_uservoice_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de UserVoice-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw UserVoice-toepassing.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster. 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/uservoice-tutorial/tutorial_general_01.png
[2]: ./media/uservoice-tutorial/tutorial_general_02.png
[3]: ./media/uservoice-tutorial/tutorial_general_03.png
[4]: ./media/uservoice-tutorial/tutorial_general_04.png

[100]: ./media/uservoice-tutorial/tutorial_general_100.png

[200]: ./media/uservoice-tutorial/tutorial_general_200.png
[201]: ./media/uservoice-tutorial/tutorial_general_201.png
[202]: ./media/uservoice-tutorial/tutorial_general_202.png
[203]: ./media/uservoice-tutorial/tutorial_general_203.png

