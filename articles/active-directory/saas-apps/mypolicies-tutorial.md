---
title: 'Zelfstudie: Azure Active Directory-integratie met myPolicies | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en myPolicies.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: bf79e858-1dfb-4ab3-a6df-74b2d5a878d2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: jeedes
ms.openlocfilehash: 120acc30c968670cd81fac4c45e246991f5f9c81
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39046223"
---
# <a name="tutorial-azure-active-directory-integration-with-mypolicies"></a>Zelfstudie: Azure Active Directory-integratie met myPolicies

In deze zelfstudie leert u hoe u myPolicies integreren met Azure Active Directory (Azure AD).

MyPolicies integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot myPolicies heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij myPolicies (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met myPolicies, moet u de volgende items:

- Een Azure AD-abonnement
- Een myPolicies eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. MyPolicies uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-mypolicies-from-the-gallery"></a>MyPolicies uit de galerie toe te voegen
Voor het configureren van de integratie van myPolicies in Azure AD, moet u myPolicies uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen myPolicies uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **myPolicies**.

    ![Het maken van een Azure AD-testgebruiker](./media/mypolicies-tutorial/tutorial_mypolicies_search.png)

5. Selecteer in het deelvenster resultaten **myPolicies**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/mypolicies-tutorial/tutorial_mypolicies_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met myPolicies op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in myPolicies is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in myPolicies tot stand worden gebracht.

In myPolicies, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met myPolicies, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker myPolicies](#creating-a-mypolicies-test-user)**  : als u wilt een equivalent van Britta Simon in myPolicies die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing myPolicies.

**Voor het configureren van Azure AD eenmalige aanmelding met myPolicies, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **myPolicies** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/mypolicies-tutorial/tutorial_mypolicies_samlbase.png)

3. Op de **myPolicies domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/mypolicies-tutorial/tutorial_mypolicies_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<tenantname>.mypolicies.com/`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<tenantname>.mypolicies.com/users/auth/saml/callback`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke id en de antwoord-URL. Neem contact op met [myPolicies ondersteuningsteam](mailto:support@mypolicies.com) om deze waarden te verkrijgen.

4. De toepassing myPolicies wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze. 

    ![Eenmalige aanmelding configureren](./media/mypolicies-tutorial/tutorial_mypolicies_attribute.png)

5. Klik op **weergeven en bewerken van alle andere gebruikerskenmerken** selectievakje in de **gebruikerskenmerken** sectie om uit te breiden de kenmerken. De volgende stappen uitvoeren op elk van de kenmerken weergegeven:

    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | ---------- |
    | givenName | User.givenName |
    | Achternaam | User.surname |
    | EmailAddress | User.mail |
    | naam | User.userPrincipalName |
    
    a. Klik op het kenmerk te openen de **kenmerk bewerken** dialoogvenster.
    
    ![Eenmalige aanmelding configureren](./media/mypolicies-tutorial/tutorial_attribute_05.png)
    
    b. Verwijderen van de URL-waarde uit de **Namespace**.
    
    c. Klik op **Ok** om op te slaan van de instelling.
    
6. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/mypolicies-tutorial/tutorial_mypolicies_certificate.png) 

7. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/mypolicies-tutorial/tutorial_general_400.png)

8. Op de **myPolicies configuratie** sectie, klikt u op **configureren myPolicies** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/mypolicies-tutorial/tutorial_mypolicies_configure.png) 

9. Het configureren van eenmalige aanmelding op **myPolicies** zijde, moet u voor het verzenden van de gedownloade **Certificate(Base64)** en **Single Sign-On Service URL voor SAML** naar [ myPolicies ondersteuningsteam](mailto:support@mypolicies.com). 

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/mypolicies-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/mypolicies-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/mypolicies-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/mypolicies-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-mypolicies-test-user"></a>Het maken van een testgebruiker myPolicies

In deze sectie maakt u een gebruiker met de naam van Britta Simon in myPolicies. Werken met [myPolicies ondersteuningsteam](mailto:support@mypolicies.com) om toe te voegen de gebruikers in het myPolicies-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot myPolicies.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan myPolicies toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **myPolicies**.

    ![Eenmalige aanmelding configureren](./media/mypolicies-tutorial/tutorial_mypolicies_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel myPolicies in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing myPolicies.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/mypolicies-tutorial/tutorial_general_01.png
[2]: ./media/mypolicies-tutorial/tutorial_general_02.png
[3]: ./media/mypolicies-tutorial/tutorial_general_03.png
[4]: ./media/mypolicies-tutorial/tutorial_general_04.png

[100]: ./media/mypolicies-tutorial/tutorial_general_100.png

[200]: ./media/mypolicies-tutorial/tutorial_general_200.png
[201]: ./media/mypolicies-tutorial/tutorial_general_201.png
[202]: ./media/mypolicies-tutorial/tutorial_general_202.png
[203]: ./media/mypolicies-tutorial/tutorial_general_203.png

