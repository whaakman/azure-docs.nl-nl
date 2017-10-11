---
title: Er zijn geen gebruikers zijn ingericht op een galerie van Azure AD-toepassing | Microsoft Docs
description: Het oplossen van veelvoorkomende problemen geconfronteerd wanneer er geen gebruikers die voorkomen een an Azure AD-galerie-toepassing die u hebt geconfigureerd voor gebruikers inrichten met Azure AD
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: asteen
ms.openlocfilehash: 30db71c0706de1dcc0d48fb72fa5d62475a14ba7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="no-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Er zijn geen gebruikers zijn ingericht op een galerie van Azure AD-toepassing

Wanneer automatische inrichting is geconfigureerd voor een toepassing (inclusief controleren of de opgegeven naar Azure AD verbinding maken met de app app-referenties geldig zijn). Vervolgens gebruikers en/of groepen op de app zijn ingericht en wordt bepaald door de volgende zaken:

-   Die gebruikers en groepen zijn **toegewezen** tot de toepassing. Zie voor meer informatie over toewijzing [een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

-   Wel of niet **kenmerktoewijzingen** zijn ingeschakeld en geconfigureerd voor het synchroniseren van geldige kenmerken vanuit Azure AD naar de app. Zie voor meer informatie over kenmerktoewijzingen [aanpassen gebruiker inrichting kenmerktoewijzingen voor SaaS-toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

-   Of er is al dan niet een **bereik filter** aanwezig die gebruikers wordt gefilterd op basis van specifieke kenmerkwaarden. Zie voor meer informatie over het bereikfilters [inrichten van toepassing op basis van kenmerken met bereikfilters](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

Wanneer observeren gebruikers niet worden ingericht, Raadpleeg de controlelogboeken in Azure AD en zoek naar logboekvermeldingen voor een specifieke gebruiker.

De inrichting controlelogboeken kunnen worden geopend in de Azure-portal in de **Azure Active Directory &gt; zakelijke Apps &gt; \[toepassingsnaam\] &gt; Audit logboeken** tabblad. Filter de logboeken op de **Account inrichten** categorie alleen de inrichting om gebeurtenissen te bekijken voor die app. U kunt zoeken naar gebruikers op basis van de 'overeenkomende ID' die is geconfigureerd voor deze in de kenmerktoewijzingen. Bijvoorbeeld, als u de 'user principal name' of 'e-mailadres' als het overeenkomende kenmerk aan de kant van de Azure AD hebt geconfigureerd en de gebruiker niet inrichten heeft de waarde 'audrey@contoso.com'. Zoek vervolgens de auditlogboeken voor 'audrey@contoso.com' en bekijk vervolgens vermeldingen geretourneerd.

De inrichting record alle bewerkingen uitgevoerd door de inrichting service, inclusief Azure AD voor toegewezen gebruikers die zich binnen het bereik van de inrichting, opvragen van de doel-app voor de aanwezigheid van deze gebruikers, vergelijken van de gebruikersobjecten opvragen voor de auditlogboeken tussen het systeem. Vervolgens toevoegen, bijwerken of het gebruikersaccount in het doelsysteem op basis van de vergelijking uitschakelen.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Algemene probleemgebieden met het leveren rekening moet houden

Hieronder volgt een lijst van de algemene probleemgebieden die u inzoomen kunt in als u een idee van waar u wilt beginnen.

* [Inrichting van de service niet starten](#provisioning-service-does-not-appear-to-start)
* [Controlelogboeken dat gebruikers worden overgeslagen en niet is ingericht, zelfs als ze worden toegewezen](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Inrichting van de service niet starten

Als u de **inrichting Status** worden **op** in de **Azure Active Directory &gt; zakelijke Apps &gt; \[toepassingsnaam\] &gt;Inrichten** sectie van de Azure-portal. Echter geen andere statusgegevens worden weergegeven op deze pagina nadat het volgende wordt opnieuw geladen, is het waarschijnlijk dat de service wordt uitgevoerd, maar niet nog een initiële synchronisatie is voltooid. Controleer de **controlelogboeken** die hierboven worden beschreven om te bepalen welke bewerkingen die de service wordt uitgevoerd, en als er geen fouten bevat.

>[!NOTE]
>Een initiële synchronisatie kan overal van 20 minuten tot enkele uren duren, afhankelijk van de grootte van de Azure AD-directory en het aantal gebruikers in het bereik voor het inrichten. Volgende synchronisaties na de eerste synchronisatie zijn sneller, zoals de inrichting service slaat watermerken die de status van beide systemen na de eerste synchronisatie vertegenwoordigen. Dit verbetert de prestaties van de volgende synchronisatie.
>
>

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Controlelogboeken dat gebruikers worden overgeslagen en niet is ingericht, hoewel ze zijn toegewezen

Wanneer een gebruiker wordt weergegeven als 'overgeslagen' in de controlelogboeken, is het belangrijk dat u de uitgebreide details in het logboekbericht om de reden vast te lezen. Hieronder vindt u veelvoorkomende oorzaken en oplossingen:

-   **Een filter scope is geconfigureerd** **die wordt uitgefilterd de gebruiker op basis van een kenmerkwaarde**. Zie voor meer informatie over het bereikfilters <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

-   **De gebruiker is 'niet effectief heeft het recht'.** Als u dit specifieke foutbericht ziet, is dit omdat er een probleem met de gebruikersrecord toewijzing opgeslagen in Azure AD. Los dit probleem, toewijzing de gebruiker (of groep) uit de app en opnieuw opnieuw toewijzen. Zie voor meer informatie over toewijzing <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

-   **Een vereist kenmerk is ontbreekt of niet ingevuld voor een gebruiker.** Een belangrijkste waarmee u rekening moet houden bij het instellen van de inrichting worden om te bekijken en configureren van de kenmerktoewijzingen en werkstromen die welke gebruiker (of groep) eigenschappen stroom van Azure AD voor de toepassing definiëren. Dit omvat de 'overeenkomende eigenschap' instelt met worden gebruikt voor unieke identificatie dient en overeen met gebruikers/groepen tussen de twee systemen. Zie voor meer informatie over dit proces belangrijk [aanpassen gebruiker inrichting kenmerktoewijzingen voor SaaS-toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

  * **Kenmerktoewijzingen voor groepen:** inrichten van de groepsnaam en de groepsgegevens, naast de leden, als dit wordt ondersteund op bepaalde toepassingen. U kunt inschakelen of uitschakelen van deze functionaliteit met in- of uitschakelen de **toewijzing** voor een groepsobjecten worden weergegeven in de **inrichten** tabblad. Als inrichting groepen is ingeschakeld, worden de kenmerktoewijzingen om te controleren of dat het juiste veld wordt gebruikt voor de 'overeenkomende ID' Lees. Dit is de weergegeven naam of e-mailalias), zoals de groep en de bijbehorende leden niet worden ingericht als de overeenkomende eigenschap leeg of niet ingevuld voor een groep in Azure AD is.

## <a name="next-steps"></a>Volgende stappen
[Azure AD Connect-synchronisatie: inzicht declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md)

