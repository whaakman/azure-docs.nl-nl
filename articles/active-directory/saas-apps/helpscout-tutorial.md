---
title: 'Zelfstudie: Azure Active Directory-integratie met Scout Help | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Help Scout.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3f999ff396a5573e6928fd8a25e1bb634f3615c0
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56179278"
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Zelfstudie: Azure Active Directory-integratie met Scout Help

In deze zelfstudie leert u hoe u Scout te integreren met Azure Active Directory (Azure AD).

Scout te integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Help Scout heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Scout Help (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Help Scout, moet u de volgende items:

- Een Azure AD-abonnement
- Een Scout Help eenmalige aanmelding ingeschakeld abonnement

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Scout Help uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-help-scout-from-the-gallery"></a>Scout Help uit de galerie toe te voegen
Voor het configureren van de integratie van Help Scout in Azure AD, moet u helpen Scout uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt helpen Scout toevoegen uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Scout Help**, selecteer **Help Scout** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Scout Help in de lijst met resultaten](./media/helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Help Scout op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de Help Scout is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de Help Scout tot stand worden gebracht.

Help Scout e-mailadressen gebruikt voor aanmeldingen, dus voor het maken van de relatie van de koppeling, gebruiken dezelfde **e-mailadres** als **gebruikersnaam** in Azure AD.

Als u wilt configureren en Azure AD eenmalige aanmelding met Scout te testen, moet u uitvoeren van de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Help Scout](#create-a-help-scout-test-user)**  : als u wilt een equivalent van Britta Simon in Help Scout, dat is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en eenmalige aanmelding in uw toepassing Scout te configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Help Scout, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Help Scout** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/helpscout-tutorial/tutorial_helpscout_samlbase.png)

1. Op de **Help Scout domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Scout-domein en URL's één aanmeldings-informatie voor de](./media/helpscout-tutorial/tutorial_helpscout_url.png)

    a. **Id** is de **"Doelgroep URI (serviceprovider entiteits-ID)"** van Help Scout, begint met `urn:`

    b. **Antwoord-URL** is de **"Na back-URL (URL van de Bevestigingsconsumerservice)"** van Help Scout, begint met `https://` 

    > [!NOTE] 
    > De waarden in deze URL's zijn alleen ter demonstratie. U moet deze waarden van de werkelijke antwoord-URL en -ID bijwerken. U krijgt deze waarden uit de **Single Sign-On** tabblad onder de sectie verificatie, die later in de zelfstudie wordt uitgelegd.

1. Als u wilt configureren van de toepassing in **SP** gestart modus selectievakje **geavanceerde URL-instellingen weergeven** en voer de volgende stap:

    ![Scout-domein en URL's één aanmeldings-informatie voor de](./media/helpscout-tutorial/tutorial_helpscout_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL als: `https://secure.helpscout.net/members/login/`
     
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/helpscout-tutorial/tutorial_helpscout_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/helpscout-tutorial/tutorial_general_400.png)


1. Op de **Help Scout configuratie** sectie, klikt u op **Help Scout configureren** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie**.

    ![Eenmalige aanmelding configureren](./media/helpscout-tutorial/config.png) 

1. In een ander browservenster aanmelden bij uw bedrijf helpen Scout site als beheerder.

1. Nadat u bent aangemeld bij klikken op **'Beheren'** van het bovenste menu en selecteer vervolgens **'Bedrijf'** in de vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/helpscout-tutorial/settings1.png) 
 
1. Selecteer **"Authentication"** in het menu links. 

    ![Eenmalige aanmelding configureren](./media/helpscout-tutorial/settings2.png) 

1. Dit gaat u naar de sectie voor SAML-instellingen en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/helpscout-tutorial/settings3.png) 
 
    a. Kopieer de **na back-URL (URL van de Bevestigingsconsumerservice)** waarde en plak de waarde in de **antwoord-URL** vak in de Azure-portal onder Help Scout **domein en URL's** sectie.
    
    b. Kopieer de **doelgroep-URI (Service Provider entiteits-ID)** waarde en plak de waarde in de **id** vak in de Azure-portal onder Help Scout **domein en URL's** sectie.

1. In-/ uitschakelen **SAML inschakelen** op en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/helpscout-tutorial/settings4.png) 
 
    a. In **-URL met eenmalige aanmelding** tekstvak, plak de waarde van **Single Sign-On Service URL**, die u hebt gekopieerd vanuit Azure portal.
    
    b. Klik op **certificaat uploaden** het uploaden van de **Certificate(Base64)** gedownload vanuit Azure portal.

    c. Voer uw organisatie e-mail domein(en) bijvoorbeeld - `contoso.com` in de **e-maildomeinen** tekstvak. U kunt meerdere domeinen scheiden met een komma. Op elk gewenst moment een helpen Scout gebruiker of beheerder van dat specifieke domein ingevoerd op de [Help Scout-aanmeldingspagina](https://secure.helpscout.net/members/login/) met id-Provider om te verifiëren met hun referenties worden doorgestuurd.

    d. Ten slotte kunt u schakelen **Force SAML-aanmelding** als u wilt dat gebruikers zich alleen aanmelden om te helpen Scout via via deze methode. Als u nog steeds laat de optie voor ze zich aanmelden met hun referenties helpen Scout wilt, kunt u deze integratie met lookout MTP laten. Zelfs als deze optie is ingeschakeld, zich eigenaar van een Account altijd aanmelden bij Scout helpen met het wachtwoord van hun account.

    e. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/helpscout-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/helpscout-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/helpscout-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/helpscout-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-help-scout-test-user"></a>Maak een testgebruiker Scout Help

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Help Scout. Help Scout biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in de Help Scout bestaat, wordt een nieuw wordt gemaakt wanneer u probeert toegang tot Help Scout.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen om te helpen Scout.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon om te helpen Scout, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Help Scout**.

    ![De Scout Help-koppeling in de lijst met toepassingen](./media/helpscout-tutorial/tutorial_helpscout_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Help Scout in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing te Scout.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/helpscout-tutorial/tutorial_general_01.png
[2]: ./media/helpscout-tutorial/tutorial_general_02.png
[3]: ./media/helpscout-tutorial/tutorial_general_03.png
[4]: ./media/helpscout-tutorial/tutorial_general_04.png

[100]: ./media/helpscout-tutorial/tutorial_general_100.png

[200]: ./media/helpscout-tutorial/tutorial_general_200.png
[201]: ./media/helpscout-tutorial/tutorial_general_201.png
[202]: ./media/helpscout-tutorial/tutorial_general_202.png
[203]: ./media/helpscout-tutorial/tutorial_general_203.png

