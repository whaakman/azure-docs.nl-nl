---
title: 'Zelfstudie: Azure Active Directory-integratie met Cisco Webex | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Cisco Webex.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2018
ms.author: jeedes
ms.openlocfilehash: 73e20afdcacec76482f8ebf01bf2cef2105912a6
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005513"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Zelfstudie: Azure Active Directory-integratie met Cisco Webex

In deze zelfstudie leert u hoe u Cisco Webex integreren met Azure Active Directory (Azure AD).

Cisco Webex integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Cisco Webex heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Cisco Webex met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Cisco Webex, moet u de volgende items:

- Een Azure AD-abonnement
- Cisco Webex één teken op ingeschakeld abonnement

> [!NOTE]
> U kunt beter geen een productie-omgeving voor het testen van de stappen in deze zelfstudie.

Als u wilt testen van de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productie-omgeving niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Cisco Webex uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-cisco-webex-from-the-gallery"></a>Cisco Webex uit de galerie toevoegen
Voor het configureren van de integratie van Cisco Webex in Azure AD, moet u Cisco Webex uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Cisco Webex uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Cisco Webex**. 

5. Selecteer **Cisco Webex** vanuit het deelvenster resultaten. Selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![Cisco Webex in de lijst met resultaten](./media/cisco-webex-tutorial/tutorial_ciscowebex_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Cisco Webex op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD te weten wie de gebruiker equivalent in de Webex Cisco is aan een gebruiker in Azure AD. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Cisco Webex vast te stellen.

Geef de waarde in de Webex Cisco, **gebruikersnaam** dezelfde waarde als **gebruikersnaam** in Azure AD. Nu hebt u de koppeling tussen de twee gebruikers gemaakt. 

Als u wilt configureren en Azure AD eenmalige aanmelding met Cisco Webex testen, voert u de volgende bouwstenen:

1. [Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on) zodat uw gebruikers deze functie wilt gebruiken.
2. [Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maak een testgebruiker Cisco Webex](#create-a-cisco-webex-test-user) hebben een equivalent van Britta Simon in Cisco Webex die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Eenmalige aanmelding testen](#test-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Cisco Webex-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Cisco Webex, voert u de volgende stappen uit:**

1. In de Azure-portal op de **Cisco Webex** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Voor het inschakelen van eenmalige aanmelding in de **eenmalige aanmelding** in het dialoogvenster de **modus** vervolgkeuzelijst, selecteer **SAML gebaseerde aanmelding**.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/cisco-webex-tutorial/tutorial_ciscowebex_samlbase.png)

3. In een ander browservenster aanmelden bij uw bedrijf Cisco Webex site als beheerder.

4. Klik op **instellingen** vanaf de linkerkant van het menu.

    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial_cisco_webex_10.png)

5. Op de instellingenpagina Schuif omlaag onder de **verificatie** sectie, klikt u op **wijzigen**.

    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial_cisco_webex_14.png)

6. Selecteer **integreren van een 3rd party-id-provider. (Geavanceerd) ** en Ga naar het volgende scherm.

    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial_cisco_webex_15.png)

7. Op de **Metagegevensmap exporteren** pagina, klikt u op **metagegevensbestand downloaden** voor het downloaden van het bestand met metagegevens.

    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial_cisco_webex_16.png)

