---
title: 'Zelfstudie: Azure Active Directory-integratie met TOPdesk - openbare | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en TOPdesk - publiek.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: f3190cb7f837464dd211acb127c4ab6f86e7a9f4
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55153297"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a>Zelfstudie: Azure Active Directory-integratie met TOPdesk - algemeen

In deze zelfstudie leert u hoe u integreert TOPdesk - openbare met Azure Active Directory (Azure AD).

Integratie van TOPdesk - openbare met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot TOPdesk - publiek heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij TOPdesk - openbare (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Azure AD-integratie configureren met TOPdesk - openbaar is, moet u de volgende items:

- Een Azure AD-abonnement
- Een TOPdesk - openbare eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. TOPdesk - openbare uit de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-topdesk---public-from-the-gallery"></a>TOPdesk - openbare uit de galerie toevoegen
Voor het configureren van de integratie van TOPdesk - openbare in Azure AD, moet u TOPdesk - publiek uit de galerie aan de lijst met beheerde SaaS-apps toevoegen.

**Als u wilt toevoegen, TOPdesk - openbare uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **TOPdesk - openbare**, selecteer **TOPdesk - openbare** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![TOPdesk - publiek in de lijst met resultaten](./media/topdesk-public-tutorial/tutorial_topdesk-public_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u configureren en testen op Azure AD eenmalige aanmelding met TOPdesk - openbare op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding voor het werk, Azure AD moet weten wat de equivalente-gebruiker in TOPdesk - openbaar is aan een gebruiker in Azure AD. Met andere woorden, een koppeling de relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in TOPdesk - openbare moet tot stand worden gebracht.

In TOPdesk - openbaar, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met TOPdesk - openbaar is, moet u de volgende bouwstenen uitvoeren:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
1. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
1. **[Maken van een TOPdesk - openbare testgebruiker](#create-a-topdesk---public-test-user)**  : als u wilt een equivalent van Britta Simon in TOPdesk - openbare die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
1. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en eenmalige aanmelding in uw TOPdesk - openbare toepassing configureren.

**Het configureren van Azure AD eenmalige aanmelding met TOPdesk - openbaar, voer de volgende stappen uit:**

1. In de Azure-portal op de **TOPdesk - openbare** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/topdesk-public-tutorial/tutorial_topdesk-public_samlbase.png)

1. Op de **TOPdesk - openbaar domein en URL's** sectie, voert u de volgende stappen uit:

    ![TOPdesk - openbaar domein en URL's, eenmalige aanmelding informatie](./media/topdesk-public-tutorial/tutorial_topdesk-public_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<companyname>.topdesk.net`
    
    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.topdesk.net/tas/public/login/verify`

    c. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://<companyname>.topdesk.net/tas/public/login/saml`
     
    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke id, antwoord-URL en aanmeldings-URL. Antwoord-URL is explaned verderop in de zelfstudie. Neem contact op met [TOPdesk - ondersteuningsteam voor openbare Client](https://help.topdesk.com/saas/enterprise/user/) om deze waarden te verkrijgen.  

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/topdesk-public-tutorial/tutorial_topdesk-public_certificate.png) 

1. Klik op **opslaan** knop.

    ![De knop voor enkelvoudige aanmelding configureren](./media/topdesk-public-tutorial/tutorial_general_400.png)
    
1. Op de **TOPdesk - configuratie voor een openbare** sectie, klikt u op **configureren TOPdesk - openbare** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![TOPdesk - openbare configuratie](./media/topdesk-public-tutorial/tutorial_topdesk-public_configure.png) 

1. Meld u aan bij uw **TOPdesk - openbare** bedrijf site als beheerder.

1. Klik in het menu van **TOPdesk** op **Settings**.
   
    ![Settings](./media/topdesk-public-tutorial/ic790598.png "Settings")

1. Klik op **Login Settings**.
   
    ![Login Settings](./media/topdesk-public-tutorial/ic790599.png "Login Settings")

1. Vouw het menu **Login Settings** uit en klik op **General**.
   
    ![General](./media/topdesk-public-tutorial/ic790600.png "General")

1. In de **openbare** sectie van de **SAML-aanmelding** configuratie sectie, voert u de volgende stappen uit:
   
    ![Technical Settings](./media/topdesk-public-tutorial/ic790601.png "Technical Settings")
   
    a. Klik op **Downloaden** om het openbare metagegevensbestand te downloaden en sla het lokaal op uw computer op.
   
    b. Open het gedownloade metagegevensbestand en zoek vervolgens de **AssertionConsumerService** knooppunt.

    ![AssertionConsumerService](./media/topdesk-public-tutorial/ic790619.png "AssertionConsumerService")
   
    c. Kopieer de **AssertionConsumerService** waarde, plak deze waarde in de **antwoord-URL** -tekstvak in **TOPdesk - openbaar domein en URL's** sectie.      
   
1. Als u een certificaatbestand wilt maken, moet u de volgende stappen uitvoeren:
    
    ![Certificaat](./media/topdesk-public-tutorial/ic790606.png "Certificaat")
    
    a. Open het gedownloade metagegevensbestand vanuit de Azure-portal.
    
    b. Vouw het knooppunt **RoleDescriptor** uit waarvan **xsi:type** is ingesteld op **fed:ApplicationServiceType**.
    
    c. Kopieer de waarde van het knooppunt **X509Certificate**.
    
    d. Sla de gekopieerde waarde van **X509Certificate** lokaal op uw computer op in een bestand.

1. Klik in de sectie **Public** op **Add**.
    
    ![SAML-aanmelding](./media/topdesk-public-tutorial/ic790625.png "SAML-aanmelding")

1. Voer de volgende stappen uit in het dialoogvenster **SAML configuration assistant**:
    
    ![SAML Configuration Assistant](./media/topdesk-public-tutorial/ic790608.png "SAML Configuration Assistant")
    
    a. Klik onder **Federatieve metagegevens** op **Bladeren** om het gedownloade metagegevensbestand te uploaden vanuit de Azure-portal.

    b. Klik onder **Certificaat (RSA)** op **Bladeren** om het certificaatbestand te uploaden.

    c. Klik onder het **Logo-pictogram** op **Bladeren** om het logobestand dat u van het TOPdesk-ondersteuningsteam hebt verkregen, te uploaden.

    d. In het tekstvak voor het **gebruikersnaamkenmerk** typt u `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. Typ in het tekstvak **Weergavenaam** een naam voor de configuratie.

    f. Klik op **Opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/topdesk-public-tutorial/create_aaduser_01.png)

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/topdesk-public-tutorial/create_aaduser_02.png)

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/topdesk-public-tutorial/create_aaduser_03.png)

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/topdesk-public-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-topdesk---public-test-user"></a>Een TOPdesk - openbare testgebruiker maken

Om in te schakelen van Azure AD-gebruikers zich aanmelden bij TOPdesk - openbaar, ze moeten worden ingericht voor TOPdesk - publiek.  
In het geval van TOPdesk - openbaar, inrichting is een handmatige taak.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voer de volgende stappen uit om de inrichting van gebruikers te configureren:
1. Meld u aan bij uw **TOPdesk - openbare** bedrijf site als administrator.

1. Klik in het menu aan de bovenkant op **TOPdesk \> nieuw \> ondersteuningsbestanden \> persoon**.
   
    ![Person](./media/topdesk-public-tutorial/ic790628.png "Person")

1. In het dialoogvenster nieuwe persoon, moet u de volgende stappen uitvoeren:
   
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

1. Selecteer in de lijst met toepassingen, **TOPdesk - openbare**.

    ![De TOPdesk - openbare koppeling in de lijst met toepassingen](./media/topdesk-public-tutorial/tutorial_topdesk-public_app.png)  

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u klikt op de TOPdesk - openbare tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw TOPdesk - openbare toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

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

