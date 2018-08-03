---
title: 'Zelfstudie: Azure Active Directory-integratie met Canvas Lms | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Canvas LMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: jeedes
ms.openlocfilehash: af2c997f0842da751eb93f0788a7402fc7d144ae
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433286"
---
# <a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Zelfstudie: Azure Active Directory-integratie met Canvas LMS

In deze zelfstudie leert u hoe u naar het Canvas integreren met Azure Active Directory (Azure AD).

Canvas integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Canvas heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Canvas (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Canvas, moet u de volgende items:

- Een Azure AD-abonnement
- Een Canvas eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Canvas uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-canvas-from-the-gallery"></a>Canvas uit de galerie toe te voegen
Voor het configureren van de integratie van Canvas in Azure AD, moet u Canvas uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Canvas uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **Canvas**.

    ![Het maken van een Azure AD-testgebruiker](./media/canvas-lms-tutorial/tutorial_canvaslms_search.png)

1. Selecteer in het deelvenster resultaten **Canvas**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/canvas-lms-tutorial/tutorial_canvaslms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Canvas op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Canvas is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in het Canvas tot stand worden gebracht.

In het Canvas, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Canvas, u nodig hebt voor de volgende bouwstenen:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Canvas](#creating-a-canvas-test-user)**  : als u wilt een equivalent van Britta Simon hebben in het Canvas dat is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Canvas.

**Voor het configureren van Azure AD eenmalige aanmelding met Canvas, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Canvas** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/canvas-lms-tutorial/tutorial_canvaslms_samlbase.png)

1. Op de **Canvas domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/canvas-lms-tutorial/tutorial_canvaslms_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<tenant-name>.instructure.com`

    b. In de **id** tekstvak typt u de waarde met behulp van het volgende patroon: `https://<tenant-name>.instructure.com/saml2`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [Canvas Client ondersteuningsteam](https://community.canvaslms.com/community/help) om deze waarden te verkrijgen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, Kopieer de **VINGERAFDRUK** waarde van het certificaat.

    ![Eenmalige aanmelding configureren](./media/canvas-lms-tutorial/tutorial_canvaslms_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/canvas-lms-tutorial/tutorial_general_400.png)

1. Op de **Canvas configuratie** sectie, klikt u op **configureren Canvas** openen **aanmelding configureren** venster. Kopiëren de **URL van wijzigen wachtwoord, URL van de afmelding, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/canvas-lms-tutorial/tutorial_canvaslms_configure.png) 
 
1. In een ander browservenster aanmelden bij uw bedrijf Canvas site als beheerder.

1. Ga naar **cursussen \> beheerde Accounts \> Microsoft**.
   
    ![Canvas](./media/canvas-lms-tutorial/IC775990.png "Canvas")

1. Selecteer in het navigatiedeelvenster aan de linkerkant **verificatie**, en klik vervolgens op **nieuwe SAML-configuratie toevoegen**.
   
    ![Verificatie](./media/canvas-lms-tutorial/IC775991.png "verificatie")

1. Op de pagina van de huidige integratie, kunt u de volgende stappen uitvoeren:
   
    ![Huidige integratie](./media/canvas-lms-tutorial/IC775992.png "huidige integratie")

    a. In **IdP entiteit-ID** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.

    b. In **Log URL** tekstvak, plak de waarde van **Single Sign-On Service URL voor SAML** die u hebt gekopieerd vanuit Azure portal.

    c. In **afmeldings-URL** tekstvak, plak de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal.

    d. In **koppeling van wijzigen wachtwoord** tekstvak, plak de waarde van **URL van wijzigen wachtwoord** die u hebt gekopieerd vanuit Azure portal. 

    e. In **certificaat vingerafdruk** tekstvak, plak de **vingerafdruk** waarde van het certificaat dat u hebt gekopieerd vanuit Azure portal.      
        
    f. Uit de **aanmelding kenmerk** in de lijst met **NameID**.

    g. Uit de **id indeling** in de lijst met **emailAddress**.

    h. Klik op **verificatie-instellingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/canvas-lms-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/canvas-lms-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/canvas-lms-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/canvas-lms-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-canvas-test-user"></a>Het maken van een testgebruiker Canvas

Als u wilt dat Azure AD-gebruikers zich aanmelden bij Canvas, moeten ze worden ingericht voor Canvas.

In het geval van Canvas is inrichten van gebruikers een handmatige taak.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **Canvas** tenant.

1. Ga naar **cursussen \> beheerde Accounts \> Microsoft**.
   
   ![Canvas](./media/canvas-lms-tutorial/IC775990.png "Canvas")

1. Klik op **gebruikers**.
   
   ![Gebruikers](./media/canvas-lms-tutorial/IC775995.png "gebruikers")

1. Klik op **nieuwe gebruiker toevoegen**.
   
   ![Gebruikers](./media/canvas-lms-tutorial/IC775996.png "gebruikers")

1. Voer de volgende stappen uit op de pagina van een nieuwe gebruiker-dialoogvenster toevoegen:
   
   ![Gebruiker toevoegen](./media/canvas-lms-tutorial/IC775997.png "gebruiker toevoegen")
   
   a. In de **volledige naam** tekstvak, voer de naam van gebruiker, zoals **BrittaSimon**.

   b. In de **e** tekstvak, voer het e-mailadres van gebruiker, zoals **brittasimon@contoso.com**.

   c. In de **aanmelding** tekstvak invoeren van de gebruiker Azure AD e-mailadres zoals **brittasimon@contoso.com**.

   d. Selecteer **e-de gebruiker informatie over het maken van dit account**.

   e. Klik op **gebruiker toevoegen**.

>[!NOTE]
>U kunt alle andere Canvas gebruiker-account maken van hulpprogramma's of API's geleverd door Canvas aan inrichten AAD-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan papier.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan papier, kunt u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Canvas**.

    ![Eenmalige aanmelding configureren](./media/canvas-lms-tutorial/tutorial_canvaslms_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Canvas in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Canvas.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/canvas-lms-tutorial/tutorial_general_01.png
[2]: ./media/canvas-lms-tutorial/tutorial_general_02.png
[3]: ./media/canvas-lms-tutorial/tutorial_general_03.png
[4]: ./media/canvas-lms-tutorial/tutorial_general_04.png

[100]: ./media/canvas-lms-tutorial/tutorial_general_100.png

[200]: ./media/canvas-lms-tutorial/tutorial_general_200.png
[201]: ./media/canvas-lms-tutorial/tutorial_general_201.png
[202]: ./media/canvas-lms-tutorial/tutorial_general_202.png
[203]: ./media/canvas-lms-tutorial/tutorial_general_203.png

