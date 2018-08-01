---
title: bestand opnemen
description: bestand opnemen
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 07/30/2018
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: a2055c3f00306fbfdad3028a16bb49d919e1e251
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39361428"
---
Dit zijn de beperkingen voor gebruik en andere Servicelimieten voor de service Azure Active Directory (Azure AD).

| Categorie | Limieten |
| --- | --- |
| Mappen | Een enkele gebruiker kan deel uitmaken van maximaal 500 Azure AD-mappen als lid of Gast.<br/>Een enkele gebruiker kan maximaal 20 mappen maken. |
| Domeinen | U kunt niet meer dan 900 beheerde domeinnamen toevoegen. Als u alle domeinen voor federatie met on-premises Active Directory instelt bent, kunt u niet meer dan 450 domeinnamen toevoegen in elke map. |
| Objecten |<ul><li>Een maximum van 500.000 objecten kan in één map worden gemaakt door gebruikers van de gratis versie van Azure Active Directory.</li><li>Niet-beheerders mogen maximaal 250 objecten maken.</li></ul> |
| Schema-uitbreidingen |<ul><li>Uitbreidingen van het type 'tekenreeks' mogen uit maximaal 256 tekens bestaan. </li><li>Uitbreidingen van het type 'binair' mogen uit maximaal 256 bytes bestaan.</li><li>Er mogen maximaal 100 uitbreidingswaarden (voor ALLE typen en ALLE toepassingen) worden geschreven naar één object.</li><li>Alleen de entiteiten User, Group, TenantDetail, Device, Application en ServicePrincipal mogen worden uitgebreid met kenmerken met één waarde van het type 'tekenreeks' of 'binair'.</li><li>Schema-uitbreidingen zijn alleen beschikbaar in de Graph API-versie 1.21-preview. Er moet schrijftoegang worden verleend aan de toepassing om een uitbreiding te kunnen registreren.</li></ul> |
| Toepassingen |Een maximum van 100 gebruikers mogen eigenaar zijn van één toepassing. |
| Groepen |<ul><li>Een maximum van 100 gebruikers mogen eigenaar zijn van één groep.</li><li>Een willekeurig aantal objecten kan lid zijn van één groep in Azure Active Directory.</li><li>Het aantal leden in een groep die u kunt synchroniseren vanaf uw on-premises Active Directory naar Azure Active Directory met Azure AD Connect is beperkt tot 50 K leden.</li></ul> |
| Toegangsvenster |<ul><li>Er is geen limiet voor het aantal toepassingen dat te zien is in het toegangsvenster per eindgebruiker, per aan een gebruiker toegewezen licentie voor Azure AD Premium en per Enterprise Mobility Suite.</li><li>Er zijn maximaal 10 app-tegels (voor bijvoorbeeld Box, Salesforce of Dropbox) te zien in het toegangspaneel voor elke eindgebruiker, voor gebruikers met een gratis licentie en voor Azure AD Basic-versies van Azure Active Directory. Deze limiet geldt niet voor beheerdersaccounts.</li></ul> |
| Rapporten | Er kunnen maximaal 1000 rijen worden bekeken of gedownload in elk rapport. Aanvullende gegevens worden afgekapt. |
| Beheereenheden | Een object kan lid zijn van maximaal dertig beheereenheden. |
