---
title: 'Zelfstudie: Azure Active Directory-integratie met Citrix Netscaler | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory- en Citrix Netscaler.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: af501bd0-8ff5-468f-9b06-21e607ae25de
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: ca5fec00d264bb50c75338d36ec793946f495bff
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58227324"
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-netscaler"></a>Zelfstudie: Azure Active Directory-integratie met Citrix Netscaler

In deze zelfstudie leert u hoe u Citrix Netscaler integreren met Azure Active Directory (Azure AD).
Citrix Netscaler integreren met Azure AD biedt u de volgende voordelen:

* U kunt beheren in Azure AD die toegang tot Citrix Netscaler heeft.
* U kunt uw gebruikers worden automatisch aangemeld Citrix Netscaler (Single Sign-On) inschakelen met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Citrix Netscaler, moet u de volgende items:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* Citrix Netscaler eenmalige aanmelding ingeschakeld abonnement

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Biedt ondersteuning voor Citrix Netscaler **SP** gestart door SSO

* Biedt ondersteuning voor Citrix Netscaler **Just In Time** inrichten van gebruikers

## <a name="adding-citrix-netscaler-from-the-gallery"></a>Citrix Netscaler uit de galerie toe te voegen

Voor het configureren van de integratie van Citrix Netscaler in Azure AD, moet u Citrix Netscaler vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Citrix Netscaler vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **Citrix Netscaler**, selecteer **Citrix Netscaler** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

     ![Citrix Netscaler in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Citrix Netscaler op basis van een testgebruiker met de naam **Britta Simon**.
Voor eenmalige aanmelding om te werken, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Citrix Netscaler tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met Citrix Netscaler, moet u de volgende bouwstenen voltooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Configureer Citrix Netscaler Single Sign-On](#configure-citrix-netscaler-single-sign-on)**  : als u wilt de Single Sign-On-instellingen configureren op de toepassing aan clientzijde.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Citrix Netscaler testgebruiker maken](#create-citrix-netscaler-test-user)**  : als u wilt een equivalent van Britta Simon in Citrix Netscaler die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voor het configureren van Azure AD eenmalige aanmelding met Citrix Netscaler, moet u de volgende stappen uitvoeren:

1. In de [Azure-portal](https://portal.azure.com/)op de **Citrix Netscaler** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Citrix Netscaler domein en URL's, eenmalige aanmelding informatie](common/sp-identifier-reply.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<<Your FQDN>>/CitrixAuthService/AuthService.asmx`
    
    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met de volgende notatie: `https://<<Your FQDN>>`

    c. In de **antwoord-URL (URL van de Bevestigingsconsumerservice)** tekstvak typt u een URL met behulp van het volgende patroon: `https://<<Your FQDN>>/CitrixAuthService/AuthService.asmx`
    
    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en id. Neem contact op met [Citrix Netscaler Client ondersteuningsteam](https://www.citrix.com/contact/technical-support.html) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

    > [!NOTE]
    > Om eenmalige aanmelding werkt, moet deze URL's toegankelijk is vanaf openbare sites. U moet de firewall of andere beveiligingsinstellingen op de Netscaler zijde enble Azure AD voor het plaatsen van het token op de geconfigureerde ACS-URL inschakelen.

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens**  te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. Op de **instellen van Citrix Netscaler** sectie, kopieert u de juiste URL('s) volgens uw behoeften.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-citrix-netscaler-single-sign-on"></a>Configureer Citrix Netscaler Single Sign-On

1. In een ander browservenster aanmelden voor uw tenant Citrix Netscaler als beheerder.

2. Zorg ervoor dat de **NetScaler firmwareversie = NS12.1: Build 48.13.nc**.

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure01.png)

3. Op de **virtuele VPN-Server** pagina, voert u de volgende stappen uit:

     ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure02.png)

    a. Gateway-instellingen instellen **ICA alleen** als **waar**.
    
    b. Stel **Authentication inschakelen** als **waar**.
    
    c. **DTLS** is optioneel.
    
    d. Zorg ervoor dat **SSLv3** als **uitgeschakelde**.

4. Een aangepaste **SSL-codering** groep is gemaakt als u wilt bereiken A + op https://www.ssllabs.com zoals hieronder wordt weergegeven:

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure03.png)

5. Op de **Authentication SAML-Server configureren** pagina, voert u de volgende stappen uit:

      ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure04.png)

    a. In de **naam** tekstvak typt u de naam van uw server.

    b. In de **Omleidings-URL** tekstvak, plak de waarde van **aanmeldings-URL** die u hebt gekopieerd vanuit Azure portal.

    c. In de **URL voor eenmalige afmelding** tekstvak, plak de waarde van **afmeldings-URL van** die u hebt gekopieerd vanuit Azure portal.

    d. In **IDP certificaatnaam**, klikt u op de **'+'** zich bij het toevoegen van het certificaat dat u hebt gedownload vanuit Azure portal. Selecteer het certificaat in de vervolgkeuzelijst nadat deze is geüpload.

    e. Volgende meer velden moeten worden ingesteld op deze pagina

      ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure24.png)

    f. Selecteer **aangevraagd verificatiecontext** als **exacte**.

    g. Selecteer **handtekeningalgoritme** als **RSA-SHA256**.

    h. Selecteer **Digest-methode** als **SHA256**.

    i. Controleer **afdwingen gebruikersnaam**.

    j. Klik op **OK**

