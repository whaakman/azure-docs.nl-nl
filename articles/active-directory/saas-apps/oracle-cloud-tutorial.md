---
title: 'Zelfstudie: Azure Active Directory-integratie met Oracle Cloud-infrastructuur Console | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Oracle Cloud-infrastructuur-Console.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: f045fe19-11f8-4ccf-a3eb-8495fdc8716f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 456c984e577e3427ce8cd62d6f63987118f2c8ed
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164158"
---
# <a name="tutorial-integrate-oracle-cloud-infrastructure-console-with-azure-active-directory"></a>Zelfstudie: Oracle Cloud-infrastructuur Console integreren met Azure Active Directory

In deze zelfstudie leert u hoe u Oracle Cloud-infrastructuur Console integreert met Azure Active Directory (Azure AD). Als u Oracle Cloud-infrastructuur Console met Azure AD integreren, kunt u het volgende doen:

* Beheren in Azure AD die toegang tot Oracle Cloud-infrastructuur-Console heeft.
* Kunnen uw gebruikers worden automatisch aangemeld Oracle Cloud-infrastructuur-Console met hun Azure AD-accounts.
* Beheer uw accounts in één centrale locatie - Azure portal.

Zie voor meer informatie over de integratie van de SaaS-app met Azure AD, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Vereisten

