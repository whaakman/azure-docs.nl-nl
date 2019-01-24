---
title: 'Zelfstudie: Azure Active Directory-integratie met Thoughtworks Singleplayer | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Thoughtworks Singleplayer.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 7652f0179725dbe04c3245028491fae5f8c4dac7
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54813250"
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Zelfstudie: Azure Active Directory-integratie met Thoughtworks Singleplayer

In deze zelfstudie leert u hoe u Thoughtworks Singleplayer integreert met Azure Active Directory (Azure AD).

Thoughtworks Singleplayer integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Thoughtworks Singleplayer heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Thoughtworks Singleplayer (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Thoughtworks Singleplayer, moet u de volgende items:

- Een Azure AD-abonnement
- Een Thoughtworks Singleplayer eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen Thoughtworks Singleplayer uit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-thoughtworks-mingle-from-the-gallery"></a>Toe te voegen Thoughtworks Singleplayer uit de galerie
Voor het configureren van de integratie van Thoughtworks Singleplayer in Azure AD, moet u Thoughtworks Singleplayer toevoegen uit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Thoughtworks Singleplayer uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Thoughtworks Singleplayer**, selecteer **Thoughtworks Singleplayer** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Thoughtworks Singleplayer in de lijst met resultaten](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Thoughtworks Singleplayer op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Thoughtworks Singleplayer is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Thoughtworks Singleplayer tot stand worden gebracht.

In het Thoughtworks Singleplayer, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Thoughtworks Singleplayer, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Thoughtworks Singleplayer](#create-a-thoughtworks-mingle-test-user)**  : als u wilt een equivalent van Britta Simon in Thoughtworks Singleplayer die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Thoughtworks Singleplayer.

**Voor het configureren van Azure AD eenmalige aanmelding met Thoughtworks Singleplayer, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Thoughtworks Singleplayer** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

1. Op de **Thoughtworks Singleplayer domein en URL's** sectie, voert u de volgende stappen uit:

    ![Thoughtworks Singleplayer domein en URL's, eenmalige aanmelding informatie](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<companyname>.mingle.thoughtworks.com`

    > [!NOTE] 
    > De waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Thoughtworks Singleplayer Client ondersteuningsteam](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) om de waarde. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/thoughtworks-mingle-tutorial/tutorial_general_400.png)

1. Meld u aan bij uw **Thoughtworks Singleplayer** bedrijf site als administrator.

1. Klik op de **Admin** tabblad en klik vervolgens op **SSO-configuratie**.
   
    ![Tabblad beheer](./media/thoughtworks-mingle-tutorial/ic785157.png "SSO-configuratie")

1. In de **SSO Config** sectie, voert u de volgende stappen uit:
   
    ![SSO Config](./media/thoughtworks-mingle-tutorial/ic785158.png "SSO Config")
    
    a. Als u wilt het bestand met metagegevens uploaden, klikt u op **bestand kiezen**. 

    b. Klik op **Wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](./media/thoughtworks-mingle-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![De koppelingen Gebruikers en groepen en Alle gebruikers](./media/thoughtworks-mingle-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![De knop toevoegen](./media/thoughtworks-mingle-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het dialoogvenster Gebruiker](./media/thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-thoughtworks-mingle-test-user"></a>Maak een testgebruiker Thoughtworks Singleplayer

Voor Azure AD-gebruikers kunnen zich aanmelden, moeten ze worden ingericht voor de toepassing Thoughtworks Singleplayer met behulp van hun Azure Active Directory-gebruikersnamen. In het geval van Thoughtworks Singleplayer is inrichten een handmatige taak.

**Voer de volgende stappen uit om de gebruikersinrichting te configureren:**

1. Meld u aan bij uw bedrijf Thoughtworks Singleplayer site aan als beheerder.

1. Klik op **profiel**.
   
    ![Uw eerste Project](./media/thoughtworks-mingle-tutorial/ic785160.png "uw eerste Project")

1. Klik op de **Admin** tabblad en klik vervolgens op **gebruikers**.
   
    ![Gebruikers](./media/thoughtworks-mingle-tutorial/ic785161.png "Gebruikers")

1. Klik op **New User**.
   
    ![New User](./media/thoughtworks-mingle-tutorial/ic785162.png "New User")

1. Op de **nieuwe gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Dialoogvenster Nieuwe gebruiker](./media/thoughtworks-mingle-tutorial/ic785163.png "nieuwe gebruiker")  
 
    a. Type de **aanmeldingsnaam**, **weergavenaam**, **kiezen wachtwoord**, **wachtwoord bevestigen** van een geldige Azure AD-account die u inrichten wilt in de bijbehorende tekstvakken. 

    b. Als **gebruikerstype**, selecteer **volledige gebruiker**.

    c. Klik op **maken van dit profiel**.

>[!NOTE]
>U kunt alle andere Thoughtworks Singleplayer gebruiker-account maken van hulpprogramma's of API's geleverd door Thoughtworks Singleplayer aan inrichten AAD-gebruikersaccounts.
> 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Thoughtworks Singleplayer.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Thoughtworks Singleplayer toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Thoughtworks Singleplayer**.

    ![De koppeling Thoughtworks Singleplayer in de lijst met toepassingen](./media/thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.

Wanneer u op de tegel Thoughtworks Singleplayer in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Thoughtworks Singleplayer.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/thoughtworks-mingle-tutorial/tutorial_general_203.png

