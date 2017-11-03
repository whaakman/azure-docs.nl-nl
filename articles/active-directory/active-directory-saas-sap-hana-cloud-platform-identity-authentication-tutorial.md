---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP Cloud Platform identiteitsverificatie | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en identiteitsverificatie voor SAP Cloud-Platform.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: jeedes
ms.openlocfilehash: e4e7c7273acc216ae82a52b3e6dcd530a6ad1bd7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform-identity-authentication"></a>Zelfstudie: Azure Active Directory-integratie met SAP Cloud Platform identiteitsverificatie

In deze zelfstudie leert u hoe SAP Cloud Platform identiteitsverificatie integreren met Azure Active Directory (Azure AD). SAP Cloud Platform-ID-verificatie wordt gebruikt als een IdP-proxy voor toegang tot SAP-toepassingen met Azure AD als de belangrijkste IdP.

SAP Cloud Platform identiteitsverificatie integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot SAP-toepassing heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij SAP-toepassingen eenmalige aanmelding (SSO) inschakelen) met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met SAP Cloud Platform-ID-verificatie, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAP Cloud Platform identiteitsverificatie eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP Cloud Platform identiteitsverificatie uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

Vooraleer technische informatie, is het essentieel om te begrijpen van de concepten die u van plan bent om te kijken. De Federatie SAP Cloud Platform-ID-verificatie en Azure Active Directory kunt u voor het implementeren van eenmalige aanmelding via toepassingen of services die zijn beveiligd door AAD (als een IdP) met SAP-toepassingen en services die zijn beveiligd door de identiteit van het Platform voor SAP-Cloud -Verificatie.

Op dit moment fungeert SAP Cloud Platform identiteitsverificatie als een Proxy-id-Provider voor SAP-toepassingen. Azure Active Directory wordt op zijn beurt fungeert als de toonaangevende id-Provider in deze installatie. 

Het volgende diagram illustreert dit:

![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sapcloudauth-tutorial/architecture-01.png)

Met deze instelling wordt uw tenant SAP Cloud Platform identiteitsverificatie worden geconfigureerd als een vertrouwde toepassing in Azure Active Directory. 

Alle SAP-toepassingen en services die u wilt beveiligen via deze manier worden vervolgens geconfigureerd in de beheerconsole voor SAP Cloud Platform identiteitsverificatie!

Dit betekent dat de autorisatie voor het verlenen van toegang tot SAP-toepassingen en services plaatsvinden in SAP Cloud Platform identiteitsverificatie voor dergelijke installatie moet (tegengesteld autorisatie configureren in Azure Active Directory).

SAP Cloud Platform identiteitsverificatie configureert als een toepassing via de Azure Active Directory Marketplace, u hoeft niet te behandelen van het configureren van benodigde afzonderlijke claims / SAML asserties en transformaties die nodig zijn voor het produceren van een geldige verificatietoken voor SAP-toepassingen.

>[!NOTE] 
>Web-SSO is momenteel getest door beide partijen alleen. Stromen nodig voor App-naar-API of API-API-communicatie moeten werken, maar zijn niet getest, nog. Ze worden getest als onderdeel van de volgende activiteiten.
>

## <a name="adding-sap-cloud-platform-identity-authentication-from-the-gallery"></a>SAP Cloud Platform identiteitsverificatie uit de galerie toevoegen
Voor het configureren van de integratie van SAP Cloud Platform identiteitsverificatie in Azure AD, moet u SAP Cloud Platform identiteitsverificatie uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SAP Cloud Platform identiteitsverificatie uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **SAP Cloud Platform identiteitsverificatie**, selecteer **SAP Cloud Platform identiteitsverificatie** van resultaat deelvenster klik vervolgens op **toevoegen** knop de toepassing toevoegen.

    ![SAP Cloud Platform identiteitsverificatie in de lijst met resultaten](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met SAP Cloud Platform identiteitsverificatie op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in SAP Cloud Platform identiteitsverificatie in Azure AD voor een gebruiker is. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in SAP Cloud Platform identiteitsverificatie tot stand worden gebracht.

Wijs in SAP Cloud Platform identiteitsverificatie de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met SAP Cloud Platform identiteitsverificatie, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een testgebruiker SAP Cloud Platform identiteitsverificatie](#create-an-sap-cloud-platform-identity-authentication-test-user)**  - SAP Cloud Platform verificatie van de identiteit die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw toepassing identiteitsverificatie voor SAP Cloud-Platform.

**Voor het configureren van Azure AD eenmalige aanmelding met SAP Cloud Platform-ID-verificatie, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **SAP Cloud Platform identiteitsverificatie** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_samlbase.png)

