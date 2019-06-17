---
title: bestand opnemen
description: bestand opnemen
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 05/22/2019
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: 067280cdad85f59106bce5ff214e2fa9eddf3b71
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133416"
---
Hier vindt u de beperkingen voor gebruik en andere servicelimieten voor de Azure Active Directory-service (Azure AD).

| Category | Limieten |
| --- | --- |
| Mappen | Eén gebruiker kan als lid of gast deel uitmaken van maximaal 500 Azure AD-directory's.<br/>Eén gebruiker kan maximaal 20 directory's maken. |
| Domeinen | U kunt niet meer dan 900 beheerde domeinnamen toevoegen. Als u al uw domeinen wilt instellen voor federatie met on-premises Active Directory, kunt u niet meer dan 450 domeinnamen toevoegen in elke directory. |
| Objecten |<ul><li>Gebruikers van de gratis versie van Azure Active Directory kunnen standaard maximaal 50.000 objecten maken in één directory. Als u minstens een geverifieerd domein hebt, wordt het standaard directoryservicequotum in Azure AD uitgebreid naar 300.000 objecten. </li><li>Niet-beheerders mogen maximaal 250 objecten maken. Zowel actieve objecten als verwijderde objecten die nog kunnen worden hersteld, tellen mee voor dit quotum. Alleen verwijderde objecten die minder dan 30 dagen geleden zijn verwijderd, kunnen nog worden hersteld. Verwijderde objecten die niet meer kunnen worden hersteld, tellen 30 dagen lang voor een kwart van de oorspronkelijke waarde mee voor het quotum. Misschien kunt u [de rol Beheerder toewijzen](../articles/active-directory/users-groups-roles/directory-assign-admin-roles.md) aan gebruikers zonder beheerdersrechten die dit quotum waarschijnlijk herhaaldelijk zullen overschrijden tijdens de uitoefening van hun normale taken.</li></ul> |
| Schema-uitbreidingen |<ul><li>Uitbreidingen van het type 'tekenreeks' mogen uit maximaal 256 tekens bestaan. </li><li>Uitbreidingen van het type 'binair' mogen uit maximaal 256 bytes bestaan.</li><li>Voor *alle* typen en *alle* toepassingen samen mogen er slechts 100 uitbreidingswaarden naar één object worden geschreven.</li><li>Alleen de entiteiten User, Group, TenantDetail, Device, Application en ServicePrincipal mogen worden uitgebreid met kenmerken met één waarde van het type 'tekenreeks' of 'binair'.</li><li>Schema-uitbreidingen zijn alleen beschikbaar in de preview van Graph API-versie 1.21. Er moet schrijftoegang worden verleend aan de toepassing om een uitbreiding te kunnen registreren.</li></ul> |
| Toepassingen |Maximaal 100 gebruikers mogen eigenaar zijn van één toepassing. |
| Groepen |<ul><li>Maximaal 100 gebruikers mogen eigenaar zijn van één groep.</li><li>Een willekeurig aantal objecten kan lid zijn van één groep.</li><li>Een gebruiker kan lid zijn van een willekeurig aantal groepen.</li><li>Het aantal leden in een groep dat u met behulp van Azure AD Connect vanaf uw on-premises Active Directory naar Azure Active Directory kunt synchroniseren, is beperkt tot 50.000.</li></ul> |
| Toepassingsproxy | <ul><li>Maximaal 500 transacties per seconde per App voor een toepassingsproxy</li><li>Maximaal 750 transacties per seconde voor de tenant</li></ul><br/>Een transactie is gedefinieerd als één http-aanvraag en respons voor een unieke resource. Als beperkt, ontvangen clients een fout 429 (te veel aanvragen). |
| Toegangsvenster |<ul><li>Er is geen limiet voor het aantal toepassingen dat per gebruiker kan worden weergegeven in het toegangsvenster. Dit geldt voor gebruikers met licenties voor Azure AD Premium of de Enterprise Mobility Suite.</li><li>Er worden maximaal 10 app-tegels weergegeven in het toegangsvenster voor iedere gebruiker. Deze limiet geldt voor gebruikers met licenties voor de gratis editie of de Azure AD Basic-editie van Azure Active Directory. Voorbeelden van app-tegels zijn Box, Salesforce en Dropbox. Deze limiet geldt niet voor beheerdersaccounts.</li></ul> |
| Rapporten | Er kunnen maximaal 1000 rijen worden bekeken of gedownload in elk rapport. Aanvullende gegevens worden afgekapt. |
| Beheereenheden | Een object kan lid zijn van maximaal dertig beheereenheden. |
| Beheerdersrollen en -machtigingen | <ul><li>Een groep kan niet worden toegevoegd als een [eigenaar](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context#object-ownership).</li><li>Een groep kan niet worden toegewezen aan een [rol](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-assign-admin-roles).</li><li>Gebruikers de mogelijkheid om te lezen van directory-informatie voor andere gebruikers kan niet worden beperkt buiten de tenant-brede switch om uit te schakelen van alle niet-beheerders gebruikers toegang tot alle directorygegevens (niet aanbevolen) zijn. Meer informatie over de standaardmachtigingen [hier](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context#to-restrict-the-default-permissions-for-member-users).</li><li>Het duurt maximaal 15 minuten of ondertekening out/aanmelden voordat u beheerder rol lidmaatschap toevoegingen en intrekking van kracht.</li></ul> |
