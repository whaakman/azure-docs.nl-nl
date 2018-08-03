---
title: 'Zelfstudie: Azure Active Directory-integratie met NetSuite | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en NetSuite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: jeedes
ms.openlocfilehash: 511fdcf587d16a59ff2bb11dfc55504b2218a569
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431402"
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Zelfstudie: Azure Active Directory-integratie met NetSuite

In deze zelfstudie leert u hoe u NetSuite integreren met Azure Active Directory (Azure AD).

NetSuite integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot NetSuite heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij NetSuite (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met NetSuite, moet u de volgende items:

- Een Azure AD-abonnement
- Een NetSuite eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. NetSuite uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-netsuite-from-the-gallery"></a>NetSuite uit de galerie toe te voegen
Voor het configureren van de integratie van NetSuite in Azure AD, moet u NetSuite uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen NetSuite uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]

1. Klik op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **NetSuite**, selecteer **NetSuite** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![NetSuite in de lijst met resultaten](./media/netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met NetSuite op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in NetSuite is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in NetSuite tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in NetSuite.

Om te configureren en testen van Azure AD eenmalige aanmelding met NetSuite, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker NetSuite](#creating-a-netsuite-test-user)**  : als u wilt een equivalent van Britta Simon in NetSuite die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing NetSuite.

**Voor het configureren van Azure AD eenmalige aanmelding met NetSuite, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **NetSuite** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/tutorial_NetSuite_samlbase.png)

1. Op de **NetSuite domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/tutorial_NetSuite_url.png)

    In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon:

    `https://<tenant-name>.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.NetSuite.com/saml2/acs`

    `https://<tenant-name>.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.sandbox.NetSuite.com/saml2/acs`
    
    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke antwoord-URL. Neem contact op met [NetSuite ondersteuningsteam](http://www.NetSuite.com/portal/services/support.shtml) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/tutorial_NetSuite_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/tutorial_general_400.png)

1. Op de **NetSuite configuratie** sectie, klikt u op **configureren NetSuite** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/tutorial_NetSuite_configure.png)

1. Open een nieuw tabblad in uw browser en meld u aan bij uw site van het bedrijf NetSuite als beheerder.

1. In de werkbalk boven aan de pagina, klikt u op **Setup**, navigeert u vervolgens naar **bedrijf** en klikt u op **-functies inschakelen**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-setupsaml.png)

1. Klik in de werkbalk op het midden van de pagina op **SuiteCloud**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-suitecloud.png)

1. Onder **verificatie beheren** sectie, selecteer **SAML SINGLE SIGN-ON** de optie SAML EENMALIGE aanmelding in NetSuite inschakelen.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-ticksaml.png)

1. Klik in de werkbalk boven aan de pagina op **Setup**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-setup.png)

1. Uit de **INSTALLATIETAKEN** lijst, klikt u op **integratie**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-integration.png)

1. In de **verificatie beheren** sectie, klikt u op **SAML Single Sign-on**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-saml.png)

1. Op de **SAML Setup** pagina onder **NetSuite configuratie** sectie de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-saml-setup.png)
  
    a. Selecteer **primaire VERIFICATIEMETHODE**.

    b. Voor het veld met het label **SAMLV2 IDENTITY PROVIDER metagegevens**, selecteer **IDP METAGEGEVENSBESTAND uploaden**. Klik vervolgens op **Bladeren** voor het uploaden van het bestand met metagegevens die u hebt gedownload van Azure portal.

    c. Klik op **indienen**.

1. Klik in Azure AD op **weergeven en bewerk alle andere gebruikerskenmerken** selectievakje en kenmerk toevoegen.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-attributes.png)

1. Voor de **kenmerknaam** veld, typt u `account`. Voor de **kenmerkwaarde** veld, typt u in uw NetSuite account-ID. Deze waarde is een constante en wijzigen met account. Instructies voor het vinden van uw account-ID zijn hieronder opgenomen:

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-add-attribute.png)

    a. In NetSuite, klikt u op **Setup** navigeer vervolgens naar **bedrijf** en klikt u op **bedrijfsgegevens** van het bovenste navigatiemenu.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-com.png)

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-account-id.png)

    b. In de **bedrijfsgegevens** pagina in de rechterkolom kopie de **ACCOUNT-ID**.

    c. Plak de **Account-ID** die u hebt gekopieerd uit NetSuite-account in de **kenmerkwaarde** veld in Azure AD. 

1. Voordat gebruikers eenmalige aanmelding in NetSuite uitvoeren kunnen, moeten worden ze eerst de juiste machtigingen in NetSuite toegewezen. Volg de onderstaande instructies om deze machtigingen te wijzen.

    a. Klik op het bovenste navigatiemenu **Setup**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-setup.png)

    b. Selecteer in het navigatiemenu links **gebruikers/rollen**, klikt u vervolgens op **rollen beheren**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-manage-roles.png)

    c. Klik op **nieuwe rol**.

    d. Typ in een **naam** voor uw nieuwe rol.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-new-role.png)

    e. Klik op **Opslaan**.

    f. Klik in het menu aan de bovenkant op **machtigingen**. Klik vervolgens op **Setup**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-sso.png)

    g. Selecteer **SAML Single Sign-on**, en klik vervolgens op **toevoegen**.

    h. Klik op **Opslaan**.

    i. Klik op het bovenste navigatiemenu **Setup**, klikt u vervolgens op **Installatiebeheer**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-setup.png)

    j. Selecteer in het navigatiemenu links **gebruikers/rollen**, klikt u vervolgens op **gebruikers beheren**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-manage-users.png)

    k. Selecteer een testgebruiker. Klik vervolgens op **bewerken** en navigeer vervolgens naar **toegang** tabblad.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-edit-user.png)

    l. In het dialoogvenster rollen toewijzen de juiste rol die u hebt gemaakt.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/ns-add-role.png)

    m. Klik op **Opslaan**.
 
### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/NetSuite-tutorial/create_aaduser_01.png) 

1.  Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/NetSuite-tutorial/create_aaduser_02.png) 

1. Aan de bovenkant van het dialoogvenster, klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/NetSuite-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/NetSuite-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**. 

### <a name="creating-a-netsuite-test-user"></a>Het maken van een testgebruiker NetSuite

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in NetSuite. NetSuite biedt ondersteuning voor just-in-time inrichting, dat standaard is ingeschakeld.
Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in NetSuite bestaat, wordt een nieuw bestand wordt gemaakt wanneer u probeert te krijgen tot NetSuite.


### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan NetSuite.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan NetSuite toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **NetSuite**.

    ![Eenmalige aanmelding configureren](./media/NetSuite-tutorial/tutorial_NetSuite_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u wilt uw instellingen voor eenmalige aanmelding testen, opent u het deelvenster toegang [ https://myapps.microsoft.com ](https://myapps.microsoft.com/), meld u aan bij de testaccount en klikt u op **NetSuite**.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Inrichten van gebruikers configureren](NetSuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/NetSuite-tutorial/tutorial_general_01.png
[2]: ./media/NetSuite-tutorial/tutorial_general_02.png
[3]: ./media/NetSuite-tutorial/tutorial_general_03.png
[4]: ./media/NetSuite-tutorial/tutorial_general_04.png

[100]: ./media/NetSuite-tutorial/tutorial_general_100.png

[200]: ./media/NetSuite-tutorial/tutorial_general_200.png
[201]: ./media/NetSuite-tutorial/tutorial_general_201.png
[202]: ./media/NetSuite-tutorial/tutorial_general_202.png
[203]: ./media/NetSuite-tutorial/tutorial_general_203.png

