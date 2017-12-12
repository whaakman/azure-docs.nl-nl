---
title: Maken en de Azure portal dashboards delen | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe maken en bewerken van dashboards in de Azure portal.
services: azure-portal
documentationcenter: 
author: sewatson
manager: timlt
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: azure-portal
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/06/2016
ms.author: sewatson
ms.openlocfilehash: 7f90c882285170bac34bfe020831d3ac04010a11
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="create-and-share-dashboards-in-the-azure-portal"></a>Maken en delen van dashboards in de Azure portal
U kunt meerdere dashboards maken en ze delen met anderen die toegang tot uw Azure-abonnementen hebben.  In dit artikel gaat met de grondbeginselen van het maken, bewerken, publiceren en beheren van toegang tot dashboards.

## <a name="create-a-dashboard"></a>Een dashboard maken
Als u een dashboard, selecteert de **nieuwe dashboard** knop naast de naam van het dashboard.  

![dashboard maken](./media/azure-portal-dashboards/new-dashboard.png)

Deze actie wordt een nieuw, leeg, persoonlijke dashboard gemaakt en wordt u aanpassing modus kunt u uw dashboard name en toevoegen of tegels opnieuw rangschikken.  In deze modus, neemt de galerie samenvouwbare tegel op het navigatiemenu links.  De tegel galerie kunt u tegels vinden voor uw Azure-resources op verschillende manieren: u kunt bladeren door [resourcegroep](../azure-resource-manager/resource-group-overview.md#resource-groups), door resource typt, by [tag](../azure-resource-manager/resource-group-using-tags.md), of door te zoeken voor uw resource met de naam.  

![dashboard aanpassen](./media/azure-portal-dashboards/customize-dashboard.png)

Tegels toevoegen door slepen en neerzetten op het dashboard voor aanvallen op elke gewenste locatie.

Er is een nieuwe categorie met de naam **algemene** voor tegels die niet gekoppeld aan een bepaalde bron zijn.  In dit voorbeeld wordt de Markdown-tegel vastmaken.  Deze tegel kunt u aangepaste inhoud toevoegen aan uw dashboard.  De tegel ondersteunt tekst zonder opmaak, [Markdown-syntaxis](https://daringfireball.net/projects/markdown/syntax), en een beperkte set van HTML-code.  (Voor de veiligheid, niet zoiets als injecteren `<script>` tags of bepaalde element stijlen van CSS die met de portal conflicteren mogelijk gebruiken.) 

![markdown toevoegen](./media/azure-portal-dashboards/add-markdown.png)

## <a name="edit-a-dashboard"></a>Een dashboard bewerken
Nadat uw dashboard is gemaakt, kunt u tegels van de tegel galerie of de tegel representatie van blades vastmaken. Laten we de weergave van onze resourcegroep vastmaken. Bij het bladeren door het item of vanuit de blade met resourcegroepen kunt u de pincode. Beide benaderingen resulteren in de tegel weergave van de resourcegroep vastmaken.

![Vastmaken aan dashboard](./media/azure-portal-dashboards/pin-to-dashboard.png)

Na het vastmaken van het item, wordt deze weergegeven op uw dashboard.

![weergave-dashboard](./media/azure-portal-dashboards/view-dashboard.png)

Nu dat we een Markdown-tegel hebben en een resourcegroep is vastgemaakt aan het dashboard, kunnen we vergroten of verkleinen en de tegels rangschikken in een geschikte indeling.

Door de muiswijzer en '...' selecteren of met de rechtermuisknop op een tegel ziet u de contextuele opdrachten voor deze tegel. Standaard zijn er twee items:

1. **Losmaken van dashboard** : Hiermee verwijdert u de tegel van het dashboard
2. **Aanpassen** – krijgt de modus aanpassen

![tegel aanpassen](./media/azure-portal-dashboards/customize-tile.png)

Door te selecteren aanpassen, kunt u het formaat en tegels opnieuw rangschikken. Als u een tegel, selecteert u de nieuwe grootte in de contextafhankelijke menu, zoals wordt weergegeven in de volgende afbeelding.

![tegel vergroten of verkleinen](./media/azure-portal-dashboards/resize-tile.png)

Of als de tegel elke grootte ondersteunt, kunt u de rechterbovenhoek naar de gewenste grootte slepen.

![tegel vergroten of verkleinen](./media/azure-portal-dashboards/resize-corner.png)

Nadat het formaat van tegels wijzigen, moet u het dashboard weergeven.

![tegel](./media/azure-portal-dashboards/view-tile.png)

Zodra u klaar bent met een dashboard aanpassen, selecteer de **gedaan aanpassen** om af te sluiten modus aanpassen of met de rechtermuisknop op en selecteer **gedaan aanpassen** in het contextmenu.

## <a name="publish-a-dashboard-and-manage-access-control"></a>Een dashboard publiceren en beheren van toegangsbeheer
Wanneer u een dashboard maakt, is het persoonlijke standaard, wat betekent dat u de enige bent die deze kunnen zien.  Als u deze zichtbaar voor anderen, gebruikt u de **Share** knop die wordt weergegeven naast de andere opdrachten in het dashboard.

![dashboard delen](./media/azure-portal-dashboards/share-dashboard.png)

U wordt gevraagd een abonnement en resourcegroep voor uw dashboard worden gepubliceerd om te kiezen. Als u wilt integreren naadloos dashboards in het ecosysteem, hebben we geïmplementeerd gedeelde dashboards als Azure-resources (zodat u niet delen door een e-mailadres in te voeren).  Toegang tot de gegevens worden weergegeven door de meeste van de tegels in de portal vallen [Azure Toegangsbeheerrol](../active-directory/role-based-access-control-configure.md). Gedeelde dashboards zijn vanuit een access control-perspectief niet anders als een virtuele machine of een opslagaccount.  

Stel dat u hebt een Azure-abonnement en leden van uw team zijn toegewezen de functies van **eigenaar**, **Inzender**, of **lezer** van het abonnement.  Gebruikers die eigenaar of inzenders kunnen weergeven, weergeven, maken, wijzigen of verwijderen van dashboards binnen dat abonnement.  Gebruikers die lezers zijn kunnen lijst en weergave dashboards, maar kunnen niet worden gewijzigd of verwijderd.  Gebruikers met leestoegang hebben, kunnen lokale wijzigingen aanbrengen in een gedeelde dashboard, maar niet kunnen deze wijzigingen publiceren naar de server.  Ze kunnen nog wel een persoonlijke kopie van het dashboard voor eigen gebruik.  Afzonderlijke tegels op het dashboard afdwingen als altijd hun eigen regels voor toegang op basis van de resources die ze met overeenkomen.  

Voor het gemak ervaring handleidingen u naar een patroon waar u dashboards in een resourcegroep plaatsen opgeroepen het publiceren van de portal **dashboards**.  

![dashboard publiceren](./media/azure-portal-dashboards/publish-dashboard.png)

U kunt ook een dashboard publiceren aan een resourcegroep.  Het toegangsbeheer voor dit dashboard komt overeen met het toegangsbeheer voor de resourcegroep.  Gebruikers die de resources in die resourcegroep kunnen beheren ook hebben toegang tot de dashboards.

![dashboard publiceren naar de resourcegroep](./media/azure-portal-dashboards/publish-to-resource-group.png)

Nadat uw dashboard is gepubliceerd, de **delen + toegang** besturingselementvenster wordt vernieuwd en ziet u informatie over het gepubliceerde dashboard, inclusief een koppeling voor het beheren van de gebruikerstoegang tot het dashboard.  Deze koppeling opent de standaard rollen gebaseerd toegangsbeheer blade gebruikt voor het beheren van toegang voor een Azure-resource.  U kunt altijd teruggaan naar deze weergave selecteert **Share**.

![toegang beheren](./media/azure-portal-dashboards/manage-access.png)

## <a name="next-steps"></a>Volgende stappen
* Zie voor het beheren van resources, [beheren Azure-resources via de portal](../azure-resource-manager/resource-group-portal.md).
* Zie voor het implementeren van resources [implementeren van resources met Resource Manager-sjablonen en Azure-portal](../azure-resource-manager/resource-group-template-deploy-portal.md).

