---
title: Gegevens herstellen vanaf een Azure Backup Server
description: Herstelt de gegevens die u hebt beveiligd naar een Recovery Services-kluis vanaf elk Azure Backup Server is geregistreerd bij deze kluis.
services: backup
author: kasinh
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: kasinh
ms.openlocfilehash: 770baeeacb5f3808eba05f9e262bcbca75c6baad
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705216"
---
# <a name="recover-data-from-azure-backup-server"></a>Gegevens herstellen vanaf Azure Backup Server
U kunt Azure Backup Server gebruiken om de gegevens die u een back-naar een Recovery Services-kluis hebt gemaakt te herstellen. Het proces voor het doen dus is geïntegreerd in de beheerconsole voor Azure Backup Server en is vergelijkbaar met de werkstroom herstellen voor andere onderdelen van Azure Backup.

> [!NOTE]
> In dit artikel is van toepassing op [System Center Data Protection Manager 2012 R2 met UR7 of hoger](https://support.microsoft.com/en-us/kb/3065246), gecombineerd met de [meest recente Azure backup-agent](https://aka.ms/azurebackup_agent).
>
>

Gegevens herstellen vanaf een Azure Backup-Server:

1. Uit de **Recovery** tabblad van de beheerconsole van Azure Backup Server, klikt u op **externe DPM toevoegen** (op linksboven in het scherm).   
    ![Externe DPM toevoegen](./media/backup-azure-alternate-dpm-server/add-external-dpm.png)
2. Download nieuwe **kluis referenties** uit de kluis die zijn gekoppeld aan de **Azure Backup Server** waar de gegevens worden hersteld, kiest u de Azure Backup-Server uit de lijst met Azure Backup-Servers geregistreerd met de Recovery Services-kluis en geeft u de **wachtwoordzin voor versleuteling** die zijn gekoppeld aan de server waarvan de gegevens wordt hersteld.

    ![Externe DPM-referenties](./media/backup-azure-alternate-dpm-server/external-dpm-credentials.png)

   > [!NOTE]
   > Alleen Azure back-up-Servers die zijn gekoppeld aan dezelfde registratie kluis kunnen elkaars gegevens herstellen.
   >
   >

    Zodra de externe Azure Backup Server is toegevoegd, kunt u de gegevens van de externe server en de lokale Azure Backup Server uit de **Recovery** tabblad.
3. Blader door de beschikbare lijst productieservers beveiligd door de externe Azure Backup Server en selecteer de geschikte gegevensbron.

    ![Externe DPM-Server bladeren](./media/backup-azure-alternate-dpm-server/browse-external-dpm.png)
4. Selecteer **de maand en jaar** uit de **herstelpunten** vervolgkeuzelijst, selecteert u de vereiste **hersteldatum** voor wanneer het herstelpunt is gemaakt en selecteer de **Hersteltijd**.

    Een lijst met bestanden en mappen wordt weergegeven in het onderste deelvenster, dat kan worden gebladerd en hersteld naar een locatie.

    ![Herstelpunten voor externe DPM-Server](./media/backup-azure-alternate-dpm-server/external-dpm-recoverypoint.png)
5. Klik met de rechtermuisknop op het gewenste item en klikt u op **herstellen**.

    ![Externe DPM-herstel](./media/backup-azure-alternate-dpm-server/recover.png)
6. Controleer de **selectie herstellen**. Controleer of de gegevens en tijd van de back-up wordt hersteld, evenals de bron van waaruit de back-up is gemaakt. Als de selectie onjuist is, klikt u op **annuleren** terug naar het tabblad herstel naar het juiste herstelpunt selecteren. Als de selectie juist is, klikt u op **volgende**.

    ![Externe DPM recovery samenvatting](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-summary.png)
7. Selecteer **herstellen naar een alternatieve locatie**. **Blader** naar de juiste locatie voor het herstel.

    ![Externe DPM recovery alternatieve locatie](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-alternate-location.png)
8. Kies de optie met betrekking tot **kopie maken**, **overslaan**, of **overschrijven**.

   * **Kopie maken** -maakt een kopie van het bestand als er een conflicterende namen.
   * **Overslaan** : als er een conflict met de naam is niet binnen het bestand dat het oorspronkelijke bestand blijft.
   * **Overschrijven** - als er een conflict met de naam van de bestaande kopie van het bestand wordt overschreven.

     Kies de gewenste optie op **beveiligingsinstellingen voor herstel**. U kunt toepassen op de beveiligingsinstellingen van de doelcomputer waar de gegevens worden hersteld of de beveiligingsinstellingen die van toepassing op het product zijn op het moment dat het herstelpunt is gemaakt.

     Bepalen of een **melding** wordt verzonden, wanneer het herstel is voltooid.

     ![Externe DPM Recovery meldingen](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-notifications.png)
9. De **samenvatting** scherm bevat de opties die tot nu toe is gekozen. Nadat u op **'Herstellen'** , de gegevens kunnen worden hersteld op de juiste on-premises locatie.

    ![Externe DPM Recovery opties samenvatting](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-options-summary.png)

   > [!NOTE]
   > De hersteltaak kan worden gecontroleerd in de **bewaking** tabblad van de Azure Backup-Server.
   >
   >

    ![Bewaking van herstel](./media/backup-azure-alternate-dpm-server/monitoring-recovery.png)
10. U kunt klikken op **externe DPM wissen** op de **Recovery** tabblad van de DPM-server verwijdert u de weergave van de externe DPM-server.

    ![Externe DPM wissen](./media/backup-azure-alternate-dpm-server/clear-external-dpm.png)

## <a name="troubleshooting-error-messages"></a>-Foutberichten oplossen
| Nee. | Foutbericht | Stappen voor probleemoplossing |
|:---:|:--- |:--- |
| 1. |Deze server is niet geregistreerd bij de kluis die is opgegeven via de kluisreferentie. |**Oorzaak:** Deze fout treedt op wanneer het kluisreferentiebestand geselecteerd behoort niet tot de Recovery Services-kluis die zijn gekoppeld aan Azure Backup Server waarop het herstel wordt uitgevoerd. <br> **Oplossing:** Download het kluisreferentiebestand van de Recovery Services-kluis waarbij de Azure Backup Server is geregistreerd. |
| 2. |De herstelbare gegevens zijn niet beschikbaar of de geselecteerde server is niet een DPM-server. |**Oorzaak:** Er zijn dat geen andere Azure back-up-Servers geregistreerd bij de Recovery Services-kluis of de servers zijn de metagegevens van de nog niet geüpload of de geselecteerde server is niet een Azure Backup-Server (met behulp van Windows Server of Windows-Client). <br> **Oplossing:** Als er dat andere Azure-back-up-Servers geregistreerd bij de Recovery Services-kluis, zorg ervoor dat de meest recente Azure Backup-agent is geïnstalleerd. <br>Als er dat andere Azure-back-up-Servers geregistreerd bij de Recovery Services-kluis, wacht u een dag na de installatie om het herstelproces te starten. De nachtelijke taak uploadt de metagegevens voor alle de beveiligde back-ups naar de cloud. De gegevens zijn beschikbaar voor herstel. |
| 3. |Er zijn geen andere DPM-server is geregistreerd bij deze kluis. |**Oorzaak:** Er zijn geen andere Azure Backup-Servers die zijn geregistreerd bij de kluis van waaruit het herstel is een poging wordt gedaan.<br>**Oplossing:** Als er dat andere Azure-back-up-Servers geregistreerd bij de Recovery Services-kluis, zorg ervoor dat de meest recente Azure Backup-agent is geïnstalleerd.<br>Als er dat andere Azure-back-up-Servers geregistreerd bij de Recovery Services-kluis, wacht u een dag na de installatie om het herstelproces te starten. De nachtelijke taak uploadt u de metagegevens voor alle beveiligde back-ups naar de cloud. De gegevens zijn beschikbaar voor herstel. |
| 4. |De opgegeven wachtwoordzin voor versleuteling komt niet overeen met de wachtwoordzin die is gekoppeld aan de volgende server:  **\<servernaam >** |**Oorzaak:** De wachtwoordzin voor versleuteling gebruikt bij het versleutelen van de gegevens van de Azure Backup-Server-gegevens die wordt hersteld, komt niet overeen met de opgegeven wachtwoordzin voor versleuteling. De agent is om de gegevens te ontsleutelen. Daarom mislukt het herstellen.<br>**Oplossing:** Geef de exacte dezelfde wachtwoordzin voor versleuteling die zijn gekoppeld aan de Azure Backup-Server waarvan de gegevens wordt hersteld. |

## <a name="next-steps"></a>Volgende stappen

Lees de andere veelgestelde vragen over:

- [Veelgestelde vragen over](backup-azure-vm-backup-faq.md) over back-ups van virtuele Azure-machine
- [Veelgestelde vragen over](backup-azure-file-folder-backup-faq.md) over de Azure backup-agent
