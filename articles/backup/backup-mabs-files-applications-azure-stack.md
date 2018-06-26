---
title: Back-up van bestanden in de Azure-Stack VMs
description: Azure Backup gebruiken voor back-up en herstellen van Azure-Stack-bestanden en toepassingen naar uw Azure-Stack-omgeving.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 6/5/2018
ms.author: adigan
ms.openlocfilehash: 2fb3bad56de781dd81d4c5f82b734c9420c75dee
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751701"
---
# <a name="back-up-files-on-azure-stack"></a>Back-up van bestanden op Azure-Stack
U kunt Azure back-up beveiligen (of back-up) bestanden en toepassingen op Azure-Stack. Als u wilt back-up van bestanden en toepassingen, Microsoft Azure Backup-Server te installeren als een virtuele machine uitgevoerd op Azure-Stack. U kunt de bestanden op elke server Azure-Stack in hetzelfde virtuele netwerk kunt beveiligen. Eenmaal u Azure Backup-Server hebt geïnstalleerd, voegt Azure-schijven voor een verhoging van de lokale opslag beschikbaar voor back-upgegevens op korte termijn. Azure Backup-Server gebruikmaakt van Azure-opslag op lange termijn.

> [!NOTE]
> Hoewel Azure Backup-Server en System Center Data Protection Manager (DPM) zijn vergelijkbaar, wordt DPM wordt niet ondersteund voor gebruik met Azure-Stack.
>

In dit artikel wordt niet uitgelegd voor het installeren van Azure Backup-Server in de Azure-Stack-omgeving. Zie het artikel voor informatie over het installeren van Azure Backup-Server op Azure-Stack [Azure Backup-Server installeren](backup-mabs-install-azure-stack.md).


## <a name="back-up-files-and-folders-in-azure-stack-vms-to-azure"></a>Back-up van bestanden en mappen in Azure-Stack van virtuele machines naar Azure

Open de console Azure Backup-Server voor het configureren van Azure Backup-Server voor het beveiligen van bestanden in de Stack van virtuele machines van Azure virtuele machines. U gebruikt de console voor het configureren van beveiligingsgroepen en de gegevens op uw virtuele machines te beveiligen.

1. Klik in de console Azure Backup-Server op **beveiliging** en klik op de werkbalk op **nieuw** openen de **nieuwe beveiligingsgroep maken** wizard.

   ![Beveiliging configureren in Azure Backup-Server-console](./media/backup-mabs-files-applications-azure-stack/1-mabs-menu-create-protection-group.png)

    Het kan enkele seconden duren voor de wizard te openen. Nadat de wizard wordt geopend, klikt u op **volgende** om door te gaan naar de **Type beveiligingsgroep selecteren** scherm.

   ![Wizard nieuwe beveiligingsgroep wordt geopend](./media/backup-mabs-files-applications-azure-stack/2-create-new-protection-group-wiz.png)

2. Op de **Type beveiligingsgroep selecteren** scherm, kiest u **Servers** en klik op **volgende**.

    ![Wizard nieuwe beveiligingsgroep wordt geopend](./media/backup-mabs-files-applications-azure-stack/3-select-protection-group-type.png)

    De **groepsleden selecteren** scherm wordt geopend. 

    ![Wizard nieuwe beveiligingsgroep wordt geopend](./media/backup-mabs-files-applications-azure-stack/4-opening-screen-choose-servers.png)

