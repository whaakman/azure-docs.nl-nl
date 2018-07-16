---
title: 'Zelfstudie: Azure Active Directory-integratie met Adobe Sign | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Adobe Sign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: jeedes
ms.openlocfilehash: d5cdc2ec0c6cfcf52f84629485d0dd879fbf6fa2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39053995"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Zelfstudie: Azure Active Directory-integratie met Adobe Sign

In deze zelfstudie leert u hoe u Adobe Sign integreren met Azure Active Directory (Azure AD).

Integratie van Adobe Sign in Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Adobe Sign heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Adobe Sign (eenmalige aanmelding) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Adobe Sign, hebt u het volgende nodig:

- Een Azure AD-abonnement
- Een Adobe Sign eenmalige aanmelding ingeschakeld abonnement

Als u wilt testen van de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen van Adobe Sign in de galerie.
2. Configureren en testen van Azure AD eenmalige aanmelding.

## <a name="add-adobe-sign-from-the-gallery"></a>Adobe Sign uit de galerie toevoegen
Voor het configureren van de integratie van Adobe Sign in Azure AD, moet u Adobe Sign uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![Schermafbeelding van het Azure Active Directory-pictogram][1]

2. Blader naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![Schermopname van Azure Active Directory-menu's, met zakelijke toepassingen en alle toepassingen die zijn gemarkeerd][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing** aan de bovenkant van het dialoogvenster.

    ![Schermafbeelding van de nieuwe optie voor de App aan de bovenkant van het dialoogvenster][3]

4. Typ in het zoekvak **Adobe Sign**.

    ![Schermopname van het zoekvak](./media/adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. Selecteer in het deelvenster resultaten **Adobe Sign**, en selecteer vervolgens **toevoegen**.

    ![Schermopname van het deelvenster resultaten](./media/adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Adobe Sign, op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD voor het herkennen van een gekoppelde relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Adobe Sign.

Voor het maken van de gekoppelde relatie in Adobe Sign, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam**.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met Adobe Sign, voert u de volgende bouwstenen:

1. [Azure AD eenmalige aanmelding configureren](#configuring-azure-ad-single-sign-on) zodat uw gebruikers deze functie wilt gebruiken.
2. [Maak een Azure AD-testgebruiker](#creating-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maak een testgebruiker Adobe Sign](#creating-an-adobe-sign-test-user) hebben een equivalent van Britta Simon in Adobe Sign die gekoppeld is aan de Azure AD-weergave van de gebruiker.
4. [Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Eenmalige aanmelding testen](#testing-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie kunt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Adobe Sign.

1. In de Azure-portal op de **Adobe Sign** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Schermafbeelding van Adobe Sign integratie toepassingspagina met eenmalige aanmelding is gemarkeerd][4]

2. Op de **eenmalige aanmelding** in het dialoogvenster voor **modus**, selecteer **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.

    ![Schermafbeelding van één aanmelding in het dialoogvenster met de modus in gemarkeerd](./media/adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. In de **Adobe Sign domein en URL's** sectie, voert u de volgende stappen uit:

    ![Schermafbeelding van Adobe Sign domein en URL's sectie](./media/adobe-echosign-tutorial/tutorial_adobesign_url.png)

    a. In de **aanmeldings-URL** tekstvak typt u een URL die gebruikmaakt van het volgende patroon: `https://<companyname>.echosign.com/`

    b. In de **id** tekstvak typt u een URL die gebruikmaakt van het volgende patroon: `https://<companyname>.echosign.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [Adobe Sign Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html) om deze waarden te verkrijgen.

4. In de **SAML-handtekeningcertificaat** sectie, selecteer **Certificate(Base64)**, en sla het certificaatbestand op uw computer.

    ![Schermafbeelding van SAML-handtekeningcertificaat sectie](./media/adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. Selecteer **Opslaan**.

    ![Schermafbeelding van opslaan](./media/adobe-echosign-tutorial/tutorial_general_400.png)

6. In de **Adobe Sign configuratie** sectie, selecteer **configureren Adobe Sign** openen de **aanmelding configureren** venster. Kopiëren de **afmelding URL**, **SAML entiteit-ID**, en **Single Sign-On Service URL voor SAML** uit de **Naslaggids** sectie.

    ![Schermafbeelding van Adobe Sign configuratiesectie, met configureren van Adobe Sign gemarkeerd](./media/adobe-echosign-tutorial/tutorial_adobesign_configure.png)

7. Voordat de configuratie, neem contact op met de [Adobe Sign Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html) aan lijst met geaccepteerde uw domein in de Adobe Sign. Dit is het toevoegen van het domein:

    a. De [Adobe Sign Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html) stuurt u een willekeurig gegenereerde token. Uw domein, is het token als volgt: **verifiëren van adobe sign xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx =**

    b. Het verificatietoken publiceren in een DNS-record voor tekst en op de hoogte stellen de [Adobe Sign Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html).
    
    > [!NOTE]
    > Dit duurt enkele dagen, of meer. Houd er rekening mee dat het DNS-doorgifte vertragingen betekent dat een waarde die is gepubliceerd in DNS mogelijk niet zichtbaar voor een uur of langer. Uw IT-beheerder moet kennis van hoe u dit token publiceert in een record van DNS-tekst.
    
    c. Wanneer u de hoogte stellen de [Adobe Sign Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html) via het ondersteuningsticket nadat het token is gepubliceerd, zij het domein valideren en toe te voegen aan uw account.
    
    d. Over het algemeen als volgt het publiceren van het token op een DNS-record:

    * Aanmelden bij uw domeinaccount
    * De pagina voor het bijwerken van de DNS-record vinden. Deze pagina kan worden aangeroepen voor DNS-beheer, Name Server Management of geavanceerde instellingen.
    * De TXT-records vinden voor uw domein.
    * Voeg een TXT-record met de volledige token waarde die is geleverd door Adobe.
    * Sla uw wijzigingen op.

8. In een ander browservenster aanmelden bij uw bedrijf Adobe Sign site als beheerder.

9. Selecteer in het menu SAML **Accountinstellingen** > **SAML-instellingen**.
   
    ![Schermafbeelding van Adobe Sign SAML instellingenpagina](./media/adobe-echosign-tutorial/ic789520.png "Account")

10. In de **SAML-instellingen** sectie, voert u de volgende stappen uit:
  
    ![Schermopname van het SAML-instellingen](./media/adobe-echosign-tutorial/ic789521.png "SAML-instellingen")
   
    a. Onder **SAML modus**, selecteer **SAML verplichte**.
   
    b. Selecteer **Echosign-accountbeheerders toestaan om aan te melden met hun referenties Echosign**.
   
    c. Onder **maken door gebruiker**, selecteer **automatisch toevoegen van gebruikers worden geverifieerd via SAML**.

    d. Plakken **SAML entiteit-ID**, die u hebt gekopieerd vanuit de Azure portal in de **URL van de entiteit-ID/verlener** in het tekstvak.
    
    e. Plakken **Single Sign-On Service URL voor SAML**, die u hebt gekopieerd vanuit Azure portal in de **URL/SSO-eindpunt voor aanmelding** in het tekstvak.
   
    f. Plakken **afmelding URL**, die u hebt gekopieerd vanuit de Azure portal in de **URL/SLO Afmeldingseindpunt** in het tekstvak.

    g. Open uw gedownloade **Certificate(Base64)** bestand in Kladblok. Kopieer de inhoud ervan in het Klembord en plakt u deze naar de **IdP certificaat** in het tekstvak.

    h. Selecteer **wijzigingen opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD
Het doel van deze sectie is het maken van een testgebruiker Britta Simon, met de naam in Azure portal.

![Schermafbeelding van de gebruikersnaam testen in Azure portal][100]

1. In de **Azure-portal**, selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram.

    ![Schermafbeelding van het Azure AD-pictogram](./media/adobe-echosign-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer **alle gebruikers**.
    
    ![Schermafbeelding van de Azure AD-menu's, met gebruikers en groepen en alle gebruikers die zijn gemarkeerd](./media/adobe-echosign-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** in het dialoogvenster, selecteer **toevoegen**.
 
    ![Schermafbeelding van de bovenkant van alle gebruikers in het dialoogvenster met de optie toevoegen gemarkeerd](./media/adobe-echosign-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:
 
    ![Schermopname van dialoogvenster](./media/adobe-echosign-tutorial/create_aaduser_04.png) 

    a. In de **naam** in het tekstvak **BrittaSimon**.

    b. In de **gebruikersnaam** tekst typt u het e-mailadres van BrittaSimon.

    c. Selecteer **wachtwoord weergeven**, en noteer de waarde van de **wachtwoord**.

    d. Selecteer **Maken**.
 
### <a name="create-an-adobe-sign-test-user"></a>Maak een testgebruiker Adobe Sign

Als u wilt dat Azure AD-gebruikers kunnen zich aanmelden bij Adobe Sign, moeten ze worden ingericht voor Adobe Sign. Dit is een handmatige taak.

>[!NOTE]
>U kunt een andere hulpprogramma's voor Adobe Sign gebruiker-account maken of API's uit Adobe Sign gebruiken voor het inrichten van gebruikersaccounts van de Azure AD. 

1. Aanmelden bij uw **Adobe Sign** bedrijf site als beheerder.

2. Selecteer in het menu aan de bovenkant, **Account**. Selecteer in het linkerdeelvenster **gebruikers en groepen** > **maken van een nieuwe gebruiker**.
   
    ![Schermafbeelding van Adobe Sign bedrijfsportal-website met het Account, gebruikers en groepen, en maak een nieuwe gebruiker gemarkeerd](./media/adobe-echosign-tutorial/ic789524.png "Account")
   
3. In de **nieuwe gebruiker maken** sectie, voert u de volgende stappen uit:
   
    ![Schermafbeelding van de nieuwe gebruiker maken sectie](./media/adobe-echosign-tutorial/ic789525.png "gebruiker maken")
   
    a. Type de **e-mailadres**, **voornaam**, en **achternaam** van een geldige Azure AD-account die u inrichten in de bijbehorende tekstvakken wilt.
   
    b. Selecteer **gebruiker maken**.

>[!NOTE]
>De houder van Azure Active Directory-account ontvangt een e-mailbericht een koppeling om te bevestigen dat het account bevat voordat deze geactiveerd wordt. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding, door toegang te verlenen aan Adobe Sign.

![Schermafbeelding van de Azure portal eenmalige aanmelding][200] 

1. Open de toepassingen in de Azure-portal. Blader vervolgens naar de map weergeven, gaat u naar **bedrijfstoepassingen**, en selecteer **alle toepassingen**.

    ![Schermafbeelding van de Azure portal toepassingen weergeven, met zakelijke toepassingen en alle toepassingen die zijn gemarkeerd][201] 

2. Selecteer in de lijst met toepassingen, **Adobe Sign**.

    ![Schermafbeelding van de lijst met toepassingen, met Adobe Sign gemarkeerd](./media/adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. Selecteer in het menu aan de linkerkant, **gebruikers en groepen**.

    ![Schermopname van menu met gebruikers en groepen die zijn gemarkeerd][202] 

4. Selecteer **Toevoegen**. Klik in de **toevoegen toewijzing** sectie, selecteer **gebruikers en groepen**.

    ![Schermopname van gebruikers en groepen-pagina- en toewijzing toevoegen][203]

5. In **gebruikers en groepen** in het dialoogvenster, in de gebruikerslijst, selecteer **Britta Simon**.

6. In de **gebruikers en groepen** in het dialoogvenster, klikt u op **Selecteer**.

7. In de **toevoegen toewijzing** in het dialoogvenster, selecteer **toewijzen**.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Wanneer u de tegel Adobe Sign in het toegangsvenster selecteert, moet u ophalen automatisch aangemeld bij uw toepassing Adobe Sign. Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/adobe-echosign-tutorial/tutorial_general_203.png
