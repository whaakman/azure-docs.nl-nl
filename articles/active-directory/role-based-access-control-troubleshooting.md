---
title: Problemen met Azure RBAC | Microsoft Docs
description: Hulp bij problemen of vragen hebt over op rollen gebaseerd toegangsbeheer resources.
services: azure-portal
documentationcenter: na
author: andredm7
manager: femila
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 407c030ea159915d4d7ac21760a3d17ec2204372
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="role-based-access-control-troubleshooting"></a>Probleemoplossing voor toegangsbeheer op basis van rollen

In dit artikel document antwoorden op veelgestelde vragen over de specifieke rechten die zijn verleend aan rollen, zodat u wat u weet kunt verwachten wanneer u de rollen in de Azure-portal en kan toegangsproblemen oplossen. Deze drie rollen hebben betrekking op alle brontypen:

* Eigenaar  
* Inzender  
* Lezer  

Eigenaars en medewerkers hebt volledige toegang tot de beheerervaring, maar een medewerker kan geen toegang verlenen aan andere gebruikers of groepen. Dingen ophalen iets interessanter met de lezersrol zodat waar we even uitgeeft. Zie de [toegangsbeheer op basis van rollen get-started artikel](role-based-access-control-configure.md) voor meer informatie over het om toegang te verlenen.

## <a name="app-service-workloads"></a>App service-werkbelastingen
### <a name="write-access-capabilities"></a>Toegang voor schrijven-mogelijkheden
Als u een gebruiker alleen-lezen toegang tot een enkel web-app verlenen, worden sommige functies zijn uitgeschakeld dat u niet verwacht. De volgende beheermogelijkheden vereisen **schrijven** toegang tot een web-app (Inzender of eigenaar) en niet beschikbaar zijn in een scenario met alleen-lezen.

* Opdrachten (zoals starten, stoppen, enz.)
* Wijzigen van de instellingen, zoals algemene configuratie, schaalinstellingen, back-upinstellingen en controle-instellingen
* Toegang tot publishing referenties en andere geheime informatie zoals het app-instellingen en verbindingsreeksen
* Streaminglogboeken
* Configuratie van diagnostische logboeken
* Console (opdrachtprompt)
* Actieve en recente implementaties (voor lokale git-continue implementatie)
* Geschatte besteden
* Webtests
* Virtueel netwerk (alleen zichtbaar voor een lezer als eerder een virtueel netwerk is geconfigureerd door een gebruiker met toegang voor schrijven).

Als u geen toegang deze tegels tot, moet u de systeembeheerder voor Inzender toegang tot de web-app.

### <a name="dealing-with-related-resources"></a>Omgaan met verwante bronnen
Web-apps worden door de aanwezigheid van een paar verschillende bronnen die wisselwerking ingewikkeld. Hier volgt een typisch resourcegroep met een paar websites:

![Web-app-resourcegroep](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Als gevolg hiervan, als u iemand toegang tot alleen de web-app veel van de functionaliteit op de websiteblade in de Azure portal is uitgeschakeld.

Deze items vereisen **schrijven** toegang tot de **App Service-abonnement** die overeenkomt met uw website:  

* Weergeven van de web-app de prijscategorie (vrije of standaard)  
* Configuratie van de schaal (aantal exemplaren, de grootte van virtuele machine, de instellingen voor automatisch schalen)  
* Quota (opslag, bandbreedte, CPU)  

Deze items vereisen **schrijven** toegang tot het gehele **resourcegroep** die uw website bevat:  

* SSL-certificaten en bindingen (SSL-certificaten kunnen worden gedeeld tussen sites in dezelfde resourcegroep en de geografische locatie)  
* Regels voor waarschuwingen  
* instellingen voor automatisch schalen  
* Application insights-onderdelen  
* Webtests  

## <a name="virtual-machine-workloads"></a>Virtual machine-werkbelasting
Veel zoals met web-apps, sommige functies op de blade van de virtuele machine vereist schrijftoegang op de virtuele machine, of op andere bronnen in de resourcegroep.

Virtuele machines zijn gerelateerd aan het domein namen, virtuele netwerken, opslagaccounts en regels voor waarschuwingen.

Deze items vereisen **schrijven** toegang tot de **virtuele machine**:

* Eindpunten  
* IP-adressen  
* Disks  
* Extensies  

Hiervoor is vereist **schrijven** toegang tot zowel de **virtuele machine**, en de **resourcegroep** (samen met de domeinnaam) dat deze zich in:  

* Beschikbaarheidsset  
* Laden van de set met gelijke taakverdeling  
* Regels voor waarschuwingen  

Als u geen toegang deze tegels tot, vraag uw beheerder voor Inzender toegang tot de resourcegroep.

## <a name="see-more"></a>Meer informatie
* [Op rollen gebaseerd toegangsbeheer](role-based-access-control-configure.md): aan de slag met RBAC in Azure portal.
* [Ingebouwde rollen](role-based-access-built-in-roles.md): meer informatie over de functies die standaard in RBAC.
* [Aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md): informatie over het maken van aangepaste rollen aan uw behoeften toegang.
* [Maken van een geschiedenisrapport voor gewijzigde van toegang](role-based-access-control-access-change-history-report.md): bijhouden van wijzigen van roltoewijzingen in RBAC.

