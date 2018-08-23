---
title: 'Zelfstudie: Azure Active Directory-integratie met Optimizely | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Optimizely.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 28ef03e1-9aad-4301-af97-d94e853edc74
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2018
ms.author: jeedes
ms.openlocfilehash: be56218e174e5d8b0e6bde394f2dfd40fc91e87d
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "42056615"
---
# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Zelfstudie: Azure Active Directory-integratie met Optimizely

In deze zelfstudie leert u hoe u Optimizely integreren met Azure Active Directory (Azure AD).

Optimizely integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Optimizely heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Optimizely (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Optimizely, moet u de volgende items:

- Een Azure AD-abonnement
- Een Optimizely eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Optimizely uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-optimizely-from-the-gallery"></a>Optimizely uit de galerie toe te voegen

Voor het configureren van de integratie van Optimizely in Azure AD, moet u Optimizely uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Optimizely uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Optimizely**.

    ![Het maken van een Azure AD-testgebruiker](./media/optimizely-tutorial/tutorial_optimizely_search.png)

5. Selecteer in het deelvenster resultaten **Optimizely**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/optimizely-tutorial/tutorial_optimizely_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding

In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Optimizely op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Optimizely is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Optimizely tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Optimizely.

Om te configureren en testen van Azure AD eenmalige aanmelding met Optimizely, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Optimizely](#creating-an-optimizely-test-user)**  : als u wilt een equivalent van Britta Simon in Optimizely die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Optimizely.

**Voor het configureren van Azure AD eenmalige aanmelding met Optimizely, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Optimizely** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/optimizely-tutorial/tutorial_optimizely_samlbase.png)

3. Op de **Optimizely domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/optimizely-tutorial/tutorial_optimizely_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://app.optimizely.net/<instance name>`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon:  `urn:auth0:optimizely:contoso`

    > [!NOTE]
    > Deze waarden zijn niet de werkelijke. U kunt de waarde wordt bijgewerkt met de werkelijke aanmeldings-URL en de id, die later in de zelfstudie wordt uitgelegd.

4. Optimizely toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze.
    
    ![Eenmalige aanmelding configureren](./media/optimizely-tutorial/tutorial_optimizely_attribute.png)
    
5. Klik op **weergeven en bewerken van alle andere gebruikerskenmerken** selectievakje in de **gebruikerskenmerken** sectie om uit te breiden de kenmerken. De volgende stappen uitvoeren op elk van de kenmerken weergegeven:

    | Naam kenmerk | Waarde kenmerk |
    | ---------------| --------------- |
    | e-mailen | User.mail |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/optimizely-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/optimizely-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak, type de **kenmerknaam** wordt weergegeven voor die rij.

    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.

    d. Klik op **OK**.

6. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/optimizely-tutorial/tutorial_optimizely_certificate.png)

7. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/optimizely-tutorial/tutorial_general_400.png)

8. Op de **Optimizely configuratie** sectie, klikt u op **configureren Optimizely** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/optimizely-tutorial/tutorial_optimizely_configure.png)

9. Het configureren van eenmalige aanmelding op **Optimizely** zijde, neem contact op met uw accountmanager Optimizely en geef de gedownloade **certificaat (Base64)**, en **Single Sign-On Service URL voor SAML**.

10. In reactie op uw e-mailadres biedt Optimizely u de aanmelding op URL (SP geïnitieerde SSO) en de id (Service Provider entiteits-ID)-waarden.

    a. Kopiëren de **Serviceprovider geïnitieerde eenmalige aanmelding URL** opgegeven door Optimizely en plakken in de **aanmelding URL** -tekstvak in **Optimizely domein en URL's** sectie in Azure portal.

    b. Kopiëren de **entiteit-ID van Service Provider** opgegeven door Optimizely en plakken in de **id** -tekstvak in **Optimizely domein en URL's** sectie in Azure portal.

11. In een ander browservenster aanmelden voor uw toepassing Optimizely.

12. Klikt u op dat u de accountnaam in de rechterbovenhoek en vervolgens **Accountinstellingen**.

    ![Azure AD voor eenmalige aanmelding](./media/optimizely-tutorial/tutorial_optimizely_09.png)

13. Schakel het selectievakje op het tabblad Account **SSO inschakelen** onder eenmalige aanmelding in de **overzicht** sectie.
  
    ![Azure AD voor eenmalige aanmelding](./media/optimizely-tutorial/tutorial_optimizely_10.png)

14. Klik op **Opslaan**.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/optimizely-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/optimizely-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/optimizely-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/optimizely-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van Britta Simon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.

### <a name="creating-an-optimizely-test-user"></a>Het maken van een testgebruiker Optimizely

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Optimizely.

1. Selecteer op de startpagina **Samenwerkers** tabblad.

2. Nieuwe samenwerker toevoegen aan het project, klikt u op **nieuwe Samenwerker**.
   
    ![Het maken van een Azure AD-testgebruiker](./media/optimizely-tutorial/create_aaduser_10.png)

3. Vul in het e-mailadres en een rol toewijzen. Klik op **uitnodigen**.

    ![Het maken van een Azure AD-testgebruiker](./media/optimizely-tutorial/create_aaduser_11.png)

4. Ze ontvangen een e-mailuitnodiging. Met behulp van het e-mailadres, hebben ze zich aanmelden bij Optimizely.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Optimizely.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Optimizely toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **Optimizely**.

    ![Eenmalige aanmelding configureren](./media/optimizely-tutorial/tutorial_optimizely_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Optimizely in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Optimizely.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/optimizely-tutorial/tutorial_general_01.png
[2]: ./media/optimizely-tutorial/tutorial_general_02.png
[3]: ./media/optimizely-tutorial/tutorial_general_03.png
[4]: ./media/optimizely-tutorial/tutorial_general_04.png

[100]: ./media/optimizely-tutorial/tutorial_general_100.png

[200]: ./media/optimizely-tutorial/tutorial_general_200.png
[201]: ./media/optimizely-tutorial/tutorial_general_201.png
[202]: ./media/optimizely-tutorial/tutorial_general_202.png
[203]: ./media/optimizely-tutorial/tutorial_general_203.png