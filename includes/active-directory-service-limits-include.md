---
title: bestand opnemen
description: bestand opnemen
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 02/19/2019
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: 619dd7f3b01e2e7ce71e945fce77aa73cb87f264
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56443386"
---
Dit zijn de beperkingen voor gebruik en andere Servicelimieten voor de service Azure Active Directory (Azure AD).

| Categorie | Limieten |
| --- | --- |
| Mappen | Een enkele gebruiker kan deel uitmaken van maximaal 500 Azure AD-mappen als lid of Gast.<br/>Een enkele gebruiker kan maximaal 20 mappen maken. |
| Domeinen | U kunt niet meer dan 900 beheerde domeinnamen toevoegen. Als u alle domeinen voor federatie met on-premises Active Directory instelt bent, kunt u niet meer dan 450 domeinnamen toevoegen in elke map. |
| Objecten |<ul><li>Een maximum van 500.000 objecten kan in één map worden gemaakt door gebruikers van de gratis versie van Azure Active Directory.</li><li>Niet-beheerders mogen maximaal 250 objecten maken.</li></ul> |
| Schema-uitbreidingen |<ul><li>Uitbreidingen van het type 'tekenreeks' mogen uit maximaal 256 tekens bestaan. </li><li>Uitbreidingen van het type 'binair' mogen uit maximaal 256 bytes bestaan.</li><li>Er mogen maximaal 100 uitbreidingswaarden (voor ALLE typen en ALLE toepassingen) worden geschreven naar één object.</li><li>Alleen de entiteiten User, Group, TenantDetail, Device, Application en ServicePrincipal mogen worden uitgebreid met kenmerken met één waarde van het type 'tekenreeks' of 'binair'.</li><li>Schema-uitbreidingen zijn alleen beschikbaar in de Graph API-versie 1.21-preview. Er moet schrijftoegang worden verleend aan de toepassing om een uitbreiding te kunnen registreren.</li></ul> |
| Applicaties |Een maximum van 100 gebruikers mogen eigenaar zijn van één toepassing. |
| Groepen |<ul><li>Een maximum van 100 gebruikers mogen eigenaar zijn van één groep.</li><li>Een willekeurig aantal objecten kan lid zijn van één groep.</li><li>Een gebruiker kan lid zijn van een willekeurig aantal groepen zijn.</li><li>Het aantal leden in een groep die u kunt synchroniseren vanaf uw on-premises Active Directory naar Azure Active Directory met Azure AD Connect is beperkt tot 50 K leden.</li></ul> |
| Toegangsvenster |<ul><li>Er is geen limiet voor het aantal toepassingen dat te zien is in het toegangsvenster per eindgebruiker, per aan een gebruiker toegewezen licentie voor Azure AD Premium en per Enterprise Mobility Suite.</li><li>Maximaal 10 app-tegels (voorbeelden: Box, Salesforce of Dropbox) kan worden weergegeven in het toegangsvenster voor elke eindgebruiker voor gebruikers die gratis licenties zijn toegewezen of Azure AD Basic edities van Azure Active Directory. Deze limiet geldt niet voor beheerdersaccounts.</li></ul> |
| Rapporten | Er kunnen maximaal 1000 rijen worden bekeken of gedownload in elk rapport. Aanvullende gegevens worden afgekapt. |
| Beheereenheden | Een object kan lid zijn van maximaal dertig beheereenheden. |
| Beheerdersrollen en machtigingen | <li>Een groep kan niet worden toegevoegd als een eigenaar<li>Een groep kan niet worden toegewezen aan een rol<li>Standaard gebruikersmachtigingen buiten de tenant-switches (gebruikersinstellingen in Azure AD) wijzigen niet |
