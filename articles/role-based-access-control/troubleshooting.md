---
title: Het oplossen van RBAC in Azure | Microsoft Docs
description: Problemen oplossen met op rollen gebaseerd toegangsbeheer in Azure (RBAC).
services: azure-portal
documentationcenter: na
author: rolyon
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: role-based-access-control
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: 186bcf26639f5cff2dcbf1e805913ac7edab7df4
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37437362"
---
# <a name="troubleshooting-rbac-in-azure"></a>Het oplossen van RBAC in Azure

In dit artikel vindt u antwoorden op veelgestelde vragen over op rollen gebaseerd toegangsbeheer (RBAC), zodat u wat weet u kunt verwachten wanneer u de rollen in de Azure-portal en kan problemen met toegang. Deze drie rollen hebben betrekking op alle resourcetypen:

* Eigenaar  
* Inzender  
* Lezer  

Eigenaren en medewerkers hebben volledige toegang tot de ervaring, maar een inzender kan geen toegang verlenen aan andere gebruikers of groepen. Dingen krijgt u iets interessanter met de rol van lezer, zodat dat is waar besteden we enige tijd. Voor meer informatie over toegangsmachtigingen, seee [toegang met RBAC en de Azure-portal beheren](role-assignments-portal.md).

## <a name="app-service"></a>App Service
### <a name="write-access-capabilities"></a>Toegang voor schrijven-mogelijkheden
Als u een gebruiker alleen-lezen toegang tot één web-app toewijst, worden sommige functies zijn uitgeschakeld dat u niet had verwacht. De volgende beheermogelijkheden vereisen **schrijven** toegang tot een web-app (Inzender of eigenaar) en niet beschikbaar zijn in elk scenario alleen-lezen.

* Opdrachten (zoals starten, stoppen, enz.)
* Wijzigen van de instellingen zoals de algemene configuratie, instellingen, back-upinstellingen en controle-instellingen
* Toegang tot publicatiereferenties en andere geheimen zoals app-instellingen en verbindingsreeksen
* Streaminglogboeken
* Configuratie van diagnostische logboeken
* Console (opdrachtprompt)
* Actieve en recente implementaties (voor lokale git continue implementatie)
* Geschatte besteding
* Webtests
* Virtueel netwerk (alleen zichtbaar voor een lezer als er eerder een virtueel netwerk is geconfigureerd door een gebruiker met schrijftoegang).

Als u geen toegang een van deze tegels tot, moet u uw beheerder vragen voor Inzender-toegang tot de web-app.

### <a name="dealing-with-related-resources"></a>Omgaan met gerelateerde resources
Web-apps worden door de aanwezigheid van een paar verschillende bronnen die wisselwerking tussen diagrammen ingewikkeld. Hier volgt een typische resourcegroep met een paar websites:

![Resourcegroep voor web-app](./media/troubleshooting/website-resource-model.png)

Als gevolg hiervan, als u iemand toegang tot alleen de web-app veel van de functies op de websiteblade in Azure portal is uitgeschakeld.

Deze items nodig **schrijven** toegang tot de **App Service-plan** die overeenkomt met uw website:  

* Weergeven van de WebApp de prijscategorie (gratis of Standard)  
* Configuratie van de schaal (aantal exemplaren, de grootte van virtuele machine, de instellingen voor automatisch schalen)  
* Quota's (storage, bandbreedte, CPU)  

Deze items nodig **schrijven** toegang tot het geheel **resourcegroep** waarin uw website:  

* SSL-certificaten en -bindingen (SSL-certificaten kunnen worden gedeeld tussen sites in dezelfde resourcegroep bevinden en geo-locatie)  
* Waarschuwingsregels  
* Instellingen voor automatisch schalen  
* Application Insights-onderdelen  
* Webtests  

## <a name="azure-functions"></a>Azure Functions
Sommige functies van [Azure Functions](../azure-functions/functions-overview.md) schrijven toegang nodig hebben. Bijvoorbeeld, als een gebruiker kan de rol van lezer is toegewezen, wordt dat niet meer mogelijk om de functies in een functie-app weer te geven. De portal wordt weergegeven **(geen toegang)**.

![Functie-apps geen toegang](./media/troubleshooting/functionapps-noaccess.png)

Een lezer kunt klikken op de **platformfuncties** tabblad en klik vervolgens op **alle instellingen** om bepaalde instellingen weer te geven met betrekking tot een functie-app (vergelijkbaar met een web-app), maar ze kunnen een van deze instellingen niet wijzigen.

## <a name="virtual-machine"></a>Virtuele machine
Veel nodig zoals met web-apps, bepaalde functies op de blade van de virtuele machine voor toegang voor schrijven naar de virtuele machine of naar andere resources in de resourcegroep.

Virtuele machines zijn gerelateerd aan het domein namen, virtuele netwerken, opslagaccounts en regels voor waarschuwingen.

Deze items nodig **schrijven** toegang tot de **virtuele machine**:

* Eindpunten  
* IP-adressen  
* Disks  
* Extensies  

Hiervoor is **schrijven** toegang tot zowel de **virtuele machine**, en de **resourcegroep** (samen met de naam van het domein) dat zich in:  

* Beschikbaarheidsset  
* Set met gelijke taakverdeling  
* Waarschuwingsregels  

Als u geen toegang een van deze tegels tot, vraagt u uw beheerder voor Inzender-toegang tot de resourcegroep.

## <a name="next-steps"></a>Volgende stappen
* [Toegang met RBAC en de Azure-portal beheren](role-assignments-portal.md)
* [Activiteitenlogboeken bekijken om wijzigingen van RBAC](change-history-report.md)

