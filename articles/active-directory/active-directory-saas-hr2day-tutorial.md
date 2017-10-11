---
title: 'Zelfstudie: Azure Active Directory-integratie met HR2day door Merces | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en HR2day door Merces.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 853d08c9-27b1-48d4-b8e7-3705140eb67f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: 77e2fcf9c306259286b15767f3a992510d6d79d8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a>Zelfstudie: Azure Active Directory-integratie met HR2day door Merces

In deze zelfstudie leert u hoe HR2day door Merces integreren met Azure Active Directory (Azure AD).

HR2day door Merces integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot HR2day door Merces heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij HR2day door Merces met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie--de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met HR2day door Merces, moet u de volgende items:

- Een Azure AD-abonnement.
- Een HR2day door eenmalige aanmelding Merces abonnement ingeschakeld.

> [!NOTE]
> Gebruik een productie-omgeving voor het testen van de stappen in deze zelfstudie aanbevolen niet.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik uw productieomgeving geen tenzij dit noodzakelijk is.
- Ophalen van een [één maand gratis proefversie van Azure AD](https://azure.microsoft.com/pricing/free-trial/) als u nog geen hebt.  

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat hier wordt beschreven, bestaat uit twee belangrijkste bouwstenen:

1. HR2day door Merces uit de galerie toevoegen.
2. Configureren en testen van Azure AD eenmalige aanmelding.

## <a name="add-hr2day-by-merces-from-the-gallery"></a>HR2day door Merces uit de galerie toevoegen
Voor het configureren van de integratie van HR2day door Merces in Azure AD HR2day door Merces uit de galerie te toevoegt aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen HR2day door Merces uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), Selecteer op het navigatiedeelvenster links de **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **HR2day door Merces**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_search.png)

5. Selecteer in het deelvenster resultaten **HR2day door Merces**, en selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met HR2day door Merces op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD te weten wie de gebruiker equivalent in HR2day door Merces is een gebruiker in Azure AD. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en de betreffende gebruiker in HR2day door Merces vast te stellen.

Wijs in HR2day door Merces de **gebruikersnaam** in Azure AD **gebruikersnaam** de relatie tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met HR2day door Merces, moet u de volgende bouwstenen voltooien:

1. [Eenmalige aanmelding Azure AD configureren](#configuring-azure-ad-single-sign-on): uw gebruikers om deze functie te gebruiken.
2. [Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user): Azure AD Test eenmalige aanmelding met Britta Simon.
3. [Maken van een HR2day door Merces testgebruiker](#creating-an-hr2day-by-merces-test-user): maken van een exemplaar van Britta Simon in HR2day door Merces die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. [Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user): inschakelen Britta Simon gebruik van eenmalige aanmelding Azure AD.
5. [Test eenmalige aanmelding](#testing-single-sign-on): controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw HR2day door Merces toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met HR2day door Merces, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **HR2day door Merces** toepassing Integratiepagina **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Eenmalige aanmelding inschakelen in de **eenmalige aanmelding** dialoogvenster, **modus** als **op basis van SAML aanmelding**.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_samlbase.png)

3. In de **HR2day Merces domein en URL's** sectie, voert de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_url.png)

    a. In de **aanmeldings-URL** vak een URL met behulp van het volgende patroon volgen: `https://<tenantname>.force.com/<instancename>`.

    b. In de **id** vak een URL met behulp van het volgende patroon volgen: `https://hr2day.force.com/<companyname>`.

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke aanmeldings-URL en de id. Neem contact op met de [HR2day door Merces client ondersteuningsteam](mailto:servicedesk@merces.nl) ophalen van deze waarden. 
 


4. Op de **SAML-certificaat voor ondertekening van** sectie **Certificate(Base64)**, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_certificate.png) 

5. Deze sectie wordt beschreven hoe gebruikers worden geverifieerd bij HR2day door Merces aan hun account in Azure AD inschakelen. Ze doen dit met behulp van de federatieserver die gebaseerd op het SAML-protocol.

    De SAML-asserties verwacht uw HR2day door Merces toepassing in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw SAML-token. De volgende Schermafbeelding toont een voorbeeld hiervan. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png)
    
    > [!NOTE] 
    Voordat u de SAML-bevestiging configureren kunt, moet u contact opneemt de [HR2day door Merces Client ondersteuningsteam](mailto:servicedesk@merces.nl) en vraagt u de waarde van het kenmerk unieke id voor uw tenant. U moet deze waarde de stappen in de volgende sectie uitvoeren. 

6. In de **eenmalige aanmelding** het dialoogvenster de **gebruikerskenmerken** sectie, het configureren van het kenmerk van SAML-token, zoals wordt weergegeven in de volgende afbeelding. Vervolgens voert u de volgende stappen uit.
    
      | Naam van kenmerk    |   De waarde van kenmerk |  
    | ------------------- | -------------------- |    
    | ATTR_LOGINCLAIM | join ([e] '102938475Z' ' @ ' |
    
      a. Openen van de **kenmerk toevoegen** dialoogvenster, selecteer **toevoegen kenmerk**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_05.png)

    b. In de **naam** in het vak **ATTR_LOGINCLAIM**.

    c. Van de **waarde** selecteert **Join()**.

    d. Van de **tekenreeks1** selecteert **user.mail**.

    e. Voor **tekenreeks2**, typt u de unieke id die wordt geleverd door uw team HR2day.

    f. In de **scheidingsteken** in het vak  **@** .
    
    g. Selecteer **Ok**.

7. Selecteer de knop **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_general_400.png)

