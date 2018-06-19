---
title: 'Zelfstudie: Azure Active Directory-integratie met PatentSQUARE | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en PatentSQUARE.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5ab95cea-4839-4588-b2d0-c8b7066415a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeedes
ms.openlocfilehash: 763dd73a65a6e459f334ed23401e4e0af187f5b3
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35914894"
---
# <a name="tutorial-azure-active-directory-integration-with-patentsquare"></a>Zelfstudie: Azure Active Directory-integratie met PatentSQUARE

In deze zelfstudie leert u hoe PatentSQUARE integreren met Azure Active Directory (Azure AD).

PatentSQUARE integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot PatentSQUARE heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij PatentSQUARE (Single Sign-On) met hun Azure AD-accounts kunt inschakelen.
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren.

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met PatentSQUARE, moet u de volgende items:

- Een Azure AD-abonnement
- Een PatentSQUARE eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. PatentSQUARE uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-patentsquare-from-the-gallery"></a>PatentSQUARE uit de galerie toevoegen
Voor het configureren van de integratie van PatentSQUARE in Azure AD, moet u PatentSQUARE uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen PatentSQUARE uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop Nieuw toepassing][3]

4. Typ in het zoekvak **PatentSQUARE**, selecteer **PatentSQUARE** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![PatentSQUARE in de lijst met resultaten](./media/patentsquare-tutorial/tutorial_patentsquare_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en testen eenmalige aanmelding Azure AD

In deze sectie configureert en test eenmalige aanmelding Azure AD met PatentSQUARE op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in PatentSQUARE is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in PatentSQUARE tot stand worden gebracht.

Om te configureren en testen van Azure AD eenmalige aanmelding met PatentSQUARE, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een testgebruiker PatentSQUARE](#create-a-patentsquare-test-user)**  - PatentSQUARE die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Test eenmalige aanmelding](#test-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding in uw toepassing PatentSQUARE configureren.

**Voor het configureren van Azure AD eenmalige aanmelding met PatentSQUARE, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **PatentSQUARE** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Dialoogvenster voor eenmalige aanmelding](./media/patentsquare-tutorial/tutorial_patentsquare_samlbase.png)

3. Op de **PatentSQUARE domein en de URL's** sectie, voert u de volgende stappen uit:

    ![URL's en PatentSQUARE domein eenmalige aanmelding informatie](./media/patentsquare-tutorial/tutorial_patentsquare_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met het volgende patroon volgen: `https://<companysubdomain>.pat-dss.com:443/patlics/secure/aad`

    b. In de **id** textbox, typ een URL met het volgende patroon volgen: `https://<companysubdomain>.pat-dss.com:443/patlics`

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![De downloadkoppeling certificaat](./media/patentsquare-tutorial/tutorial_patentsquare_certificate.png) 

5. Klik op **opslaan** knop.

    ![Knop Single Sign-On opslaan configureren](./media/patentsquare-tutorial/tutorial_general_400.png)

6. Eenmalige aanmelding configureren op **PatentSQUARE** zijde, moet u de gedownloade verzenden **Metadata XML** naar [PatentSQUARE ondersteuningsteam](https://www.panasonic.com/jp/business/its/patentsquare.html). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

   ![Een Azure AD-testgebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure-portal in het linkerdeelvenster op het **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/patentsquare-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/patentsquare-tutorial/create_aaduser_02.png)

3. Openen van de **gebruiker** in het dialoogvenster klikt u op **toevoegen** boven aan de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/patentsquare-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voert u de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/patentsquare-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje, en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-patentsquare-test-user"></a>Een testgebruiker PatentSQUARE maken

In deze sectie kunt u een gebruiker Britta Simon aangeroepen in PatentSQUARE maken. Werken met [PatentSQUARE ondersteuningsteam](https://www.panasonic.com/jp/business/its/patentsquare.html) de gebruikers van het platform PatentSQUARE toevoegen. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan PatentSQUARE.

![Toewijzen van de gebruikersrol][200] 

**Britta Simon om aan te wijzen PatentSQUARE, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **PatentSQUARE**.

    ![De koppeling PatentSQUARE in de lijst met toepassingen](./media/patentsquare-tutorial/tutorial_patentsquare_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel PatentSQUARE in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw toepassing PatentSQUARE.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/patentsquare-tutorial/tutorial_general_01.png
[2]: ./media/patentsquare-tutorial/tutorial_general_02.png
[3]: ./media/patentsquare-tutorial/tutorial_general_03.png
[4]: ./media/patentsquare-tutorial/tutorial_general_04.png

[100]: ./media/patentsquare-tutorial/tutorial_general_100.png

[200]: ./media/patentsquare-tutorial/tutorial_general_200.png
[201]: ./media/patentsquare-tutorial/tutorial_general_201.png
[202]: ./media/patentsquare-tutorial/tutorial_general_202.png
[203]: ./media/patentsquare-tutorial/tutorial_general_203.png

