---
title: "Zelfstudie: Azure Active Directory-integratie met IBM Kenexa enquête Enterprise | Microsoft Docs"
description: "Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en IBM Kenexa enquête Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 5c276c23288292a1c54dd9d57177d5072b90c9e3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met IBM Kenexa enquête Enterprise

In deze zelfstudie leert u hoe IBM Kenexa enquête Enterprise integreren met Azure Active Directory (Azure AD).

IBM Kenexa enquête Enterprise integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot IBM Kenexa enquête Enterprise heeft.
- U kunt uw gebruikers automatisch aanmelden bij IBM Kenexa enquête Enterprise met behulp van eenmalige aanmelding (SSO) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één locatie beheren: de Azure-portal.

Als u meer weten over software als een dienst (SaaS)-app-integratie met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met IBM Kenexa enquête voor ondernemingen, moet u de volgende items:

- Een Azure AD-abonnement
- Een abonnement IBM Kenexa enquête Enterprise SSO-ingeschakeld

> [!NOTE]
> Wanneer u de stappen in deze zelfstudie test, wordt u aangeraden een productie-omgeving niet te gebruiken.

Test de stappen in deze zelfstudie, volgt u deze aanbevelingen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie kunt u Azure AD SSO testen in een testomgeving. Het scenario in de zelfstudie bestaat uit twee belangrijkste bouwstenen:

* IBM Kenexa enquête Enterprise uit de galerie toevoegen
* Configureren en testen van Azure AD-SSO

## <a name="add-ibm-kenexa-survey-enterprise-from-the-gallery"></a>IBM Kenexa enquête Enterprise uit de galerie toevoegen
Voor het configureren van de integratie van IBM Kenexa enquête Enterprise in Azure AD IBM Kenexa enquête Enterprise uit de galerie te toevoegt aan de lijst met beheerde SaaS-apps.

Als u wilt toevoegen IBM Kenexa enquête Enterprise uit de galerie, het volgende doen:

1. In de [Azure-portal](https://portal.azure.com), in het linkerdeelvenster klikt u op de **Azure Active Directory** knop. 

    ![De Azure Active Directory-knop][1]

2. Selecteer **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Als u wilt een toepassing toevoegen, klikt u op de **nieuwe toepassing** knop.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **IBM Kenexa enquête Enterprise**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_search.png)

5. Selecteer in de lijst met resultaten **IBM Kenexa enquête Enterprise**, en klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![IBM Kenexa enquête onderneming in de lijst met resultaten](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD
In deze sectie kunt u configureren en testen van Azure AD-SSO met IBM Kenexa enquête Enterprise op basis van een testgebruiker genaamd "Britta Simon."

Azure AD moet de gebruiker IBM Kenexa enquête Enterprise equivalent identificeren in Azure AD voor eenmalige aanmelding werkt. Azure AD moet met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en een bijbehorende gebruiker maken in IBM Kenexa enquête onderneming.

Om vast te stellen op de koppeling relatie, wijs de waarde van de **gebruikersnaam** in IBM Kenexa enquête Enterprise als de waarde van de **gebruikersnaam** in Azure AD.

Als u wilt configureren en testen van Azure AD-SSO met IBM Kenexa enquête Enterprise, voltooi de bouwstenen in de volgende twee secties.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

In dit gedeelte Azure AD-eenmalige aanmelding inschakelen in de Azure-portal en eenmalige aanmelding in uw toepassing IBM Kenexa enquête Enterprise als volgt configureren:

1. In de Azure-portal op de **IBM Kenexa enquête Enterprise** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![IBM Kenexa enquête Enterprise configureren één aanmelding koppeling][4]

2. In de **eenmalige aanmelding** het dialoogvenster de **modus** de optie **op basis van SAML aanmelding** eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_samlbase.png)

