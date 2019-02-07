---
title: Disaster recovery beginselen en de voorbereiding op SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Disaster recovery beginselen en de voorbereiding op SAP HANA op Azure (grote instanties)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 73643cd6954932f933e200baad09e4301300aac2
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55822541"
---
# <a name="disaster-recovery-principles"></a>Principes van herstel na noodgevallen

HANA grote instanties bieden een herstel na noodgevallen tussen HANA grote instantie stempels in verschillende Azure-regio's. Bijvoorbeeld, als u de eenheden HANA grote instantie in de regio VS West van Azure implementeert, kunt u de eenheden HANA grote instantie in de regio VS-Oost als disaster recovery-eenheden. Zoals eerder vermeld, herstel na noodgevallen niet automatisch geconfigureerd, omdat het vereist dat u betaalt voor een andere HANA grote instantie eenheid in de DR-regio. De installatie van noodhersteladapter werkt voor zowel opschalen als scale-out-instellingen. 

In de scenario's tot nu toe is geïmplementeerd, wordt met klanten de eenheid in de DR-regio gebruiken om uit te voeren niet-productiesystemen die gebruikmaken van een geïnstalleerde HANA-exemplaar. De eenheid HANA grote instantie moet van dezelfde SKU hebben als de SKU die wordt gebruikt voor productiedoeleinden. De volgende afbeelding ziet u wat de schijfconfiguratie tussen de server-eenheid in de-Azure-productieregio en de Dr-regio ziet eruit als:

![Configuratie van de DR-installatie uit oogpunt van schijf](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_setup.PNG)

Zoals u in deze afbeelding overzicht, moet u vervolgens een tweede set schijfvolumes rangschikken. De doel-schijfvolumes zijn even groot is als de productievolumes voor de productie-instantie in de disaster recovery-eenheden. Deze volumes op schijven zijn gekoppeld aan de eenheid van de server HANA grote instantie in de site voor noodherstel. De volgende volumes worden gerepliceerd van de productieregio naar de DR-site:

- /hana/data
- / hana/logbackups 
- /Hana/Shared (inclusief/usr/sap)

Het volume /hana/log wordt niet gerepliceerd omdat het transactielogboek van SAP HANA is niet nodig op de manier waarop het terugzetten van die volumes wordt uitgevoerd. 

De basis van het herstel na noodgevallen aangeboden functionaliteit voor de opslag van replicatie wordt aangeboden door de infrastructuur HANA grote instantie. De functionaliteit die wordt gebruikt op de opslag is niet een constante stream met wijzigingen die worden gerepliceerd op asynchrone wijze als wijzigingen in het opslagvolume optreden. In plaats daarvan is het een mechanisme dat is afhankelijk van het feit dat momentopnamen van deze volumes regelmatig worden gemaakt. De verschillen tussen een momentopname van een al gerepliceerde en een nieuwe momentopname die nog niet is gerepliceerd wordt vervolgens overgedragen naar de site voor noodherstel in target-schijfvolumes.  Deze momentopnamen worden opgeslagen op de volumes en, als er een failover van het herstel na noodgevallen moeten worden teruggezet op deze volumes.  

De eerste overdracht van de volledige gegevens van het volume moet zijn voordat de hoeveelheid gegevens kleiner is dan de verschillen tussen momentopnamen wordt. Als gevolg hiervan, bevatten de volumes in de DR-site om een van de volumemomentopnamen uitgevoerd in de productiesite. Uiteindelijk, kunt u dat DR-systeem om te gaan naar een eerdere status verloren gaan om gegevens te herstellen, zonder het terugdraaien van het productiesysteem.

Als er een MCOD-implementatie met meerdere onafhankelijke SAP HANA-instanties in één eenheid van HANA grote instantie, is het waarschijnlijk dat alle SAP HANA-instanties zijn worden gebruikt voor het ophalen van opslag gerepliceerd naar de DR-zijde.

