---
title: 'Zelfstudie: Azure Active Directory-integratie met Qumu Cloud | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Qumu Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d8c4a97b-4de6-49d4-b64e-42222c2ec6c9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2018
ms.author: jeedes
ms.openlocfilehash: efa2c421b405b11acb605106bc14ccc76f1a2149
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051629"
---
# <a name="tutorial-azure-active-directory-integration-with-qumu-cloud"></a>Zelfstudie: Azure Active Directory-integratie met Qumu Cloud

In deze zelfstudie leert u hoe u Qumu Cloud integreren met Azure Active Directory (Azure AD).

Qumu Cloud integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Qumu Cloud heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij de Qumu Cloud (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Qumu Cloud, moet u de volgende items:

- Een Azure AD-abonnement
- Een Qumu Cloud eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Qumu Cloud uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-qumu-cloud-from-the-gallery"></a>Qumu Cloud uit de galerie toe te voegen
Voor het configureren van de integratie van Qumu Cloud in Azure AD, moet u Qumu Cloud uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Qumu Cloud uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Qumu Cloud**, selecteer **Qumu Cloud** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Qumu Cloud in de lijst met resultaten](./media/qumucloud-tutorial/tutorial_qumucloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie configureert u en test Azure AD eenmalige aanmelding met Qumu Cloud op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de Qumu Cloud is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de Qumu Cloud tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Qumu Cloud, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Qumu Cloud](#create-a-qumu-cloud-test-user)**  : als u wilt een equivalent van Britta Simon in Qumu Cloud die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Qumu Cloud.

**Voor het configureren van Azure AD eenmalige aanmelding met Qumu Cloud, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Qumu Cloud** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/qumucloud-tutorial/tutorial_qumucloud_samlbase.png)

3. Op de **Qumu Cloud domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Qumu Cloud domein en URL's, eenmalige aanmelding informatie](./media/qumucloud-tutorial/tutorial_qumucloud_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.qumucloud.com/saml/SSO`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.qumucloud.com/saml/SSO`

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Qumu Cloud domein en URL's, eenmalige aanmelding informatie](./media/qumucloud-tutorial/tutorial_qumucloud_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.qumucloud.com`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [Qumu Cloud Client ondersteuningsteam](mailto:support@qumu.com) om deze waarden te verkrijgen.

4. Qumu cloudtoepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/qumucloud-tutorial/attribute.png)
    
5. Klik op **weergeven en bewerken van alle andere gebruikerskenmerken** selectievakje in de **gebruikerskenmerken** sectie om uit te breiden de kenmerken. De volgende stappen uitvoeren op elk van de kenmerken weergegeven:

    | Naam kenmerk | Waarde kenmerk |
    | ---------------| --------------- |    
    | urn: oid:2.5.4.42 | User.givenName |
    | urn: oid:2.5.4.4 | User.surname |
    | urn:oid:0.9.2342.19200300.100.1.3 | User.mail |
    | urn:oid:0.9.2342.19200300.100.1.1 | User.userPrincipalName |

    a. Klik op het kenmerk te openen de **kenmerk bewerken** venster.

    ![Eenmalige aanmelding configureren](./media/qumucloud-tutorial/tutorial_attribute_04.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.

    ![Eenmalige aanmelding configureren](./media/qumucloud-tutorial/tutorial_attribute_05.png)

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Houd de **Namespace** tekstvak leeg.
    
    e. Klik op **OK**.

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/qumucloud-tutorial/tutorial_qumucloud_certificate.png) 

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/qumucloud-tutorial/tutorial_general_400.png)
    
7. Het configureren van eenmalige aanmelding op **Qumu Cloud** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [Qumu Cloud-ondersteuningsteam](mailto:support@qumu.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/qumucloud-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/qumucloud-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/qumucloud-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/qumucloud-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-qumu-cloud-test-user"></a>Maak een testgebruiker Qumu Cloud

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Qumu Cloud. Qumu Cloud biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot Qumu Cloud als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [Qumu Cloud Client ondersteuningsteam](mailto:support@qumu.com).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Qumu Cloud.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Qumu Cloud, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Qumu Cloud**.

    ![De koppeling Qumu Cloud in de lijst met toepassingen](./media/qumucloud-tutorial/tutorial_qumucloud_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Qumu Cloud in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Qumu Cloud.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/qumucloud-tutorial/tutorial_general_01.png
[2]: ./media/qumucloud-tutorial/tutorial_general_02.png
[3]: ./media/qumucloud-tutorial/tutorial_general_03.png
[4]: ./media/qumucloud-tutorial/tutorial_general_04.png

[100]: ./media/qumucloud-tutorial/tutorial_general_100.png

[200]: ./media/qumucloud-tutorial/tutorial_general_200.png
[201]: ./media/qumucloud-tutorial/tutorial_general_201.png
[202]: ./media/qumucloud-tutorial/tutorial_general_202.png
[203]: ./media/qumucloud-tutorial/tutorial_general_203.png

