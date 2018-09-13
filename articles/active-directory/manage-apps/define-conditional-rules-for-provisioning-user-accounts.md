---
title: Inrichten van apps met bereikfilters | Microsoft Docs
description: Informatie over het gebruik van bereikfilters toevoegen om te voorkomen dat objecten in apps die ondersteuning bieden voor geautomatiseerde gebruikersinrichting van wordt ingericht als een object niet voldoet aan uw zakelijke vereisten.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: barbkess
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b62679332a45127d7c1a45593c162182886a7150
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44719355"
---
# <a name="attribute-based-application-provisioning-with-scoping-filters"></a>Op kenmerken gebaseerde toepassing inrichten met bereikfilters
Het doel van dit artikel is waarin wordt uitgelegd hoe u bereikfilters die zijn gebruikt om op kenmerken gebaseerde regels die bepalen welke gebruikers worden ingericht voor een toepassing te definiëren.

## <a name="scoping-filter-use-cases"></a>Scoping filter use-cases

Een bereikfilter kunt de Azure Active Directory (Azure AD) inrichtingsservice wilt opnemen of uitsluiten van alle gebruikers die een kenmerk dat overeenkomt met een specifieke waarde hebben. Bijvoorbeeld bij het inrichten van gebruikers van Azure AD voor een SaaS-toepassing die wordt gebruikt door een verkoopteam, kunt u opgeven dat alleen gebruikers met een kenmerk 'Afdeling' van 'Verkoop' moet binnen het bereik voor het inrichten van.

Bereikfilters kan anders worden gebruikt, afhankelijk van het type inrichting connector:

* **Uitgaande via Azure AD in te richten SaaS-toepassingen**. Wanneer Azure AD het bronsysteem [gebruikers en groepen toewijzingen](assign-user-or-group-access-portal.md) zijn de meest gebruikte methode voor het bepalen van welke gebruikers zijn binnen het bereik voor het inrichten. Deze toewijzingen worden gebruikt voor het inschakelen van eenmalige aanmelding ook en bieden een één methode voor het beheren van toegang en de inrichting. Bereikfilters kan worden gebruikt (optioneel), naast toewijzingen of in plaats van ze voor het filteren van gebruikers op basis van kenmerkwaarden.

    >[!TIP]
    > U kunt uitschakelen inrichten op basis van toewijzingen voor een zakelijke toepassing door het wijzigen van instellingen in de [bereik](user-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) menu onder de inrichting instellingen **alle gebruikers en groepen synchroniseren**. Met deze optie plus bereikfilters op kenmerken gebaseerde biedt snellere prestaties dan het gebruik van toewijzingen op basis van een groep.  

* **Inkomende inrichten van HCM-toepassingen naar Azure AD en Active Directory**. Wanneer een [HCM toepassing zoals Workday](../saas-apps/workday-tutorial.md) is het bronsysteem bereikfilters worden de primaire methode voor het bepalen van welke gebruikers moeten worden ingericht vanuit de toepassing HCM naar Active Directory of Azure AD.

Standaard hoeft Azure AD-inrichting connectors niet op kenmerken gebaseerde bereikfilters die zijn geconfigureerd. 

## <a name="scoping-filter-construction"></a>Scoping filter constructie

Een bereikfilter bestaat uit een of meer *componenten*. Componenten bepalen welke gebruikers zijn toegestaan om door te geven via de bereikfilter door het evalueren van de kenmerken van elke gebruiker. Bijvoorbeeld, mogelijk hebt u een van de componenten die vereist dat een gebruikerskenmerk 'State' is gelijk aan 'Antwerpen', zodat alleen gebruikers van de New York zijn ingericht in de toepassing. 

Één component definieert één voorwaarde voor een enkel kenmerk-waarde. Als meerdere componenten zijn gemaakt in een enkele bereikfilter, werkt ze samen geëvalueerd met behulp van en-logica. Dit betekent dat alle componenten moeten worden geëvalueerd tot 'true' in order voor een gebruiker moet worden ingericht.

Ten slotte kunnen meerdere bereikfilters worden gemaakt voor één toepassing. Als er meerdere bereikfilters aanwezig zijn, worden ze samen geëvalueerd met behulp van of-logica. Dit betekent dat als de componenten in een van de geconfigureerde bereikfilters evalueren op 'true', de gebruiker is ingericht.

Elke gebruiker of groep verwerkt door de Azure AD-inrichtingsservice worden is altijd afzonderlijk geëvalueerd op basis van elke bereikfilter.

Houd rekening met de volgende bereikfilter als een voorbeeld:

![Bereikfilter](./media/define-conditional-rules-for-provisioning-user-accounts/scoping-filter.PNG) 

Gebruikers moeten voldoen aan de volgende criteria om te worden ingericht op basis van deze bereikfilter:

