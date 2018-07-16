---
title: 'Zelfstudie: Azure Active Directory-integratie met UserVoice | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en UserVoice.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: 8ead383ef0967fcaf67f3157f0a51104126ad618
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39045407"
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Zelfstudie: Azure Active Directory-integratie met UserVoice

In deze zelfstudie leert u hoe u UserVoice integreren met Azure Active Directory (Azure AD).

UserVoice integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot UserVoice heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij UserVoice (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met UserVoice, moet u de volgende items:

- Een Azure AD-abonnement
- Een UserVoice eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. UserVoice uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-uservoice-from-the-gallery"></a>UserVoice uit de galerie toe te voegen
Voor het configureren van de integratie van UserVoice in Azure AD, moet u UserVoice uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen UserVoice uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **UserVoice**, selecteer **UserVoice** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![UserVoice in de lijst met resultaten](./media/uservoice-tutorial/tutorial_uservoice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met UserVoice op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD weten wat de gebruiker equivalent in UserVoice wordt aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in UserVoice tot stand worden gebracht.

In UserVoice, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met UserVoice, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker UserVoice](#create-a-uservoice-test-user)**  : als u wilt een equivalent van Britta Simon in UserVoice die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw UserVoice-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met UserVoice, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **UserVoice** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/uservoice-tutorial/tutorial_uservoice_samlbase.png)

3. Op de **UserVoice-domein en URL's** sectie, voert u de volgende stappen uit:

    ![UserVoice-domein en URL's, eenmalige aanmelding informatie](./media/uservoice-tutorial/tutorial_uservoice_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<tenantname>.UserVoice.com`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<tenantname>.UserVoice.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [UserVoice-Client-ondersteuningsteam](https://www.uservoice.com/) om deze waarden te verkrijgen.

4. Op de **SAML-handtekeningcertificaat** sectie, Kopieer de **VINGERAFDRUK** waarde van het certificaat.

    ![De downloadkoppeling certificaat](./media/uservoice-tutorial/tutorial_uservoice_certificate.png) 

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/uservoice-tutorial/tutorial_general_400.png)

6. Op de **UserVoice configuratie** sectie, klikt u op **configureren UserVoice** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![UserVoice-configuratie](./media/uservoice-tutorial/tutorial_uservoice_configure.png) 

7. In een ander browservenster aanmelden bij uw UserVoice-site bedrijf als beheerder.

8. Klik in de werkbalk bovenaan op **instellingen**, en selecteer vervolgens **webportal** in het menu.
   
    ![De sectie instellingen-App aan](./media/uservoice-tutorial/ic777519.png "instellingen")

9. Op de **webportal** tabblad, in de **gebruikersverificatie** sectie, klikt u op **bewerken** openen de **gebruikersverificatie bewerken** dialoogvenster pagina.
   
    ![Webportal tabblad](./media/uservoice-tutorial/ic777520.png "webportal")

10. Op de **gebruikersverificatie bewerken** dialoogvenster pagina, voert u de volgende stappen uit:
   
    ![Verificatie van de gebruiker bewerken](./media/uservoice-tutorial/ic777521.png "verificatie van de gebruiker bewerken")
   
    a. Klik op **Single Sign-On (SSO)**.
 
    b. Plakken de **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd vanuit de Azure portal in de **extern aanmelden SSO** tekstvak.

    c. Plakken de **afmelding URL** waarde die u hebt gekopieerd vanuit de Azure portal in de **SSO externe afmelding tekstvak**.
 
    d. Plak de **vingerafdruk** waarde die u hebt gekopieerd vanuit Azure portal in de **huidige certificaat SHA1-vingerafdruk** tekstvak.
    
    e. Klik op **verificatie-instellingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/uservoice-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/uservoice-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/uservoice-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/uservoice-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-uservoice-test-user"></a>Maak een testgebruiker UserVoice

Als u wilt dat Azure AD-gebruikers zich aanmelden met UserVoice, moeten ze worden ingericht in UserVoice. In het geval van UserVoice is inrichten een handmatige taak.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:
1. Meld u aan bij uw **UserVoice** tenant.

2. Ga naar **instellingen**.
   
    ![Instellingen voor](./media/uservoice-tutorial/ic777811.png "instellingen")

3. Klik op **algemene**.

4. Klik op **Agents en machtigingen**.
   
    ![Agents en machtigingen](./media/uservoice-tutorial/ic777812.png "Agents en machtigingen")

5. Klik op **beheerders toevoegen**.
   
    ![Beheerders toevoegen](./media/uservoice-tutorial/ic777813.png "beheerders toevoegen")

6. Op de **uitnodigen beheerders** dialoogvenster, voer de volgende stappen uit:
   
    ![Beheerders uitnodigen](./media/uservoice-tutorial/ic777814.png "beheerders uitnodigen")
   
    a. Typ het e-mailadres van het account dat u wilt inrichten, en klik vervolgens op in het tekstvak e-mailberichten **toevoegen**.
   
    b. Klik op **uitnodigen**.

> [!NOTE]
> U kunt alle andere UserVoice gebruiker-account maken van hulpprogramma's of API's geleverd door UserVoice aan inrichten AAD-gebruikersaccounts.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan UserVoice.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon met UserVoice, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **UserVoice**.

    ![De UserVoice-koppeling in de lijst met toepassingen](./media/uservoice-tutorial/tutorial_uservoice_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de UserVoice-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw UserVoice-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/uservoice-tutorial/tutorial_general_01.png
[2]: ./media/uservoice-tutorial/tutorial_general_02.png
[3]: ./media/uservoice-tutorial/tutorial_general_03.png
[4]: ./media/uservoice-tutorial/tutorial_general_04.png

[100]: ./media/uservoice-tutorial/tutorial_general_100.png

[200]: ./media/uservoice-tutorial/tutorial_general_200.png
[201]: ./media/uservoice-tutorial/tutorial_general_201.png
[202]: ./media/uservoice-tutorial/tutorial_general_202.png
[203]: ./media/uservoice-tutorial/tutorial_general_203.png

