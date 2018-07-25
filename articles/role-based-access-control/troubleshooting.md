---
title: Oplossen van RBAC in Azure | Microsoft Docs
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
ms.date: 07/23/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: d1a0e46fe348bbc60a4d02a4727a9bb27cb26742
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223293"
---
# <a name="troubleshoot-rbac-in-azure"></a>RBAC in Azure oplossen

In dit artikel vindt u antwoorden op veelgestelde vragen over op rollen gebaseerd toegangsbeheer (RBAC), zodat u wat weet u kunt verwachten wanneer u de rollen in de Azure-portal en kan problemen met toegang.

## <a name="web-app-features-that-require-write-access"></a>Web-appfuncties waarvoor schrijftoegang is vereist

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

## <a name="web-app-resources-that-require-write-access"></a>Web app-resources waarvoor de toegang voor schrijven

Web-apps worden door de aanwezigheid van een paar verschillende bronnen die wisselwerking tussen diagrammen ingewikkeld. Hier volgt een typische resourcegroep met een paar van websites:

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

## <a name="virtual-machine-features-that-require-write-access"></a>Functies van virtuele machines die toegang voor schrijven vereisen

Net als bij de web-apps, vereisen sommige functies op de blade van de virtuele machine toegang voor schrijven naar de virtuele machine of naar andere resources in de resourcegroep.

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

## <a name="azure-functions-and-write-access"></a>Azure Functions en toegang voor schrijven

Sommige functies van [Azure Functions](../azure-functions/functions-overview.md) schrijven toegang nodig hebben. Bijvoorbeeld, als een gebruiker kan de rol van lezer is toegewezen, wordt dat niet meer mogelijk om de functies in een functie-app weer te geven. De portal wordt weergegeven **(geen toegang)**.

![Functie-apps geen toegang](./media/troubleshooting/functionapps-noaccess.png)

Een lezer kunt klikken op de **platformfuncties** tabblad en klik vervolgens op **alle instellingen** om bepaalde instellingen weer te geven met betrekking tot een functie-app (vergelijkbaar met een web-app), maar ze kunnen een van deze instellingen niet wijzigen.

## <a name="rbac-changes-are-not-being-detected"></a>RBAC wijzigingen worden niet wordt gedetecteerd.

Azure Resource Manager slaat soms configuraties en gegevens om prestaties te verbeteren. Bij het maken of verwijderen van roltoewijzingen, kan het wijzigingen van kracht tot 30 minuten duren voordat. Als u de Azure portal, Azure PowerShell of Azure CLI gebruikt, kunt u een vernieuwing van uw wijzigingen aan toewijzingen van rol forceren door afmelden en aanmelden. Als u wijzigingen aan toewijzingen van rol met REST API-aanroepen, kunt u een vernieuwing afdwingen door het vernieuwen van uw toegangstoken.

## <a name="next-steps"></a>Volgende stappen
* [Toegang beheren met op rollen gebaseerd toegangsbeheer en Azure Portal](role-assignments-portal.md)
* [Activiteitenlogboeken bekijken om wijzigingen van RBAC](change-history-report.md)

