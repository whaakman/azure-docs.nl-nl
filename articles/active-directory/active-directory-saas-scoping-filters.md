---
title: Apps met bereikfilters inrichten | Microsoft Docs
description: Informatie over het bereik filters gebruiken om te voorkomen dat de objecten in apps die ondersteuning bieden voor geautomatiseerde gebruikersinrichting van ingericht als een object niet voldoet aan uw zakelijke vereisten.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: bcfbda74-e4d4-4859-83bc-06b104df3918
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e7a2322239945a529a544054c2273e37a3d65abf
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a>Inrichten van toepassing op basis van kenmerken met bereikfilters
Het doel van dit artikel is legt uit hoe bereik filters gebruiken om te bepalen op kenmerken gebaseerde regels die bepalen welke gebruikers zijn ingericht voor een toepassing.

## <a name="scoping-filter-use-cases"></a>Filter gebruiksvoorbeelden scoping

Een scope filter kunt de Azure Active Directory (Azure AD)-service inricht wilt opnemen of uitsluiten van gebruikers die een kenmerk dat overeenkomt met een specifieke waarde hebben. Bijvoorbeeld bij het inrichten van een SaaS-toepassing die wordt gebruikt door een verkoopteam gebruikers van Azure AD, kunt u alleen gebruikers met een kenmerk 'Afdeling' van 'Verkoop' moet binnen het bereik van de inrichting.

Bereikfilters kan anders worden gebruikt, afhankelijk van het type inrichting connector:

* **Uitgaande inrichten van Azure AD SaaS-toepassingen**. Wanneer u Azure AD is het bronsysteem [toewijzingen van gebruikers en groepen](active-directory-coreapps-assign-user-azure-portal.md) zijn de meest voorkomende methode om te bepalen welke gebruikers in het bereik voor het inrichten. Deze toewijzingen worden ook worden gebruikt voor het inschakelen van eenmalige aanmelding en bieden een één methode voor het beheren van toegang en de inrichting. Bereikfilters kan worden gebruikt eventueel naast toewijzingen of in plaats van ze, voor het filteren van gebruikers op basis van kenmerkwaarden.

    >[!TIP]
    > U kunt uitschakelen inrichten op basis van de toewijzingen voor een zakelijke toepassing door het wijzigen van instellingen in de [bereik](active-directory-saas-app-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) menu onder de inrichting instellingen **synchroniseren van alle gebruikers en groepen**. Met deze optie plus bereik filters op basis van kenmerken, biedt betere prestaties dan wanneer u toewijzingen op basis van een groep.  

* **Inkomende inrichting van HCM toepassingen naar Azure AD en Active Directory**. Wanneer een [HCM toepassing zoals Workday](active-directory-saas-workday-tutorial.md) is het bronsysteem bereik filters zijn de primaire methode voor het bepalen van welke gebruikers moeten worden ingericht vanuit de toepassing HCM naar Active Directory of Azure AD.

Standaard hoeft Azure AD-inrichting connectors niet op basis van het kenmerk SCOPE filters geconfigureerd. 

## <a name="scoping-filter-construction"></a>Filter constructie scoping

Een scope filter bestaat uit een of meer *componenten*. Componenten bepalen welke gebruikers kunnen het bereik filter doorgeven door elke gebruiker kenmerken te evalueren. Bijvoorbeeld wellicht een component die vereist dat een gebruikerskenmerk 'Status' is gelijk aan 'New York', zodat alleen Den Haag gebruikers zijn ingericht in de toepassing. 

Één component definieert één voorwaarde voor een enkele kenmerkwaarde. Als meerdere componenten in één scope filter worden gemaakt, zijn ze samen geëvalueerd met behulp van 'En' logica. Dit betekent dat alle componenten moeten worden geëvalueerd op 'true' in volgorde van een gebruiker moeten worden ingericht.

Ten slotte kunnen meerdere scope filters worden gemaakt voor één toepassing. Als meerdere scope filters aanwezig zijn, zijn ze samen geëvalueerd met behulp van 'Of' logica. Dit betekent dat als de componenten in een van de geconfigureerde bereik filters evalueren op 'true', de gebruiker is ingericht.

Elke gebruiker of groep verwerkt door de service van Azure AD-inrichting worden wordt altijd afzonderlijk geëvalueerd tegen elke scope filter.

Overweeg bijvoorbeeld het volgende bereik filter:

![Bereikfilter](./media/active-directory-saas-scoping-filters/scoping-filter.PNG) 

Gebruikers moeten voldoen aan de volgende criteria om te worden ingericht volgens dit bereik filter:

