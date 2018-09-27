---
title: Azure portal dashboards maken en delen | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u maken en bewerken van dashboards in de Azure-portal.
services: azure-portal
documentationcenter: ''
author: sewatson
manager: doubeby
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: azure-portal
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/06/2016
ms.author: cwatson
ms.openlocfilehash: 6062e54f0fb584dec4c2db540565c5860ae92a2d
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394952"
---
# <a name="create-and-share-dashboards-in-the-azure-portal"></a>Dashboards in de Azure-portal maken en delen
U kunt meerdere dashboards maken en delen met anderen die toegang tot uw Azure-abonnementen hebben.  In dit artikel gaat de basisbeginselen besproken van het maken, bewerken, publiceren en beheren van toegang tot de dashboards.

## <a name="create-a-dashboard"></a>Een dashboard maken
Om een dashboard te maken, selecteert u de knop **Nieuw dashboard** naast de naam van het huidige dashboard.  

![dashboard maken](./media/azure-portal-dashboards/new-dashboard.png)

Met deze actie wordt een nieuw, leeg, persoonlijk dashboard gemaakt en in de aanpassingsmodus gezet. In deze modus kunt u het dashboard een naam geven en tegels toevoegen of opnieuw rangschikken.  In deze modus wordt de samenvouwbare tegelgalerie heeft ten opzichte van het navigatiemenu links.  De tegelgalerie kunt u tegels voor uw Azure-resources op verschillende manieren vinden: u kunt bladeren door [resourcegroep](../azure-resource-manager/resource-group-overview.md#resource-groups), per resourcetype, met [tag](../azure-resource-manager/resource-group-using-tags.md), of door te zoeken naar de resource met de naam.  

![dashboard aanpassen](./media/azure-portal-dashboards/customize-dashboard.png)

Tegels toevoegen via slepen en neerzetten naar het oppervlak dashboard waar u maar wilt.

Er is een nieuwe categorie met de naam **algemene** voor tegels die niet gekoppeld aan een bepaalde resource zijn.  In dit voorbeeld wordt de tegel Markdown vastmaken.  Deze tegel kunt u aangepaste inhoud toevoegen aan uw dashboard.  De tegel ondersteunt tekst zonder opmaak, [Markdown-syntaxis](https://daringfireball.net/projects/markdown/syntax), en een beperkt aantal HTML-code.  (Voor de veiligheid, u kunt geen zaken als invoeren `<script>` tags of bepaalde element stijl van CSS die leiden tot met de portal problemen mogelijk te gebruiken.) 

![markdown toevoegen](./media/azure-portal-dashboards/add-markdown.png)

## <a name="edit-a-dashboard"></a>Een dashboard bewerken
Nadat uw dashboard is gemaakt, kunt u tegels uit de tegelgalerie of de weergave van de tegel van blades vastmaken. Laten we de weergave van onze resourcegroep vastmaken. U kunt een pincode bij het bladeren door het item, of vanuit de blade van de resourcegroep. Beide methoden resulteren in de weergave van de tegel van de resourcegroep vast te maken.

![Vastmaken aan dashboard](./media/azure-portal-dashboards/pin-to-dashboard.png)

Na het vastmaken van het item, wordt deze weergegeven op uw dashboard.

![dashboard weergeven](./media/azure-portal-dashboards/view-dashboard.png)

We kunnen nu dat we beschikken over een tegel Markdown en een resourcegroep die is vastgemaakt aan het dashboard, vergroten of verkleinen en herschikken de tegels in een geschikte indeling.

Door te bewegen en '...' selecteren of met de rechtermuisknop op een tegel ziet u de contextuele opdrachten voor deze tegel. Standaard zijn er twee items:

1. **Van dashboard losmaken** : Hiermee verwijdert u de tegel van het dashboard
2. **Aanpassen** – krijgt de modus voor aanpassingen

![tegel aanpassen](./media/azure-portal-dashboards/customize-tile.png)

Door het selecteren van aanpassen, kunt u vergroten of verkleinen en de volgorde van tegels wijzigen. Als u wilt het formaat van een tegel, selecteert u de nieuwe grootte in de contextafhankelijke menu, zoals wordt weergegeven in de volgende afbeelding.

![formaat van tegel wijzigen](./media/azure-portal-dashboards/resize-tile.png)

Of, als de tegel elke grootte ondersteunt, kunt u de rechterbenedenhoek slepen naar de gewenste grootte.

![formaat van tegel wijzigen](./media/azure-portal-dashboards/resize-corner.png)

Grootte wijzigen van tegels, het dashboard te bekijken.

![tegel weergave](./media/azure-portal-dashboards/view-tile.png)

Zodra u klaar bent met het aanpassen van een dashboard, selecteer de **aanpassen voltooid** om af te sluiten modus aanpassen of met de rechtermuisknop op en selecteer **aanpassen voltooid** in het contextmenu.

## <a name="publish-a-dashboard-and-manage-access-control"></a>Een dashboard publiceren en beheren van toegangsbeheer
Wanneer u een dashboard maakt, is het standaard persoonlijk. Dat betekent dat u de enige bent die het kan zien.  Als u het zichtbaar wilt maken voor anderen, gebruik dan de knop **Delen** die naast de andere dashboardopdrachten wordt weergegeven.

![dashboard delen](./media/azure-portal-dashboards/share-dashboard.png)

U wordt gevraagd een abonnement en resourcegroep te kiezen waarnaar uw dashboard zal worden gepubliceerd. Als u wilt integreren dashboards naadloos in het ecosysteem, hebben we geïmplementeerd gedeelde dashboards als Azure-resources (zodat u kunt niet delen door een e-mailadres te typen).  Toegang tot de informatie die wordt weergegeven door de meeste van de tegels in de portal gelden [Azure Role Based Access Control](../role-based-access-control/role-assignments-portal.md). Gedeelde dashboards zijn vanuit een access control-perspectief niet anders als een virtuele machine of een storage-account.  

Stel dat u hebt een Azure-abonnement en leden van uw team zijn toegewezen de rollen van **eigenaar**, **Inzender**, of **lezer** van het abonnement.  Gebruikers die eigenaren of bijdragers zijn kunnen weergeven, weergeven, maken, wijzigen of verwijderen in dat abonnement.  Gebruikers die lezers kunnen aan dashboards, vermelden en weergeven, maar kunnen niet worden gewijzigd of verwijderd.  Gebruikers met leestoegang hebben zijn lokale wijzigingen aanbrengen in een gedeeld dashboard, maar zijn niet in staat zijn om deze wijzigingen publiceren naar de server.  Deze kan echter een persoonlijke kopie van het dashboard voor eigen gebruik aanbrengen.  Zoals altijd afdwingen afzonderlijke tegels op het dashboard op basis van de resources die ze met overeenkomen regels voor hun eigen toegang.  

Voor het gemak helpt de publicatie-ervaring van de portal u naar een patroon waar u dashboards kunt plaatsen in een resourcegroep die **dashboards** wordt genoemd.  

![Dashboard publiceren](./media/azure-portal-dashboards/publish-dashboard.png)

U kunt ook een dashboard publiceren naar een bepaalde resourcegroep.  Het toegangsbeheer voor dat dashboard komt overeen met het toegangsbeheer voor de resourcegroep.  Gebruikers die de resources in die resourcegroep kunnen beheren hebben ook toegang tot de dashboards.

![dashboard publiceren naar de resourcegroep](./media/azure-portal-dashboards/publish-to-resource-group.png)

Nadat uw dashboard is gepubliceerd, de **delen en toegang** besturingselement deelvenster wordt vernieuwd en informatie weergegeven over de gepubliceerde dashboard, ook een koppeling voor het beheren van de gebruikerstoegang tot het dashboard.  Deze koppeling wordt gestart van de standaard rollen gebaseerd toegangsbeheer-blade die wordt gebruikt voor het beheren van toegang voor een Azure-resource.  U kunt altijd teruggaan naar deze weergave door te selecteren **Share**.

![toegangsbeheer beheren](./media/azure-portal-dashboards/manage-access.png)

## <a name="next-steps"></a>Volgende stappen
* Zie voor het beheren van resources, [Azure-resources beheren via portal](../azure-resource-manager/resource-group-portal.md).
* Zie voor het implementeren van resources, [resources implementeren met Resource Manager-sjablonen en Azure portal](../azure-resource-manager/resource-group-template-deploy-portal.md).

