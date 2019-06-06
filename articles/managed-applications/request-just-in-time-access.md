---
title: Inschakelen en de aanvraag voor just-in-time-toegang voor Azure Managed Applications
description: Hierin wordt beschreven hoe uitgevers van Azure Managed Applications just-in-time toegang vragen tot een beheerde toepassing.
author: MSEvanhi
ms.service: managed-applications
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: evanhi
ms.openlocfilehash: ea933f5382cb42c01de523326b094c1813401132
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481772"
---
# <a name="enable-and-request-just-in-time-access-for-azure-managed-applications"></a>Inschakelen en de aanvraag voor just-in-time-toegang voor Azure Managed Applications

Het is mogelijk dat gebruikers van uw beheerde toepassing terughoudend u permanente toegang verlenen tot de beheerde resourcegroep. Als uitgever van een manager-toepassing, kunt u misschien liever dat consumenten precies wanneer u nodig hebt weet voor toegang tot de beheerde resources. Meer controle over het verlenen van toegang tot beheerde resources, Azure beheerde toepassingen biedt zodat gebruikers een functie met de naam van just-in-time (JIT) toegang, dat zich momenteel in preview.

JIT-toegang kunt u om aan te vragen met verhoogde bevoegdheden toegang tot bronnen van een beheerde toepassing voor probleemoplossing of onderhoud. U altijd alleen-lezen toegang hebben tot de resources, maar voor een bepaalde periode kunt u betere toegang hebt.

De werkstroom voor het verlenen van toegang is:

1. U voegt een beheerde toepassing in de marketplace en opgeven dat toegang tot JIT beschikbaar is.

1. Tijdens de implementatie kan de consument JIT-toegang voor het exemplaar van de beheerde toepassing.

1. De consument kan de instellingen voor toegang tot JIT wijzigen na de implementatie.

1. U kunt een aanvraag voor toegang verzenden wanneer u nodig hebt bij het oplossen of de beheerde bronnen bijwerken.

1. De consument goedkeurt uw aanvraag.

In dit artikel richt zich op de acties die uitgevers aan JIT-toegang inschakelen en het verzenden van aanvragen uitvoeren. Zie voor meer informatie over het goedkeuren van aanvragen voor JIT-toegang, [just-in-time-toegang in Azure beheerde toepassingen goedkeuren](approve-just-in-time-access.md).

## <a name="add-jit-access-step-to-ui"></a>Stap voor JIT-toegang toevoegen aan de gebruikersinterface

Het bestand CreateUiDefinition.json is precies hetzelfde als de gebruikersinterface dat u hebt gemaakt voor permanente toegang alleen voegt u een stap waarmee gebruikers toegang tot JIT inschakelen. Zie voor meer informatie over het publiceren van uw eerste beheerde toepassing in de Azure Marketplace-aanbieding, [Azure beheerde toepassingen in de Marketplace](publish-marketplace-app.md).

Ter ondersteuning van JIT-mogelijkheid voor uw aanbod, voeg de volgende inhoud toe aan het bestand CreateUiDefinition.json:

In 'stappen':

```json
{
    "name": "jitConfiguration",
    "label": "JIT Configuration",
    "subLabel": {
        "preValidation": "Configure JIT settings for your application",
        "postValidation": "Done"
    },
    "bladeTitle": "JIT Configuration",
    "elements": [
        {
          "name": "jitConfigurationControl",
          "type": "Microsoft.Solutions.JitConfigurator",
          "label": "JIT Configuration"
        }
    ]
}
```
 
In 'uitvoer':

```json
"jitAccessPolicy": "[parse(concat('{\"jitAccessEnabled\":', string(steps('jitConfiguration').jitConfigurationControl.jitEnabled), ',\"jitApprovalMode\":\"', steps('jitConfiguration').jitConfigurationControl.jitApprovalMode, '\",\"maximumJitAccessDuration\":\"', steps('jitConfiguration').jitConfigurationControl.maxAccessDuration, '\",\"jitApprovers\":', string(steps('jitConfiguration').jitConfigurationControl.approvers), '}'))]"
```

> [!NOTE]
> JIT-toegang is in preview. Het schema voor de configuratie van JIT kan in toekomstige iteraties wijzigen.

## <a name="enable-jit-access"></a>JIT-toegang inschakelen

Bij het definiëren van uw aanbieding op marketplace, zorg er dan voor dat u toegang tot JIT inschakelen.

1. Aanmelden bij de [Cloud Partner-portal voor publiceren](https://cloudpartner.azure.com).

1. Geef waarden voor het publiceren van uw beheerde toepassing in de marketplace. Selecteer **Ja** voor **JIT-toegang inschakelen?**

   ![Just-in-time-toegang inschakelen](./media/request-just-in-time-access/marketplace-enable.png)

U een stap van de configuratie van JIT hebt toegevoegd aan de gebruikersinterface en JIT-toegang in de marketplace-aanbieding hebt ingeschakeld. Wanneer gebruikers de beheerde toepassing implementeren, kunnen ze [inschakelen JIT-toegang voor de instantie](approve-just-in-time-access.md#enable-during-deployment).

## <a name="request-access"></a>Toegang aanvragen

Wanneer u nodig hebt voor toegang tot beheerde resources van de consumenten, stuurt u een aanvraag voor een specifieke rol, de tijd en de duur. De consument moet de aanvraag vervolgens goedkeuren.

Voor het verzenden van een aanvraag voor JIT-toegang:

1. Selecteer **toegang tot JIT** voor de beheerde toepassing die u dient te openen.

1. Selecteer **in aanmerking komende rollen**, en selecteer **activeren** in de kolom actie voor de rol die u wilt.

   ![Aanvraag voor toegang activeren](./media/request-just-in-time-access/send-request.png)

1. Op de **rol activeren** vormen, selecteert u een begintijd en duur van uw rol actief. Selecteer **activeren** om de aanvraag te verzenden.

   ![-Toegang activeren](./media/request-just-in-time-access/activate-access.png) 

1. De meldingen om te zien dat de nieuwe JIT-aanvraag is verzonden naar de gebruiker weergeven.

   ![Melding](./media/request-just-in-time-access/in-progress.png)

   Nu u de consument moet wachten [uw aanvraag goedkeuren](approve-just-in-time-access.md#approve-requests).

1. Als u wilt de status van alle JIT-aanvragen voor een beheerde toepassing bekijken, selecteert u **toegang tot JIT** en **aanvragen geschiedenis**.

   ![Status weergeven](./media/request-just-in-time-access/view-status.png)

## <a name="known-issues"></a>Bekende problemen

De principal-ID van het account dat aanvragen van toegang tot JIT moet expliciet worden opgenomen in de definitie van beheerde toepassing. Het account kan niet alleen zijn opgenomen in een groep die is opgegeven in het pakket. Deze beperking wordt opgelost in een toekomstige release.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het goedkeuren van aanvragen voor toegang tot JIT, [just-in-time-toegang in Azure beheerde toepassingen goedkeuren](approve-just-in-time-access.md).
