---
title: 'Zelfstudie: Azure Active Directory-integratie met Teamphoria | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Teamphoria.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2018
ms.author: jeedes
ms.openlocfilehash: 794945caeea113dc6f1cc2ab5e11a76c3e88c83e
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041905"
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a>Zelfstudie: Azure Active Directory-integratie met Teamphoria

In deze zelfstudie leert u hoe u Teamphoria integreren met Azure Active Directory (Azure AD).

Teamphoria integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Teamphoria heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Teamphoria (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Teamphoria, moet u de volgende items:

- Een Azure AD-abonnement
- Een Teamphoria eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Teamphoria uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-teamphoria-from-the-gallery"></a>Teamphoria uit de galerie toe te voegen
Voor het configureren van de integratie van Teamphoria in Azure AD, moet u Teamphoria uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Teamphoria uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure Portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Klik op **toevoegen** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Teamphoria**.

    ![Het maken van een Azure AD-testgebruiker](./media/teamphoria-tutorial/tutorial_teamphoria_search.png)

5. Selecteer in het deelvenster resultaten **Teamphoria**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Teamphoria op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Teamphoria is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Teamphoria tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Teamphoria, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Teamphoria](#creating-a-teamphoria-test-user)**  : als u wilt een equivalent van Britta Simon in Teamphoria die is gekoppeld aan de Azure AD-weergave van haar hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Teamphoria.

**Voor het configureren van Azure AD eenmalige aanmelding met Teamphoria, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Teamphoria** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. Op de **Teamphoria domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/teamphoria-tutorial/tutorial_teamphoria_url.png)

    In de **aanmeldings-URL** tekstvak typt u de URL met behulp van het volgende patroon: `https://<sub-domain>.teamphoria.com/login`   

    > [!NOTE] 
    > De aanmeldings-URL-waarde is niet echt. U moet deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [Teamphoria Client ondersteuningsteam](https://www.teamphoria.com/) om op te halen van de aanmeldings-URL.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en sla het certificaat op uw computer.

    ![Eenmalige aanmelding configureren](./media/teamphoria-tutorial/tutorial_teamphoria_certificate.png)

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/teamphoria-tutorial/tutorial_general_400.png)

6. Op de **Teamphoria configuratie** sectie, klikt u op **configureren Teamphoria** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/teamphoria-tutorial/tutorial_teamphoria_configure.png)

7. Het configureren van eenmalige aanmelding op **Teamphoria** aan clientzijde, meld u aan bij uw toepassing Teamphoria als beheerder.

8. Ga naar **BEHEERDERSINSTELLINGEN** optie in de werkbalk links en op het tabblad configureren klikt u op **EENMALIGE aanmelding** om de SSO-configuratie-venster te openen.

    ![Eenmalige aanmelding configureren](./media/teamphoria-tutorial/admin_sso_configure.png)

9. Klik op **nieuwe id-PROVIDER toevoegen** optie in de rechterbovenhoek om het formulier voor het toevoegen van de instellingen voor eenmalige aanmelding te openen.

    ![Eenmalige aanmelding configureren](./media/teamphoria-tutorial/add_new_identity_provider.png)

10. Voer de details in de velden, zoals beschreven onder-

    ![Eenmalige aanmelding configureren](./media/teamphoria-tutorial/Teamphoria_sso_save.png)

    a. **WEERGAVENAAM**: Voer de weergavenaam van de invoegtoepassing op de beheerpagina.

    b. **NAAM van de knop**: de naam van het tabblad dat wordt weergegeven op de aanmeldingspagina voor logboekregistratie via eenmalige aanmelding.

    c. **CERTIFICAAT**: Open het certificaat eerder hebt gedownload vanuit Azure portal in Kladblok en kopieer de inhoud van dezelfde en plak deze hier in het vak.

    d. **TOEGANGSPUNT**: plak de **Single Sign-On Service URL voor SAML** eerder hebt gekopieerd vanuit Azure portal.

    e. Schakel de optie voor het **ON** en klikt u op **opslaan**.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/teamphoria-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    ![Het maken van een Azure AD-testgebruiker](./media/teamphoria-tutorial/create_aaduser_02.png) 

3. Aan de bovenkant van het dialoogvenster klikt u op **toevoegen** openen de **gebruiker** dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/teamphoria-tutorial/create_aaduser_03.png)

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/teamphoria-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.

### <a name="creating-a-teamphoria-test-user"></a>Het maken van een testgebruiker Teamphoria

Als u wilt inschakelen in Azure AD-gebruikers zich aanmelden bij Teamphoria, moeten ze worden ingericht voor Teamphoria. In het geval van Teamphoria is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf Teamphoria site aan als beheerder.

2. Klik op **ADMIN** instellingen op de werkbalk links en klikt u onder de **beheren** Klik op tabblad **gebruikers** openen de beheerpagina voor gebruikers.

    ![Werknemer toevoegen](./media/teamphoria-tutorial/admin_manage_users.png)

3. Klik op de **handmatig UITNODIGEN** optie.

    ![Anderen uitnodigen](./media/teamphoria-tutorial/admin_manage_add_users.png)

4. Op deze pagina uitvoeren na de actie.
    
    ![Anderen uitnodigen](./media/teamphoria-tutorial/manual_user_invite.png)

    a. In de **e-MAILADRES** tekstvak de **e-mailadres** van BrittaSimon.

    b. In de **VOORNAAM** tekstvak, type **Julia**.

    c. In de **ACHTERNAAM** tekstvak, type **Simon**.

    d. Klik op **uitnodiging 1 gebruiker**. Gebruiker moet de uitdaging aannemen farmherstelpunten gemaakt in het systeem.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Teamphoria.

![Gebruiker toewijzen][200]

**Als u wilt Britta Simon aan Teamphoria toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Teamphoria**.

    ![Eenmalige aanmelding configureren](./media/teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/teamphoria-tutorial/tutorial_general_01.png
[2]: ./media/teamphoria-tutorial/tutorial_general_02.png
[3]: ./media/teamphoria-tutorial/tutorial_general_03.png
[4]: ./media/teamphoria-tutorial/tutorial_general_04.png

[100]: ./media/teamphoria-tutorial/tutorial_general_100.png

[200]: ./media/teamphoria-tutorial/tutorial_general_200.png
[201]: ./media/teamphoria-tutorial/tutorial_general_201.png
[202]: ./media/teamphoria-tutorial/tutorial_general_202.png
[203]: ./media/teamphoria-tutorial/tutorial_general_203.png
