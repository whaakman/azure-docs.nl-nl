---
title: 'Zelfstudie: Azure Active Directory-integratie met StatusPage | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en StatusPage.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: f6ee8bb3-df43-4c0d-bf84-89f18deac4b9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: c3532ff5422ae35aa6bb079fcedf3a2a90d45349
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55181568"
---
# <a name="tutorial-azure-active-directory-integration-with-statuspage"></a>Zelfstudie: Azure Active Directory-integratie met StatusPage

In deze zelfstudie leert u hoe u StatusPage integreren met Azure Active Directory (Azure AD).

StatusPage integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot StatusPage heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij StatusPage (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met StatusPage, moet u de volgende items:

- Een Azure AD-abonnement
- Een StatusPage eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. StatusPage uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-statuspage-from-the-gallery"></a>StatusPage uit de galerie toe te voegen
Voor het configureren van de integratie van StatusPage in Azure AD, moet u StatusPage uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen StatusPage uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **StatusPage**.

    ![Het maken van een Azure AD-testgebruiker](./media/statuspage-tutorial/tutorial_statuspage_search.png)

1. Selecteer in het deelvenster resultaten **StatusPage**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/statuspage-tutorial/tutorial_statuspage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met StatusPage op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in StatusPage is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in StatusPage tot stand worden gebracht.

In StatusPage, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met StatusPage, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker StatusPage](#creating-a-statuspage-test-user)**  : als u wilt een equivalent van Britta Simon in StatusPage die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing StatusPage.

**Voor het configureren van Azure AD eenmalige aanmelding met StatusPage, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **StatusPage** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_statuspage_samlbase.png)

1. Op de **StatusPage domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_statuspage_url.png)

    a. In het tekstvak **Id** typt u een URL met het volgende patroon:
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/` |
    | `https://<subdomain>.statuspage.io/` |

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie: 
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/sso/saml/consume` |
    | `https://<subdomain>.statuspage.io/sso/saml/consume` |

    > [!NOTE]
    > Neem contact op met het ondersteuningsteam StatusPage op [ SupportTeam@statuspage.io ](mailto:SupportTeam@statuspage.io)om aan te vragen van metagegevens die nodig zijn voor het configureren van eenmalige aanmelding. 
    >
    >a. Kopieer de uitgeverwaarde van de metagegevens en plak deze in de **id** tekstvak.
    >
    >b. De antwoord-URL kopiëren van de metagegevens en plak deze in de **antwoord-URL** tekstvak.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_statuspage_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_general_400.png)

1. Op de **StatusPage configuratie** sectie, klikt u op **configureren StatusPage** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_statuspage_configure.png) 

1. In een ander browservenster, meld u aan bij uw bedrijf StatusPage site als beheerder.

1. Klik in de belangrijkste werkbalk **Account beheren**.
   
    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_statuspage_06.png) 

1. Klik op de **Single Sign-on** tabblad. 
   
    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_statuspage_07.png) 

1. Voer de volgende stappen uit op de pagina instellingen voor eenmalige aanmelding:
   
    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_statuspage_08.png) 

    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_statuspage_09.png) 
 
    a. In de **doel-URL voor eenmalige aanmelding** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal.

    b. Open het gedownloade certificaat in Kladblok, Kopieer de inhoud en plak deze in de **certificaat** tekstvak. 

    c. Klik op **configuratie opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/statuspage-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/statuspage-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/statuspage-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/statuspage-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-statuspage-test-user"></a>Het maken van een testgebruiker StatusPage

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in StatusPage.

StatusPage biedt ondersteuning voor just-in-time inrichting. U hebt al ingeschakeld in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).

**Voor het maken van een gebruiker met de naam van Britta Simon in StatusPage, moet u de volgende stappen uitvoeren:**

1. Aanmelding bij uw bedrijf StatusPage site als beheerder.

1. Klik in het menu aan de bovenkant op **Account beheren**.

    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_statuspage_06.png)

1. Klik op de **teamleden** tabblad. 
   
    ![Het maken van een Azure AD-testgebruiker](./media/statuspage-tutorial/tutorial_statuspage_10.png) 

1. Klik op **TEAMLID toevoegen**. 
   
    ![Het maken van een Azure AD-testgebruiker](./media/statuspage-tutorial/tutorial_statuspage_11.png) 

1. Type de **e-mailadres**, **voornaam**, en **achternaam** van een geldige gebruiker die u inrichten in de bijbehorende tekstvakken wilt. 
   
    ![Het maken van een Azure AD-testgebruiker](./media/statuspage-tutorial/tutorial_statuspage_12.png) 

1. Als **rol**, kiest u **Clientbeheerder**.

1. Klik op **maken ACCOUNT**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan StatusPage.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan StatusPage toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **StatusPage**.

    ![Eenmalige aanmelding configureren](./media/statuspage-tutorial/tutorial_statuspage_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.

Wanneer u op de tegel StatusPage in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing StatusPage.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/statuspage-tutorial/tutorial_general_01.png
[2]: ./media/statuspage-tutorial/tutorial_general_02.png
[3]: ./media/statuspage-tutorial/tutorial_general_03.png
[4]: ./media/statuspage-tutorial/tutorial_general_04.png

[100]: ./media/statuspage-tutorial/tutorial_general_100.png

[200]: ./media/statuspage-tutorial/tutorial_general_200.png
[201]: ./media/statuspage-tutorial/tutorial_general_201.png
[202]: ./media/statuspage-tutorial/tutorial_general_202.png
[203]: ./media/statuspage-tutorial/tutorial_general_203.png

