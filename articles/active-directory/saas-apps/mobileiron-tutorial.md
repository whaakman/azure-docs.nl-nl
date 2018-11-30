---
title: 'Zelfstudie: Azure Active Directory-integratie met MobileIron | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en MobileIron.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 3e4bbd5b-290e-4951-971b-ec0c1c11aaa2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/9/2017
ms.author: jeedes
ms.openlocfilehash: 1b6527207793558c132be4cf004b7d6fdde14a90
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584108"
---
# <a name="tutorial-azure-active-directory-integration-with-mobileiron"></a>Zelfstudie: Azure Active Directory-integratie met MobileIron

In deze zelfstudie leert u hoe u MobileIron integreren met Azure Active Directory (Azure AD).

MobileIron integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot MobileIron heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij MobileIron (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met MobileIron, moet u de volgende items:

- Een Azure AD-abonnement
- Een MobileIron eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. MobileIron uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-mobileiron-from-the-gallery"></a>MobileIron uit de galerie toe te voegen
Voor het configureren van de integratie van MobileIron in Azure AD, moet u MobileIron uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen MobileIron uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

1. Typ in het zoekvak **MobileIron**, selecteer **MobileIron** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![MobileIron in de lijst met resultaten](./media/mobileiron-tutorial/tutorial_mobileiron_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met MobileIron op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in MobileIron is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in MobileIron tot stand worden gebracht.

In MobileIron, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met MobileIron, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Maak een testgebruiker MobileIron](#create-a-mobileiron-test-user)**  : als u wilt een equivalent van Britta Simon in MobileIron die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing MobileIron.

**Voor het configureren van Azure AD eenmalige aanmelding met MobileIron, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **MobileIron** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/mobileiron-tutorial/tutorial_mobileiron_samlbase.png)

1. Op de **MobileIron domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![MobileIron domein en URL's, eenmalige aanmelding informatie](./media/mobileiron-tutorial/tutorial_mobileiron_url.png)

    1. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://www.mobileiron.com/<key>`

    1. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<host>.mobileiron.com/saml/SSO/alias/<key>`

1. Controleer **geavanceerde URL-instellingen weergeven** en voer de volgende stap als u wilt configureren van de toepassing in **SP** modus gestart:

    ![MobileIron domein en URL's, eenmalige aanmelding](./media/mobileiron-tutorial/tutorial_mobileiron_url1.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<host>.mobileiron.com/user/login.html`
    
    > [!NOTE]  Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. U ontvangt de waarden van sleutel en de host van de beheerdersportal van MobileIron die later in de zelfstudie wordt uitgelegd.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/mobileiron-tutorial/tutorial_mobileiron_certificate.png) 

1. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/mobileiron-tutorial/tutorial_general_400.png)

1. In een ander browservenster aanmelden bij uw bedrijf MobileIron site als beheerder.

1. Ga naar **Admin** > **identiteit**.

   * Selecteer **AAD** optie in de **informatie over Cloud id-provider Setup** veld.

    ![Knop van de beheerder voor eenmalige aanmelding configureren](./media/mobileiron-tutorial/tutorial_mobileiron_admin.png)

1. Kopieer de waarden van **sleutel** en **Host** en plak ze voor het voltooien van de URL's in de **MobileIron domein en URL's** sectie in Azure portal.

    ![Knop van de beheerder voor eenmalige aanmelding configureren](./media/mobileiron-tutorial/key.png)

1. In de **metagegevensbestand exporteren uit AAD en importeer MobileIron Cloud veld** klikt u op **bestand kiezen** voor het uploaden van de metagegevens van de gedownloade vanuit Azure portal. Klik op **gedaan** eenmaal geüploade.
 
    ![Single Sign-On admin metagegevens knop configureren](./media/mobileiron-tutorial/tutorial_mobileiron_adminmetadata.png)

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/mobileiron-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/mobileiron-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/mobileiron-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/mobileiron-tutorial/create_aaduser_04.png)

    1. In de **naam** in het vak **BrittaSimon**.

    1. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    1. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    1. Klik op **Create**.
  
### <a name="create-a-mobileiron-test-user"></a>Maak een testgebruiker MobileIron

Als u wilt dat Azure AD-gebruikers zich aanmelden bij MobileIron, moeten ze worden ingericht voor MobileIron.  
In het geval van MobileIron is inrichten een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw bedrijf MobileIron site aan als beheerder.

1. Ga naar **gebruikers** en klikt u op **toevoegen** > **één gebruiker**.

    ![Knop Single Sign-On gebruiker configureren](./media/mobileiron-tutorial/tutorial_mobileiron_user.png)

1. Op de **'Één gebruiker'** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Configureren van eenmalige aanmelding voor gebruiker de knop toevoegen](./media/mobileiron-tutorial/tutorial_mobileiron_useradd.png)

    1. In **e-mailadres** tekst vak, voer het e-mailadres van gebruiker, zoals brittasimon@contoso.com.

    1. In **voornaam** tekst voert u de voornaam van de gebruiker, zoals Julia.

    1. In **achternaam** tekst voert u de achternaam van de gebruiker, zoals Simon.
    
    1. Klik op **Gereed**.  

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan MobileIron.

![De de gebruikersrol toewijzen][200] 

**Als u wilt Britta Simon aan MobileIron toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **MobileIron**.

    ![De koppeling MobileIron in de lijst met toepassingen](./media/mobileiron-tutorial/tutorial_mobileiron_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel MobileIron in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing MobileIron.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)


<!--Image references-->

[1]: ./media/mobileiron-tutorial/tutorial_general_01.png
[2]: ./media/mobileiron-tutorial/tutorial_general_02.png
[3]: ./media/mobileiron-tutorial/tutorial_general_03.png
[4]: ./media/mobileiron-tutorial/tutorial_general_04.png

[100]: ./media/mobileiron-tutorial/tutorial_general_100.png

[200]: ./media/mobileiron-tutorial/tutorial_general_200.png
[201]: ./media/mobileiron-tutorial/tutorial_general_201.png
[202]: ./media/mobileiron-tutorial/tutorial_general_202.png
[203]: ./media/mobileiron-tutorial/tutorial_general_203.png

