---
title: 'Zelfstudie: Azure Active Directory-integratie met SAP Business Object Cloud | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en SAP Business Object Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: b0ea48aea2e13027c5af273a416489e48987b44a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a>Zelfstudie: Azure Active Directory-integratie met SAP Business Object Cloud

In deze zelfstudie leert u hoe SAP Business Object Cloud integreren met Azure Active Directory (Azure AD).

Wanneer u een SAP Business Object Cloud met Azure AD integreert profiteren van de volgende voordelen:

- In Azure AD, kunt u bepalen wie toegang heeft tot een SAP Business Object Cloud.
- U kunt uw gebruikers SAP Business Object Cloud automatisch aanmelden met behulp van eenmalige aanmelding en Azure AD-account van een gebruiker.
- U kunt uw accounts op één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over software als een dienst (SaaS)-app-integratie met Azure AD, [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Als u Azure AD-integratie met SAP Business Object Cloud instelt, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAP Business Object Cloud met eenmalige aanmelding ingeschakeld

> [!NOTE]
> Als u de stappen in deze zelfstudie hebt getest, wordt u aangeraden dat u deze niet in een productieomgeving testen.

Aanbevelingen voor het testen van de stappen in deze zelfstudie:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. 

Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP Business Object Cloud uit de galerie toevoegen.
2. Instellen en testen van eenmalige aanmelding Azure AD.

## <a name="add-sap-business-object-cloud-from-the-gallery"></a>SAP Business Object Cloud uit de galerie toevoegen
Voor het instellen van de integratie van SAP Business Object Cloud met Azure AD in de galerie toevoegen SAP Business Object Cloud aan de lijst met beheerde SaaS-apps.

SAP Business Object Cloud uit de galerie toevoegen:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het menu links **Azure Active Directory**. 

    ![De Azure Active Directory-knop][1]

2. Selecteer **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De pagina met Enterprise-toepassingen][2]
    
3. Selecteer om een nieuwe toepassing toe **nieuwe toepassing**.

    ![De knop Nieuw toepassing][3]

4. Voer in het zoekvak **SAP Business Object Cloud**.

    ![Het zoekvak](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_search.png)

5. Selecteer in het deelvenster resultaten **SAP Business Object Cloud**, en selecteer vervolgens **toevoegen**.

    ![SAP Business Object Cloud in de lijst met resultaten](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_addfromgallery.png)

##  <a name="set-up-and-test-azure-ad-single-sign-on"></a>Instellen en testen eenmalige aanmelding Azure AD

In deze sectie kunt u instellen en eenmalige aanmelding Azure AD-test met SAP Business Object Cloud op basis van een testgebruiker met de naam *Britta Simon*.

Voor eenmalige aanmelding werkt, moet Azure AD de equivalente Azure AD-gebruiker in een SAP Business Object Cloud bekend. Een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in een SAP Business Object Cloud moet worden ingesteld.

Tot stand brengen van de relatie koppeling in een SAP Business Object Cloud voor **gebruikersnaam**, wijs de waarde van de **gebruikersnaam** in Azure AD.

Als u wilt configureren en testen Azure AD eenmalige aanmelding met SAP Business Object Cloud, moet u de volgende taken uitvoeren:

