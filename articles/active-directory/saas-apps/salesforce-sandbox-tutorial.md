---
title: 'Zelfstudie: Azure Active Directory-integratie met Salesforce Sandbox | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Salesforce Sandbox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2019
ms.author: jeedes
ms.openlocfilehash: b2940f3eee3112fe1c6d57cc92157c573ecad109
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65904276"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Zelfstudie: Azure Active Directory-integratie met Salesforce-Sandbox

In deze zelfstudie leert u hoe u Salesforce Sandbox integreren met Azure Active Directory (Azure AD).

Sandboxes bieden u de mogelijkheid om te maken van meerdere kopieën van uw organisatie in afzonderlijke omgevingen voor verschillende doeleinden, zoals ontwikkeling, testen en training, zonder verlies van gegevens en toepassingen in uw Salesforce-productie de organisatie.
Zie voor meer informatie, [sandbox-overzicht](https://help.salesforce.com/articleView?id=create_test_instance.htm&language=en_us&type=5).

Salesforce-Sandbox integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot de Salesforce-Sandbox heeft.
* U kunt uw gebruikers worden automatisch aangemeld Salesforce-Sandbox (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Salesforce Sandbox, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/)
* SalesForce-Sandbox eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Salesforce-Sandbox **SP en IDP** gestart door SSO
* Biedt ondersteuning voor Salesforce-Sandbox **Just In Time** inrichten van gebruikers
* Biedt ondersteuning voor Salesforce-Sandbox [ **automatisch** inrichten van gebruikers](salesforce-sandbox-provisioning-tutorial.md)

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Salesforce-Sandbox uit de galerie toe te voegen

Voor het configureren van de integratie van Salesforce Sandbox in Azure AD, moet u Salesforce-Sandbox uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Salesforce-Sandbox uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Salesforce Sandbox**, selecteer **Salesforce Sandbox** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SalesForce-Sandbox in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Salesforce-Sandbox op basis van een testgebruiker met de naam **Britta Simon**.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Salesforce Sandbox is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Salesforce Sandbox tot stand worden gebracht.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Salesforce Sandbox, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureren van Salesforce Sandbox Single Sign-On](#configure-salesforce-sandbox-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Salesforce-Sandbox testgebruiker maken](#create-salesforce-sandbox-test-user)**  : als u wilt een equivalent van Britta Simon in Salesforce-Sandbox die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Salesforce Sandbox, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Salesforce Sandbox** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Op de **SAML-basisconfiguratie** sectie, hebt u **Service Provider-bestand met metagegevens** en wilt configureren **IDP** gestart modus de volgende stappen uitvoeren:

    a. Klik op **Metagegevensbestand uploaden**.

    ![Metagegevensbestand uploaden](common/upload-metadata.png)

    b. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.

    ![Metagegevensbestand kiezen](common/browse-upload-metadata.png)

    > [!NOTE]
    > U ontvangt de serviceprovider-bestand met metagegevens van de Salesforce-Sandbox-beheerportal die later in de zelfstudie wordt uitgelegd.

    c. Nadat het bestand met metagegevens is geüpload, de **antwoord-URL** waarde krijgt automatisch ingevuld **antwoord-URL** tekstvak.

    ![image](common/both-replyurl.png)

    > [!Note]
    > Als de **antwoord-URL** waarde krijg niet automatisch polulated, vul vervolgens de waarde handmatig uit volgens uw behoeften.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om de **metagegevens-XML** te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. Op de **instellen van Salesforce Sandbox** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-salesforce-sandbox-single-sign-on"></a>Salesforce-Sandbox Single Sign-On configureren

1. Open een nieuw tabblad in uw browser en meld u aan uw Salesforce-Sandbox-beheerdersaccount.

2. Klik op **Instellen** onder het **instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure1.png)

3. Schuif omlaag naar de **instellingen** in het navigatiedeelvenster links, klikt u op **identiteit** om uit te breiden de gerelateerde sectie. Klik vervolgens op **Instellingen voor eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

4. Op de pagina **Instellingen voor eenmalige aanmelding** klikt u op de knop **Bewerken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure3.png)

5. Selecteer **SAML ingeschakeld** en klik vervolgens op **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

6. Als u uw SAML-instellingen voor eenmalige aanmelding wilt configureren, klikt u op **Nieuw op basis van het bestand met metagegevens**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

7. Klik op **Bestand kiezen** om het XML-bestand met metagegevens te uploaden dat u hebt gedownload uit Azure Portal. Klik dan op **Maken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/xmlchoose.png)

