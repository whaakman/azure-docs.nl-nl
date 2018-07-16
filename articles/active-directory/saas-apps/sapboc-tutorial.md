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
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: ffd4480a13549caba17becff27a43f51fcaa1988
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041735"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a>Zelfstudie: Azure Active Directory-integratie met SAP Business Object Cloud

In deze zelfstudie leert u hoe u SAP Business Object Cloud integreren met Azure Active Directory (Azure AD).

U ontvangt de volgende voordelen wanneer u SAP Business Object Cloud met Azure AD integreren:

- In Azure AD, kunt u bepalen wie toegang heeft tot SAP Business Object Cloud.
- U kunt uw gebruikers naar SAP Business Object Cloud automatisch aanmelden met behulp van eenmalige aanmelding en een gebruikersaccount met Azure AD.
- U kunt uw accounts in één, centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over software als een service (SaaS)-app-integratie met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Als u Azure AD-integratie met SAP Business Object Cloud instelt, moet u de volgende items:

- Een Azure AD-abonnement
- Een Cloud SAP Business-Object, met eenmalige aanmelding ingeschakeld

> [!NOTE]
> Als u de stappen in deze zelfstudie hebt getest, wordt u aangeraden dat u ze niet in een productieomgeving testen.

Aanbevelingen voor het testen van de stappen in deze zelfstudie:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. 

Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP Business Object Cloud uit de galerie toevoegen.
2. Instellen en Azure AD eenmalige aanmelding testen.

## <a name="add-sap-business-object-cloud-from-the-gallery"></a>SAP Business Object Cloud uit de galerie toevoegen
Toevoegen als u wilt de integratie van SAP Business Object Cloud met Azure AD instellen in de galerie, SAP Business Object Cloud aan de lijst met beheerde SaaS-apps.

SAP Business Object Cloud uit de galerie toevoegen:

1. In de [Azure-portal](https://portal.azure.com), selecteert u in het menu links **Azure Active Directory**. 

    ![De Azure Active Directory-knop][1]

2. Selecteer **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De pagina van de Enterprise-toepassingen][2]
    
3. Als u wilt een nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.

    ![De knop nieuwe toepassing][3]

4. Voer in het zoekvak **SAP Business Object Cloud**.

    ![Het zoekvak](./media/sapboc-tutorial/tutorial_sapboc_search.png)

5. Selecteer in het deelvenster resultaten **SAP Business Object Cloud**, en selecteer vervolgens **toevoegen**.

    ![SAP Business Object Cloud in de lijst met resultaten](./media/sapboc-tutorial/tutorial_sapboc_addfromgallery.png)

##  <a name="set-up-and-test-azure-ad-single-sign-on"></a>Instellen en Azure AD eenmalige aanmelding testen

In deze sectie maakt u een instellen en test Azure AD eenmalige aanmelding met SAP Business Object Cloud op basis van een testgebruiker met de naam *Britta Simon*.

Voor eenmalige aanmelding om te werken, moet Azure AD de gebruiker voor het equivalent van Azure AD in SAP Business Object Cloud weten. Er moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SAP Business Object Cloud tot stand worden gebracht.

Tot stand brengen van de relatie koppeling in SAP Business Object Cloud voor **gebruikersnaam**, wijs de waarde van de **gebruikersnaam** in Azure AD.

Als u wilt configureren en testen van Azure AD eenmalige aanmelding met SAP Business Object Cloud, moet u de volgende taken uitvoeren:

