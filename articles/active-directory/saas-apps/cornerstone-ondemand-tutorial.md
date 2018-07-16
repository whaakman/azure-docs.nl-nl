---
title: 'Zelfstudie: Azure Active Directory-integratie met hoeksteen OnDemand | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en hoeksteen OnDemand.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f57c5fef-49b0-4591-91ef-fc0de6d654ab
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: jeedes
ms.openlocfilehash: cfbf431f8fa483b9a34beb71d9b1dbe3e691cdba
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39053239"
---
# <a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Zelfstudie: Azure Active Directory-integratie met hoeksteen OnDemand

In deze zelfstudie leert u hoe u hoeksteen OnDemand integreren met Azure Active Directory (Azure AD).

Hoeksteen OnDemand integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de hoeksteen OnDemand heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij de hoekstenen OnDemand (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met hoeksteen OnDemand, moet u de volgende items:

- Een Azure AD-abonnement
- Een OnDemand-hoeksteen eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Hoeksteen OnDemand uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Hoeksteen OnDemand uit de galerie toe te voegen
Voor het configureren van de integratie van hoeksteen OnDemand in Azure AD, moet u hoeksteen OnDemand uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen hoeksteen OnDemand uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **hoeksteen OnDemand**.

    ![Het maken van een Azure AD-testgebruiker](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_search.png)

5. Selecteer in het deelvenster resultaten **hoeksteen OnDemand**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met hoeksteen OnDemand op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in hoeksteen OnDemand is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in hoeksteen OnDemand tot stand worden gebracht.

In de hoeksteen OnDemand, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met hoeksteen OnDemand, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker hoeksteen OnDemand](#creating-a-cornerstone-ondemand-test-user)**  : als u wilt een equivalent van Britta Simon in hoeksteen OnDemand die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing hoeksteen OnDemand.

**Voor het configureren van Azure AD eenmalige aanmelding met hoeksteen OnDemand, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **hoeksteen OnDemand** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_samlbase.png)

3. Op de **hoeksteen OnDemand domein en URL's** sectie, voert u de volgende stap:

    ![Eenmalige aanmelding configureren](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<company>.csod.com`

    b. In **id** tekstvak, een URL met behulp van het volgende patroon: `https://<company>.csod.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [hoeksteen OnDemand Client ondersteuningsteam](mailTo:moreinfo@csod.com) om deze waarden te verkrijgen.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/cornerstone-ondemand-tutorial/tutorial_general_400.png)

6. Op de **hoeksteen OnDemand configuratie** sectie, klikt u op **configureren hoeksteen OnDemand** openen **aanmelding configureren** venster. Kopiëren de **afmelding URL's en SAML Single Sign-On Service** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_configure.png) 

7. Het configureren van eenmalige aanmelding op **hoeksteen OnDemand** zijde, moet u voor het verzenden van de gedownloade **certificaat**, **afmelding URL** en **SAML Single Sign-On Service-URL** naar [hoeksteen OnDemand-ondersteuningsteam](mailTo:moreinfo@csod.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/cornerstone-ondemand-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.

    ![Het maken van een Azure AD-testgebruiker](./media/cornerstone-ondemand-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.

    ![Het maken van een Azure AD-testgebruiker](./media/cornerstone-ondemand-tutorial/create_aaduser_03.png)

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Het maken van een Azure AD-testgebruiker](./media/cornerstone-ondemand-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.

### <a name="creating-a-cornerstone-ondemand-test-user"></a>Het maken van een testgebruiker hoeksteen OnDemand

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in hoeksteen OnDemand. Hoeksteen OnDemand ondersteunt automatisch gebruikers inrichten, dit is standaard ingeschakeld. Meer informatie vindt u [hier](cornerstone-ondemand-provisioning-tutorial.md) voor het automatisch inrichten van gebruikers configureren.

**Als u moet de gebruiker handmatig hebt gemaakt, voert u de volgende stappen uit:**

Als u wilt configureren met het inrichten van gebruikers, gegevens worden verzonden (bijvoorbeeld: naam, e-) over de Azure AD-gebruiker u wilt inrichten op de [hoeksteen OnDemand-ondersteuningsteam](mailTo:moreinfo@csod.com).

>[!NOTE]
>Kunt u een andere OnDemand hoeksteen gebruiker account hulpmiddelen voor het maken of API's die door de hoeksteen OnDemand inrichten AAD-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan hoeksteen OnDemand.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan hoeksteen OnDemand toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **hoeksteen OnDemand**.

    ![Eenmalige aanmelding configureren](./media/cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel hoeksteen OnDemand in het toegangsvenster, u moet u automatisch aangemeld bij uw hoeksteen OnDemand-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Inrichten van gebruikers configureren](cornerstone-ondemand-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/cornerstone-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/cornerstone-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/cornerstone-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/cornerstone-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/cornerstone-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/cornerstone-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/cornerstone-ondemand-tutorial/tutorial_general_203.png
