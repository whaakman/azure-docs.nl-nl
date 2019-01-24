---
title: 'Zelfstudie: Azure Active Directory-integratie met IBM Kenexa enquête Enterprise | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en IBM Kenexa enquête Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: d8027aca628185b1e3d2e80323c9f83e6832a913
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54823926"
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met IBM Kenexa enquête Enterprise

In deze zelfstudie leert u hoe u IBM Kenexa enquête Enterprise integreren met Azure Active Directory (Azure AD).

IBM Kenexa enquête Enterprise integreren met Azure AD biedt u de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot IBM Kenexa enquête Enterprise heeft.
- U kunt uw gebruikers automatisch aanmelden bij IBM Kenexa enquête Enterprise met behulp van eenmalige aanmelding (SSO) met hun Azure AD-accounts inschakelen.
- U kunt uw accounts in één centrale locatie kunt beheren: de Azure-portal.

Als u meer weten over de software als een service (SaaS)-app-integratie met Azure AD wilt, Zie [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met IBM Kenexa enquête Enterprise, moet u de volgende items:

- Een Azure AD-abonnement
- Een abonnement IBM Kenexa enquête Enterprise SSO is ingeschakeld

> [!NOTE]
> Wanneer u de stappen in deze zelfstudie hebt getest, wordt u aangeraden dat u niet een productie-omgeving gebruiken.

Volg deze aanbevelingen als u de stappen in deze zelfstudie wilt testen:

- Gebruik niet de productieomgeving, tenzij dit echt nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie, kunt u Azure AD-eenmalige aanmelding testen in een testomgeving. Het scenario wordt beschreven in de zelfstudie bestaat uit twee belangrijkste bouwstenen:

* IBM Kenexa enquête Enterprise uit de galerie toe te voegen
* Configureren en testen van Azure AD-eenmalige aanmelding

## <a name="add-ibm-kenexa-survey-enterprise-from-the-gallery"></a>IBM Kenexa enquête Enterprise uit de galerie toevoegen
Voor het configureren van de integratie van IBM Kenexa enquête Enterprise in Azure AD, IBM Kenexa enquête Enterprise uit de galerie te toevoegt aan uw lijst met beheerde SaaS-apps.

Als u wilt toevoegen IBM Kenexa enquête Enterprise uit de galerie, het volgende doen:

1. In de [Azure-portal](https://portal.azure.com), in het linkerdeelvenster klikt u op de **Azure Active Directory** knop. 

    ![De knop Azure Active Directory][1]

1. Selecteer **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.

    ![De blade Bedrijfstoepassingen][2]
    
1. Als u wilt een toepassing hebt toegevoegd, klikt u op de **nieuwe toepassing** knop.

    ![De knop Nieuwe toepassing][3]

1. Typ in het zoekvak **IBM Kenexa enquête Enterprise**.

    ![Het maken van een Azure AD-testgebruiker](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_search.png)

1. Selecteer in de lijst met resultaten **IBM Kenexa enquête Enterprise**, en klik vervolgens op de **toevoegen** om toe te voegen van de toepassing.

    ![IBM Kenexa enquête onderneming in de lijst met resultaten](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen
In deze sectie kunt u configureren en testen van Azure AD-eenmalige aanmelding met IBM Kenexa enquête Enterprise op basis van een testgebruiker met de naam "Britta Simon."

Azure AD moet het equivalent van de gebruiker IBM Kenexa enquête Enterprise identificatie in Azure AD voor eenmalige aanmelding om te werken. Met andere woorden, Azure AD moet een relatie tot stand koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in IBM Kenexa enquête onderneming.

Voor het maken van de relatie van de koppeling, wijs de waarde van de **gebruikersnaam** in IBM Kenexa enquête Enterprise als de waarde van de **gebruikersnaam** in Azure AD.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met IBM Kenexa enquête Enterprise, voltooi de bouwstenen die u in de volgende twee secties.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

In deze sectie maakt u Azure AD-eenmalige aanmelding inschakelen in Azure portal en configureren van eenmalige aanmelding in uw toepassing IBM Kenexa enquête Enterprise door het volgende te doen:

1. In de Azure-portal op de **IBM Kenexa enquête Enterprise** toepassingspagina integratie, klikt u op **eenmalige aanmelding**.

    ![Configureren van IBM Kenexa enquête Enterprise eenmalige aanmelding koppeling][4]

1. In de **eenmalige aanmelding** in het dialoogvenster de **modus** Schakel **SAML gebaseerde aanmelding** SSO inschakelen.
 
    ![In het dialoogvenster voor eenmalige aanmelding](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_samlbase.png)

1. In de **IBM Kenexa enquête Enterprise domein en URL's** sectie, voert u de volgende stappen uit:

    ![IBM Kenexa enquête Enterprise domein en URL's, eenmalige aanmelding informatie](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_url.png)

    a. In de **id** tekstvak, typ een URL met het volgende patroon: `https://surveys.kenexa.com/<companycode>`

    b. In de **antwoord-URL** tekstvak, typ een URL met het volgende patroon: `https://surveys.kenexa.com/<companycode>/tools/sso.asp`

    > [!NOTE] 
    > De bovenstaande waarden zijn niet echt. Deze bijwerken met de werkelijke-id en antwoord-URL. De werkelijke waarden, neem contact op met de [IBM Kenexa enquête Enterprise-ondersteuningsteam](https://www.ibm.com/support/home/?lnk=fcw).

1. Onder **SAML-handtekeningcertificaat**, klikt u op **certificaat (Base64)**, en sla het certificaatbestand op uw computer.

    ![De downloadkoppeling certificaat (Base64)](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_certificate.png) 

    De bedrijfstoepassing IBM Kenexa enquête wordt verwacht dat de asserties Security Asserties Markup Language (SAML) ontvangen in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de kenmerken van SAML-token is vereist. De waarde van de claim van de gebruiker-id in het antwoord moet overeenkomen met de SSO-ID die geconfigureerd in het systeem Kenexa. Als u wilt toewijzen op de juiste gebruikers-id in uw organisatie als SSO Internet Datagram Protocol (IDP), werken met de [IBM Kenexa enquête Enterprise-ondersteuningsteam](https://www.ibm.com/support/home/?lnk=fcw). 

    Standaard wordt de gebruikers-id in Azure AD ingesteld als de waarde van gebruiker UPN (User Principal Name). U kunt deze waarde wijzigen op de **kenmerk** tabblad, zoals wordt weergegeven in de volgende schermafbeelding. De integratie werkt pas nadat u de toewijzing hebt voltooid, juist.
    
    ![Het dialoogvenster gebruikerskenmerken](./media/kenexasurvey-tutorial/tutorial_attribute.png) 

1. Klik op **Opslaan**.

    ![De configureren eenmalige aanmelding knop Opslaan](./media/kenexasurvey-tutorial/tutorial_general_400.png)

1. Om te openen de **aanmelding configureren** venster onder **IBM Kenexa enquête Ondernemingsconfiguratie**, klikt u op **configureren IBM Kenexa enquête Enterprise**. 
 
    ![De koppeling IBM Kenexa enquête Enterprise configureren](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_configure.png)

1. Kopiëren de **afmelding URL**, **SAML entiteit-ID**, en **SAML-aanmelding Service-URL met eenmalige** waarden van de **Naslaggids** sectie.

1. In de **aanmelding configureren** venster onder **Naslaggids**, kopiëren de **afmelding URL**, **SAML entiteit-ID**, en **SAML eenmalige aanmelding in de Service-URL** waarden.

1. Voor het configureren van eenmalige aanmelding op de **IBM Kenexa enquête Enterprise** zijde, verzendt de gedownloade **certificaat (Base64)**, **afmelding URL**, **SAML entiteit-ID**, en **SAML-aanmelding Service-URL met eenmalige** waarden naar de [IBM Kenexa enquête Enterprise-ondersteuningsteam](https://www.ibm.com/support/home/?lnk=fcw).

> [!TIP]
> U kunt verwijzen naar een beknopte versie van deze instructies in de [Azure-portal](https://portal.azure.com) tijdens het instellen van de app. Nadat u de app vanuit de **Active Directory** > **bedrijfstoepassingen** sectie, klikt u op de **eenmalige aanmelding** tabblad, en vervolgens toegang tot de ingesloten documentatie via de **configuratie** sectie aan het einde. Zie voor meer informatie over de documentatie voor embedded-functie, [documentatie over Azure AD embedded](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
In deze sectie maakt maken u testgebruiker Britta Simon in Azure portal door het volgende te doen:

![Een Azure AD-testgebruiker maken][100]

1. In de Azure portal, in het linkerdeelvenster klikt u op de **Azure Active Directory** knop.

    ![De Azure Active Directory-knop](./media/kenexasurvey-tutorial/create_aaduser_01.png) 

1. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen**, en klik vervolgens op **alle gebruikers**.
    
    !['Gebruikers en groepen' en 'Alle gebruikers' koppelingen](./media/kenexasurvey-tutorial/create_aaduser_02.png) 

1. Om te openen de **gebruiker** in het dialoogvenster, klikt u op **toevoegen** aan de bovenkant van de **alle gebruikers** in het dialoogvenster.
 
    ![De knop toevoegen](./media/kenexasurvey-tutorial/create_aaduser_03.png) 

1. In de **gebruiker** dialoogvenster vak, voer de volgende stappen uit:
 
    ![Het dialoogvenster gebruiker](./media/kenexasurvey-tutorial/create_aaduser_04.png) 

    a. In de **naam** in het vak **BrittaSimon**.

    b. In de **gebruikersnaam** typt u het e-mailadres van gebruiker Britta Simon.

    c. Selecteer de **wachtwoord weergeven** selectievakje en noteer de waarde die wordt weergegeven in de **wachtwoord** vak.

    d. Klik op **Create**.
 
### <a name="create-an-ibm-kenexa-survey-enterprise-test-user"></a>Maak een testgebruiker IBM Kenexa enquête Enterprise

In deze sectie maakt u een gebruiker met de naam van Britta Simon in IBM Kenexa enquête onderneming. 

Gebruikers in het systeem IBM Kenexa enquête Enterprise maken en de SSO-ID voor hen toewijzen, kunt u werken met de [IBM Kenexa enquête Enterprise-ondersteuningsteam](https://www.ibm.com/support/home/?lnk=fcw). Deze SSO-ID-waarde moet ook worden toegewezen aan de gebruiker-id-waarde van Azure AD. U kunt deze standaardinstelling wijzigen op de **kenmerk** tabblad.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie maakt inschakelen u gebruiker Britta Simon toegang verlenen tot IBM Kenexa enquête Enterprise gebruiken Azure-eenmalige aanmelding.

![De de gebruikersrol toewijzen][200] 

Als u wilt toewijzen gebruiker Britta Simon met IBM Kenexa enquête Enterprise, het volgende doen:

1. Open in de Azure-portal, de **toepassingen** weergeven, gaat u naar de **Directory** weergave, selecteer **bedrijfstoepassingen**, en klik vervolgens op **alle toepassingen** .

    ![De 'Zakelijke toepassingen' en 'Alle Application' koppelingen][201] 

1. In de **toepassingen** in de lijst met **IBM Kenexa enquête Enterprise**.

    ![De IBM Kenexa enquête Enterprise-koppeling in de lijst met toepassingen](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_app.png) 

1. Klik in het linkerdeelvenster op **gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen][202] 

1. Klik op de **toevoegen** knop en klik vervolgens op de **toevoegen toewijzing** venster **gebruikers en groepen**.

    ![Het deelvenster Toewijzing toevoegen][203]

1. In de **gebruikers en groepen** in het dialoogvenster de **gebruikers** in de lijst met **Britta Simon**.

1. In de **gebruikers en groepen** in het dialoogvenster, klikt u op de **Selecteer** knop.

1. In de **toevoegen toewijzing** in het dialoogvenster, klikt u op de **toewijzen** knop.
    
### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen

In deze sectie maakt testen u de configuratie van uw Azure AD-eenmalige aanmelding met behulp van het toegangsvenster.

Wanneer u klikt op de **IBM Kenexa enquête Enterprise** tegel in het toegangsvenster, u moet worden automatisch aangemeld bij uw bedrijfstoepassing IBM Kenexa enquête.

## <a name="additional-resources"></a>Aanvullende resources

* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

<!--Image references-->

[1]: ./media/kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/kenexasurvey-tutorial/tutorial_general_04.png

[100]: ./media/kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/kenexasurvey-tutorial/tutorial_general_201.png
[202]: ./media/kenexasurvey-tutorial/tutorial_general_202.png
[203]: ./media/kenexasurvey-tutorial/tutorial_general_203.png

 
