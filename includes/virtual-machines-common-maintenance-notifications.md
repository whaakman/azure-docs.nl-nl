---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: include
ms.date: 07/02/2018
ms.author: shants
ms.custom: include file
ms.openlocfilehash: b3f71c4710bd9711a3209dd55f8e680f63627c1b
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50035102"
---
## <a name="view-vms-scheduled-for-maintenance-in-the-portal"></a>Gepland voor onderhoud in de portal van de virtuele machines weergeven

Nadat een geplande onderhoudsgolf is gepland, kunt u de lijst met virtuele machines die worden beïnvloed door de geplande onderhoudsgolf kunt zien. 

U kunt gebruiken de Azure-portal en zoek naar virtuele machines die zijn gepland voor onderhoud.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Klik in het linkernavigatievenster op **virtuele Machines**.

3. Klik in het deelvenster met virtuele Machines op de **kolommen** knop om de lijst van beschikbare kolommen te openen.

4. Selecteer en voeg de volgende kolommen:

   **Onderhoud**: toont de onderhoudsstatus voor de virtuele machine. Hier volgen de mogelijke waarden:
      
      | Waarde | Beschrijving |
      |-------|-------------|
      | Nu beginnen | De virtuele machine is in de selfservice-onderhoud-venster waarmee u het onderhoud zelf initiëren. Zie hieronder voor het starten van onderhoud op de virtuele machine. | 
      | Gepland | Er is onderhoud gepland voor uw virtuele machine, zonder mogelijkheid dit onderhoud zelf te starten. U kunt meer over het onderhoudsvenster door te selecteren van het onderhoud - geplande venster in deze weergave of door te klikken op de virtuele machine. | 
      | Al bijgewerkt | Uw virtuele machine al is bijgewerkt en op dit moment is geen verdere actie vereist. | 
      | Later opnieuw proberen | U hebt gestart onderhoud is mislukt. U kunt zich de optie selfservice-onderhoud gebruiken op een later tijdstip. | 
      | Nu opnieuw proberen | U kunt een eerder mislukte zelf gestart onderhoud opnieuw proberen. | 
      | - | Uw virtuele machine maakt geen deel uit van een geplande onderhoudsgolf. |
      

   **Onderhoud - venster selfservice**: het tijdvenster ziet wanneer u zelf onderhoud op uw virtuele machines starten kunt.
   
   **Onderhoud - venster voor geplande**: bevat het tijdvenster wanneer Azure uw virtuele machine blijft om te kunnen voltooien onderhoud. 



## <a name="notification-and-alerts-in-the-portal"></a>Meldingen en waarschuwingen in de portal

Een schema voor gepland onderhoud communiceert Azure met een e-mailbericht verzenden naar de abonnement-eigenaar en mede-eigenaars-groep. U kunt extra ontvangers en kanalen toevoegen aan deze communicatie met het maken van Azure-activiteit waarschuwingen. Zie voor meer informatie, [abonnement activiteiten met de Azure-activiteitenlogboek](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu aan de linkerkant, **Monitor**. 
3. In de **Monitor - waarschuwingen (klassiek)** deelvenster, klikt u op **+ waarschuwing voor activiteitenlogboek toevoegen**.
5. Vul de gegevens in de **waarschuwing voor activiteitenlogboek toevoegen** pagina en zorg ervoor dat u het volgende instellen in de **Criteria**:
   - **Gebeurteniscategorie**: servicestatus
   - **Services**: Virtual Machine Scale Sets en virtuele Machines
   - **Type**: gepland onderhoud 
    
Zie voor meer informatie over het configureren van waarschuwingen voor activiteitenlogboeken, [waarschuwingen voor activiteitenlogboek maken](../articles/monitoring-and-diagnostics/monitoring-activity-log-alerts.md).
    
    
## <a name="start-maintenance-on-your-vm-from-the-portal"></a>Onderhoud starten op de virtuele machine vanuit de portal

Tijdens het zoeken naar de details van de virtuele machine, kunt u zich voor meer informatie met betrekking tot onderhoud.  
Aan de bovenkant van de detailweergave van de virtuele machine, een nieuwe melding-lint toegevoegd als uw virtuele machine is opgenomen in een geplande onderhoudsgolf. Bovendien wordt een nieuwe optie toegevoegd aan het starten van onderhoud indien mogelijk. 


Klik op de onderhoudsmelding voor de onderhoudspagina met meer informatie over het geplande onderhoud. Van daaruit kunt u zich aan **onderhoud starten** op de virtuele machine.

Als u onderhoud gestart, wordt uw virtuele machine worden onderhouden en de onderhoudsstatus wordt bijgewerkt om het resultaat binnen enkele minuten weer te geven.

Als u het venster selfservice hebt gemist, kunt u zich nog steeds het venster zien wanneer uw virtuele machine wordt onderhouden door Azure. 
