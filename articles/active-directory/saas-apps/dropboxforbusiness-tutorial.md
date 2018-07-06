---
title: 'Zelfstudie: Azure Active Directory-integratie met Dropbox voor bedrijven | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Dropbox voor bedrijven.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: jeedes
ms.openlocfilehash: 3423f723b3f4857db44c609cd0e2143b6b04653e
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868626"
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Zelfstudie: Azure Active Directory-integratie met Dropbox voor bedrijven

In deze zelfstudie leert u hoe u Dropbox voor bedrijven integreren met Azure Active Directory (Azure AD).

Dropbox voor bedrijven integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Dropbox voor bedrijven heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Dropbox voor bedrijven (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Dropbox voor bedrijven, moet u de volgende items:

- Een Azure AD-abonnement
- Een Dropbox voor bedrijven eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Dropbox voor bedrijven uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-dropbox-for-business-from-the-gallery"></a>Dropbox voor bedrijven uit de galerie toe te voegen
Voor het configureren van de integratie van Dropbox voor bedrijven in Azure AD, moet u Dropbox voor bedrijven uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Dropbox voor bedrijven uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Dropbox voor bedrijven**, selecteer **Dropbox voor bedrijven** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Dropbox voor bedrijven in de lijst met resultaten](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Dropbox voor bedrijven op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Dropbox voor bedrijven is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Dropbox voor bedrijven tot stand worden gebracht.

In Dropbox voor bedrijven en wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Dropbox voor bedrijven, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een Dropbox voor bedrijven-testgebruiker](#create-a-dropbox-for-business-test-user)**  : als u wilt een equivalent van Britta Simon in Dropbox voor bedrijven die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Dropbox voor Business-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Dropbox voor bedrijven, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Dropbox voor bedrijven** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. Op de **Dropbox voor bedrijven-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Dropbox voor bedrijven-domein en URL's één aanmeldings-informatie](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url1.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://www.dropbox.com/sso/<id>`

    b. In de **id** tekstvak, een waarde: `Dropbox`

    > [!NOTE] 
    > De bovenstaande aanmeldings-URL-waarde is geen echte waarde. U kunt de waarde wordt bijgewerkt met de werkelijke aanmeldings-URL, die later in de zelfstudie wordt uitgelegd. Neem contact op met [Dropbox voor bedrijven-Client-ondersteuningsteam](https://www.dropbox.com/business/contact) om de waarde. 
 

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/dropboxforbusiness-tutorial/tutorial_general_400.png)

6. Op de **Dropbox voor bedrijven-configuratie** sectie, klikt u op **Dropbox voor bedrijven configureren** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Dropbox voor bedrijven-configuratie](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. Het configureren van eenmalige aanmelding op **Dropbox voor bedrijven** zijde, gaat u in uw Dropbox voor bedrijven-tenant.

    a. Meld u aan bij uw Dropbox voor bedrijven-tenant. 
   
    ![Configureren van eenmalige aanmelding](./media/dropboxforbusiness-tutorial/ic769509.png "eenmalige aanmelding configureren")
   
    b. Klik in het navigatiedeelvenster aan de linkerkant op **-beheerconsole**. 
   
    ![Configureren van eenmalige aanmelding](./media/dropboxforbusiness-tutorial/ic769510.png "eenmalige aanmelding configureren")
   
    c. Op de **-beheerconsole**, klikt u op **verificatie** in het navigatiedeelvenster links. 
   
    ![Configureren van eenmalige aanmelding](./media/dropboxforbusiness-tutorial/ic769511.png "eenmalige aanmelding configureren")
   
    d. In de **eenmalige aanmelding** sectie, selecteer **eenmalige aanmelding inschakelen**, en klik vervolgens op **meer** om uit te vouwen in deze sectie.  
   
    ![Configureren van eenmalige aanmelding](./media/dropboxforbusiness-tutorial/ic769512.png "eenmalige aanmelding configureren")
   
    e. Kopieer de URL naast **gebruikers kunnen zich aanmelden door te voeren van het e-mailadres of kunnen ze rechtstreeks naar** en plak deze in de **aanmeldings-URL** tekstvak van **Dropbox voor bedrijven-domein en URL's** sectie in Azure portal. 
    
    ![Eenmalige aanmelding configureren](./media/dropboxforbusiness-tutorial/ic769513.png)
    
8. In de **eenmalige aanmelding** sectie van de **verificatie** pagina, voert u de volgende stappen uit: 
   
    ![Configureren van eenmalige aanmelding](./media/dropboxforbusiness-tutorial/IC769516.png "eenmalige aanmelding configureren")
   
    a. Klik op **vereist**.
   
    b. In de **aanmelden URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.

    c. Klik op **certificaat kiezen**, en blader vervolgens naar uw **Base64-gecodeerd certificaatbestand**.

    d. Klik op **wijzigingen opslaan** om de configuratie in uw DropBox voor bedrijven-tenant te voltooien.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/dropboxforbusiness-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/dropboxforbusiness-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/dropboxforbusiness-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/dropboxforbusiness-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-dropbox-for-business-test-user"></a>Een Dropbox voor bedrijven-testgebruiker maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Dropbox voor bedrijven. Dropbox voor bedrijven biedt ondersteuning voor just-in-time inrichting, dat standaard is ingeschakeld.

Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in Dropbox voor bedrijven bestaat, wordt een nieuwe resourcegroep wordt gemaakt wanneer u probeert te krijgen tot Dropbox voor bedrijven.

>[!Note]
>Als u wilt maken van een gebruiker handmatig, neem contact op met [Dropbox voor bedrijven-Client-ondersteuningsteam](https://www.dropbox.com/business/contact) 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Dropbox voor bedrijven.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Dropbox voor bedrijven, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Dropbox voor bedrijven**.

    ![De Dropbox voor bedrijven-koppeling in de lijst met toepassingen](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u de Dropbox voor bedrijven-tegel in het toegangsvenster klikt, krijgt u de aanmeldingspagina dropbox voor Business-toepassing.
 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/dropboxforbusiness-tutorial/tutorial_general_203.png

