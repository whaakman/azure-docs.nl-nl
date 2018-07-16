---
title: 'Zelfstudie: Azure Active Directory-integratie met TOPdesk - openbare | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TOPdesk - publiek.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 7f8bedda36e4008bdb9c3aeefc4d5acdf6ba253e
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39042126"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a>Zelfstudie: Azure Active Directory-integratie met TOPdesk - algemeen

In deze zelfstudie leert u hoe u integreert TOPdesk - openbare met Azure Active Directory (Azure AD).

Integratie van TOPdesk - openbare met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TOPdesk - publiek heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij TOPdesk - openbare (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met TOPdesk - openbaar is, moet u de volgende items:

- Een Azure AD-abonnement
- Een TOPdesk - openbare eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TOPdesk - openbare uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-topdesk---public-from-the-gallery"></a>TOPdesk - openbare uit de galerie toevoegen
Voor het configureren van de integratie van TOPdesk - openbare in Azure AD, moet u TOPdesk - publiek uit de galerie aan de lijst met beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen, TOPdesk - openbare uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **TOPdesk - openbare**, selecteer **TOPdesk - openbare** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![TOPdesk - publiek in de lijst met resultaten](./media/topdesk-public-tutorial/tutorial_topdesk-public_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie kunt u configureren en testen op Azure AD eenmalige aanmelding met TOPdesk - openbare op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding voor het werk, Azure AD moet weten wat de equivalente-gebruiker in TOPdesk - openbaar is aan een gebruiker in Azure AD. Met andere woorden, een koppeling de relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in TOPdesk - openbare moet tot stand worden gebracht.

In TOPdesk - openbaar, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TOPdesk - openbaar is, moet u de volgende bouwstenen uitvoeren:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een TOPdesk - openbare testgebruiker](#create-a-topdesk---public-test-user)**  : als u wilt een equivalent van Britta Simon in TOPdesk - openbare die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en eenmalige aanmelding in uw TOPdesk - openbare toepassing configureren.

**Het configureren van Azure AD eenmalige aanmelding met TOPdesk - openbaar, voer de volgende stappen uit:**

1. In de Azure-portal op de **TOPdesk - openbare** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/topdesk-public-tutorial/tutorial_topdesk-public_samlbase.png)

3. Op de **TOPdesk - openbaar domein en URL's** sectie, voert u de volgende stappen uit:

    ![TOPdesk - openbaar domein en URL's, eenmalige aanmelding informatie](./media/topdesk-public-tutorial/tutorial_topdesk-public_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.topdesk.net`
    
    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.topdesk.net/tas/public/login/verify`

    c. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.topdesk.net/tas/public/login/saml`
     
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Antwoord-URL is explaned verderop in de zelfstudie. Neem contact op met [TOPdesk - ondersteuningsteam voor openbare Client](https://help.topdesk.com/saas/enterprise/user/) om deze waarden te verkrijgen.  

4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/topdesk-public-tutorial/tutorial_topdesk-public_certificate.png) 

5. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/topdesk-public-tutorial/tutorial_general_400.png)
    
6. Op de **TOPdesk - configuratie voor een openbare** sectie, klikt u op **configureren TOPdesk - openbare** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![TOPdesk - openbare configuratie](./media/topdesk-public-tutorial/tutorial_topdesk-public_configure.png) 

7. Meld u aan bij uw **TOPdesk - openbare** bedrijf site als beheerder.

8. In de **TOPdesk** menu, klikt u op **instellingen**.
   
    ![Instellingen voor](./media/topdesk-public-tutorial/ic790598.png "instellingen")

9. Klik op **Login Settings**.
   
    ![Login Settings](./media/topdesk-public-tutorial/ic790599.png "Login Settings")

10. Vouw de **Login Settings** menu en klik vervolgens op **algemene**.
   
    ![Algemene](./media/topdesk-public-tutorial/ic790600.png "algemeen")

11. In de **openbare** sectie van de **SAML-aanmelding** configuratie sectie, voert u de volgende stappen uit:
   
    ![Technische instellingen](./media/topdesk-public-tutorial/ic790601.png "technische instellingen")
   
    a. Klik op **downloaden** -bestand met de openbare metagegevens downloaden en lokaal opslaan op uw computer.
   
    b. Open het gedownloade metagegevensbestand en zoek vervolgens de **AssertionConsumerService** knooppunt.

    ![AssertionConsumerService](./media/topdesk-public-tutorial/ic790619.png "AssertionConsumerService")
   
    c. Kopieer de **AssertionConsumerService** waarde, plak deze waarde in de **antwoord-URL** -tekstvak in **TOPdesk - openbaar domein en URL's** sectie.      
   
12. Als u wilt een certificaatbestand maakt, moet u de volgende stappen uitvoeren:
    
    ![Certificaat](./media/topdesk-public-tutorial/ic790606.png "certificaat")
    
    a. Open het gedownloade metagegevensbestand vanuit Azure portal.
    
    b. Vouw de **RoleDescriptor** knooppunt dat is een **xsi: type** van **ingevoerd: ApplicationServiceType**.
    
    c. Kopieer de waarde van de **X509Certificate** knooppunt.
    
    d. Sla de gekopieerde **X509Certificate** lokaal op uw computer in een bestand.

13. In de **openbare** sectie, klikt u op **toevoegen**.
    
    ![SAML-aanmelding](./media/topdesk-public-tutorial/ic790625.png "SAML-aanmelding")

14. Op de **SAML-configuratie-assistent** dialoogvenster pagina, voert u de volgende stappen uit:
    
    ![SAML-configuratie-assistent](./media/topdesk-public-tutorial/ic790608.png "SAML-configuratie-assistent")
    
    a. Het uploaden van uw gedownloade metagegevensbestand van Azure-portal onder **Federatiemetagegevens**, klikt u op **Bladeren**.

    b. Het uploaden van het certificaatbestand onder **certificaat (RSA)**, klikt u op **Bladeren**.

    c. Het uploaden van het logobestand dat u hebt verkregen via het ondersteuningsteam TOPdesk onder **Logo pictogram**, klikt u op **Bladeren**.

    d. In de **gebruikersnaamkenmerk** tekstvak, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. In de **weergavenaam** tekstvak, typ een naam voor uw configuratie.

    f. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/topdesk-public-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/topdesk-public-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/topdesk-public-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/topdesk-public-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-topdesk---public-test-user"></a>Een TOPdesk - openbare testgebruiker maken

Om in te schakelen van Azure AD-gebruikers zich aanmelden bij TOPdesk - openbaar, ze moeten worden ingericht voor TOPdesk - publiek.  
In het geval van TOPdesk - openbaar, inrichting is een handmatige taak.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt inrichten van gebruikers configureren, moet u de volgende stappen uitvoeren:
1. Meld u aan bij uw **TOPdesk - openbare** bedrijf site als administrator.

2. Klik in het menu aan de bovenkant op **TOPdesk \> nieuw \> ondersteuningsbestanden \> persoon**.
   
    ![Persoon](./media/topdesk-public-tutorial/ic790628.png "persoon")

3. In het dialoogvenster nieuwe persoon, moet u de volgende stappen uitvoeren:
   
    ![Nieuwe persoon](./media/topdesk-public-tutorial/ic790629.png "nieuwe persoon")
   
    a. Klik op het tabblad Algemeen.

    b. In de **achternaam** tekstvak typt u de achternaam van de gebruiker, zoals steen
 
    c. Selecteer een **Site** voor het account.
 
    d. Klik op **Opslaan**.

> [!NOTE]
> U kunt elke andere TOPdesk - hulpmiddelen voor het openbare gebruiker-account maken of API's wordt geleverd door TOPdesk - openbare voor het inrichten van gebruikersaccounts van de Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan TOPdesk - openbare.

![De de gebruikersrol toewijzen][200] 

**Britta Simon toewijzen aan TOPdesk - openbaar, voer de volgende stappen uit:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **TOPdesk - openbare**.

    ![De TOPdesk - openbare koppeling in de lijst met toepassingen](./media/topdesk-public-tutorial/tutorial_topdesk-public_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de TOPdesk - openbare tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw TOPdesk - openbare toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/topdesk-public-tutorial/tutorial_general_01.png
[2]: ./media/topdesk-public-tutorial/tutorial_general_02.png
[3]: ./media/topdesk-public-tutorial/tutorial_general_03.png
[4]: ./media/topdesk-public-tutorial/tutorial_general_04.png

[100]: ./media/topdesk-public-tutorial/tutorial_general_100.png

[200]: ./media/topdesk-public-tutorial/tutorial_general_200.png
[201]: ./media/topdesk-public-tutorial/tutorial_general_201.png
[202]: ./media/topdesk-public-tutorial/tutorial_general_202.png
[203]: ./media/topdesk-public-tutorial/tutorial_general_203.png

