---
title: Problemen met back-ups van Azure-bestanden oplossen
description: Dit artikel gaat over het oplossen van problemen die optreden bij het beveiligen van uw Azure-bestandsshares.
services: backup
ms.service: backup
author: markgalioto
ms.author: markgal
ms.date: 2/21/2018
ms.topic: tutorial
ms.workload: storage-backup-recovery
manager: carmonm
ms.openlocfilehash: 2e067e0a1f673480bc08abfee61d2b1b2c92f885
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-problems-backing-up-azure-files"></a>Problemen oplossen met back-ups maken van Azure-bestanden
U kunt met behulp van de informatie in de volgende tabellen problemen en fouten oplossen die optreden tijdens het back-ups maken van Azure-bestanden.

## <a name="preview-boundaries"></a>Beperkingen van preview
Back-ups maken van Azure-bestanden is een preview-versie. De volgende back-upscenario's worden niet ondersteund voor Azure-bestandsshares:
- Azure-bestandsshares beveiligen in opslagaccounts met replicatie via [zone-redundante opslag](../storage/common/storage-redundancy-zrs.md) (ZRS) of [geografisch redundante opslag met leestoegang](../storage/common/storage-redundancy-grs.md) (RA-GRS).
- Azure-bestandshares beveiligen in opslagaccounts waarvoor virtuele netwerken zijn ingeschakeld.
- Back-ups maken van Azure-bestandsshares met PowerShell of CLI.

### <a name="limitations"></a>Beperkingen
- Maximum aantal geplande back-ups per dag is 1.
- Maximum aantal back-ups op verzoek per dag is 4.
- Gebruik resourcevergrendelingen voor het Storage-account om abusievelijk verwijderen van back-ups uit de Recovery Services-kluis te voorkomen.
- Verwijder geen momentopnamen die met Azure Backup zijn gemaakt. Het verwijderen van momentopnamen kan leiden tot het verlies van herstelpunten en/of herstelfouten

## <a name="configuring-backup"></a>Back-up configureren
De volgende tabel is bedoeld voor het configureren van de back-up:

| Back-up configureren | Tips voor (tijdelijke) oplossing |
| ------------------ | ----------------------------- |
| Kan mijn opslagaccount niet vinden voor het configureren van back-up voor Azure-bestandsshares | <ul><li>Wacht totdat de detectie is voltooid. <li>Controleer of een bestandsshare van het opslagaccount al is beveiligd met een andere Recovery Services-kluis. **Opmerking**: alle bestandsshares in een opslagaccount kunnen in maar één Recovery Services-kluis worden beveiligd. <li>Controleer of de bestandsshare niet aanwezig is in een van de niet-ondersteunde opslagaccounts.|
| Fout in de portal geeft aan dat de detectie van opslagaccounts is mislukt. | Als uw abonnement een partnerabonnement (CSP ingeschakeld) is, kunt u de fout negeren. Als CSP niet is ingeschakeld voor uw abonnement en uw opslagaccounts niet kunnen worden gedetecteerd, neemt u contact op met de ondersteuning.|
| Validatie of registratie van geselecteerd opslagaccount is mislukt.| Voer de bewerking opnieuw uit. Neem contact op met de ondersteuning als het probleem zich blijft voordoen.|
| Kan bestandsshares niet vinden of weergeven in het geselecteerde opslagaccount. | <ul><li> Controleer of het opslagaccount bestaat in de resourcegroep (en niet is verwijderd of verplaatst na de laatste validatie/registratie in de kluis).<li>Controleer of de bestandsshare die u wilt beveiligen niet is verwijderd. <li>Controleer of het opslagaccount een ondersteund opslagaccount voor het maken van back-ups van bestandsshares is.<li>Controleer of de bestandsshare al in dezelfde Recovery Services-kluis wordt beveiligd.|
| Configuratie van de back-up van bestandsshares (of de configuratie van het beveiligingsbeleid) mislukt. | <ul><li>Voer de bewerking opnieuw uit om te kijken of het probleem zich blijft voordoen. <li> Controleer of de bestandsshare die u wilt beveiligen niet is verwijderd. <li> Als u probeert om meerdere bestandsshares tegelijk te beveiligen en sommige bestandsshares mislukken, kunt u de back-up van de mislukte bestandsshares opnieuw proberen te configureren. |
| De Recovery Services-kluis kan niet worden verwijderd nadat de beveiliging van een bestandsshare is gestopt. | Open in Azure Portal **Back-upinfrastructuur** > **Opslagaccounts** en klik op **Registratie opheffen** om het opslagaccount uit de Recovery Services-kluis te verwijderen.|


## <a name="error-messages-for-backup-or-restore-job-failures"></a>Foutberichten voor back-ups of fouten in hersteltaak

