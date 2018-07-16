---
title: 'Zelfstudie: Azure Active Directory-integratie met BenefitHub | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en BenefitHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4069fe32-a452-463f-973e-7aa0baa4c2fa
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 64019237370d867b2f9a35a59139b5c4cc719ffa
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054162"
---
# <a name="tutorial-azure-active-directory-integration-with-benefithub"></a>Zelfstudie: Azure Active Directory-integratie met BenefitHub

In deze zelfstudie leert u hoe u BenefitHub integreren met Azure Active Directory (Azure AD).

BenefitHub integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot BenefitHub heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij BenefitHub (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met BenefitHub, moet u de volgende items:

- Een Azure AD-abonnement
- Een BenefitHub eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. BenefitHub uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-benefithub-from-the-gallery"></a>BenefitHub uit de galerie toe te voegen
Voor het configureren van de integratie van BenefitHub in Azure AD, moet u BenefitHub uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen BenefitHub uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **BenefitHub**.

    ![Het maken van een Azure AD-testgebruiker](./media/benefithub-tutorial/tutorial_benefithub_search.png)

5. Selecteer in het deelvenster resultaten **BenefitHub**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/benefithub-tutorial/tutorial_benefithub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met BenefitHub op basis van een testgebruiker met de naam "Britta Simon."

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in BenefitHub is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in BenefitHub tot stand worden gebracht.

In BenefitHub, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met BenefitHub, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een testgebruiker BenefitHub](#creating-a-benefithub-test-user)**  : als u wilt een equivalent van Britta Simon in BenefitHub die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw toepassing BenefitHub.

**Voor het configureren van Azure AD eenmalige aanmelding met BenefitHub, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **BenefitHub** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/benefithub-tutorial/tutorial_benefithub_samlbase.png)

3. Op de **BenefitHub domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/benefithub-tutorial/tutorial_benefithub_url1.png)
  
    a. In de **id** tekstvak, type: `urn:benefithub:passport`
    
    b. In de **antwoord-URL** tekstvak, type: `https://passport.benefithub.info/saml/post/ac`

4. De toepassing BenefitHub wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. Configureer de volgende claims voor deze toepassing. U kunt de waarden van deze kenmerken vanuit beheren de "**gebruikerskenmerken**" sectie op de pagina van de toepassing-integratie. 

    ![Eenmalige aanmelding configureren](./media/benefithub-tutorial/tutorial_benefithub_attribute.png)

5. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de voorgaande afbeelding en voer de volgende stappen uit:
    
    | Naam kenmerk | Waarde kenmerk |
    | ------------------- | -------------------- |    
    | OrganizationId | < organizationid > |

    > [!NOTE]
    > De waarde van dit kenmerk is niet echt. Deze waarde met de werkelijke organizationid bijwerken. Neem contact op met [BenefitHub ondersteuningsteam](https://www.benefithub.com/Home/ContactUs) om op te halen van de werkelijke organizationid.
    
    a. Klik op **kenmerk toevoegen** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/benefithub-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/benefithub-tutorial/tutorial_attribute_05.png)

    b. In de **naam** tekstvak typt u de naam van het kenmerk wordt weergegeven voor die rij.
    
    c. Uit de **waarde** weergeven, typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

    > [!NOTE] 
    > Voordat u de SAML-verklaring configureren kunt, moet u contact op met uw [BenefitHub ondersteuning](https://www.benefithub.com/Home/ContactUs) en vraagt u de waarde van het kenmerk unieke id voor uw tenant. U moet deze waarde voor het configureren van de aangepaste claim voor uw toepassing.

6. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Metadata XML** en sla het bestand met metagegevens op uw computer.

    ![Eenmalige aanmelding configureren](./media/benefithub-tutorial/tutorial_benefithub_certificate.png) 

7. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/benefithub-tutorial/tutorial_general_400.png)

8. Het configureren van eenmalige aanmelding op **BenefitHub** zijde, moet u voor het verzenden van de gedownloade **Metadata XML** naar [BenefitHub ondersteuningsteam](https://www.benefithub.com/Home/ContactUs). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/benefithub-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/benefithub-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/benefithub-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/benefithub-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-benefithub-test-user"></a>Het maken van een testgebruiker BenefitHub

In deze sectie maakt u een gebruiker met de naam van Britta Simon in BenefitHub. Werken met [BenefitHub ondersteuningsteam](https://www.benefithub.com/Home/ContactUs) om toe te voegen de gebruikers in het BenefitHub-platform. Gebruikers moeten worden gemaakt en worden geactiveerd voordat u eenmalige aanmelding gebruiken. 

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie maakt inschakelen u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan BenefitHub.

![Gebruiker toewijzen][200] 

**Als u wilt Britta Simon aan BenefitHub toewijst, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **BenefitHub**.

    ![Eenmalige aanmelding configureren](./media/benefithub-tutorial/tutorial_benefithub_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel BenefitHub in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing BenefitHub.
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/benefithub-tutorial/tutorial_general_01.png
[2]: ./media/benefithub-tutorial/tutorial_general_02.png
[3]: ./media/benefithub-tutorial/tutorial_general_03.png
[4]: ./media/benefithub-tutorial/tutorial_general_04.png

[100]: ./media/benefithub-tutorial/tutorial_general_100.png

[200]: ./media/benefithub-tutorial/tutorial_general_200.png
[201]: ./media/benefithub-tutorial/tutorial_general_201.png
[202]: ./media/benefithub-tutorial/tutorial_general_202.png
[203]: ./media/benefithub-tutorial/tutorial_general_203.png
