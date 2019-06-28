---
title: Ontdek wanneer een specifieke gebruiker toegang krijgen tot een toepassing worden | Microsoft Docs
description: Nagaan wanneer een cruciaal gebruiker toegang heeft tot een toepassing die u hebt geconfigureerd voor het inrichten van gebruikers met Azure AD
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1fd6b70e7a4542a4cad2ee95fa280ddf8fbe6553
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67310034"
---
# <a name="check-the-status-of-user-provisioning"></a>Controleer de status van het inrichten van gebruikers

De Azure AD-inrichtingsservice wordt uitgevoerd een cyclus van een eerste inrichting op basis van het bronsysteem en doelsysteem, gevolgd door periodieke incrementele cycli. Wanneer u voor een app wordt ingericht configureert, kunt u de huidige status van de inrichtingsservice controleren en zien wanneer een gebruiker zich toegang tot een app.

## <a name="view-the-provisioning-progress-bar-preview"></a>De inrichting weer voortgangsbalk (preview)

 Op de **Provisioning** pagina voor een app, kunt u de status van de Azure AD-inrichtingsservice weergeven. De **huidige Status** sectie aan de onderkant van de pagina ziet u of een inrichting cyclus is gestart met het aanbieden van gebruikersaccounts. U kunt de voortgang van de cyclus bekijken, zien hoeveel gebruikers en groepen zijn ingericht en zien hoeveel rollen worden gemaakt.

