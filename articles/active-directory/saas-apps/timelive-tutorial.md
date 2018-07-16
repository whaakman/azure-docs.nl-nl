---
title: 'Zelfstudie: Azure Active Directory-integratie met TimeLive | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TimeLive.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 34123629-4ad5-465c-a4c1-8299f857e720
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: jeedes
ms.openlocfilehash: 4cf5bcc3fe803212b01f14c950b97b3bb16b13a5
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041249"
---
# <a name="tutorial-azure-active-directory-integration-with-timelive"></a>Zelfstudie: Azure Active Directory-integratie met TimeLive

In deze zelfstudie leert u hoe u TimeLive integreren met Azure Active Directory (Azure AD).

TimeLive integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TimeLive heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij TimeLive (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TimeLive, moet u de volgende items:

- Een Azure AD-abonnement
- Een TimeLive eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TimeLive uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-timelive-from-the-gallery"></a>TimeLive uit de galerie toe te voegen
Voor het configureren van de integratie van TimeLive in Azure AD, moet u TimeLive uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen TimeLive uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **TimeLive**, selecteer **TimeLive** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![TimeLive in de lijst met resultaten](./media/timelive-tutorial/tutorial_timelive_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met TimeLive op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in TimeLive is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in TimeLive tot stand worden gebracht.

In TimeLive, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TimeLive, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker TimeLive](#create-a-timelive-test-user)**  : als u wilt een equivalent van Britta Simon in TimeLive die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing TimeLive.

**Voor het configureren van Azure AD eenmalige aanmelding met TimeLive, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **TimeLive** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/timelive-tutorial/tutorial_timelive_samlbase.png)

3. Op de **TimeLive domein en URL's** sectie, voert u de volgende stappen uit:

    ![TimeLive domein en URL's, eenmalige aanmelding informatie](./media/timelive-tutorial/tutorial_timelive_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://domainname.livetecs.com/`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://domainname.livetecs.com/`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke id en de aanmeldings-URL. Neem contact op met [TimeLive Client ondersteuningsteam](mailto:support@livetecs.com) om deze waarden te verkrijgen. 

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/timelive-tutorial/tutorial_timelive_certificate.png) 

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/timelive-tutorial/tutorial_general_400.png)
    
6. Op de **TimeLive configuratie** sectie, klikt u op **configureren TimeLive** openen **aanmelding configureren** venster. Kopiëren de **afmelding URL's en SAML Single Sign-On Service** uit de **Naslaggids sectie.**

    ![TimeLive configuratie](./media/timelive-tutorial/tutorial_timelive_configure.png)

7. In een ander browservenster aanmelden bij uw bedrijf TimeLive site als beheerder.

8. Selecteer **voorkeuren** onder **beheeropties**.

    ![TimeLive configuratie](./media/timelive-tutorial/configure1.png)

9. In de **toepassing voorkeur** sectie, voert u de volgende stappen uit:
    
    ![TimeLive configuratie](./media/timelive-tutorial/configure2.png)

    a. Selecteer **Security** tabblad.

    b. Controleer **Schakel eenmalige aanmelding (SSO)** selectievakje.

    c. Selecteer **SAML** in de vervolgkeuzelijst met kop **Meld u aan met behulp van eenmalige aanmelding (SSO) met**.

    d. In de **URL voor SAML SSO-**, plakken **Single Sign-On Service URL voor SAML** waarde die u hebt gekopieerd, vormen de Azure-portal.

    e. In de **externe afmeldings-URL van**, plakken **afmelding URL** waarde die u hebt gekopieerd, vormen de Azure-portal.

    f. Open het gedownloade **base-64 gecodeerde certificaat** vanuit Azure portal in Kladblok, Kopieer de inhoud en plak deze in de **X.509-certificaat** tekstvak.

    g. Klik op **Update**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/timelive-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/timelive-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/timelive-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/timelive-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-timelive-test-user"></a>Maak een testgebruiker TimeLive

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in TimeLive. TimeLive biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot TimeLive als deze nog niet bestaat.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan TimeLive.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan TimeLive toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **TimeLive**.

    ![De koppeling TimeLive in de lijst met toepassingen](./media/timelive-tutorial/tutorial_timelive_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel TimeLive in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing TimeLive.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/timelive-tutorial/tutorial_general_01.png
[2]: ./media/timelive-tutorial/tutorial_general_02.png
[3]: ./media/timelive-tutorial/tutorial_general_03.png
[4]: ./media/timelive-tutorial/tutorial_general_04.png

[100]: ./media/timelive-tutorial/tutorial_general_100.png

[200]: ./media/timelive-tutorial/tutorial_general_200.png
[201]: ./media/timelive-tutorial/tutorial_general_201.png
[202]: ./media/timelive-tutorial/tutorial_general_202.png
[203]: ./media/timelive-tutorial/tutorial_general_203.png

