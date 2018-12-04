---
title: 'Zelfstudie: G Suite configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het automatisch inrichten en verwijdering van gebruikersaccounts uit Azure AD met G Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 48a835bf8c63ffa5512173b600fb85abd5c6cb45
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52840347"
---
# <a name="tutorial-configure-g-suite-for-automatic-user-provisioning"></a>Zelfstudie: G Suite configureren voor het automatisch inrichten van gebruikers

Er is het doel van deze zelfstudie leert u hoe u automatisch inrichten en de inrichting van gebruikersaccounts uit Azure Active Directory (Azure AD) met G Suite ongedaan maken.

> [!NOTE]
> Deze zelfstudie beschrijft een connector die is gebaseerd op de Provisioning-Service van Azure AD-gebruiker. Zie voor belangrijke informatie over wat deze service biedt, hoe het werkt en veelgestelde vragen [automatiseren van gebruikersinrichting en -opheffing in SaaS-toepassingen met Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met G Suite, moet u de volgende items:

- Een Azure AD-abonnement
- Een G Suite eenmalige aanmelding ingeschakeld abonnement
- Een abonnement op Google Apps of Google Cloud Platform-abonnement.

> [!NOTE]
> Als u wilt testen van de stappen in deze zelfstudie, raden we niet met behulp van een productie-omgeving.

Als u wilt testen van de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik uw productie-omgeving, niet als dat nodig is.
- Als u geen een proefversie Azure AD-omgeving hebt, kunt u [een proefversie van één maand krijgen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assign-users-to-g-suite"></a>Gebruikers toewijzen aan G Suite

Azure Active Directory maakt gebruik van een concept genaamd "toewijzingen" om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van het inrichten van automatische gebruikersaccounts, worden alleen de gebruikers en groepen die '' aan een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureren en inschakelen van de inrichtingsservice, moet u bepalen welke gebruikers of groepen in Azure AD toegang hebben tot uw app moeten. Nadat u deze beslissing hebt genomen, kunt u deze gebruikers toewijzen aan uw app door de instructies in [een gebruiker of groep toewijzen aan een enterprise-app](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

> [!IMPORTANT]
> We raden aan dat een enkele Azure AD-gebruiker worden toegewezen aan G Suite voor het testen van de configuratie van de inrichting. U kunt extra gebruikers en groepen later toewijzen.

> Wanneer u een gebruiker aan G Suite toewijzen, selecteert u de **gebruiker** of **groep** rol in het dialoogvenster toewijzing. De **standaardtoegang** rol werkt niet voor het inrichten.

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde gebruikersinrichting inschakelen

Deze sectie helpt u bij het proces van het verbinden van uw Azure AD voor het gebruikersaccount waarbij API van G Suite wordt ingericht. Ook helpt u bij het configureren van de provisioning-service voor het maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in G Suite op basis van gebruiker en groepstoewijzing in Azure AD.

>[!TIP]
>U kunt er ook voor kiezen om in te schakelen op basis van SAML eenmalige aanmelding voor G Suite, door de instructies in de [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies een fraaie aanvulling in elkaar.

### <a name="configure-automatic-user-account-provisioning"></a>Het inrichten van automatische gebruikersaccounts configureren

> [!NOTE]
> Een andere beschikbare optie voor het automatiseren van gebruikersinrichting met G Suite, is met [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en). GADS voorziet uw on-premises Active Directory-identiteiten met G Suite. De oplossing in deze zelfstudie richt daarentegen uw Azure Active Directory (cloud): gebruikers en groepen e-mail kunnen ontvangen met G Suite. 

1. Aanmelden bij de [Google Apps-beheerconsole](https://admin.google.com/) met uw administrator-account en selecteer vervolgens **Security**. Als u de koppeling niet ziet, is deze mogelijk verborgen onder de **meer besturingselementen** menu aan de onderkant van het scherm.
   
    ![Schakel de beveiliging.][10]

1. Op de **Security** weergeeft, schakelt **API-verwijzing**.
   
    ![Selecteer de API-verwijzing.][15]

1. Selecteer **inschakelen API-toegang**.
   
    ![Selecteer de API-verwijzing.][16]

    > [!IMPORTANT]
    > Voor iedere gebruiker die u van plan bent om in te richten met G Suite, hun gebruikersnaam in Azure Active Directory *moet* worden gekoppeld aan een aangepast domein. Bijvoorbeeld, de gebruiker die eruit namen bob@contoso.onmicrosoft.com worden niet geaccepteerd door G Suite. Aan de andere kant bob@contoso.com wordt geaccepteerd. U kunt het domein van een bestaande gebruiker wijzigen door de eigenschappen bewerken in Azure AD. We hebben opgenomen instructies voor het instellen van een aangepast domein voor G Suite en Azure Active Directory in de volgende stappen.
      
1. Als u nog een aangepaste domeinnaam voor uw Azure Active Directory nog niet hebt toegevoegd, klikt u vervolgens de volgende stappen uitvoeren:
  
    a. In de [Azure-portal](https://portal.azure.com), selecteer in het navigatiedeelvenster links **Active Directory**. Selecteer uw directory in de adreslijst. 

    b. Selecteer **domeinnaam** in het navigatiedeelvenster links in en selecteer vervolgens **toevoegen**.
     
     ![Domain](./media/google-apps-provisioning-tutorial/domain_1.png)

     ![Domein toevoegen](./media/google-apps-provisioning-tutorial/domain_2.png)

    c. Typ de naam van uw domein in de **domeinnaam** veld. Deze domeinnaam moet dezelfde domeinnaam die u wilt gebruiken voor G Suite. Selecteer vervolgens de **domein toevoegen** knop.
     
     ![Domeinnaam](./media/google-apps-provisioning-tutorial/domain_3.png)

    d. Selecteer **volgende** om te gaan naar de pagina verificatie. Als u wilt controleren of u eigenaar dit domein bent, van het domein DNS-records op basis van de waarden die beschikbaar zijn op deze pagina te bewerken. U kunt selecteren om te controleren met behulp van **MX-records** of **TXT-records**, afhankelijk van wat u selecteert voor de **recordtype** optie. 
    
    Zie voor meer uitgebreide instructies voor het controleren van domeinnamen met Azure AD [uw eigen domeinnaam toevoegen aan Azure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).
     
     ![Domain](./media/google-apps-provisioning-tutorial/domain_4.png)

    e. Herhaal de voorgaande stappen voor alle domeinen die u van plan bent om toe te voegen aan uw directory.

    > [!NOTE]
    Voor het inrichten van gebruikers, kan het aangepaste domein moet overeenkomen met de domeinnaam van de Azure AD-bron. Als ze niet overeenkomen, kunt u mogelijk het probleem oplossen door het implementeren van kenmerk toewijzing aanpassen.


1. Nu dat u hebt gecontroleerd dat alle domeinen met Azure AD, moet u ze opnieuw verifiëren met Google Apps. Voor elk domein dat al is niet geregistreerd bij Google, voert u de volgende stappen uit:
   
    a. In de [Google Apps-beheerconsole](https://admin.google.com/), selecteer **domeinen**.
     
     ![Domeinen selecteren][20]

    b. Selecteer **toevoegen van een domein of een domeinalias**.
     
     ![Een nieuw domein toevoegen][21]

    c. Selecteer **toevoegen van een ander domein**, en typ vervolgens de naam van het domein dat u wilt toevoegen.
     
     ![Typ de naam van uw domein][22]

    d. Selecteer **doorgaan en domeineigendom controleren**. Volg de stappen om te controleren dat u eigenaar bent van de domeinnaam. Zie voor uitgebreide instructies voor het controleren van uw domein met Google [uw site verifiëren met Google Apps](https://support.google.com/webmasters/answer/35179).

    e. Herhaal de voorgaande stappen voor elke extra domeinen die u wilt toevoegen aan Google Apps.
     
     > [!WARNING]
     > Als u het primaire domein te voor uw G Suite-tenant wijzigen, en als u al hebt geconfigureerd eenmalige aanmelding met Azure AD, wordt er herhaalt u stap #3 onder [stap 2: eenmalige aanmelding inschakelen](#step-two-enable-single-sign-on).
       
1. In de [Google Apps-beheerconsole](https://admin.google.com/), selecteer **-beheerdersrollen**.
   
     ![Google Apps selecteren][26]

1. Bepalen welke beheerdersaccount dat u gebruiken wilt voor het inrichten van gebruikers beheren. Voor de **beheerdersrol** bewerken van het account de **bevoegdheden** voor die rol. Zorg ervoor dat u alle **API beheerdersbevoegdheden** zodat dit account kan worden gebruikt voor het inrichten.
   
     ![Google Apps selecteren][27]
   
    > [!NOTE]
    > Als u een productie-omgeving configureert, wordt de aanbevolen procedure is het maken van een beheerdersaccount in G Suite specifiek voor deze stap. Deze accounts moet een beheerdersrol gekoppeld die de juiste API-rechten heeft.
     
1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory** > **Bedrijfsapps** > **alle toepassingen** sectie.

1. Als u al G Suite hebt geconfigureerd voor eenmalige aanmelding, zoekt u uw exemplaar van G Suite met behulp van het zoekveld. Selecteer anders **toevoegen**, en zoek vervolgens **G Suite** of **Google Apps** in de toepassingengalerie. Selecteer de app in de lijst met zoekresultaten en deze vervolgens toevoegen aan uw lijst met toepassingen.

1. Selecteer uw exemplaar van G Suite, en selecteer vervolgens de **Provisioning** tabblad.

1. Stel de **Inrichtingsmodus** naar **automatische**. 

     ![Inrichten](./media/google-apps-provisioning-tutorial/provisioning.png)

1. Onder de **beheerdersreferenties** sectie, selecteer **autoriseren**. Er wordt een dialoogvenster voor autorisatie van Google geopend in een nieuw browservenster.

1. Bevestig dat u wilt machtigen voor Azure Active Directory wijzigingen aanbrengen in uw G Suite-tenant. Selecteer **Accepteren**.
    
     ![Controleer of u machtigingen.][28]

1. Selecteer in de Azure portal, **testverbinding** om ervoor te zorgen dat Azure AD verbinding met uw app maken kunt. Als de verbinding is mislukt, zorg ervoor dat uw G Suite-account Teambeheerder machtigingen heeft. Probeer de **autoriseren** stap opnieuw uit.

1. Voer het e-mailadres van een persoon of groep die inrichting fout meldingen moet ontvangen de **e-mailmelding** veld. Selecteer vervolgens het selectievakje in.

1. Selecteer **opslaan.**

1. Onder de **toewijzingen** sectie, selecteer **synchroniseren Azure Active Directory: gebruikers met Google Apps**.

1. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die worden gesynchroniseerd vanuit Azure AD met G Suite. De kenmerken die zijn **overeenkomende** eigenschappen worden gebruikt zodat deze overeenkomen met de gebruikersaccounts in G Suite voor update-bewerkingen. Selecteer **opslaan** doorvoeren van wijzigingen.

1. Als wilt inschakelen in de Azure AD-inrichtingsservice voor G Suite, wijzigt de **Inrichtingsstatus** naar **op** in **instellingen**.

1. Selecteer **Opslaan**.

Dit proces wordt gestart voor de initiële synchronisatie van gebruikers of groepen die zijn toegewezen aan G Suite in de sectie gebruikers en groepen. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten plaatsvinden terwijl de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en koppelingen volgen voor het inrichten van activiteitenlogboeken. Deze logboeken worden alle acties die worden uitgevoerd met de provisioning-service op uw app beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over het inrichten van automatische gebruikersaccounts](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het inrichten van gebruikersaccounts voor bedrijfs-Apps beheren](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Eenmalige aanmelding configureren](google-apps-tutorial.md)



<!--Image references-->

[10]: ./media/google-apps-provisioning-tutorial/gapps-security.png
[15]: ./media/google-apps-provisioning-tutorial/gapps-api.png
[16]: ./media/google-apps-provisioning-tutorial/gapps-api-enabled.png
[20]: ./media/google-apps-provisioning-tutorial/gapps-domains.png
[21]: ./media/google-apps-provisioning-tutorial/gapps-add-domain.png
[22]: ./media/google-apps-provisioning-tutorial/gapps-add-another.png
[24]: ./media/google-apps-provisioning-tutorial/gapps-provisioning.png
[25]: ./media/google-apps-provisioning-tutorial/gapps-provisioning-auth.png
[26]: ./media/google-apps-provisioning-tutorial/gapps-admin.png
[27]: ./media/google-apps-provisioning-tutorial/gapps-admin-privileges.png
[28]: ./media/google-apps-provisioning-tutorial/gapps-auth.png
