---
title: 'Zelfstudie: Azure Active Directory-integratie met de beheerportal Cloud voor Microsoft Azure | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Cloud Management Portal voor Microsoft Azure.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4ea9f47c-25ca-42b0-a878-9e7aa6f34973
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 8ea156f09f79d2e2718b0aeb1a6a9e870828c32d
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051833"
---
# <a name="tutorial-azure-active-directory-integration-with-cloud-management-portal-for-microsoft-azure"></a>Zelfstudie: Azure Active Directory-integratie met de beheerportal Cloud voor Microsoft Azure

In deze zelfstudie leert u hoe u Cloud Management Portal voor Microsoft Azure integreert met Azure Active Directory (Azure AD).

Cloud Management Portal voor Microsoft Azure integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang hebben tot Cloud Management Portal voor Microsoft Azure
- U kunt uw gebruikers automatisch ophalen aangemeld bij de beheerportal Cloud voor Microsoft Azure (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met de beheerportal Cloud voor Microsoft Azure, moet u de volgende items:

- Een Azure AD-abonnement
- Een Cloud-Management-Portal voor Microsoft Azure eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, krijgt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Cloud Management Portal voor Microsoft Azure uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-cloud-management-portal-for-microsoft-azure-from-the-gallery"></a>Cloud Management Portal voor Microsoft Azure uit de galerie toe te voegen
Voor het configureren van de integratie van Cloud Management Portal voor Microsoft Azure in Azure AD, moet u Cloud Management Portal voor Microsoft Azure uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Cloud Management Portal voor Microsoft Azure uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Cloud Management Portal voor Microsoft Azure**.

    ![Het maken van een Azure AD-testgebruiker](./media/newsignature-tutorial/tutorial_newsignature_search.png)

5. Selecteer in het deelvenster resultaten **Cloud Management Portal voor Microsoft Azure**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/newsignature-tutorial/tutorial_newsignature_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en Azure AD eenmalige aanmelding testen met Cloud-beheerportal voor Microsoft Azure op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent in de Cloud Management Portal voor Microsoft Azure is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de Cloud Management Portal voor Microsoft Azure tot stand worden gebracht.

In de Cloud Management Portal voor Microsoft Azure, wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Als u wilt configureren en Azure AD eenmalige aanmelding testen met Cloud-beheerportal voor Microsoft Azure, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Het maken van een Cloud Management Portal voor Microsoft Azure-testgebruiker](#creating-a-cloud-management-portal-for-microsoft-azure-test-user)**  : als u wilt een equivalent van Britta Simon hebben in de Cloud Management Portal voor Microsoft Azure die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u schakelt Azure AD eenmalige aanmelding in de Azure-portal en configureren van eenmalige aanmelding in uw Cloud-Management-Portal voor Microsoft Azure-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met de beheerportal Cloud voor Microsoft Azure, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Cloud Management Portal voor Microsoft Azure** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/newsignature-tutorial/tutorial_newsignature_samlbase.png)

3. Op de **Cloud Management Portal voor Microsoft Azure-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/newsignature-tutorial/tutorial_newsignature_url.png)

    a. In de **aanmeldings-URL** tekstvak, een URL met behulp van de volgende patronen: 
    
    | |
    |--|
    | `https://portal.newsignature.com/<instancename>` |   
    | `https://portal.igcm.com/<instancename>` |
    
    b. In de **id** tekstvak, een URL met behulp van de volgende patronen: 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com` |
    | `https://<subdomain>.newsignature.com` |

    c. In de **antwoord-URL** tekstvak, een URL met behulp van de volgende patronen: 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com/<instancename>` |
    | `https://<subdomain>.newsignature.com` |
    | `https://<subdomain>.newsignature.com/<instancename>` |

    > [!NOTE] 
    > Deze waarden zijn niet echt. Werk deze waarden met de werkelijke aanmeldings-URL, id en antwoord-URL. Neem contact op met [Cloud Management Portal voor Microsoft Azure-Client-ondersteuningsteam](mailto:jczernuszka@newsignature.com) om deze waarden te verkrijgen. 
 
4. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/newsignature-tutorial/tutorial_newsignature_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/newsignature-tutorial/tutorial_general_400.png)

6. Op de **Cloud Management Portal voor Microsoft Azure-configuratie** sectie, klikt u op **Cloud Management-Portal configureren voor Microsoft Azure** openen **aanmelding configureren** het venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/newsignature-tutorial/tutorial_newsignature_configure.png) 

7. Het configureren van eenmalige aanmelding op **Cloud Management Portal voor Microsoft Azure** zijde, moet u voor het verzenden van de gedownloade **certificaat**, **afmelding URL**,  **Single Sign-On Service URL voor SAML** en **SAML entiteit-ID** naar [Cloud Management Portal voor Microsoft Azure-ondersteuningsteam](mailto:jczernuszka@newsignature.com). Ze stelt u deze optie om de SAML SSO-verbinding instellen goed aan beide zijden.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl het instellen van de app!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de  **Configuratie** sectie aan de onderkant. U kunt meer lezen over de documentatie voor embedded-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is het maken van een testgebruiker in Azure portal Britta Simon genoemd.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/newsignature-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/newsignature-tutorial/create_aaduser_02.png) 

3. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/newsignature-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/newsignature-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-cloud-management-portal-for-microsoft-azure-test-user"></a>Het maken van een Cloud-beheerportal voor de testgebruiker Microsoft Azure

Het doel van deze sectie is het maken van een gebruiker met de naam van Britta Simon in Cloud Management Portal voor Microsoft Azure. Neem contact op met [Cloud Management Portal voor Microsoft Azure-ondersteuningsteam](mailto:jczernuszka@newsignature.com) om toe te voegen de gebruikers in de Cloud Management Portal voor Microsoft Azure-account.


### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen voor Cloud Management Portal voor Microsoft Azure.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon voor Cloud Management Portal voor Microsoft Azure, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Cloud Management Portal voor Microsoft Azure**.

    ![Eenmalige aanmelding configureren](./media/newsignature-tutorial/tutorial_newsignature_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie via het toegangsvenster.
Wanneer u op de Cloud Management Portal voor Microsoft Azure-tegel in het toegangsvenster, u moet u automatisch aangemeld bij uw Cloud-Management-Portal voor Microsoft Azure-toepassing.

Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/newsignature-tutorial/tutorial_general_01.png
[2]: ./media/newsignature-tutorial/tutorial_general_02.png
[3]: ./media/newsignature-tutorial/tutorial_general_03.png
[4]: ./media/newsignature-tutorial/tutorial_general_04.png

[100]: ./media/newsignature-tutorial/tutorial_general_100.png

[200]: ./media/newsignature-tutorial/tutorial_general_200.png
[201]: ./media/newsignature-tutorial/tutorial_general_201.png
[202]: ./media/newsignature-tutorial/tutorial_general_202.png
[203]: ./media/newsignature-tutorial/tutorial_general_203.png