In gevallen waar u HANA System Replication in uw productiesite gebruiken in plaats van de functionaliteit voor hoge beschikbaarheid en opslag op basis van replicatie voor de DR-site gebruikt, zijn de volumes van zowel de knooppunten van de primaire site naar de DR-instantie worden gerepliceerd. U moet extra opslag (dezelfde grootte vanaf primaire knooppunt) op de DR-site voor replicatie van primaire en secundaire naar de DR kopen. 



>[!NOTE]
>De replicatie van HANA grote instantie opslagfunctionaliteit is spiegelen en storage-momentopnamen te repliceren. Als u geen opslagmomentopnamen uitvoert, zoals geïntroduceerd in de sectie back-up en terugzetten van dit artikel, kan dit niet een-replicatie naar de site voor noodherstel. Uitvoering van de momentopname Storage is een vereiste voor storage-replicatie naar de site voor noodherstel.



## <a name="preparation-of-the-disaster-recovery-scenario"></a>De voorbereiding van de noodherstelscenario
In dit scenario hebt u een productiesysteem op HANA grote instanties in de productie van Azure-regio. Voor de stappen die volgen, gaan we ervan uit dat de SID van dat HANA-systeem "PRD" is en dat u hebt een niet-productie-systeem dat wordt uitgevoerd op HANA grote instanties in de DR-Azure-regio. Voor de laatste, gaan we wordt ervan uitgegaan dat de SID "TST." Deze configuratie wordt weergegeven in de volgende afbeelding:

![Begin van herstel na Noodgeval instellen](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start1.PNG)

Als de server-exemplaar is niet al besteld met de extra opslagruimte volume is ingesteld, SAP HANA op Azure Service Management wordt de aanvullende set volumes als een doel voor de productie-replica op de HANA grote instantie-eenheid waarvoor u de TST uitvoert HANA-instantie. Voor dit doel moet u de SID van uw HANA-productie-exemplaar opgeven. Nadat de SAP HANA op Azure Service Management wordt bevestigd dat de bijlage van deze volumes, moet u deze volumes aan de HANA grote instantie eenheid koppelen.

![Volgende stap voor herstel na Noodgeval instellen](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start2.PNG)

De volgende stap is voor installatie van de tweede instantie van SAP HANA op de eenheid HANA grote instantie in de DR-Azure-regio, waar u de TST HANA-exemplaar uitvoeren. De nieuw geïnstalleerde SAP HANA-instantie moet de dezelfde beveiligings-id hebben. De gebruikers die zijn gemaakt, moet dezelfde gebruikers-id en groep-ID die de productie-instantie is. Lezen [back-up en herstel](hana-backup-restore.md) voor meer informatie. Als de installatie is voltooid, moet u naar:

- Stap 2 van de voorbereiding van momentopname wordt beschreven in uitvoeren [back-up en herstel](hana-backup-restore.md).
- Maak een openbare sleutel voor de DR-eenheid van HANA grote instantie als u dat nog niet nog gedaan. Zie stap 3 van de voorbereiding van momentopname wordt beschreven in [back-up en herstel](hana-backup-restore.md).
- Onderhouden de *HANABackupCustomerDetails.txt* met het nieuwe exemplaar van HANA en testen of verbinding naar de opslag goed werkt.  
- Stop de nieuw geïnstalleerde SAP HANA-exemplaar op de eenheid HANA grote instantie in de DR-Azure-regio.
- Ontkoppel deze volumes PRD en neem contact op met SAP HANA op Azure Service Management. De volumes niet kunnen blijven gekoppeld aan de eenheid omdat ze niet toegankelijk tijdens functioneert als opslag ingesteld als replicatiedoel.  

![Herstel na Noodgevallen instellingsstap voordat het tot stand brengen van replicatie](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start3.PNG)

Het operationele team stelt de replicatierelatie tussen de PRD volumes in de productie-Azure-regio en de volumes prd. in de DR-Azure-regio.

>[!IMPORTANT]
>Het volume /hana/log wordt niet gerepliceerd omdat het is niet nodig de gerepliceerde SAP HANA-database herstellen naar een consistente status in de site voor noodherstel.