8. Op de pagina **SAML-instellingen voor eenmalige aanmelding** worden de velden automatisch ingevuld. Klik op Opslaan.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/salesforcexml.png)

9. Op de **instellingen voor eenmalige aanmelding** pagina, klikt u op de **metagegevens downloaden** knop voor het downloaden van het metagegevensbestand van de service-provider. Gebruik dit bestand in de **SAML-basisconfiguratie** sectie in Azure portal voor het configureren van de vereiste URL's, zoals hierboven is uitgelegd.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure4.png)

10. Als u wilt configureren van de toepassing in **SP** gestart modus, zijn de vereisten voor die:

    a. U moet een geverifieerd domein hebben.

    b. U wilt configureren en inschakelen van uw Salesforce-Sandbox-domein, de stappen voor deze verderop in deze zelfstudie worden beschreven.

    c. In de Azure-portal op de **SAML-basisconfiguratie** sectie, klikt u op **extra URL's instellen** en voer de volgende stap:
  
    ![SalesForce sandbox-domein en URL's, eenmalige aanmelding informatie](common/both-signonurl.png)

    In de **aanmeldings-URL** tekstvak typt u de waarde met behulp van het volgende patroon: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    > [!NOTE]
    > Deze waarde moet worden gekopieerd van de Salesforce-Sandbox-portal nadat u het domein hebt ingeschakeld.

11. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **federatieve metagegevens-XML** en sla het xml-bestand op uw computer.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

12. Open een nieuw tabblad in uw browser en meld u aan uw Salesforce-Sandbox-beheerdersaccount.

13. Klik op **Instellen** onder het **instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure1.png)

14. Schuif omlaag naar de **instellingen** in het navigatiedeelvenster links, klikt u op **identiteit** om uit te breiden de gerelateerde sectie. Klik vervolgens op **Instellingen voor eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

15. Op de pagina **Instellingen voor eenmalige aanmelding** klikt u op de knop **Bewerken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure3.png)

16. Selecteer **SAML ingeschakeld** en klik vervolgens op **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

17. Als u uw SAML-instellingen voor eenmalige aanmelding wilt configureren, klikt u op **Nieuw op basis van het bestand met metagegevens**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

18. Klik op **bestand kiezen** voor het uploaden van het metagegevens-XML-bestand en klik op **maken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/xmlchoose.png)

19. Op de **eenmalige SAML-aanmelding instellingen** pagina velden automatisch vullen, typ de naam van de configuratie (bijvoorbeeld: *SPSSOWAAD_Test*) in de **naam** tekstvak en klik op opslaan.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

20. Als u wilt inschakelen in uw Salesforce-Sandbox-domein, moet u de volgende stappen uitvoeren:

    > [!NOTE]
    > Voordat het inschakelen van het domein moet u hetzelfde maken op de Salesforce-Sandbox. Zie voor meer informatie, [definiëren van uw domeinnaam](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US). Zodra het domein is gemaakt, zorg ervoor dat deze correct geconfigureerd.

21. Klik in het linkernavigatiedeelvenster in Salesforce-Sandbox op **Bedrijfsinstellingen** de gerelateerde sectie uitvouwen en klik vervolgens op **mijn domein**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-my-domain.png)

22. In de **verificatieconfiguratie** sectie, klikt u op **bewerken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-edit-auth-config.png)

23. In de **verificatieconfiguratie** sectie als **verificatieservice**, selecteert u de naam van de SAML Single Sign-On-instelling die u hebt ingesteld tijdens het configureren van eenmalige aanmelding in Sandbox met Salesforce en Klik op **opslaan**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure2.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype `brittasimon@yourcompanydomain.extension`. Bijvoorbeeld BrittaSimon@contoso.com.

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon verleent toegang tot Salesforce-Sandbox gebruiken Azure eenmalige aanmelding.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Salesforce Sandbox**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Salesforce Sandbox**.

    ![De Salesforce-Sandbox-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-salesforce-sandbox-test-user"></a>Salesforce-Sandbox testgebruiker maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Salesforce Sandbox. SalesForce-Sandbox biedt ondersteuning voor just-in-time inrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in Sandbox met Salesforce bestaat, wordt een nieuw wordt gemaakt wanneer u probeert te krijgen tot de Salesforce-Sandbox. SalesForce-Sandbox biedt ook ondersteuning voor automatisch inrichten van gebruikers, vindt u meer details [hier](salesforce-sandbox-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Salesforce-Sandbox-tegel in het toegangsvenster, moet u worden automatisch aangemeld bij de Salesforce-Sandbox waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

* [Inrichten van gebruikers configureren](salesforce-sandbox-provisioning-tutorial.md)
