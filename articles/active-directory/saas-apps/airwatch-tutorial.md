---
title: 'Zelfstudie: Azure Active Directory-integratie met AirWatch | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en AirWatch.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: bf95b949d6fee4057f67d1e44ded36f363aa5e2b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848915"
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Zelfstudie: Azure Active Directory-integratie met AirWatch

In deze zelfstudie leert u hoe u AirWatch integreren met Azure Active Directory (Azure AD).

AirWatch integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot AirWatch heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij AirWatch (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met AirWatch, moet u de volgende items:

- Een Azure AD-abonnement
- Een AirWatch eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. AirWatch uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-airwatch-from-the-gallery"></a>AirWatch uit de galerie toe te voegen
Voor het configureren van de integratie van AirWatch in Azure AD, moet u AirWatch uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen AirWatch uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **AirWatch**.

    ![Het maken van een Azure AD-testgebruiker](./media/airwatch-tutorial/tutorial_airwatch_search.png)

5. Selecteer in het deelvenster resultaten **AirWatch**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met AirWatch op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in AirWatch is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in AirWatch tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in AirWatch.

Om te configureren en testen van Azure AD eenmalige aanmelding met AirWatch, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker AirWatch](#creating-a-airwatch-test-user)**  : als u wilt een equivalent van Britta Simon in AirWatch die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing AirWatch.

**Voor het configureren van Azure AD eenmalige aanmelding met AirWatch, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **AirWatch** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. Op de **AirWatch domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/airwatch-tutorial/tutorial_airwatch_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`

    b. In de **id** tekstvak typt u de waarde als `AirWatch`

    > [!NOTE] 
    > Deze waarde is niet de werkelijke. Deze waarde bijwerken met de werkelijke aanmeldings-URL. Neem contact op met [AirWatch Client ondersteuningsteam](https://www.air-watch.com/company/contact-us/) deze waarde op te halen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. Op de **AirWatch configuratie** sectie, klikt u op **configureren AirWatch** openen **aanmelding configureren** venster. Kopiëren de **Single Sign-On Service URL voor SAML** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/airwatch-tutorial/tutorial_airwatch_configure.png) 

6. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/airwatch-tutorial/tutorial_general_400.png)
<CS>
7. In een ander browservenster aanmelden bij uw bedrijf AirWatch site als beheerder.

8. Klik in het linkernavigatiedeelvenster op **Accounts**, en klik vervolgens op **beheerders**.
   
   ![Beheerders](./media/airwatch-tutorial/ic791920.png "beheerders")

9. Vouw de **instellingen** menu en klik vervolgens op **Directory Services**.
   
   ![Instellingen voor](./media/airwatch-tutorial/ic791921.png "instellingen")

10. Klik op de **gebruiker** tabblad, in de **Base DN** tekstvak, typ de naam van uw domein en klik vervolgens op **opslaan**.
   
   ![Gebruiker](./media/airwatch-tutorial/ic791922.png "gebruiker")

11. Klik op de **Server** tabblad.
   
   ![Server](./media/airwatch-tutorial/ic791923.png "Server")

12. Voer de volgende stappen uit:
    
    ![Uploaden](./media/airwatch-tutorial/ic791924.png "uploaden")   
    
    a. Als **maptype**, selecteer **geen**.

    b. Selecteer **SAML gebruiken voor verificatie**.

    c. Als u wilt het gedownloade certificaat uploaden, klikt u op **uploaden**.

13. In de **aanvragen** sectie, voert u de volgende stappen uit:
    
    ![Aanvraag](./media/airwatch-tutorial/ic791925.png "aanvragen")  

    a. Als **Binding aanvraagtype**, selecteer **POST**.

    b. In de Azure-portal op de **configureren van eenmalige aanmelding bij Airwatch** dialoogvenster pagina-, Kopieer de **Single Sign-On Service URL voor SAML** waarde en plak deze in de **Identity Provider eenmalige aanmelding URL** tekstvak.

    c. Als **NameID-indeling**, selecteer **e-mailadres**.

    d. Klik op **Opslaan**.

14. Klik op de **gebruiker** tabblad opnieuw.
    
    ![Gebruiker](./media/airwatch-tutorial/ic791926.png "gebruiker")

15. In de **kenmerk** sectie, voert u de volgende stappen uit:
    
    ![Kenmerk](./media/airwatch-tutorial/ic791927.png "kenmerk")

    a. In de **Object-id** tekstvak, type **http://schemas.microsoft.com/identity/claims/objectidentifier**.

    b. In de **gebruikersnaam** tekstvak, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    c. In de **weergavenaam** tekstvak, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    d. In de **voornaam** tekstvak, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    e. In de **achternaam** tekstvak, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    f. In de **e** tekstvak, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    g. Klik op **Opslaan**.

<CE>

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/airwatch-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/airwatch-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/airwatch-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/airwatch-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van Britta Simon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-airwatch-test-user"></a>Het maken van een testgebruiker AirWatch

Om Azure AD-gebruikers zich aanmelden bij AirWatch, moeten ze worden ingericht voor AirWatch.

* AirWatch, wordt ingericht als een handmatige taak is.

**Voor het inrichten van een gebruikersaccount, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **AirWatch** bedrijf site als administrator.
2. Klik in het navigatiedeelvenster aan de linkerkant op **Accounts**, en klik vervolgens op **gebruikers**.
   
   ![Gebruikers](./media/airwatch-tutorial/ic791929.png "gebruikers")
3. In de **gebruikers** menu, klikt u op **lijstweergave**, en klik vervolgens op **toevoegen \> gebruiker toevoegen**.
   
   ![Gebruiker toevoegen](./media/airwatch-tutorial/ic791930.png "gebruiker toevoegen")
4. Op de **toevoegen / bewerken van de gebruiker** dialoogvenster, voer de volgende stappen uit:

   ![Gebruiker toevoegen](./media/airwatch-tutorial/ic791931.png "gebruiker toevoegen")   
   1. Type de **gebruikersnaam**, **wachtwoord**, **wachtwoord bevestigen**, **voornaam**, **achternaam**,  **E-mailadres** van een geldige Azure Active Directory-account dat u inrichten in de bijbehorende tekstvakken wilt.
   2. Klik op **Opslaan**.

>[!NOTE]
>U kunt alle andere AirWatch gebruiker-account maken van hulpprogramma's of API's geleverd door AirWatch aan inrichten AAD-gebruikersaccounts.
>  

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan AirWatch.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan AirWatch toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **AirWatch**.

    ![Eenmalige aanmelding configureren](./media/airwatch-tutorial/tutorial_airwatch_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het toegangsvenster. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/airwatch-tutorial/tutorial_general_01.png
[2]: ./media/airwatch-tutorial/tutorial_general_02.png
[3]: ./media/airwatch-tutorial/tutorial_general_03.png
[4]: ./media/airwatch-tutorial/tutorial_general_04.png

[100]: ./media/airwatch-tutorial/tutorial_general_100.png

[200]: ./media/airwatch-tutorial/tutorial_general_200.png
[201]: ./media/airwatch-tutorial/tutorial_general_201.png
[202]: ./media/airwatch-tutorial/tutorial_general_202.png
[203]: ./media/airwatch-tutorial/tutorial_general_203.png