Vervolgens instellen of aanpassen van de back-upschema van opslag momentopname te krijgen tot uw RTO en RPO in het geval van noodherstel. Om te beperken het beoogde herstelpunt, stelt u de volgende replicatie-intervallen in de service HANA grote instantie:
- Voor de volumes wordt gedekt door de momentopname van het gecombineerde (type momentopname **hana**), ingesteld om te repliceren om de 15 minuten aan de doelen van het volume gelijk opslag in de site voor noodherstel.
- Voor de back-up logboekvolume van de transactie (type momentopname **logboeken**), ingesteld om te repliceren naar de doelen van het volume gelijk opslag in de site voor noodherstel om de 3 minuten.

Om te beperken het beoogde herstelpunt, instellen van het volgende:
- Voer een **hana** type opslag momentopname (Zie ' stap 7: Uitvoeren van momentopnamen') om de 30 minuten naar één uur.
- Voer SAP HANA transactielogboekback-ups om de 5 minuten.
- Voer een **logboeken** typt u een momentopname van elke 5 tot 15 minuten opslag. Met deze Intervalperiode, moet u een RPO van ongeveer 15-25 minuten bereiken.

Met deze installatie, de volgorde van de transactielogboekback-ups, opslagmomentopnamen en de replicatie van de transactie HANA Meld u back-upgegevens voor volume en hana-/ en /hana/shared (inclusief/usr/sap) kan eruitzien als de gegevens op deze afbeelding:

 ![Relatie tussen een transactie logboek back-upmomentopname en een mirror module op een tijdas](./media/hana-overview-high-availability-disaster-recovery/snapmirror.PNG)

Voor het bereiken van een nog betere RPO in het geval van de herstel na noodgevallen, kunt u de HANA transactielogboekback-ups van SAP HANA op Azure (grote instanties) kopiëren naar de andere Azure-regio. Als u wilt deze verdere vermindering van het RPO bereiken, moet u de volgende stappen uitvoeren:

1. Meld u net zo vaak mogelijk /hana/logbackups maakt u een back-up van de HANA-transactie.
1. Rsync gebruiken om te kopiëren van de transactielogboekback-ups naar de NFS-share gehoste virtuele machines van Azure. De virtuele machines zijn in virtuele netwerken van Azure in de-Azure-productieregio en in de DR-regio's. U moet beide virtuele netwerken van Azure verbinden met het circuit verbinding maken met de productie-HANA grote instanties van Azure. Zie de afbeeldingen in de [netwerk overwegingen voor herstel na noodgeval met HANA grote instanties](#Network-considerations-for-disaster recovery-with-HANA-Large-Instances) sectie. 
1. Houd de transactielogboekback-ups in de regio in de virtuele machine die is gekoppeld aan de NFS geëxporteerd opslag.
1. In geval failover na noodgevallen vormen een aanvulling op de transactielogboekback-ups die u vinden op het volume /hana/logbackups met meer recent gebruikte transactielogboekback-ups op de NFS delen in de site voor noodherstel. 
1. Start een transactielogboekback-up te herstellen naar de meest recente back-up die via kan worden opgeslagen in de DR-regio.

Wanneer HANA grote instantie operations Bevestig de instellingen van de relatie replicatie en u de uitvoering van opslagmomentopnamen start, begint de replicatie van gegevens.

![Herstel na Noodgevallen instellingsstap voordat het tot stand brengen van replicatie](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start4.PNG)

Als de replicatie vordert, de momentopnamen op de volumes prd. in de DR-Azure-regio's niet teruggezet worden. Ze worden alleen opgeslagen. Als de volumes zijn gekoppeld in een status heeft, geven ze de status waarin u de volumes ontkoppeld nadat het PRD SAP HANA-exemplaar is geïnstalleerd in de eenheid van de server in de DR-Azure-regio. Ze geven ook de opslag back-ups die nog niet zijn hersteld.

Als er een failover, kunt ook u om een momentopname van een oudere opslag in plaats van de meest recente momentopname van de opslag te herstellen.

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg [failover noodherstelprocedure](hana-failover-procedure.md).