| Foutberichten | Tips voor (tijdelijke) oplossing |
| -------------- | ----------------------------- |
| Bewerking is mislukt omdat de bestandsshare niet is gevonden. | Controleer of de bestandsshare die u wilt beveiligen niet is verwijderd.|
| Opslagaccount niet gevonden of wordt niet ondersteund. | <ul><li>Controleer of het opslagaccount bestaat in de resourcegroep en niet uit de resourcegroep is verwijderd of verplaatst na de laatste validatie. <li> Controleer of het opslagaccount een ondersteund opslagaccount voor het maken van back-ups van bestandsshares is.|
| U hebt het maximumaantal momentopnamen voor deze bestandsshare bereikt. U kunt nieuwe maken zodra de oudere zijn verlopen. | <ul><li> Deze fout kan optreden wanneer u meerdere back-ups op aanvraag voor een bestand maakt. <li> Er is een limiet van 200 momentopnamen per bestandsshare, met inbegrip van de opnamen die worden gemaakt door Azure Backup. Oudere geplande back-ups (of momentopnamen) worden automatisch opgeschoond. Back-ups (of momentopnamen) op aanvraag moeten worden verwijderd als de limiet is bereikt.<li> Verwijder de back-ups op aanvraag (momentopnamen van Azure-bestandsshares) uit de Azure Files-portal. **Opmerking**: als u momentopnamen verwijdert die zijn gemaakt door Azure Backup, gaan de herstelpunten verloren. |
| Back-up of terugzetten van bestandsshare is mislukt vanwege een beperking van de opslagservice. Dit komt mogelijk doordat de opslagservice bezig is met het verwerken van andere aanvragen voor het betreffende opslagaccount.| Probeer de bewerking na enige tijd opnieuw. |
| Terugzetten is mislukt met de melding Kan de doelbestandsshare niet vinden. | <ul><li>Controleer of het geselecteerde opslagaccount bestaat en of de doelbestandsshare niet is verwijderd. <li> Controleer of het opslagaccount een ondersteund opslagaccount voor het maken van back-ups van bestandsshares is. |
| Azure Backup wordt op dit moment niet ondersteund voor Azure-bestanden in opslagaccounts waarvoor virtuele netwerken zijn ingeschakeld. | Schakel virtuele netwerken op uw opslagaccount uit om back-ups of herstelbewerkingen goed te laten verlopen. |
| Back-up- of hersteltaken mislukken omdat het opslagaccount is vergrendeld. | Verwijder de vergrendeling van het opslagaccount of gebruik verwijderingsvergrendeling in plaats van leesvergrendeling en probeer het opnieuw. |
| Herstellen mislukt omdat het aantal mislukte bestanden de drempelwaarde overschrijdt. | <ul><li> De oorzaken voor het mislukken van het herstellen worden in een bestand weergegeven (het pad staat in de taakgegevens). Los de fouten op en voer de herstelbewerking alleen opnieuw uit voor de mislukte bestanden. <li> Veelvoorkomende redenen voor fouten bij bestand terugzetten: <br/> -Controleer of de mislukte bestanden niet op dit moment worden gebruikt. <br/> -Er bestaat een map met dezelfde naam als het mislukte bestand in de bovenliggende map. |
| Herstellen mislukt omdat er geen bestand kan worden hersteld. | <ul><li> De oorzaken voor het mislukken van het herstellen worden in een bestand weergegeven (het pad staat in de taakgegevens). Los de fouten op en voer de herstelbewerkingen alleen opnieuw uit voor de mislukte bestanden. <li> Veelvoorkomende redenen voor fouten bij bestand terugzetten: <br/> -Controleer of de mislukte bestanden niet op dit moment worden gebruikt. <br/> -Er bestaat een map met dezelfde naam als het mislukte bestand in de bovenliggende map. |
| Terugzetten mislukt omdat een van de bestanden in de bron niet bestaat. | <ul><li> De geselecteerde items zijn niet aanwezig in de herstelpuntgegevens. Geef de juiste bestandslijst op om de bestanden te herstellen. <li> De momentopname van de bestandsshare die overeenkomt met het herstelpunt wordt handmatig verwijderd. Selecteer een ander herstelpunt en voer de herstelbewerking opnieuw uit. |
| Er is een hersteltaak actief met hetzelfde doel. | <ul><li>Back-ups van bestandsshares ondersteunen geen parallel herstel naar dezelfde doelbestandsshare. <li>Wacht tot de bestaande hersteltaak is voltooid en probeer het opnieuw. Als u geen hersteltaak in de Recovery Services-kluis kunt vinden, controleert u andere Recovery Services-kluizen in hetzelfde abonnement. |
| Herstelbewerking mislukt omdat de doelbestandsshare vol is. | Vergroot het quotum voor grootte van de doelbestandsshare om ruimte te bieden voor de herstelgegevens en probeer het opnieuw. |
| Een of meer bestanden kunnen niet worden hersteld. Controleer de lijst met mislukte bestanden in het pad hierboven voor meer informatie. | <ul> <li> Oorzaken voor het mislukken van de hersteltaak worden in het bestand vermeld (pad staat in de taakgegevens). Los de oorzaken op en voer de herstelbewerking alleen voor de mislukte bestanden opnieuw uit. <li> Veelvoorkomende oorzaken voor fouten bij bestand terugzetten: <br/> -Controleer of de mislukte bestanden niet op dit moment in gebruik zijn. <br/> -Er bestaat een map met dezelfde naam als het mislukte bestand in de bovenliggende map. |

## <a name="see-also"></a>Zie ook
Voor meer informatie over het maken van back-ups van Azure-bestandsshares raadpleegt u de volgende artikelen:
- [Een back-up maken van Azure-bestandsshares](backup-azure-files.md)
- [Back up Azure File share FAQ](backup-azure-files-faq.md) (Veelgestelde vragen over back-ups maken van Azure-bestandsshares)
