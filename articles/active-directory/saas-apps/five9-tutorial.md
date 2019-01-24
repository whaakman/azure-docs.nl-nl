---
title: 'Zelfstudie: Azure Active Directory-integratie met Five9 Plus-Adapter (CTI, neem contact op met Center-Agents) | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Five9 Plus Adapter (CTI, neem contact op met Center-Agents).
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: 7b31cbbf639e5835a3a46063b4b15d67915fb3d4
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54816667"
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Zelfstudie: Azure Active Directory-integratie met Five9 Plus-Adapter (CTI, neem contact op met Center-Agents)

In deze zelfstudie leert u hoe u Five9 Plus Adapter (CTI, neem contact op met Center-Agents) integreren met Azure Active Directory (Azure AD).

Five9 Plus Adapter (CTI, neem contact op met Center-Agents) integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot Five9 Plus Adapter (CTI, neem contact op met Center-Agents heeft)
- U kunt uw gebruikers automatisch ophalen aangemeld bij Five9 Plus Adapter (CTI, neem contact op met Center-Agents) inschakelen (Single Sign-On) met hun Azure AD-accounts
- U kunt uw accounts in één centrale locatie - Azure portal beheren

Als u wilt graag meer informatie over de integratie van de SaaS-app met Azure AD, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Five9 Plus-Adapter (CTI, neem contact op met Center-Agents), moet u de volgende items:

