---
title: Back-up van bestanden in Azure Stack-VM's
description: Gebruik Azure Backup een back-up en herstellen van Azure Stack-bestanden en toepassingen naar uw Azure Stack-omgeving.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 6/5/2018
ms.author: adigan
ms.openlocfilehash: acbd1bb98fd9a3eb24b7b3262c3fe9fe47200385
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579181"
---
# <a name="back-up-files-on-azure-stack"></a>Back-up van bestanden in Azure Stack
U kunt Azure Backup gebruiken om te beveiligen (of een back-up) bestanden en toepassingen in Azure Stack. Als u wilt back-up van bestanden en toepassingen, Microsoft Azure Backup Server te installeren als een virtuele machine die wordt uitgevoerd op Azure Stack. U kunt de bestanden op een willekeurige Azure Stack-server in hetzelfde virtuele netwerk beveiligen. Nadat u Azure Backup-Server hebt geïnstalleerd, voegt Azure-schijven om te verhogen van de lokale opslag beschikbaar voor gegevensback-up op korte termijn. Azure Backup-Server maakt gebruik van Azure storage voor langdurige bewaarperioden.

> [!NOTE]
> Hoewel Azure Backup Server en System Center Data Protection Manager (DPM) vergelijkbaar zijn, wordt DPM wordt niet ondersteund voor gebruik met Azure Stack.
>

Dit artikel wordt niet beschreven voor het installeren van Azure Backup Server in de Azure Stack-omgeving. Zie het artikel voor informatie over het installeren van Azure Backup Server in Azure Stack [installeren van Azure Backup Server](backup-mabs-install-azure-stack.md).


## <a name="back-up-files-and-folders-in-azure-stack-vms-to-azure"></a>Back-up van bestanden en mappen in Azure Stack-VM's naar Azure

Open de Azure Backup Server-console voor het configureren van Azure Backup Server voor het beveiligen van bestanden in virtuele machines van Azure Stack. U gebruikt de console-beveiligingsgroepen configureren en de gegevens op uw virtuele machines te beveiligen.

1. Klik in de Azure Backup Server-console op **Protection** en in de werkbalk op **nieuw** te openen de **nieuwe beveiligingsgroep maken** wizard.

   ![Beveiliging configureren in Azure Backup Server-console](./media/backup-mabs-files-applications-azure-stack/1-mabs-menu-create-protection-group.png)

    Het duurt een paar seconden om de wizard te openen. Nadat de wizard wordt geopend, klikt u op **volgende** om door te gaan naar de **Type beveiligingsgroep selecteren** scherm.

   ![Wizard nieuwe beveiligingsgroep wordt geopend](./media/backup-mabs-files-applications-azure-stack/2-create-new-protection-group-wiz.png)

2. Op de **Type beveiligingsgroep selecteren** scherm, kiest u **Servers** en klikt u op **volgende**.

    ![Wizard nieuwe beveiligingsgroep wordt geopend](./media/backup-mabs-files-applications-azure-stack/3-select-protection-group-type.png)

    De **groepsleden selecteren** scherm wordt geopend. 

    ![Wizard nieuwe beveiligingsgroep wordt geopend](./media/backup-mabs-files-applications-azure-stack/4-opening-screen-choose-servers.png)

