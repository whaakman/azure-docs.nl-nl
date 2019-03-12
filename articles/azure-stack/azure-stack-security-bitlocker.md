---
title: Data-at-Rest versleuteling in Azure Stack
description: Leer hoe u Azure Stack beschermt uw gegevens met versleuteling-at-rest
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 03/11/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.lastreviewed: 03/11/2019
keywords: ''
ms.openlocfilehash: 018b8f6cf4fc5d3cd380535fca71a038b7fd4208
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57769376"
---
# <a name="data-at-rest-encryption-in-azure-stack"></a>Data-at-rest versleuteling in Azure Stack

Azure Stack beveiligt gebruikers- en infrastructuur op het niveau van het subsysteem voor opslag met behulp van versleuteling at-rest. Subsysteem voor opslag van Azure Stack is versleuteld met BitLocker met 128-bits AES-versleuteling. BitLocker-sleutels zijn opgeslagen in een interne geheime store.

Data-at-rest versleuteling is een algemene vereiste voor veel van de belangrijkste nalevingsstandaarden (bijvoorbeeld PCI-DSS, FedRAMP, HIPAA). Azure Stack kunt u voldoen aan deze vereisten zonder extra werk-of configuraties die nodig zijn. Voor meer informatie over hoe u Azure Stack voldoen aan de standaarden voor compliance kunt, Zie de [Microsoft Service Trust Portal](https://aka.ms/AzureStackCompliance).

> [!NOTE]
> Data-at-rest versleuteling beveiligt uw gegevens tegen door iemand die fysiek zou een of meer harde schijven wordt geopend. Data-at-rest versleuteling biedt geen bescherming tegen de gegevens in het algemeen worden onderschept via het netwerk (gegevens die onderweg zijn), die momenteel wordt gebruikt gegevens (gegevens in het geheugen) of meer gegevens worden exfiltrated terwijl het systeem actief is en wordt uitgevoerd.

## <a name="retrieving-bitlocker-recovery-keys"></a>Bij het ophalen van BitLocker-sleutels voor herstel

Azure Stack-BitLocker-sleutels voor data-at-rest worden intern beheerd. U bent niet verplicht om dit te regelen voor normale bewerkingen of tijdens het opstarten van het systeem. Ondersteuning voor scenario's vereisen echter de herstelsleutels BitLocker op het systeem online brengen.  

> [!WARNING]
> De BitLocker-herstel-sleutels ophalen en op te slaan op een veilige locatie buiten Azure Stack. Omdat u niet de herstelsleutels hoeft tijdens bepaalde scenario's kan leiden tot verlies van gegevens en vereisen dat een systeem herstellen vanaf een back-up.

Bij het ophalen van de BitLocker-herstelsleutel is vereist voor toegang tot de [bevoegde eindpunt](azure-stack-privileged-endpoint.md) (PEP). Voer de cmdlet Get-AzsRecoveryKeys vanuit een sessie PEP.

```powershell
##This cmdlet retrieves the recovery keys for all the volumes that are encrypted with BitLocker.
Get-AzsRecoveryKeys
```

Optionele parameters voor *Get-AzsRecoveryKeys* cmdlet:

| Parameter | Description | Type | Vereist |
|---------|---------|---------|---------|
|*onbewerkte* | retourneert de onbewerkte gegevens van de toewijzing tussen de herstelsleutel, computernaam en wachtwoord op voor elk volume dat is gecodeerd  | Switch | geen (ontworpen voor ondersteuning van scenario's)|


## <a name="troubleshoot-issues"></a>Problemen oplossen

In uitzonderlijke gevallen kan een met BitLocker ontgrendelen aanvraag kan mislukken, wat resulteert in een bepaald volume niet opstarten. Afhankelijk van de beschikbaarheid van een aantal van de onderdelen van de architectuur van kan deze fout leiden tot downtime en gegevensverlies als u de BitLocker-herstel-sleutels hebt.

> [!WARNING]
> De BitLocker-herstel-sleutels ophalen en op te slaan op een veilige locatie buiten Azure Stack. Omdat u niet de herstelsleutels hoeft tijdens bepaalde scenario's kan leiden tot verlies van gegevens en vereisen dat een systeem herstellen vanaf een back-up.

Als u vermoedt dat uw systeem problemen met BitLocker, zoals Azure Stack niet ondervindt te starten, moet u contact op met ondersteuning. -Ondersteuning vereist de BitLocker-herstelsleutels. Het merendeel van de BitLocker gerelateerde problemen worden opgelost met een FRU-bewerking voor die specifieke virtuele machine / / hostvolume. Voor de andere gevallen kan een handmatige ontgrendelen procedure met behulp van BitLocker-herstelsleutels worden uitgevoerd. Als BitLocker-sleutels voor herstel niet beschikbaar zijn, is de enige optie om te herstellen vanaf een back-up. Afhankelijk van wanneer de laatste back-up is uitgevoerd, kunnen gegevens verloren gaan optreden.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over Azure Stack-beveiliging](azure-stack-security-foundations.md)
- Zie voor meer informatie over hoe BitLocker worden beveiligd met CSV's, [beveiligen cluster shared volumes en storage area network met BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/protecting-cluster-shared-volumes-and-storage-area-networks-with-bitlocker).