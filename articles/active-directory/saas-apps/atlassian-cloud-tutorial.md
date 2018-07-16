---
title: 'Zelfstudie: Azure Active Directory-integratie met Atlassian Cloud | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de Atlassian-Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes
ms.openlocfilehash: f0ad879bb084a8d3a50a0934557eae64621c0160
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054248"
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Zelfstudie: Azure Active Directory-integratie met Atlassian Cloud

In deze zelfstudie leert u hoe u Atlassian Cloud integreren met Azure Active Directory (Azure AD).

Atlassian Cloud integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Atlassian-Cloud heeft.
- U kunt uw gebruikers kunnen automatisch worden aangemeld inschakelen (eenmalige aanmelding) naar de Atlassian Cloud met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over software als een service (SaaS)-app-integratie met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Atlassian Cloud, moet u de volgende items:

- Een Azure AD-abonnement.
- Om in te schakelen Security Assertion Markup Language (SAML) eenmalige aanmelding voor Atlassian cloudproducten, moet u instellen van Identity Manager. Meer informatie over [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager).

> [!NOTE]
> Wanneer u de stappen in deze zelfstudie hebt getest, raden wij aan dat u niet een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario wordt beschreven in de zelfstudie bestaat uit twee belangrijkste bouwstenen:

* Atlassian Cloud uit de galerie toe te voegen
* Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-atlassian-cloud-from-the-gallery"></a>Atlassian Cloud uit de galerie toevoegen
Voor het configureren van de integratie van Atlassian Cloud met Azure AD, Atlassian Cloud uit de galerie toevoegt aan uw lijst met beheerde SaaS-apps door het volgende te doen:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop][1]

2. Selecteer **bedrijfstoepassingen** > **alle toepassingen**.

    ![Het deelvenster met Enterprise-toepassingen][2]
    