3. In de **groepsleden selecteren** scherm, klikt u op **+** aan de lijst met subitems uitvouwen. Schakel het selectievakje in voor alle items die u wilt beveiligen. Nadat u alle items zijn geselecteerd, klikt u op **volgende**.

    ![Wizard nieuwe beveiligingsgroep wordt geopend](./media/backup-mabs-files-applications-azure-stack/5-select-group-members.png)

    Microsoft raadt het plaatsen van alle gegevens die een protection-beleid in een beveiligingsgroep wordt delen. Voor volledige informatie over het plannen en implementeren van beveiligingsgroepen, Zie het artikel System Center DPM [beveiligingsgroepen implementeren](https://docs.microsoft.com/en-us/system-center/dpm/create-dpm-protection-groups?view=sc-dpm-1801).

4. In de **methode voor gegevensbeveiliging selecteren** scherm, typ een naam voor de beveiligingsgroep. Schakel het selectievakje voor **ik wil kortetermijnbeveiliging met:** en **ik wil online beveiliging**. Klik op **Volgende**.

    ![Wizard nieuwe beveiligingsgroep wordt geopend](./media/backup-mabs-files-applications-azure-stack/6-select-data-protection-method.png)

    Selecteer **ik wil online beveiliging**, moet u eerst selecteren **ik wil kortetermijnbeveiliging met:** schijf. Azure Backup-Server biedt geen bescherming op tape, zodat de schijf is alleen de mogelijkheid voor beveiliging op korte termijn.

5. In de **Kortetermijndoelen opgeven** scherm, kiest u hoe lang de herstelpunten die zijn opgeslagen op schijf en het opslaan van incrementele back-ups behouden. Klik op **Volgende**.

    > [!IMPORTANT]
    > U moet **niet** operationele herstelgegevens (back-up) op schijven met Azure Backup-Server gekoppeld voor meer dan vijf dagen bewaren.
    >

    ![Wizard nieuwe beveiligingsgroep wordt geopend](./media/backup-mabs-files-applications-azure-stack/7-select-short-term-goals.png) 

    In plaats van het selecteren van een interval voor incrementele back-ups, voert u een snelle volledige back-up vóór elk gepland herstelpunt, klikt u op **net vóór een herstelpunt**. Als u beveiligt toepassingswerkbelastingen, maakt Azure Backup-Server herstelpunten per de synchronisatieplanning voor de frequentie (op voorwaarde dat de toepassing incrementele back-ups worden ondersteund). Als de toepassing biedt geen ondersteuning voor incrementele back-ups, Azure Backup-Server wordt uitgevoerd voor een snelle volledige back-up.

    Voor **bestand herstelpunten**, Geef op wanneer u om herstelpunten te maken. Klik op **wijzigen** om in te stellen de tijdstippen en dagen van de week waarop herstelpunten zijn gemaakt.

6. In de **Controleer toegewezen schijfruimte** scherm, Controleer de opslaggroepschijfruimte die voor de beveiligingsgroep is toegewezen.

    **Totale grootte van de gegevens** is de grootte van de gegevens die u back wilt-up en **schijfruimte om te worden ingericht** op Azure Backup-Server is de aanbevolen ruimte voor de beveiligingsgroep. Azure Backup-Server kiest de ideale back-volume, op basis van de instellingen. U kunt echter de back-upvolume keuzes in de details van de toewijzing van de schijf bewerken. Selecteer de gewenste opslag in het vervolgkeuzemenu voor de werkbelastingen. De waarden wijzigen voor de totale opslag en vrije opslagruimte op uw bewerkingen in het deelvenster beschikbaar schijfopslag. Underprovisioned ruimte is de hoeveelheid opslag stelt dat u toevoegen aan het volume om door te gaan met back-ups in de toekomst soepel voor Azure Backup-Server.

7. In **methode voor het maken van replica selecteren**, selecteer de manier waarop u wilt de initiële volledige gegevensreplicatie verwerken. Als u besluit om te repliceren via het netwerk, raadt Azure dat u een tijdstip buiten de piekuren kiezen. Voor grote hoeveelheden gegevens of minder dan optimale netwerkomstandigheden, kunt u de gegevens met behulp van verwijderbare media repliceren.

8. In **opties voor consistentiecontrole selecteren**, selecteer de manier waarop u wilt automatiseren consistentiecontroles. Schakel consistentiecontroles alleen wanneer gegevensreplicatie inconsistent wordt of volgens een schema uit te voeren. Als u niet dat automatische consistentiecontrole configureren wilt, moet u een handmatige controle uitvoeren op elk gewenst moment door:
    * In de **beveiliging** gebied van de console Azure Backup-Server met de rechtermuisknop op de beveiligingsgroep en selecteer **consistentiecontrole uitvoeren**.

9. Als u back wilt-up naar Azure, op de **gegevens voor online beveiliging opgeven** pagina Zorg ervoor dat de werkbelastingen die u back wilt-up naar Azure zijn geselecteerd.

10. In **online back-upschema opgeven**, geven wanneer incrementele back-ups naar Azure moeten plaatsvinden. 

    U kunt back-ups elke dag/week/maand/jaar en de tijd/datum waarop ze moeten worden uitgevoerd. Back-ups kunnen maximaal twee keer per dag plaatsvinden. Telkens wanneer die een back-uptaak wordt uitgevoerd, wordt een gegevens-herstelpunt gemaakt in Azure via de kopie van de back-upgegevens opgeslagen op de schijf van de Azure Backup-Server.

11. In **onlinebewaarbeleid opgeven**, opgeven hoe de herstelpunten gemaakt op basis van de dagelijkse/wekelijks/maandelijks/jaarlijks back-ups blijven behouden in Azure.

12. In **kiezen onlinereplicatie**, opgeven hoe de initiële volledige replicatie van gegevens plaatsvindt. 

13. Op **samenvatting**, Controleer uw instellingen. Wanneer u klikt op **groep maken**, de initiële gegevensreplicatie optreedt. Wanneer de gegevensreplicatie is voltooid, op de **Status** pagina de beveiligingsstatus van de groep wordt weergegeven als **OK**. De eerste back-uptaak vindt plaats in overeenstemming met de beveiligingsgroepsinstellingen.

Vragen die u beantwoorden moeten: hoe vouwt u schijfopslag voor kortetermijnschijfruimte Azure-Stack. Wat zijn richtlijnen die worden genoemd moeten, waarin wordt uitgelegd kortetermijnschijfruimte?

## <a name="recover-file-data"></a>Bestandsgegevens herstellen

Back-upserver van Azure-console gebruiken om gegevens naar uw virtuele machine te herstellen.

1. Klik in de console Azure Backup-Server op de navigatiebalk op **herstel** en bladert u naar de gegevens die u wilt herstellen. Selecteer de gegevens in het deelvenster met resultaten.

2. In de kalender in het gedeelte herstelpunten aangeven datums vet herstelpunten beschikbaar zijn. Selecteer de datum voor het herstellen van een herstelpunt wordt gemaakt.

3. In de **herstelbaar item** deelvenster, selecteert u het item dat u wilt herstellen.

4. In de **acties** deelvenster, klikt u op **herstellen** om de Wizard herstel te openen.

5. U kunt gegevens als volgt herstellen:

    * **Herstellen naar oorspronkelijke locatie** -als de clientcomputer is verbonden via VPN, deze optie niet werkt. Gebruik in plaats daarvan een alternatieve locatie en kopieert u gegevens van die locatie.
    * **Herstellen naar een alternatieve locatie**

6. De herstelopties opgeven:

    * Voor **herstelgedrag voor bestaande versie**, selecteer **kopie maken**, **overslaan**, of **overschrijven**. Overschrijven is alleen beschikbaar wanneer naar de oorspronkelijke locatie herstellen.
    * Voor **beveiligingsinstellingen voor herstel**, kies **instellingen van de doelcomputer toepassen** of **de beveiligingsinstellingen van herstelpuntversie toepassen**.
    * Voor **netwerkbandbreedtegebruik**, klikt u op **wijzigen** netwerkbandbreedtegebruik inschakelen.
    * **Melding** klikt u op **e-mail verzenden wanneer dit herstel is voltooid**, en geef de geadresseerden die de melding wordt ontvangen. Scheid de e-mailadressen met komma's.
    * Nadat u de geselecteerd, klikt u op **volgende**

7. Controleer uw herstelinstellingen en klik op **herstellen**. 

    > [!Note] 
    > Terwijl de hersteltaak uitgevoerd wordt, worden alle synchronisatietaken voor de geselecteerde herstelitems geannuleerd.
    >

Als u moderne back-up opslag (MB), wordt niet herstel door eindgebruikers voor File Server (EUR) ondersteund. File Server EUR heeft een afhankelijkheid op Volume Shadow Copy Service (VSS), die geen gebruik maakt van opslag voor moderne back-up. Als het herstel is ingeschakeld, gebruikt u de volgende stappen uit om gegevens te herstellen:

1. Navigeer naar de beveiligde bestanden en met de rechtermuisknop op de naam van het bestand en selecteer **eigenschappen**.

2. Op de **eigenschappen** menu, klikt u op **vorige versies** en kiest u de versie die u wilt herstellen.

## <a name="view-azure-backup-server-with-a-vault"></a>Weergave Azure Backup-Server met een kluis
Als u wilt weergeven entiteiten van de Azure Backup-Server in Azure Portal, kunt u de volgende stappen uit:
1. Open de Recovery Services-kluis.
2. Klik op de back-upinfrastructuur.
3. Weergave van back-beheerservers.

## <a name="see-also"></a>Zie ook
Zie voor informatie over andere werkbelastingen beveiligen met behulp van Azure Backup-Server een van de volgende artikelen:
- [Back-up van SharePoint-farm](https://docs.microsoft.com/en-us/azure/backup/backup-mabs-sharepoint-azure-stack)
- [Back-up van SQL server](https://docs.microsoft.com/en-us/azure/backup/backup-mabs-sql-azure-stack)
