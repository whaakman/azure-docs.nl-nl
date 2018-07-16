---
title: 'Zelfstudie: Azure Active Directory-integratie met ondersteuning voor externe Bomgar | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ondersteuning voor externe Bomgar.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 193b163f-bdee-4974-b16d-777c51b991df
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: jeedes
ms.openlocfilehash: c59f4291726b24b7c96bb60d0497c1578a3e4b0f
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048178"
---
# <a name="tutorial-azure-active-directory-integration-with-bomgar-remote-support"></a>Zelfstudie: Azure Active Directory-integratie met ondersteuning voor externe Bomgar

In deze zelfstudie leert u hoe u ondersteuning voor externe Bomgar integreren met Azure Active Directory (Azure AD).

Ondersteuning voor externe Bomgar integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ondersteuning voor externe Bomgar heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij ondersteuning voor externe Bomgar (Single Sign-On) inschakelen met hun Azure AD-accounts.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met externe Bomgar-ondersteuning, moet u de volgende items:

- Een Azure AD-abonnement
- Een externe ondersteuning Bomgar eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Ondersteuning voor externe Bomgar uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-bomgar-remote-support-from-the-gallery"></a>Ondersteuning voor externe Bomgar uit de galerie toevoegen
Voor het configureren van de integratie van externe Bomgar-ondersteuning in Azure AD, moet u ondersteuning voor externe Bomgar uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Om toe te voegen ondersteuning voor externe Bomgar uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **ondersteuning voor externe Bomgar**, selecteer **Bomgar-ondersteuning voor externe** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Ondersteuning voor externe Bomgar in de lijst met resultaten](./media/bomgarremotesupport-tutorial/tutorial_bomgarremotesupport_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met ondersteuning voor externe Bomgar op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in ondersteuning voor externe Bomgar is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ondersteuning voor externe Bomgar tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met de ondersteuning voor externe Bomgar, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker ondersteuning voor externe Bomgar](#create-a-bomgar-remote-support-test-user)**  : als u wilt een equivalent van Britta Simon in Bomgar externe ondersteuning die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing ondersteuning voor externe Bomgar.

**Voor het configureren van Azure AD eenmalige aanmelding met de ondersteuning voor externe Bomgar, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ondersteuning voor externe Bomgar** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/bomgarremotesupport-tutorial/tutorial_bomgarremotesupport_samlbase.png)

3. Op de **Bomgar externe ondersteuning van domein en URL's** sectie, voert u de volgende stappen uit:

    ![Bomgar externe ondersteuning van domein en URL's, eenmalige aanmelding informatie](./media/bomgarremotesupport-tutorial/tutorial_bomgarremotesupport_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<SUBDOMAIN>.trafficmanager.net/saml`

    b. In de **id (entiteits-ID)** tekstvak, een URL met behulp van het volgende patroon: `https://<SUBDOMAIN>.trafficmanager.net`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id (entiteits-ID). Neem contact op met [Bomgar externe ondersteuning voor Client-ondersteuningsteam](https://www.bomgar.com/docs/index.htm#support) om deze waarden te verkrijgen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/bomgarremotesupport-tutorial/tutorial_bomgarremotesupport_certificate.png) 

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/bomgarremotesupport-tutorial/tutorial_general_400.png)

6. Het configureren van eenmalige aanmelding op **ondersteuning voor externe Bomgar** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [ondersteuningsteam Bomgar externe ondersteuning](https://www.bomgar.com/docs/index.htm#support). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/bomgarremotesupport-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/bomgarremotesupport-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/bomgarremotesupport-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/bomgarremotesupport-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-bomgar-remote-support-test-user"></a>Maak een testgebruiker Bomgar externe ondersteuning

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in ondersteuning voor externe Bomgar. Ondersteuning voor externe Bomgar biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot ondersteuning voor externe Bomgar als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [ondersteuningsteam ondersteuning voor externe Bomgar](https://www.bomgar.com/docs/index.htm#support).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan ondersteuning voor externe Bomgar.

![De de gebruikersrol toewijzen][200] 

**Als u wilt toewijzen Britta Simon Bomgar externe ondersteuning, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **ondersteuning voor externe Bomgar**.

    ![De ondersteuning voor externe Bomgar-koppeling in de lijst met toepassingen](./media/bomgarremotesupport-tutorial/tutorial_bomgarremotesupport_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Bomgar externe ondersteuning in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing ondersteuning voor externe Bomgar.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bomgarremotesupport-tutorial/tutorial_general_01.png
[2]: ./media/bomgarremotesupport-tutorial/tutorial_general_02.png
[3]: ./media/bomgarremotesupport-tutorial/tutorial_general_03.png
[4]: ./media/bomgarremotesupport-tutorial/tutorial_general_04.png

[100]: ./media/bomgarremotesupport-tutorial/tutorial_general_100.png

[200]: ./media/bomgarremotesupport-tutorial/tutorial_general_200.png
[201]: ./media/bomgarremotesupport-tutorial/tutorial_general_201.png
[202]: ./media/bomgarremotesupport-tutorial/tutorial_general_202.png
[203]: ./media/bomgarremotesupport-tutorial/tutorial_general_203.png