3. Als u wilt een toepassing hebt toegevoegd, selecteert u **nieuwe toepassing**.

    ![De 'nieuwe toepassing"knop][3]

4. Typ in het zoekvak **Atlassian Cloud**, selecteer in de lijst met resultaten **Atlassian Cloud**, en selecteer vervolgens **toevoegen**.

    ![Atlassian Cloud in de lijst met resultaten](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Atlassian Cloud, op basis van een testgebruiker met de naam *Britta Simon*.

Voor eenmalige aanmelding om te werken, moet Azure AD de Atlassian-Cloud-gebruiker en het bijbehorende equivalent identificatie in Azure AD. Met andere woorden, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker maken in de Atlassian Cloud.

Toewijzen als de Atlassian-Cloud voor het maken van de relatie koppeling *gebruikersnaam* dezelfde waarde die toegewezen aan de Azure AD *gebruikersnaam*.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Atlassian Cloud, moet u de bouwstenen die u in de volgende secties te voltooien.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Atlassian Cloud.

Voor het configureren van Azure AD eenmalige aanmelding met Atlassian Cloud, het volgende doen:

1. In de Azure-portal in de **Atlassian Cloud** toepassing integratie venster **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. In de **eenmalige aanmelding** venster in de **modus voor één aanmelding** Schakel **SAML gebaseerde aanmelding**.

    ![Venster voor eenmalige aanmelding](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. Het configureren van de toepassing in de modus voor IDP gestart door onder **Atlassian Cloud domein en URL's**, doet u het volgende:

    ![Atlassian Cloud domein en URL's, eenmalige aanmelding informatie](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)
    
    a. In de **id** in het vak **`https://auth.atlassian.com/saml/<unique ID>`**.
    
    b. In de **antwoord-URL** in het vak **`https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`**.

    c. In de **Relaystatus** vak, typ een URL met de volgende syntaxis: **`https://<instancename>.atlassian.net`**.

4. Voor het configureren van de toepassing in de modus SP geïnitieerde, selecteer de **geavanceerde URL-instellingen weergeven** en klikt u op de **aanmeldings-URL** vak, typ een URL met de volgende syntaxis: **`https://<instancename>.atlassian.net`** .

    ![Atlassian Cloud domein en URL's, eenmalige aanmelding informatie](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    > [!NOTE]
    > De bovenstaande waarden zijn niet echt. Deze bijwerken met de werkelijke-id, de antwoord-URL en de aanmeldings-URL-waarden. U krijgt de werkelijke waarden van het scherm Atlassian Cloud SAML-configuratie. We leggen de waarden later in de zelfstudie.

5. Onder **SAML-handtekeningcertificaat**, selecteer **Certificate(Base64)**, en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png)

6. Uw cloudtoepassing Atlassian wordt verwacht dat de SAML-asserties ondertekend vinden in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw Token kenmerken van SAML-configuratie. 

    Standaard de **gebruikers-id** waarde is toegewezen aan user.userprincipalname. Deze waarde om toe te wijzen aan user.mail wijzigen. U kunt ook een andere geschikte waarde op basis van de installatie van uw organisatie, maar in de meeste gevallen, e-mailbericht moet werken.

    ![De downloadkoppeling certificaat](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_attribute.png)

7. Selecteer **Opslaan**.

    ![De configureren eenmalige aanmelding knop Opslaan](./media/atlassian-cloud-tutorial/tutorial_general_400.png)

8. Om te openen de **aanmelding configureren** venster in de **Atlassian Cloudconfiguratie** sectie, selecteer **Atlassian Cloud configureren**.

9. In de **Naslaggids** sectie, Kopieer de **SAML entiteit-ID** en **Single Sign-On Service URL voor SAML**.

    ![Atlassian Cloud-configuratie](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png)

10. Als u eenmalige aanmelding configureren voor uw toepassing, moet u zich aanmelden bij de portal Atlassian met beheerdersreferenties.

11. U moet uw domein verifiëren voordat u verdergaat met het configureren van eenmalige aanmelding. Zie voor meer informatie, [Atlassian domeinverificatie](https://confluence.atlassian.com/cloud/domain-verification-873871234.html) document.

12. Selecteer in het linkerdeelvenster **eenmalige aanmelding SAML**. Als u dit nog niet hebt gedaan, zich abonneren op Atlassian Identity Manager.

    ![Eenmalige aanmelding configureren](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

13. In de **toevoegen de SAML-configuratie** venster de volgende handelingen uit:

    ![Eenmalige aanmelding configureren](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. In de **id-provider entiteit-ID** vak, plak de SAML-entiteit-ID die u hebt gekopieerd vanuit Azure portal.

    b. In de **id-provider voor eenmalige aanmelding URL** vak, plak de URL voor SAML-service voor eenmalige aanmelding die u hebt gekopieerd vanuit Azure portal.

    c. Open het gedownloade certificaat vanuit de Azure-portal in een txt-bestand, Kopieer de waarde (zonder de *Begin Certificate* en *eindcertificaat* regels), en plak deze in de **openbare X509 certificaat** vak.
    
    d. Selecteer **configuratie op te slaan**.
     
14. Om ervoor te zorgen dat u de juiste URL's hebt ingesteld, moet u de Azure AD-instellingen bijwerken door het volgende te doen:

    ![Eenmalige aanmelding configureren](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. Kopieer in het SAML-venster de **SP identiteit ID** en klikt u op de Azure-portal, onder Atlassian Cloud **domein en URL's**, plak deze in de **id** vak.
    
    b. Kopieer in het SAML-venster de **SP URL van de Bevestigingsconsumerservice** en klikt u op de Azure-portal, onder Atlassian Cloud **domein en URL's**, plak deze in de **antwoord-URL** vak. De aanmeldings-URL is de tenant-URL van uw Atlassian-Cloud.

    > [!NOTE]
    > Als u een bestaande klant bent, na het bijwerken van de **SP identiteit ID** en **SP URL van de Bevestigingsconsumerservice** waarden in de Azure-portal, selecteer **Ja, werk de configuratie van**. Als u een nieuwe klant bent, kunt u deze stap overslaan.
    
15. Selecteer in de Azure portal, **opslaan**.

    ![Eenmalige aanmelding configureren](./media/atlassian-cloud-tutorial/tutorial_general_400.png)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

In deze sectie maakt maken u testgebruiker Britta Simon in Azure portal door het volgende te doen:

   ![Maak een testgebruiker Azure AD][100]

1. Selecteer in de Azure portal, in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/atlassian-cloud-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, selecteert u **gebruikers en groepen** > **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/atlassian-cloud-tutorial/create_aaduser_02.png)

3. In de **alle gebruikers** venster **toevoegen**.

    ![De knop toevoegen](./media/atlassian-cloud-tutorial/create_aaduser_03.png)

4. In de **gebruiker** venster de volgende handelingen uit:

    ![Het venster van de gebruiker](./media/atlassian-cloud-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Selecteer **Maken**.

### <a name="create-an-atlassian-cloud-test-user"></a>Maak een testgebruiker Atlassian Cloud

Azure AD-gebruikers zich aanmelden bij Atlassian Cloud, de gebruikersaccounts handmatig in de Atlassian Cloud inrichten als volgt inschakelen:

1. In de **beheer** venster **gebruikers**.

    ![De koppeling Atlassian Cloudgebruikers](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png)

2. Voor het maken van een gebruiker in de Atlassian Cloud selecteert **uitnodiging gebruiker**.

    ![Maakt u een gebruiker Atlassian Cloud](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png)

3. In de **e-mailadres** vak, voer het e-mailadres van de gebruiker en klikt u vervolgens de toepassing toewijst.

    ![Maakt u een gebruiker Atlassian Cloud](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)

4. Selecteer voor het verzenden van een e-mailbericht naar de gebruiker, **gebruikers uitnodigen**. Een e-mailbericht is verzonden naar de gebruiker en, nadat u hebt de uitnodiging geaccepteerd, de gebruiker is actief in het systeem.

>[!NOTE]
>U kunt ook bulksgewijs-gebruikers maken door te selecteren de **bulksgewijs maken** knop in de **gebruikers** sectie.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u gebruiker Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Atlassian Cloud. Om dit te doen, het volgende doen:

![De de gebruikersrol toewijzen][200]

1. Open in de Azure-portal, de **toepassingen** weergeven, gaat u naar de directoryweergave en selecteer vervolgens **bedrijfstoepassingen** > **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. In de **toepassingen** in de lijst met **Atlassian Cloud**.

    ![De Atlassian-Cloud-koppeling in de lijst met toepassingen](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png)

3. Selecteer in het linkerdeelvenster **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Selecteer **toevoegen** en klikt u op de **toevoegen toewijzing** venster **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** venster in de **gebruikers** in de lijst met **Britta Simon**.

6. In de **gebruikers en groepen** venster **Selecteer**.

7. In de **toevoegen toewijzing** venster **toewijzen**.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u selecteert de **Atlassian Cloud** tegel in het toegangsvenster, u moet zijn aangemeld op automatisch aan uw toepassing Atlassian Cloud.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/atlassian-cloud-tutorial/tutorial_general_203.png
