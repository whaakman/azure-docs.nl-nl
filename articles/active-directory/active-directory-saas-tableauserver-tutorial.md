---
title: 'Zelfstudie: Azure Active Directory-integratie met Tableau Server | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Tableau-Server.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 6b35609d88fbbf649e15863901d521886db2a4d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Zelfstudie: Azure Active Directory-integratie met Tableau Server

In deze zelfstudie leert u hoe Tableau Server integreren met Azure Active Directory (Azure AD).

Tableau Server integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de Server Tableau heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Tableau Server (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Tableau Server, moet u de volgende items:

- Een Azure AD-abonnement
- Een Server Tableau eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Tableau Server uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-tableau-server-from-the-gallery"></a>Tableau Server uit de galerie toevoegen
Voor het configureren van de integratie van Tableau Server in Azure AD, moet u Tableau Server uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Tableau Server uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Tableau Server**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_search.png)

5. Selecteer in het deelvenster resultaten **Tableau Server**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met Tableau-Server op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in Tableau Server in Azure AD voor een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in Tableau Server tot stand worden gebracht.

Wijs in Tableau-Server de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Tableau Server, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker Tableau Server](#creating-a-tableau-server-test-user)**  - Tableau-Server die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing Tableau Server configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met Tableau Server, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Tableau Server** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_samlbase.png)

3. Op de **Tableau-serverdomein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen:`https://azure.<domain name>.link`
    
    b. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://azure.<domain name>.link`

    c. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://azure.<domain name>.link/wg/saml/SSO/index.html`
     
    > [!NOTE] 
    > De voorgaande waarden zijn niet echte waarden. Later kunt bijwerken u de waarden met de werkelijke URL en de id van de configuratiepagina Tableau Server. 

4. De SAML-asserties verwacht tableau servertoepassing in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken van beheren de **'Gebruikerskenmerken'** sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor dezelfde.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/3.png)
    
5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding hierboven en voer de volgende stappen uit:
    
    | Naam van kenmerk | De waarde van kenmerk |
    | ---------------| --------------- |    
    | gebruikersnaam | *User.DisplayName* |

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_04.png)

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_05.png)
    
    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **Ok**


6. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_certificate.png) 

7. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS>
8. Als u eenmalige aanmelding voor uw toepassing is geconfigureerd, moet u eenmalige aanmelding voor uw tenant Tableau Server als beheerder.
   
   a. In de serverconfiguratie Tableau, klikt u op de **SAML** tabblad.
  
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 
  
   b. Schakel het selectievakje van **gebruik SAML voor eenmalige aanmelding**.
   
   c. Tableau Server retour-URL, de URL die Tableau Server-gebruikers toegang, zoals http://tableau_server tot krijgen. Gebruik http://localhost wordt niet aanbevolen. Via een URL met een slash (bijvoorbeeld http://tableau_server/) wordt niet ondersteund. Kopiëren **Tableau Server retour-URL** en plak deze naar Azure AD **aanmelding op URL** textbox in **Tableau-serverdomein en URL's** sectie.
   
   d. SAML-entiteit-ID, de entiteit-ID is uniek voor de serverinstallatie van Tableau aan de IdP. Hier geeft u de URL van de Server Tableau opnieuw, indien gewenst, maar deze hoeft niet te worden van de URL van de Server Tableau. Kopiëren **SAML entiteit-ID** en plak deze naar Azure AD **id** textbox in **Tableau-serverdomein en URL's** sectie.
     
   e. Klik op de **metagegevensbestand exporteren** en open het in de toepassing van de editor voor tekst. Zoek Assertion Consumer Service-URL met Http Post en 0-Index en kopieer de URL. Plak nu naar Azure AD **antwoord-URL** textbox in **Tableau-serverdomein en URL's** sectie.
   
   f. Zoek uw Federatiemetagegevens bestand gedownload van Azure-portal en uploadt u dit in de **SAML Idp metagegevensbestand**.
   
   g. Klik op de **OK** knop op de pagina serverconfiguratie Tableau.
   
    >[!NOTE] 
    >Klant hebben voor het uploaden van een certificaat in de SSO-Tableau Server SAML-configuratie en het in de SSO-stroom ophalen genegeerd.
    >Als u moet helpen SAML configureren op de Server Tableau Raadpleeg dit artikel [SAML configureren](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).
    >
<CE>

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-tableau-server-test-user"></a>Een testgebruiker Tableau Server maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in Tableau-Server. U moet alle gebruikers in de server Tableau inrichten. 

Deze gebruikersnaam van de gebruiker moet overeenkomen met de waarde die u hebt geconfigureerd in het aangepaste kenmerk Azure AD van **gebruikersnaam**. Met de juiste toewijzing de integratie moet werken [eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on).

>[!NOTE]
>Als u een gebruiker handmatig maken wilt, moet u contact op met de serverbeheerder Tableau in uw organisatie.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Tableau Server.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen Tableau Server, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Tableau Server**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel Tableau Server in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing Tableau-Server.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png

