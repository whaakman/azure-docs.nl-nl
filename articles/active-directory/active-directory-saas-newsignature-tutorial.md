---
title: 'Zelfstudie: Azure Active Directory-integratie met de beheerportal Cloud voor Microsoft Azure | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Cloud-Management-Portal voor Microsoft Azure.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4ea9f47c-25ca-42b0-a878-9e7aa6f34973
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 5eabff9399f35328feb973ad4a0abbf793fe7458
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cloud-management-portal-for-microsoft-azure"></a>Zelfstudie: Azure Active Directory-integratie met de beheerportal Cloud voor Microsoft Azure

In deze zelfstudie leert u hoe Cloud Management-Portal voor Microsoft Azure integreren met Azure Active Directory (Azure AD).

Cloud-Management-Portal voor Microsoft Azure integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang hebben tot Cloud-beheerportal voor Microsoft Azure
- U kunt uw gebruikers automatisch ophalen aangemeld bij de beheerportal Cloud voor Microsoft Azure (Single Sign-On) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met de beheerportal Cloud voor Microsoft Azure, moet u de volgende items:

- Een Azure AD-abonnement
- Een Cloud-Management-Portal voor Microsoft Azure-eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Cloud-Management-Portal voor Microsoft Azure uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-cloud-management-portal-for-microsoft-azure-from-the-gallery"></a>Cloud-Management-Portal voor Microsoft Azure uit de galerie toevoegen
Voor het configureren van de integratie van Cloud-Management-Portal voor Microsoft Azure met Azure AD, moet u Cloud-Management-Portal voor Microsoft Azure uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Cloud Management-Portal voor Microsoft Azure uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **Cloud Management-Portal voor Microsoft Azure**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_search.png)

5. Selecteer in het deelvenster resultaten **Cloud Management-Portal voor Microsoft Azure**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen Azure AD eenmalige aanmelding met de beheerportal Cloud voor Microsoft Azure op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in de beheerportal Cloud voor Microsoft Azure is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in de beheerportal Cloud voor Microsoft Azure tot stand worden gebracht.

Wijs in het beheerportal voor Microsoft Azure Cloud, de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met de beheerportal Cloud voor Microsoft Azure, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een Cloud-Management-Portal voor Microsoft Azure-testgebruiker](#creating-a-cloud-management-portal-for-microsoft-azure-test-user)**  - Cloud-Management-Portal voor Microsoft Azure die is gekoppeld aan de Azure AD-weergave van de gebruiker van een exemplaar van Britta Simon bevatten.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie maakt u Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw Cloud-Management-Portal voor Microsoft Azure-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met de beheerportal Cloud voor Microsoft Azure, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Cloud Management-Portal voor Microsoft Azure** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_samlbase.png)

3. Op de **Cloud Management-Portal voor Microsoft Azure-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_url.png)

    a. In de **aanmeldings-URL** textbox, typ een URL met de volgende patronen: 
    
    | |
    |--|
    | `https://portal.newsignature.com/<instancename>` |   
    | `https://portal.igcm.com/<instancename>` |
    
    b. In de **id** textbox, typ een URL met de volgende patronen: 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com` |
    | `https://<subdomain>.newsignature.com` |

    c. In de **antwoord-URL** textbox, typ een URL met de volgende patronen: 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com/<instancename>` |
    | `https://<subdomain>.newsignature.com` |
    | `https://<subdomain>.newsignature.com/<instancename>` |

    > [!NOTE] 
    > Deze waarden zijn niet echt. Deze waarden bijwerken met de werkelijke aanmeldings-URL, -id en antwoord-URL. Neem contact op met [Cloud Management-Portal voor Microsoft Azure Client ondersteuningsteam](mailto:jczernuszka@newsignature.com) ophalen van deze waarden. 
 
4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Certificate(Base64)** en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_certificate.png) 

5. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-newsignature-tutorial/tutorial_general_400.png)

6. Op de **Cloud Management-Portal voor Microsoft Azure Configuration** sectie, klikt u op **Cloud-beheerportal configureren voor Microsoft Azure** openen **eenmalige aanmelding configureren** venster. Kopieer de **Sign-Out-URL, SAML entiteit-ID en SAML Single Sign-On Service-URL** van de **Naslaggids punt.**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_configure.png) 

7. Eenmalige aanmelding configureren op **Cloud Management-Portal voor Microsoft Azure** zijde, moet u de gedownloade verzenden **certificaat**, **Sign-Out URL**, **SAML Single Sign-On Service-URL** en **SAML entiteit-ID** naar [Cloud Management-Portal voor Microsoft Azure-ondersteuningsteam](mailto:jczernuszka@newsignature.com). Ze deze instelling zodat de SAML SSO-verbinding juist is ingesteld op beide zijden ingesteld.

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-newsignature-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-newsignature-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-newsignature-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-newsignature-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-cloud-management-portal-for-microsoft-azure-test-user"></a>Een Cloud-Management-Portal voor Microsoft Azure-testgebruiker maken

Het doel van deze sectie is het maken van een gebruiker Britta Simon aangeroepen in de beheerportal Cloud voor Microsoft Azure. Neem contact op met [Cloud Management-Portal voor Microsoft Azure-ondersteuningsteam](mailto:jczernuszka@newsignature.com) de gebruikers in de Cloud-Management-Portal voor Microsoft Azure-account toevoegen.


### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon gebruikt Azure eenmalige aanmelding toegang verlenen aan Cloud Management-Portal voor Microsoft Azure.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon Cloud Management Portal voor Microsoft Azure, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Cloud Management-Portal voor Microsoft Azure**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is het testen van uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.
Als u op de Cloud-beheerportal voor Microsoft Azure-tegel in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw Cloud-Management-Portal voor Microsoft Azure-toepassing.

Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_203.png

