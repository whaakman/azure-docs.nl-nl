---
title: Er zijn geen gebruikers worden ingericht tot een toepassing met Azure AD-galerie | Microsoft Docs
description: Het oplossen van veelvoorkomende problemen met het er geen gebruikers worden weergegeven in een Azure AD-galerie toepassing die u hebt geconfigureerd voor het inrichten van gebruikers met Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: fd5d833656d4ba395857c84ee8386bc7fb6d704e
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54465393"
---
# <a name="no-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Er zijn geen gebruikers worden ingericht tot een toepassing met Azure AD-galerie
Nadat de automatische inrichting is geconfigureerd voor een toepassing (met inbegrip van verifiëren dat de opgegeven naar Azure AD om verbinding maken met de app app-referenties geldig zijn), zijn vervolgens gebruikers en/of groepen ingericht met de app. Inrichting wordt bepaald door de volgende zaken:

-   Welke gebruikers en groepen zijn **toegewezen** naar de toepassing. Zie voor meer informatie over toewijzing [een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](assign-user-or-group-access-portal.md).
-   Wel of niet **kenmerktoewijzingen** zijn ingeschakeld en geconfigureerd voor het synchroniseren van geldige kenmerken van Azure AD aan de app. Zie voor meer informatie over kenmerktoewijzingen [aanpassen inrichting kenmerk gebruikerstoewijzingen voor de SaaS-toepassingen in Azure Active Directory](customize-application-attributes.md).
-   Ongeacht of er is of niet een **bereikfilter** aanwezig zijn die gebruikers wordt gefilterd op basis van specifieke kenmerkwaarden. Zie voor meer informatie over het bereikfilters [op kenmerken gebaseerde toepassing inrichten met bereikfilters](define-conditional-rules-for-provisioning-user-accounts.md).
  
Als u merkt dat gebruikers niet wordt ingericht, raadpleegt u de controlelogboeken in Azure AD. Zoeken naar vermeldingen voor een specifieke gebruiker.

De inrichting auditlogboeken kunnen worden geopend in de Azure-portal in de **Azure Active Directory &gt; Bedrijfsapps &gt; \[toepassingsnaam\] &gt; controleren logboeken** tabblad. De logboeken filteren op de **Accountinrichting** categorie alleen de inrichten om gebeurtenissen te bekijken voor die app. U kunt zoeken naar gebruikers op basis van de "overeenkomende ID" die is geconfigureerd voor deze in de kenmerktoewijzingen. Bijvoorbeeld, als u de 'user principal name' of 'e-mailadres' als het overeenkomstige kenmerk aan de Azure AD geconfigureerd en de gebruiker niet ingericht heeft een waarde van "audrey@contoso.com', zoekt u de auditlogboeken voor 'audrey@contoso.com' en bekijk de vermeldingen geretourneerd.

De inrichting auditlogboeken record alle bewerkingen worden uitgevoerd door de provisioning-service, met inbegrip van Azure AD voor toegewezen gebruikers die zich binnen het bereik van de inrichting, uitvoeren van query's de doel-app voor de aanwezigheid van deze gebruikers, werkt het vergelijken van de gebruikersobjecten uitvoeren van query 's tussen het systeem. Vervolgens toevoegen, bijwerken of het gebruikersaccount in het doelsysteem op basis van de vergelijking uitschakelen.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Algemene probleemgebieden met inrichten om te overwegen
Hieronder volgt een lijst van de algemene probleemgebieden die u inzoomen kunt op als u een idee van waar u hebben wilt beginnen.

- [Inrichting van de service niet wordt weergegeven om te starten](#provisioning-service-does-not-appear-to-start)
- [Auditlogboeken dat gebruikers worden overgeslagen en niet is ingericht, zelfs als ze zijn toegewezen](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Inrichting van de service niet wordt weergegeven om te starten
Als u de **Inrichtingsstatus** moet **op** in de **Azure Active Directory &gt; Bedrijfsapps &gt; \[toepassingsnaam\] &gt;Provisioning** sectie van de Azure portal. Maar er geen andere statusdetails worden weergegeven op die pagina nadat de volgende geladen, is het waarschijnlijk dat de service wordt uitgevoerd, maar niet nog een eerste synchronisatie is voltooid. Controleer de **auditlogboeken** die hierboven worden beschreven om te bepalen welke bewerkingen die de service wordt uitgevoerd, en als er fouten zijn.

>[!NOTE]
>Een eerste synchronisatie kan duren vanaf 20 minuten tot enkele uren, afhankelijk van de grootte van de Azure AD-map en het aantal gebruikers in het bereik voor het inrichten. Volgende synchronisaties na de initiële synchronisatie zijn sneller, zoals de provisioning-service slaat watermerken die staan voor de status van beide systemen na de initiële synchronisatie. De eerste synchronisatie verbetert de prestaties van de volgende wordt gesynchroniseerd.
>


## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Auditlogboeken dat gebruikers worden overgeslagen en niet ingericht zelfs als ze zijn toegewezen

Wanneer een gebruiker wordt weergegeven als 'overgeslagen' in de controlelogboeken, is het belangrijk dat u de uitgebreide details in het logboekbericht om de reden vast te lezen. Hieronder vindt u veelvoorkomende oorzaken en oplossingen:

- **Een bereikfilter is geconfigureerd** **die is gefilterd op de gebruiker op basis van een kenmerkwaarde**. Zie voor meer informatie over het bereikfilters [bereikfilters](define-conditional-rules-for-provisioning-user-accounts.md).
- **De gebruiker is "niet effectief recht".** Als u dit bericht specifieke fout ziet, is omdat er een probleem met de gebruiker een record die zijn opgeslagen in Azure AD. Los dit probleem op door de gebruiker (of groep) uit de app intrekken en opnieuw toewijzen. Zie voor meer informatie over toewijzing [gebruiker of groep toegang toewijzen](assign-user-or-group-access-portal.md).
- **Een vereist kenmerk is ontbreekt of niet ingevuld voor een gebruiker.** Een belangrijke dingen om te overwegen bij het instellen van de inrichting is om te controleren en configureren van de kenmerktoewijzingen en werkstromen die welke gebruiker (of groep) eigenschappen stroom van Azure AD aan de toepassing definiëren. Deze configuratie omvat het instellen de 'overeenkomende eigenschap"dat wordt gebruikt voor unieke identificatie en overeenkomen met de gebruikers/groepen tussen de twee systemen. Zie voor meer informatie over dit belangrijk proces [aanpassen inrichting kenmerk gebruikerstoewijzingen voor de SaaS-toepassingen in Azure Active Directory](customize-application-attributes.md).
- **Kenmerktoewijzingen voor groepen:** Inrichting van de groepsnaam en de Groepsdetails, naast de leden, als dit wordt ondersteund voor sommige toepassingen. U kunt inschakelen of uitschakelen van deze functionaliteit met in- of uitschakelen de **toewijzing** voor groepsobjecten worden weergegeven in de **Provisioning** tabblad. Als u groepen inrichten is ingeschakeld, worden de kenmerktoewijzingen om te controleren of dat het juiste veld wordt gebruikt voor de "overeenkomende ID" Lees. De overeenkomende ID is de naam of e-mailalias van weergave. De groep en de bijbehorende leden zijn niet ingericht als de overeenkomende eigenschap leeg of niet ingevuld voor een groep in Azure AD is.

## <a name="next-steps"></a>Volgende stappen

[Azure AD Connect-synchronisatie: Informatie over declaratieve inrichting](../hybrid/concept-azure-ad-connect-sync-declarative-provisioning.md)
