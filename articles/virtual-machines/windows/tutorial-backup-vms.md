---
title: Back-up van Windows Azure-machines | Microsoft-Docs
description: Uw Windows-VM's beveiligen door deze back-ups van maken met Azure Backup.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 12859bf967cf8de1b57ab9dfd5c0bd080806f2eb
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="back-up-windows-virtual-machines-in-azure"></a>Back-up van Windows virtuele machines in Azure

U kunt uw gegevens beschermen door regelmatig back-ups te maken. Azure Backup maakt herstelpunten die worden opgeslagen in geografisch redundante Recovery Services-kluizen. Wanneer u vanaf een herstelpunt herstelt, kunt u de hele VM of alleen specifieke bestanden herstellen. In dit artikel wordt uitgelegd hoe een enkel bestand terugzetten naar een virtuele machine met Windows Server- en IIS. Als u een VM voor gebruik van nog geen hebt, kunt u één die gebruikmaakt van de [Windows-snelstartgids](quick-create-portal.md). In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een back-up maken van een VM
> * Een dagelijkse back-up plannen
> * Een bestand herstellen vanaf een back-up




## <a name="backup-overview"></a>Overzicht van Backup

Wanneer de Azure Backup-service een back-uptaak initieert, wordt de Backup-extensie om een momentopname punt in tijd geactiveerd. De Azure Backup-service wordt gebruikt de _VMSnapshot_ extensie. De extensie is geïnstalleerd tijdens de eerste VM-back-up als de VM actief is. Als de VM niet actief is, wordt met de Backup-service een momentopname gemaakt van de onderliggende opslag (aangezien er geen schrijfbewerkingen van toepassingen plaatsvinden als de VM is gestopt).

Wanneer een momentopname maken van VM's van Windows, coördineert de Backup-service met de Volume Shadow Copy Service (VSS) om op te halen van een consistente momentopname van de schijven van de virtuele machine. Nadat de momentopname is gemaakt met de Azure Backup-service, worden de gegevens overgedragen naar de kluis. Voor maximale efficiëntie wordt met de service geïdentificeerd welke gegevensblokken sinds de vorige back-up zijn gewijzigd. Alleen deze worden vervolgens overgedragen.

Wanneer de gegevensoverdracht is voltooid, wordt de momentopname verwijderd en wordt er een herstelpunt gemaakt.


## <a name="create-a-backup"></a>Een back-up maken
U plant als volgt een eenvoudige dagelijkse back-up naar een Recovery Services-kluis. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **Virtuele machines** in het menu aan de linkerkant. 
3. Selecteer in de lijst de virtuele machine waarvan u een back-up wilt maken.
4. Klik in de sectie **Instellingen** van de VM-blade op **Back-up**. De blade **Back-up inschakelen** wordt geopend.
5. Klik in **Recovery Services-kluis** op **Nieuwe maken** en geef de naam op voor de nieuwe kluis. Een nieuwe kluis wordt gemaakt in dezelfde resourcegroep en op dezelfde locatie als de virtuele machine.
6. Klik op **Back-upbeleid**. Handhaaf voor dit voorbeeld de standaardwaarden en klik op **OK**.
7. Klik op de blade **Back-up inschakelen** op **Back-up inschakelen**. Hiermee maakt u een dagelijkse back-up op basis van het standaardschema.
10. Klik op de blade **Back-up** op **Nu een back-up maken** om een initieel herstelpunt te maken.
11. Klik op de blade **Nu een back-up maken** op het kalenderpictogram en selecteer in de kalender de laatste dag dat dit herstelpunt wordt bewaard. Klik vervolgens op **Back-up**.
12. Op de blade **Backup** voor uw VM ziet u het aantal herstelpunten dat is voltooid.

    ![Herstelpunten](./media/tutorial-backup-vms/backup-complete.png)
    
De eerste back-up duurt ongeveer 20 minuten. Ga nadat de back-up is voltooid verder met het volgende gedeelte van deze zelfstudie.

## <a name="recover-a-file"></a>Herstellen van een bestand

