---
title: Er zijn geen gebruikers worden ingericht tot een toepassing met Azure AD-galerie | Microsoft Docs
description: Geconfronteerd met het oplossen van veelvoorkomende problemen wanneer er geen gebruikers worden weergegeven een Azure AD-galerie-toepassing die u hebt geconfigureerd voor het inrichten van gebruikers met Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/04/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: e3017612fe27ba5efbcdbf66c04d4d15641f216b
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364654"
---
# <a name="no-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Er zijn geen gebruikers worden ingericht tot een toepassing met Azure AD-galerie

Één keer automatische inrichting is geconfigureerd voor een toepassing (met inbegrip van verifiëren dat de opgegeven naar Azure AD om verbinding maken met de app app-referenties geldig zijn). Vervolgens gebruikers en/of groepen zijn ingericht met de app en wordt bepaald door de volgende zaken:

-   Welke gebruikers en groepen zijn **toegewezen** naar de toepassing. Zie voor meer informatie over toewijzing [een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

-   Wel of niet **kenmerktoewijzingen** zijn ingeschakeld en geconfigureerd voor het synchroniseren van geldige kenmerken van Azure AD aan de app. Zie voor meer informatie over kenmerktoewijzingen [aanpassen inrichting kenmerk gebruikerstoewijzingen voor de SaaS-toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

-   Ongeacht of er is of niet een **bereikfilter** aanwezig zijn die gebruikers wordt gefilterd op basis van specifieke kenmerkwaarden. Zie voor meer informatie over het bereikfilters [op kenmerken gebaseerde toepassing inrichten met bereikfilters](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

Als uw probleem dat gebruikers niet wordt ingericht, raadpleegt u de controlelogboeken in Azure AD en zoek naar logboekvermeldingen voor een specifieke gebruiker.

De inrichting auditlogboeken kunnen worden geopend in de Azure-portal in de **Azure Active Directory &gt; Bedrijfsapps &gt; \[toepassingsnaam\] &gt; controleren logboeken** tabblad. De logboeken filteren op de **Accountinrichting** categorie alleen de inrichten om gebeurtenissen te bekijken voor die app. U kunt zoeken naar gebruikers op basis van de "overeenkomende ID" die is geconfigureerd voor deze in de kenmerktoewijzingen. Bijvoorbeeld, als u de 'user principal name' of 'e-mailadres' als het overeenkomstige kenmerk aan de Azure AD geconfigureerd en de gebruiker niet ingericht heeft een waarde van "audrey@contoso.com'. Zoek de auditlogboeken voor 'audrey@contoso.com' en bekijk vervolgens vermeldingen geretourneerd.

De inrichting auditlogboeken record alle bewerkingen worden uitgevoerd door de provisioning-service, met inbegrip van Azure AD voor toegewezen gebruikers die zich binnen het bereik van de inrichting, uitvoeren van query's de doel-app voor de aanwezigheid van deze gebruikers, werkt het vergelijken van de gebruikersobjecten uitvoeren van query 's tussen het systeem. Vervolgens toevoegen, bijwerken of het gebruikersaccount in het doelsysteem op basis van de vergelijking uitschakelen.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Algemene probleemgebieden met inrichten om te overwegen

Hieronder volgt een lijst van de algemene probleemgebieden die u inzoomen kunt op als u een idee van waar u hebben wilt beginnen.

* [Inrichting van de service niet wordt weergegeven om te starten](#provisioning-service-does-not-appear-to-start)
* [Auditlogboeken dat gebruikers worden overgeslagen en niet is ingericht, zelfs als ze zijn toegewezen](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Inrichting van de service niet wordt weergegeven om te starten

Als u de **Inrichtingsstatus** moet **op** in de **Azure Active Directory &gt; Bedrijfsapps &gt; \[toepassingsnaam\] &gt;Provisioning** sectie van de Azure portal. Maar er geen andere statusdetails worden weergegeven op die pagina nadat de volgende geladen, is het waarschijnlijk dat de service wordt uitgevoerd, maar niet nog een eerste synchronisatie is voltooid. Controleer de **auditlogboeken** die hierboven worden beschreven om te bepalen welke bewerkingen die de service wordt uitgevoerd, en als er fouten zijn.

>[!NOTE]
>Een eerste synchronisatie kan duren vanaf 20 minuten tot enkele uren, afhankelijk van de grootte van de Azure AD-map en het aantal gebruikers in het bereik voor het inrichten. Volgende synchronisaties na de initiële synchronisatie zijn sneller, zoals de provisioning-service slaat watermerken die staan voor de status van beide systemen na de initiële synchronisatie. Dit verbetert de prestaties van de volgende wordt gesynchroniseerd.
>
>

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Auditlogboeken dat gebruikers worden overgeslagen en niet ingericht zelfs als ze zijn toegewezen

Wanneer een gebruiker wordt weergegeven als 'overgeslagen' in de controlelogboeken, is het heel belangrijk dat u de uitgebreide details in het logboekbericht om de reden vast te lezen. Hieronder vindt u veelvoorkomende oorzaken en oplossingen:

-   **Een bereikfilter is geconfigureerd** **die is gefilterd op de gebruiker op basis van een kenmerkwaarde**. Zie voor meer informatie over het bereikfilters <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

-   **De gebruiker is "niet effectief recht".** Als u dit bericht specifieke fout ziet, is omdat er een probleem met de gebruiker een record die zijn opgeslagen in Azure AD. Los dit probleem, toewijzing de gebruiker (of groep) uit de app en weer opnieuw toewijzen. Zie voor meer informatie over toewijzing <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

-   **Een vereist kenmerk is ontbreekt of niet ingevuld voor een gebruiker.** Een belangrijke dingen die u moet overwegen bij het instellen van de inrichting worden om te controleren en configureren van de kenmerktoewijzingen en werkstromen die welke gebruiker (of groep) eigenschappen stroom van Azure AD aan de toepassing definiëren. Dit omvat instellen van de 'overeenkomende eigenschap"die worden gebruikt voor het uniek identificeren en overeenkomen met de gebruikers/groepen tussen de twee systemen. Zie voor meer informatie over dit belangrijk proces [aanpassen inrichting kenmerk gebruikerstoewijzingen voor de SaaS-toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

  * **Kenmerktoewijzingen voor groepen:** inrichting van de groepsnaam en de Groepsdetails, naast de leden, als dit wordt ondersteund voor sommige toepassingen. U kunt inschakelen of uitschakelen van deze functionaliteit met in- of uitschakelen de **toewijzing** voor groepsobjecten worden weergegeven in de **Provisioning** tabblad. Als u groepen inrichten is ingeschakeld, worden de kenmerktoewijzingen om te controleren of dat het juiste veld wordt gebruikt voor de "overeenkomende ID" Lees. Dit is de weergegeven naam of e-mailalias), zoals de groep en de bijbehorende leden niet worden ingericht als de overeenkomende eigenschap leeg of niet ingevuld voor een groep in Azure AD is.

## <a name="next-steps"></a>Volgende stappen
[Azure AD Connect-synchronisatie: inzicht in declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md)

