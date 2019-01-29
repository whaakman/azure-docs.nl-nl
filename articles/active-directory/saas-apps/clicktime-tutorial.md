---
title: 'Zelfstudie: Azure Active Directory-integratie met ClickTime | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ClickTime.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeedes
ms.openlocfilehash: 9cce73712bd3122916d18c3ed7f7744e30d5fa1a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55176519"
---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Zelfstudie: Azure Active Directory-integratie met ClickTime

In deze zelfstudie leert u hoe u ClickTime integreren met Azure Active Directory (Azure AD).

ClickTime integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ClickTime heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij ClickTime (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ClickTime, moet u de volgende items:

- Een Azure AD-abonnement
- Een ClickTime eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ClickTime uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-clicktime-from-the-gallery"></a>ClickTime uit de galerie toe te voegen
Voor het configureren van de integratie van ClickTime in Azure AD, moet u ClickTime uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ClickTime uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **ClickTime**, selecteer **ClickTime** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![ClickTime in de lijst met resultaten](./media/clicktime-tutorial/tutorial_clicktime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met ClickTime op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in ClickTime is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ClickTime tot stand worden gebracht.

In ClickTime, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met ClickTime, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker ClickTime](#create-a-clicktime-test-user)**  : als u wilt een equivalent van Britta Simon in ClickTime die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing ClickTime.

**Voor het configureren van Azure AD eenmalige aanmelding met ClickTime, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ClickTime** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/clicktime-tutorial/tutorial_clicktime_samlbase.png)

1. Op de **ClickTime domein en URL's** sectie, voert u de volgende stappen uit:

    ![ClickTime domein en URL's, eenmalige aanmelding informatie](./media/clicktime-tutorial/tutorial_clicktime_url.png)

    a. In de **id** tekstvak, een URL als: `https://app.clicktime.com/sp/`
    
    b. In de **antwoord-URL** tekstvak, een URL met behulp van de volgende patronen: 

    | |
    |--|
    | `https://app.clicktime.com/Login/` |
    | `https://app.clicktime.com/App/Login/Consume.aspx` |

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/clicktime-tutorial/tutorial_clicktime_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/clicktime-tutorial/tutorial_general_400.png)

1. Op de **ClickTime configuratie** sectie, klikt u op **configureren ClickTime** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![ClickTime configuratie](./media/clicktime-tutorial/tutorial_clicktime_configure.png) 

1. Meld u in een ander browservenster in uw bedrijf ClickTime site als beheerder.

1. Klik in de werkbalk bovenaan op **voorkeuren**, en klik vervolgens op **beveiligingsinstellingen**.

1. In de **Single Sign-On voorkeuren** configuratie sectie, voert u de volgende stappen uit:
   
    ![Beveiligingsinstellingen](./media/clicktime-tutorial/tic777280.png "beveiligingsinstellingen")
   
    a.  Selecteer **toestaan** aanmelden met eenmalige aanmelding (SSO) met **Azure AD**.
   
    b. In de **Identity Provider eindpunt** tekstvak plakken **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.
   
    c.  Open de **base-64 gecodeerde certificaat** gedownload vanuit Azure portal in **Kladblok**, Kopieer de inhoud en plak deze in de **X.509-certificaat** tekstvak.
   
    d.  Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/clicktime-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/clicktime-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.
 
    ![De knop toevoegen](./media/clicktime-tutorial/create_aaduser_03.png) 

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:
 
    ![Het dialoogvenster gebruiker](./media/clicktime-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="create-a-clicktime-test-user"></a>Maak een testgebruiker ClickTime

Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij ClickTime, moeten ze worden ingericht voor ClickTime.  
In het geval van ClickTime is inrichten een handmatige taak.

> [!NOTE]
> U kunt alle andere ClickTime gebruiker-account maken van hulpprogramma's of API's geleverd door ClickTime voor het inrichten van gebruikersaccounts van de Azure AD.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**
1. Meld u aan bij uw **ClickTime** tenant.
1. Klik in de werkbalk bovenaan op **bedrijf**, en klik vervolgens op **mensen**.
   
    ![People](./media/clicktime-tutorial/tic777282.png "People")
1. Klik op **persoon**.
   
    ![Add Person](./media/clicktime-tutorial/tic777283.png "Add Person")
1. In de sectie nieuwe persoon, moet u de volgende stappen uitvoeren:
   
    ![People](./media/clicktime-tutorial/tic777284.png "People")
   
    a.  In de **volledige naam** tekstvak, type volledige naam van gebruiker, zoals **Britta Simon**. 
  
    b.  In de **e-mailadres** tekstvak, typ het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.
       
    > [!NOTE]
    > Als u wilt, kunt u extra eigenschappen van het nieuwe object persoon instellen.
   
    c.  Klik op **Opslaan**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan ClickTime.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan ClickTime toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **ClickTime**.

    ![ClickTimne koppeling in de lijst met toepassingen](./media/clicktime-tutorial/tutorial_clicktime_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel ClickTime in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing ClickTime.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/clicktime-tutorial/tutorial_general_01.png
[2]: ./media/clicktime-tutorial/tutorial_general_02.png
[3]: ./media/clicktime-tutorial/tutorial_general_03.png
[4]: ./media/clicktime-tutorial/tutorial_general_04.png

[100]: ./media/clicktime-tutorial/tutorial_general_100.png

[200]: ./media/clicktime-tutorial/tutorial_general_200.png
[201]: ./media/clicktime-tutorial/tutorial_general_201.png
[202]: ./media/clicktime-tutorial/tutorial_general_202.png
[203]: ./media/clicktime-tutorial/tutorial_general_203.png

