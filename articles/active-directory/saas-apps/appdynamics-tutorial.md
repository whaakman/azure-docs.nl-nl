---
title: 'Zelfstudie: Azure Active Directory-integratie met AppDynamics | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en AppDynamics.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 25fd1df0-411c-4f55-8be3-4273b543100f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2018
ms.author: jeedes
ms.openlocfilehash: 3600e83d18f8cabd03c46af2ef47445c588cbdb5
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38548272"
---
# <a name="tutorial-azure-active-directory-integration-with-appdynamics"></a>Zelfstudie: Azure Active Directory-integratie met AppDynamics

In deze zelfstudie leert u hoe u AppDynamics integreren met Azure Active Directory (Azure AD).

AppDynamics integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot AppDynamics heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij AppDynamics (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met AppDynamics, moet u de volgende items:

- Een Azure AD-abonnement
- Een AppDynamics eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving.
Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. AppDynamics uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-appdynamics-from-the-gallery"></a>AppDynamics uit de galerie toe te voegen
Voor het configureren van de integratie van AppDynamics in Azure AD, moet u AppDynamics uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen AppDynamics uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]

3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **AppDynamics**.

    ![Het maken van een Azure AD-testgebruiker](./media/appdynamics-tutorial/tutorial_appdynamics_search.png)

5. Selecteer in het deelvenster resultaten **AppDynamics**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/appdynamics-tutorial/tutorial_appdynamics_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met AppDynamics op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in AppDynamics is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in AppDynamics tot stand worden gebracht.

In AppDynamics, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met AppDynamics, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker AppDynamics](#creating-an-appdynamics-test-user)**  : als u wilt een equivalent van Britta Simon in AppDynamics die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing AppDynamics.

**Voor het configureren van Azure AD eenmalige aanmelding met AppDynamics, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **AppDynamics** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/appdynamics-tutorial/tutorial_appdynamics_samlbase.png)

3. Op de **AppDynamics domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/appdynamics-tutorial/tutorial_appdynamics_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.saas.appdynamics.com`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.saas.appdynamics.com/controller`

    > [!NOTE]
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [AppDynamics Client ondersteuningsteam](https://www.appdynamics.com/support/) om deze waarden te verkrijgen.

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **certificaat (Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/appdynamics-tutorial/tutorial_appdynamics_certificate.png)

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/appdynamics-tutorial/tutorial_general_400.png)

6. Op de **AppDynamics configuratie** sectie, klikt u op **configureren AppDynamics** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/appdynamics-tutorial/tutorial_appdynamics_configure.png)

7. In een ander browservenster aanmelden bij uw bedrijf AppDynamics site als beheerder.

8. Klik in de werkbalk bovenaan op **instellingen**, en klik vervolgens op **beheer**.

    ![Beheer](./media/appdynamics-tutorial/ic790216.png "beheer")

9. Klik op de **verificatieprovider** tabblad.

    ![Verificatieprovider](./media/appdynamics-tutorial/ic790224.png "Authentication-Provider")

10. In de **verificatieprovider** sectie, voert u de volgende stappen uit:

    ![De SAML-configuratie](./media/appdynamics-tutorial/ic790225.png "SAML-configuratie")

    a. Als **verificatieprovider**, selecteer **SAML**.

    b. In de **aanmeldings-URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.

    c. In de **afmeldings-URL van** tekstvak, plak de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal.

    d. Het base-64 gecodeerde certificaat openen in Kladblok, Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **certificaat** tekstvak

    e. Klik op **Opslaan**.

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/appdynamics-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.

    ![Het maken van een Azure AD-testgebruiker](./media/appdynamics-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.

    ![Het maken van een Azure AD-testgebruiker](./media/appdynamics-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:

    ![Het maken van een Azure AD-testgebruiker](./media/appdynamics-tutorial/create_aaduser_04.png)

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.

### <a name="creating-an-appdynamics-test-user"></a>Het maken van een testgebruiker AppDynamics

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in AppDynamics. AppDynamics biedt ondersteuning voor just-in-time inrichting, dit is standaard ingeschakeld. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is gemaakt tijdens een poging tot toegang tot AppDynamics als deze nog niet bestaat.
>[!Note]
>Als u maken van een gebruiker handmatig wilt, neem dan contact op met [AppDynamics Client ondersteuningsteam](https://www.appdynamics.com/support/).

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan AppDynamics.

![Gebruiker toewijzen][200]

**Als u wilt Britta Simon aan AppDynamics toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **AppDynamics**.

    ![Eenmalige aanmelding configureren](./media/appdynamics-tutorial/tutorial_appdynamics_app.png)

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.

### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.

Wanneer u op de tegel AppDynamics in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing AppDynamics.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/appdynamics-tutorial/tutorial_general_01.png
[2]: ./media/appdynamics-tutorial/tutorial_general_02.png
[3]: ./media/appdynamics-tutorial/tutorial_general_03.png
[4]: ./media/appdynamics-tutorial/tutorial_general_04.png

[100]: ./media/appdynamics-tutorial/tutorial_general_100.png

[200]: ./media/appdynamics-tutorial/tutorial_general_200.png
[201]: ./media/appdynamics-tutorial/tutorial_general_201.png
[202]: ./media/appdynamics-tutorial/tutorial_general_202.png
[203]: ./media/appdynamics-tutorial/tutorial_general_203.png
