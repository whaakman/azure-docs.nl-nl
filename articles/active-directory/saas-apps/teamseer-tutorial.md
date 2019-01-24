---
title: 'Zelfstudie: Azure Active Directory-integratie met TeamSeer | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TeamSeer.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: ffe94d8d3e08bb35cf8ea20f4b1e32b1ec84aa35
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54814151"
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Zelfstudie: Azure Active Directory-integratie met TeamSeer

In deze zelfstudie leert u hoe u TeamSeer integreren met Azure Active Directory (Azure AD).

TeamSeer integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TeamSeer heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij TeamSeer (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met TeamSeer, moet u de volgende items:

- Een Azure AD-abonnement
- Een TeamSeer eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TeamSeer uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-teamseer-from-the-gallery"></a>TeamSeer uit de galerie toe te voegen
Voor het configureren van de integratie van TeamSeer in met Azure AD, moet u TeamSeer uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen TeamSeer uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **TeamSeer**.

    ![Het maken van een Azure AD-testgebruiker](./media/teamseer-tutorial/tutorial_teamseer_search.png)

1. Selecteer in het deelvenster resultaten **TeamSeer**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met TeamSeer op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in TeamSeer is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in TeamSeer tot stand worden gebracht.

In TeamSeer, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TeamSeer, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker TeamSeer](#creating-a-teamseer-test-user)**  : als u wilt een equivalent van Britta Simon in TeamSeer die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing TeamSeer.

**Voor het configureren van Azure AD eenmalige aanmelding met TeamSeer, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **TeamSeer** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/teamseer-tutorial/tutorial_teamseer_samlbase.png)

1. Op de **TeamSeer domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/teamseer-tutorial/tutorial_teamseer_url.png)

     Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://www.teamseer.com/<companyid>`

    > [!NOTE] 
    > De waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [TeamSeer Client ondersteuningsteam](https://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) om de waarde. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/teamseer-tutorial/tutorial_teamseer_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/teamseer-tutorial/tutorial_general_400.png)

1. Op de **TeamSeer configuratie** sectie, klikt u op **configureren TeamSeer** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/teamseer-tutorial/tutorial_teamseer_configure.png)

1. In een ander browservenster aanmelden bij uw bedrijf TeamSeer site als beheerder.

1. Ga naar **HR Admin**.
   
    ![HR-beheerder](./media/teamseer-tutorial/ic789634.png "HR-beheerder")

1. Klik op **Setup**.
   
    ![Setup](./media/teamseer-tutorial/ic789635.png "Setup")

1. Klik op **details voor SAML-provider instellen**.
   
    ![SAML Settings](./media/teamseer-tutorial/ic789636.png "SAML Settings")

1. In de sectie van de informatie in de SAML-provider, moet u de volgende stappen uitvoeren:
   
    ![SAML Settings](./media/teamseer-tutorial/ic789637.png "SAML Settings")   

    a. Plak de **Single Sign-On Service URL** waarde die moet worden de **URL** tekstvak.
          
    b. Het base-64 gecodeerde certificaat openen in Kladblok, Kopieer de inhoud van in naar het Klembord en plakt u deze naar de **IdP openbaar certificaat** tekstvak.

1. Voor het voltooien van de configuratie van de SAML-provider, moet u de volgende stappen uitvoeren:
    
    ![SAML Settings](./media/teamseer-tutorial/ic789638.png "SAML Settings") 

    a. In de **e-mailadressen testen**, typ de e-mailadres van de testgebruiker. 
  
    b. In de **verlener** tekstvak typt u de URL-verlener van de serviceprovider. 
  
    c. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/teamseer-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/teamseer-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/teamseer-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/teamseer-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-teamseer-test-user"></a>Het maken van een testgebruiker TeamSeer

Om Azure AD-gebruikers zich aanmelden bij TeamSeer, moeten ze worden ingericht voor ShiftPlanning. In het geval van TeamSeer is inrichten een handmatige taak.

**Als u een gebruikersaccount wilt inrichten, voert u de volgende stappen uit:**

1. Meld u aan bij uw **TeamSeer** bedrijf site als beheerder.

1. Voer de volgende stappen uit:
   
    ![HR-beheerder](./media/teamseer-tutorial/ic789640.png "HR-beheerder")  
 
    a. Ga naar **HR Admin \> gebruikers**.
  
    b. Klik op **voert u de wizard nieuwe gebruiker**.

1. In de **Gebruikersdetails** sectie, voert u de volgende stappen uit:
   
    ![Gebruikersdetails](./media/teamseer-tutorial/ic789641.png "Gebruikersdetails")

    a. Type de **voornaam**, **achternaam**, **gebruikersnaam (e-mailadres)** van een geldige AAD-account dat u inrichten wilt de bijbehorende tekstvakken.
  
    b. Klik op **volgende**.

1. Volg de aanwijzingen op het scherm instructies voor het toevoegen van een nieuwe gebruiker en klikt u op **voltooien**.

>[!NOTE]
>U kunt alle andere TeamSeer gebruiker-account maken van hulpprogramma's of API's geleverd door TeamSeer voor het inrichten van gebruikersaccounts van de Azure AD. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan TeamSeer.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan TeamSeer toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **TeamSeer**.

    ![Eenmalige aanmelding configureren](./media/teamseer-tutorial/tutorial_teamseer_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/teamseer-tutorial/tutorial_general_01.png
[2]: ./media/teamseer-tutorial/tutorial_general_02.png
[3]: ./media/teamseer-tutorial/tutorial_general_03.png
[4]: ./media/teamseer-tutorial/tutorial_general_04.png

[100]: ./media/teamseer-tutorial/tutorial_general_100.png

[200]: ./media/teamseer-tutorial/tutorial_general_200.png
[201]: ./media/teamseer-tutorial/tutorial_general_201.png
[202]: ./media/teamseer-tutorial/tutorial_general_202.png
[203]: ./media/teamseer-tutorial/tutorial_general_203.png

