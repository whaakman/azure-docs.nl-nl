---
title: 'Azure Backup: Monitor voor Azure Backup beveiligde werkbelastingen'
description: Azure Backup-werkbelastingen met behulp van Azure portal controleren
services: backup
author: pvrk
manager: shivamg
keywords: Azure back-up. Waarschuwingen;
ms.service: backup
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: pullabhk
ms.assetid: 86ebeb03-f5fa-4794-8a5f-aa5cbbf68a81
ms.openlocfilehash: 2513f2e7a2e82a541fa5638b70d0543192c84765
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56885352"
---
# <a name="monitoring-azure-backup-workloads"></a>Azure Backup bewakingsworkloads

Azure Backup biedt meerdere back-upoplossingen op basis van de back-vereiste en infrastructuur topologie (On-premises versus Azure). Een back-gebruiker/beheerder ziet wat er gebeurt in alle oplossingen en verwacht om bericht te krijgen in belangrijke scenario's. Dit artikel worden de mogelijkheden voor controle en meldingen geleverd door Azure Backup-service.

## <a name="backup-jobs-in-recovery-services-vault"></a>Back-uptaken in Recovery Services-kluis

Azure Backup biedt ingebouwde bewaking- en waarschuwingsfuncties voor workloads die wordt beveiligd door Azure Backup. In de Recovery Services-kluis-instellingen biedt de sectie 'controleren' ingebouwde taken en waarschuwingen.

![RS kluis ingebouwde bewaking](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltmonitoring.png)

Taken worden gegenereerd wanneer bewerkingen zoals configureren back-up, back-upbewerking, terugzetten, verwijderen back-ups enzovoort worden uitgevoerd.
Taken van de volgende back-up van Azure-oplossingen worden hier weergegeven.

- Azure VM-back-ups
- Back-ups voor Azure-
- Back-ups van Azure-workload, zoals SQL
- Azure backup-agent (Microsoft Azure Backup)

Taken van System Center Data Protection Manager (SC-DPM), Microsoft Azure Backup-Server (MABS) worden hier niet weergegeven.

> [!NOTE]
> Met betrekking tot Azure-workloads zoals SQL-back-ups in Azure-VM's zijn het aantal back-uptaken enorm. Bijvoorbeeld, de logboekback-ups voor elke 15 minuten kunnen uitvoeren. Daarom kan voor deze werkbelastingen DB moet alleen geactiveerd gebruikersbewerkingen worden weergegeven. Geplande back-upbewerkingen worden niet weergegeven.

## <a name="backup-alerts-in-recovery-services-vault"></a>Back-Upwaarschuwingen in Recovery Services-kluis

Waarschuwingen zijn voornamelijk scenario's waar de klant nodig heeft om te worden geïnformeerd dat ze de relevante actie kunnen ondernemen. De sectie 'Waarschuwingen voor back-up' bevat waarschuwingen die worden gegenereerd door Azure Backup-service. Deze waarschuwingen zijn gedefinieerd door de service en de gebruiker kan geen aangepaste waarschuwingen maken. De volgende scenario's zijn gedefinieerd door de service als Signaleerbare scenario 's

- Back-up/herstel fouten
- Back-up is geslaagd met waarschuwingen
- Stop de beveiliging met gegevens/Stop de beveiliging en verwijder gegevens behouden

Er zijn enkele uitzonderingen wanneer een waarschuwing wordt gegenereerd op een fout.

- Gebruiker expliciet de actieve taak is geannuleerd
- De taak is mislukt omdat een andere back-uptaak uitgevoerd (om te reageren op Hier wordt, aangezien we net hebben na afloop van de vorige taak is voltooid)
- De virtuele machine back-uptaak is mislukt omdat de Azure-VM back-up niet meer bestaat

De bovenstaande uitzonderingen zijn ontworpen uit het begrip die het resultaat van deze bewerkingen (voornamelijk gebruiker geactiveerd) weergegeven onmiddellijk op portal/PS/CLI-clients wordt. Daarom kan de gebruiker onmiddellijk op de hoogte en hoeft niet een melding.

Waarschuwingen van de volgende back-up van Azure-oplossingen worden hier weergegeven.

- Azure VM-back-ups
- Back-ups voor Azure-
- Back-ups van Azure-workload, zoals SQL
- Azure backup-agent (Microsoft Azure Backup)

> [!NOTE]
> Waarschuwingen van System Center Data Protection Manager (SC-DPM), Microsoft Azure Backup-Server (MABS) worden hier niet weergegeven.

Op basis van ernst van waarschuwing, kunnen waarschuwingen worden gedefinieerd in drie typen:

- **Kritiek:** In principe, een back-up of herstel storing (gepland of gebruiker heeft geactiveerd) zou leiden tot het genereren van een waarschuwing en als een kritieke waarschuwing wordt weergegeven. Destructieve bewerkingen zoals back-ups verwijderen zou ook een kritieke waarschuwingen.
- **Waarschuwing**: Als de back-upbewerking is voltooid, maar met enkele waarschuwingen, worden ze weergegeven als waarschuwingen.
- **Informatief**: Vanaf vandaag, wordt geen informatieve waarschuwing gegenereerd door Azure Backup-service.

### <a name="notification-for-backup-alerts"></a>Melding voor back-Upwaarschuwingen

> [!NOTE]
> Configuratie van de melding kan alleen via Azure Portal worden gedaan. PS/CLI/REST API/Azure Resource Manager-sjabloon-ondersteuning is nog niet opgegeven.

Wanneer een waarschuwing wordt gegenereerd, wordt de klant nodig heeft om te worden geïnformeerd. Azure Backup biedt een meldingsmechanisme voor de ingebouwde via e-mail. Een kunt afzonderlijke e-mailadressen of distributielijsten om te worden geïnformeerd wanneer een waarschuwing wordt gegenereerd opgeven. U kunt ook kiezen of om te worden geïnformeerd voor elke afzonderlijke waarschuwing of ze te groeperen in een samenvatting per uur en vervolgens een melding ontvangen.

![RS kluis ingebouwde e-mailmelding](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltnotification.png)

Zodra u meldingen configureert, ontvangt u een Welkom/inleidende e-mailadres waarmee wordt bevestigd dat Azure Backup e-mailberichten naar deze adressen verzenden kunnen wanneer een waarschuwing wordt gegenereerd. Als de frequentie is ingesteld op een samenvatting per uur en als een waarschuwing is gegenereerd en binnen het uur opgelost, wordt het niet een deel van de toekomstige samenvatting per uur zijn.

> [!NOTE]
> Als een destructieve bewerking, zoals 'stop de beveiliging met verwijderen van gegevens' wordt uitgevoerd, wordt een waarschuwing wordt gegenereerd en wordt een e-mailbericht is verzonden naar abonnementseigenaren, beheerders en co-beheerders, zelfs als meldingen zijn niet geconfigureerd voor de kluis RS.

## <a name="next-steps"></a>Volgende stappen

[Azure-back-workloads met Azure Monitor bewaken](backup-azure-monitoring-use-azuremonitor.md)