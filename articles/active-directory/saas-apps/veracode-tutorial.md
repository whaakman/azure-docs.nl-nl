---
title: 'Zelfstudie: Azure Active Directory-integratie met Veracode | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Veracode.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 4fe78050-cb6d-4db9-96ec-58cc0779167f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e80a2e554d31ad85ddb1111f84a96e1814c3969f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56194645"
---
# <a name="tutorial-azure-active-directory-integration-with-veracode"></a>Zelfstudie: Azure Active Directory-integratie met Veracode

In deze zelfstudie leert u hoe u Veracode integreren met Azure Active Directory (Azure AD).

Veracode integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Veracode heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Veracode (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Veracode, moet u de volgende items:

- Een Azure AD-abonnement
- Een Veracode eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Veracode uit de galerie toevoegen
1. Azure AD-eenmalige aanmelding configureren en testen

## <a name="add-veracode-from-the-gallery"></a>Veracode uit de galerie toevoegen
Voor het configureren van de integratie van Veracode in Azure AD, moet u Veracode uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Veracode uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **Veracode**, selecteer **Veracode** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Veracode in de lijst met resultaten](./media/veracode-tutorial/tutorial_veracode_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Veracode op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Veracode is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Veracode tot stand worden gebracht.

In Veracode, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Veracode, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maak een testgebruiker Veracode](#create-a-veracode-test-user)**  : als u wilt een equivalent van Britta Simon in Veracode die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Veracode.

**Voor het configureren van Azure AD eenmalige aanmelding met Veracode, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Veracode** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/veracode-tutorial/tutorial_veracode_samlbase.png)

1. Op de **Veracode domein en URL's** sectie, de gebruiker beschikt niet over de stappen uitvoeren omdat de app is al vooraf geïntegreerd met Azure. 

    ![Eenmalige aanmelding configureren](./media/veracode-tutorial/tutorial_veracode_url.png)

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De link om het certificaat te downloaden](./media/veracode-tutorial/tutorial_veracode_certificate.png) 

1. Het doel van deze sectie is om een overzicht van hoe u gebruikers wilt verifiëren naar Veracode met hun account in Azure AD gebruikmaakt van Federatie op basis van het SAML-protocol te.

    Uw toepassing Veracode wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, die vereist dat u om toe te voegen van aangepast kenmerktoewijzingen aan uw **saml-token kenmerken** configuratie. In de volgende schermopname ziet u een voorbeeld hiervan.
    
    ![Kenmerken](./media/veracode-tutorial/tutorial_veracode_attr.png "Kenmerken")

1. Als u wilt toevoegen de vereiste kenmerktoewijzingen, moet u de volgende stappen uitvoeren:

    | Naam kenmerk | Waarde kenmerk |
    |--- |--- |
    | firstname |User.givenName |
    | lastname |User.surname |
    | e-mail |User.mail |
    
    a. Voor elke gegevensrij in de bovenstaande tabel, klikt u op **toevoegen gebruikerskenmerk**.
    
    ![Kenmerken](./media/veracode-tutorial/tutorial_veracode_addattr.png "Kenmerken")
    
    ![Kenmerken](./media/veracode-tutorial/tutorial_veracode_addattr1.png "Kenmerken")
    
    b. In de **kenmerknaam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. In de **kenmerkwaarde** textbox, selecteert u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

1. Klik op de knop **Save**.

    ![De knop voor enkelvoudige aanmelding configureren](./media/veracode-tutorial/tutorial_general_400.png)

1. Op de **Veracode configuratie** sectie, klikt u op **configureren Veracode** openen **aanmelding configureren** venster. Kopiëren de **SAML entiteit-ID** uit de **Naslaggids sectie.**

    ![Veracode configuratie](./media/veracode-tutorial/tutorial_veracode_configure.png) 

1. Meld u in een ander browservenster in uw bedrijf Veracode site als beheerder.

1. Klik in het menu aan de bovenkant op **instellingen**, en klik vervolgens op **Admin**.
   
    ![Beheer](./media/veracode-tutorial/ic802911.png "Beheer")

1. Klik op de **SAML** tabblad.

1. In de **organisatie SAML-instellingen** sectie, voert u de volgende stappen uit:
   
    ![Beheer](./media/veracode-tutorial/ic802912.png "Beheer")
   
    a.  In **verlener** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.
    
    b. Als u wilt uploaden uw gedownloade certificaat vanuit Azure portal, klikt u op **bestand kiezen**.
   
    c. Selecteer **inschakelen zelfregistratie**.

1. In de **Self registratie-instellingen** sectie, het uitvoeren van de volgende stappen uit en klik vervolgens op **opslaan**:
   
    ![Beheer](./media/veracode-tutorial/ic802913.png "Beheer")
   
    a. Als **nieuwe gebruiker-activering**, selecteer **geen activering vereist**.
   
    b. Als **gebruiker Gegevensupdates**, selecteer **voorkeur Veracode gebruikersgegevens**.
   
    c. Voor **Details van de SAML-kenmerk**, selecteert u het volgende:
      * **Gebruikersrollen**
      * **Beleid voor de beheerder**
      * **Reviewer**
      * **Beveiliging Lead**
      * **Executive**
      * **Inzender**
      * **Maker**
      * **Alle scannen typen**
      * **Teamlidmaatschappen**
      * **Standaardteam**

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/veracode-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/veracode-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/veracode-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/veracode-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-veracode-test-user"></a>Maak een testgebruiker Veracode
Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij Veracode, moeten ze worden ingericht voor Veracode. In het geval van Veracode is inrichten een geautomatiseerde taak. Er is geen actie-item voor u. Gebruikers worden automatisch gemaakt als dat nodig tijdens de eerste eenmalige aanmelding.

> [!NOTE]
> U kunt alle andere Veracode gebruiker-account maken van hulpprogramma's of API's geleverd door Veracode voor het inrichten van gebruikersaccounts van de Azure AD.
> 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Veracode.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Veracode toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Veracode**.

    ![De koppeling Veracode in de lijst met toepassingen](./media/veracode-tutorial/tutorial_veracode_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Veracode in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Veracode.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/veracode-tutorial/tutorial_general_01.png
[2]: ./media/veracode-tutorial/tutorial_general_02.png
[3]: ./media/veracode-tutorial/tutorial_general_03.png
[4]: ./media/veracode-tutorial/tutorial_general_04.png

[100]: ./media/veracode-tutorial/tutorial_general_100.png

[200]: ./media/veracode-tutorial/tutorial_general_200.png
[201]: ./media/veracode-tutorial/tutorial_general_201.png
[202]: ./media/veracode-tutorial/tutorial_general_202.png
[203]: ./media/veracode-tutorial/tutorial_general_203.png

