---
title: 'Zelfstudie: ZenDesk configureren voor het automatisch gebruikers inrichten met Azure Active Directory | Microsoft Docs'
description: Informatie over het configureren van Azure Active Directory voor het automatisch inrichten en gebruikersaccounts aan ZenDesk ongedaan in te richten.
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: 1a1414eefd20e6d7c025da08cfd5ae7c45daad33
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-configuring-zendesk-for-automatic-user-provisioning"></a>Zelfstudie: ZenDesk configureren voor het automatisch gebruikers inrichten


Het doel van deze zelfstudie is zodat u de stappen die u wilt uitvoeren in de ZenDesk en Azure AD automatisch inrichten en inrichten van gebruikersaccounts vanuit Azure AD naar ZenDesk ongedaan. 

## <a name="prerequisites"></a>Vereisten

Het scenario in deze zelfstudie wordt ervan uitgegaan dat u al de volgende items hebt:

*   Een Azure Active directory-tenant
*   Een ZenDesk-tenant met de [enterpriseplan](https://www.zendesk.com/product/pricing/) of beter ingeschakeld 
*   Een gebruikersaccount in de ZenDesk met beheerdersmachtigingen 

> [!NOTE]
> De Azure AD integratie inrichting is afhankelijk van de [ZenDesk REST-API](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), die beschikbaar zijn voor ZenDesk-teams in de essentiële plan of hoger is.

## <a name="assigning-users-to-zendesk"></a>Gebruikers toewijzen aan ZenDesk

Azure Active Directory gebruikt een concept 'toewijzingen' genoemd om te bepalen welke gebruikers krijgen toegang tot geselecteerde apps. In de context van automatische gebruikers account inrichten, worden alleen de gebruikers en groepen die '' tot een toepassing in Azure AD toegewezen zijn gesynchroniseerd. 

Voordat u configureren en inschakelen van de inrichting service, moet u bepalen welke gebruikers en/of groepen in Azure AD vertegenwoordigen de gebruikers die toegang nodig tot uw ZenDesk-app. Als besloten, kunt u deze gebruikers toewijzen aan uw ZenDesk-app door de volgende instructies te volgen:

[Een gebruiker of groep toewijzen aan een enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Belangrijke tips voor het toewijzen van gebruikers aan ZenDesk

*   Het is raadzaam om één Azure AD-gebruiker is toegewezen aan ZenDesk voor het testen van de configuratie van de inrichting. Extra gebruikers en/of groepen kunnen later worden toegewezen.

*   Wanneer een gebruiker aan ZenDesk toewijzen, moet u ofwel de **gebruiker** functie of een andere geldige toepassingsspecifieke-rol (indien beschikbaar) in het dialoogvenster toewijzing. De **standaardtoegang** rol werkt niet voor het inrichten en deze gebruikers worden overgeslagen.

> [!NOTE]
> Als een toegevoegde functie en de inrichting service eventuele aangepaste rollen gedefinieerd in de Zendesk leest en importeert ze in Azure AD waarin ze kunnen worden geselecteerd in het dialoogvenster rol selecteren. Deze rollen zijn zichtbaar in de Azure portal nadat de inrichting-service is ingeschakeld en een synchronisatiecyclus is voltooid.

## <a name="configuring-user-provisioning-to-zendesk"></a>Gebruikersaanvragen voor ZenDesk configureren 

Deze sectie helpt u bij uw Azure AD verbinden met de ZenDesk-gebruikersaccount inrichten API en configureren van de inrichting service te maken, bijwerken en uitschakelen van toegewezen gebruikersaccounts in de ZenDesk op basis van gebruikers en groepen toewijzen in Azure AD.

> [!TIP] 
> U kunt ook op basis van SAML eenmalige aanmelding is ingeschakeld voor ZenDesk, vindt u de instructies te volgen in [Azure-portal](https://portal.azure.com). Eenmalige aanmelding kan worden geconfigureerd onafhankelijk van automatische inrichting, hoewel deze twee functies aanvulling van elkaar.


### <a name="configure-automatic-user-account-provisioning-to-zendesk-in-azure-ad"></a>Automatische account gebruikersaanvragen naar ZenDesk in Azure AD configureren


1. In de [Azure-portal](https://portal.azure.com), blader naar de **Azure Active Directory > zakelijke Apps > alle toepassingen** sectie.

2. Als u al ZenDesk hebt geconfigureerd voor eenmalige aanmelding, zoekt u uw exemplaar van ZenDesk met behulp van het zoekveld. Selecteer anders **toevoegen** en zoek naar **ZenDesk** in de galerie met toepassingen. ZenDesk selecteert in de zoekresultaten en toe te voegen aan uw lijst met toepassingen.

3. Selecteer uw ZenDesk-exemplaar en selecteer vervolgens de **inrichten** tabblad.

4. Stel de **Inrichtingsmodus** naar **automatische**.

    ![ZenDesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk1.png)

5. Onder de **beheerdersreferenties** sectie, voer de **Admin Username & tokenkey & domein** die worden gegenereerd door uw ZenDesk-account (u vindt het token onder uw account: **Admin**  >  **API** > **instellingen**). 

    ![ZenDesk-inrichting](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk2.png)

6. Klik in de Azure-portal op **testverbinding** om te controleren of Azure AD, kan verbinding maken met uw ZenDesk-app. Als de verbinding is mislukt, zorg ervoor dat uw ZenDesk-account beheerdersmachtigingen heeft en probeer het opnieuw stap 5.

7. Voer het e-mailadres van een persoon of groep die in inrichting fout meldingen moet ontvangen de **e-mailmelding** veld en schakel het selectievakje "Een e-mailmelding verzenden wanneer een fout optreedt."

8. Klik op **Opslaan**. 

9. Selecteer onder de sectie toewijzingen **synchroniseren Azure Active Directory-gebruikers ZenDesk**.

10. In de **kenmerktoewijzingen** sectie, moet u de kenmerken van de gebruiker is gesynchroniseerd vanuit Azure AD ZenDesk controleren. De kenmerken die zijn geselecteerd als **overeenkomend** eigenschappen overeenkomen met de gebruikersaccounts in de ZenDesk voor update-bewerkingen worden gebruikt. Selecteer de knop Opslaan eventuele wijzigingen doorvoeren.

11. Om de Azure AD voor ZenDesk-service inricht, wijzigen de **inrichting Status** naar **op** in de **instellingen** sectie

12. Klik op **Opslaan**. 

Deze bewerking begint de initiële synchronisatie van gebruikers en/of groepen die zijn toegewezen aan ZenDesk in de sectie gebruikers en groepen. De eerste synchronisatie langer duren om uit te voeren dan het volgende wordt gesynchroniseerd, die ongeveer 20 minuten optreden als de service wordt uitgevoerd. U kunt de **synchronisatiedetails** sectie voortgang en volg de koppelingen voor het inrichten van de activiteitsrapporten, waarin alle acties die worden uitgevoerd door de inrichting service beschrijven.

Zie voor meer informatie over het lezen van de Azure AD inrichting logboeken [rapportage over automatische account gebruikersaanvragen](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Aanvullende bronnen

* [Het beheren van gebruikers account inrichten voor zakelijke Apps](active-directory-enterprise-apps-manage-provisioning.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Volgende stappen

* [Informatie over het bekijken van Logboeken en rapporten over het inrichten van de activiteit ophalen](active-directory-saas-provisioning-reporting.md)
