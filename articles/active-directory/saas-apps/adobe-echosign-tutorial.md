---
title: 'Zelfstudie: Azure Active Directory-integratie met Adobe teken | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Adobe aanmelding.
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
ms.openlocfilehash: 9a1a1f1a1866e5221775d583a9bafe86eef17131
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221220"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Zelfstudie: Azure Active Directory-integratie met Adobe teken

In deze zelfstudie leert u hoe Adobe aanmelding integreren met Azure Active Directory (Azure AD).

Meld u Adobe integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Adobe aanmelding heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij de aanmelding Adobe (eenmalige aanmelding) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie--de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Adobe teken, hebt u het volgende nodig:

- Een Azure AD-abonnement
- Een Adobe aanmelding eenmalige aanmelding ingeschakeld abonnement

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Adobe teken uit de galerie toevoegen.
2. Configureren en testen van Azure AD eenmalige aanmelding.

## <a name="add-adobe-sign-from-the-gallery"></a>Adobe teken uit de galerie toevoegen
Voor het configureren van de integratie van Adobe aanmelding in Azure AD, moet u Adobe teken uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram. 

    ![Schermopname van het Azure Active Directory-pictogram][1]

2. Blader naar **bedrijfstoepassingen** > **alle toepassingen**.

    ![Schermopname van Azure Active Directory-menu's met bedrijfstoepassingen en alle toepassingen die zijn gemarkeerd][2]
    
3. Selecteer om een nieuwe toepassing toe **nieuwe toepassing** aan de bovenkant van het dialoogvenster.

    ![Schermafbeelding van de nieuwe toepassing optie aan de bovenkant van het dialoogvenster][3]

4. Typ in het zoekvak **Adobe aanmelding**.

    ![Schermopname van het zoekvak](./media/adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. Selecteer in het deelvenster resultaten **Adobe aanmelding**, en selecteer vervolgens **toevoegen**.

    ![Schermopname van het resultatenpaneel](./media/adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Adobe teken, op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD voor het herkennen van een gekoppelde relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Adobe aanmelding.

Om vast te stellen de gekoppelde relatie in Adobe aanmelding, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam**.

Om te configureren en testen van Azure AD eenmalige aanmelding met Adobe teken, voert u de volgende elementen:

1. [Eenmalige aanmelding Azure AD configureren](#configuring-azure-ad-single-sign-on) zodat uw gebruikers om deze functie te gebruiken.
2. [Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user) voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. [Maken van een testgebruiker Adobe aanmelding](#creating-an-adobe-sign-test-user) hebben een equivalent van Britta Simon in Adobe aanmelding die gekoppeld is aan de Azure AD-weergave van de gebruiker.
4. [Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user) Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. [Test eenmalige aanmelding](#testing-single-sign-on) om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie kunt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing Adobe aanmelding.

1. In de Azure-portal op de **Adobe aanmelding** toepassing Integratiepagina **eenmalige aanmelding**.

    ![Schermopname van Adobe aanmelding Integratiepagina toepassing met eenmalige aanmelding gemarkeerd][4]

2. Op de **eenmalige aanmelding** in het dialoogvenster voor **modus**, selecteer **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Schermopname van eenmalige aanmelding in het dialoogvenster met modus in gemarkeerd](./media/adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. In de **Adobe aanmelding domein en de URL's** sectie, voert u de volgende stappen uit:

    ![Schermopname van Adobe aanmelding domein en URL's sectie](./media/adobe-echosign-tutorial/tutorial_adobesign_url.png)

    a. In de **aanmeldings-URL** tekstvak, typt u een URL die gebruikmaakt van het volgende patroon volgen: `https://<companyname>.echosign.com/`

    b. In de **id** tekstvak, typt u een URL die gebruikmaakt van het volgende patroon volgen: `https://<companyname>.echosign.com`

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke aanmeldings-URL en de id. Neem contact op met [Adobe aanmelding Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html) ophalen van deze waarden.

4. In de **SAML-certificaat voor ondertekening van** sectie **Certificate(Base64)**, en sla het certificaatbestand op uw computer.

    ![Schermopname van SAML certificaat voor ondertekening van sectie](./media/adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. Selecteer **Opslaan**.

    ![Knop schermafbeelding opslaan](./media/adobe-echosign-tutorial/tutorial_general_400.png)

6. In de **Adobe aanmelding configuratie** sectie **Adobe aanmelding configureren** openen de **eenmalige aanmelding configureren** venster. Kopiëren de **Sign-Out URL**, **SAML entiteit-ID**, en **SAML Single Sign-On Service-URL** van de **Naslaggids** sectie.

    ![Schermopname van Adobe aanmelding configuratiesectie, met het configureren van Adobe teken gemarkeerd](./media/adobe-echosign-tutorial/tutorial_adobesign_configure.png)

7. Voordat de configuratie, neem contact op met de [Adobe aanmelding Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html) naar geaccepteerde uw domein in het Adobe-teken. Dit is het toevoegen van het domein:

    a. De [Adobe aanmelding Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html) stuurt u een willekeurig gegenereerde token. Uw domein, is het token als volgt: **adobe aanmelding verifiëren xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx =**

    b. Het verificatietoken publiceren in een DNS-record voor tekst en melden de [Adobe aanmelding Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html).
    
    > [!NOTE]
    > Dit kan een paar duren dagen of langer. Houd er rekening mee dat het vertragingen bij het doorgeven van de DNS-betekent dat een waarde die is gepubliceerd in DNS niet mogelijk zichtbaar voor een uur of langer. Uw IT-beheerder moet grondige kennis van hoe u dit token publiceert in een DNS-record voor tekst.
    
    c. Wanneer u de hoogte stellen de [Adobe aanmelding Client ondersteuningsteam](https://helpx.adobe.com/in/contact/support.html) via de ondersteuningsticket nadat het token is gepubliceerd, ze valideren van het domein en toe te voegen aan uw account.
    
    d. Dit is doorgaans het publiceren van het token op een DNS-record:

    * Aanmelden bij uw domeinaccount
    * De pagina voor het bijwerken van de DNS-record vinden. Deze pagina kan worden aangeroepen voor DNS-beheer, de naam van Serverbeheer of geavanceerde instellingen.
    * De TXT-records vinden voor uw domein.
    * Voeg een TXT-record met de volledige token waarde die is geleverd door Adobe.
    * Sla uw wijzigingen op.

8. In een ander browservenster aanmelden bij uw bedrijf Adobe aanmelding site als beheerder.

9. Selecteer in het menu SAML **Accountinstellingen** > **SAML instellingen**.
   
    ![Schermopname van Adobe aanmelding SAML instellingenpagina](./media/adobe-echosign-tutorial/ic789520.png "Account")

10. In de **SAML instellingen** sectie, voert u de volgende stappen uit:
  
    ![Schermopname van SAML-instellingen](./media/adobe-echosign-tutorial/ic789521.png "SAML-instellingen")
   
    a. Onder **SAML modus**, selecteer **SAML verplichte**.
   
    b. Selecteer **Echosign-accountbeheerders toestaan om aan te melden met hun referenties Echosign**.
   
    c. Onder **maken van een gebruikersaccount**, selecteer **automatisch toevoegen van gebruikers worden geverifieerd via SAML**.

    d. Plakken **SAML entiteit-ID**, die u hebt gekopieerd vanuit de Azure-portal in de **URL van de entiteit-ID/verlener** in het tekstvak.
    
    e. Plakken **SAML Single Sign-On Service-URL**, die u hebt gekopieerd vanuit Azure-portal in de **aanmelding URL/SSO eindpunt** in het tekstvak.
   
    f. Plakken **Sign-Out URL**, die u hebt gekopieerd vanuit de Azure-portal in de **afmelding URL/SLO eindpunt** in het tekstvak.

    g. Open uw gedownloade **Certificate(Base64)** bestand in Kladblok. Kopieer de inhoud ervan naar het Klembord en plakt u deze naar de **IdP certificaat** in het tekstvak.

    h. Selecteer **wijzigingen opslaan**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het maken van een testgebruiker Britta Simon, met de naam in de Azure portal.

![Schermafbeelding van de test-gebruikersnaam in de Azure portal][100]

1. In de **Azure-portal**, selecteer in het linkerdeelvenster de **Azure Active Directory** pictogram.

    ![Schermopname van het Azure AD-pictogram](./media/adobe-echosign-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer **alle gebruikers**.
    
    ![Schermopname van Azure AD-menu's met gebruikers en groepen en alle gebruikers die zijn gemarkeerd](./media/adobe-echosign-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, **toevoegen**.
 
    ![Schermopname van boven aan alle gebruikers in het dialoogvenster met de optie toevoegen gemarkeerd](./media/adobe-echosign-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:
 
    ![Schermopname van dialoogvenster](./media/adobe-echosign-tutorial/create_aaduser_04.png) 

    a. In de **naam** in het tekstvak **BrittaSimon**.

    b. In de **gebruikersnaam** tekst BrittaSimon van het e-mailadres typt.

    c. Selecteer **wachtwoord weergeven**, en noteer de waarde van de **wachtwoord**.

    d. Selecteer **Maken**.
 
### <a name="create-an-adobe-sign-test-user"></a>Een testgebruiker Adobe aanmelding maken

Om Azure AD-gebruikers aan te melden bij Adobe aanmelding, moeten ze worden ingericht in Adobe aanmelding. Dit is een handmatige taak.

>[!NOTE]
>Kunt u geen andere hulpprogramma's van Adobe aanmelding gebruiker-account maken of API's die worden geleverd door Adobe aanmelding voor het inrichten van Azure AD-gebruikersaccounts. 

1. Aanmelden bij uw **Adobe aanmelding** bedrijf site als beheerder.

2. Selecteer in het menu bovenaan **Account**. Selecteer in het linkerdeelvenster **gebruikers en groepen** > **Maak een nieuwe gebruiker**.
   
    ![Schermopname van Adobe aanmelding site met een Account, gebruikers en groepen uit de bedrijfsportal en maak een nieuwe gebruiker gemarkeerd](./media/adobe-echosign-tutorial/ic789524.png "Account")
   
3. In de **nieuwe gebruiker maken** sectie, voert u de volgende stappen uit:
   
    ![Schermafbeelding van de nieuwe gebruiker maken sectie](./media/adobe-echosign-tutorial/ic789525.png "gebruiker maken")
   
    a. Typ de **e-mailadres**, **voornaam**, en **achternaam** van een geldig Azure AD-account die u inrichten in de bijbehorende tekstvakken wilt.
   
    b. Selecteer **gebruiker maken**.

>[!NOTE]
>De houder van Azure Active Directory-account ontvangt een e-mailbericht een koppeling om te bevestigen van het account bevat voordat deze geactiveerd wordt. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding, toegang te verlenen om de Adobe te ondertekenen.

![Schermopname van Azure portal eenmalige aanmelding][200] 

1. Open de toepassingen in de Azure portal. Blader vervolgens naar de map weergeven, gaat u naar **bedrijfstoepassingen**, en selecteer **alle toepassingen**.

    ![Schermopname van Azure portal toepassingen weergeven, met bedrijfstoepassingen en alle toepassingen die zijn gemarkeerd][201] 

2. Selecteer in de lijst met toepassingen **Adobe aanmelding**.

    ![Schermafbeelding van de lijst met toepassingen, met Adobe aanmelding gemarkeerd](./media/adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. Selecteer in het menu aan de linkerkant **gebruikers en groepen**.

    ![Schermopname van menu met gebruikers en groepen die zijn gemarkeerd][202] 

4. Selecteer **Toevoegen**. Klik in de **toevoegen toewijzing** sectie **gebruikers en groepen**.

    ![Schermopname van gebruikers en groepen pagina en toewijzing sectie toevoegen][203]

5. In **gebruikers en groepen** in het dialoogvenster in de gebruikerslijst selecteert **Britta Simon**.

6. In de **gebruikers en groepen** in het dialoogvenster, klikt u op **Selecteer**.

7. In de **toevoegen toewijzing** dialoogvenster, **toewijzen**.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Wanneer u de tegel Adobe aanmelding in het deelvenster toegang selecteert, moet u ophalen automatisch aangemeld op uw programma Adobe ondertekenen. Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

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