* Ze moeten zich in New York.
* Ze moeten werken in de technische afdeling.
* Hun bedrijf werknemer-ID moet tussen 1.000.000 en 2.000.000.
* De functie moet niet null of leeg zijn.

## <a name="create-scoping-filters"></a>Bereikfilters maken
Bereikfilters zijn geconfigureerd als onderdeel van de kenmerktoewijzingen voor elke Azure AD-gebruiker connector wordt ingericht. De volgende procedure wordt ervan uitgegaan dat u al automatische inrichting instellen voor [een van de ondersteunde toepassingen](../saas-apps/tutorial-list.md) en een bereikfilter toe toevoegt.

### <a name="create-a-scoping-filter"></a>Maken van een bereikfilter
1. In de [Azure-portal](https://portal.azure.com), gaat u naar de **Azure Active Directory** > **bedrijfstoepassingen** > **alle toepassingen** sectie.

2. Selecteer de toepassing waarvoor u automatische inrichting hebt geconfigureerd: bijvoorbeeld 'ServiceNow'.

3. Selecteer de **Provisioning** tabblad.

4. In de **toewijzingen** sectie, selecteert u de toewijzing die u wilt configureren van een bereikfilter voor: bijvoorbeeld 'synchroniseren Azure Active Directory: gebruikers aan ServiceNow'.

5. Selecteer de **bron objectbereik** menu.

6. Selecteer **bereikfilter toevoegen**.

7. Een component definiëren door het selecteren van een bron **kenmerknaam**, een **Operator**, en een **kenmerkwaarde** voor het vergelijken van. De volgende operators worden ondersteund:

   a. **IS GELIJK AAN**. Component retourneert 'true' als de geëvalueerde kenmerk precies overeenkomen met de ingevoerde tekenreeks-waarde (hoofdlettergevoelig).

   b. **NIET GELIJK IS AAN**. Component retourneert 'true' als de geëvalueerde kenmerk komt niet overeen met de ingevoerde tekenreeks-waarde (hoofdlettergevoelig).

   c. **IS INGESTELD OP TRUE**. Component retourneert 'true' als de geëvalueerde kenmerk een Booleaanse waarde ' True ' geretourneerd bevat.

   d. **IS INGESTELD OP FALSE**. Component retourneert 'true' als de geëvalueerde kenmerk Booleaanse waarde false bevat.

   e. **IS NULL**. Component retourneert 'true' als de geëvalueerde kenmerk leeg is.

   f. **IS NIET NULL**. Component retourneert 'true' als de geëvalueerde kenmerk niet leeg zijn.

   g. **REGULIERE EXPRESSIE OVEREENKOMST**. Component retourneert 'true' als de geëvalueerde kenmerk overeenkomt met een reguliere-expressiepatroon. Bijvoorbeeld: ([1-9][0-9]) komt overeen met een willekeurig getal tussen 10 en 99.

   h. **NIET OVEREEN MET REGULIERE EXPRESSIE**. Component retourneert 'true' als de geëvalueerde kenmerk komt niet overeen met een reguliere-expressiepatroon.

8. Selecteer **nieuwe scope component toevoegen**.

9. (Optioneel) Herhaal stap 7-8 als u wilt meer bereikcomponenten toevoegen.

10. In **titel Scoping**, een naam voor het bereikfilter toevoegen.

11. Selecteer **OK**.

12. Selecteer **OK** opnieuw op de **Scoping Filters** scherm. (Optioneel) Herhaal stap 6-11 als een ander bereik filter wilt toevoegen.

13. Selecteer **opslaan** op de **kenmerk toewijzing** scherm. 

>[!IMPORTANT] 
> Een nieuwe scope filter triggers wordt een nieuwe volledige synchronisatie uit voor de toepassing, waarbij alle gebruikers in het bronsysteem opnieuw worden geëvalueerd op basis van de nieuwe bereikfilter opgeslagen. Als een gebruiker in de toepassing eerder binnen het bereik van de inrichting, maar valt buiten het bereik was, wordt hun account uitgeschakeld of in de toepassing de inrichting ongedaan gemaakt.


## <a name="related-articles"></a>Verwante artikelen:
* [Gebruiker-inrichting en ongedaan maken van inrichting voor SaaS-toepassingen automatiseren](user-provisioning.md)
* [Kenmerktoewijzingen voor het inrichten van gebruikers aanpassen](customize-application-attributes.md)
* [Expressies schrijven voor kenmerktoewijzingen](functions-for-customizing-application-data.md)
* [Meldingen over accountinrichting](user-provisioning.md)
* [SCIM gebruiken om in te schakelen automatische inrichting van gebruikers en groepen uit Azure Active Directory voor toepassingen](use-scim-to-provision-users-and-groups.md)
* [Lijst met zelfstudies over het integreren van SaaS-apps](../saas-apps/tutorial-list.md)