* Ze moeten zich in New York.
* Ze moeten in de afdeling Engineering werken.
* Werknemer-ID van hun bedrijf moet liggen tussen 1.000.000 en 2.000.000.
* De functie mag geen null of leeg.

## <a name="create-scoping-filters"></a>Bereik filters maken
Bereik filters zijn geconfigureerd als onderdeel van de kenmerktoewijzingen voor elke Azure AD-gebruikers inrichten van connector. De volgende procedure wordt ervan uitgegaan dat u al ingesteld voor een automatische inrichting [een van de ondersteunde toepassingen](active-directory-saas-tutorial-list.md) en het toevoegen van een scope filter aan.

### <a name="create-a-scoping-filter"></a>Maken van een scope filter
1. In de [Azure-portal](https://portal.azure.com), gaat u naar de **Azure Active Directory** > **bedrijfstoepassingen** > **alle toepassingen** sectie.

2. Selecteer de toepassing waarvoor u automatische inrichting hebt geconfigureerd: bijvoorbeeld 'ServiceNow'.

3. Selecteer de **inrichten** tabblad.

4. In de **toewijzingen** sectie, selecteert u de toewijzing die u wilt configureren voor een scope-filter: bijvoorbeeld 'synchroniseren Azure Active Directory: gebruikers aan ServiceNow'.

5. Selecteer de **bron objectbereik** menu.

6. Selecteer **scope-filter toevoegen**.

7. Een component definiëren door te selecteren van een bron **kenmerknaam**, een **Operator**, en een **kenmerkwaarde** voor het vergelijken van. De volgende operators worden ondersteund:

   a. **IS GELIJK AAN**. Component retourneert 'true' als de geëvalueerde kenmerk precies overeenkomen met de waarde voor de invoer (hoofdlettergevoelig).

   b. **NIET GELIJK AAN**. Component retourneert 'true' als de geëvalueerde kenmerk komt niet overeen met de waarde invoerreeks (hoofdlettergevoelig).

   c. **IS INGESTELD OP TRUE**. Component retourneert 'true' als de geëvalueerde kenmerk een Booleaanse waarde waar bevat.

   d. **IS INGESTELD OP FALSE**. Component retourneert 'true' als de geëvalueerde kenmerk een Booleaanse waarde ONWAAR bevat.

   e. **NULL**. Component retourneert 'true' als de geëvalueerde kenmerk leeg is.

   f. **IS GEEN NULL**. Component retourneert 'true' als de geëvalueerde kenmerk niet leeg zijn.

   g. **REGEX-OVEREENKOMST**. Component retourneert 'true' als de geëvalueerde kenmerk overeenkomt met een reguliere-expressiepatroon. Bijvoorbeeld: ([1-9][0-9]) komt overeen met een willekeurig getal tussen 10 en 99.

   h. **NIET OVEREEN MET REGULIERE EXPRESSIE**. Component retourneert 'true' als de geëvalueerde kenmerk komt niet overeen met een reguliere-expressiepatroon.

8. Selecteer **toevoegen van de nieuwe scope component**.

9. Optioneel, herhaalt u stap 7-8 als u wilt meer bereik componenten toevoegen.

10. In **Scoping filtertitel**, Voeg een naam voor uw filter bereik toe.

11. Selecteer **OK**.

12. Selecteer **OK** opnieuw op de **Scoping Filters** scherm. Eventueel Herhaal stap 6-11 als een ander bereik filter wilt toevoegen.

13. Selecteer **opslaan** op de **kenmerk toewijzing** scherm. 

>[!IMPORTANT] 
> Een nieuwe scope filter triggers wordt een nieuwe volledige synchronisatie voor de toepassing, waarbij alle gebruikers in het bronsysteem opnieuw worden geëvalueerd op basis van de nieuwe scope filter opgeslagen. Als een gebruiker in de toepassing eerder binnen het bereik van de inrichting, maar valt buiten het bereik was, hun account is uitgeschakeld of in de toepassing gemaakt.


## <a name="related-articles"></a>Verwante artikelen:
* [Artikel index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
* [Automatisch gebruikers inrichten en opheffen van inrichting tot SaaS-toepassingen](active-directory-saas-app-provisioning.md)
* [Kenmerktoewijzingen voor gebruikers inrichten aanpassen](active-directory-saas-customizing-attribute-mappings.md)
* [Expressies voor kenmerktoewijzingen schrijven](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Meldingen inrichten van een account](active-directory-saas-account-provisioning-notifications.md)
* [SCIM gebruiken om in te schakelen automatische inrichting van gebruikers en groepen van Azure Active Directory voor toepassingen](active-directory-scim-provisioning.md)
* [Lijst met zelfstudies over het integreren van SaaS-apps](active-directory-saas-tutorial-list.md)

