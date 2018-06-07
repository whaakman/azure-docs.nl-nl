---
title: 'Zelfstudie: G Suite configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het automatisch inrichten en de gebruikersaccounts van Azure AD G Suite ongedaan in te richten.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: a34e6882c73725acd7827f63ff88b9ba4bac52db
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34590185"
---
# <a name="tutorial-configure-g-suite-for-automatic-user-provisioning"></a>Zelfstudie: G Suite configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is het ziet u hoe u automatisch inrichten en de gebruikersaccounts van Azure Active Directory (Azure AD) G Suite ongedaan in te richten.

> [!NOTE]
> Deze zelfstudie wordt een connector die is ingebouwd in de Azure AD-gebruiker inrichtingsservice beschreven. Zie voor belangrijke informatie over wat deze service doet, hoe het werkt en veelgestelde vragen [gebruikers inrichten en opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met Suite G, moet u de volgende items:

- Een Azure AD-abonnement
- Een G Suite eenmalige aanmelding ingeschakeld abonnement
- Een abonnement van Google Apps of Google Cloud Platform-abonnement.

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u [ophalen van een proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assign-users-to-g-suite"></a>Gebruikers toewijzen aan G-Suite

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, worden alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureert en de inrichting-service inschakelen, moet u bepalen welke gebruikers of groepen in Azure AD toegang hebben tot uw app moeten. Nadat u deze beslissing hebt genomen, kunt u deze gebruikers toewijzen aan uw app door de instructies in [een gebruiker of groep toewijzen aan een app enterprise](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

> [!IMPORTANT]
> We raden aan dat één Azure AD-gebruiker worden toegewezen aan G Suite voor het testen van de configuratie van de inrichting. U kunt extra gebruikers en groepen later toewijzen.

> Wanneer u een gebruiker aan G Suite toewijst, selecteert u de **gebruiker** of **groep** rol in het dialoogvenster toewijzing. De **standaardtoegang** rol werkt niet voor het inrichten.

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde gebruikersinrichting inschakelen

Deze sectie leidt u door het proces van het verbinden van uw Azure AD aan het gebruikersaccount inrichting-API van G Suite. Hiermee kunt u ook de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in G Suite op basis van gebruiker en groepstoewijzing in Azure AD configureren.

>[!TIP]
>U kunt er ook voor kiezen om in te schakelen op basis van SAML eenmalige aanmelding voor G Suites, door de instructies in de [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.

### <a name="configure-automatic-user-account-provisioning"></a>Automatisch gebruikers inrichten van account configureren

> [!NOTE]
> Een andere geschikte optie voor het automatiseren van gebruikersinrichting G Suite is het gebruik [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en). GADS richt uw on-premises Active Directory-identiteiten G Suite. Daarentegen is richt de oplossing in deze zelfstudie uw Azure Active Directory (cloud) gebruikers en groepen e-mailfunctionaliteit G Suite. 

1. Aanmelden bij de [Google Apps-beheerconsole](http://admin.google.com/) met uw beheerdersaccount en selecteer vervolgens **beveiliging**. Als u de koppeling niet ziet, is deze mogelijk verborgen onder de **meer besturingselementen** menu aan de onderkant van het scherm.
   
    ![Selecteer beveiliging.][10]

2. Op de **beveiliging** pagina **API Reference**.
   
    ![Selecteer de API-verwijzing.][15]

3. Selecteer **inschakelen API-toegang**.
   
    ![Selecteer de API-verwijzing.][16]

    > [!IMPORTANT]
    > Voor elke gebruiker die u wilt inrichten met Suite G, hun gebruikersnaam in Azure Active Directory *moet* worden gekoppeld aan een aangepast domein. Bijvoorbeeld: de gebruiker die zijn opgemaakt als namen bob@contoso.onmicrosoft.com worden niet geaccepteerd door G Suite. Aan de andere kant bob@contoso.com wordt geaccepteerd. U kunt het domein van een bestaande gebruiker wijzigen door de eigenschappen bewerken in Azure AD. Instructies voor het instellen van een aangepast domein voor zowel de Azure Active Directory en de G Suite in de volgende stappen hebt opgenomen.
      
4. Als u nog een aangepaste domeinnaam naar uw Azure Active Directory hebt toegevoegd, klikt u vervolgens de volgende stappen uitvoeren:
  
    a. In de [Azure-portal](https://portal.azure.com), Selecteer op het navigatiedeelvenster links **Active Directory**. Selecteer uw directory in de adreslijst. 

    b. Selecteer **domeinnaam** in het navigatiedeelvenster links en selecteer **toevoegen**.
     
     ![Domein](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_1.png)

     ![domein toevoegen](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_2.png)

    c. Typ de naam van uw domein in de **domeinnaam** veld. Deze domeinnaam moet dezelfde domeinnaam die u wilt gebruiken voor G Suite. Selecteer vervolgens de **domein toevoegen** knop.
     
     ![Domeinnaam](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_3.png)

    d. Selecteer **volgende** naar de pagina verificatie. Om te controleren of de eigenaar van dit domein, DNS-records van het domein volgens de waarden die beschikbaar zijn op deze pagina te bewerken. U kunt ervoor kiezen om te controleren met behulp van **MX-records** of **TXT-records**, afhankelijk van wat u selecteren voor de **recordtype** optie. 
    
    Zie voor meer uitgebreide instructies voor het controleren van domeinnamen met Azure AD [uw eigen domeinnaam toevoegen aan Azure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).
     
     ![Domein](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_4.png)

    e. Herhaal de voorgaande stappen voor alle domeinen die u wilt toevoegen aan uw directory.

    > [!NOTE]
    Het aangepaste domein moet overeenkomen met de domeinnaam van de Azure AD-bron voor het inrichten van de gebruiker. Als ze niet overeenkomen, is het mogelijk dat u kunt het probleem oplossen door het kenmerk toewijzing aanpassing implementeren.


5. Nu dat u uw domeinen met Azure AD hebt gecontroleerd, moet u ze opnieuw verifiëren met Google Apps. Voor elk domein dat al is niet geregistreerd bij Google, moet u de volgende stappen uitvoeren:
   
    a. In de [Google Apps-beheerconsole](http://admin.google.com/), selecteer **domeinen**.
     
     ![Selecteer domeinen][20]

    b. Selecteer **toevoegen van een domein of de domeinalias van een**.
     
     ![Een nieuw domein toevoegen][21]

    c. Selecteer **toevoegen van een ander domein**, en typ in het domein dat u wilt toevoegen.
     
     ![Typ de domeinnaam van uw][22]

    d. Selecteer **doorgaan en verifiëren dat dit domein**. Volg de stappen om te controleren of de eigenaar van de domeinnaam. Zie voor uitgebreide instructies voor het controleren van uw domein met Google [verifiëren dat uw site met Google Apps](https://support.google.com/webmasters/answer/35179).

    e. Herhaal de voorgaande stappen voor aanvullende domeinen die u wilt toevoegen aan Google Apps.
     
     > [!WARNING]
     > Als u het primaire domein voor uw tenant G Suite wijzigen en als u al hebt geconfigureerd eenmalige aanmelding met Azure AD, wordt er herhaalt u stap #3 onder [stap 2: eenmalige aanmelding inschakelen](#step-two-enable-single-sign-on).
       
6. In de [Google Apps-beheerconsole](http://admin.google.com/), selecteer **beheerdersrollen**.
   
     ![Google Apps selecteren][26]

7. Bepalen welke beheerdersaccount dat u gebruiken wilt voor het beheren van gebruikers inrichten. Voor de **beheerrol** bewerken van het account, de **bevoegdheden** voor die rol. Zorg ervoor dat u ervoor zorgen dat al **API beheerdersbevoegdheden** zodat dit account kan worden gebruikt voor het inrichten.
   
     ![Google Apps selecteren][27]
   
    > [!NOTE]
    > Als u een productie-omgeving configureert, wordt de aanbevolen procedure is het maken van een beheerdersaccount te gebruiken in G Suite specifiek voor deze stap. Deze accounts moeten een beheerdersrol gekoppeld met de benodigde API-bevoegdheden hebben.
     
8. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory** > **zakelijke Apps** > **alle toepassingen** sectie.

9. Als u G Suite al hebt geconfigureerd voor eenmalige aanmelding, zoekt u uw exemplaar van G-Suite met behulp van het zoekveld. Selecteer anders **toevoegen**, en zoek vervolgens naar **G Suite** of **Google Apps** in de galerie met toepassingen. Selecteer uw app in de zoekresultaten en vervolgens toe te voegen aan uw lijst met toepassingen.

10. Selecteer uw exemplaar van de Suite G, en selecteer vervolgens de **inrichten** tabblad.

11. Stel de **Inrichtingsmodus** naar **automatische**. 

     ![Inrichten](./media/active-directory-saas-google-apps-provisioning-tutorial/provisioning.png)

12. Onder de **beheerdersreferenties** sectie **autoriseren**. Er wordt een dialoogvenster voor autorisatie van Google in een nieuw browservenster geopend.

13. Bevestig dat u wilt machtigen voor Azure Active Directory wijzigingen aanbrengen in uw tenant G Suite. Selecteer **accepteren**.
    
     ![Controleer de machtigingen.][28]

14. Selecteer in de Azure-portal **testverbinding** om ervoor te zorgen dat Azure AD verbinding met uw app maken kan. Als de verbinding is mislukt, zorg ervoor dat uw account G Suite Team beheerdersmachtigingen heeft. Probeer de **autoriseren** stap opnieuw.

15. Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld. Selecteer vervolgens het selectievakje in.

16. Selecteer **opslaan.**

17. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-gebruikers met Google Apps**.

18. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die zijn gesynchroniseerd vanuit Azure AD G Suite. De kenmerken die zijn **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in G Suite voor update-bewerkingen worden gebruikt. Selecteer **opslaan** eventuele wijzigingen doorvoeren.

19. Om de Azure AD-service voor G Suite inricht, wijzigen de **inrichting Status** naar **op** in **instellingen**.

20. Selecteer **Opslaan**.

Dit proces wordt gestart voor de initiële synchronisatie van gebruikers of groepen die zijn toegewezen aan G Suite in de sectie gebruikers en groepen. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer elke 40 minuten plaats terwijl de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van activiteitenlogboeken. Deze logboeken worden alle acties die worden uitgevoerd door de inrichting van uw app-service beschreven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Aanvullende resources

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](manage-apps/what-is-single-sign-on.md)
* [Eenmalige aanmelding configureren](active-directory-saas-google-apps-tutorial.md)



<!--Image references-->

[10]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-security.png
[15]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api-enabled.png
[20]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-another.png
[24]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-auth.png