Wanneer u het eerst automatische inrichting configureert, het **huidige Status** sectie aan de onderkant van de pagina toont de status van de eerste inrichting cyclus. In deze sectie wordt bijgewerkt telkens wanneer een cyclus van een incrementele wordt uitgevoerd. De volgende gegevens worden weergegeven:
- Het type inrichting cyclus (initiële of incrementele) die momenteel wordt uitgevoerd of het laatst is voltooid.
- Een **voortgangsbalk** met het percentage van de inrichting cyclus dat is voltooid. Het percentage geeft het aantal pagina's die zijn ingericht. Houd er rekening mee dat elke pagina bevatten kan meerdere gebruikers of groepen, zodat het percentage rechtstreeks niet met het aantal gebruikers correleert, groepen of rollen die zijn ingericht.
- Een **vernieuwen** knop kunt u de weergave bijgewerkt te houden.
- Het aantal **gebruikers** en **groepen** ingericht, en het aantal rollen die zijn gemaakt. Tijdens de eerste cyclus, de **gebruikers** getal van 1 van wordt geteld wanneer een gebruiker wordt gemaakt of bijgewerkt, en deze telt omlaag door 1 wanneer een gebruiker wordt verwijderd. Tijdens een cyclus van incrementele updates van de gebruiker niet van invloed op de **gebruikers** aantal; het verandert alleen wanneer gebruikers worden gemaakt of verwijderd.
- Een **weergave controleren logboeken** koppelen, die wordt geopend met de Azure AD-audit-logboeken voor meer informatie over alle bewerkingen die worden uitgevoerd door de gebruiker provisioning-service, met inbegrip van Inrichtingsstatus voor afzonderlijke gebruikers (Zie de [gebruik auditlogboeken](#use-audit-logs-to-check-a-users-provisioning-status)hieronder).

Nadat een inrichting cyclus voltooid is, de **statistieken op datum** sectie ziet u de cumulatieve cijfers van gebruikers en groepen die zijn ingericht tot op heden, samen met de voltooiingsdatum en de duur van de laatste cyclus. De **activiteits-ID** unieke identificatie van de meest recente inrichting cyclus. De **taak-ID** is een unieke id voor de inrichtingstaak en specifiek is voor de app in uw tenant.

De voortgang van de inrichting kunnen worden weergegeven in de Azure-portal in de **Azure Active Directory &gt; Bedrijfsapps &gt; \[toepassingsnaam\] &gt; Provisioning** tabblad.

![Inrichting van de voortgangsbalk pagina](media/application-provisioning-when-will-provisioning-finish-specific-user/provisioning-progress-bar-section.png)

## <a name="use-audit-logs-to-check-a-users-provisioning-status"></a>Auditlogboeken gebruiken om te controleren of de Inrichtingsstatus van een gebruiker

Als u wilt zien de Inrichtingsstatus voor de geselecteerde gebruiker, raadpleegt u de controlelogboeken in Azure AD. Alle bewerkingen uitgevoerd door de gebruiker inrichtingsservice worden vastgelegd in de Azure AD auditlogboeken. Dit omvat alle lees- en schrijfbewerkingen die zijn aangebracht in de bron en doel-systemen en de gebruikersgegevens die is gelezen of geschreven bij elke bewerking.

De inrichting auditlogboeken kunnen worden geopend in de Azure-portal in de **Azure Active Directory &gt; Bedrijfsapps &gt; \[toepassingsnaam\] &gt; controleren logboeken** tabblad. De logboeken filteren op de **Accountinrichting** categorie alleen de inrichten om gebeurtenissen te bekijken voor die app. U kunt zoeken naar gebruikers op basis van de "overeenkomende ID" die is geconfigureerd voor deze in de kenmerktoewijzingen. 

Bijvoorbeeld, als u de 'user principal name' of 'e-mailadres' als het overeenkomstige kenmerk aan de Azure AD geconfigureerd en de gebruiker niet ingericht heeft een waarde van "audrey@contoso.com', zoekt u de auditlogboeken voor 'audrey@contoso.com' en bekijk vervolgens vermeldingen geretourneerd.

De inrichting auditlogboeken Noteer alle bewerkingen die worden uitgevoerd door de provisioning-service, met inbegrip van:

* Een query uitvoeren op Azure AD voor toegewezen gebruikers die zich in het bereik vallen voor inrichting
* Uitvoeren van query's de doel-app voor de aanwezigheid van deze gebruikers
* Vergelijking van de gebruikersobjecten van het systeem
* Toevoegen, bijwerken of het gebruikersaccount in het doelsysteem op basis van de vergelijking uitschakelen

Zie voor meer informatie over het lezen van de controlelogboeken in Azure portal, de [inrichting handleiding over rapportering](check-status-user-account-provisioning.md).

## <a name="how-long-will-it-take-to-provision-users"></a>Hoe lang duurt het inrichten van gebruikers?
Wanneer u automatisch inrichten van gebruikers met een toepassing, Azure AD automatisch ingericht en updates van gebruikersaccounts in een app op basis van items zoals [gebruikers- en groepstoewijzing](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) op een regelmatig geplande tijdstip interval, doorgaans elke 10 minuten.

De tijd die nodig zijn voor een bepaalde gebruiker moeten worden ingericht, is afhankelijk van voornamelijk op of uw provisioning-taak wordt uitgevoerd een eerste synchronisatie of een incrementele synchronisatie.

- Voor **wordt gesynchroniseerd voor de eerste**, de tijd voor de taak is afhankelijk van verschillende factoren, waaronder het aantal gebruikers en groepen in het bereik vallen voor inrichting, en het totale aantal gebruikers en groepen in het bronsysteem. De eerste synchronisatie tussen Azure AD en een app kunt tussen 20 minuten duren tot enkele uren, afhankelijk van de grootte van de Azure AD-map en het aantal gebruikers in het bereik voor het inrichten. Een uitgebreide lijst van de factoren die invloed hebben op prestaties van de initiële synchronisatie worden later samengevat in deze sectie.

- Voor **incrementele synchronisaties** na de initiële synchronisatie, taaktijden zijn meestal sneller (bijvoorbeeld binnen 10 minuten), zoals de provisioning-service watermerken die staan voor de status van beide systemen na de initiële synchronisatie slaat, verbeterde prestaties van de volgende wordt gesynchroniseerd. De tijd voor de taak is afhankelijk van het aantal wijzigingen gedetecteerd in de inrichting altijd die cyclus. Als er minder dan 5000 gebruiker of groepslidmaatschap wijzigingen, kan de taak binnen een incrementele inrichting cyclus voltooien. 

De volgende tabel geeft een overzicht van synchronisatieschema de synchronisatietijden voor algemene inrichting scenario's. In deze scenario's, het bronsysteem is Azure AD en het doelsysteem is een SaaS-toepassing. De synchronisatietijden zijn afgeleid van een statistische analyse van synchronisatietaken voor de SaaS-toepassingen ServiceNow, werkplek, Salesforce en G Suite.


| Scopeconfiguratie | Gebruikers, groepen en leden binnen het bereik | Tijd van de initiële synchronisatie | Tijd van incrementele synchronisatie |
| -------- | -------- | -------- | -------- |
| Toegewezen gebruikers en groepen alleen synchroniseren |  < 1,000 |  < 30 minuten | < 30 minuten |
| Toegewezen gebruikers en groepen alleen synchroniseren |  1\.000 - 10.000 | 142 - 708 minuten | < 30 minuten |
| Toegewezen gebruikers en groepen alleen synchroniseren |   10,000 - 100,000 | 1,170 - 2,340 minuten | < 30 minuten |
| Alle gebruikers en groepen in Azure AD worden gesynchroniseerd |  < 1,000 | < 30 minuten  | < 30 minuten |
| Alle gebruikers en groepen in Azure AD worden gesynchroniseerd |  1\.000 - 10.000 | < 30 tot 120 minuten | < 30 minuten |
| Alle gebruikers en groepen in Azure AD worden gesynchroniseerd |  10,000 - 100,000  | 713 - 1,425 minuten | < 30 minuten |
| Alle gebruikers in Azure AD worden gesynchroniseerd|  < 1,000  | < 30 minuten | < 30 minuten |
| Alle gebruikers in Azure AD worden gesynchroniseerd | 1\.000 - 10.000  | 43 - 86 minuten | < 30 minuten |


Voor de configuratie van **synchronisatie toegewezen gebruikers en groepen alleen**, kunt u de volgende formules om te bepalen van de geschatte minimale en maximale verwacht **voor de eerste synchronisatie** tijden:

    Minimum minutes =  0.01 x [Number of assigned users, groups, and group members]
    Maximum minutes = 0.08 x [Number of assigned users, groups, and group members] 
    
Overzicht van de factoren die invloed hebben op de tijd die nodig zijn om uit te voeren een **voor de eerste synchronisatie**:

- Het totale aantal gebruikers en groepen in het bereik vallen voor inrichting.

- Het totale aantal gebruikers, groepen en groepsleden aanwezig zijn in het bronsysteem (Azure AD).

- Of gebruikers in het bereik voor het inrichten van zijn afgestemd op bestaande gebruikers in de doeltoepassing of moeten worden gemaakt voor de eerste keer. Synchronisatietaken voor dat alle gebruikers worden gemaakt voor het eerst duren over *tweemaal zo lang* als taken voor die alle gebruikers zijn afgestemd op bestaande gebruikers worden gesynchroniseerd.

- Aantal fouten in de [auditlogboeken](check-status-user-account-provisioning.md). Is trager als er veel fouten en de provisioning-service is geworden in een status in quarantaine plaatsen.    

- Aanvragen frequentielimieten en beperking geïmplementeerd door het doelsysteem. Sommige doelsystemen implementeren aanvraag frequentielimieten en beperking, die kan invloed hebben op prestaties bij grote synchronisatiebewerkingen. In deze omstandigheden kan een app die te veel aanvragen te snel ontvangt de respons vertragen of Verbreek de verbinding. Voor betere prestaties wordt de verbinding moet aanpassen door de app-aanvragen niet sneller dan ze kan worden verwerkt door de app verzendt. Inrichting connectors die zijn gemaakt door Microsoft u deze aanpassing. 

- Het aantal en de grootte van de toegewezen groepen. Toegewezen groepen synchroniseren duurt langer dan het synchroniseren van gebruikers. Zowel het aantal en de grootte van de toegewezen groepen invloed hebben op prestaties. Als een toepassing heeft [toewijzingen ingeschakeld voor synchronisatie van de groep object](customize-application-attributes.md#editing-group-attribute-mappings)en eigenschappen, zoals namen groeperen lidmaatschappen naast gebruikers worden gesynchroniseerd. Deze extra wordt gesynchroniseerd duurt langer dan alleen synchroniseren gebruikersobjecten.

## <a name="next-steps"></a>Volgende stappen
[Automate user provisioning and deprovisioning to SaaS applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) (Automatisch gebruikers inrichten en de inrichting ongedaan maken voor SaaS-toepassingen met Azure Active Directory)
