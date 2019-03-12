---
title: bestand opnemen
description: bestand opnemen
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 02/21/2019
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: 38f2dd301ddc2a5f8d28322856b2011bd2034c30
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554698"
---
Dit zijn de beperkingen voor gebruik en andere Servicelimieten voor de service Azure Active Directory (Azure AD).

| Categorie | Limieten |
| --- | --- |
| Mappen | Een enkele gebruiker kan deel uitmaken van maximaal 500 Azure AD-mappen als lid of Gast.<br/>Een enkele gebruiker kan maximaal 20 mappen maken. |
| Domeinen | U kunt niet meer dan 900 beheerde domeinnamen toevoegen. Als u van alle domeinen voor federatie met on-premises Active Directory instelt, kunt u niet meer dan 450 domeinnamen toevoegen in elke map. |
| Objecten |<ul><li>Een maximum van 500.000 objecten kan in één map worden gemaakt door gebruikers van de gratis versie van Azure Active Directory.</li><li>Niet-beheerders mogen maximaal 250 objecten maken. Zowel active-objecten en verwijderde objecten die beschikbaar zijn voor het herstellen van tellen mee voor dit quotum. Alleen verwijderde objecten die zijn verwijderd is minder dan 30 dagen geleden zijn beschikbaar om te herstellen. Verwijderde objecten die niet langer beschikbaar is voor het herstellen van tellen mee voor deze quota bij een waarde van een kwart gedurende 30 dagen. Misschien [een beheerdersrol toewijzen](../articles/active-directory/users-groups-roles/directory-assign-admin-roles.md) voor gebruikers van niet-beheerders die waarschijnlijk herhaaldelijk dit quotum voor uitoefening van hun normale taken.</li></ul> |
| Schema-uitbreidingen |<ul><li>Tekenreekstype extensies mag maximaal 256 tekens hebben. </li><li>Uitbreidingen van het binaire type zijn beperkt tot 256 bytes.</li><li>Slechts 100 extensie waarden over meerdere *alle* typen en *alle* toepassingen kunnen worden geschreven naar één object.</li><li>Alleen entiteiten van gebruiker, groep, TenantDetail, apparaat, toepassing en Service-Principal kunnen worden uitgebreid met het type tekenreeks of binaire bestanden van het type kenmerken met één waarde.</li><li>Schema-uitbreidingen zijn alleen beschikbaar in de Graph API-versie 1.21-preview. Er moet schrijftoegang worden verleend aan de toepassing om een uitbreiding te kunnen registreren.</li></ul> |
| Applicaties |Een maximum van 100 gebruikers mogen eigenaar zijn van één toepassing. |
| Groepen |<ul><li>Een maximum van 100 gebruikers mogen eigenaar zijn van één groep.</li><li>Een willekeurig aantal objecten kan lid zijn van één groep.</li><li>Een gebruiker kan lid zijn van een willekeurig aantal groepen zijn.</li><li>Het aantal leden in een groep die u vanaf uw on-premises Active Directory naar Azure Active Directory synchroniseren kunt met behulp van Azure AD Connect is beperkt tot 50.000 leden.</li></ul> |
| Toegangsvenster |<ul><li>Er is geen limiet voor het aantal toepassingen die kunnen worden weergegeven in het toegangsvenster per gebruiker. Dit geldt voor de gebruiker toegewezen licentie voor Azure AD Premium of Enterprise Mobility Suite.</li><li>In het toegangsvenster voor iedere gebruiker maximaal 10 app-tegels weergegeven. Deze limiet geldt voor gebruikers die zijn toegewezen licenties gratis of Azure AD Basic edities van Azure Active Directory. Voorbeelden van app-tegels zijn Box, Salesforce of Dropbox. Deze limiet geldt niet voor beheerdersaccounts.</li></ul> |
| Rapporten | Er kunnen maximaal 1000 rijen worden bekeken of gedownload in elk rapport. Aanvullende gegevens worden afgekapt. |
| Beheereenheden | Een object kan lid zijn van maximaal dertig beheereenheden. |
| Beheerdersrollen en machtigingen | <li>Een groep kan niet worden toegevoegd als een eigenaar.<li>Een groep kan niet worden toegewezen aan een rol.<li>Standaard gebruiker machtigingen kunnen niet worden gewijzigd, met uitzondering van tenant-switches, die instellingen in Azure AD zijn. |
