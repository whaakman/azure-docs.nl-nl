---
title: 'Zelfstudie: Azure Active Directory-integratie met PagerDuty | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en PagerDuty.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 703bc11ec7b0de0e8b0cfe3b99b7516afe8cac0b
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54810020"
---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Zelfstudie: Azure Active Directory-integratie met PagerDuty

In deze zelfstudie leert u hoe u PagerDuty integreren met Azure Active Directory (Azure AD).

PagerDuty integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot PagerDuty heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij PagerDuty (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het Azure AD-integratie configureren met PagerDuty, moet u de volgende items:

- Een Azure AD-abonnement
- Een PagerDuty eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. PagerDuty uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-pagerduty-from-the-gallery"></a>PagerDuty uit de galerie toe te voegen
Voor het configureren van de integratie van PagerDuty in Azure AD, moet u PagerDuty uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen PagerDuty uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **PagerDuty**, selecteer **PagerDuty** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/pagerduty-tutorial/tutorial_pagerduty_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met PagerDuty op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in PagerDuty is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in PagerDuty tot stand worden gebracht.

In PagerDuty, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met PagerDuty, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker PagerDuty](#create-a-pagerduty-test-user)**  : als u wilt een equivalent van Britta Simon in PagerDuty die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing PagerDuty.

**Azure AD eenmalige aanmelding configureren met PagerDuty, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **PagerDuty** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/pagerduty-tutorial/tutorial_pagerduty_samlbase.png)

1. Op de **PagerDuty-domein en URL's** sectie, voert u de volgende stappen uit:

    ![PagerDuty-domein en URL's, eenmalige aanmelding informatie](./media/pagerduty-tutorial/tutorial_pagerduty_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<tenant-name>.pagerduty.com`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<tenant-name>.pagerduty.com`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [PagerDuty-Client-ondersteuningsteam](https://www.pagerduty.com/support/) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/pagerduty-tutorial/tutorial_pagerduty_certificate.png)

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/pagerduty-tutorial/tutorial_general_400.png)

1. Op de **PagerDuty configuratie** sectie, klikt u op **configureren PagerDuty** openen **aanmelding configureren** venster. Kopiëren de **afmelding URL's en SAML Single Sign-On Service** uit de **Naslaggids sectie.**

    ![PagerDuty-configuratie](./media/pagerduty-tutorial/tutorial_pagerduty_configure.png)

1. Meld u in een ander browservenster in uw bedrijf Pagerduty site als beheerder.

1. Klik in het menu aan de bovenkant op **Accountinstellingen**.

    ![Account Settings](./media/pagerduty-tutorial/ic778535.png "Account Settings")

1. Klik op **Eenmalige aanmelding**.

    ![Single sign-on](./media/pagerduty-tutorial/ic778536.png "Single sign-on")

1. Op de **Schakel eenmalige aanmelding (SSO)** pagina, voert u de volgende stappen uit:

    ![Eenmalige aanmelding inschakelen](./media/pagerduty-tutorial/ic778537.png "eenmalige aanmelding inschakelen")

    a. Open uw base-64 gecodeerde certificaat gedownload vanuit Azure portal in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **X.509-certificaat** tekstvak
  
    b. In de **aanmeldings-URL** tekstvak plakken **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.
  
    c. In de **afmeldings-URL van** tekstvak plakken **afmelding URL** die u hebt gekopieerd vanuit Azure portal.

    d. Selecteer **toestaan gebruikersnaam en wachtwoord aanmelden**.

    e. Selecteer **EXACTE vereist verificatie context vergelijking** selectievakje.

    f. Klik op **Wijzigingen opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](./media/pagerduty-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![De koppelingen Gebruikers en groepen en Alle gebruikers](./media/pagerduty-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![De knop toevoegen](./media/pagerduty-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het dialoogvenster Gebruiker](./media/pagerduty-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-pagerduty-test-user"></a>Maak een testgebruiker PagerDuty

Als u wilt dat Azure AD-gebruikers zich aanmelden bij PagerDuty, moeten ze worden ingericht voor PagerDuty.  
In het geval van PagerDuty is inrichten een handmatige taak.

>[!NOTE]
>U kunt elke andere Pagerduty gebruiker account hulpmiddelen voor het maken of API's geleverd door Pagerduty voor het inrichten van Azure Active Directory-gebruikersaccounts.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw **Pagerduty** tenant.

1. Klik in het menu aan de bovenkant op **gebruikers**.

1. Klik op **gebruikers toevoegen**.
   
    ![Gebruikers toevoegen](./media/pagerduty-tutorial/ic778539.png "gebruikers toevoegen")

1.  Op de **uitnodigen van uw team** dialoogvenster, voer de volgende stappen uit:
   
    ![Uw team uitnodigen](./media/pagerduty-tutorial/ic778540.png "uitnodigen van uw team")

    a. Type de **eerste en laatste naam** van gebruiker, zoals **Britta Simon**. 
   
    b. Voer **e** -adres van de gebruiker, zoals **brittasimon@contoso.com**.
   
    c. Klik op **toevoegen**, en klik vervolgens op **uitnodigingen verzenden**.
   
    >[!NOTE]
    >Alle toegevoegde gebruikers ontvangen een uitnodiging om een PagerDuty-account te maken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan PagerDuty.

![De de gebruikersrol toewijzen][200]

**Als u wilt Britta Simon aan PagerDuty toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **PagerDuty**.

    ![De PagerDuty-koppeling in de lijst met toepassingen](./media/pagerduty-tutorial/tutorial_pagerduty_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de tegel PagerDuty in de Panelyou toegang moet krijgen automatisch aangemeld bij uw toepassing PagerDuty.

Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/pagerduty-tutorial/tutorial_general_01.png
[2]: ./media/pagerduty-tutorial/tutorial_general_02.png
[3]: ./media/pagerduty-tutorial/tutorial_general_03.png
[4]: ./media/pagerduty-tutorial/tutorial_general_04.png

[100]: ./media/pagerduty-tutorial/tutorial_general_100.png

[200]: ./media/pagerduty-tutorial/tutorial_general_200.png
[201]: ./media/pagerduty-tutorial/tutorial_general_201.png
[202]: ./media/pagerduty-tutorial/tutorial_general_202.png
[203]: ./media/pagerduty-tutorial/tutorial_general_203.png