3. In de **groepsleden selecteren** scherm, klikt u op **+** om uit te breiden de lijst met subitems. Schakel het selectievakje in voor alle items die u wilt beveiligen. Wanneer alle items zijn geselecteerd, klikt u op **volgende**.

    ![Wizard nieuwe beveiligingsgroep wordt geopend](./media/backup-mabs-files-applications-azure-stack/5-select-group-members.png)

    Microsoft raadt aan om alle gegevens die een beveiligingsbeleid in een beveiligingsgroep delen. Voor volledige informatie over het plannen en implementeren van beveiligingsgroepen, Zie het artikel System Center DPM [beveiligingsgroepen implementeren](https://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups?view=sc-dpm-1801).

4. In de **methode voor gegevensbeveiliging selecteren** scherm, typt u een naam voor de beveiligingsgroep. Schakel het selectievakje voor **ik wil kortetermijnbeveiliging met:** en **ik Kies voor online beveiliging**. Klik op **Volgende**.

    ![Wizard nieuwe beveiligingsgroep wordt geopend](./media/backup-mabs-files-applications-azure-stack/6-select-data-protection-method.png)

    Selecteer **ik Kies voor online beveiliging**, moet u eerst selecteren **ik wil kortetermijnbeveiliging met:** schijf. Azure Backup Server biedt geen bescherming op tape, zodat de schijf is de enige optie voor beveiliging op korte termijn.

5. In de **Kortetermijndoelen opgeven** scherm, kiest u hoe lang de herstelpunten die zijn opgeslagen op schijf, en bij het opslaan van incrementele back-ups behouden. Klik op **Volgende**.

    > [!IMPORTANT]
    > U moet **niet** operationele herstelgegevens (back-up) op Azure Backup-Server-gekoppelde schijven meer dan vijf dagen bewaren.
    >

    ![Wizard nieuwe beveiligingsgroep wordt geopend](./media/backup-mabs-files-applications-azure-stack/7-select-short-term-goals.png) 

    Plaats een interval voor incrementele back-ups, om uit te voeren van een snelle volledige back-up vóór elk gepland herstelpunt, klikt u op **net vóór een herstelpunt**. Als u toepassingswerkbelastingen beveiligt, maakt Azure Backup Server herstelpunten per de synchronisatieplanning voor de frequentie (op voorwaarde dat de toepassing incrementele back-ups worden ondersteund). Als de toepassing biedt geen ondersteuning voor incrementele back-ups, Azure Backup-Server wordt uitgevoerd voor een snelle volledige back-up.

    Voor **bestand herstelpunten**, Geef op wanneer om herstelpunten te maken. Klik op **wijzigen** om in te stellen de tijdstippen en dagen van de week waarop de herstelpunten worden gemaakt.

6. In de **schijftoewijzing controleren** scherm, controleert u de storage pool toegewezen schijfruimte voor de beveiligingsgroep.

    **Totale grootte van de gegevens** is de grootte van de gegevens die u back wilt-up en **schijfruimte die moet worden ingericht** op Azure Backup Server is de aanbevolen ruimte voor de beveiligingsgroep. Azure Backup Server kiest de ideale back-upvolume, op basis van de instellingen. U kunt echter de back-upvolume keuzes in de details van de toewijzing van schijf bewerken. Selecteer de gewenste opslag in het vervolgkeuzemenu voor de werkbelastingen. Uw bewerkingen voor het wijzigen van de waarden voor de totale opslag en vrije opslagruimte in het deelvenster beschikbare schijfopslag. Ingerichte ruimte is de hoeveelheid opslag die Azure Backup Server stelt dat u toevoegen aan het volume om door te gaan met back-ups in de toekomst probleemloos.

7. In **methode voor het maken van replica selecteren**, selecteer de manier waarop u wilt verwerken van de eerste volledige gegevensreplicatie. Als u besluit om te repliceren via het netwerk, raadt Azure u een tijdstip buiten de piekuren. Voor grote hoeveelheden gegevens of minder dan optimale netwerkomstandigheden, kunt u repliceren van gegevens met verwisselbare media.

8. In **opties voor consistentiecontrole selecteren**, selecteer de manier waarop u wilt automatiseren consistentiecontroles. Schakel consistentiecontroles alleen wanneer de replicatie van gegevens inconsistent wordt, of volgens een schema uit te voeren. Als u niet dat automatische consistentiecontrole configureren wilt, moet u een handmatige controle uitvoeren op elk gewenst moment door:
    * In de **Protection** van de Azure Backup Server-console met de rechtermuisknop op de beveiligingsgroep en selecteer **consistentiecontrole uitvoeren**.

9. Als u back wilt-up naar Azure, op de **gegevens voor online beveiliging opgeven** pagina Zorg ervoor dat de werkbelastingen die u back wilt-up naar Azure zijn geselecteerd.

10. In **onlineback-upschema opgeven**, geven wanneer incrementele back-ups naar Azure moeten plaatsvinden. 

    Back-ups uitvoeren elke dag/week/maand/jaar en de tijd en datum waarop u deze wilt uitvoeren, kunt u plannen. Back-ups kunnen maximaal twee keer per dag uitgevoerd. Telkens wanneer die een back-uptaak wordt uitgevoerd, is een herstelpunt voor gegevens gemaakt in Azure vanaf de kopie van de back-upgegevens die zijn opgeslagen op de schijf van de Azure Backup-Server.

11. In **onlinebewaarbeleid opgeven**, opgeven hoe de herstelpunten die zijn gemaakt op basis van de dagelijkse/wekelijkse/maandelijkse/jaarlijkse back-ups worden bewaard in Azure.

12. In **Kies onlinereplicatie**, opgeven hoe de eerste volledige replicatie van gegevens plaatsvindt. 

13. Op **samenvatting**, Controleer uw instellingen. Wanneer u klikt op **groep maken**, vindt de initiële replicatie plaats. Wanneer de replicatie van gegevens is voltooid, op de **Status** pagina, de status van de beveiliging groep wordt weergegeven als **OK**. De eerste back-uptaak vindt plaats in overeenstemming met de beveiligingsgroepsinstellingen.

## <a name="recover-file-data"></a>Bestandsgegevens herstellen

Azure Backup Server-console gebruiken om gegevens naar uw virtuele machine te herstellen.

1. Klik in de Azure Backup Server-console op de navigatiebalk op **Recovery** en bladert u naar de gegevens die u wilt herstellen. Selecteer de gegevens in het deelvenster met resultaten.

2. Op de kalender in het gedeelte herstelpunten geven datums vetgedrukt herstelpunten zijn beschikbaar. Selecteer de datum om te herstellen.

3. In de **herstelbaar item** deelvenster, selecteert u het item dat u wilt herstellen.

4. In de **acties** deelvenster, klikt u op **herstellen** om de Wizard herstel te openen.

5. U kunt gegevens als volgt herstellen:

    * **Herstellen naar oorspronkelijke locatie** -als de clientcomputer is verbonden via VPN, deze optie werkt niet. Gebruik in plaats daarvan een alternatieve locatie en kopieert u gegevens vanaf die locatie.
    * **Herstellen naar een alternatieve locatie**

6. Geef de opties voor herstel:

    * Voor **herstelgedrag voor bestaande versie**, selecteer **kopie maken**, **overslaan**, of **overschrijven**. Overschrijven is alleen beschikbaar wanneer er naar de oorspronkelijke locatie hersteld.
    * Voor **beveiligingsinstellingen voor herstel**, kiest u **instellingen van de doelcomputer toepassen** of **de beveiligingsinstellingen van herstelpuntversie toepassen**.
    * Voor **netwerkbandbreedtegebruik**, klikt u op **wijzigen** netwerkbandbreedtegebruik inschakelen.
    * **Melding** klikt u op **e-mailbericht verzenden wanneer dit herstel is voltooid**, en geef de geadresseerden die de melding wordt ontvangen. Scheid de e-mailadressen met komma's.
    * Nadat u de selecties, klikt u op **volgende**

7. Controleer uw herstelinstellingen en klik op **herstellen**. 

    > [!Note] 
    > Terwijl de hersteltaak uitgevoerd wordt, worden alle synchronisatietaken voor de geselecteerde herstelitems geannuleerd.
    >

Als u Modern Backup Storage (MBS) gebruikt, wordt herstel van bestandsserver door eindgebruikers (EUR) wordt niet ondersteund. Bestand Server EUR heeft een afhankelijkheid op Volume Shadow Copy Service (VSS), die geen gebruik maakt van Modern Backup Storage. Als EUR is ingeschakeld, gebruikt u de volgende stappen uit om gegevens te herstellen:

1. Navigeer naar de beveiligde bestanden en met de rechtermuisknop op de bestandsnaam en selecteer **eigenschappen**.

2. Op de **eigenschappen** menu, klikt u op **vorige versies** en kies de versie die u wilt herstellen.

## <a name="view-azure-backup-server-with-a-vault"></a>Weergave Azure Backup Server met een kluis
Als u wilt weergeven van Azure Backup Server entiteiten in Azure Portal, kunt u de volgende stappen volgen:
1. Open de Recovery Services-kluis.
2. Klik op een back-upinfrastructuur.
3. Weergave back-upbeheer Servers.

## <a name="see-also"></a>Zie ook
Zie een van de volgende artikelen voor meer informatie over het gebruik van Azure Backup Server ter bescherming van andere workloads:
- [Back-up van SharePoint-farm](https://docs.microsoft.com/azure/backup/backup-mabs-sharepoint-azure-stack)
- [Back-up van SQL server](https://docs.microsoft.com/azure/backup/backup-mabs-sql-azure-stack)