Om te beginnen, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen abonnement hebt, krijgt u een [gratis account](https://azure.microsoft.com/free/).
* Ingeschakeld abonnement Oracle Cloud-infrastructuur Console eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie hebt u configureren en testen van Azure AD-eenmalige aanmelding in een testomgeving. Biedt ondersteuning voor Oracle Cloud-infrastructuur Console **SP** gestart door SSO.

## <a name="adding-oracle-cloud-infrastructure-console-from-the-gallery"></a>Oracle Cloud-infrastructuur Console uit de galerie toe te voegen

Voor het configureren van de integratie van Oracle Cloud-infrastructuur-Console in Azure AD, moet u de Oracle Cloud-infrastructuur Console uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Selecteer in het navigatiedeelvenster links in de **Azure Active Directory** service.
1. Navigeer naar **bedrijfstoepassingen** en selecteer vervolgens **alle toepassingen**.
1. Nieuwe toepassing toevoegen, selecteert u **nieuwe toepassing**.
1. In de **toevoegen vanuit de galerie** sectie, typt u **Oracle Cloud-infrastructuur Console** in het zoekvak in.
1. Selecteer **Oracle Cloud-infrastructuur Console** van resultaten van het deelvenster en vervolgens de app toevoegen. Wacht een paar seconden terwijl de app wordt toegevoegd aan uw tenant.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

Configureren en testen van Azure AD-eenmalige aanmelding met Oracle Cloud-infrastructuur Console met behulp van een testgebruiker met de naam **B. Simon**. Voor eenmalige aanmelding om te werken, moet u een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Oracle Cloud-infrastructuur Console vast te stellen.

Als u wilt configureren en testen van Azure AD-eenmalige aanmelding met Oracle Cloud-infrastructuur-Console, voert u de volgende bouwstenen:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-sso)**  zodat uw gebruikers deze functie wilt gebruiken.
1. **[Configureren van Oracle Cloud-infrastructuur Console](#configure-oracle-cloud-infrastructure-console)**  de SSO-instellingen configureren op de kant van de toepassing.
1. **[Maak een Azure AD-testgebruiker](#create-an-azure-ad-test-user)**  voor het testen van Azure AD eenmalige aanmelding met B. Simon.
1. **[Toewijzen van de Azure AD-testgebruiker](#assign-the-azure-ad-test-user)**  B. Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
1. **[Oracle Cloud-infrastructuur Console testgebruiker maken](#create-oracle-cloud-infrastructure-console-test-user)**  hebben een equivalent van B. Simon in Oracle Cloud-infrastructuur-Console die is gekoppeld aan de Azure AD-weergave van de gebruiker.
1. **[Eenmalige aanmelding testen](#test-sso)**  om te controleren of de configuratie werkt.

### <a name="configure-azure-ad-sso"></a>Azure AD-eenmalige aanmelding configureren

Volg deze stappen voor het inschakelen van Azure AD-eenmalige aanmelding in de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com/)op de **Oracle Cloud-infrastructuur Console** toepassingspagina integratie, vinden de **beheren** sectie en selecteer **één aanmeldings-** .
1. Op de **selecteert u een methode voor eenmalige aanmelding** pagina, selecteert u **SAML**.
1. Op de **instellen van eenmalige aanmelding met SAML** pagina, klikt u op het pictogram voor bewerken/pen voor **SAML-basisconfiguratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Op de **SAML-basisconfiguratie** pagina, voert u de waarden voor de volgende velden:

   > [!NOTE]
   > U krijgt het bestand met de Service Provider-metagegevens van de **configureren Oracle Cloud infrastructuur Console Single Sign-On** gedeelte van de zelfstudie.
    
   1. Klik op **metagegevensbestand uploaden**.

   1. Klik op het **mappictogram** om het metagegevensbestand te selecteren en klik op **Uploaden**.

   1. Zodra het bestand met metagegevens is geüpload, de **id** en **antwoord-URL** waarden ophalen automatisch ingevuld **SAML-basisconfiguratie** sectie tekstvak.
    
      > [!NOTE]
      > Als de waarden voor **Id** en **Antwoord-URL** niet automatisch worden ingevuld, kunt u de waarden zelf invullen afhankelijk van uw behoeften.

      In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://console.<REGIONNAME>.oraclecloud.com/`

      > [!NOTE]
      > De waarde is niet echt. Werk de waarde bij met de werkelijke aanmeldings-URL. Neem contact op met [Oracle Cloud-infrastructuur-Console-Client-ondersteuningsteam](https://www.oracle.com/support/advanced-customer-support/products/cloud.html) om de waarde. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Op de **instellen van eenmalige aanmelding met SAML** pagina, in de **SAML-handtekeningcertificaat** sectie, zoeken **federatieve metagegevens-XML** en selecteer **downloaden** voor het downloaden van het certificaat en sla deze op uw computer.

   ![De link om het certificaat te downloaden](common/metadataxml.png)

1. Oracle Cloud-infrastructuur-consoletoepassing wordt verwacht dat de SAML-asserties ondertekend in een specifieke indeling, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan de configuratie van de SAML-token kenmerken. In de volgende schermafbeelding wordt de lijst met standaardkenmerken weergegeven. Klik op het pictogram **Bewerken** om het dialoogvenster Gebruikerskenmerken te openen.

   ![image](common/edit-attribute.png)

1. Bovendien met bovenstaande consoletoepassing Oracle Cloud-infrastructuur wordt verwacht dat enkele meer kenmerken moeten worden doorgegeven in SAML-antwoord. In de **gebruikerskenmerken en Claims** sectie op de **groepclaims (Preview)**  dialoogvenster, voer de volgende stappen uit:

   1. Klik op de **pen** naast **naam id-waarde**.

   1. Selecteer **permanente** als **id indeling kiezen**.
 
   1. Klik op **Opslaan**.

      ![image](./media/oracle-cloud-tutorial/config07.png)
    
      ![image](./media/oracle-cloud-tutorial/config11.png)

   1. Klik op de **pen** naast **groepen die worden geretourneerd in de claim**.

   1. Selecteer **beveiligingsgroepen** uit de lijst met keuzerondjes.

   1. Selecteer **bronkenmerk** van **groeps-ID**.

   1. Controleer **aanpassen van de naam van de groepclaim**.

   1. In de **naam** in het tekstvak **groupName**.

   1. In de **Namespace (optioneel)** in het tekstvak `https://auth.oraclecloud.com/saml/claims`.

   1. Klik op **Opslaan**.

      ![image](./media/oracle-cloud-tutorial/config08.png)

1. Op de **Oracle Cloud-infrastructuur Console instellen** sectie, kopieert u de juiste URL's op basis van uw behoeften.

   ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="configure-oracle-cloud-infrastructure-console"></a>Oracle Cloud-infrastructuur-Console configureren

1. In een ander browservenster aanmelden bij Oracle Cloud-infrastructuur-Console als beheerder.

1. Klik op het menu aan de linkerkant en klik op **identiteit** navigeer vervolgens naar **Federation**.

   ![Configuratie](./media/oracle-cloud-tutorial/config01.png)

1. Opslaan de **Service Provider-bestand met metagegevens** door te klikken op de **downloaden van dit document** koppelen en upload het naar de **SAML-basisconfiguratie** sectie van Azure-portal en vervolgens Klik op **id-Provider toevoegen**.

   ![Configuratie](./media/oracle-cloud-tutorial/config02.png)

1. Op de **id-Provider toevoegen** pop, voer de volgende stappen uit:

   ![Configuratie](./media/oracle-cloud-tutorial/config03.png)

   1. In de **naam** tekst vak, Voer uw naam in.

   1. In de **beschrijving** tekst voert u de beschrijving.

   1. Selecteer **MICROSOFT ACTIVE DIRECTORY FEDERATION Services (ADFS) of SAML 2.0 COMPATIBELE id-PROVIDER** als **TYPE**.

   1. Klik op **Bladeren** voor het uploaden van de federatieve metagegevens-XML, die u hebt gedownload vanuit Azure portal.

   1. Klik op **doorgaan** en klik op de **id-Provider bewerken** sectie de volgende stappen uitvoeren:

      ![Configuratie](./media/oracle-cloud-tutorial/config09.png)

   1. Voor **IDENTITY PROVIDER groep** velden, voer de groepsnaam en de groeps-ID die zijn ingesteld in Azure portal. De groep moet worden toegewezen met de bijbehorende groep in **OCI groep** veld.

   1. U kunt meerdere groepen toewijzen aan de hand van de instellingen in Azure portal en de noodzaak van uw organisatie. Klik op **+ toevoegen toewijzing** zoveel groepen als u wilt toevoegen.

   1. Klik op **Indienen**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een testgebruiker in Azure portal B. Simon genoemd.

1. Selecteer in het linkerdeelvenster in de Azure-portal, **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens **alle gebruikers**.
1. Selecteer **Nieuwe gebruiker** boven aan het scherm.
1. In de **gebruiker** eigenschappen als volgt te werk:
   1. Voer in het veld **Naam** `B. Simon` in.  
   1. In de **gebruikersnaam** en voer de username@companydomain.extension. Bijvoorbeeld `B. Simon@contoso.com`.
   1. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak **Wachtwoord**.
   1. Klik op **Create**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie zult u B. Simon gebruiken Azure eenmalige aanmelding door toegang te verlenen aan Oracle Cloud-infrastructuur Console inschakelen.

1. Selecteer in de Azure portal, **bedrijfstoepassingen**, en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst met toepassingen, **Oracle Cloud-infrastructuur Console**.
1. Zoek in de pagina overzicht van de app, de **beheren** sectie en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in de **toevoegen toewijzing** dialoogvenster.

   ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. In de **gebruikers en groepen** dialoogvenster, selecteer **B. Simon** uit de lijst met gebruikers, klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Als u een waarde voor de rol in het SAML-verklaring verwacht de **rol selecteren** dialoogvenster, selecteer de juiste rol voor de gebruiker in de lijst en klik vervolgens op de **Selecteer** knop aan de onderkant van het scherm.
1. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-oracle-cloud-infrastructure-console-test-user"></a>Oracle Cloud-infrastructuur Console testgebruiker maken

 Oracle Cloud-infrastructuur-Console biedt ondersteuning voor just-in-time inrichting, dit is standaard. Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker is niet gemaakt tijdens een poging tot toegang en ook niet nodig om de gebruiker te maken.

### <a name="test-sso"></a>Test eenmalige aanmelding

Wanneer u de Oracle Cloud-infrastructuur Console-tegel in het toegangsvenster selecteert, wordt u omgeleid naar de aanmeldingspagina voor de Oracle Cloud-infrastructuur-Console. Selecteer de **id-PROVIDER** uit de vervolgkeuzelijst en klik op **doorgaan** zoals hieronder wordt weergegeven aan te melden. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

![Configuratie](./media/oracle-cloud-tutorial/config10.png)

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