8. In de **HR2day door Merces configuratie** sectie **HR2day configureren door Merces** openen de **eenmalige aanmelding configureren** venster. Kopiëren de **Sign-Out URL**, **SAML entiteit-ID**, en **SAML Single Sign-On Service-URL** van de **Naslaggids** sectie.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_configure.png) 

9. Voor meer informatie over het configureren van eenmalige aanmelding voor uw toepassing Neem contact op met de [HR2day door Merces client ondersteuningsteam](mailTo:servicedesk@merces.nl). Koppel de gedownloade **Certificate(Base64)** bestand naar uw e-mailadres. Bieden ook de **Sign-Out URL**, **SAML entiteit-ID**, en **SAML Single Sign-On Service-URL** zodat ze kunnen worden geconfigureerd voor eenmalige aanmelding-integratie.

    > [!NOTE]
    >Aan het team Merces vermeld dat deze integratie de entiteit-ID moet moet worden ingesteld met het patroon **https://hr2day.force.com/INSTANCENAME**.

    > [!TIP]
    >U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad. Vervolgens toegang krijgen tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de functie embedded-documentatie in de [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, Selecteer op het navigatiedeelvenster links de **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hr2day-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hr2day-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. In de **gebruiker** dialoogvenster vak, voert de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** in het vak de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven**, en noteer het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-an-hr2day-by-merces-test-user"></a>Een HR2day door Merces testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon in HR2day door Merces aangeroepen. Als u wilt toevoegen de gebruikers in het account HR2day, werken met de [HR2day door Merces client ondersteuningsteam](mailto:servicedesk@merces.nl). 

> [!NOTE]
> Als u maken van een gebruiker handmatig wilt, neem dan contact op met de [HR2day door Merces client ondersteuningsteam](mailto:servicedesk@merces.nl).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door haar toegang verlenen aan HR2day door Merces.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen HR2day door Merces, moet u de volgende stappen uitvoeren:**

1. Open de weergave van toepassingen in de Azure-portal, Ga naar de directoryweergave en gaat u naar **bedrijfstoepassingen**. Selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **HR2day door Merces**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_app.png) 

3. Selecteer in het menu aan de linkerkant **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Selecteer de **toevoegen** knop. Klik in de **toevoegen toewijzing** dialoogvenster, **gebruikers en groepen**.

    ![Gebruiker toewijzen][203]

5. In de **gebruikers en groepen** het dialoogvenster de **gebruikers** selecteert **Britta Simon**.

6. Klik op de **Selecteer** knop.

7. In de **toevoegen toewijzing** dialoogvenster, **toewijzen**.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

Het doel van deze sectie is het testen van uw configuratie Azure AD eenmalige aanmelding met behulp van het toegangsvenster.  

Wanneer u de HR2day door Merces-tegel in het toegangsvenster selecteert, ophalen u automatisch aangemeld bij uw HR2day door Merces toepassing.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png

