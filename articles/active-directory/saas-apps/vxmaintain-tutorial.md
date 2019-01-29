---
title: 'Zelfstudie: Azure Active Directory-integratie met vxMaintain | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en vxMaintain.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 841a1066-593c-4603-9abe-f48496d73d10
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: d4dba956ecd88683b124d4faf997a5fde6603727
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55184769"
---
# <a name="tutorial-azure-active-directory-integration-with-vxmaintain"></a>Zelfstudie: Azure Active Directory-integratie met vxMaintain

In deze zelfstudie leert u hoe u vxMaintain integreren met Azure Active Directory (Azure AD).

Deze integratie biedt verschillende belangrijke voordelen. U kunt:

- Beheren in Azure AD die toegang tot vxMaintain heeft.
- Kunnen uw gebruikers automatisch aan te melden vxMaintain met eenmalige aanmelding (SSO) met behulp van hun Azure AD-accounts.
- Uw accounts in één centrale locatie beheren: de Azure-portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met vxMaintain, moet u de volgende items:

- Een Azure AD-abonnement
- Een vxMaintain abonnement SSO is ingeschakeld

> [!NOTE]
> Wanneer u de stappen in deze zelfstudie hebt getest, wordt u aangeraden dat u niet een productie-omgeving gebruiken.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u de Azure AD eenmalige aanmelding in een testomgeving. 

Het scenario dat geeft een overzicht van deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

* VxMaintain uit de galerie toe te voegen
* Configureren en testen van Azure AD eenmalige aanmelding

## <a name="add-vxmaintain-from-the-gallery"></a>VxMaintain uit de galerie toevoegen
Voor het configureren van de integratie van vxMaintain met Azure AD, moet u vxMaintain uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

Als u wilt toevoegen vxMaintain uit de galerie, het volgende doen:

