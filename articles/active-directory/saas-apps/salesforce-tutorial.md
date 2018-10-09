---
title: 'Zelfstudie: Azure Active Directory-integratie met Salesforce | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Salesforce.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2018
ms.author: jeedes
ms.openlocfilehash: 36f1bd9c11c8932968a3501ef22fdb7153411256
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867558"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Zelfstudie: Azure Active Directory-integratie met Salesforce

In deze zelfstudie leert u hoe u Salesforce integreren met Azure Active Directory (Azure AD).

Salesforce integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Salesforce heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Salesforce (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Salesforce, moet u de volgende items:

- Een Azure AD-abonnement
- Een Salesforce eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Salesforce uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-salesforce-from-the-gallery"></a>Salesforce uit de galerie toe te voegen

Voor het configureren van de integratie van Salesforce in Azure AD, moet u Salesforce uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Salesforce uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Salesforce**, selecteer **Salesforce** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![SalesForce in de lijst met resultaten](./media/salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Salesforce op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Salesforce is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Salesforce tot stand worden gebracht.

In Salesforce, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Salesforce, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Salesforce](#create-a-salesforce-test-user)**  : als u wilt een equivalent van Britta Simon in Salesforce die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Salesforce-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Salesforce, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Salesforce** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Klik op **modus voor één wijziging aanmelding** boven op het scherm selecteren de **SAML** modus.

    ![Koppeling voor eenmalige aanmelding configureren](./media/salesforce-tutorial/tutorial_general_300.png)

3. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Koppeling voor eenmalige aanmelding configureren](./media/salesforce-tutorial/tutorial_general_301.png)

4. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.
   
    ![Koppeling voor eenmalige aanmelding configureren](./media/salesforce-tutorial/tutorial_general_302.png)

5. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    ![SalesForce-domein en URL's, eenmalige aanmelding informatie](./media/salesforce-tutorial/tutorial_salesforce_url.png)

    a. In de **aanmeldings-URL** tekstvak typt u de waarde met behulp van het volgende patroon:

    Enterprise-account: `https://<subdomain>.my.salesforce.com`

    Developer-account: `https://<subdomain>-dev-ed.my.salesforce.com`

    b. In de **id** tekstvak typt u de waarde met behulp van het volgende patroon:

    Enterprise-account: `https://<subdomain>.my.salesforce.com`

    Developer-account: `https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [Salesforce-Client-ondersteuningsteam](https://help.salesforce.com/support) om deze waarden te verkrijgen.

6. Op de **SAML-handtekeningcertificaat** sectie, klikt u op op **downloaden** downloaden **federatieve metagegevens-XML** en sla het xml-bestand op uw computer.

    ![De downloadkoppeling certificaat](./media/salesforce-tutorial/tutorial_salesforce_certificate.png) 

7. Open een nieuw tabblad in uw browser en aan te melden bij uw Salesforce-administrator-account.

8. Klik op de **Setup** onder **Instellingenpictogram** in de rechterbovenhoek van de pagina.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/configure1.png)

9. Schuif omlaag naar de **instellingen** in het navigatiedeelvenster, klikt u op **identiteit** om uit te breiden de gerelateerde sectie. Klik vervolgens op **instellingen voor eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-admin-sso.png)

10. Op de **instellingen voor eenmalige aanmelding** pagina, klikt u op de **bewerken** knop.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-admin-sso-edit.png)

    > [!NOTE]
    > Als u zich niet aan het inschakelen van Single Sign-On-instellingen voor uw Salesforce-account, moet u mogelijk contact op met [Salesforce-Client-ondersteuningsteam](https://help.salesforce.com/support).

11. Selecteer **SAML ingeschakeld**, en klik vervolgens op **opslaan**.

      ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-enable-saml.png)

12. Uw SAML eenmalige aanmelding om instellingen te configureren, klikt u op **nieuw op basis van het bestand met metagegevens**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-admin-sso-new.png)

13. Klik op **bestand kiezen** voor het uploaden van het XML-bestand met metagegevens die u hebt gedownload van de Azure portal en klik op **maken**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/xmlchoose.png)

14. Op de **eenmalige SAML-aanmelding instellingen** pagina velden automatisch vullen en klik op opslaan.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/salesforcexml.png)

15. Klik op het navigatiedeelvenster links in Salesforce, **Bedrijfsinstellingen** de gerelateerde sectie uitvouwen en klik vervolgens op **mijn domein**.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-my-domain.png)

16. Schuif omlaag naar de **verificatieconfiguratie** uit en klikt u op de **bewerken** knop.

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-edit-auth-config.png)

17. In de **verificatieconfiguratie** sectie, Controleer de **AzureSSO** als **verificatie gevolgd** van de SAML SSO-configuratie, en klik vervolgens op **opslaan** .

    ![Eenmalige aanmelding configureren](./media/salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Als meer dan één authentication-service is ingeschakeld, wordt gebruikers gevraagd om te selecteren welke ze zich aanmelden met tijdens de initialisatie van single sign-on bij uw Salesforce-omgeving, zoals authentication-service. Als u niet dat dit wilt gebeurt, moet u **laat alle andere verificatieservices uitgeschakeld**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](./media/salesforce-tutorial/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](./media/salesforce-tutorial/create_aaduser_02.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="create-a-salesforce-test-user"></a>Maak een testgebruiker Salesforce

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Salesforce. SalesForce biedt ondersteuning voor just-in-time inrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in Salesforce bestaat, wordt een nieuw bestand wordt gemaakt wanneer u probeert te krijgen tot Salesforce. SalesForce biedt ook ondersteuning voor automatisch inrichten van gebruikers, vindt u meer details [hier](salesforce-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon verleent toegang tot Salesforce gebruiken Azure eenmalige aanmelding.

![De de gebruikersrol toewijzen][200]

**Als u wilt toewijzen Britta Simon met Salesforce, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Salesforce**.

    ![De Salesforce-koppeling in de lijst met toepassingen](./media/salesforce-tutorial/tutorial_salesforce_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **gebruiker toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Salesforce-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Salesforce-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Inrichten van gebruikers configureren](salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/salesforce-tutorial/tutorial_general_01.png
[2]: ./media/salesforce-tutorial/tutorial_general_02.png
[3]: ./media/salesforce-tutorial/tutorial_general_03.png
[4]: ./media/salesforce-tutorial/tutorial_general_04.png

[100]: ./media/salesforce-tutorial/tutorial_general_100.png

[200]: ./media/salesforce-tutorial/tutorial_general_200.png
[201]: ./media/salesforce-tutorial/tutorial_general_201.png
[202]: ./media/salesforce-tutorial/tutorial_general_202.png
[203]: ./media/salesforce-tutorial/tutorial_general_203.png