---
title: Automatische update van de Mobility-Service in herstel na noodgevallen van Azure naar Azure | Microsoft Docs
description: Biedt een overzicht van automatische update van de Mobility-Service bij het repliceren van virtuele Azure-machines met Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: 061811705b0572059f99300f2a658c5c5b4406c5
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52839837"
---
# <a name="automatic-update-of-the-mobility-service-in-azure-to-azure-replication"></a>Automatische update van de Mobility-Service in Azure naar Azure-replicatie

Azure Site Recovery heeft een maandelijkse vrijgeeffrequentie waar verbeteringen in bestaande functies of nieuwe labels worden toegevoegd en bekende problemen, indien van toepassing zijn opgelost. Dit betekent dat er om te blijven met de service, moet u van plan bent voor de implementatie van deze patches maandelijks. Gebruikers kunnen om te voorkomen dat de boven-kop die zijn gekoppeld aan de upgrade, in plaats daarvan kiezen om toe te staan van Site Recovery om updates van de onderdelen te beheren. Zoals beschreven in de [architectuur verwijzing](azure-to-azure-architecture.md) voor noodherstel van Azure naar Azure, de Mobility-Service wordt geïnstalleerd op alle Azure virtual machines waarvoor replicatie is ingeschakeld tijdens het repliceren van virtuele machines van het ene Azure regio naar een andere. Wanneer u automatische updates inschakelen, wordt de Mobility-service-extensie wordt bijgewerkt met elke nieuwe release. In dit document worden de volgende:

- Hoe werkt automatisch bijwerken?
- Automatische updates inschakelen
- Veelvoorkomende problemen en probleemoplossing
 
## <a name="how-does-automatic-update-work"></a>Hoe werkt automatisch bijwerken

Zodra u Site Recovery voor het beheren van updates toestaat, wordt een algemene runbook (die wordt gebruikt door Azure-services) geïmplementeerd via een automation-account wordt gemaakt in hetzelfde abonnement als de kluis. Een automation-account wordt gebruikt voor een bepaalde kluis. Het runbook wordt gecontroleerd op elke virtuele machine in een kluis waarvoor automatische updates zijn ingeschakeld en start een upgrade van de Mobility-Service-extensie als een nieuwere versie beschikbaar is. De standaardplanning van de runbook-recurrs elke dag om 12:00 uur aan de hand van de tijdzone van de gerepliceerde virtuele machine van geografische gebied. De planning voor runbook kan ook worden gewijzigd via het automation-account door de gebruiker, indien nodig. 

> [!NOTE]
> Inschakelen van automatische updates vereist geen herstart van uw Azure-VM's en heeft geen invloed op continue replicatie.

> [!NOTE]
> Facturering voor taken die worden gebruikt door automation-account is gebaseerd op het aantal taakuitvoertijd minuten gebruikt in de maand en standaard 500 minuten opgenomen gratis eenheden voor een automation-account zijn. De uitvoering van de dagelijkse hoeveelheden van de taak van een **enkele seconden naar ongeveer een minuut** en **behandeld in de gratis tegoed**.

GRATIS eenheden inbegrepen (PER maand) ** prijs taak uitgevoerd tijd 500 minuten ₹0.14 / minuut

## <a name="enable-automatic-updates"></a>Automatische updates inschakelen

U kunt kiezen om Site Recovery voor het beheren van updates in de volgende manieren:-

