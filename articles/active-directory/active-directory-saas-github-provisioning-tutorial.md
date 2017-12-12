---
title: 'Zelfstudie: GitHub configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts met GitHub ongedaan in te richten.
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: mtillman
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: 7e202899b697872227da4d7bea54406f99089000
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configuring-github-for-automatic-user-provisioning"></a>Zelfstudie: GitHub configureren voor het automatisch gebruikers inrichten


Het doel van deze zelfstudie is zodat u de stappen die u uitvoeren in GitHub en Azure AD wilt om automatisch inrichten en de gebruikersaccounts van Azure AD met GitHub ongedaan in te richten. 

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant
*   Een Github-tenant met de [bedrijfsplan](https://help.github.com/articles/organization-billing-plans/#business-plan) of beter ingeschakeld 
*   Een gebruikersaccount in GitHub met beheerdersmachtigingen 

> [!NOTE]
> De Azure AD integratie inrichting is afhankelijk van de [GitHub SCIM API](https://developer.github.com/v3/scim/), die beschikbaar zijn voor Github teams op het bedrijfsplan of hoger is.

## <a name="assigning-users-to-github"></a>Gebruikers toewijzen aan GitHub

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichting service, moet u om te bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang nodig tot uw app in GitHub. Als besloten, kunt u deze gebruikers toewijzen aan uw GitHub-app door de volgende instructies te volgen:

[Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-github"></a>Belangrijke tips voor het toewijzen van gebruikers met GitHub

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan GitHub voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Bij het toewijzen van een gebruiker met GitHub, moet u ofwel de **gebruiker** functie of een andere geldige toepassingsspecifieke-rol (indien beschikbaar) in het dialoogvenster toewijzing. De **standaardtoegang** rol werkt niet voor het inrichten en deze gebruikers worden overgeslagen.


## <a name="configuring-user-provisioning-to-github"></a>Configuratie van gebruikers inrichten met GitHub 

Deze sectie helpt u bij uw Azure AD verbinden met de GitHub-gebruikersaccount inrichten API en configureren van de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in GitHub op basis van gebruikers en groepen toewijzen in Azure AD.

> [!TIP]
> U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor GitHub, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.


### <a name="configure-automatic-user-account-provisioning-to-github-in-azure-ad"></a>Automatisch gebruikers account inrichten met GitHub in Azure AD configureren


1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

2. Als u al GitHub voor eenmalige aanmelding hebt geconfigureerd, kunt u zoeken naar uw exemplaar van het zoekveld met GitHub. Selecteer anders **toevoegen** en zoek naar **GitHub** in de galerie met toepassingen. GitHub selecteert in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw exemplaar van GitHub en selecteer vervolgens de **inrichten** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![Het inrichten van GitHub](./media/active-directory-saas-github-provisioning-tutorial/GitHub1.png)

5. Onder de **beheerdersreferenties** sectie, klikt u op **autoriseren**. Deze bewerking wordt een dialoogvenster met GitHub autorisatie in een nieuw browservenster geopend. 

6. Aanmelden bij uw beheerdersaccount met GitHub in het nieuwe venster. Selecteer in de resulterende autorisatie dialoogvenster de GitHub-team dat u inschakelen wilt voor inrichting en selecteer vervolgens **autoriseren**. Als voltooid, terug naar de Azure-portal om het inrichtingsproces configuratie te voltooien.

    ![Dialoogvenster autorisatie](./media/active-directory-saas-github-provisioning-tutorial/GitHub2.png)

7. Voer in de Azure portal **Tenant-URL** en klik op **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw GitHub-app. Als de verbinding is mislukt, zorg ervoor dat uw GitHub-account beheerdersmachtigingen heeft en **Tenant-URl** is opgegeven correct en probeer het opnieuw de stap 'Autoriseren' (u kunt vormen **Tenant-URL** door regel: 'https://api.github.com/scim/v2/organizations/ + < Organizations_name >', kunt u uw organisaties vinden onder uw GitHub-account: **instellingen** > **organisaties**).

    ![Dialoogvenster autorisatie](./media/active-directory-saas-github-provisioning-tutorial/GitHub3.png)

8. Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje "Een e-mailmelding verzenden wanneer een fout optreedt."

9. Klik op **Opslaan**. 

10. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-gebruikers met GitHub**.

11. In de **kenmerktoewijzingen** sectie, controleert u de kenmerken van de gebruiker die zijn gesynchroniseerd vanuit Azure AD met GitHub. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in GitHub voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

12. Om de Azure AD-service voor GitHub inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie

13. Klik op **Opslaan**. 

Deze bewerking begint de initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan GitHub in de sectie gebruikers en groepen. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service beschrijven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Aanvullende bronnen

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Volgende stappen

* [Informatie over het bekijken van Logboeken en rapporten over het inrichten van de activiteit ophalen](active-directory-saas-provisioning-reporting.md)
