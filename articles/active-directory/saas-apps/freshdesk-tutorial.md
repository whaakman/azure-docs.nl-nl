---
title: 'Zelfstudie: Azure Active Directory-integratie met FreshDesk | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en FreshDesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2018
ms.author: jeedes
ms.openlocfilehash: b5968b83fc9beb481e2ad2c0cd44d2c284747fa1
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51010810"
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Zelfstudie: Azure Active Directory-integratie met FreshDesk

In deze zelfstudie leert u hoe u FreshDesk integreren met Azure Active Directory (Azure AD).

FreshDesk integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot FreshDesk heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij FreshDesk (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met FreshDesk, moet u de volgende items:

- Een Azure AD-abonnement
- Een FreshDesk eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. FreshDesk uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-freshdesk-from-the-gallery"></a>FreshDesk uit de galerie toe te voegen

Voor het configureren van de integratie van FreshDesk in Azure AD, moet u FreshDesk uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen FreshDesk uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **FreshDesk**, selecteer **FreshDesk** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![FreshDesk in de lijst met resultaten](./media/freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met FreshDesk op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in FreshDesk is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in FreshDesk tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met FreshDesk, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker FreshDesk](#creating-a-freshdesk-test-user)**  : als u wilt een equivalent van Britta Simon in FreshDesk die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw FreshDesk-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met FreshDesk, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **FreshDesk** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **selecteert u een methode voor eenmalige aanmelding** dialoogvenster, klikt u op **Selecteer** voor **SAML** modus voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](common/tutorial_general_301.png)

3. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op **bewerken** pictogram openen **SAML-basisconfiguratie** dialoogvenster.

    ![Eenmalige aanmelding configureren](common/editconfigure.png)

4. Op de **SAML-basisconfiguratie** sectie, voert u de volgende stappen uit:

    ![FreshDesk-domein en URL's, eenmalige aanmelding informatie](./media/freshdesk-tutorial/tutorial_freshdesk_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<tenant-name>.freshdesk.com` of een andere waarde Freshdesk is voorgesteld.

    b. In de **id (entiteits-ID)** tekstvak, een URL met behulp van het volgende patroon: `https://<tenant-name>.freshdesk.com` of een andere waarde Freshdesk is voorgesteld.

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [FreshDesk-Client-ondersteuningsteam](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) om deze waarden te verkrijgen.

5. FreshDesk-toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. De volgende Schermafbeelding toont een voorbeeld voor deze. De standaardwaarde van **gebruikers-id** is **user.userprincipalname** maar **FreshDesk** wordt verwacht dat deze optie om te worden toegewezen met de e-mailadres van de gebruiker. Hiervoor kunt u **user.mail** kenmerk in de lijst of gebruik de waarde van het juiste kenmerk op basis van de organisatieconfiguratie van uw.

    ![image](./media/freshdesk-tutorial/i4-attribute.png)

6. In de **gebruikersclaims** sectie op de **gebruikerskenmerken en Claims** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de bovenstaande afbeelding en voer de volgende stappen uit:
    
    a. Klik op **bewerkingspictogram** openen de **gebruikersclaims beheren** dialoogvenster.

    ![image](./media/freshdesk-tutorial/i2-attribute.png)

    ![image](./media/freshdesk-tutorial/i3-attribute.png)

    b. Uit de **bronkenmerk** in de lijst met **user.mail**.

    c. Klik op **Opslaan**.

7. Op de **SAML-handtekeningcertificaat** pagina, in de **SAML-handtekeningcertificaat** sectie, klikt u op **downloaden** downloaden **certificaat (Base64)** en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/freshdesk-tutorial/tutorial_freshdesk_certificate.png)

8. Open **opdrachtprompt** en voer de volgende opdrachten uit:

    a. Voer `certutil.exe -dump FreshDesk.cer` waarde in de opdrachtprompt.

    > [!NOTE]
    > Hier **FreshDesk.cer** is het certificaat dat u hebt gedownload vanuit Azure portal.

    b. Kopieer de **Cert Hash(sha256)** waarde en plak deze in Kladblok. 

9. Op de **instellen van FreshDesk** sectie, kopieert u de juiste URL aan de hand van uw behoeften.

    a. Aanmeldings-URL

    b. Azure AD-id

    c. URL voor afmelden

    ![Configuratie van FreshDesk](common/configuresection.png)

10. Meld u in een ander browservenster in uw bedrijf Freshdesk site als beheerder.

11. Selecteer de **Instellingenpictogram** en in de **Security** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding](./media/freshdesk-tutorial/IC776770.png "eenmalige aanmelding")
  
    a. Voor **eenmalige aanmelding (SSO)**, selecteer **op**.

    b. Selecteer **SAML SSO**.

    c. In de **aanmeldings-URL voor SAML** tekstvak plakken **aanmeldings-URL** waarde die u hebt gekopieerd vanuit Azure portal.

    d. In de **afmeldings-URL van** tekstvak plakken **afmeldings-URL van** waarde die u hebt gekopieerd vanuit Azure portal.

    e. In de **Security certificaat vingerafdruk** tekstvak plakken **Cert Hash(sha256)** waarde die u eerder hebt ontvangen.
  
    f. Klik op **Opslaan**.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

1. Selecteer in de Azure portal, in het linkerdeelvenster **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.

    ![Azure AD-gebruiker maken][100]

2. Selecteer **nieuwe gebruiker** aan de bovenkant van het scherm.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_01.png) 