- [Als onderdeel van de stap van de replicatie inschakelen](#as-part-of-the-enable-replication-step)
- [Schakelen tussen de extensie-instellingen in de kluis bijwerken](#toggle-the-extension-update-settings-inside-the-vault)

### <a name="as-part-of-the-enable-replication-step"></a>Als onderdeel van de stap van de replicatie inschakelen:

Wanneer u replicatie inschakelt voor een virtuele machine te beginnen [uit de weergave van de virtuele machine](azure-to-azure-quickstart.md), of [vanuit de recovery services-kluis](azure-to-azure-how-to-enable-replication.md), krijgt u een optie om te kiezen om een Site Recovery beheren van updates voor de extensie voor Site Recovery of dezelfde handmatig te beheren.

![inschakelen replicatie-automatisch bijwerken](./media/azure-to-azure-autoupdate/enable-rep.png)

### <a name="toggle-the-extension-update-settings-inside-the-vault"></a>Schakelen tussen de extensie-instellingen in de kluis bijwerken

1. Navigeer in de kluis naar **beheren**-> **Site Recovery-infrastructuur**
2. Onder **voor Azure virtual Machines**-> **extensie Update-instellingen**, klikt u op de wisselknop om te kiezen of u wilt toestaan dat *ASR om updates te beheren* of *handmatig beheren*. Klik op **Opslaan**.

![kluis-in-/ uitschakelen-autuo-update](./media/azure-to-azure-autoupdate/vault-toggle.png)

> [!Important] 
> Als u ervoor kiest *ASR toestaan voor het beheren van*, de instelling wordt toegepast op alle virtuele machines in de betreffende kluis.


> [!Note] 
> Zowel de opties ontvangt een melding van het automation-account dat wordt gebruikt voor het beheren van de updates. Als u deze functie voor het eerst in een kluis inschakelen wilt, kunt u een nieuw automation-account wordt gemaakt. Alle volgende inschakelen replicaties in dezelfde kluis gebruikt het eerder gemaakte document.

### <a name="manage-manually"></a>Handmatig beheren

1. Als er nieuwe updates beschikbaar voor de Mobility-Service geïnstalleerd op uw Azure Virtual machines zijn, ziet u een melding die leest "de nieuwe agent bijwerken van Azure Site recovery-replicatie is beschikbaar. Klik om te installeren."

     ![Venster gerepliceerde items](.\media\vmware-azure-install-mobility-service\replicated-item-notif.png)
3. Selecteer de melding om de virtuele machine selectie pagina te openen.
4. Selecteer de virtuele machines die u wilt upgraden van de mobility-service op en selecteer **OK**.

     ![Lijst met items voor VM gerepliceerd](.\media\vmware-azure-install-mobility-service\update-okpng.png)

De Mobility-Service-Update-taak wordt gestart voor elk van de geselecteerde virtuele machines.


## <a name="common-issues--troubleshooting"></a>Veelvoorkomende problemen en probleemoplossing

Als er een probleem met de automatische updates, krijgt u een bericht van dezelfde onder 'Configuratieproblemen' in het dashboard van de kluis. 

U hebt geprobeerd om in te schakelen van automatische updates en is mislukt, Raadpleeg hieronder voor het oplossen van problemen.

**Fout**: U bent niet gemachtigd voor het maken van een Azure uitvoeren als-account (service-principal) en de rol Inzender verlenen aan de service-principal. 
- Aanbevolen actie: Zorg ervoor dat het aangemelde account de inzender voor is toegewezen en probeer het opnieuw. Raadpleeg [dit](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) document voor meer informatie over het toewijzen van de juiste machtigingen.
 
Wanneer automatische updates zijn ingeschakeld, de meeste van de problemen kunnen worden hersteld door de Site Recovery-service en moet u op de '**herstellen**' knop.

![knop herstellen](./media/azure-to-azure-autoupdate/repair.png)

Als de knop herstellen niet beschikbaar is, raadpleegt u het foutbericht dat wordt weergegeven onder het deelvenster voor extensie-instellingen.

 - **Fout**: de uitvoeren als-account beschikt niet over de machtiging voor toegang tot de recovery services-resource.

    **Aanbevolen actie**: verwijderen en vervolgens [opnieuw uitvoeren als-account maken](https://docs.microsoft.com/azure/automation/automation-create-runas-account) of zorg ervoor dat het Automation uitvoeren als-account van Azure Active Directory-toepassing toegang tot de recovery services-resource heeft.

- **Fout**: uitvoeren als-account is niet gevonden. Een van deze is verwijderd of is niet gemaakt: Azure Active Directory-toepassing, Service-Principal, rol, Automation-certificaatasset, Automation-verbindingsasset- of de vingerafdruk is niet identiek certificaat en de verbinding. 

    **Aanbevolen actie**: verwijderen en [en maak opnieuw uitvoeren als-account](https://docs.microsoft.com/azure/automation/automation-create-runas-account).
