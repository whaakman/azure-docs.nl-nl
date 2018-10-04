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
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: 37d20eabfc8fb883cda346dc8b55a17b8b959218
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268171"
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

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Zendesk uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-zendesk-from-the-gallery"></a>Zendesk uit de galerie toe te voegen

Voor het configureren van de integratie van Zendesk in Azure AD, moet u Zendesk uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Zendesk uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![image](./media/zendesk-tutorial/selectazuread.png)

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![image](./media/zendesk-tutorial/a_select_app.png)
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![image](./media/zendesk-tutorial/a_new_app.png)

4. Typ in het zoekvak **Zendesk**, selecteer **Zendesk** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![image](./media/zendesk-tutorial/a_add_app.png)

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

1. In de [Azure-portal](https://portal.azure.com/)op de **Zendesk** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![image](./media/zendesk-tutorial/b1_b2_select_sso.png)

2. Klik op **modus voor één wijziging aanmelding** boven op het scherm selecteren de **SAML** modus.

      ![image](./media/zendesk-tutorial/b1_b2_saml_ssso.png)

3. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![image](./media/zendesk-tutorial/b1_b2_saml_sso.png)

4. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **SAML-basisconfiguratie** dialoogvenster.

    ![image](./media/zendesk-tutorial/b1-domains_and_urlsedit.png)

5. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    a. In de **aanmeldings-URL** tekstvak typt u een URL met behulp van het volgende patroon: `https://<subdomain>.zendesk.com`.

    b. In de **id** tekstvak typt u een URL met behulp van het volgende patroon: `<subdomain>.zendesk.com`.

    ![image](./media/zendesk-tutorial/b1-domains_and_urls.png)

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [Zendesk-Client-ondersteuningsteam](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) om deze waarden te verkrijgen.

6. Zendesk wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Er zijn geen verplichte SAML-kenmerken, maar u kunt desgewenst een kenmerk van toevoegen **gebruikerskenmerken** sectie op de pagina van de toepassing-integratie. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** te openen **gebruikerskenmerken** dialoogvenster.

    ![image](./media/zendesk-tutorial/i4-attribute.png)

7. In de **gebruikersclaims** sectie op de **gebruikerskenmerken** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:

    a. Klik op **toevoegen nieuwe claim** openen de **gebruikersclaims beheren** dialoogvenster.

    ![image](./media/zendesk-tutorial/i2-attribute.png)

    ![image](./media/zendesk-tutorial/i3-attribute.png)
    
    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    c. Laat de **Namespace** leeg.

    d. Selecteer de bron als **kenmerk**.
    
    e. Uit de **bronkenmerk** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    f. Klik op **Ok**

    g. Klik op **Opslaan**.

    > [!NOTE]
    > U extensiekenmerken gebruiken om toe te voegen kenmerken die zich niet in Azure AD standaard. Klik op [gebruikerskenmerken die kunnen worden ingesteld in SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) om op te halen van de volledige lijst van kenmerken die met SAML **Zendesk** accepteert.

8. In de SAML-ondertekening sectie, in de **SAML-handtekeningcertificaat** sectie, Kopieer de **vingerafdruk**, en sla deze op uw computer.

    ![image](./media/zendesk-tutorial/C3_certificate.png)

    a. Selecteer de gewenste optie voor **ondertekening optie** indien nodig.

    b. Selecteer de gewenste optie voor **algoritme voor ondertekening** indien nodig.

    c. Klik op **Opslaan**.

9. Op de **instellen Zendesk** sectie, klikt u op **Stapsgewijze instructies bekijken over** openen **aanmelding configureren** venster. Kopiëren de onderstaande URL's van de **Naslaggids sectie.**

    Houd er rekening mee dat de url kan bijvoorbeeld het volgende:

    a. URL voor SAML-Service voor eenmalige aanmelding

    b. Entiteits-id

    c. URL voor afmelden

    ![image](./media/zendesk-tutorial/d1_saml.png) 

10. Er zijn twee manieren waarop Zendesk kan worden geconfigureerd - Automatic en Manual.
  
11. Voor het automatiseren van de configuratie in Zendesk, die u wilt installeren **mijn Apps beveiligde aanmelding browserextensie** door te klikken op **de extensie installeren**.

    ![image](./media/zendesk-tutorial/install_extension.png)

12. Na het toevoegen van uitbreiding naar de browser, klikt u op **Setup Zendesk** wordt u doorgeleid naar de Zendesk-toepassing. Van daaruit, geef de beheerdersreferenties aan te melden bij Zendesk. De browserextensie wordt automatisch configureren van de toepassing voor u en stap 13 automatiseren.

     ![image](./media/zendesk-tutorial/d2_saml.png) 

13. Als u Zendesk handmatig instellen wilt, opent u een nieuw browservenster en log in uw site van het bedrijf Zendesk als beheerder en voer de volgende stappen uit:

    * Klik op **Admin**.

    * Klik in het linkernavigatiedeelvenster op **instellingen**, en klik vervolgens op **Security**.

    * Op de **Security** pagina, voert u de volgende stappen uit:

      ![Beveiliging](././media/zendesk-tutorial/ic773089.png "beveiliging")

      ![Eenmalige aanmelding](././media/zendesk-tutorial/ic773090.png "eenmalige aanmelding")

      a. Klik op de **Admin & Agents** tabblad.

      b. Selecteer **eenmalige aanmelding (SSO) en SAML**, en selecteer vervolgens **SAML**.

      c. In **URL voor SAML SSO-** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.

      d. In **externe URL voor afmelden** tekstvak, plak de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal.

      e. In **certificaat vingerafdruk** tekstvak, plak de **vingerafdruk** waarde van het certificaat dat u hebt gekopieerd vanuit Azure portal.

      f. Klik op **Opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD 

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![image](./media/zendesk-tutorial/d_users_and_groups.png)

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![image](./media/zendesk-tutorial/d_adduser.png)

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![image](./media/zendesk-tutorial/d_userproperties.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veldtype **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="create-a-zendesk-test-user"></a>Maak een testgebruiker Zendesk

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Zendesk. Zendesk ondersteunt automatisch gebruikers inrichten, dit is standaard ingeschakeld. Meer informatie vindt u [hier](Zendesk-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

**Als u moet de gebruiker handmatig hebt gemaakt, kunt voert u de volgende stappen:**

> [!NOTE]
> **Eindgebruikers** accounts worden automatisch ingericht bij het aanmelden. **Agent** en **Admin** accounts moeten handmatig worden ingericht in **Zendesk** voordat u zich aanmeldt.

1. Meld u aan bij uw **Zendesk** tenant.

2. Selecteer de **klantenlijst** tabblad.

3. Selecteer de **gebruiker** tabblad en klik op **toevoegen**.

    ![Gebruiker toevoegen](././media/zendesk-tutorial/ic773632.png "gebruiker toevoegen")
4. Type de **naam** en **e** van een bestaande Azure AD-account dat u wilt inrichten, en klik vervolgens op **opslaan**.

    ![Nieuwe gebruiker](././media/zendesk-tutorial/ic773633.png "nieuwe gebruiker")

> [!NOTE]
> U kunt alle andere Zendesk gebruiker-account maken van hulpprogramma's of API's geleverd door Zendesk aan inrichten AAD-gebruikersaccounts.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding toegang verlenen tot Zendesk.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![image](./media/zendesk-tutorial/d_all_applications.png)

2. Selecteer in de lijst met toepassingen, **Zendesk**.

    ![image](./media/zendesk-tutorial/d_all_zendeskapplications.png)

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![image](./media/zendesk-tutorial/d_leftpaneusers.png)

4. Selecteer de **toevoegen** knop en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

    ![image](./media/zendesk-tutorial/d_assign_user.png)

4. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

5. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Zendesk-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Zendesk-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Inrichten van gebruikers configureren](zendesk-provisioning-tutorial.md)
