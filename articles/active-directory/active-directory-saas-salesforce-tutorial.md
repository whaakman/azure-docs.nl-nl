---
title: 'Zelfstudie: Azure Active Directory-integratie met Salesforce | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Salesforce.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 639e40ca7e406a1726033e9f5c5363c289087589
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Zelfstudie: Azure Active Directory-integratie met Salesforce

In deze zelfstudie leert u hoe Salesforce integreren met Azure Active Directory (Azure AD).

Salesforce integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Salesforce heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Salesforce (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Salesforce, moet u de volgende items:

- Een Azure AD-abonnement
- Een Salesforce eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Salesforce uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-salesforce-from-the-gallery"></a>Salesforce uit de galerie toevoegen
Voor het configureren van de integratie van Salesforce in Azure AD, moet u Salesforce uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Salesforce uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Salesforce**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_search.png)

5. Selecteer in het deelvenster resultaten **Salesforce**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Salesforce op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Salesforce is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Salesforce tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Salesforce.

Om te configureren en testen van Azure AD eenmalige aanmelding met Salesforce, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Salesforce](#creating-a-salesforce-test-user)**  - Salesforce die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw Salesforce-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Salesforce, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Salesforce** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_samlbase.png)

3. Op de **Salesforce-domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_url.png)

    In de **aanmeldings-URL** textbox, typ de waarde met het volgende patroon volgen: 
   * Account voor de onderneming:`https://<subdomain>.my.salesforce.com`
   * Developer-account:`https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE] 
    > Deze waarden zijn niet de werkelijke. Deze waarden bijwerken met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [Salesforce Client ondersteuningsteam](https://help.salesforce.com/support) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **certificaat** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/tutorial_general_400.png)

6. Op de **Salesforce configuratie** sectie, klikt u op **configureren Salesforce** openen **eenmalige aanmelding configureren** venster. Kopieer de **SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.** 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS>
7.  Open een nieuw tabblad in de browser en meld u aan op uw Salesforce-beheerdersaccount.

8.  Onder de **beheerder** navigatiedeelvenster, klikt u op **beveiligingsmechanismen** uitbreiden van de bijbehorende sectie. Klik vervolgens op **instellingen voor eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png)

9.  Op de **instellingen voor eenmalige aanmelding** pagina, klikt u op de **bewerken** knop.
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)

      > [!NOTE]
      > Als u nog geen Single Sign-On-instellingen voor uw Salesforce-account inschakelen, moet u mogelijk contact opnemen met [Salesforce Client ondersteuningsteam](https://help.salesforce.com/support). 

10. Selecteer **SAML ingeschakeld**, en klik vervolgens op **opslaan**.

      ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png)
11. Uw SAML eenmalige aanmelding om instellingen te configureren, klikt u op **nieuw**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png)

12. Op de **SAML Single Sign-On instelling bewerken** maken de volgende configuraties:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png)

    a. Voor de **naam** veld, typ een beschrijvende naam voor deze configuratie. Om een waarde voor **naam** automatisch invullen de **API-naam** textbox.

    b. Plakken **SMAL entiteit-ID** waarde in de **verlener** veld in Salesforce.

    c. In de **tekstvak voor de entiteit-Id**, typ de naam van uw Salesforce-domein met het volgende patroon volgen:
      
      * Account voor de onderneming:`https://<subdomain>.my.salesforce.com`
      * Developer-account:`https://<subdomain>-dev-ed.my.salesforce.com`
      
    d. Klik op **Bladeren** of **bestand kiezen** openen de **te uploaden bestand kiezen** dialoogvenster uw Salesforce-certificaat selecteren en klik vervolgens op **openen** om het certificaat te uploaden.

    e. Voor **SAML identiteitstype**, selecteer **verklaring van de gebruiker salesforce.com gebruikersnaam bevat**.

    f. Voor **SAML identiteit locatie**, selecteer **identiteit is in het element NameIdentifier van het onderwerp overzicht**

    g. Plakken **Single Sign-On Service-URL** in de **identiteit Provider aanmeldings-URL** veld in Salesforce.
    
    h. Voor **Provider geïnitieerd aanvragen servicebinding**, selecteer **HTTP-omleiding**.
    
    ik. Tot slot op **opslaan** instellingen toe te passen uw SAML eenmalige aanmelding.

13. Klik op het linkernavigatiedeelvenster in Salesforce **domeinbeheer** Vouw de bijbehorende sectie en klik vervolgens op **mijn domein**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png)

14. Schuif omlaag naar de **verificatieconfiguratie** uit en klik op de **bewerken** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png)

15. In de **verificatieservice** sectie, selecteert u de beschrijvende naam van uw configuratie SAML SSO en klik vervolgens op **opslaan**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Als meer dan één authentication-service is ingeschakeld, worden gebruikers gevraagd om te selecteren welke verificatieservice die ze aanmelden willen tijdens de initialisatie van eenmalige aanmelding met uw Salesforce-omgeving. Als u niet dat dit wilt gebeurt, dan u moet **laat alle andere verificatieservices uitgeschakeld**.
<CE>    
> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In het navigatiedeelvenster links in de **Azure-portal**, klikt u op **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-salesforce-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-salesforce-tutorial/create_aaduser_02.png) 

3. Aan de bovenkant van het dialoogvenster, klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-salesforce-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-salesforce-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-salesforce-test-user"></a>Een testgebruiker Salesforce maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Salesforce. SalesForce ondersteunt just-in-time-inrichting, die standaard is ingeschakeld.
Er is geen actie-item voor u in deze sectie. Als een gebruiker in Salesforce nog niet bestaat, wordt een nieuw gemaakt wanneer u probeert te krijgen tot Salesforce.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Salesforce.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Salesforce, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Salesforce**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Als u wilt testen van uw instellingen voor eenmalige aanmelding, opent u het toegangsvenster op [https://myapps.microsoft.com](https://myapps.microsoft.com/), meldt u zich bij de testaccount en klikt u op **Salesforce**.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Gebruikers inrichten configureren](active-directory-saas-salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_203.png