- Een Azure AD-abonnement
- Een Five9 Plus Adapter (CTI, neem contact op met Center-Agents) eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u niet beschikt over een evaluatieomgeving in Azure AD, kunt u hier een gratis proefversie van één maand krijgen: [Proefversie](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Five9 Plus Adapter (CTI, neem contact op met Center-Agents) uit de galerie toevoegen
1. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a>Five9 Plus Adapter (CTI, neem contact op met Center-Agents) uit de galerie toevoegen
Als u wilt de integratie van Five9 Plus Adapter (CTI, neem contact op met Center-Agents) in Azure AD configureert, moet u Five9 Plus netwerkadapter (CTI, neem contact op met Center-Agents) worden toegevoegd vanuit de galerie aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Five9 Plus Adapter (CTI, neem contact op met Center-Agents) uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de **[Azure-portal](https://portal.azure.com)**, klik in het navigatievenster aan de linkerkant op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

1. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Applicaties][2]
    
1. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![Applicaties][3]

1. Typ in het zoekvak **Five9 Plus Adapter (CTI, neem contact op met Center-Agents)**.

    ![Het maken van een Azure AD-testgebruiker](./media/five9-tutorial/tutorial_five9_search.png)

1. Selecteer in het deelvenster resultaten **Five9 Plus Adapter (CTI, neem contact op met Center-Agents)**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Het maken van een Azure AD-testgebruiker](./media/five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie maakt u configureert en test Azure AD eenmalige aanmelding met Five9 Plus-Adapter (CTI, neem contact op met Center-Agents) op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, moet Azure AD om te weten wat de gebruiker equivalent Five9 Plus adapter (CTI, neem contact op met Center-Agents) is aan een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker Five9 Plus adapter (CTI, neem contact op met Center-Agents) tot stand worden gebracht.

In Five9 Plus-Adapter (CTI, neem contact op met Center-Agents), wijs de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** de relatie van de koppeling tot stand brengen.

Om te configureren en testen van Azure AD eenmalige aanmelding met Five9 Plus-Adapter (CTI, neem contact op met Center-Agents), moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD eenmalige aanmelding](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
1. **[Het maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
1. **[Het maken van een testgebruiker Five9 Plus Adapter (CTI, neem contact op met Center-Agents)](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)**  : als u wilt een equivalent van Britta Simon in Five9 Plus-Adapter (CTI, neem contact op met Center-Agents) die is gekoppeld aan de Azure AD-weergave van de gebruiker hebben.
1. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Eenmalige aanmelding testen](#testing-single-sign-on)**  : als u wilt controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD eenmalige aanmelding configureren

In deze sectie maakt u Azure AD eenmalige aanmelding in de Azure-portal inschakelen en configureren van eenmalige aanmelding in uw toepassing Five9 Plus Adapter (CTI, neem contact op met Center-Agents).

**Voor het configureren van Azure AD eenmalige aanmelding met Five9 Plus-Adapter (CTI, neem contact op met Center-Agents), moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **Five9 Plus Adapter (CTI, neem contact op met Center-Agents)** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

1. Op de **eenmalige aanmelding** dialoogvenster, selecteer **modus** als **SAML gebaseerde aanmelding** eenmalige aanmelding inschakelen.
 
    ![Eenmalige aanmelding configureren](./media/five9-tutorial/tutorial_five9_samlbase.png)

1. Op de **Five9 Plus Adapter (CTI, neem contact op met Center-Agents)-domein en URL's** sectie, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/five9-tutorial/tutorial_five9_url.png)
    
    a. In het tekstvak **Id** typt u een URL met de volgende patronen:

    |    Omgeving      |       URL      |
    | :-- | :-- |
    | Voor 'Five9 Plus -Adapter voor Microsoft Dynamics CRM' | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | Voor "Five9 Plus -Adapter voor Zendesk" | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | Voor "Five9 Plus -Adapter voor Agent bureaublad Toolkit" | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. In het tekstvak **Antwoord-URL** typt u een URL met de volgende notatie:

    |      Omgeving     |      URL      |
    | :--                  | :--           |
    | Voor 'Five9 Plus -Adapter voor Microsoft Dynamics CRM' | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | Voor "Five9 Plus -Adapter voor Zendesk" | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | Voor "Five9 Plus -Adapter voor Agent bureaublad Toolkit" | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

1. Op de **SAML-handtekeningcertificaat** sectie, klikt u op **Certificate(Base64)** en slaat u het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/five9-tutorial/tutorial_five9_certificate.png) 

1. Klik op de knop **Save**.

    ![Eenmalige aanmelding configureren](./media/five9-tutorial/tutorial_general_400.png)

1. Op de **Five9 Plus (CTI, neem contact op met Center-Agents) adapterconfiguratie** sectie, klikt u op **configureren Five9 Plus-Adapter (CTI, neem contact op met Center-Agents)** openen **aanmeldingconfigureren**venster. Kopiëren de **afmelding-URL, SAML-entiteit-ID en Single Sign-On Service URL voor SAML-** uit de **Naslaggids sectie.**

    ![Eenmalige aanmelding configureren](./media/five9-tutorial/tutorial_five9_configure.png) 

1. Het configureren van eenmalige aanmelding op **Five9 Plus Adapter (CTI, neem contact op met Center-Agents)** zijde, moet u voor het verzenden van de gedownloade **Certificate(Base64), de URL van de afmelding, SAML entiteit-ID en Single Sign-On Service URL voor SAML-** naar [Five9 Plus (CTI, neem contact op met Center-Agents) ondersteuning adapterteam](https://www.five9.com/about/contact). Bovendien ook voor het configureren van eenmalige aanmelding verder Volg de onderstaande stappen op basis van de adapter:

    a. 'Five9 Plus -Adapter voor bureaublad Toolkit Agent' beheerdershandleiding voor de: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. 'Five9 Plus -Adapter voor Microsoft Dynamics CRM' beheerdershandleiding voor de: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. "Five9 Plus -Adapter voor Zendesk" beheerdershandleiding voor de: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)


> [!TIP]
> U kunt nu een beknopte versie van deze instructies in [Azure Portal](https://portal.azure.com) lezen terwijl u de app instelt!  Klik nadat u deze app onder **Active Directory > Bedrijfstoepassingen** hebt toegevoegd op het tabblad **Eenmalige aanmelding** en open de ingesloten documentatie via het gedeelte **Configuratie** onderaan. Hier leest u meer over de functie voor ingesloten documentatie: [Ingesloten documentatie in Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Het maken van een Azure AD-testgebruiker
Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Het maken van een Azure AD-testgebruiker](./media/five9-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klikt u op **alle gebruikers**.
    
    ![Het maken van een Azure AD-testgebruiker](./media/five9-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Het maken van een Azure AD-testgebruiker](./media/five9-tutorial/create_aaduser_03.png) 

1. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Het maken van een Azure AD-testgebruiker](./media/five9-tutorial/create_aaduser_04.png) 

    a. In de **naam** tekstvak, type **BrittaSimon**.

    b. In de **gebruikersnaam** tekstvak, type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Het maken van een testgebruiker Five9 Plus Adapter (CTI, neem contact op met Center-Agents)

In deze sectie maakt u een gebruiker met de naam van Britta Simon Five9 Plus adapter (CTI, neem contact op met Center-Agents). Werken met [Five9 Plus (CTI, neem contact op met Center-Agents) ondersteuning adapterteam](https://www.five9.com/about/contact) om toe te voegen de gebruikers in het platform Five9 Plus Adapter (CTI, neem contact op met Center-Agents). Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker van de test Azure AD

In deze sectie schakelt u Britta Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Five9 Plus Adapter (CTI, neem contact op met Center-Agents).

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon Five9 Plus adapter (CTI, neem contact op met Center-Agents), moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de mapweergave en Ga naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

1. Selecteer in de lijst met toepassingen, **Five9 Plus Adapter (CTI, neem contact op met Center-Agents)**.

    ![Eenmalige aanmelding configureren](./media/five9-tutorial/tutorial_five9_app.png) 

1. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

1. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

1. Op **gebruikers en groepen** dialoogvenster, selecteer **Britta Simon** in de lijst gebruikers.

1. Klik op **Selecteer** op knop **gebruikers en groepen** dialoogvenster.

1. Klik op **toewijzen** op knop **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster.

Wanneer u op de tegel Five9 Plus Adapter (CTI, neem contact op met Center-Agents) in het toegangsvenster, u moet u automatisch aangemeld bij uw toepassing Five9 Plus Adapter (CTI, neem contact op met Center-Agents).
Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/five9-tutorial/tutorial_general_01.png
[2]: ./media/five9-tutorial/tutorial_general_02.png
[3]: ./media/five9-tutorial/tutorial_general_03.png
[4]: ./media/five9-tutorial/tutorial_general_04.png

[100]: ./media/five9-tutorial/tutorial_general_100.png

[200]: ./media/five9-tutorial/tutorial_general_200.png
[201]: ./media/five9-tutorial/tutorial_general_201.png
[202]: ./media/five9-tutorial/tutorial_general_202.png
[203]: ./media/five9-tutorial/tutorial_general_203.png

