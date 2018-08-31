---
title: 'Zelfstudie: Azure Active Directory-integratie met Comeet werving Software | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Comeet werving Software.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 75f51dc9-9525-4ec6-80bf-28374f0c8adf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2018
ms.author: jeedes
ms.openlocfilehash: 137ba7a7e82ff3e57d862868859e8049838701a3
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307750"
---
# <a name="tutorial-azure-active-directory-integration-with-comeet-recruiting-software"></a>Zelfstudie: Azure Active Directory-integratie met Comeet werving Software

In deze zelfstudie leert u hoe u Comeet werving om Software te integreren met Azure Active Directory (Azure AD).

Comeet werving Software integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Comeet werving Software heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Comeet werving Software (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Comeet werven van Software, moet u de volgende items:

- Een Azure AD-abonnement
- Een Comeet werving Software eenmalige aanmelding ingeschakeld abonnement

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Comeet werving Software uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-comeet-recruiting-software-from-the-gallery"></a>Comeet werving Software uit de galerie toe te voegen

Voor het configureren van de integratie van Comeet werving Software in Azure AD, moet u Comeet werving Software uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Comeet werving Software uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Comeet werving Software**, selecteer **Comeet werving Software** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Comeet werving Software in de lijst met resultaten](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Comeet werven van Software op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Comeet werving Software is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Comeet werving Software tot stand worden gebracht.

Als u wilt configureren en Azure AD eenmalige aanmelding met Comeet werving Software testen, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker Comeet werving Software](#create-a-comeet-recruiting-software-test-user)**  : als u wilt een equivalent van Britta Simon in Comeet werven van Software die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Comeet werving Software.

**Voor het configureren van Azure AD eenmalige aanmelding met Comeet werven van Software, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Comeet werving Software** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![In het dialoogvenster voor eenmalige aanmelding](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_samlbase.png)

3. Op de **Comeet werving Software domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Comeet domein en URL's, eenmalige aanmelding informatie](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_url1.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://app.comeet.co/adfs_auth/acs/<UNIQUEID>/`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://app.comeet.co/adfs_auth/acs/<UNIQUEID>/`

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id en antwoord-URL. Ontvangt u deze waarden van de portal Comeet werving Software zoals wordt weergegeven in de [ondersteuningspagina](https://support.comeet.co/knowledgebase/adfs-single-sign-on/).

4. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Werving Software domein en URL's één aanmelding informatie comeet](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_url2.png)

    In de **aanmeldings-URL** tekstvak typt u de URL: `https://app.comeet.co`

5. Comeet werving softwaretoepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. De volgende Schermafbeelding toont een voorbeeld voor deze. De standaardwaarde van **gebruikers-id** is **user.userprincipalname** maar **Comeet werving Software** wordt verwacht dat deze optie om te worden toegewezen met de e-mailadres van de gebruiker. Hiervoor kunt u **user.mail** kenmerk in de lijst of gebruik de waarde van het juiste kenmerk op basis van de organisatieconfiguratie van uw.

    ![Eenmalige aanmelding configureren](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_attribute.png)

6. Klik op **weergeven en bewerken van alle andere gebruikerskenmerken** selectievakje in de **gebruikerskenmerken** sectie om uit te breiden de kenmerken. De volgende stappen uitvoeren op elk van de kenmerken weergegeven:

    | Naam kenmerk | Waarde kenmerk |
    | ---------------| --------------- |
    | comeet_id | User.userPrincipalName |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/comeetrecruitingsoftware-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/comeetrecruitingsoftware-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak, type de **kenmerknaam** wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Klik op **OK**.

7. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_certificate.png)

8. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/comeetrecruitingsoftware-tutorial/tutorial_general_400.png)

9. Het configureren van eenmalige aanmelding op **Comeet werving Software** zijde, plak de inhoud van de gedownloade Metadata-XML in Comeet werven van Software, zoals wordt weergegeven in de [ondersteuningspagina](https://support.comeet.co/knowledgebase/adfs-single-sign-on/).

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/comeetrecruitingsoftware-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/comeetrecruitingsoftware-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/comeetrecruitingsoftware-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/comeetrecruitingsoftware-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.

### <a name="create-a-comeet-recruiting-software-test-user"></a>Maak een testgebruiker Comeet werving Software

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Comeet werving Software. Werken met [Comeet werven van Software-ondersteuningsteam](mailto:support@comeet.co) om toe te voegen de gebruikers in de Comeet werven van Software-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot Comeet werving Software.

![De de gebruikersrol toewijzen][200]

**Als u wilt toewijzen Britta Simon Comeet werven van software, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Comeet werving Software**.

    ![De koppeling Comeet werving Software in de lijst met toepassingen](./media/comeetrecruitingsoftware-tutorial/tutorial_comeetrecruitingsoftware_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Comeet werving Software in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Comeet werving Software.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_01.png
[2]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_02.png
[3]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_03.png
[4]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_04.png

[100]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_100.png

[200]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_200.png
[201]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_201.png
[202]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_202.png
[203]: ./media/comeetrecruitingsoftware-tutorial/tutorial_general_203.png