3. In de eigenschappen van de gebruiker de volgende stappen uitvoeren.

    ![Het maken van een Azure AD-testgebruiker](common/create_aaduser_02.png)

    a. In de **naam** veld **BrittaSimon**.
  
    b. In de **gebruikersnaam** veld, typt u **brittasimon@yourcompanydomain.extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Selecteer **eigenschappen**, selecteer de **Show wachtwoord** selectievakje en noteer de waarde die wordt weergegeven in het wachtwoord.

    d. Selecteer **Maken**.

### <a name="creating-a-freshdesk-test-user"></a>Het maken van een testgebruiker FreshDesk

Om in te schakelen in Azure AD-gebruikers zich aanmelden bij FreshDesk, moeten ze worden ingericht in FreshDesk.  
In het geval van FreshDesk is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Freshdesk** tenant.

2. Klik in het menu aan de bovenkant op **Admin**.

    ![Beheerder](./media/freshdesk-tutorial/IC776772.png "Admin")

3. In de **algemene instellingen** tabblad **Agents**.
  
    ![Agents](./media/freshdesk-tutorial/IC776773.png "Agents")

4. Klik op **nieuwe Agent**.

    ![Nieuwe Agent](./media/freshdesk-tutorial/IC776774.png "nieuwe-Agent")

5. In het dialoogvenster voor informatie over agents, moet u de volgende stappen uitvoeren:

    ![Agentgegevens](./media/freshdesk-tutorial/IC776775.png "agentgegevens")

    a. In de **e** tekstvak, typ de Azure AD e-mailadres van de Azure AD-account dat u inrichten wilt.

    b. In de **volledige naam** tekstvak, typ de naam van de Azure AD-account dat u inrichten wilt.

    c. In de **titel** tekstvak typt u de titel van de Azure AD-account dat u inrichten wilt.

    d. Klik op **Opslaan**.

    >[!NOTE]
    >De houder van Azure AD-account ontvangt een e-mailbericht een koppeling om te bevestigen dat het account bevat voordat deze wordt geactiveerd.
    >
    >[!NOTE]
    >U kunt alle andere Freshdesk gebruiker-account maken van hulpprogramma's of API's geleverd door Freshdesk inrichten AAD-accounts voor gebruikers naar freshdesk kopiëren.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan FreshDesk.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **FreshDesk**.

    ![Eenmalige aanmelding configureren](./media/freshdesk-tutorial/tutorial_freshdesk_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst met gebruikers, en klik op de **Selecteer** knop aan de onderkant van het scherm.

6. In de **toevoegen toewijzing** dialoogvenster Selecteer de **toewijzen** knop.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel FreshDesk in het toegangsvenster, u moet u automatisch aangemeld bij uw FreshDesk-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
