---
title: Gegevens herstellen vanaf een back-upserver van Azure | Microsoft Docs
description: Herstel de gegevens die u naar een Recovery Services-kluis hebt beveiligd vanaf een willekeurige Azure-back-up-Server geregistreerd in deze kluis.
services: backup
documentationcenter: 
author: nkolli1
manager: shreeshd
editor: 
ms.assetid: a55f8c6b-3627-42e1-9d25-ed3e4ab17b1f
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: adigan;giridham;trinadhk;markgal
ms.openlocfilehash: 688d155b68bc2d76d53f78d251bc2f659582845f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="recover-data-from-azure-backup-server"></a>Gegevens herstellen vanaf Azure Backup Server
U kunt Azure Backup-Server gebruiken voor het herstellen van de gegevens die u hebt een back-up naar een Recovery Services-kluis. Het proces voor dit zo is geïntegreerd in de beheerconsole voor Azure Backup-Server en is vergelijkbaar met de werkstroom herstellen voor andere Azure Backup-onderdelen.

> [!NOTE]
> In dit artikel is van toepassing voor [System Center Data Protection Manager 2012 R2 met UR7 of hoger] (https://support.microsoft.com/en-us/kb/3065246), gecombineerd met de [meest recente Azure backup-agent](http://aka.ms/azurebackup_agent).
>
>

Gegevens herstellen uit een Azure Backup-Server:

1. Van de **herstel** tabblad van de beheerconsole van Azure Backup-Server, klikt u op **externe DPM toevoegen** (op linksboven op het scherm).   
    ![Externe DPM toevoegen](./media/backup-azure-alternate-dpm-server/add-external-dpm.png)
2. Download nieuwe **vault referenties** uit de kluis die is gekoppeld aan de **Azure Backup-Server** waar de gegevens worden hersteld, kiest u de Azure Backup-Server in de lijst met Azure Backup-Servers geregistreerd met de Recovery Services-kluis en geef de **wachtwoordzin voor versleuteling** gekoppeld aan de server waarvan gegevens worden hersteld.

    ![Externe DPM-referenties](./media/backup-azure-alternate-dpm-server/external-dpm-credentials.png)

   > [!NOTE]
   > Alleen Servers met Azure back-up die is gekoppeld aan dezelfde registratie kluis kunt elkaars gegevens herstellen.
   >
   >

    Nadat de externe Azure Backup-Server is toegevoegd, kunt u de gegevens van de externe server en de lokale Server van Azure back-up van de **herstel** tabblad.
3. De lijst met beschikbare productieservers beveiligd door de externe Server van de Azure-back-up bladeren en selecteer de geschikte gegevensbron.

    ![Bladeren door externe DPM-Server](./media/backup-azure-alternate-dpm-server/browse-external-dpm.png)
4. Selecteer **de maand en jaar** van de **herstelpunten** vervolgkeuzelijst, selecteert u de vereiste **herstel datum** het herstelpunt is gemaakt en selecteer de **Hersteltijd**.

    Een lijst met bestanden en mappen wordt weergegeven in het onderste deelvenster, die kan worden bekeken en op elke locatie kunt herstellen.

    ![Herstelpunten van externe DPM-Server](./media/backup-azure-alternate-dpm-server/external-dpm-recoverypoint.png)
5. Klik met de rechtermuisknop op het desbetreffende item en op **herstellen**.

    ![Externe DPM-herstel](./media/backup-azure-alternate-dpm-server/recover.png)
6. Controleer de **selectie herstellen**. Controleer of de gegevens en tijd van de back-up wordt hersteld, evenals de bron waaruit de back-up is gemaakt. Als de selectie onjuist is, klikt u op **annuleren** terug naar het tabblad herstel naar de juiste herstelpunt selecteren. Als de selectie correct is, klikt u op **volgende**.

    ![Externe DPM recovery samenvatting](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-summary.png)
7. Selecteer **herstellen naar een alternatieve locatie**. **Blader** naar de juiste locatie voor het herstel.

    ![Externe DPM alternatieve herstellocatie](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-alternate-location.png)
8. Kies de optie die betrekking hebben op **kopie maken**, **overslaan**, of **overschrijven**.

   * **Maak kopie** -maakt een kopie van het bestand als er een conflicterende namen.
   * **Overslaan** - als er een conflict met de naam is niet binnen het bestand dat het oorspronkelijke bestand blijft.
   * **Overschrijven** - als er een conflict met de naam van de bestaande kopie van het bestand wordt overschreven.

     Kies de gewenste optie voor **beveiligingsinstellingen voor herstel**. U kunt toepassen op de beveiligingsinstellingen van de doelcomputer waarop de gegevens worden hersteld of de beveiligingsinstellingen die van toepassing op product waren op het moment dat het herstelpunt is gemaakt.

     Bepalen of een **melding** wordt verzonden wanneer het herstel is voltooid.

     ![Externe DPM Recovery meldingen](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-notifications.png)
9. De **samenvatting** scherm toont de opties die tot nu toe is gekozen. Nadat u op **'Herstellen'**, de gegevens kunnen worden hersteld op de juiste on-premises locatie.

    ![Externe DPM Recovery opties samenvatting](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-options-summary.png)

   > [!NOTE]
   > De hersteltaak kan worden bewaakt in de **bewaking** tabblad van de Azure Backup-Server.
   >
   >

    ![Bewaking van herstel](./media/backup-azure-alternate-dpm-server/monitoring-recovery.png)
10. U kunt klikken op **externe DPM wissen** op de **herstel** tabblad van de DPM-server verwijderen van de weergave van de externe DPM-server.

    ![Externe DPM wissen](./media/backup-azure-alternate-dpm-server/clear-external-dpm.png)

## <a name="troubleshooting-error-messages"></a>Foutberichten
| Nee. | Foutbericht | Stappen voor probleemoplossing |
|:---:|:--- |:--- |
| 1. |Deze server is niet geregistreerd bij de kluis die is opgegeven bij de kluisreferentie. |**Oorzaak:** deze fout treedt op wanneer het kluisreferentiebestand geselecteerd behoort niet tot de Recovery Services-kluis die is gekoppeld aan Azure Backup-Server waarop het herstel wordt uitgevoerd. <br> **Oplossing:** Download het kluisreferentiebestand van de Recovery Services-kluis voor de Azure Backup-Server is geregistreerd. |
| 2. |De herstelbare gegevens is niet beschikbaar of de geselecteerde server is niet een DPM-server. |**Oorzaak:** er zijn geen andere Azure back-up-Servers naar de Recovery Services-kluis geregistreerd of de servers zijn nog niet geüpload voor de metagegevens of de geselecteerde server is niet een back-upserver van Azure (ook wel Windows-Server of Windows-Client). <br> **Oplossing:** als er andere Azure-back-up-Servers geregistreerd in de Recovery Services-kluis, zorg ervoor dat de meest recente Azure Backup-agent is geïnstalleerd. <br>Als er dat andere Azure-back-up-Servers geregistreerd in de Recovery Services-kluis, wacht u totdat een dag na de installatie van het herstelproces te starten. De nachtelijke taak gaat u de metagegevens voor alle beveiligde back-ups naar cloud uploaden. De gegevens zijn beschikbaar voor herstel. |
| 3. |Er zijn geen andere DPM-server is geregistreerd in deze kluis. |**Oorzaak:** er zijn geen andere Azure back-up Servers die zijn geregistreerd bij de kluis van waaruit het herstel wordt wordt uitgevoerd.<br>**Oplossing:** als er andere Azure-back-up-Servers geregistreerd in de Recovery Services-kluis, zorg ervoor dat de meest recente Azure Backup-agent is geïnstalleerd.<br>Als er dat andere Azure-back-up-Servers geregistreerd in de Recovery Services-kluis, wacht u totdat een dag na de installatie van het herstelproces te starten. De nachtelijke taak uploadt u de metagegevens voor alle beveiligde back-ups in de cloud. De gegevens zijn beschikbaar voor herstel. |
| 4. |De opgegeven wachtwoordzin voor versleuteling komt niet overeen met de wachtwoordzin die bij de volgende server hoort:**<server name>** |**Oorzaak:** de wachtwoordzin voor versleuteling gebruikt voor het versleutelen van de gegevens van de gegevens van de Azure Backup-Server die wordt hersteld komt niet overeen met de opgegeven wachtwoordzin voor versleuteling. De agent is niet de gegevens ontsleutelen. Daarom wordt het herstel mislukt.<br>**Oplossing:** Geef de exacte dezelfde wachtwoordzin voor versleuteling die zijn gekoppeld aan de Azure Backup-Server waarvan gegevens worden hersteld. |

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="why-cant-i-add-an-external-dpm-server-after-installing-ur7-and-latest-azure-backup-agent"></a>Waarom niet kan ik een externe DPM-server toevoegen na de installatie van UR7 en de meest recente Azure backup-agent?

De DPM-servers met gegevensbronnen die zijn beveiligd met de cloud (met behulp van een updatepakket ouder zijn dan updatepakket 7 bijwerken), moet u ten minste één dag na de installatie van de UR7 en de nieuwste Azure Backup-agent te starten wachten **externe DPM toevoegen server**. De periode van één dag is nodig om de metagegevens van de DPM-beveiligingsgroepen te uploaden naar Azure. Metagegevens van de beveiliging is via elke nacht een taak voor het eerst worden geüpload.

### <a name="what-is-the-minimum-version-of-the-microsoft-azure-recovery-services-agent-needed"></a>Wat is de minimale versie van de Microsoft Azure Recovery Services agent nodig?

De minimumversie van de Microsoft Azure Recovery Services-agent of de Azure backup-agent, vereist voor het inschakelen van deze functie is 2.0.8719.0.  Om weer te geven van de agent-versie: open het Configuratiescherm  **>**  alle Configuratiescherm-items  **>**  programma's en onderdelen  **>**  Microsoft Azure Recovery Services-Agent. Als de versie minder dan 2.0.8719.0 is, downloadt en installeert de [meest recente Azure backup-agent](https://go.microsoft.com/fwLink/?LinkID=288905).

![Externe DPM wissen](./media/backup-azure-alternate-dpm-server/external-dpm-azurebackupagentversion.png)

## <a name="next-steps"></a>Volgende stappen:
• [Veelgestelde vragen over azure Backup](backup-azure-backup-faq.md)