3. Op de **SAP Cloud Platform identiteit verificatiedomein en de URL's** sectie als u wilt configureren van de toepassing in **IDP** modus gestart:

    ![URL's en SAP Cloud Platform Identity verificatie Domain eenmalige aanmelding informatie](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_url.png)

    In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<entity-id>.accounts.ondemand.com`

    > [!NOTE] 
    > Deze waarde is geen echte. Deze waarde door de werkelijke ID bijwerken. Neem contact op met [SAP Cloud Platform Identity-verificatieclient ondersteuningsteam](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) deze waarde op te halen. Als u deze waarde niet weet, volg de documentatie SAP Cloud Platform identiteitsverificatie op [Tenantconfiguratie SAML 2.0](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

4. Controleer **weergeven geavanceerde instellingen voor URL**. Als u wilt configureren van de toepassing in **SP** modus gestart:

    ![URL's en SAP Cloud Platform Identity verificatie Domain eenmalige aanmelding informatie](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_url1.png)

    In de **aanmelding op URL** textbox, typ een URL met het volgende patroon volgen:`https://<entity-id>.accounts.ondemand.com/admin`

    > [!NOTE] 
    > Deze waarde is geen echte. Deze waarde bijwerken met de werkelijke URL voor eenmalige aanmelding. Neem contact op met [SAP Cloud Platform Identity-verificatieclient ondersteuningsteam](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) deze waarde op te halen.

5. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_certificate.png)

6. De SAML-asserties verwacht SAP Cloud Platform identiteitsverificatie toepassing in een specifieke indeling. U kunt beheren de waarden van deze kenmerken van de '**gebruikerskenmerken**' sectie op de pagina van de toepassing-integratie. De volgende Schermafbeelding toont een voorbeeld voor deze. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapcloudauth-tutorial/attribute.png)

7. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster als uw SAP-toepassing een kenmerk bijvoorbeeld 'firstName' wordt verwacht. Voeg het 'firstName'-kenmerk in het dialoogvenster SAML-token kenmerken.

    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_attribute_04.png)
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_attribute_05.png)
    
    b. In de **naam** textbox, typt u het kenmerk name 'firstName'.
    
    c. Van de **waarde** , selecteert u de waarde van het kenmerk 'user.givenname'.
    
    d. Klik op **OK**.

8. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_400.png)

9. Op de **SAP Cloud Identity verificatie platformconfiguratie** sectie, klikt u op **configureren SAP Cloud Platform identiteitsverificatie** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![SAP Cloud Platform identiteit verificatie-configuratie](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_configure.png) 

10. Als u eenmalige aanmelding die zijn geconfigureerd voor uw toepassing, gaat u naar SAP Cloud Platform identiteit verificatie-beheerconsole. De URL is het volgende patroon volgen: `https://<tenant-id>.accounts.ondemand.com/admin`. Volg de documentatie op SAP Cloud Platform identiteitsverificatie naar [Configure Microsoft Azure AD als id-Provider op SAP Cloud Platform identiteitsverificatie zakelijke](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

11. Klik in de Azure-portal op **opslaan** knop.

12. De volgende stappen blijven alleen als u wilt toevoegen en eenmalige aanmelding inschakelen voor een andere SAP-toepassing. Herhaal de stappen in de sectie 'Toevoegen SAP Cloud Platform identiteitsverificatie uit de galerie' een ander exemplaar van SAP Cloud Platform identiteitsverificatie toevoegen.

13. In de Azure-portal op de **SAP Cloud Platform identiteitsverificatie** toepassing Integratiepagina, klikt u op **gekoppelde aanmelding**.

    ![Gekoppelde aanmelding configureren](./media/active-directory-saas-sapcloudauth-tutorial/linked_sign_on.png)

14. De configuratie op te slaan.

>[!NOTE] 
>De nieuwe toepassing zal gebruikmaken van de SSO-configuratie voor de vorige SAP-toepassing. Controleer of dat u dezelfde zakelijke id-Providers gebruiken in de beheerconsole SAP Cloud Platform identiteit verificatie.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/active-directory-saas-sapcloudauth-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-sap-cloud-platform-identity-authentication-test-user"></a>Een SAP Cloud Platform identiteitsverificatie testgebruiker maken

U hoeft niet te maken van een gebruiker op identiteitsverificatie voor SAP Cloud-Platform. Gebruikers die zich in het archief van de Azure AD-gebruiker kunnen de functionaliteit van eenmalige aanmelding gebruiken.

SAP Cloud Platform identiteitsverificatie ondersteunt de optie identiteitsfederatie. Deze optie kunt de toepassing om te controleren als de gebruikers die zijn geverifieerd door de identiteitsprovider zakelijke aanwezig zijn in het archief van SAP Cloud Platform identiteit gebruikersverificatie. 

In de standaardinstelling is de optie identiteitsfederatie is uitgeschakeld. Als identiteitsfederatie is ingeschakeld, zijn alleen de gebruikers die worden geïmporteerd in SAP Cloud Platform identiteitsverificatie toegang tot de toepassing. 

Zie voor meer informatie over het in- of uitschakelen van identiteitsfederatie met SAP Cloud Platform identiteitsverificatie identiteitsfederatie inschakelen met SAP Cloud Platform identiteitsverificatie in [identiteitsfederatie configureren met de Archief van SAP Cloud Platform identiteit gebruikersverificatie](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan SAP Cloud Platform-ID-verificatie.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen identiteitsverificatie voor SAP Cloud-Platform, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SAP Cloud Platform identiteitsverificatie**.

    ![De koppeling SAP Cloud Platform identiteitsverificatie in de lijst met toepassingen](./media/active-directory-saas-sapcloudauth-tutorial/tutorial_sapcpia_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel SAP Cloud Platform identiteitsverificatie in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing identiteitsverificatie voor SAP Cloud-Platform.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapcloudauth-tutorial/tutorial_general_203.png