6. Het configureren van de **sessieprofiel**, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure06.png)

    a. In de **naam** tekstvak typt u de naam van uw sessieprofiel.

    b. Op de **Clientervaring** tabblad, de wijzigingen aanbrengen, zoals wordt weergegeven in de onderstaande schermafbeelding.

    c. Doorgaan met de wijzigingen aanbrengen op de **tabblad Algemeen** zoals hieronder wordt weergegeven en klik op **OK**

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure07.png)

    d. Op de **gepubliceerde toepassingen** tabblad, de wijzigingen aanbrengen, zoals wordt weergegeven in de onderstaande schermafbeelding en op **OK**.

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure08.png)

    e. Op de **Security** tabblad, de wijzigingen aanbrengen, zoals wordt weergegeven in de onderstaande schermafbeelding en op **OK**.

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure09.png)

7. Maak de verbinding te maken op sessie betrouwbaarheid poort ICA-verbindingen **2598** zoals wordt weergegeven in de schermafbeelding hieronder.

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure10.png)

8. Op de **SAML** sectie, voegt de **Servers** zoals wordt weergegeven in de onderstaande schermafbeelding.

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure11.png)

9. Op de **SAML** sectie, voegt de **beleid** zoals wordt weergegeven in de onderstaande schermafbeelding.

     ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure12.png)

10. Op de **globale instellingen** pagina, Ga naar de **clientloze toegang** sectie.

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure13.png)

11. Op de **configuratie** tabblad, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure14.png)

    a. Selecteer **domeinen toestaan**.

    b. In de **domeinnaam** textbox, selecteert u het domein.

    c. Klik op **OK**.

12. Controleer de **StoreFront** instellingen op de **ontvanger voor Web Sites** zoals wordt weergegeven in de onderstaande schermafbeelding:

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure15.png)

13. Op de **verificatiemethoden beheren - Corp** pop, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure16.png)

    a. Selecteer **gebruikersnaam en wachtwoord**.

    b. Selecteer **Pass Through-van NetScaler Gateway**.

    c. Klik op **OK**.

14. Op de **vertrouwde domeinen configureren** pop, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure17.png)

    a. Selecteer **vertrouwde domeinen alleen**.

    b. Klik op **toevoegen** om toe te voegen van uw domein in **vertrouwde domeinen** tekstvak.

    c. Selecteer standaarddomein van uw **standaarddomein** lijst.

    d. Selecteer **lijst met domeinen weergeven in de aanmeldingspagina**.

    e. Klik op **OK**.

15. Op de **NetScaler Gateways beheren** pop, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure18.png)

    a. Klik op **toevoegen** om toe te voegen uw Gateways NetScaler in **NetScaler Gateways** tekstvak.

    b. Klik op **Sluiten**.

16. Op de **StoreFront algemene instellingen** tabblad, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure19.png)

    a. In de **weergavenaam** tekstvak typt u de naam van uw NetScaler Gateway.

    b. In de **URL van de Gateway NetScaler** tekstvak typt u de URL van de Gateway NetScaler.

    c. Selecteer **gebruik of rol** als **verificatie en uitlezen van HDX routering**.

    d. Klik op **OK**.

17. Op de **StoreFront Secure Ticket autoriteit** tabblad, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure20.png)

    a. Klik op **toevoegen** om toe te voegen uw **Secure Ticket de URL van instantie van** in het tekstvak.

    b. Selecteer **inschakelen sessie betrouwbaarheid**.

    c. Klik op **OK**.

18. Op de **StoreFront verificatie-instellingen** tabblad, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure21.png)

    a. Selecteer uw **versie**.

    b. Selecteer **aanmeldingstype** als **domein**.

    c. Voer uw **URL voor terugbellen**.

    d. Klik op **OK**.

19. Op de **StoreFront implementeren Citrix ontvanger** tabblad, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure22.png)

    a. Selecteer **Implementatieoptie** als **gebruik ontvanger voor HTML5 als lokale ontvanger niet beschikbaar is**.

    b. Klik op **OK**.

20. Op de **beheren Beacons** pop, voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/citrix-netscaler-tutorial/configure23.png)

    a. Selecteer de **interne aangeven via baken** als **gebruikt u de service-URL**.

    b. Klik op **toevoegen** om toe te voegen in uw de URL van de **externe beacons** tekstvak.

    c. Klik op **OK**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **Nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam** **Britta Simon**in.
  
    b. In het veld **Gebruikersnaam** typt u **brittasimon@yourcompanydomain.extension**.  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Citrix Netscaler.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Citrix Netscaler**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst met toepassingen, **Citrix Netscaler**.

    ![De Citrix Netscaler-koppeling in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoogvenster **Gebruikers en groepen** **Britta Simon** in de lijst met gebruikers en klik op de knop **Selecteren** onder aan het scherm.

6. Als u een waarde voor een rol verwacht in de SAML-bewering, moet u in het dialoogvenster **Rol selecteren** de juiste rol voor de gebruiker in de lijst selecteren en vervolgens op de knop **Selecteren** onder aan het scherm klikken.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-citrix-netscaler-test-user"></a>Citrix Netscaler testgebruiker maken

In deze sectie wordt een gebruiker met de naam Britta Simon gemaakt in Citrix Netscaler. Citrix Netscaler biedt ondersteuning voor just-in-time-gebruikersinrichting, dat standaard is ingeschakeld. Er is geen actie-item voor u in deze sectie. Als een gebruiker nog niet in Citrix Netscaler bestaat, wordt een nieuw gemaakt nadat verificatie.

>[!NOTE]
>Als u een gebruiker handmatig hebt gemaakt wilt, moet u contact op met de [Citrix Netscaler Client ondersteuningsteam](https://www.citrix.com/contact/technical-support.html).

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Citrix Netscaler in het toegangsvenster, moet u worden automatisch aangemeld bij de Citrix Netscaler waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) (Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

