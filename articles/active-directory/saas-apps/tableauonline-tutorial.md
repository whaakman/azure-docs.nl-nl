---
title: 'Zelfstudie: Azure Active Directory-integratie met Tableau Online | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Tableau Online.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd5e3087c21908600be9cd369a15f3036e5acb2f
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58884700"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Zelfstudie: Azure Active Directory-integratie met Tableau Online

In deze zelfstudie leert u hoe u Tableau Online integreren met Azure Active Directory (Azure AD).

Tableau Online integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Tableau Online heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Tableau Online (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Tableau Online, moet u de volgende items:

- Een Azure AD-abonnement
- Een Online Tableau eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Tableau Online toevoegen vanuit de galerie
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-tableau-online-from-the-gallery"></a>Tableau Online toevoegen vanuit de galerie
Voor het configureren van de integratie van Online Tableau in Azure AD, moet u Tableau Online toevoegen vanuit de galerie aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen vanuit de galerie Tableau Online, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Tableau Online**.

    ![Het maken van een Azure AD-testgebruiker](./media/tableauonline-tutorial/tutorial_tableauonline_search.png)

1. Selecteer in het deelvenster resultaten **Tableau Online**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/tableauonline-tutorial/tutorial_tableauonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Tableau Online op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Tableau Online is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Tableau Online tot stand worden gebracht.

In Online Tableau, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Tableau Online, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een Online Tableau testgebruiker](#creating-a-tableau-online-test-user)**  : als u wilt een equivalent van Britta Simon in Tableau Online die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Tableau Online.

**Voor het configureren van Azure AD eenmalige aanmelding met Tableau Online, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Tableau Online** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/tutorial_tableauonline_samlbase.png)

1. Op de **Tableau Online domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/tutorial_tableauonline_url.png)
    
    a. Typ een URL in het tekstvak **Aanmeldings-URL**: `https://sso.online.tableau.com`

    b. Typ de URL in het tekstvak **Id**: `https://sso.online.tableau.com/public/sp/metadata?alias=<entityid>`

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/tutorial_tableauonline_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/tutorial_general_400.png)

1. In een ander browservenster aanmelden voor uw toepassing Tableau Online. Ga naar **instellingen** en vervolgens **verificatie**.
   
    ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/tutorial_tableauonline_09.png)
    
1. Om in te schakelen SAML, onder **verificatietypen** sectie. Controleer de **eenmalige aanmelding met SAML** selectievakje.
   
    ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/tutorial_tableauonline_12.png)

1. Schuif omlaag totdat **bestand met metagegevens importeren in Tableau Online** sectie.  Klik op Bladeren en importeer het metagegevensbestand dat u hebt gedownload van Azure AD. Klik vervolgens op **toepassen**.
   
   ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/tutorial_tableauonline_13.png)

1. In de **overeenkomen met asserties** sectie, voegt u de bijbehorende verklaring-naam van id-Provider voor **e-mailadres**, **voornaam**, en **achternaam**. Deze informatie ophalen uit Azure AD: 
  
    a. Ga in de Azure-portal op de **Tableau Online** toepassingspagina integratie.
    
    b. Selecteer in de kenmerkensectie de **'weergeven en bewerk alle andere gebruikerskenmerken'** selectievakje. 
    
   ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/attributesection.png)
      
    c. Kopieer de naamruimtewaarde voor deze kenmerken: givenname, e-mail- en achternaam met behulp van de volgende stappen uit:

   ![Azure AD voor eenmalige aanmelding](./media/tableauonline-tutorial/tutorial_tableauonline_10.png)
    
    d. Klik op **user.givenname** waarde 
    
    e. Kopieer de waarde van de **naamruimte** tekstvak.

   ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/attributesection2.png)

    f. Als u wilt kopiëren van de naamruimte stappen waarden voor de e-mail- en achternaam de voorgaande.

    g. Schakel over naar de Online Tableau toepassing, en stel vervolgens de **Tableau Online kenmerken** sectie als volgt:
     * E-mailadres: **e-mail** of **userprincipalname**
     * Voornaam: **givenname**
     * Achternaam: **achternaam**
   
   ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/tutorial_tableauonline_14.png)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/tableauonline-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/tableauonline-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/tableauonline-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/tableauonline-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-tableau-online-test-user"></a>Het maken van een Online Tableau testgebruiker

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Tableau Online.

1. Op **Tableau Online**, klikt u op **instellingen** en vervolgens **verificatie** sectie. Schuif omlaag naar **Select Users** sectie. Klik op **gebruikers toevoegen** en vervolgens **Voer e-mailadressen**.
   
    ![Het maken van een Azure AD-testgebruiker](./media/tableauonline-tutorial/tutorial_tableauonline_15.png)
1. Selecteer **toevoegen van gebruikers voor eenmalige aanmelding (SSO) verificatie**. In de **e-mailadressen invoeren** tekstvak toevoegen britta.simon@contoso.com
   
    ![Het maken van een Azure AD-testgebruiker](./media/tableauonline-tutorial/tutorial_tableauonline_11.png)
1. Klik op **Create**.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot Tableau Online.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon Tableau online, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Tableau Online**.

    ![Eenmalige aanmelding configureren](./media/tableauonline-tutorial/tutorial_tableauonline_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op de knop **Add**. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van de configuratie van uw Azure AD-eenmalige aanmelding via het toegangsvenster.

Wanneer u op de tegel Tableau Online in het toegangsvenster, u moet u automatisch aangemeld bij uw Tableau Online-toepassing.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/tableauonline-tutorial/tutorial_general_04.png

[100]: ./media/tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/tableauonline-tutorial/tutorial_general_201.png
[202]: ./media/tableauonline-tutorial/tutorial_general_202.png
[203]: ./media/tableauonline-tutorial/tutorial_general_203.png
