---
title: 'Zelfstudie: Azure Active Directory-integratie met Salesforce Sandbox | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Salesforce Sandbox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6794d7eaccb488bb345227161f0bca02f14bc518
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57852552"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Zelfstudie: Azure Active Directory-integratie met Salesforce-Sandbox

In deze zelfstudie leert u hoe u Salesforce Sandbox integreren met Azure Active Directory (Azure AD).

Sandboxes bieden u de mogelijkheid om te maken van meerdere kopieën van uw organisatie in afzonderlijke omgevingen voor verschillende doeleinden, zoals ontwikkeling, testen en training, zonder verlies van gegevens en toepassingen in uw Salesforce-productie de organisatie.
Zie voor meer informatie, [sandbox-overzicht](https://help.salesforce.com/articleView?id=create_test_instance.htm&language=en_us&type=5).

Salesforce-Sandbox integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Salesforce-Sandbox heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Salesforce-Sandbox (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Salesforce Sandbox, moet u de volgende items:

- Een Azure AD-abonnement
- Een Salesforce-Sandbox eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Salesforce-Sandbox uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Salesforce-Sandbox uit de galerie toe te voegen

Voor het configureren van de integratie van Salesforce Sandbox in Azure AD, moet u Salesforce-Sandbox uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Salesforce-Sandbox uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

4. Typ in het zoekvak **Salesforce Sandbox**, selecteer **Salesforce Sandbox** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SalesForce-Sandbox in de lijst met resultaten](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met Salesforce-Sandbox op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Salesforce Sandbox is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Salesforce Sandbox tot stand worden gebracht.

In Salesforce-Sandbox, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Salesforce Sandbox, u nodig hebt voor de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
3. **[Maak een testgebruiker Salesforce Sandbox](#create-a-salesforce-sandbox-test-user)**  : als u wilt een equivalent van Britta Simon in Salesforce-Sandbox die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Salesforce-Sandbox-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Salesforce Sandbox, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Salesforce Sandbox** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

2. Klik op **modus voor één wijziging aanmelding** boven op het scherm selecteren de **SAML** modus.

      ![Koppeling Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/tutorial_general_300.png)

3. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Koppeling Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/tutorial_general_301.png)

4. Klik op de pagina **Eenmalige aanmelding met SAML instellen** u de knop **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.
   
    ![Koppeling Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/tutorial_general_302.png)

5. Voer in het gedeelte **Standaard SAML-configuratie** de volgende stappen uit als u de toepassing in de door **IDP** geïnitieerde modus wilt configureren:

    a. Klik op **Metagegevensbestand uploaden**.

    ![Metagegevensbestand uploaden](./media/salesforce-sandbox-tutorial/upload_metadata.png)

    b. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.

    ![Metagegevensbestand kiezen](./media/salesforce-sandbox-tutorial/browse_upload_metadata.png)

    > [!NOTE]
    > U ontvangt de serviceprovider-bestand met metagegevens van de Salesforce-Sandbox-beheerportal die later in de zelfstudie wordt uitgelegd.

    c. Nadat het bestand met metagegevens is geüpload, de **antwoord-URL** waarde krijgt automatisch ingevuld **antwoord-URL** tekstvak.

    ![SalesForce sandbox-domein en URL's, eenmalige aanmelding informatie](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url1.png)

6. Op de **SAML-handtekeningcertificaat** sectie, klikt u op op **downloaden** downloaden **federatieve metagegevens-XML** en sla het xml-bestand op uw computer.

    ![De link om het certificaat te downloaden](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

7. Open een nieuw tabblad in uw browser en meld u aan bij uw Salesforce-Sandbox administrator-account.

8. Klik op **Instellen** onder het **instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure1.png)

9. Schuif omlaag naar de **instellingen** in het navigatiedeelvenster links, klikt u op **identiteit** om uit te breiden de gerelateerde sectie. Klik vervolgens op **Instellingen voor eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

10. Op de pagina **Instellingen voor eenmalige aanmelding** klikt u op de knop **Bewerken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure3.png)

11. Selecteer **SAML ingeschakeld** en klik vervolgens op **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

12. Als u uw SAML-instellingen voor eenmalige aanmelding wilt configureren, klikt u op **Nieuw op basis van het bestand met metagegevens**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

13. Klik op **Bestand kiezen** om het XML-bestand met metagegevens te uploaden dat u hebt gedownload uit Azure Portal. Klik dan op **Maken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/xmlchoose.png)

14. Op de pagina **SAML-instellingen voor eenmalige aanmelding** worden de velden automatisch ingevuld. Klik op Opslaan.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/salesforcexml.png)

15. Op de **instellingen voor eenmalige aanmelding** pagina, klikt u op de **metagegevens downloaden** knop voor het downloaden van het metagegevensbestand van de service-provider. Gebruik dit bestand in de **SAML-basisconfiguratie** sectie in Azure portal voor het configureren van de vereiste URL's, zoals hierboven is uitgelegd.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure4.png)

