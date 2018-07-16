---
title: 'Zelfstudie: Azure Active Directory-integratie met Learning Seat LMS | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de Learning-Seat LMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: bb056fcf-4135-478e-85b1-5015d1f07b85
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: jeedes
ms.openlocfilehash: d1a15617830f7b517210c7e86146a88d38ae8343
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39046818"
---
# <a name="tutorial-azure-active-directory-integration-with-learning-seat-lms"></a>Zelfstudie: Azure Active Directory-integratie met Learning Seat LMS

In deze zelfstudie leert u hoe u aan het Learning-Seat LMS integreren met Azure Active Directory (Azure AD).

Learning-Seat LMS integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Learning-Seat LMS heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Learning-Seat LMS (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, raadpleegt u. [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Learning Seat LMS, moet u de volgende items:

- Een Azure AD-abonnement
- Een Learning Seat LMS eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Learning-Seat LMS uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-learning-seat-lms-from-the-gallery"></a>Learning-Seat LMS uit de galerie toe te voegen
Voor het configureren van de integratie van Learning Seat LMS in Azure AD, moet u de Learning-Seat LMS uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Learning Seat LMS uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Learning Seat LMS**.

    ![Het maken van een Azure AD-testgebruiker](./media/learningseatlms-tutorial/tutorial_learnconnect_search.png)

5. Selecteer in het deelvenster resultaten **Learning Seat LMS**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Learning-Seat LMS op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de Learning-Seat LMS is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de Learning-Seat LMS tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Learning-Seat LMS.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Learning Seat LMS, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker Learning Seat LMS](#creating-a-learnconnect-test-user)**  - Learning-Seat LMS die is gekoppeld aan de Azure AD-weergave van de gebruiker een equivalent van Britta Simon hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw leerproces Seat LMS-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Learning Seat LMS, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Learning Seat LMS** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/learningseatlms-tutorial/tutorial_learnconnect_samlbase.png)

3. Op de **Learning Seat LMS-domein en URL's** sectie, voert u de volgende stappen uit als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/learningseatlms-tutorial/tutorial_learnconnect_url.png)

    a. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.learningseatlms.com`

    b. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.learningseatlms.com/Account/AssertionConsumerService`

4. Controleer **geavanceerde URL-instellingen weergeven**, als u wilt configureren van de toepassing in **SP** modus gestart:

    ![Eenmalige aanmelding configureren](./media/learningseatlms-tutorial/tutorial_learnconnect_url2.png)

    In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.learningseatlms.com`
     
    > [!NOTE] 
    > Deze waarden zijn niet de werkelijke waarden. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [Learning-Seat-ondersteuningsteam](http://help.learningseatlms.com/help) om deze waarden te verkrijgen. 

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/learningseatlms-tutorial/tutorial_learnconnect_certificate.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/learningseatlms-tutorial/tutorial_general_400.png)

7. Het configureren van eenmalige aanmelding op **Learning Seat LMS** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [Learning-Seat-ondersteuningsteam](http://help.learningseatlms.com/help).

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD](https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/learningseatlms-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/learningseatlms-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/learningseatlms-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/learningseatlms-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-learning-seat-lms-test-user"></a>Het maken van een testgebruiker Learning Seat LMS

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Learning-Seat LMS. Neem contact op met [Learning-Seat-ondersteuningsteam](http://help.learningseatlms.com/help) met alle gebruikersgegevens toe te voegen de gebruikers in de Learning-Seat LMS-toepassing.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Learning Seat LMS.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Learning Seat LMS, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Learning Seat LMS**.

    ![Eenmalige aanmelding configureren](./media/learningseatlms-tutorial/tutorial_learnconnect_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster. 

Klik op de Learning-Seat LMS-tegel in het toegangsvenster, u zal worden automatisch aangemeld bij de Learning-Seat LMS-toepassing. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/learnconnect-tutorial/tutorial_general_01.png
[2]: ./media/learnconnect-tutorial/tutorial_general_02.png
[3]: ./media/learnconnect-tutorial/tutorial_general_03.png
[4]: ./media/learnconnect-tutorial/tutorial_general_04.png

[100]: ./media/learnconnect-tutorial/tutorial_general_100.png

[200]: ./media/learnconnect-tutorial/tutorial_general_200.png
[201]: ./media/learnconnect-tutorial/tutorial_general_201.png
[202]: ./media/learnconnect-tutorial/tutorial_general_202.png
[203]: ./media/learnconnect-tutorial/tutorial_general_203.png

