---
title: Back-up van Windows Azure-machines | Microsoft-Docs
description: Uw Windows-VM's beveiligen door deze back-ups van maken met Azure Backup.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 20760650b093216a2929de580f5971c45e0534a8
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="back-up-windows-virtual-machines-in-azure"></a>Back-up van Windows virtuele machines in Azure

U kunt uw gegevens beschermen door regelmatig back-ups te maken. Azure Backup maakt herstelpunten die zijn opgeslagen in een geografisch redundante recovery kluizen. Wanneer u vanaf een herstelpunt herstelt, kunt u de hele virtuele machine of alleen specifieke bestanden kunt herstellen. In dit artikel wordt uitgelegd hoe een enkel bestand terugzetten naar een virtuele machine met Windows Server- en IIS. Als u een VM voor gebruik van nog geen hebt, kunt u één die gebruikmaakt van de [Windows-snelstartgids](quick-create-portal.md). In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een back-up van een virtuele machine
> * Een dagelijkse back-up plannen
> * Een bestand te herstellen vanaf een back-up




## <a name="backup-overview"></a>Overzicht van Backup

Wanneer de Azure Backup-service een back-uptaak initieert, wordt de Backup-extensie om een momentopname punt in tijd geactiveerd. De Azure Backup-service wordt gebruikt de _VMSnapshot_ extensie. De uitbreiding wordt geïnstalleerd tijdens de eerste VM back-up als de virtuele machine wordt uitgevoerd. Als de virtuele machine niet wordt uitgevoerd, maakt de Backup-service een momentopname van de onderliggende opslag (omdat er geen schrijfbewerkingen toepassing optreden terwijl de virtuele machine is gestopt).

Wanneer een momentopname maken van VM's van Windows, coördineert de Backup-service met de Volume Shadow Copy Service (VSS) om op te halen van een consistente momentopname van de schijven van de virtuele machine. Zodra de Azure Backup-service de momentopname maakt, worden de gegevens worden overgedragen naar de kluis. Als u wilt optimaliseren, wordt de service identificeert en draagt alleen de blokken met gegevens die zijn gewijzigd sinds de vorige back-up.

Wanneer de overdracht van gegevens voltooid is, wordt de momentopname is verwijderd en een herstelpunt wordt gemaakt.


## <a name="create-a-backup"></a>Maak een back-up
U plant als volgt een eenvoudige dagelijkse back-up naar een Recovery Services-kluis. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **Virtuele machines** in het menu aan de linkerkant. 
3. Selecteer in de lijst de virtuele machine waarvan u een back-up wilt maken.
4. Op de blade VM in de **instellingen** sectie, klikt u op **back-up**. De **back-up** blade wordt geopend.
5. In **Recovery Services-kluis**, klikt u op **nieuw** en geef de naam op voor de nieuwe kluis. Een nieuwe kluis wordt gemaakt in dezelfde resourcegroep en locatie als de virtuele machine.
6. Klik op **back-up maken van beleid**. In dit voorbeeld behoud de standaardinstellingen en klik op **OK**.
7. Op de **back-up** blade, klikt u op **back-up inschakelen**. Hiermee maakt u een dagelijkse back-up op basis van het standaardschema.
10. Een eerste herstelpunt maken op de **back-up** Klik op de blade **back-up nu**.
11. Op de **back-up nu** blade, klikt u op het pictogram Agenda, gebruikt u het kalenderbesturingselement selecteren van de laatste dag van dit herstelpunt wordt bewaard en op **back-up**.
12. In de **back-up** blade voor uw virtuele machine, ziet u het aantal herstelpunten die voltooid zijn.

    ![Herstelpunten](./media/tutorial-backup-vms/backup-complete.png)
    
De eerste back-up duurt ongeveer 20 minuten. Ga naar het volgende gedeelte van deze zelfstudie nadat de back-up is voltooid.

## <a name="recover-a-file"></a>Herstellen van een bestand

