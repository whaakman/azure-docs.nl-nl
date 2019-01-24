---
title: 'Zelfstudie: Azure Active Directory-integratie met BetterWorks | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en BetterWorks.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 5bb9505a-be02-46ae-9979-5308715d2b47
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 1ccd960d0cc849cc1544ed151c081a7fa023a385
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54826765"
---
# <a name="tutorial-azure-active-directory-integration-with-betterworks"></a>Zelfstudie: Azure Active Directory-integratie met BetterWorks

In deze zelfstudie leert u hoe u BetterWorks integreren met Azure Active Directory (Azure AD).

BetterWorks integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot BetterWorks heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij BetterWorks (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met BetterWorks, moet u de volgende items:

- Een Azure AD-abonnement
- Een BetterWorks eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u nog geen proefversie van Azure AD hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) een proefversie van één maand aanvragen.

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. BetterWorks uit de galerie toe te voegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-betterworks-from-the-gallery"></a>BetterWorks uit de galerie toe te voegen
Voor het configureren van de integratie van BetterWorks in Azure AD, moet u BetterWorks uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen BetterWorks uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **BetterWorks**.

    ![Het maken van een Azure AD-testgebruiker](./media/betterworks-tutorial/tutorial_betterworks_search.png)

1. Selecteer in het deelvenster resultaten **BetterWorks**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/betterworks-tutorial/tutorial_betterworks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met BetterWorks op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in BetterWorks is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in BetterWorks tot stand worden gebracht.

In BetterWorks, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met BetterWorks, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker BetterWorks](#creating-a-betterworks-test-user)**  : als u wilt een equivalent van Britta Simon in BetterWorks die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing BetterWorks.

**Voor het configureren van Azure AD eenmalige aanmelding met BetterWorks, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **BetterWorks** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/betterworks-tutorial/tutorial_betterworks_samlbase.png)

1. Op de **BetterWorks domein en URL's** sectie, als u wilt configureren van de toepassing in **IDP gestart door modus**:

    ![Eenmalige aanmelding configureren](./media/betterworks-tutorial/tutorial_betterworks_url.png)

    a. Typ in het tekstvak **Id** een URL met het volgende patroon: `https://app.betterworks.com/saml2/metadata/`

    b. In het tekstvak **Antwoord-URL** typt u een URL met behulp van het volgende patroon: `https://app.betterworks.com/saml2/acs/`

1. Op de **BetterWorks domein en URL's** sectie, als u wilt configureren van de toepassing in **SP geïnitieerde modus**, voer de volgende stappen uit:
    
    ![Eenmalige aanmelding configureren](./media/betterworks-tutorial/tutorial_betterworks_url1.png)

    a. Klik op de **geavanceerde URL-instellingen weergeven**.

    b. In de **aanmelding URL** tekstvak, een URL met behulp van het volgende patroon: `https://app.betterworks.com`

    > [!NOTE] 
    > Dit zijn geen echte waarden. Werk deze waarden met de antwoord-URL, de id en de werkelijke teken op URL. Neem contact op met [BetterWorks ondersteuningsteam](mailto:support@betterworks.com) om deze waarden te verkrijgen.
 
1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/betterworks-tutorial/tutorial_betterworks_certificate.png)  

1. BetterWorks toepassing verwacht het SAML-asserties ondertekend in een specifieke indeling. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**kenmerk**" tabblad van de toepassing. In de volgende schermopname ziet u een voorbeeld hiervan. 

    ![Eenmalige aanmelding configureren](./media/betterworks-tutorial/tutorial_betterworks_attribute.png)

1. Op de **SAML-token kenmerken** dialoogvenster uitvoeren voor elke rij in de onderstaande tabel wordt weergegeven op de volgende stappen uit:
 
   | Naam kenmerk | Waarde kenmerk |
   | -------------- |  ------------ |
   | saml_token     | bd189cf6-1701-11e6-8f90-d26992eca2a5 |

   a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/betterworks-tutorial/tutorial_officespace_04.png)

    ![Eenmalige aanmelding configureren](./media/betterworks-tutorial/tutorial_officespace_05.png)

   b. In het tekstvak **Naam** typt u de naam van het kenmerk die voor die rij wordt weergegeven. 

   c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
   d. Klik op **OK**.

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/betterworks-tutorial/tutorial_general_400.png)

1. Het configureren van eenmalige aanmelding op **BetterWorks** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [BetterWorks ondersteuningsteam](mailto:support@betterworks.com).


> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/betterworks-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/betterworks-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/betterworks-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/betterworks-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-betterworks-test-user"></a>Het maken van een testgebruiker BetterWorks

In deze sectie maakt u een gebruiker met de naam van Britta Simon in BetterWorks. Werken met [BetterWorks ondersteuningsteam](mailto:support@betterworks.com) om toe te voegen de gebruikers in het BetterWorks-platform.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan BetterWorks.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan BetterWorks toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **BetterWorks**.

    ![Eenmalige aanmelding configureren](./media/betterworks-tutorial/tutorial_betterworks_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.

Wanneer u op de tegel BetterWorks in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing BetterWorks.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)


<!--Image references-->

[1]: ./media/betterworks-tutorial/tutorial_general_01.png
[2]: ./media/betterworks-tutorial/tutorial_general_02.png
[3]: ./media/betterworks-tutorial/tutorial_general_03.png
[4]: ./media/betterworks-tutorial/tutorial_general_04.png

[100]: ./media/betterworks-tutorial/tutorial_general_100.png

[200]: ./media/betterworks-tutorial/tutorial_general_200.png
[201]: ./media/betterworks-tutorial/tutorial_general_201.png
[202]: ./media/betterworks-tutorial/tutorial_general_202.png
[203]: ./media/betterworks-tutorial/tutorial_general_203.png

