---
title: Problemen in Azure Data Box Disk oplossen | Microsoft Docs
description: Hierin wordt beschreven hoe u problemen in Azure Data Box Disk oplost.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/30/2018
ms.author: alkohli
ms.openlocfilehash: 1ae6d3dbd01d2623fef511ed0663270ac605f257
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39362657"
---
# <a name="troubleshoot-issues-in-azure-data-box-disk-preview"></a>Problemen in Azure Data Box Disk oplossen (preview)

Dit artikel is van toepassing op de preview-versie van Microsoft Azure Data Box. In dit artikel worden enkele van de complexe werkstromen en beheertaken beschreven die kunnen worden uitgevoerd in de Data Box en Data Box Disk. 

U kunt de Data Box Disk beheren via de Azure-portal. Dit artikel richt zich op de taken die u kunt uitvoeren met behulp van de Azure-portal. Gebruik de Azure-portal voor het beheren van orders, het beheren van apparaten en het bijhouden van de status van de order als deze naar de voltooiingsfase gaat.

Dit artikel bevat de volgende zelfstudies:

- Logboeken met diagnostische gegevens downloaden
- Logboeken met queryactiviteit


> [!IMPORTANT]
> Data Box is in de preview-fase. Lees de [Azure-gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing implementeert.

## <a name="download-diagnostic-logs"></a>Logboeken met diagnostische gegevens downloaden

Als er fouten optreden tijdens het kopiëren van gegevens, geeft de portal een pad weer naar de map waarin de logboeken met diagnostische gegevens zich bevinden. 

De soorten diagnostische logboeken van Azure zijn:
- Foutenlogboeken
- Uitgebreide logboeken  

Als u naar het pad met een kopie van het logboek wilt navigeren, gaat u naar het dat is gekoppeld aan uw Data Box-order. 

1.  Ga naar **Algemeen > Ordergegevens** en noteer het opslagaccount dat is gekoppeld aan uw order.
 

2.  Ga naar **Alle resources** en zoek naar het opslagaccount dat in de vorige stap is aangegeven. Selecteer en klik op het opslagaccount.

    ![Logboeken kopiëren 1](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs1.png)

3.  Ga naar **Blob-service > Blobs verkennen** en zoek naar de blob die overeenkomt met het opslagaccount. Ga naar **diagnosticslogcontainer > waies**. 

    ![Logboeken kopiëren 2](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs2.png)

    U ziet zowel de foutenlogboeken als de uitgebreide logboeken voor het kopiëren van gegevens. Selecteer en klik op elk bestand en download vervolgens een lokale kopie.

## <a name="query-activity-logs"></a>Logboeken met queryactiviteit

Gebruik de activiteitenlogboeken om fouten te vinden bij foutoplossing of om te controleren hoe een gebruiker in uw organisatie een resource heeft gewijzigd. Met activiteitenlogboeken kunt u het volgende bepalen:

- Welke bewerkingen zijn uitgevoerd voor de resources in uw abonnement.
- Wie de bewerking heeft gestart. 
- Wanneer de bewerking is uitgevoerd.
- De status van de bewerking.
- De waarden van andere eigenschappen die u kunnen helpen bij het onderzoeken van het probleem.

In het activiteitenlogboek staan alle schrijfbewerkingen (zoals PUT, POST, DELETE) die op uw resources zijn uitgevoerd, maar er staan geen leesbewerkingen (zoals GET) in. 

Activiteitenlogboeken worden gedurende 90 dagen bewaard. U kunt een query uitvoeren voor een willekeurig bereik van datums, zolang de begindatum niet verder dan 90 dagen in het verleden is. U kunt ook filteren op een van de ingebouwde query's in Insights. Klik bijvoorbeeld op een fout en selecteer vervolgens specifieke problemen om de hoofdoorzaak te begrijpen.

## <a name="data-box-disk-unlock-tool-errors"></a>Fouten met het ontgrendelingsprogramma voor Data Box Disk


| Fout bericht/gedrag hulpprogramma      | Aanbevelingen                                                                                               |
|-------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| Geen<br><br>Het ontgrendelingsprogramma voor Data Box Disk loopt vast.                                                                            | BitLocker is niet geïnstalleerd. Zorg ervoor dat het hulpprogramma BitLocker is geïnstalleerd op de computer waarop het ontgrendelingsprogramma voor Data Box Disk wordt uitgevoerd.                                                                            |
| Het huidige .NET Framework wordt niet ondersteund. De ondersteunde versies zijn 4.5 of hoger.<br><br>Hulpprogramma wordt beëindigd met een bericht.  | .NET 4.5 is niet geïnstalleerd. Installeer .NET 4.5 of hoger op de hostcomputer waarop het ontgrendelingsprogramma voor Data Box Disk wordt uitgevoerd.                                                                            |
| Kan geen volumes ontgrendelen of verifiëren. Neem contact op met Microsoft Ondersteuning.  <br><br>Vergrendelde stations kunnen niet worden ontgrendeld of geverifieerd door het hulpprogramma. | De vergrendelde stations kunnen niet worden ontgrendeld of geverifieerd door het hulpprogramma met de verstrekte wachtwoordsleutel. Neem contact op met Microsoft Ondersteuning voor volgende stappen.                                                |
| De volgende volumes zijn ontgrendeld en geverifieerd. <br>Volumestationsletters: E:<br>Er konden geen volumes worden ontgrendeld met de volgende wachtwoordsleutels: werwerqomnf, qwerwerqwdfda <br><br>Door het hulpprogramma worden sommige stations ontgrendeld en worden de letters van stations waarbij het is mislukt weergegeven.| Gedeeltelijk geslaagd. Sommige van de stations kunnen niet worden ontgrendeld met de verstrekte wachtwoordsleutel. Neem contact op met Microsoft Ondersteuning voor volgende stappen. |
| De vergrendelde volumes zijn niet gevonden. Controleer of de schijf die is ontvangen van Microsoft op de juiste manier is aangesloten en is vergrendeld.          | Er zijn geen vergrendelde stations gevonden door het hulpprogramma. De stations zijn al ontgrendeld of niet gedetecteerd. Zorg ervoor dat de stations zijn verbonden en zijn vergrendeld.                                                           |
| Onherstelbare fout: ongeldige parameter<br>Parameternaam: invalid_arg<br>GEBRUIK:<br>DataBoxDiskUnlock /PassKeys:<passkey_list_separated_by_semicolon><br><br>Voorbeeld: DataBoxDiskUnlock /PassKeys:passkey1; passkey2; passkey3<br>Voorbeeld: DataBoxDiskUnlock /SystemCheck<br>Voorbeeld: DataBoxDiskUnlock/Help<br><br>/PassKeys:       Haal deze wachtwoordsleutel op vanuit de Azure DataBox Disk-order. De wachtwoordsleutel ontgrendelt uw schijven.<br>/ Help:           Deze optie biedt Help bij het gebruik van cmdlets en voorbeelden.<br>/ SystemCheck:     met deze optie wordt gecontroleerd of uw systeem voldoet aan de vereisten voor het uitvoeren van het hulpprogramma.<br><br>Druk op een willekeurige toets om af te sluiten. | Ongeldige parameter ingevoerd. De enige toegestane parameters zijn /SystemCheck, /PassKey en /Help.                                                                            |
## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Data Box Disk beheren via Azure Portal](data-box-portal-ui-admin.md).