Als u per ongeluk verwijdert of wijzigingen in een bestand aanbrengen, kunt u bestandsherstel het bestand te herstellen vanuit uw back-upkluis. Bestandsherstel maakt gebruik van een script dat wordt uitgevoerd op de virtuele machine te koppelen van het herstelpunt als lokale schijf. Deze stations blijft gekoppelde 12 uur zodat u kunt bestanden van het herstelpunt kopiëren en deze naar de virtuele machine terugzetten.  

In dit voorbeeld laten we zien hoe de installatiekopiebestand dat wordt gebruikt in de standaardwebpagina voor IIS herstellen. 

1. Open een browser en maak verbinding met het IP-adres van de virtuele machine om de standaard IIS-pagina weer te geven.

    ![IIS-standaardwebpagina](./media/tutorial-backup-vms/iis-working.png)

2. Verbinding maken met de virtuele machine.
3. Open op de virtuele machine **Verkenner** en navigeer naar \inetpub\wwwroot en verwijder het bestand **iisstart.png**.
4. Vernieuw de browser om te zien dat de installatiekopie op de standaard IIS-pagina verwijderd is op de lokale computer.

    ![IIS-standaardwebpagina](./media/tutorial-backup-vms/iis-broken.png)

5. Open een nieuw tabblad op uw lokale computer en Ga de de [Azure-portal](https://portal.azure.com).
6. Selecteer in het menu aan de linkerkant **virtuele machines** en selecteer de virtuele machine in de lijst.
8. Op de blade VM in de **instellingen** sectie, klikt u op **back-up**. De **back-up** blade wordt geopend. 
9. Selecteer in het menu bovenaan de blade **bestandsherstel**. De **bestandsherstel** blade wordt geopend.
10. In **stap 1: Selecteer herstelpunt**, selecteer een herstelpunt in de vervolgkeuzelijst.
11. In **stap 2: downloaden script om te zoeken en herstellen van bestanden**, klikt u op de **uitvoerbaar bestand downloaden** knop. Sla het bestand op uw **downloadt** map.
12. Open op uw lokale computer **Verkenner** en navigeer naar uw **downloadt** map en kopieer het gedownloade .exe-bestand. De bestandsnaam wordt voorafgegaan door de naam van uw virtuele machine. 
13. Plak het .exe-bestand op het bureaublad van uw virtuele machine op de virtuele machine (via de RDP-verbinding). 
14. Navigeer naar het bureaublad van uw virtuele machine en dubbelklik op het .exe. Hiermee wordt een opdrachtprompt en koppel vervolgens het herstelpunt dat als een bestandsshare die toegankelijk is. Wanneer het klaar is met maken van de share, typt u **q** vanaf de opdrachtprompt sluiten.
15. Open op uw virtuele machine, **Verkenner** en navigeer naar de stationsletter die is gebruikt voor de bestandsshare.
16. Navigeer naar \inetpub\wwwroot en kopieer **iisstart.png** uit het bestand delen en plak deze in \inetpub\wwwroot. Bijvoorbeeld: F:\inetpub\wwwroot\iisstart.png Kopieer en plak deze in c:\inetpub\wwwroot het bestand te herstellen.
17. Open het browsertabblad waar u bent verbonden met het IP-adres van de virtuele machine van de pagina van de standaard IIS op de lokale computer. Druk op CTRL + F5 om de browserpagina te vernieuwen. U ziet nu dat de installatiekopie is hersteld.
18. Op de lokale computer terug naar het browsertabblad voor de Azure-portal en in **stap 3: Ontkoppel de schijven na het herstel** klikt u op de **schijven ontkoppelen** knop. Als u vergeet Voer deze stap, is de verbinding met het koppelpunt wordt automatisch sluiten na 12 uur. Na deze 12 uur moet u een nieuw script voor het maken van een nieuwe koppelpunt downloaden.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Maak een back-up van een virtuele machine
> * Een dagelijkse back-up plannen
> * Een bestand te herstellen vanaf een back-up

Ga naar de volgende zelfstudie voor meer informatie over het bewaken van virtuele machines.

> [!div class="nextstepaction"]
> [Virtuele machines bewaken](tutorial-monitoring.md)









