---
title: 'Zelfstudie: Azure Active Directory-integratie met iQualify LMS | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en iQualify LMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 8a3caaff-dd8d-4afd-badf-a0fd60db3d2c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: jeedes
ms.openlocfilehash: 3ff5a833da5bbe99c5c6d1576b9775051b3ce07b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048086"
---
# <a name="tutorial-azure-active-directory-integration-with-iqualify-lms"></a>Zelfstudie: Azure Active Directory-integratie met iQualify LMS

In deze zelfstudie leert u hoe u iQualify LMS integreert met Azure Active Directory (Azure AD).

IQualify LMS integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot iQualify LMS heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij iQualify LMS (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met iQualify LMS, moet u de volgende items:

- Een Azure AD-abonnement
- Een iQualify LMS eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. IQualify LMS uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-iqualify-lms-from-the-gallery"></a>IQualify LMS uit de galerie toe te voegen
Voor het configureren van de integratie van iQualify LMS in Azure AD, moet u iQualify LMS uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen iQualify LMS uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **iQualify LMS**, selecteer **iQualify LMS** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![iQualify LMS in de lijst met resultaten](./media/iqualify-tutorial/tutorial_iqualify_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met iQualify die LMS op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de equivalente-gebruiker in iQualify LMS aan een gebruiker in Azure AD is. Met andere woorden, een koppeling de relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in iQualify LMS moet tot stand worden gebracht.

In iQualify LMS, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met iQualify LMS, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker van iQualify LMS](#create-an-iqualify-lms-test-user)**  : als u wilt een equivalent van Britta Simon in iQualify LMS die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw iQualify LMS-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met iQualify LMS, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **iQualify LMS** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/iqualify-tutorial/tutorial_iqualify_samlbase.png)

3. Op de **iQualify LMS-domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in de modus voor IDP gestart:

    ![informatie iQualify LMS-domein en URL's eenmalige aanmelding](./media/iqualify-tutorial/tutorial_iqualify_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: 
    | |
    |--|--|
    | Productie-omgeving: `https://<yourorg>.iqualify.com/`|
    | Testomgeving: `https://<yourorg>.iqualify.io`|
    
    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: 
    | |
    |--|--|
    | Productie-omgeving: `https://<yourorg>.iqualify.com/auth/saml2/callback` |
    | Testomgeving: `https://<yourorg>.iqualify.io/auth/saml2/callback` |

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![informatie iQualify LMS-domein en URL's eenmalige aanmelding](./media/iqualify-tutorial/tutorial_iqualify_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon:
    | |
    |--|--|
    | Productie-omgeving: `https://<yourorg>.iqualify.com/login` |
    | Testomgeving: `https://<yourorg>.iqualify.io/login` |
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [iQualify LMS-Client-ondersteuningsteam](https://www.iqualify.com) om deze waarden te verkrijgen. 

5. De iQualify LMS-toepassing wordt verwacht dat de Security Assertion Markup Language (SAML) asserties moet worden weergegeven in een specifieke indeling. De claims configureren en beheren van de waarden van de kenmerken in de **gebruikerskenmerken** sectie van de iQualify integratie toepassingspagina, zoals wordt weergegeven in de volgende schermafbeelding:
    
    ![Eenmalige aanmelding configureren](./media/iqualify-tutorial/atb.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster voert de volgende stappen uit voor elke rij in de onderstaande tabel wordt weergegeven:
    
    | Naam kenmerk | Waarde kenmerk |
    | --- | --- |    
    | e-mailen | User.userPrincipalName |
    | first_name | User.givenName |
    | last_name | User.surname |
    | person_id | "het kenmerk" | 

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/iqualify-tutorial/atb2.png)

    ![Eenmalige aanmelding configureren](./media/iqualify-tutorial/atb3.png)
    
    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **Ok**

    e. Herhaal stap "a" tot "d" voor de rijen van de volgende tabel. 

    > [!Note]
    > "A" tot "d" stappen herhalen voor de **person_id** kenmerk is **optioneel**

7. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base 64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/iqualify-tutorial/tutorial_iqualify_certificate.png) 

8. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/iqualify-tutorial/tutorial_general_400.png)
    
9. Op de **iQualify LMS-configuratie** sectie, klikt u op **iQualify LMS configureren** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![iQualify LMS-configuratie](./media/iqualify-tutorial/tutorial_iqualify_configure.png) 

10.  Open een nieuw browservenster en meld u aan uw omgeving iQualify als beheerder.

11. Nadat u bent aangemeld, klikt u op uw avatar in de rechterbovenhoek en klik op **"Accountinstellingen."**

    ![Accountinstellingen](./media/iqualify-tutorial/setting1.png) 
12. In het gebied van de instellingen voor account op het lint in het menu aan de linkerkant en klik op **"INTEGRATIES."**
    
    ![INTEGRATIES](./media/iqualify-tutorial/setting2.png)

13. Klik onder INTEGRATIES, op de **SAML** pictogram.

    ![SAML-pictogram](./media/iqualify-tutorial/setting3.png)

14. In de **SAML-verificatie-instellingen** dialoogvenster vak, voer de volgende stappen uit:

    ![SAML-verificatie-instellingen](./media/iqualify-tutorial/setting4.png)

    a. In de **URL voor SAML SINGLE SIGN-ON SERVICE** vak, plak de **SAML Sign‑On Service-URL met eenmalige** waarde opgehaald uit het Configuratievenster van Azure AD-toepassing.
    
    b. In de **URL voor SAML-AFMELDING** vak, plak de **Sign‑Out URL** waarde opgehaald uit het Configuratievenster van Azure AD-toepassing.
    
    c. Open het gedownloade certificaat-bestand in Kladblok, Kopieer de inhoud en plak deze in de **openbaar certificaat** vak.
    
    d. In **aanmelding OPDRACHTKNOP KNOPLABEL** Voer de naam in voor de knop moet worden weergegeven op de aanmeldingspagina.
    
    e. Klik op **OPSLAAN**.

    f. Klik op **UPDATE**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/iqualify-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/iqualify-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/iqualify-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/iqualify-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-iqualify-lms-test-user"></a>Maak een testgebruiker van iQualify LMS

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in iQualify. iQualify LMS ondersteunt just‑in‑time inrichten van gebruikers, die standaard is ingeschakeld.

Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in iQualify bestaat, wordt een nieuwe resourcegroep wordt gemaakt wanneer u probeert te krijgen tot iQualify LMS.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot iQualify LMS.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan iQualify LMS toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **iQualify LMS**.

    ![De iQualify LMS-koppeling in de lijst met toepassingen](./media/iqualify-tutorial/tutorial_iqualify_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u de iQualify LMS-tegel in het toegangsvenster klikt, krijgt u de aanmeldingspagina van uw iQualify LMS-toepassing. 

   ![aanmeldingspagina](./media/iqualify-tutorial/login.png) 

Klik op **Meld u aan met Azure AD** knop en u moet u automatisch aangemeld bij uw iQualify LMS-toepassing.

Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/iqualify-tutorial/tutorial_general_01.png
[2]: ./media/iqualify-tutorial/tutorial_general_02.png
[3]: ./media/iqualify-tutorial/tutorial_general_03.png
[4]: ./media/iqualify-tutorial/tutorial_general_04.png

[100]: ./media/iqualify-tutorial/tutorial_general_100.png

[200]: ./media/iqualify-tutorial/tutorial_general_200.png
[201]: ./media/iqualify-tutorial/tutorial_general_201.png
[202]: ./media/iqualify-tutorial/tutorial_general_202.png
[203]: ./media/iqualify-tutorial/tutorial_general_203.png