3. In de **IBM Kenexa enquête Enterprise domein en de URL's** sectie, voert u de volgende stappen uit:

    ![IBM Kenexa enquête Enterprise domein en de URL's van eenmalige aanmelding informatie](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_url.png)

    a. In de **id** textbox type een URL met het volgende patroon volgen:`https://surveys.kenexa.com/<companycode>`

    b. In de **antwoord-URL** textbox type een URL met het volgende patroon volgen:`https://surveys.kenexa.com/<companycode>/tools/sso.asp`

    > [!NOTE] 
    > De voorgaande waarden zijn niet echt. Deze bijwerken met de werkelijke-id en antwoord-URL. De werkelijke waarden, neem contact op met de [IBM Kenexa enquête Enterprise ondersteuningsteam](https://www.ibm.com/support/home/?lnk=fcw).

4. Onder **SAML-certificaat voor ondertekening van**, klikt u op **certificaat (Base64)**, en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat (Base64)](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_certificate.png) 

    De bedrijfstoepassing IBM Kenexa enquête verwacht te ontvangen van de asserties Security Asserties Markup Language (SAML) in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de kenmerken van SAML-token. De waarde van de claim gebruiker-id in het antwoord moet overeenkomen met de ID voor eenmalige aanmelding die geconfigureerd in het systeem Kenexa. Als u wilt toewijzen in de juiste gebruikers-id in uw organisatie als SSO Internet Datagram Protocol (IDP), werkt u met de [IBM Kenexa enquête Enterprise ondersteuningsteam](https://www.ibm.com/support/home/?lnk=fcw). 

    Azure AD wordt standaard ingesteld van de gebruikers-id als de waarde van de principal-naam (User Principal Name) gebruiker. U kunt deze waarde wijzigen op de **kenmerk** tabblad, zoals wordt weergegeven in de volgende schermafbeelding. De integratie werkt alleen na voltooiing van de toewijzing correct.
    
    ![Het dialoogvenster gebruikerskenmerken](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_attribute.png)   

5. Klik op **Opslaan**.

    ![De configureren eenmalige aanmelding knop Opslaan](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_400.png)

6. Openen van de **eenmalige aanmelding configureren** venster onder **IBM Kenexa enquête Enterprise Configuration**, klikt u op **configureren IBM Kenexa enquête Enterprise**. 
 
    ![De koppeling IBM Kenexa enquête Enterprise configureren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_configure.png)

7. Kopieer de **Sign-Out URL**, **SAML entiteit-ID**, en **SAML aanmelding Service-URL met eenmalige** waarden van de **Naslaggids** sectie.

8. In de **eenmalige aanmelding configureren** venster onder **Naslaggids**, Kopieer de **Sign-Out URL**, **SAML entiteit-ID**, en **SAML eenmalige aanmelding Service-URL** waarden.

9. Eenmalige aanmelding configureren op de **IBM Kenexa enquête Enterprise** zijde, verzenden de gedownloade **certificaat (Base64)**, **Sign-Out URL**, **SAML entiteit-ID**, en **SAML één Service-URL aanmelding** waarden voor de [IBM Kenexa enquête Enterprise ondersteuningsteam](https://www.ibm.com/support/home/?lnk=fcw).

> [!TIP]
> U kunt verwijzen naar een beknopte versie van deze instructies in de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app. Na het toevoegen van de app vanuit de **Active Directory** > **bedrijfstoepassingen** sectie, klikt u op de **eenmalige aanmelding** tabblad en vervolgens naar de Embedded-documentatie via de **configuratie** sectie aan het einde. Zie voor meer informatie over de functie embedded-documentatie, [documentatie van Azure AD ingesloten](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
In deze sectie kunt u testgebruiker Britta Simon in de Azure portal maken door het volgende te doen:

![Een Azure AD-testgebruiker maken][100]

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.
 
    ![De knop toevoegen](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:
 
    ![Het dialoogvenster gebruiker](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-ibm-kenexa-survey-enterprise-test-user"></a>Een IBM Kenexa enquête Enterprise testgebruiker maken

In deze sectie kunt u een gebruiker met de naam van Britta Simon in IBM Kenexa enquête onderneming maken. 

Als u wilt maken van gebruikers in het systeem IBM Kenexa enquête Enterprise en de SSO-ID voor deze toewijst, kunt u samenwerken met de [IBM Kenexa enquête Enterprise ondersteuningsteam](https://www.ibm.com/support/home/?lnk=fcw). Deze SSO-ID-waarde moet ook worden toegewezen aan de waarde van de gebruiker-id van Azure AD. U kunt deze standaardinstelling wijzigen op de **kenmerk** tabblad.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u de gebruiker Britta Simon Azure SSO toegang verleent tot IBM Kenexa enquête onderneming gebruiken.

![Toewijzen van de gebruikersrol][200] 

Gebruiker Britta Simon om aan te wijzen IBM Kenexa enquête Enterprise, het volgende doen:

1. Open in de Azure-portal, de **toepassingen** weergeven, gaat u naar de **Directory** weergave, selecteer **bedrijfstoepassingen**, en klik vervolgens op **alle toepassingen** .

    ![De 'Bedrijfstoepassingen' en 'Alle toepassingen' koppelingen][201] 

2. In de **toepassingen** selecteert **IBM Kenexa enquête Enterprise**.

    ![De IBM Kenexa enquête Enterprise-koppeling in de lijst met toepassingen](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_app.png) 

3. Klik in het linkerdeelvenster op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202] 

4. Klik op de **toevoegen** knop en klikt u op de **toevoegen toewijzing** deelvenster **gebruikers en groepen**.

    ![Het deelvenster toewijzing toevoegen][203]

5. In de **gebruikers en groepen** het dialoogvenster de **gebruikers** selecteert **Britta Simon**.

6. In de **gebruikers en groepen** in het dialoogvenster, klikt u op de **Selecteer** knop.

7. In de **toevoegen toewijzing** in het dialoogvenster, klikt u op de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u de configuratie van uw Azure AD SSO testen met behulp van het toegangsvenster.

Wanneer u klikt op de **IBM Kenexa enquête Enterprise** tegel in het deelvenster toegang u moet worden automatisch aangemeld bij uw toepassing IBM Kenexa enquête Enterprise.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png

 
