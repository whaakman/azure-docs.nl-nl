---
title: bestand opnemen
description: bestand opnemen
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 06/20/2018
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: 5529a4510b6d9f005dc46165e1be799585e5a153
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37100841"
---
Hier vindt u de beperkingen voor gebruik en andere servicelimieten voor de Azure Active Directory-service.

| Category | Limieten |
| --- | --- |
| Mappen |Een enkele gebruiker kan worden gekoppeld met een maximum van 500 mappen van Azure Active Directory zijn.<br />Voorbeelden van mogelijke combinaties: <ul> <li>Een enkele gebruiker maakt 500 mappen.</li><li>Een enkele gebruiker is toegevoegd aan 500 mappen als een lid.</li><li>Een enkele gebruiker 250 mappen maakt en later aan wordt toegevoegd door anderen 250 andere mappen.</li></ul> |
| Domeinen | U kunt niet meer dan 900 beheerde domeinnamen toevoegen. Als u alle domeinen voor federatie met lokale Active Directory instellen wilt, kunt u niet meer dan 450 domeinnamen toevoegen in elke map. |
| Objecten |<ul><li>Maximaal 500.000 objecten kan worden gemaakt in een enkele map door gebruikers van de editie Free van Azure Active Directory.</li><li>Niet-beheerders mogen maximaal 250 objecten maken.</li></ul> |
| Schema-uitbreidingen |<ul><li>Uitbreidingen van het type 'tekenreeks' mogen uit maximaal 256 tekens bestaan. </li><li>Uitbreidingen van het type 'binair' mogen uit maximaal 256 bytes bestaan.</li><li>Er mogen maximaal 100 uitbreidingswaarden (voor ALLE typen en ALLE toepassingen) worden geschreven naar één object.</li><li>Alleen de entiteiten User, Group, TenantDetail, Device, Application en ServicePrincipal mogen worden uitgebreid met kenmerken met één waarde van het type 'tekenreeks' of 'binair'.</li><li>Schema-uitbreidingen zijn alleen beschikbaar in de Graph API-versie 1.21-preview. Er moet schrijftoegang worden verleend aan de toepassing om een uitbreiding te kunnen registreren.</li></ul> |
| Toepassingen |Maximaal 100 gebruikers kunt eigenaar zijn van één toepassing. |
| Groepen |<ul><li>Maximaal 100 gebruikers kunt eigenaar zijn van één groep.</li><li>Een willekeurig aantal objecten kan lid zijn van één groep in Azure Active Directory.</li><li>Het aantal leden in een groep dat u kunt synchroniseren vanaf uw on-premises Active Directory naar Azure Active Directory is beperkt tot 15.000 (met gebruik van Active Directory-directorysynchronisatie (DirSync)).</li><li>Het aantal leden in een groep dat u kunt synchroniseren vanaf uw on-premises Active Directory naar Azure Active Directory met Azure AD Connect is beperkt tot 50.000.</li></ul> |
| Toegangsvenster |<ul><li>Er is geen limiet voor het aantal toepassingen dat te zien is in het toegangsvenster per eindgebruiker, per aan een gebruiker toegewezen licentie voor Azure AD Premium en per Enterprise Mobility Suite.</li><li>Er zijn maximaal 10 app-tegels (voor bijvoorbeeld Box, Salesforce of Dropbox) te zien in het toegangspaneel voor elke eindgebruiker, voor gebruikers met een gratis licentie en voor Azure AD Basic-versies van Azure Active Directory. Deze limiet geldt niet voor beheerdersaccounts.</li></ul> |
| Rapporten | Er kunnen maximaal 1000 rijen worden bekeken of gedownload in elk rapport. Aanvullende gegevens worden afgekapt. |
| Beheereenheden | Een object kan lid zijn van maximaal dertig beheereenheden. |
