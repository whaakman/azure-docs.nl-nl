---
title: Het beheren van toegang tot apps die gebruikmaken van Azure AD | Microsoft Docs
description: Hierin wordt beschreven hoe Azure Active Directory kunnen organisaties om op te geven de apps waartoe elke gebruiker toegang heeft.
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
ms.assetid: b0829f18-9e57-4107-925d-5f0457d81671
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 46e001b440802e0d5d16b7cf75344c7b9ce6fad3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="managing-access-to-apps"></a>Toegang tot apps beheren
Beheer lopende toegang, gebruik beoordeling en rapportage blijven een challenge nadat een app is geïntegreerd in uw organisatie identiteitsbeheersysteem. In veel gevallen hebben IT-beheerders of de helpdesk te nemen van een doorlopende actieve rol bij het beheren van toegang tot uw apps. Soms wordt toewijzing uitgevoerd door een algemene of divisie IT-team. Vaak wordt de beslissing toewijzing bedoeld worden gedelegeerd naar de besluitvormer vereisen van hun goedkeuring voordat IT heeft de toewijzing.  Andere organisaties Investeer in integratie met een bestaand geautomatiseerde identiteits- en toegangsbeheer management-systeem, zoals rollen gebaseerd toegangsbeheer (RBAC) of toegangsbeheer op basis van kenmerken (ABAC). De integratie en de ontwikkeling van de regel zijn meestal gespecialiseerde en duur. Controleren of rapportage over de aanpak van beide management is een eigen afzonderlijke, complexe en dure investering.

