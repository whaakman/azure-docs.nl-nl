---
title: 'Zelfstudie: Azure Active Directory-integratie met Zendesk | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Zendesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: e5ef74329e2adb6f3b8b60f547231a245a03b1fe
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050592"
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Zelfstudie: Azure Active Directory-integratie met Zendesk

In deze zelfstudie leert u hoe u Zendesk integreren met Azure Active Directory (Azure AD).

Zendesk integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Zendesk heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Zendesk (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Zendesk, moet u de volgende items:

- Een Azure AD-abonnement
- Een Zendesk eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Zendesk uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zendesk-from-the-gallery"></a>Zendesk uit de galerie toe te voegen
Voor het configureren van de integratie van Zendesk in Azure AD, moet u Zendesk uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Zendesk uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Zendesk**, selecteer **Zendesk** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Zendesk in de lijst met resultaten](./media/zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Zendesk op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Zendesk is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Zendesk tot stand worden gebracht.

In Zendesk, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Zendesk, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Zendesk](#create-a-zendesk-test-user)**  : als u wilt een equivalent van Britta Simon in Zendesk die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Zendesk-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Zendesk, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Zendesk** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. Op de **Zendesk-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Zendesk-domein en URL's, eenmalige aanmelding informatie](./media/zendesk-tutorial/tutorial_zendesk_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.zendesk.com`

    b. In de **id** tekstvak typt u de waarde met behulp van het volgende patroon: `<subdomain>.zendesk.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [Zendesk-Client-ondersteuningsteam](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) om deze waarden te verkrijgen.

4. Op de **SAML-handtekeningcertificaat** sectie, Kopieer de **VINGERAFDRUK** waarde van het certificaat.

    ![De downloadkoppeling certificaat](./media/zendesk-tutorial/tutorial_zendesk_certificate.png)

5. Zendesk wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Er zijn geen verplichte SAML-kenmerken, maar u kunt desgewenst een kenmerk van toevoegen **gebruikerskenmerken** sectie door de onderstaande stappen te volgen: 

     ![Eenmalige aanmelding configureren](./media/zendesk-tutorial/tutorial_zendesk_attributes1.png)

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Configureren van eenmalige aanmelding toevoegen](./media/zendesk-tutorial/tutorial_attribute_04.png)

    ![Single Sign-On addattb configureren](./media/zendesk-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

    > [!NOTE]
    > U extensiekenmerken gebruiken om toe te voegen kenmerken die zich niet in Azure AD standaard. Klik op [gebruikerskenmerken die kunnen worden ingesteld in SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) om op te halen van de volledige lijst van kenmerken die met SAML **Zendesk** accepteert.

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/zendesk-tutorial/tutorial_general_400.png)

7. Op de **Zendesk configuratie** sectie, klikt u op **configureren Zendesk** openen **aanmelding configureren** venster. Kopiëren de **afmelding URL's en SAML Single Sign-On Service** uit de **Naslaggids sectie.**

    ![Zendesk-configuratie](./media/zendesk-tutorial/tutorial_zendesk_configure.png) 

8. Meld u in een ander browservenster in uw bedrijf Zendesk site als beheerder.

9. Klik op **Admin**.

10. Klik in het linkernavigatiedeelvenster op **instellingen**, en klik vervolgens op **Security**.

11. Op de **Security** pagina, voert u de volgende stappen uit: 

     ![Beveiliging](./media/zendesk-tutorial/ic773089.png "beveiliging")

    ![Eenmalige aanmelding](./media/zendesk-tutorial/ic773090.png "eenmalige aanmelding")

     a. Klik op de **Admin & Agents** tabblad.

     b. Selecteer **eenmalige aanmelding (SSO) en SAML**, en selecteer vervolgens **SAML**.

     c. In **URL voor SAML SSO-** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal. 

     d. In **externe URL voor afmelden** tekstvak, plak de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal.

     e. In **certificaat vingerafdruk** tekstvak, plak de **vingerafdruk** waarde van het certificaat dat u hebt gekopieerd vanuit Azure portal.

     f. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/zendesk-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/zendesk-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/zendesk-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/zendesk-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-zendesk-test-user"></a>Maak een testgebruiker Zendesk

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Zendesk. Zendesk ondersteunt automatisch gebruikers inrichten, dit is standaard ingeschakeld. Meer informatie vindt u [hier](zendesk-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

**Als u moet de gebruiker handmatig hebt gemaakt, kunt voert u de volgende stappen:**

> [!NOTE]
> **Eindgebruikers** accounts worden automatisch ingericht bij het aanmelden. **Agent** en **Admin** accounts moeten handmatig worden ingericht in **Zendesk** voordat u zich aanmeldt.

1. Meld u aan bij uw **Zendesk** tenant.

2. Selecteer de **klantenlijst** tabblad.

3. Selecteer de **gebruiker** tabblad en klik op **toevoegen**.

    ![Gebruiker toevoegen](./media/zendesk-tutorial/ic773632.png "gebruiker toevoegen")
4. Type de **naam** en **e** van een bestaande Azure AD-account dat u wilt inrichten, en klik vervolgens op **opslaan**.

    ![Nieuwe gebruiker](./media/zendesk-tutorial/ic773633.png "nieuwe gebruiker")

> [!NOTE]
> U kunt alle andere Zendesk gebruiker-account maken van hulpprogramma's of API's geleverd door Zendesk aan inrichten AAD-gebruikersaccounts.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot Zendesk.

![De de gebruikersrol toewijzen][200]

**Als u wilt toewijzen Britta Simon met Zendesk, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Zendesk**.

    ![De Zendesk-koppeling in de lijst met toepassingen](./media/zendesk-tutorial/tutorial_zendesk_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Zendesk-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Zendesk-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Inrichten van gebruikers configureren](zendesk-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/zendesk-tutorial/tutorial_general_01.png
[2]: ./media/zendesk-tutorial/tutorial_general_02.png
[3]: ./media/zendesk-tutorial/tutorial_general_03.png
[4]: ./media/zendesk-tutorial/tutorial_general_04.png

[100]: ./media/zendesk-tutorial/tutorial_general_100.png

[200]: ./media/zendesk-tutorial/tutorial_general_200.png
[201]: ./media/zendesk-tutorial/tutorial_general_201.png
[202]: ./media/zendesk-tutorial/tutorial_general_202.png
[203]: ./media/zendesk-tutorial/tutorial_general_203.png
