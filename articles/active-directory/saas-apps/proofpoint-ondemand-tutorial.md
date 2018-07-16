---
title: 'Zelfstudie: Azure Active Directory-integratie met Proofpoint on-Demand | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Proofpoint op aanvraag.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 773e7f7d-ec31-411b-860d-6a6633335d43
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: jeedes
ms.openlocfilehash: 36668708cc787e7d515efdaa1c2038b967a74093
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051724"
---
# <a name="tutorial-azure-active-directory-integration-with-proofpoint-on-demand"></a>Zelfstudie: Azure Active Directory-integratie met Proofpoint op aanvraag

In deze zelfstudie leert u hoe u Proofpoint on-Demand integreren met Azure Active Directory (Azure AD).

Proofpoint on-Demand integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Proofpoint op aanvraag heeft.
- U kunt uw gebruikers automatisch ophalen aangemeld bij Proofpoint op aanvraag (Single Sign-On) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie - Azure portal beheren.

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Proofpoint op aanvraag, moet u de volgende items:

- Een Azure AD-abonnement
- Een Proofpoint op aanvraag eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Proofpoint op aanvraag vanuit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-proofpoint-on-demand-from-the-gallery"></a>Proofpoint op aanvraag vanuit de galerie toevoegen
Voor het configureren van de integratie van Proofpoint op aanvraag in Azure AD, moet u Proofpoint op aanvraag uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Proofpoint op aanvraag vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![De Azure Active Directory-knop][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![De blade Enterprise-toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![De knop nieuwe toepassing][3]

4. Typ in het zoekvak **Proofpoint on-Demand**, selecteer **Proofpoint on-Demand** van resultaat deelvenster klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Proofpoint op aanvraag in de lijst met resultaten](./media/proofpoint-ondemand-tutorial/tutorial_proofpointondemand_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureren en Azure AD eenmalige aanmelding testen

In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Proofpoint op aanvraag op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Proofpoint op aanvraag is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Proofpoint op aanvraag tot stand worden gebracht.

Wijs in Proofpoint op aanvraag, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Proofpoint op aanvraag, moet u de volgende bouwstenen voltooien:

1. **[Azure AD eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maak een Proofpoint op aanvraag testgebruiker](#create-a-proofpoint-on-demand-test-user)**  : als u wilt een equivalent van Britta Simon in Proofpoint op aanvraag die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#test-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Proofpoint on-Demand-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met Proofpoint op aanvraag, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Proofpoint on-Demand** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Koppeling voor eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/proofpoint-ondemand-tutorial/tutorial_proofpointondemand_samlbase.png)

3. Op de **Proofpoint op aanvraag domein en URL's** sectie, voert u de volgende stappen uit:

    ![Proofpoint op aanvraag domein en URL's één aanmeldings-informatie](./media/proofpoint-ondemand-tutorial/tutorial_proofpointondemand_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<hostname>.pphosted.com/ppssamlsp_hostname`

    b. In de **id** tekstvak, een URL met behulp van het volgende patroon: `https://<hostname>.pphosted.com/ppssamlsp`

    c. In de **antwoord-URL** tekstvak, een URL met behulp van het volgende patroon: `https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer`
    
    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke-id, de antwoord-URL en aanmeldings-URL. Neem contact op met [Proofpoint op aanvraag Client ondersteuningsteam](https://www.proofpoint.com/us/support-services) om deze waarden te verkrijgen.

5. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat](./media/proofpoint-ondemand-tutorial/tutorial_proofpointondemand_certificate.png) 

6. Klik op **opslaan** knop.

    ![Configureren van eenmalige aanmelding opslaan](./media/proofpoint-ondemand-tutorial/tutorial_general_400.png)
    
7. Op de **Proofpoint op verzoek-configuratie** sectie, klikt u op **Proofpoint configureren op aanvraag** openen **aanmelding configureren** venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Proofpoint op verzoek-configuratie](./media/proofpoint-ondemand-tutorial/tutorial_proofpointondemand_configure.png) 

8. Het configureren van eenmalige aanmelding op **Proofpoint on-Demand** zijde, moet u voor het verzenden van de gedownloade **Certificate(Base64)**, **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** naar [Proofpoint op aanvraag ondersteuningsteam](https://www.proofpoint.com/us/support-services). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Maak een testgebruiker Azure AD

Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

   ![Maak een testgebruiker Azure AD][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/proofpoint-ondemand-tutorial/create_aaduser_01.png)

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.

    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/proofpoint-ondemand-tutorial/create_aaduser_02.png)

3. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.

    ![De knop toevoegen](./media/proofpoint-ondemand-tutorial/create_aaduser_03.png)

4. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:

    ![Het dialoogvenster gebruiker](./media/proofpoint-ondemand-tutorial/create_aaduser_04.png)

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-a-proofpoint-on-demand-test-user"></a>Een Proofpoint op aanvraag testgebruiker maken

In deze sectie maakt u een gebruiker met de naam van Britta Simon in Proofpoint op aanvraag. Werken met [Proofpoint op aanvraag Client ondersteuningsteam](https://www.proofpoint.com/us/support-services) gebruikers toevoegen in de Proofpoint op aanvraag platform.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Proofpoint op aanvraag.

![De de gebruikersrol toewijzen][200] 

**Britta Simon om aan te wijzen Proofpoint op aanvraag, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Proofpoint on-Demand**.

    ![De Proofpoint op aanvraag koppeling in de lijst met toepassingen](./media/proofpoint-ondemand-tutorial/tutorial_proofpointondemand_app.png)  

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![De koppeling 'Gebruikers en groepen'][202]

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Het deelvenster toewijzing toevoegen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de Proofpoint op aanvraag tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Proofpoint on-Demand-toepassing.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/proofpoint-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/proofpoint-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/proofpoint-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/proofpoint-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/proofpoint-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/proofpoint-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/proofpoint-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/proofpoint-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/proofpoint-ondemand-tutorial/tutorial_general_203.png