1. In de [Azure-portal](https://portal.azure.com), selecteer in het linkerdeelvenster de **Azure Active Directory** knop. 

    ![De knop Azure Active Directory][1]

1. Selecteer **bedrijfstoepassingen** > **alle toepassingen**.

    ![Het deelvenster 'Enterprise Application'][2]
    
1. Toevoegen van een toepassing, in de **alle toepassingen** in het dialoogvenster, selecteer **nieuwe toepassing**.

    ![De 'nieuwe toepassing"knop][3]

1. Typ in het zoekvak **vxMaintain**.

    ![De vervolgkeuzelijst 'Single Sign-on modus'](./media/vxmaintain-tutorial/tutorial_vxmaintain_search.png)

1. Selecteer in de lijst met resultaten **vxMaintain**, en selecteer vervolgens **toevoegen**.

    ![De koppeling vxMaintain](./media/vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen
In deze sectie kunt u configureren en testen van Azure AD-eenmalige aanmelding met behulp van vxMaintain, op basis van een testgebruiker met de naam "Britta Simon."

Azure AD moet weten van de vxMaintain equivalent aan de Azure AD-gebruiker voor eenmalige aanmelding om te werken. Dat wil zeggen, moet u een koppeling relatie tussen de Azure AD-gebruiker en de bijbehorende vxMaintain-gebruiker maken.

Voor het maken van de relatie van de koppeling, wijzen de vxMaintain **gebruikersnaam** waarde als de Azure AD **gebruikersnaam** waarde.

Als u wilt configureren en Azure AD-eenmalige aanmelding testen met behulp van vxMaintain, voer de volgende bouwstenen.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

In deze sectie kunt u zowel Azure AD-eenmalige aanmelding inschakelen in Azure portal en het configureren van eenmalige aanmelding in uw toepassing vxMaintain door het volgende te doen:

1. In de Azure-portal op de **vxMaintain** toepassing integratie weergeeft, schakelt **eenmalige aanmelding**.

    ![De opdracht "Single sign-on"][4]

1. Voor het inschakelen van eenmalige aanmelding de **modus voor één aanmelding** vervolgkeuzelijst, selecteer **SAML gebaseerde aanmelding**.
 
    ![De opdracht 'SAML gebaseerde aanmelding'](./media/vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

1. Onder **vxMaintain domein en URL's**, doet u het volgende:

    ![De vxMaintain sectie domein en URL 's](./media/vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    a. In de **id** typt u een URL die heeft de volgende syntaxis: `https://<company name>.verisae.com`

    b. In de **antwoord-URL** typt u een URL die heeft de volgende syntaxis: `https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`

    > [!NOTE] 
    > De bovenstaande waarden zijn niet echt. Deze bijwerken met de werkelijke-id en antwoord-URL. De waarden, neem contact op met de [vxMaintain ondersteuningsteam](https://www.hubspot.com/company/contact).
 
1. Onder **SAML-handtekeningcertificaat**, selecteer **Metadata XML**, en sla het bestand met metagegevens op uw computer.

    ![De sectie "SAML-handtekeningcertificaat"](./media/vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

1. Selecteer **Opslaan**.

    ![De knop Opslaan](./media/vxmaintain-tutorial/tutorial_general_400.png)

1. Het configureren van **vxMaintain** SSO, verzendt de gedownloade **Metadata XML** van het bestand in de [vxMaintain ondersteuningsteam](https://www.hubspot.com/company/contact).

> [!TIP]
> Als u de app hebt ingesteld, vindt u een beknopte versie van de voorgaande instructies in de [Azure-portal](https://portal.azure.com). Nadat u de app vanuit de **Active Directory** > **bedrijfstoepassingen** sectie, selecteer de **Single Sign-On** tabblad, en vervolgens toegang tot de ingesloten documentatie van de **configuratie** sectie. 
>
>Zie voor meer informatie over de documentatie voor embedded-functie, [beheren van eenmalige aanmelding voor zakelijke apps](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
In deze sectie maakt maken u testgebruiker Britta Simon in Azure portal door het volgende te doen:

![De Azure AD-testgebruiker][100]

1. In de **Azure-portal**, selecteer in het linkerdeelvenster de **Azure Active Directory** knop.

    ![De knop 'Azure Active Directory'](./media/vxmaintain-tutorial/create_aaduser_01.png) 

1. Voor een lijst van gebruikers, gaat u naar **gebruikers en groepen** > **alle gebruikers**.
    
    ![De koppeling 'Alle gebruikers'](./media/vxmaintain-tutorial/create_aaduser_02.png)  
    De **alle gebruikers** in het dialoogvenster wordt geopend. 

1. Om te openen de **gebruiker** in het dialoogvenster, selecteer **toevoegen**.
 
    ![De knop toevoegen](./media/vxmaintain-tutorial/create_aaduser_03.png) 

1. In de **gebruiker** dialoogvenster de volgende handelingen uit:
 
    ![Het dialoogvenster Gebruiker](./media/vxmaintain-tutorial/create_aaduser_04.png) 

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van de testgebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die is gegenereerd in de **wachtwoord** vak.

    d. Selecteer **Maken**.
 
### <a name="create-a-vxmaintain-test-user"></a>Maak een testgebruiker vxMaintain

In deze sectie maakt u testgebruiker Britta Simon vxMaintain. Als gebruikers wilt toevoegen in het platform vxMaintain, werken met de [vxMaintain ondersteuningsteam](https://www.hubspot.com/company/contact). Voordat u eenmalige aanmelding gebruiken, maken en activeren van de gebruikers.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u testgebruiker Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot vxMaintain. Om dit te doen, het volgende doen:

![Testgebruiker in de lijst weergavenaam][200] 

1. In de Azure-portal **toepassingen** weergeven, gaat u naar **Directory** weergave > **bedrijfstoepassingen** > **alle toepassingen**.

    ![De koppeling 'Alle Application'][201] 

1. In de **toepassingen** in de lijst met **vxMaintain**.

    ![De koppeling vxMaintain](./media/vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

1. Selecteer in het linkerdeelvenster **gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen][202] 

1. Selecteer **toevoegen** en klikt u op de **toevoegen toewijzing** venster **gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen][203]

1. In de **gebruikers en groepen** in het dialoogvenster de **gebruikers** in de lijst met **Britta Simon**, en selecteer vervolgens de **Selecteer** knop.

1. In de **toevoegen toewijzing** in het dialoogvenster, selecteer **toewijzen**.
    
### <a name="test-your-azure-ad-single-sign-on"></a>Uw Azure AD eenmalige aanmelding testen

In deze sectie maakt testen u de configuratie van uw Azure AD-eenmalige aanmelding met behulp van het toegangsvenster.

Selecteren van de **vxMaintain** tegel in het toegangsvenster moet aanmelden bij uw toepassing vxMaintain automatisch.

Zie voor meer informatie over het toegangsvenster, [Inleiding tot het toegangsvenster](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="next-steps"></a>Volgende stappen

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/vxmaintain-tutorial/tutorial_general_01.png
[2]: ./media/vxmaintain-tutorial/tutorial_general_02.png
[3]: ./media/vxmaintain-tutorial/tutorial_general_03.png
[4]: ./media/vxmaintain-tutorial/tutorial_general_04.png

[100]: ./media/vxmaintain-tutorial/tutorial_general_100.png

[200]: ./media/vxmaintain-tutorial/tutorial_general_200.png
[201]: ./media/vxmaintain-tutorial/tutorial_general_201.png
[202]: ./media/vxmaintain-tutorial/tutorial_general_202.png
[203]: ./media/vxmaintain-tutorial/tutorial_general_203.png

