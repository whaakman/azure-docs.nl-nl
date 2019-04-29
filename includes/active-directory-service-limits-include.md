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
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60472214"
---
Hier vindt u de beperkingen voor gebruik en andere servicelimieten voor de Azure Active Directory-service (Azure AD).

| Categorie | Limieten |
| --- | --- |
| Mappen | Eén gebruiker kan als lid of gast deel uitmaken van maximaal 500 Azure AD-directory's.<br/>Eén gebruiker kan maximaal 20 directory's maken. |
| Domeinen | U kunt niet meer dan 900 beheerde domeinnamen toevoegen. Als u al uw domeinen wilt instellen voor federatie met on-premises Active Directory, kunt u niet meer dan 450 domeinnamen toevoegen in elke directory. |
| Objecten |<ul><li>Gebruikers van de gratis versie van Azure Active Directory kunnen maximaal 500.000 objecten maken in één directory.</li><li>Niet-beheerders mogen maximaal 250 objecten maken. Zowel actieve objecten als verwijderde objecten die nog kunnen worden hersteld, tellen mee voor dit quotum. Alleen verwijderde objecten die minder dan 30 dagen geleden zijn verwijderd, kunnen nog worden hersteld. Verwijderde objecten die niet meer kunnen worden hersteld, tellen 30 dagen lang voor een kwart van de oorspronkelijke waarde mee voor het quotum. Misschien kunt u [de rol Beheerder toewijzen](../articles/active-directory/users-groups-roles/directory-assign-admin-roles.md) aan gebruikers zonder beheerdersrechten die dit quotum waarschijnlijk herhaaldelijk zullen overschrijden tijdens de uitoefening van hun normale taken.</li></ul> |
| Schema-uitbreidingen |<ul><li>Uitbreidingen van het type 'tekenreeks' mogen uit maximaal 256 tekens bestaan. </li><li>Uitbreidingen van het type 'binair' mogen uit maximaal 256 bytes bestaan.</li><li>Voor *alle* typen en *alle* toepassingen samen mogen er slechts 100 uitbreidingswaarden naar één object worden geschreven.</li><li>Alleen de entiteiten User, Group, TenantDetail, Device, Application en ServicePrincipal mogen worden uitgebreid met kenmerken met één waarde van het type 'tekenreeks' of 'binair'.</li><li>Schema-uitbreidingen zijn alleen beschikbaar in de preview van Graph API-versie 1.21. Er moet schrijftoegang worden verleend aan de toepassing om een uitbreiding te kunnen registreren.</li></ul> |
| Toepassingen |Maximaal 100 gebruikers mogen eigenaar zijn van één toepassing. |
| Groepen |<ul><li>Maximaal 100 gebruikers mogen eigenaar zijn van één groep.</li><li>Een willekeurig aantal objecten kan lid zijn van één groep.</li><li>Een gebruiker kan lid zijn van een willekeurig aantal groepen.</li><li>Het aantal leden in een groep dat u met behulp van Azure AD Connect vanaf uw on-premises Active Directory naar Azure Active Directory kunt synchroniseren, is beperkt tot 50.000.</li></ul> |
| Toegangsvenster |<ul><li>Er is geen limiet voor het aantal toepassingen dat per gebruiker kan worden weergegeven in het toegangsvenster. Dit geldt voor gebruikers met licenties voor Azure AD Premium of de Enterprise Mobility Suite.</li><li>Er worden maximaal 10 app-tegels weergegeven in het toegangsvenster voor iedere gebruiker. Deze limiet geldt voor gebruikers met licenties voor de gratis editie of de Azure AD Basic-editie van Azure Active Directory. Voorbeelden van app-tegels zijn Box, Salesforce en Dropbox. Deze limiet geldt niet voor beheerdersaccounts.</li></ul> |
| Rapporten | Er kunnen maximaal 1000 rijen worden bekeken of gedownload in elk rapport. Aanvullende gegevens worden afgekapt. |
| Beheereenheden | Een object kan lid zijn van maximaal dertig beheereenheden. |
| Beheerdersrollen en -machtigingen | <li>Een groep kan niet worden toegevoegd als eigenaar.<li>Een groep kan niet worden toegewezen aan een rol.<li>Standaard gebruikersmachtigingen kunnen niet worden gewijzigd, met uitzondering van tenantwijzigingen, die worden ingesteld als gebruikersinstelling in Azure AD. |