## <a name="how-does-azure-active-directory-help"></a>Hoe helpt Azure Active Directory?
 Azure AD ondersteunt uitgebreide toegangsbeheer voor geconfigureerde toepassingen, waarmee organisaties eenvoudig de juiste toegangsbeleid, variërend van automatische, op basis van kenmerken toewijzing (ABAC of RBAC scenario's) via overdracht en inclusief bereiken beheerdersbeheer. Met Azure AD kunt eenvoudig complexe beleidsregels bereiken combineren van meerdere management modellen voor één toepassing en regels voor poortbeheer opnieuw zelfs voor toepassingen met de dezelfde doelgroepen kunt gebruiken.

* [Toevoegen van nieuwe of bestaande toepassingen](active-directory-sso-integrate-saas-apps.md)

 Toewijzing van Azure AD-toepassing is gericht op twee primaire toewijzing modi:

* **Afzonderlijke toewijzing** een IT-beheerder met de globale beheerder mapmachtigingen kunt selecteren van afzonderlijke gebruikersaccounts en ze toegang verlenen tot de toepassing.
* **Toewijzing op basis van een groep (betaald alleen Azure AD)** een IT-beheerder met globale beheerdersmachtigingen voor directory een groep kunt toewijzen aan de toepassing. Specifieke gebruikers toegang is afhankelijk van of ze lid zijn van de groep op het moment dat ze proberen toegang tot de toepassing. Een beheerder kan met andere woorden, een mededeling 'alle huidige leden van de toegewezen groep toegang heeft tot de toepassing' toewijzingsregel effectief maken. Met deze optie toewijzing beheerders kunnen profiteren van een Azure AD-groep voor beheeropties, met inbegrip van [dynamische groepen op basis van kenmerken](active-directory-accessmanagement-manage-groups.md), extern systeemgroepen (bijvoorbeeld on-premises Active Directory of werkdag) of de Administrator beheerde of zelfstandige-verkeer beheerde groepen. Een groep kan worden eenvoudig toegewezen aan meerdere apps ervoor te zorgen dat toepassingen met toewijzing affiniteit toewijzingsregels, kunnen delen de algehele management complexiteit te verminderen. Houd er rekening mee dat geneste groep lidmaatschappen worden niet ondersteund voor toewijzing op basis van een groep van toepassingen op dit moment.

Met behulp van deze toewijzing van de twee modi, kunnen beheerders eventuele wenselijk toewijzing managementbenadering bereiken.

Met Azure AD is gebruiks- en rapportage over volledig geïntegreerd, waardoor beheerders gemakkelijk rapporteren over de status van sitetoewijzing toewijzingsfouten en zelfs informatie over het gebruik.

## <a name="complex-application-assignment-with-azure-ad"></a>Toewijzing van complexe toepassingen met Azure AD
U kunt een toepassing zoals Salesforce. In veel organisaties wordt Salesforce voornamelijk gebruikt door de marketing- en organisaties. Vaak uitgebreide marketing teamleden maximaal toegang tot Salesforce, terwijl de leden van het verkoopteam beperkte toegang hebben. In veel gevallen een algemene populatie informatiemedewerkers beperkte toegang hebben tot de toepassing. Uitzonderingen op deze regels bemoeilijken belangrijk is. Het is vaak het recht van de leidinggevende teams marketing- of een gebruikerstoegang verlenen of hun rollen onafhankelijk van deze algemene regels wijzigen.

Met Azure AD kunnen toepassingen zoals Salesforce zijn vooraf geconfigureerd voor eenmalige aanmelding (SSO) en de geautomatiseerde inrichting. Zodra de toepassing is geconfigureerd, kan een beheerder de eenmalige actie maken en toewijzen van de juiste groepen duren. In dit voorbeeld kan een beheerder de volgende toewijzingen uitvoeren:

* [Dynamische groepen](active-directory-accessmanagement-manage-groups.md) ter vertegenwoordiging van alle leden van de marketing- en teams met kenmerken zoals afdeling of rol automatisch kunnen worden gedefinieerd:
  
  * Alle leden van marketing groepen wordt toegewezen aan de rol 'marketing' in Salesforce
  * Alle leden van het verkoopteam groepen wordt toegewezen aan de rol 'verkoop' in Salesforce. Een verdere verfijning kan meerdere groepen die overeenkomen met regionale sales-teams toegewezen aan verschillende rollen van Salesforce gebruiken.
* Om de uitzonderingsmechanisme, kan een groepsbeheer met Self-service voor elke rol worden gemaakt. De groep 'Salesforce marketing uitzondering' kan bijvoorbeeld worden gemaakt als een groepsbeheer met Self-service. De groep kan worden toegewezen aan de rol van de Salesforce-marketing en de marketing leiding eigenaren kan worden gemaakt. Leden van de marketing leiding kunnen toevoegen of verwijderen gebruikers, stelt u een beleid join of zelfs goedkeuren of weigeren van afzonderlijke gebruikers aanvragen voor deelname aan. Dit wordt ondersteund door een information worker juiste ervaring die geen speciale training voor eigenaars of leden vereist.

In dit geval zouden alle toegewezen gebruikers worden automatisch ingericht bij Salesforce, wanneer ze worden toegevoegd aan verschillende groepen die hun roltoewijzing zou worden bijgewerkt in Salesforce. Gebruikers zou kunnen detecteren en toegang tot de Salesforce via de toepassing toegang Configuratiescherm van Microsoft Office web-clients, of zelfs door te navigeren naar de aanmeldingspagina van hun organisatie Salesforce. Beheerders zou kunnen eenvoudig bekijken en toewijzing van de status van met behulp van Azure AD-rapportage.

Beheerders kunnen gebruikmaken van [voorwaardelijke toegang van Azure AD](active-directory-conditional-access.md) beleidsregels voor toegang voor specifieke rollen instellen. Deze beleidsregels kunnen bevatten of toegang is toegestaan buiten de bedrijfsomgeving en zelfs multi-Factor Authentication vereisten of apparaten toegang in verschillende gevallen bereiken.

## <a name="how-can-i-get-started"></a>Hoe kan ik aan de slag?
Eerste, als u niet al Azure AD gebruikt en u een IT-beheerder:

* [Probeer het nu!](https://azure.microsoft.com/trial/get-started-active-directory/) -u kunt zich aanmelden voor een gratis proefversie van 30 dagen vandaag en implementeren van uw eerste cloudoplossing onder 5 minuten via deze koppeling

Azure AD-functies die delen van het account inschakelen:

* [Toewijzing van updategroep](active-directory-accessmanagement-self-service-group-management.md)
* Toepassingen toevoegen aan Azure AD
* Aan de slag met toewijzing
* Toewijzing van de toepassing Veelgestelde vragen
* [App-dashboard/gebruiksrapporten](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Waar vind ik meer informatie?
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Apps met voorwaardelijke toegang beveiligen](active-directory-conditional-access.md)
* [Groepsbeheer met Self-Service management/SSAA](active-directory-accessmanagement-self-service-group-management.md)