16. Als u wilt configureren van de toepassing in **SP** gestart modus, zijn de vereisten voor die:

    a. U moet een geverifieerd domein hebben.

    b. U wilt configureren en inschakelen van uw Salesforce-Sandbox-domein, de stappen voor deze verderop in deze zelfstudie worden beschreven.

    c. In de Azure-portal op de **SAML-basisconfiguratie** sectie, klikt u op **extra URL's instellen** en voer de volgende stap:
  
    ![SalesForce sandbox-domein en URL's, eenmalige aanmelding informatie](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url.png)

    In de **aanmeldings-URL** tekstvak typt u de waarde met behulp van het volgende patroon: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    > [!NOTE]
    > Deze waarde moet worden gekopieerd van de Salesforce-Sandbox-portal nadat u het domein hebt ingeschakeld.

17. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **federatieve metagegevens-XML** en sla het xml-bestand op uw computer.

    ![De link om het certificaat te downloaden](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png)

18. Open een nieuw tabblad in uw browser en meld u aan bij uw Salesforce-Sandbox administrator-account.

19. Klik op **Instellen** onder het **instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure1.png)

20. Schuif omlaag naar de **instellingen** in het navigatiedeelvenster links, klikt u op **identiteit** om uit te breiden de gerelateerde sectie. Klik vervolgens op **Instellingen voor eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

21. Op de pagina **Instellingen voor eenmalige aanmelding** klikt u op de knop **Bewerken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure3.png)

22. Selecteer **SAML ingeschakeld** en klik vervolgens op **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

23. Als u uw SAML-instellingen voor eenmalige aanmelding wilt configureren, klikt u op **Nieuw op basis van het bestand met metagegevens**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

24. Klik op **bestand kiezen** voor het uploaden van het metagegevens-XML-bestand en klik op **maken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/xmlchoose.png)

25. Op de **eenmalige SAML-aanmelding instellingen** pagina velden automatisch vullen, typ de naam van de configuratie (bijvoorbeeld: *SPSSOWAAD_Test*) In de **naam** tekstvak en klik op opslaan.

    ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

26. Als u wilt inschakelen in uw Salesforce-Sandbox-domein, moet u de volgende stappen uitvoeren:

    > [!NOTE]
    > Voordat het inschakelen van het domein moet u hetzelfde maken op de Salesforce-Sandbox. Zie voor meer informatie, [definiëren van uw domeinnaam](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US). Zodra het domein is gemaakt, zorg ervoor dat deze correct geconfigureerd.

    * Klik in het linkernavigatiedeelvenster in Salesforce-Sandbox op **Bedrijfsinstellingen** de gerelateerde sectie uitvouwen en klik vervolgens op **mijn domein**.

         ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-my-domain.png)

    * In de **verificatieconfiguratie** sectie, klikt u op **bewerken**.

        ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/sf-edit-auth-config.png)

    * In de **verificatieconfiguratie** sectie als **verificatieservice**, selecteert u de naam van de SAML Single Sign-On-instelling die u hebt ingesteld tijdens het configureren van eenmalige aanmelding in Sandbox met Salesforce en Klik op **opslaan**.

        ![Eenmalige aanmelding configureren](./media/salesforce-sandbox-tutorial/configure2.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](./media/salesforce-sandbox-tutorial/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](./media/salesforce-sandbox-tutorial/create_aaduser_02.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In de **gebruikersnaam** veldtype **brittasimon\@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="create-a-salesforce-sandbox-test-user"></a>Maak een testgebruiker Salesforce-Sandbox

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Salesforce Sandbox. SalesForce-Sandbox biedt ondersteuning voor just-in-time inrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in Sandbox met Salesforce bestaat, wordt een nieuw wordt gemaakt wanneer u probeert te krijgen tot de Salesforce-Sandbox. SalesForce-Sandbox biedt ook ondersteuning voor automatisch inrichten van gebruikers, vindt u meer details [hier](salesforce-sandbox-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon verleent toegang tot Salesforce-Sandbox gebruiken Azure eenmalige aanmelding.

![De de gebruikersrol toewijzen][200]

**Als u wilt toewijzen Britta Simon naar Salesforce Sandbox, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Salesforce Sandbox**.

    ![De Salesforce-Sandbox-koppeling in de lijst met toepassingen](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen][202]

4. Klik op **gebruiker toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Salesforce-Sandbox-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Salesforce-Sandbox-toepassing.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Inrichten van gebruikers configureren](salesforce-sandbox-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/salesforce-sandbox-tutorial/tutorial_general_01.png
[2]: ./media/salesforce-sandbox-tutorial/tutorial_general_02.png
[3]: ./media/salesforce-sandbox-tutorial/tutorial_general_03.png
[4]: ./media/salesforce-sandbox-tutorial/tutorial_general_04.png

[100]: ./media/salesforce-sandbox-tutorial/tutorial_general_100.png

[200]: ./media/salesforce-sandbox-tutorial/tutorial_general_200.png
[201]: ./media/salesforce-sandbox-tutorial/tutorial_general_201.png
[202]: ./media/salesforce-sandbox-tutorial/tutorial_general_202.png
[203]: ./media/salesforce-sandbox-tutorial/tutorial_general_203.png
