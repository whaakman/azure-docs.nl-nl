---
title: Automatische update van de Mobility-Service in Azure-Azure-noodherstel | Microsoft Docs
description: Biedt een overzicht van het automatisch bijwerken van de Mobility-Service bij het repliceren van virtuele Azure-machines met Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/16/2018
ms.author: rajanaki
ms.openlocfilehash: add80b17c76e7262f55e50cd07d4e9b053cfa1ff
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34209828"
---
# <a name="automatic-update-of-the-mobility-service-in-azure-to-azure-replication"></a>Automatische update van de Mobility-Service in de replicatie van Azure naar Azure

Azure Site Recovery heeft een maandelijkse release uitgebracht waarbij verbeteringen in bestaande functies of er nieuwe bestanden worden toegevoegd en bekende problemen, indien van toepassing zijn opgelost. Dit betekent dat er om te blijven met de service, moet u over de implementatie van deze patches per maand. Om te voorkomen dat de failover-kop die zijn gekoppeld aan de upgrade, kunnen gebruikers in plaats daarvan kiezen om toe te staan van Site Recovery om updates van de onderdelen te beheren. Zoals beschreven in de [architectuur verwijzing](azure-to-azure-architecture.md) voor noodherstel van Azure naar Azure, de Mobility-Service wordt geïnstalleerd op alle Azure virtuele machines waarvoor replicatie is ingeschakeld tijdens het repliceren van virtuele machines van één Azure de regio naar een andere. Wanneer u automatische updates hebt ingeschakeld, wordt de Mobility-extensie met elke nieuwe release bijgewerkt. In dit document worden de volgende:

- Hoe werkt het automatisch bijwerken?
- Automatische updates inschakelen
- Veelvoorkomende problemen en probleemoplossing
 
## <a name="how-does-automatic-update-work"></a>Hoe werkt het automatisch bijwerken

Wanneer u Site Recovery voor het beheren van updates toestaat, is een globale runbook (die wordt gebruikt door de Azure-services) wordt geïmplementeerd via een automation-account is gemaakt in hetzelfde abonnement als de kluis. Een automation-account wordt gebruikt voor een specifieke kluis. Het runbook controleert voor elke virtuele machine in een kluis waarvoor automatische updates zijn ingeschakeld en een upgrade van de uitbreiding van de Mobility-Service wordt gestart als een nieuwere versie beschikbaar is. De standaardplanning van de runbook-recurrs dagelijks om 12:00 uur volgens de tijdzone van de gerepliceerde virtuele machine geo. De runbook-planning kan ook worden gewijzigd via de automation-account door de gebruiker, indien nodig. 

> [!NOTE]
> Inschakelen van automatische updates hoeft niet opnieuw opstarten van uw Azure VM's en heeft geen invloed op continue replicatie.

## <a name="enable-automatic-updates"></a>Automatische updates inschakelen

U kunt kiezen om Site Recovery voor het beheren van updates in de volgende manieren:-

- [Als onderdeel van de stap van de replicatie inschakelen](#as-part-of-the-enable-replication-step)
- [Schakelen tussen de extensie-instellingen in de kluis bijwerken](#toggle-the-extension-update-settings-inside-the-vault)

### <a name="as-part-of-the-enable-replication-step"></a>Als onderdeel van de stap van de replicatie inschakelen:

Wanneer u replicatie inschakelt voor een virtuele machine te beginnen [uit de weergave van de virtuele machine](azure-to-azure-quickstart.md), of [van de recovery services-kluis](azure-to-azure-how-to-enable-replication.md), ontvangt u een optie te kiezen voor de Site Recovery voor ofwel toestaan updates voor de Site Recovery-extensie of handmatig te beheren dezelfde beheren.

![Schakel replicatie-automatisch bijwerken](./media/azure-to-azure-autoupdate/enable-rep.png)

### <a name="toggle-the-extension-update-settings-inside-the-vault"></a>Schakelen tussen de extensie-instellingen in de kluis bijwerken

1. In de kluis, gaat u naar **beheren**-> **Site Recovery-infrastructuur**
2. Onder **voor Azure virtuele Machines**-> **extensie Update-instellingen**, klikt u op de wisselknop om te kiezen of u wilt toestaan *ASR om updates te beheren* of *handmatig beheren*. Klik op **Opslaan**.

![kluis, in-of uitschakelen, autuo, bijwerken](./media/azure-to-azure-autoupdate/vault-toggle.png)

> [!Important] 
> Wanneer u de optie *ASR toestaan voor het beheren van*, de instelling wordt toegepast op alle virtuele machines in de betreffende kluis.


> [!Note] 
> De beide opties u ontvangt een melding van het automatiseringsaccount dat wordt gebruikt voor het beheren van de updates. Als u deze functie voor het eerst in een kluis inschakelen wilt, kunt u een nieuw automatiseringsaccount wordt gemaakt. Alle volgende inschakelen replicaties op dezelfde kluis gebruikt het eerder gemaakte document.

## <a name="common-issues--troubleshooting"></a>Veelvoorkomende problemen en probleemoplossing

Als er een probleem met de automatische updates, moet u een melding van hetzelfde onder configuratieproblemen in het kluisdashboard. 

Als u probeert uit te schakelen van automatische updates en is mislukt, Raadpleeg hieronder voor het oplossen van problemen.

**Fout**: U bent niet gemachtigd voor het maken van een Azure uitvoeren als-account (service-principal) en de rol Inzender verlenen aan de service-principal. 
- Aanbevolen actie: Zorg ervoor dat het aangemelde account 'Inzender' is toegewezen en probeer het opnieuw.
 
Wanneer automatische updates zijn ingeschakeld, de meeste van de problemen kunnen worden moeten worden hersteld door de Site Recovery-service en moet u klikken op de '**reparatie**' knop.

![knop herstellen](./media/azure-to-azure-autoupdate/repair.png)

Als de knop herstellen niet beschikbaar is, raadpleegt u het foutbericht dat wordt weergegeven onder het deelvenster van de extensie-instellingen.

 - **Fout**: de Run As-account is niet gemachtigd de toegang tot de recovery services-bron.

    **Aanbevolen actie**: verwijderen en vervolgens [opnieuw het Run As-account maken](https://docs.microsoft.com/en-us/azure/automation/automation-create-runas-account) of zorg ervoor dat Automation uitvoeren als-account van Azure Active Directory-toepassing toegang tot de recovery services-resource heeft.

- **Fout**: Run As-account is niet gevonden. Een van deze is verwijderd of niet gemaakt - Azure Active Directory-toepassing, Service-Principal, rol, Automation-certificaatasset, Automation-verbindingsasset- of de vingerafdruk is niet identiek certificaat en de verbinding. 

    **Aanbevolen actie**: verwijderen en [opnieuw uitvoeren als-account maken](https://docs.microsoft.com/en-us/azure/automation/automation-create-runas-account).
