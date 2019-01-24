---
title: 'Zelfstudie: Azure Active Directory-integratie met de Portal financiering | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de financiering Portal.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 4663cc8a-976a-4c6c-b3b4-1e5df9b66744
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: ada7a35f36dbac74fa67111c8fa2571dce70069d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54822124"
---
# <a name="tutorial-azure-active-directory-integration-with-the-funding-portal"></a>Zelfstudie: Azure Active Directory-integratie met de financiering Portal

In deze zelfstudie leert u hoe u de financiering Portal integreren met Azure Active Directory (Azure AD).

De financiering Portal integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot de financiering Portal heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij de financiering Portal (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met de financiering Portal, moet u de volgende items:

- Een Azure AD-abonnement
- Een Portal voor de financiering eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. De financiering Portal uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-the-funding-portal-from-the-gallery"></a>De financiering Portal uit de galerie toe te voegen
Voor het configureren van de integratie van de financiering Portal in Azure AD, moet u de financiering Portal vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen de financiering Portal vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **de financiering Portal**.

    ![Het maken van een Azure AD-testgebruiker](./media/thefundingportal-tutorial/tutorial_thefundingportal_search.png)

1. Selecteer in het deelvenster resultaten **de financiering Portal**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/thefundingportal-tutorial/tutorial_thefundingportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met de financiering-Portal op basis van een testgebruiker 'Julia steen' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de Portal financiering is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de Portal financiering tot stand worden gebracht.

In de Portal voor financiering, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met de financiering Portal, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van de Portal financiering testgebruiker](#creating-the-funding-portal-test-user)**  : als u wilt een equivalent van Britta Simon hebben in de Portal financiering die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing de financiering Portal.

**Voor het configureren van Azure AD eenmalige aanmelding met de financiering Portal, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **de financiering Portal** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/thefundingportal-tutorial/tutorial_thefundingportal_samlbase.png)

1. Op de **Portal van de financiering domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/thefundingportal-tutorial/tutorial_thefundingportal_url.png)

    a. Typ in het tekstvak **Aanmeldings-URL** een URL met het volgende patroon: `https://<subdomain>.regenteducation.net/`

    b. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<subdomain>.regenteducation.net`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden bij met de daadwerkelijke aanmeldings-URL en id. Neem contact op met [de financiering Portal Client ondersteuningsteam](mailto:info@regenteducation.com) om deze waarden te verkrijgen. 

1. De toepassing financiering Portal wordt verwacht dat de SAML-asserties ondertekend bevat een kenmerk met de naam 'externalId1'. De waarde van 'externalId1' moet een herkende studentID. Configureer de claim 'externalId1' voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de **gebruikerskenmerken** van de toepassing. In de volgende schermopname ziet u een voorbeeld hiervan.

    ![Eenmalige aanmelding configureren](./media/thefundingportal-tutorial/tutorial_thefundingportal_attribute.png)

1. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en voer de volgende stappen uit:

    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | ---------------- |
    | externalId1 | user.extensionattribute1 |

    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/thefundingportal-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/thefundingportal-tutorial/tutorial_attribute_05.png)

    b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven.

    c. Uit de **kenmerkwaarde** , selecteert u het kenmerk dat u wilt gebruiken voor uw implementatie. Bijvoorbeeld, als u de waarde StudentID in de ExtensionAttribute1 hebt opgeslagen, selecteert u user.extensionattribute1.
    
    d. Klik op **OK**.
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/thefundingportal-tutorial/tutorial_thefundingportal_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/thefundingportal-tutorial/tutorial_general_400.png)

1. Het configureren van eenmalige aanmelding op **de financiering Portal** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [de financiering Portal ondersteuningsteam](mailto:info@regenteducation.com). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/thefundingportal-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/thefundingportal-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/thefundingportal-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/thefundingportal-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-the-funding-portal-test-user"></a>Het maken van de Portal financiering testgebruiker

In deze sectie maakt u een gebruiker met de naam van Britta Simon in de Portal financiering. Werken met [de financiering Portal ondersteuningsteam](mailto:info@regenteducation.com) toevoegen van de testgebruiker en eenmalige aanmelding inschakelen.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen tot de financiering Portal.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon tot de financiering Portal, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **de financiering Portal**.

    ![Eenmalige aanmelding configureren](./media/thefundingportal-tutorial/tutorial_thefundingportal_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.

Wanneer u op de financiering Portal-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing de financiering Portal.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/thefundingportal-tutorial/tutorial_general_04.png

[100]: ./media/thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/thefundingportal-tutorial/tutorial_general_201.png
[202]: ./media/thefundingportal-tutorial/tutorial_general_202.png
[203]: ./media/thefundingportal-tutorial/tutorial_general_203.png

