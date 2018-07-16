---
title: 'Zelfstudie: Azure Active Directory-integratie met GoToMeeting | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en GoToMeeting.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bcaf19f2-5809-4e1c-acbc-21a8d3498ccf
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: jeedes
ms.openlocfilehash: b62b3b7f9f3bfd55237ed4d894954a0bde48e7fc
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043717"
---
# <a name="tutorial-azure-active-directory-integration-with-gotomeeting"></a>Zelfstudie: Azure Active Directory-integratie met GoToMeeting

In deze zelfstudie leert u hoe u GoToMeeting integreren met Azure Active Directory (Azure AD).

GoToMeeting integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot GoToMeeting heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij GoToMeeting (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met GoToMeeting, moet u de volgende items:

- Een Azure AD-abonnement
- Een GoToMeeting eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. GoToMeeting uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-gotomeeting-from-the-gallery"></a>GoToMeeting uit de galerie toe te voegen
Voor het configureren van de integratie van GoToMeeting in Azure AD, moet u GoToMeeting uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen GoToMeeting uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **GoToMeeting**, selecteer **GoToMeeting** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![GoToMeeting in de lijst met resultaten](./media/citrix-gotomeeting-tutorial/tutorial_gotomeeting_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met GoToMeeting op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in GoToMeeting is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in GoToMeeting tot stand worden gebracht.

In GoToMeeting, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met GoToMeeting, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker GoToMeeting](#create-a-gotomeeting-test-user)**  : als u wilt een equivalent van Britta Simon in GoToMeeting die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing GoToMeeting.

**Voor het configureren van Azure AD eenmalige aanmelding met GoToMeeting, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **GoToMeeting** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/citrix-gotomeeting-tutorial/tutorial_gotomeeting_samlbase.png)

3. Op de **GoToMeeting domein en URL's** sectie, voert u de volgende stappen uit:

    ![GoToMeeting domein en URL's, eenmalige aanmelding informatie](./media/citrix-gotomeeting-tutorial/tutorial_gotomeeting_url.png)

    In de **id** tekstvak typt u de URL: `https://authentication.logmeininc.com/saml/sp`

4. Klik op **geavanceerde URL weergeven configuratie** en configureert u de onderstaande URL's

    **Aanmeldings-URL** (laat dit leeg)
    
    **Antwoord-URL**: `https://authentication.logmeininc.com/saml/acs`
    
    **RelayState**:
    
    - Gebruik voor GoToMeeting-App `https://global.gotomeeting.com`
    
    - Gebruik voor GoToTraining, `https://global.gototraining.com`
    
    - Voor gotowebinar toevoegen, gebruikt u `https://global.gotowebinar.com` 
    
    - Gebruik voor GoToAssist, `https://app.gotoassist.com`
    
5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/citrix-gotomeeting-tutorial/tutorial_general_400.png)

6. In een ander browservenster en meld u aan bij uw [GoToMeeting organisatie Center](https://organization.logmeininc.com/). U wordt gevraagd om te bevestigen dat de id-provider is bijgewerkt

7. Schakel het selectievakje 'Mijn id-Provider is bijgewerkt met het nieuwe domein'. Klik op **gedaan** wanneer u klaar bent.


> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/citrix-gotomeeting-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/citrix-gotomeeting-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/citrix-gotomeeting-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/citrix-gotomeeting-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-gotomeeting-test-user"></a>Maak een testgebruiker GoToMeeting

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in GoToMeeting. GoToMeeting biedt ondersteuning voor just-in-time inrichting, dat standaard is ingeschakeld.

Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in GoToMeeting bestaat, wordt een nieuw bestand wordt gemaakt wanneer u probeert te krijgen tot GoToMeeting.

> [!NOTE]
> Als u wilt maken van een gebruiker handmatig, neem contact op met [GoToMeeting ondersteuningsteam](https://support.logmeininc.com/gotomeeting).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan GoToMeeting.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan GoToMeeting, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **GoToMeeting**.

    ![De koppeling GoToMeeting in de lijst met toepassingen](./media/citrix-gotomeeting-tutorial/tutorial_gotomeeting_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel GoToMeeting in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing GoToMeeting.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Inrichten van gebruikers configureren](https://docs.microsoft.com/azure/active-directory/active-directory-saas-citrixgotomeeting-provisioning-tutorial)


<!--Image references-->

[1]: ./media/gotomeeting-tutorial/tutorial_general_01.png
[2]: ./media/gotomeeting-tutorial/tutorial_general_02.png
[3]: ./media/gotomeeting-tutorial/tutorial_general_03.png
[4]: ./media/gotomeeting-tutorial/tutorial_general_04.png

[100]: ./media/gotomeeting-tutorial/tutorial_general_100.png

[200]: ./media/gotomeeting-tutorial/tutorial_general_200.png
[201]: ./media/gotomeeting-tutorial/tutorial_general_201.png
[202]: ./media/gotomeeting-tutorial/tutorial_general_202.png
[203]: ./media/gotomeeting-tutorial/tutorial_general_203.png