1. [Instellen van eenmalige aanmelding Azure AD](#set-up-azure-ad-single-sign-on). Hiermee stelt u een gebruiker deze functie wilt gebruiken.
2. [Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user). Azure AD tests eenmalige aanmelding met de gebruiker Britta Simon.
3. [Maken van een SAP Business Object Cloud testgebruiker](#create-an-sap-business-object-cloud-test-user). Hiermee maakt een exemplaar van Britta Simon in SAP Business Object Cloud die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user). Ingesteld Britta Simon naar Azure AD gebruikt eenmalige aanmelding.
5. [Test eenmalige aanmelding](#test-single-sign-on). Verifieert dat de configuratie werkt.

### <a name="set-up-azure-ad-single-sign-on"></a>Instellen van eenmalige aanmelding Azure AD

In deze sectie maakt inschakelen u Azure AD één aanmelding in de Azure portal. Vervolgens instellen u eenmalige aanmelding in uw toepassing SAP Business Object Cloud.

Voor het instellen van Azure AD eenmalige aanmelding met SAP Business Object Cloud:

1. In de Azure-portal op de **SAP Business Object Cloud** toepassing Integratiepagina **eenmalige aanmelding**.

    ![Schakel eenmalige aanmelding][4]

2. Op de **eenmalige aanmelding** pagina voor **modus**, selecteer **op basis van SAML aanmelding**.
 
    ![Selecteer op basis van SAML eenmalige aanmelding](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_samlbase.png)

3. Onder **SAP Business Object Cloud-domein en URL's**, de volgende stappen uit:

    1. In de **aanmeldings-URL** Voer een URL die het volgende patroon volgen is: 
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    2. In de **id** Voer een URL die het volgende patroon volgen is:
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    ![URL's en SAP Business Object Cloud-domein-URL 's](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_url.png)
 
    > [!NOTE] 
    > De waarden in deze URL's zijn voor de demonstratie alleen. Werk de waarden op met de werkelijke aanmeldings-URL en identificatie-URL. Als u de URL voor eenmalige aanmelding, neem contact op met de [SAP Business Object Cloud Client ondersteuningsteam](https://www.sap.com/product/analytics/cloud-analytics.support.html). U kunt de identificatie-URL ophalen door het downloaden van de metagegevens van de SAP Business Object Cloud van de beheerconsole. Dit wordt verderop in de zelfstudie uitgelegd. 

4. Onder **SAML-certificaat voor ondertekening van**, selecteer **Metadata XML**. Sla het bestand met metagegevens op uw computer.

    ![Selecteer de metagegevens-XML](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_certificate.png) 

5. Selecteer **Opslaan**.

    ![Opslaan selecteren](./media/active-directory-saas-sapboc-tutorial/tutorial_general_400.png)

6. In een ander browservenster aanmelden bij uw bedrijf SAP Business Object Cloud site als beheerder.

7. Selecteer **Menu** > **System** > **beheer**.
    
    ![Selecteer vervolgens Menu, systeem en beheer](./media/active-directory-saas-sapboc-tutorial/config1.png)

8. Op de **beveiliging** tabblad de **bewerken** pictogram (pen).
    
    ![Selecteer het pictogram bewerken op het tabblad Beveiliging](./media/active-directory-saas-sapboc-tutorial/config2.png)  

9. Voor **verificatiemethode**, selecteer **SAML eenmalige aanmelding (SSO)**.

    ![SAML eenmalige aanmelding voor de verificatiemethode selecteren](./media/active-directory-saas-sapboc-tutorial/config3.png)  

10. Voor het downloaden van de metagegevens van de provider (stap 1) Selecteer **downloaden**. Zoek in het bestand met metagegevens en kopieer de **id van de entiteit** waarde. In de Azure-portal onder **SAP Business Object Cloud-domein en URL's**, plakt u de waarde in de **id** vak.

    ![Kopieer en plak de waarde van de id van de entiteit](./media/active-directory-saas-sapboc-tutorial/config4.png)  

11. Voor het uploaden van de metagegevens van de provider (stap 2) in het bestand dat u hebt gedownload van de Azure-portal onder **identiteitsprovider uploaden metagegevens**, selecteer **uploaden**.  

    ![Selecteer onder de metagegevens van de identiteitsprovider uploaden, uploaden](./media/active-directory-saas-sapboc-tutorial/config5.png)

12. In de **gebruikerskenmerk** , selecteert u het gebruikerskenmerk (stap 3) die u wilt gebruiken voor uw implementatie. Dit gebruikerskenmerk is toegewezen aan de id-provider. U kunt een aangepast kenmerk op de pagina van de gebruiker met behulp de **aangepaste SAML-toewijzing** optie. Of u kunt een selecteren **e** of **gebruikers-ID** als het gebruikerskenmerk. In ons voorbeeld we geselecteerd **e** omdat we de gebruikers-id claim met een toegewezen de **userprincipalname** kenmerk in de **gebruikerskenmerken** sectie in Azure Portal. Dit biedt een unieke gebruikers e-mailadres waarmee de toepassing SAP Business Object Cloud bij elke geslaagde SAML-reactie wordt verzonden.

    ![Selecteer de gebruiker-kenmerk](./media/active-directory-saas-sapboc-tutorial/config6.png)

13. Het account verifiëren met de id-provider (stap 4), in de **aanmelding referentie (E-mail)** Voer het e-mailadres van de gebruiker. Selecteer **Account controleren**. Het systeem wordt aanmeldingsreferenties toegevoegd aan het gebruikersaccount.

    ![Voer e-mailadres en selecteer Account controleren](./media/active-directory-saas-sapboc-tutorial/config7.png)

14. Selecteer de **opslaan** pictogram.

    ![Pictogram opslaan](./media/active-directory-saas-sapboc-tutorial/save.png)

> [!TIP]
> Vindt u een beknopte versie van deze instructies in de [Azure-portal](https://portal.azure.com), terwijl u uw app instelt. Nadat u de app door te selecteren toevoegen **Active Directory** > **bedrijfstoepassingen**, selecteer de **Single Sign-On** tabblad. U hebt toegang tot het embedded-documentatie in de **configuratie** sectie aan de onderkant van de pagina. Zie voor meer informatie [documentatie van Azure AD ingesloten]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
In deze sectie maakt u een testgebruiker Britta Simon met de naam in de Azure portal.

Een testgebruiker maken in Azure AD:

1. Selecteer in de Azure-portal in het menu links **Azure Active Directory**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sapboc-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, selecteer **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sapboc-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, **toevoegen**.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-sapboc-tutorial/create_aaduser_03.png) 

4. In de **gebruiker** dialoogvenster Vervolledig de volgende stappen uit:
 
    1. In de **naam** Voer **BrittaSimon**.

    2. In de **gebruikersnaam** Voer het e-mailadres van de gebruiker Britta Simon.

    3. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    4. Selecteer **Maken**.

        ![Het dialoogvenster gebruiker](./media/active-directory-saas-sapboc-tutorial/create_aaduser_04.png) 

    ![Azure AD-gebruiker maken][100]

### <a name="create-an-sap-business-object-cloud-test-user"></a>Een SAP Business Object Cloud testgebruiker maken

Azure AD-gebruikers moeten worden ingericht in SAP Business Object Cloud voordat deze zich voor SAP Business Object Cloud aanmelden kunnen. In een SAP Business Object Cloud is inrichting een handmatige taak.

Voor het inrichten van een gebruikersaccount:

1. Meld u aan bij uw bedrijf SAP Business Object Cloud site als beheerder.

2. Selecteer **Menu** > **beveiliging** > **gebruikers**.

    ![Werknemer toevoegen](./media/active-directory-saas-sapboc-tutorial/user1.png)

3. Op de **gebruikers** pagina details van de nieuwe gebruiker toevoegen selecteren  **+** . 

    ![Gebruikers toevoegen](./media/active-directory-saas-sapboc-tutorial/user4.png)

    Voltooi de volgende stappen uit:

    1. In de **gebruikers-ID** Geef de gebruikers-ID van de gebruiker, zoals **Britta**.

    2. In de **VOORNAAM** Geef de voornaam van de gebruiker, zoals **Britta**.

    3. In de **ACHTERNAAM** Geef de achternaam van de gebruiker, zoals **Simon**.

    4. In de **WEERGAVENAAM** en voer de volledige naam van de gebruiker, zoals **Britta Simon**.

    5. In de **e** Voer het e-mailadres van de gebruiker, zoals  **brittasimon@contoso.com** .

    6. Op de **rollen selecteren** pagina, selecteer de juiste rol voor de gebruiker en selecteer vervolgens **OK**.

      ![Rol selecteren](./media/active-directory-saas-sapboc-tutorial/user3.png)

    7. Selecteer de **opslaan** pictogram.    


### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie kunt u de gebruiker Britta Simon Azure AD eenmalige aanmelding gebruiken door de gebruikerstoegang verlenen aan SAP Business Object Cloud toestaan.

Britta Simon toewijzen aan SAP Business Object Cloud:

1. Open de toepassingen in de Azure portal en gaat u naar de directoryweergave. Selecteer **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SAP Business Object Cloud**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_app.png) 

3. Selecteer in het menu links **gebruikers en groepen**.

    ![Gebruikers en groepen selecteren][202] 

4. Selecteer **Toevoegen**. Klik op de **toevoegen toewijzing** pagina **gebruikers en groepen**.

    ![De pagina toewijzing toevoegen][203]

5. Op de **gebruikers en groepen** pagina in de lijst met gebruikers, selecteer **Britta Simon**.

6. Op de **gebruikers en groepen** pagina **Selecteer**.

7. Op de **toevoegen toewijzing** pagina **toewijzen**.

![Toewijzen van de gebruikersrol][200] 
    
### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het toegangsvenster.

Wanneer u de tegel SAP Business Object Cloud in het deelvenster toegang selecteert, moet u worden automatisch aangemeld bij uw toepassing SAP Business Object Cloud.

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_203.png

