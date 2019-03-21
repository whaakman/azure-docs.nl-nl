---
title: Een playbook uitvoeren in Azure Sentinel Preview | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een playbook uitvoeren in Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: e4afc5c8-ffad-4169-8b73-98d00155fa5a
ms.service: sentinel
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 5ce409b984ab9110a8ca44f47675045e4d80aed5
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58088635"
---
# <a name="tutorial-set-up-automated-threat-responses-in-azure-sentinel-preview"></a>Zelfstudie: Instellen van geautomatiseerde threat antwoorden in Azure Sentinel Preview

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

In deze zelfstudie helpt u bij het gebruik van beveiligingsplaybooks in Azure Sentinel geautomatiseerde threat reacties op beveiligingsproblemen gedetecteerd door Azure Sentinel instellen.


> [!div class="checklist"]
> * Playbooks begrijpen
> * Een playbook maken
> * Een playbook uitvoeren


## <a name="what-is-a-security-playbook-in-azure-sentinel"></a>Wat is een beveiligingsplaybook in Azure Sentinel?

Een beveiligingsplaybook is een verzameling procedures die kunnen worden uitgevoerd vanaf Azure Sentinel in reactie op een waarschuwing. Een beveiligingsplaybook kan kunt automatiseren en organiseren van uw antwoord en worden handmatig uitvoeren of ingesteld op automatisch worden uitgevoerd wanneer bepaalde waarschuwingen worden geactiveerd. Beveiligingsplaybooks in Azure Sentinel zijn gebaseerd op [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps), wat betekent dat u alle kracht, aanpassingsmogelijkheden en ingebouwde sjablonen voor logische Apps krijgen. Elke playbook is gemaakt voor het specifieke abonnement dat u kiest, maar als u de pagina Playbooks bekijkt, ziet u alle playbooks voor alle geselecteerde abonnementen.

> [!NOTE]
> Playbooks gebruikmaken van Azure Logic Apps, daarom in rekening gebracht. Ga naar de pagina met prijzen voor [Azure Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/) voor meer informatie.

Als u bang over kwaadwillende aanvallers toegang tot uw netwerkbronnen bent, kunt u bijvoorbeeld een waarschuwing dat zoekt naar schadelijke IP-adressen toegang tot uw netwerk instellen. Vervolgens kunt u een playbook die het volgende doet:
1. Wanneer de waarschuwing wordt geactiveerd, opent u een ticket in ServiceNow of een andere IT-ticketsysteem.
2. Verzend een bericht naar uw beveiligingskanaal bewerkingen in Microsoft Teams of Slack om ervoor te zorgen dat uw beveiligingsanalisten zich bewust bent van het incident.
3. Alle informatie in de waarschuwing verzenden naar uw senior netwerkbeheerder beheer en beveiliging. Het e-mailbericht bevat ook twee gebruiker keuzerondjes **blok** of **negeren**.
4. De playbook blijft nadat een antwoord wordt ontvangen van de beheerders.
5. Als de beheerders **blok**, het IP-adres in de firewall geblokkeerd en wordt de gebruiker is uitgeschakeld in Azure AD.
6. Als de beheerders **negeren**, de waarschuwing is gesloten in Azure Sentinel en het incident is gesloten in ServiceNow.

Beveiligingsplaybooks kunnen worden uitgevoerd, handmatig of automatisch. U ze handmatig uitvoert, betekent dat wanneer u een waarschuwing ontvangt, kunt u een playbook op aanvraag uitvoeren als reactie op de geselecteerde waarschuwing. Ze automatisch worden uitgevoerd, betekent dat bij het ontwerpen van de correlatie-regel in te stellen om een of meer playbooks automatisch uitgevoerd wanneer de waarschuwing wordt geactiveerd.


## <a name="create-a-security-playbook"></a>Een beveiligingsplaybook maken

Volg deze stappen voor het maken van een nieuwe beveiligingsplaybook in Azure Sentinel:

1. Open de **Azure Sentinel** dashboard.
2. Onder **Management**, selecteer **Playbooks**.

   ![Logische apps](./media/tutorial-respond-threats-playbook/playbookimg.png)

3. In de **Azure Sentinel - Playbooks (Preview)** pagina, klikt u op **toevoegen** knop.

   ![Logische app maken](./media/tutorial-respond-threats-playbook/create-playbook.png) 

4. In de **logische app maken** pagina, typt u de benodigde gegevens op voor de nieuwe logische app maken en op **maken**. 

5. In de [ **Logic App Designer**](../logic-apps/logic-apps-overview.md), selecteert u de sjabloon die u wilt gebruiken. Als u een sjabloon die het referenties selecteert, moet u deze verstrekken. U kunt ook een nieuwe lege playbook helemaal maken. Selecteer **lege logische App**. 

   ![Ontwerper van logische app](./media/tutorial-respond-threats-playbook/playbook-template.png)

6. U gaat naar de Logic App Designer waarop u kunt nieuwe maken of bewerken van de sjabloon. Voor meer informatie over het maken van een playbook met [Logic Apps](../logic-apps/logic-apps-create-logic-apps-from-templates.md).

7. Als u een lege playbook maakt de **alle connectors en triggers doorzoeken** veld, typt u *Azure Sentinel*, en selecteer **wanneer een reactie op een waarschuwing Sentinel Azure activeringsmethodeis**. <br>Nadat deze is gemaakt, de nieuwe playbook weergegeven in de **Playbooks** lijst. Als deze niet wordt weergegeven, klikt u op **vernieuwen**. 

7. Nu kunt u bepalen wat er moet gebeuren wanneer de playbook wordt geactiveerd. U kunt een actie, logische voorwaarde, switch case-voorwaarden of lussen toevoegen.

   ![Ontwerper van logische app](./media/tutorial-respond-threats-playbook/logic-app.png)

## <a name="how-to-run-a-security-playbook"></a>Hoe u kunt een beveiligingsplaybook uitvoeren

U kunt een playbook uitvoeren op aanvraag.

Een playbook op aanvraag uitvoeren:

1. In de **gevallen** pagina, selecteert u een aanvraag en klik op **volledige details weergeven**.

2. In de **waarschuwingen** tabblad, klik op de waarschuwing die u de playbook uitvoeren wilt op en schuif helemaal naar rechts en klik op **playbooks weergeven** en selecteer een playbook aan **uitvoeren** vanuit de lijst met beschikbare playbooks voor het abonnement. 




## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u een playbook uitvoeren in Azure Sentinel. Zie voor meer informatie over Azure Sentinel, de volgende artikelen: In deze zelfstudie hebt u geleerd hoe u een playbook uitvoeren in Azure Sentinel. Blijven de [hoe u proactief bedreigingen opsporen](hunting.md) met behulp van Azure Sentinel.
> [!div class="nextstepaction"]
> [Zoeken naar een baan voor bedreigingen](hunting.md) proactief bedreigingen te vinden in uw netwerk.

