---
title: 'Zelfstudie: Google Apps configureren voor het automatisch gebruikers inrichten in Azure | Microsoft Docs'
description: Informatie over het automatisch inrichten en gebruikersaccounts vanuit Azure AD naar Google Apps ongedaan in te richten.
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
ms.date: 12/01/2017
ms.author: jeedes
ms.openlocfilehash: a77b5b1fff670ed7698d0ef48fa63f8a8f9be819
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configure-google-apps-for-automatic-user-provisioning"></a>Zelfstudie: Google Apps configureren voor het automatisch gebruikers inrichten

Het doel van deze zelfstudie is het ziet u hoe u automatisch in te richten en ongedaan inrichten gebruikersaccounts met Google Apps van Azure Active Directory (Azure AD).

## <a name="prerequisites"></a>Vereisten

Het scenario dat wordt beschreven in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant.
*   Een geldige tenant voor Google Apps voor werk of Google Apps voor onderwijs. U kunt een gratis proefaccount gebruiken voor de service.
*   Een gebruikersaccount in Google Apps met beheerdersmachtigingen Team.

## <a name="assign-users-to-google-apps"></a>Gebruikers toewijzen aan Google Apps

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, worden alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd.

Voordat u configureert en de inrichting-service inschakelen, moet u bepalen welke gebruikers of groepen in Azure AD toegang hebben tot uw app in Google Apps moeten. Nadat u deze beslissing hebt genomen, kunt u deze gebruikers toewijzen aan uw app in Google Apps door de instructies in [een gebruiker of groep toewijzen aan een app enterprise](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

> [!IMPORTANT]
> We raden aan dat één Azure AD-gebruiker worden toegewezen aan Google Apps in de configuratie van de inrichting te testen. U kunt extra gebruikers en groepen later toewijzen.

> Wanneer u een gebruiker aan Google Apps toewijst, selecteert u de **gebruiker** of **groep** rol in het dialoogvenster toewijzing. De **standaardtoegang** rol werkt niet voor het inrichten.

## <a name="enable-automated-user-provisioning"></a>Geautomatiseerde gebruikersinrichting inschakelen

Deze sectie leidt u door het proces van het verbinden van uw Azure AD aan het gebruikersaccount inrichting-API van Google Apps. Hiermee kunt u ook de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in Google Apps op basis van gebruiker en groepstoewijzing in Azure AD configureren.

>[!TIP]
>U kunt er ook voor kiezen om in te schakelen op basis van SAML eenmalige aanmelding voor Google Apps, door de instructies in de [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.

### <a name="configure-automatic-user-account-provisioning"></a>Automatisch gebruikers inrichten van account configureren

> [!NOTE]
> Een andere geschikte optie voor het automatiseren van gebruikers inrichten met Google Apps wordt met [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en). GADS richt uw on-premises Active Directory-identiteiten met Google Apps. Daarentegen is richt de oplossing in deze zelfstudie uw Azure Active Directory (cloud)-gebruikers en groepen e-mail naar Google Apps. 

1. Aanmelden bij de [Google Apps-beheerconsole](http://admin.google.com/) met uw beheerdersaccount en selecteer vervolgens **beveiliging**. Als u de koppeling niet ziet, is deze mogelijk verborgen onder de **meer besturingselementen** menu aan de onderkant van het scherm.
   
    ![Selecteer beveiliging.][10]

2. Op de **beveiliging** pagina **API Reference**.
   
    ![Selecteer de API-verwijzing.][15]

3. Selecteer **inschakelen API-toegang**.
   
    ![Selecteer de API-verwijzing.][16]

    > [!IMPORTANT]
    > Voor elke gebruiker die u wilt inrichten met Google Apps, hun gebruikersnaam in Azure Active Directory *moet* worden gekoppeld aan een aangepast domein. Bijvoorbeeld: de gebruiker die zijn opgemaakt als namen bob@contoso.onmicrosoft.com worden niet geaccepteerd door Google Apps. Aan de andere kant bob@contoso.com wordt geaccepteerd. U kunt het domein van een bestaande gebruiker wijzigen door de eigenschappen bewerken in Azure AD. Instructies voor het instellen van een aangepast domein voor zowel de Azure Active Directory en de Google Apps in de volgende stappen hebt opgenomen.
      
4. Als u nog een aangepaste domeinnaam naar uw Azure Active Directory hebt toegevoegd, klikt u vervolgens de volgende stappen uitvoeren:
  
    a. In de [Azure-portal](https://portal.azure.com), Selecteer op het navigatiedeelvenster links **Active Directory**. Selecteer uw directory in de adreslijst. 

    b. Selecteer **domeinnaam** in het navigatiedeelvenster links en selecteer **toevoegen**.
     
     ![Domein](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_1.png)

     ![domein toevoegen](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_2.png)

    c. Typ de naam van uw domein in de **domeinnaam** veld. Deze domeinnaam moet dezelfde domeinnaam die u wilt gebruiken voor Google Apps. Selecteer vervolgens de **domein toevoegen** knop.
     
     ![Domeinnaam](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_3.png)

    d. Selecteer **volgende** naar de pagina verificatie. Om te controleren of de eigenaar van dit domein, DNS-records van het domein volgens de waarden die beschikbaar zijn op deze pagina te bewerken. U kunt ervoor kiezen om te controleren met behulp van **MX-records** of **TXT-records**, afhankelijk van wat u selecteren voor de **recordtype** optie. 
    
    Zie voor meer uitgebreide instructies voor het controleren van domeinnamen met Azure AD [uw eigen domeinnaam toevoegen aan Azure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).
     
     ![Domein](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_4.png)

    e. Herhaal de voorgaande stappen voor alle domeinen die u wilt toevoegen aan uw directory.

    > [!NOTE]
    Voor gebruikers inrichten, moet het aangepaste domein Google Apps overeenkomen met de domeinnaam van de Azure AD-bron. Als ze niet overeenkomen, is het mogelijk dat u kunt het probleem oplossen door het kenmerk toewijzing aanpassing implementeren.


5. Nu dat u uw domeinen met Azure AD hebt gecontroleerd, moet u ze opnieuw verifiëren met Google Apps. Voor elk domein dat al is niet geregistreerd bij Google Apps, moet u de volgende stappen uitvoeren:
   
    a. In de [Google Apps-beheerconsole](http://admin.google.com/), selecteer **domeinen**.
     
     ![Selecteer domeinen][20]

    b. Selecteer **toevoegen van een domein of de domeinalias van een**.
     
     ![Een nieuw domein toevoegen][21]

    c. Selecteer **toevoegen van een ander domein**, en typ in het domein dat u wilt toevoegen.
     
     ![Typ de domeinnaam van uw][22]

    d. Selecteer **doorgaan en verifiëren dat dit domein**. Volg de stappen om te controleren of de eigenaar van de domeinnaam. Zie voor uitgebreide instructies voor het controleren van uw domein met Google Apps [verifiëren dat uw site met Google Apps](https://support.google.com/webmasters/answer/35179).

    e. Herhaal de voorgaande stappen voor aanvullende domeinen die u wilt toevoegen aan Google Apps.
     
     > [!WARNING]
     > Als u het primaire domein voor uw Google Apps-tenant wijzigen en als u al hebt geconfigureerd eenmalige aanmelding met Azure AD, wordt er herhaalt u stap #3 onder [stap 2: eenmalige aanmelding inschakelen](#step-two-enable-single-sign-on).
       
6. In de [Google Apps-beheerconsole](http://admin.google.com/), selecteer **beheerdersrollen**.
   
     ![Google Apps selecteren][26]

7. Bepalen welke beheerdersaccount dat u gebruiken wilt voor het beheren van gebruikers inrichten. Voor de **beheerrol** bewerken van het account, de **bevoegdheden** voor die rol. Zorg ervoor dat u ervoor zorgen dat al **API beheerdersbevoegdheden** zodat dit account kan worden gebruikt voor het inrichten.
   
     ![Google Apps selecteren][27]
   
    > [!NOTE]
    > Als u een productie-omgeving configureert, wordt de aanbevolen procedure is het maken van een beheerdersaccount te gebruiken in Google Apps specifiek voor deze stap. Deze accounts moeten een beheerdersrol gekoppeld met de benodigde API-bevoegdheden hebben.
     
8. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory** > **zakelijke Apps** > **alle toepassingen** sectie.

9. Als u al Google Apps hebt geconfigureerd voor eenmalige aanmelding, zoekt u uw exemplaar van Google Apps met behulp van het zoekveld. Selecteer anders **toevoegen**, en zoek vervolgens naar **Google Apps** in de galerie met toepassingen. Selecteer **Google Apps** uit de lijst met zoekresultaten en voeg deze toe aan de lijst met toepassingen.

10. Selecteer uw exemplaar van Google Apps, en selecteer vervolgens de **inrichten** tabblad.

11. Stel de **Inrichtingsmodus** naar **automatische**. 

     ![Inrichten](./media/active-directory-saas-google-apps-provisioning-tutorial/provisioning.png)

12. Onder de **beheerdersreferenties** sectie **autoriseren**. Er wordt een dialoogvenster voor autorisatie van Google Apps in een nieuw browservenster geopend.

13. Bevestig dat u wilt machtigen voor Azure Active Directory wijzigingen aanbrengen in uw Google Apps-tenant. Selecteer **accepteren**.
    
     ![Controleer de machtigingen.][28]

14. Selecteer in de Azure-portal **testverbinding** om ervoor te zorgen dat Azure AD verbinding met uw app in Google Apps maken kan. Als de verbinding is mislukt, zorg ervoor dat de Apps van Google-account Team beheerdersmachtigingen heeft. Probeer de **autoriseren** stap opnieuw.

15. Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld. Selecteer vervolgens het selectievakje in.

16. Selecteer **opslaan.**

17. Onder de **toewijzingen** sectie **synchroniseren Azure Active Directory-gebruikers met Google Apps**.

18. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die met Google Apps van Azure AD worden gesynchroniseerd. De kenmerken die zijn **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in Google Apps voor update-bewerkingen worden gebruikt. Selecteer **opslaan** eventuele wijzigingen doorvoeren.

19. Wijzigen om de Azure AD-service voor Google Apps inricht, de **inrichting Status** naar **op** in **instellingen**.

20. Selecteer **Opslaan**.

Dit proces wordt gestart voor de initiële synchronisatie van gebruikers of groepen die zijn toegewezen aan Google Apps in de sectie gebruikers en groepen. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten plaats terwijl de service wordt uitgevoerd. 

U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van rapporten van activiteiten. Deze rapporten worden alle acties die de worden uitgevoerd door de inrichting service op uw app in Google Apps beschreven.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
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
