---
title: 'Zelfstudie: Azure Active Directory-integratie met HR2day door Merces | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en HR2day door Merces.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 853d08c9-27b1-48d4-b8e7-3705140eb67f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: bb03506bac22c8c82e856f403710a4908af6f9de
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55149251"
---
# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a>Zelfstudie: Azure Active Directory-integratie met HR2day door Merces

In deze zelfstudie leert u hoe u HR2day door Merces integreren met Azure Active Directory (Azure AD).

HR2day door Merces integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot HR2day door Merces heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij HR2day door Merces met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie, de Azure-portal beheren.

Zie voor meer informatie over de integratie met Azure AD SaaS [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met HR2day door Merces, moet u de volgende items:

- Een Azure AD-abonnement
- Een HR2day door Merces eenmalige aanmelding ingeschakeld abonnement.

> [!NOTE]
> U kunt beter geen een productie-omgeving voor het testen van de stappen in deze zelfstudie.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik uw productie-omgeving niet als dat nodig is.
- Krijgen een [één maand gratis proefversie van Azure AD](https://azure.microsoft.com/pricing/free-trial/) als u nog geen hebt.  

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario dat hier wordt beschreven, bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen HR2day door Merces uit de galerie.
1. Configureren en testen van Azure AD eenmalige aanmelding.

## <a name="add-hr2day-by-merces-from-the-gallery"></a>HR2day door Merces uit de galerie toevoegen
Voor het configureren van de integratie van HR2day door Merces in Azure AD, HR2day door Merces uit de galerie te toevoegt aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen HR2day door Merces uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de [Azure-portal](https://portal.azure.com), selecteer in het navigatiedeelvenster links in de **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Ga naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u wilt een nieuwe toepassing toevoegen, selecteert u de **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **HR2day door Merces**.

    ![Het maken van een Azure AD-testgebruiker](./media/hr2day-tutorial/tutorial_hr2daybymerces_search.png)

1. Selecteer in het deelvenster resultaten **HR2day door Merces**, en selecteer vervolgens de **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/hr2day-tutorial/tutorial_hr2daybymerces_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met HR2day door Merces op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD te weten wie de gebruiker equivalent in HR2day door Merces is aan een gebruiker in Azure AD. Met andere woorden, moet u een koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in HR2day door Merces vast te stellen.

In HR2day door Merces toewijzen de **gebruikersnaam** in Azure AD **gebruikersnaam** de relatie tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met HR2day door Merces, moet u de volgende bouwstenen voltooien:

1. [Azure AD eenmalige aanmelding configureren](#configuring-azure-ad-single-sign-on): Zodat uw gebruikers kunnen deze functie wilt gebruiken.
1. [Maak een Azure AD-testgebruiker](#creating-an-azure-ad-test-user): Azure AD eenmalige aanmelding met Britta Simon testen.
1. [Maken van een HR2day door Merces testgebruiker](#creating-an-hr2day-by-merces-test-user): Maak een equivalent van Britta Simon in HR2day door Merces die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. [Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user): Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. [Eenmalige aanmelding testen](#testing-single-sign-on): Controleer of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw HR2day door Merces toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met HR2day door Merces, voert u de volgende stappen uit:**

1. In de Azure-portal op de **HR2day door Merces** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Voor het inschakelen van eenmalige aanmelding in de **eenmalige aanmelding** in het dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding**.
 
    ![Eenmalige aanmelding configureren](./media/hr2day-tutorial/tutorial_hr2daybymerces_samlbase.png)

1. In de **HR2day Merces domein en URL's** sectie, de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/hr2day-tutorial/tutorial_hr2daybymerces_url.png)

    a. In de **aanmeldings-URL** vak, typ een URL met behulp van het volgende patroon: `https://<tenantname>.force.com/<instancename>`.

    b. In de **id** vak, typ een URL met behulp van het volgende patroon: `https://hr2day.force.com/<companyname>`.

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met de [HR2day door Merces client ondersteuningsteam](mailto:servicedesk@merces.nl) om deze waarden te verkrijgen. 
 


1. Op de **SAML-handtekeningcertificaat** sectie, selecteer **Certificate(Base64)**, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/hr2day-tutorial/tutorial_hr2daybymerces_certificate.png) 

1. Deze sectie wordt beschreven hoe u gebruikers wilt verifiëren op HR2day door Merces met hun account in Azure AD. Ze doen dit met behulp van de Federatie die gebaseerd op het SAML-protocol.

    Uw HR2day door Merces toepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw SAML-token is vereist. De volgende schermafbeelding ziet u een voorbeeld hiervan. 

    ![Eenmalige aanmelding configureren](./media/hr2day-tutorial/tutorial_hr2day_00.png)
    
    > [!NOTE] 
    Voordat u de SAML-verklaring configureren kunt, moet u contact met de [HR2day door het ondersteuningsteam Merces Client](mailto:servicedesk@merces.nl) en vraagt u de waarde van het kenmerk unieke id voor uw tenant. U moet deze waarde om de stappen in de volgende sectie te voltooien. 

1. In de **eenmalige aanmelding** in het dialoogvenster de **gebruikerskenmerken** sectie, het configureren van het kenmerk van SAML-token, zoals wordt weergegeven in de volgende afbeelding. Vervolgens de volgende stappen uitvoeren.
    
      | De naam van kenmerk    |   De waarde van kenmerk |  
    | ------------------- | -------------------- |    
    | ATTR_LOGINCLAIM | `join([mail],"102938475Z","@"` |
    
      a. Om te openen de **kenmerk toevoegen** dialoogvenster, selecteer **kenmerk toevoegen**.

    ![Eenmalige aanmelding configureren](./media/hr2day-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/hr2day-tutorial/tutorial_attribute_05.png)

    b. In de **naam** in het vak **ATTR_LOGINCLAIM**.

    c. Uit de **waarde** in de lijst met **Join()**.

    d. Uit de **tekenreeks1** in de lijst met **user.mail**.

    e. Voor **tekenreeks2**, typt u de unieke id die wordt geleverd door uw team HR2day.

    f. In de **scheidingsteken** in het vak **\@**.
    
    g. Selecteer **OK**.

1. Selecteer de knop **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/hr2day-tutorial/tutorial_general_400.png)

1. In de **HR2day door Merces configuratie** sectie, selecteer **HR2day configureren door Merces** openen de **aanmelding configureren** venster. Kopiëren de **afmelding URL**, **SAML entiteit-ID**, en **Single Sign-On Service URL voor SAML** uit de **Naslaggids** sectie.

    ![Eenmalige aanmelding configureren](./media/hr2day-tutorial/tutorial_hr2daybymerces_configure.png) 

1. Voor het configureren van eenmalige aanmelding voor uw toepassing, neem contact op met de [HR2day door Merces client ondersteuningsteam](mailTo:servicedesk@merces.nl). Koppel de gedownloade **Certificate(Base64)** -bestand naar uw e-mailadres. Bieden ook de **afmelding URL**, **SAML entiteit-ID**, en **Single Sign-On Service URL voor SAML** zodat ze kunnen worden geconfigureerd voor integratie van eenmalige aanmelding.

    > [!NOTE]
    >Vermeld aan het team Merces dat deze integratie de entiteit-ID moet moet worden ingesteld met het patroon **https://hr2day.force.com/INSTANCENAME**.

    > [!TIP]
    >U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Nadat u deze app uit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad. Vervolgens toegang tot de ingesloten documentatie via de **configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie in de [documentatie over Azure AD embedded]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, selecteer in het navigatiedeelvenster links in de **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/hr2day-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en selecteer vervolgens **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/hr2day-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** in het dialoogvenster, selecteer **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/hr2day-tutorial/create_aaduser_03.png) 

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/hr2day-tutorial/create_aaduser_04.png) 

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven**, en noteer het wachtwoord.

    d. Selecteer **Maken**.
 
### <a name="create-an-hr2day-by-merces-test-user"></a>Een HR2day door Merces testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in HR2day door Merces. Om toe te voegen de gebruikers in het account HR2day, werken met de [HR2day door Merces client ondersteuningsteam](mailto:servicedesk@merces.nl). 

> [!NOTE]
> Als u maken van een gebruiker handmatig wilt, neem dan contact op met de [HR2day door Merces client ondersteuningsteam](mailto:servicedesk@merces.nl).

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot HR2day door Merces.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan HR2day door Merces toewijst, moet u de volgende stappen uitvoeren:**

1. In Azure portal, opent u de weergave van toepassingen, gaat u naar de directoryweergave en ga vervolgens naar **bedrijfstoepassingen**. Selecteer vervolgens **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **HR2day door Merces**.

    ![Eenmalige aanmelding configureren](./media/hr2day-tutorial/tutorial_hr2daybymerces_app.png) 

1. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Selecteer de knop **Add**. Klik in de **toevoegen toewijzing** in het dialoogvenster, selecteer **gebruikers en groepen**.

    ![Gebruiker toewijzen][203]

1. In de **gebruikers en groepen** in het dialoogvenster de **gebruikers** in de lijst met **Britta Simon**.

1. Klik op de **Selecteer** knop.

1. In de **toevoegen toewijzing** in het dialoogvenster, selecteer **toewijzen**.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding-configuratie met behulp van het toegangsvenster.  

Wanneer u de HR2day door Merces tegel in het toegangsvenster selecteert, ophalen u automatisch aangemeld bij uw HR2day door Merces toepassing.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het SaaS-Apps integreren met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/hr2day-tutorial/tutorial_general_01.png
[2]: ./media/hr2day-tutorial/tutorial_general_02.png
[3]: ./media/hr2day-tutorial/tutorial_general_03.png
[4]: ./media/hr2day-tutorial/tutorial_general_04.png

[100]: ./media/hr2day-tutorial/tutorial_general_100.png

[200]: ./media/hr2day-tutorial/tutorial_general_200.png
[201]: ./media/hr2day-tutorial/tutorial_general_201.png
[202]: ./media/hr2day-tutorial/tutorial_general_202.png
[203]: ./media/hr2day-tutorial/tutorial_general_203.png