Als u een bestand per ongeluk verwijdert of onbedoelde wijzigingen in een bestand aanbrengt, kunt u het bestand herstellen vanuit de back-upkluis. Bestandsherstel maakt gebruik van een script dat wordt uitgevoerd op de virtuele machine te koppelen van het herstelpunt als lokale schijf. Deze stations blijven gedurende 12 uur gekoppeld zodat u bestanden vanaf het herstelpunt kunt kopiëren en herstellen op de VM.  

In dit voorbeeld laten we zien hoe de installatiekopiebestand dat wordt gebruikt in de standaardwebpagina voor IIS herstellen. 

1. Open een browser en maak verbinding met het IP-adres van de virtuele machine om de standaard IIS-pagina weer te geven.

    ![IIS-standaardwebpagina](./media/tutorial-backup-vms/iis-working.png)

2. Verbinding maken met de virtuele machine.
3. Open op de virtuele machine **Verkenner** en navigeer naar \inetpub\wwwroot en verwijder het bestand **iisstart.png**.
4. Vernieuw de browser om te zien dat de installatiekopie op de standaard IIS-pagina verwijderd is op de lokale computer.

    ![IIS-standaardwebpagina](./media/tutorial-backup-vms/iis-broken.png)

5. Open een nieuw tabblad op uw lokale computer en Ga de de [Azure-portal](https://portal.azure.com).
6. Selecteer in het menu aan de linkerkant **virtuele machines** en selecteer de virtuele machine in de lijst.
8. Klik in de sectie **Instellingen** van de VM-blade op **Back-up**. De blade **Back-up** wordt geopend. 
9. Selecteer in het menu boven aan de blade de optie **Bestandsherstel**. De blade **Bestandsherstel** wordt geopend.
10. Selecteer in **Stap 1: Herstelpunt selecteren** een herstelpunt in de vervolgkeuzelijst.
11. Klik in **Stap 2: Script downloaden om naar bestanden te zoeken en ze te herstellen** op de knop **Uitvoerbaar bestand downloaden**. Sla het bestand op uw **downloadt** map.
12. Open op uw lokale computer **Verkenner** en navigeer naar uw **downloadt** map en kopieer het gedownloade .exe-bestand. De bestandsnaam wordt voorafgegaan door de naam van uw virtuele machine. 
13. Plak het .exe-bestand op het bureaublad van uw virtuele machine op de virtuele machine (via de RDP-verbinding). 
14. Navigeer naar het bureaublad van uw virtuele machine en dubbelklik op het .exe. Hiermee wordt een opdrachtprompt en koppel vervolgens het herstelpunt dat als een bestandsshare die toegankelijk is. Wanneer het klaar is met maken van de share, typt u **q** vanaf de opdrachtprompt sluiten.
15. Open op uw virtuele machine, **Verkenner** en navigeer naar de stationsletter die is gebruikt voor de bestandsshare.
16. Navigeer naar \inetpub\wwwroot en kopieer **iisstart.png** uit het bestand delen en plak deze in \inetpub\wwwroot. Bijvoorbeeld: F:\inetpub\wwwroot\iisstart.png Kopieer en plak deze in c:\inetpub\wwwroot het bestand te herstellen.
17. Open het browsertabblad waar u bent verbonden met het IP-adres van de virtuele machine van de pagina van de standaard IIS op de lokale computer. Druk op Ctrl+F5 om de browserpagina te vernieuwen. U ziet nu dat de installatiekopie is hersteld.
18. Ga op de lokale computer terug naar het browsertabblad voor Azure Portal en klik in **Stap 3: De schijven ontkoppelen na het herstel** op de knop **Schijven ontkoppelen**. Als u vergeet Voer deze stap, is de verbinding met het koppelpunt wordt automatisch sluiten na 12 uur. Na deze 12 uur moet u een nieuw script downloaden voor het maken van een nieuw koppelpunt.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een back-up maken van een VM
> * Een dagelijkse back-up plannen
> * Een bestand herstellen vanaf een back-up

Ga naar de volgende zelfstudie voor meer informatie over het controleren van virtuele machines.

> [!div class="nextstepaction"]
> [Virtuele machines beheren](tutorial-govern-resources.md)









