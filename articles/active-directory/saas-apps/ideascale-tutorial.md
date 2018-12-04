---
title: 'Zelfstudie: Azure Active Directory-integratie met IdeaScale | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en IdeaScale.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: ecb73e4b520936b573254f2cf209d4a02c0fdd32
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848745"
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Zelfstudie: Azure Active Directory-integratie met IdeaScale

In deze zelfstudie leert u hoe u IdeaScale integreren met Azure Active Directory (Azure AD).

IdeaScale integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot IdeaScale heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij IdeaScale (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met IdeaScale, moet u de volgende items:

- Een Azure AD-abonnement
- Een IdeaScale eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. IdeaScale uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-ideascale-from-the-gallery"></a>IdeaScale uit de galerie toe te voegen
Voor het configureren van de integratie van IdeaScale in Azure AD, moet u IdeaScale uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen IdeaScale uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
1. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

1. Typ in het zoekvak **IdeaScale**.

    ![Het maken van een Azure AD-testgebruiker](./media/ideascale-tutorial/tutorial_ideascale_search.png)

1. Selecteer in het deelvenster resultaten **IdeaScale**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met IdeaScale op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in IdeaScale is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in IdeaScale tot stand worden gebracht.

In IdeaScale, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met IdeaScale, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker IdeaScale](#creating-an-ideascale-test-user)**  : als u wilt een equivalent van Britta Simon in IdeaScale die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing IdeaScale.

**Voor het configureren van Azure AD eenmalige aanmelding met IdeaScale, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **IdeaScale** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/ideascale-tutorial/tutorial_ideascale_samlbase.png)

1. Op de **IdeaScale domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/ideascale-tutorial/tutorial_ideascale_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<companyname>.ideascale.com`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon:
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL en -id. Neem contact op met [IdeaScale Client ondersteuningsteam](https://support.ideascale.com/) om deze waarden te verkrijgen. 
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/ideascale-tutorial/tutorial_ideascale_certificate.png) 

1. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/ideascale-tutorial/tutorial_general_400.png)

1. Op de **IdeaScale configuratie** sectie, klikt u op **configureren IdeaScale** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL en SAML entiteit-ID** uit de **Naslaggids sectie**.

    ![Eenmalige aanmelding configureren](./media/ideascale-tutorial/tutorial_ideascale_configure.png) 

1. In een ander browservenster aanmelden bij uw bedrijf IdeaScale site als beheerder.

1. Ga naar **Community instellingen**.
   
    ![Instellingen van de community](./media/ideascale-tutorial/ic790847.png "Community-instellingen")

1. Ga naar **Security \> eenmalige aanmelding instellingen**.
   
    ![Eenmalige aanmelding instellingen](./media/ideascale-tutorial/ic790848.png "eenmalige aanmelding-instellingen")

1. Als **eenmaal aanmelden Type**, selecteer **SAML 2.0**.
   
    ![Eenmalige aanmelding Type](./media/ideascale-tutorial/ic790849.png "eenmalige aanmelding-Type")

1. Op de **instellingen voor eenmalige aanmelding** dialoogvenster, voer de volgende stappen uit:
   
    ![Eenmalige aanmelding instellingen](./media/ideascale-tutorial/ic790850.png "eenmalige aanmelding-instellingen")
   
    a. In **SAML IdP entiteit-ID** tekstvak, plak de waarde van **SAML entiteit-ID** die u hebt gekopieerd vanuit Azure portal.

    b. De inhoud van uw gedownloade metagegevensbestand kopiëren vanuit Azure portal en plak deze in de **SAML-metagegevens voor IdP** tekstvak.

    c. In **afmeldings-URL van succes** tekstvak, plak de waarde van **afmelding URL** die u hebt gekopieerd vanuit Azure portal.

    d. Klik op **wijzigingen opslaan**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/ideascale-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/ideascale-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/ideascale-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/ideascale-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-ideascale-test-user"></a>Het maken van een testgebruiker IdeaScale

Om in te schakelen in Azure AD-gebruikers zich aanmelden bij IdeaScale, moeten ze worden ingericht voor IdeaScale. In het geval van IdeaScale is inrichten een handmatige taak.

**Als u wilt inrichten van gebruikers configureren, moet u de volgende stappen uitvoeren:**

1. Meld u aan bij uw **IdeaScale** bedrijf site als administrator.

1. Ga naar **Community instellingen**.
   
    ![Instellingen van de community](./media/ideascale-tutorial/ic790847.png "Community-instellingen")

1. Ga naar **basisinstellingen \> lid Management**.

1. Klik op **lid toevoegen**.
   
    ![Lid Management](./media/ideascale-tutorial/ic790852.png "lid Management")

1. Voer de volgende stappen uit in de sectie van de nieuwe leden toevoegen:
   
    ![Nieuw lid toevoegt](./media/ideascale-tutorial/ic790853.png "nieuw lid toevoegen")
   
    a. In de **e-mailadressen** tekstvak typt u het e-mailadres van een geldige AAD-account dat u inrichten wilt.
   
    b. Klik op **wijzigingen opslaan**. 
   
    >[!NOTE]
    >De houder van Azure Active Directory-account ontvangt een e-mail met een koppeling naar het account te bevestigen voordat deze geactiveerd wordt.
      
>[!NOTE]
>U kunt alle andere IdeaScale gebruiker-account maken van hulpprogramma's of API's geleverd door IdeaScale aan inrichten AAD-gebruikersaccounts.
 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan IdeaScale.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan IdeaScale toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **IdeaScale**.

    ![Eenmalige aanmelding configureren](./media/ideascale-tutorial/tutorial_ideascale_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen


Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.

Wanneer u op de tegel IdeaScale in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing IdeaScale.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/ideascale-tutorial/tutorial_general_01.png
[2]: ./media/ideascale-tutorial/tutorial_general_02.png
[3]: ./media/ideascale-tutorial/tutorial_general_03.png
[4]: ./media/ideascale-tutorial/tutorial_general_04.png

[100]: ./media/ideascale-tutorial/tutorial_general_100.png

[200]: ./media/ideascale-tutorial/tutorial_general_200.png
[201]: ./media/ideascale-tutorial/tutorial_general_201.png
[202]: ./media/ideascale-tutorial/tutorial_general_202.png
[203]: ./media/ideascale-tutorial/tutorial_general_203.png

