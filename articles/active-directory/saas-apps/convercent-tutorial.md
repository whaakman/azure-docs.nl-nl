---
title: 'Zelfstudie: Azure Active Directory-integratie met Convercent | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Convercent.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: f9c9d290-0e13-490b-b559-0be772d6a690
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: jeedes
ms.openlocfilehash: 1fc70285a4852db5f76e72cc7222dbfe2e0884e6
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54822243"
---
# <a name="tutorial-azure-active-directory-integration-with-convercent"></a>Zelfstudie: Azure Active Directory-integratie met Convercent

In deze zelfstudie leert u hoe u Convercent integreren met Azure Active Directory (Azure AD).

Convercent integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Convercent heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij Convercent (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Convercent, moet u de volgende items:

- Een Azure AD-abonnement
- Een Convercent eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Convercent uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-convercent-from-the-gallery"></a>Convercent uit de galerie toe te voegen
Voor het configureren van de integratie van Convercent in Azure AD, moet u Convercent uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Convercent uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Convercent**.

    ![Het maken van een Azure AD-testgebruiker](./media/convercent-tutorial/tutorial_convercent_search.png)

1. Selecteer in het deelvenster resultaten **Convercent**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/convercent-tutorial/tutorial_convercent_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met Convercent op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in Convercent is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Convercent tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door toe te wijzen de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in Convercent.

Om te configureren en testen van Azure AD eenmalige aanmelding met Convercent, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Convercent](#creating-a-convercent-test-user)**  : als u wilt een equivalent van Britta Simon in Convercent die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing Convercent.

**Voor het configureren van Azure AD eenmalige aanmelding met Convercent, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Convercent** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/convercent-tutorial/tutorial_convercent_samlbase.png)

1. Op de **Convercent domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP gestart door modus**, de volgende stap uitvoeren:

    ![Eenmalige aanmelding configureren](./media/convercent-tutorial/tutorial_convercent_url.png)

    Typ in het tekstvak **Id** een URL met het volgende patroon: `https://<instancename>.convercent.com/`
 
1. Als u wilt configureren van de toepassing in **SP geïnitieerde modus**op de **Convercent domein en URL's** sectie de volgende stappen uitvoeren:
    
    ![Eenmalige aanmelding configureren](./media/convercent-tutorial/tutorial_convercent_url1.png)

     a. Klik op **'Show geavanceerde URL-instellingen.** 

     b. In de **aanmelding URL** tekstvak typt u de waarde met behulp van het volgende patroon: `https://<instancename>.convercent.com/`

     c. In de **Relaystatus** tekstvak typt u de waarde met behulp van het volgende patroon: `https://<instancename>.convercent.com/`

    > [!NOTE] 
    > Deze waarden zijn niet de werkelijke waarden. Deze waarden met de werkelijke-id, meld u op de URL en Relaystatus bijwerken. Neem contact op met [Convercent Client ondersteuningsteam](http://support.convercent.com) om deze waarden te verkrijgen.

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/convercent-tutorial/tutorial_convercent_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/convercent-tutorial/tutorial_general_400.png)

1. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met [Convercent ondersteuningsteam](mailto:support@convercent.com) en geef ze de gedownloade **Metadata XML**.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/convercent-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/convercent-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/convercent-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/convercent-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-convercent-test-user"></a>Het maken van een testgebruiker Convercent

Werken met [Convercent ondersteuningsteam](mailto:support@convercent.com) om toe te voegen de gebruikers in het Convercent-platform.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Convercent.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan Convercent toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Convercent**.

    ![Eenmalige aanmelding configureren](./media/convercent-tutorial/tutorial_convercent_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Convercent in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Convercent.
Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster. 

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)



<!--Image references-->

[1]: ./media/convercent-tutorial/tutorial_general_01.png
[2]: ./media/convercent-tutorial/tutorial_general_02.png
[3]: ./media/convercent-tutorial/tutorial_general_03.png
[4]: ./media/convercent-tutorial/tutorial_general_04.png

[100]: ./media/convercent-tutorial/tutorial_general_100.png

[200]: ./media/convercent-tutorial/tutorial_general_200.png
[201]: ./media/convercent-tutorial/tutorial_general_201.png
[202]: ./media/convercent-tutorial/tutorial_general_202.png
[203]: ./media/convercent-tutorial/tutorial_general_203.png