8. In de Azure-portal, onder de **Cisco Webex-domein en URL's** gedeelte in en upload het gedownloade **Service Provider-bestand met metagegevens** en configureren van de toepassing door de volgende stappen uit:

    a. Klik op **metagegevensbestand uploaden**.

    ![Cisco Webex-domein en URL's, eenmalige aanmelding informatie](./media/cisco-webex-tutorial/tutorial_ciscowebex_upload.png)

    b. Klik op **map logo** voor het selecteren van het bestand met metagegevens en klikt u op **uploaden**.

    ![Cisco Webex-domein en URL's, eenmalige aanmelding informatie](./media/cisco-webex-tutorial/tutorial_ciscowebex_uploadconfig.png)

    c. Na voltooiing van het uploaden van **Service Provider-bestand met metagegevens** de **id** en **antwoord-URL** waarden ophalen automatisch ingevuld **Cisco Webex Domein en URL's** sectie tekstvak, zoals hieronder weergegeven:

    ![Cisco Webex-domein en URL's, eenmalige aanmelding informatie](./media/cisco-webex-tutorial/tutorial_ciscowebex_url.png)

    d. In de **aanmeldings-URL** vak, typ een URL met het volgende patroon: `https://<SUBDOMAIN>.webex.com/`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL. Neem contact op met [Cisco Webex-Client-ondersteuningsteam](https://www.webex.co.in/support/support-overview.html) om deze waarden te verkrijgen.

9. Cisco Webex-toepassing wordt verwacht dat de SAML-asserties ondertekend naar specifieke kenmerken bevatten. Configureer de volgende kenmerken voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial_ciscowebex_07.png) 

10. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    |  Naam kenmerk  | Waarde kenmerk |
    | --------------- | -------------------- |    
    |   Voornaam    | User.givenName |
    |   Achternaam    | User.surname |
    |   UID    | User.mail |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

11. Op de **SAML-handtekeningcertificaat** sectie, selecteer **Metadata XML**, en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/cisco-webex-tutorial/tutorial_ciscowebex_certificate.png) 

12. Selecteer **Opslaan**.

    ![Configureer de eenmalige aanmelding knop Opslaan](./media/cisco-webex-tutorial/tutorial_general_400.png)
    
13. Gebruik de optie bestand browser om te zoeken en de Azure AD-metagegevensbestand uploaden op de pagina Cisco Webex bedrijf site beheerder. Selecteer **certificaat dat is ondertekend door een certificeringsinstantie in de metagegevens (veiliger) vereisen** en Ga naar het volgende scherm. 

    ![Eenmalige aanmelding configureren](./media/cisco-webex-tutorial/tutorial_cisco_webex_11.png)

14. Selecteer **SSO-testverbinding**, en wanneer er wordt een nieuw browsertabblad geopend, verifieert u met Azure AD met het aanmelden.

15. Ga terug naar de **Cisco samenwerking Cloudbeheer** browsertabblad. Als de test voltooid is, selecteert u **deze test is geslaagd. Inschakelen van Single Sign-On optie** en klikt u op **opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Selecteer in de Azure portal, in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/cisco-webex-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/cisco-webex-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, selecteer **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/cisco-webex-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/cisco-webex-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-cisco-webex-test-user"></a>Maak een testgebruiker Cisco Webex

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Cisco Webex. Cisco Webex biedt ondersteuning voor just-in-time inrichting en automatisch gebruikers inrichten, dit standaard ingeschakeld is. Meer informatie vindt u [hier](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial) voor het automatisch inrichten van gebruikers configureren.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u de gebruiker Britta Simon Azure eenmalige aanmelding gebruiken ze door toegang te verlenen aan Cisco Webex.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan Cisco Webex toewijst, moet u de volgende stappen uitvoeren:**

1. Open de toepassingen in de Azure-portal. Volgende, gaat u naar de directoryweergave, en vervolgens naar **bedrijfstoepassingen**.  

2. Selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

3. Selecteer in de lijst met toepassingen, **Cisco Webex**.

    ![De Cisco Webex-koppeling in de lijst met toepassingen](./media/cisco-webex-tutorial/tutorial_ciscowebex_app.png)  

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer de **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** in het dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** in het dialoogvenster, selecteer **Britta Simon** in de **gebruikers** lijst.

6. In de **gebruikers en groepen** in het dialoogvenster, klikt u op de **Selecteer** knop.

7. Selecteer de **toewijzen** knop in de **toevoegen toewijzing** in het dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de Cisco Webex-tegel in het toegangsvenster selecteert, ophalen u automatisch aangemeld bij uw Cisco Webex-toepassing.

Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/cisco-webex-tutorial/tutorial_general_01.png
[2]: ./media/cisco-webex-tutorial/tutorial_general_02.png
[3]: ./media/cisco-webex-tutorial/tutorial_general_03.png
[4]: ./media/cisco-webex-tutorial/tutorial_general_04.png

[100]: ./media/cisco-webex-tutorial/tutorial_general_100.png

[200]: ./media/cisco-webex-tutorial/tutorial_general_200.png
[201]: ./media/cisco-webex-tutorial/tutorial_general_201.png
[202]: ./media/cisco-webex-tutorial/tutorial_general_202.png
[203]: ./media/cisco-webex-tutorial/tutorial_general_203.png