1. [Instellen van Azure AD eenmalige aanmelding](#set-up-azure-ad-single-sign-on). Hiermee stelt u een gebruiker deze functie wilt gebruiken.
2. [Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user). Tests Azure AD eenmalige aanmelding met de gebruiker Britta Simon.
3. [Maak een testgebruiker SAP Business Object Cloud](#create-an-sap-business-object-cloud-test-user). Hiermee maakt u een equivalent van Britta Simon in SAP Business Object Cloud die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. [Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user). Ingesteld Britta Simon gebruik van Azure AD eenmalige aanmelding.
5. [Eenmalige aanmelding testen](#test-single-sign-on). Hiermee wordt gecontroleerd of de configuratie werkt.

### <a name="set-up-azure-ad-single-sign-on"></a>Instellen van Azure AD eenmalige aanmelding

In deze sectie maakt inschakelen u Azure AD-eenmalige aanmelding in de Azure-portal. Vervolgens kunt instellen u van eenmalige aanmelding in uw cloudtoepassing SAP Business-Object.

Een Azure AD eenmalige aanmelding met SAP Business Object Cloud instellen:

1. In de Azure-portal op de **SAP Business Object Cloud** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Schakel eenmalige aanmelding][4]

2. Op de **eenmalige aanmelding** pagina voor **modus**, selecteer **SAML gebaseerde aanmelding**.
 
    ![Selecteer SAML gebaseerde aanmelding](./media/sapboc-tutorial/tutorial_sapboc_samlbase.png)

3. Onder **SAP Business Object Cloud domein en URL's**, voer de volgende stappen uit:

    1. In de **aanmeldings-URL** voert u een URL met de volgende indeling: 
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    2. In de **id** voert u een URL met de volgende indeling:
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    ![SAP Business Object Cloud domein en URL's pagina-URL 's](./media/sapboc-tutorial/tutorial_sapboc_url.png)
 
    > [!NOTE] 
    > De waarden in deze URL's zijn alleen demonstratie. Werk de waarden bij met de werkelijke aanmeldings-URL en identificatie-URL. Als u de aanmeldings-URL, neem contact op met de [SAP Business Object Cloud Client ondersteuningsteam](https://help.sap.com/viewer/product/SAP_BusinessObjects_Cloud/release/en-US). U kunt de identificatie-URL ophalen door het downloaden van de metagegevens van de SAP Business Object Cloud van de beheerconsole. Dit wordt verderop in de zelfstudie. 

4. Onder **SAML-handtekeningcertificaat**, selecteer **Metadata XML**. Sla het bestand met metagegevens op uw computer.

    ![Selecteer de metagegevens-XML](./media/sapboc-tutorial/tutorial_sapboc_certificate.png) 

5. Selecteer **Opslaan**.

    ![Opslaan selecteren](./media/sapboc-tutorial/tutorial_general_400.png)

6. In een ander browservenster aanmelden bij uw bedrijf SAP Business Object Cloud site als beheerder.

7. Selecteer **Menu** > **System** > **beheer**.
    
    ![Selecteer in het Menu en vervolgens System en beheer](./media/sapboc-tutorial/config1.png)

8. Op de **Security** tabblad de **bewerken** (penpictogram).
    
    ![Selecteer het bewerkingspictogram op het tabblad Beveiliging](./media/sapboc-tutorial/config2.png)  

9. Voor **verificatiemethode**, selecteer **SAML eenmalige aanmelding (SSO)**.

    ![SAML Single Sign-On voor de verificatiemethode selecteren](./media/sapboc-tutorial/config3.png)  

10. Voor het downloaden van de metagegevens van de provider (stap 1), selecteer **downloaden**. Zoek in het bestand met metagegevens, en kopieer de **entityID** waarde. In de Azure-portal onder **SAP Business Object Cloud domein en URL's**, plak de waarde in de **id** vak.

    ![Kopieer en plak de waarde van de id van de entiteit](./media/sapboc-tutorial/config4.png)  

11. Het uploaden van de metagegevens van de provider (stap 2) in het bestand dat u hebt gedownload van de Azure-portal onder **metagegevens van de identiteitsprovider uploaden**, selecteer **uploaden**.  

    ![Selecteer onder de metagegevens van de identiteitsprovider uploaden, uploaden](./media/sapboc-tutorial/config5.png)

12. In de **gebruikerskenmerk** , selecteert u het gebruikerskenmerk (stap 3) die u wilt gebruiken voor uw implementatie. Dit gebruikerskenmerk wordt toegewezen aan de id-provider. Als u wilt een aangepast kenmerk op de pagina van de gebruiker invoert, gebruikt u de **aangepaste SAML-toewijzing** optie. Of u kunt een selecteren **e** of **gebruikers-ID** als het gebruikerskenmerk. In ons voorbeeld we geselecteerd **e** omdat we de gebruikers-id claim met een toegewezen de **userprincipalname** kenmerk in de **gebruikerskenmerken** sectie in Azure Portal. Dit biedt een uniek e-mailadres gebruiker, die wordt verzonden naar de cloudtoepassing SAP Business Object bij elke geslaagde SAML-reactie.

    ![Kenmerk van de gebruiker selecteren](./media/sapboc-tutorial/config6.png)

13. Om te controleren of het account met de id-provider (stap 4), in de **aanmelding referenties (e)** vak, voer het e-mailadres van de gebruiker. Selecteer **Account controleren**. Het systeem wordt aanmeldingsreferenties toegevoegd aan het gebruikersaccount.

    ![Voer e-mailadres en controleer of-Account selecteren](./media/sapboc-tutorial/config7.png)

14. Selecteer de **opslaan** pictogram.

    ![Pictogram opslaan](./media/sapboc-tutorial/save.png)

> [!TIP]
> U vindt een beknopte versie van deze instructies in de [Azure-portal](https://portal.azure.com), terwijl het instellen van uw app. Nadat u de app door te selecteren toevoegen **Active Directory** > **bedrijfstoepassingen**, selecteer de **Single Sign-On** tabblad. U hebt toegang tot de ingesloten documentatie in de **configuratie** sectie aan de onderkant van de pagina. Zie voor meer informatie, [documentatie over Azure AD embedded]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD
In deze sectie maakt u een testgebruiker Britta Simon met de naam in Azure portal.

Een testgebruiker maken in Azure AD:

1. Selecteer in de Azure portal, in het menu links **Azure Active Directory**.

    ![Het maken van een Azure AD-testgebruiker](./media/sapboc-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, selecteert u **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/sapboc-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** in het dialoogvenster, selecteer **toevoegen**.
 
    ![Het maken van een Azure AD-testgebruiker](./media/sapboc-tutorial/create_aaduser_03.png) 

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:
 
    1. In de **naam** Voer **BrittaSimon**.

    2. In de **gebruikersnaam** voert u het e-mailadres van de gebruiker Britta Simon.

    3. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    4. Selecteer **Maken**.

        ![Het dialoogvenster gebruiker](./media/sapboc-tutorial/create_aaduser_04.png) 

    ![Azure AD-gebruiker maken][100]

### <a name="create-an-sap-business-object-cloud-test-user"></a>Maak een testgebruiker SAP Business Object Cloud

Azure AD-gebruikers moeten worden ingericht in SAP Business Object Cloud voordat ze kunnen zich aanmelden bij de SAP-Cloud voor bedrijven-Object. In SAP Business Object Cloud is inrichten een handmatige taak.

Voor het inrichten van een gebruikersaccount:

1. Meld u aan uw bedrijf SAP Business Object Cloud site als een beheerder.

2. Selecteer **Menu** > **Security** > **gebruikers**.

    ![Werknemer toevoegen](./media/sapboc-tutorial/user1.png)

3. Op de **gebruikers** pagina toe te voegen nieuwe gebruikersgegevens **+**. 

    ![Gebruikers toevoegen](./media/sapboc-tutorial/user4.png)

    Voltooi de volgende stappen uit:

    1. In de **gebruikers-ID** voert u de gebruikers-ID van de gebruiker, zoals **Julia**.

    2. In de **VOORNAAM** voert u de voornaam van de gebruiker, zoals **Julia**.

    3. In de **ACHTERNAAM** voert u de achternaam van de gebruiker, zoals **Simon**.

    4. In de **WEERGAVENAAM** voert u de volledige naam van de gebruiker, zoals **Britta Simon**.

    5. In de **e** voert u het e-mailadres van de gebruiker, zoals **brittasimon@contoso.com**.

    6. Op de **rollen selecteren** pagina, selecteer de juiste rol voor de gebruiker en selecteer vervolgens **OK**.

      ![Rol selecteren](./media/sapboc-tutorial/user3.png)

    7. Selecteer de **opslaan** pictogram.    


### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt toestaan u de gebruiker Britta Simon gebruik van Azure AD eenmalige aanmelding door de gebruikersaccount toegang verlenen tot SAP Business Object Cloud.

Britta Simon toewijzen aan SAP Business Object Cloud:

1. In de Azure-portal, opent u de weergave van toepassingen en gaat u naar de directoryweergave. Selecteer **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **SAP Business Object Cloud**.

    ![Eenmalige aanmelding configureren](./media/sapboc-tutorial/tutorial_sapboc_app.png) 

3. Selecteer in het menu links **gebruikers en groepen**.

    ![Gebruikers en groepen selecteren][202] 

4. Selecteer **Toevoegen**. Klik vervolgens op de **toevoegen toewijzing** weergeeft, schakelt **gebruikers en groepen**.

    ![De pagina toewijzing toevoegen][203]

5. Op de **gebruikers en groepen** pagina in de lijst met gebruikers, selecteert **Britta Simon**.

6. Op de **gebruikers en groepen** weergeeft, schakelt **Selecteer**.

7. Op de **toevoegen toewijzing** weergeeft, schakelt **toewijzen**.

![De de gebruikersrol toewijzen][200] 
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw configuratie Azure AD eenmalige aanmelding testen met behulp van het toegangsvenster.

Wanneer u de tegel SAP Business Object Cloud in het toegangsvenster selecteert, moet u worden automatisch aangemeld bij uw cloudtoepassing SAP Business-Object.

Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/sapboc-tutorial/tutorial_general_01.png
[2]: ./media/sapboc-tutorial/tutorial_general_02.png
[3]: ./media/sapboc-tutorial/tutorial_general_03.png
[4]: ./media/sapboc-tutorial/tutorial_general_04.png

[100]: ./media/sapboc-tutorial/tutorial_general_100.png

[200]: ./media/sapboc-tutorial/tutorial_general_200.png
[201]: ./media/sapboc-tutorial/tutorial_general_201.png
[202]: ./media/sapboc-tutorial/tutorial_general_202.png
[203]: ./media/sapboc-tutorial/tutorial_general_203.png

