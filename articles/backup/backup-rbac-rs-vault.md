---
title: Back-ups met op rollen gebaseerd toegangsbeheer in Azure beheren '
description: Toegangsbeheer op basis van rollen gebruiken voor het beheren van toegang tot de back-upbeheer bewerkingen in Recovery Services-kluis.
services: backup
author: trinadhk
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 7/11/2018
ms.author: trinadhk
ms.openlocfilehash: 855b75652fca421df12766f7711152d1e3ca2aeb
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009250"
---
# <a name="use-role-based-access-control-to-manage-azure-backup-recovery-points"></a>Toegangsbeheer op basis van rollen gebruiken voor het beheren van Azure Backup-herstelpunten
Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over geavanceerd toegangsbeheer voor Azure. Met op rollen gebaseerd toegangsbeheer kunt u taken scheiden binnen uw team en alleen de mate van toegang verlenen aan gebruikers die nodig is om de taken uit te voeren.

> [!IMPORTANT]
> Geleverd door Azure Backup zijn beperkt tot de acties die kunnen worden uitgevoerd in Azure portal of PowerShell-cmdlets voor Recovery Services-kluis. Acties die worden uitgevoerd in Azure backup Agent Client-gebruikersinterface of System center Data Protection Manager UI of de gebruikersinterface van Azure Backup Server zijn buiten het besturingselement van deze rollen.

Azure Backup biedt 3 ingebouwde rollen voor het beheren van back-beheerbewerkingen. Meer informatie over [ingebouwde Azure RBAC-rollen](../role-based-access-control/built-in-roles.md)

* [Back-Inzender](../role-based-access-control/built-in-roles.md#backup-contributor) -deze rol heeft alle machtigingen voor het maken en beheren van back-up met uitzondering van de Recovery Services-kluis maken en het verlenen van toegang aan anderen. Stel dat u deze rol als beheerder van de back-upbeheer die elke back-upbeheer bewerking kunt uitvoeren.
* [Back-up van Operator](../role-based-access-control/built-in-roles.md#backup-operator) -deze rol heeft machtigingen voor alles wat een inzender met uitzondering van back-up en beheren van back-upbeleidsregels verwijderen. Deze rol is gelijk aan Inzender, behalve het destructieve bewerkingen uitvoeren kunt, zoals back-up stoppen met gegevens of verwijderen van registratie van on-premises resources.
* [Back-up lezer](../role-based-access-control/built-in-roles.md#backup-reader) -deze rol heeft machtigingen voor het weergeven van alle bewerkingen van de back-upbeheer. Stel dat u deze rol niet de bewaking van een persoon.

Als u op zoek bent voor het definiëren van uw eigen rollen voor nog meer controle, Zie hoe u [bouwen van aangepaste rollen](../role-based-access-control/custom-roles.md) in Azure RBAC.



## <a name="mapping-backup-built-in-roles-to-backup-management-actions"></a>Back-up ingebouwde rollen toewijzen aan back-upbeheer acties
De volgende tabel bevat de acties voor back-up en de bijbehorende minimale RBAC-rol om uit te voeren die voor deze bewerking.

| Bewerking voor het beheer | Minimale RBAC-rol vereist |
| --- | --- |
| Recovery Services-kluis maken | Inzender voor de resourcegroep van de kluis |
| Back-up van virtuele Azure-machines inschakelen | Back-upoperator gedefinieerd op het bereik van de resourcegroep met de kluis, Inzender voor virtuele machines op virtuele machines |
| On-demand back-up van virtuele machine | Back-upoperator |
| VM herstellen | Back-upoperator, Resource-groep Inzender waarin de virtuele machine geïmplementeerd, wordt gebruikt op Vnet lezen en neem deel aan op het geselecteerde subnet |
| Herstellen van afzonderlijke bestanden van back-up van VM-schijven | Back-upoperator, Inzender voor virtuele machines op virtuele machines |
| Back-upbeleid voor back-up van virtuele Azure-machine maken | Back-Inzender |
| Back-upbeleid van Azure VM backup wijzigen | Back-Inzender |
| Back-upbeleid van Azure VM backup | Back-Inzender |
| Back-up stoppen (met behoud van gegevens of verwijderen van gegevens) op virtuele machine back-up | Back-Inzender |
| On-premises Windows Server/client-/ SCDPM of Azure Backup-Server registreren | Back-upoperator |
| Geregistreerde on-premises Windows Server/client-/ SCDPM of Azure Backup-Server verwijderen | Back-Inzender |

## <a name="next-steps"></a>Volgende stappen
* [Op rollen gebaseerd toegangsbeheer](../role-based-access-control/role-assignments-portal.md): aan de slag met RBAC in Azure portal.
* Informatie over het beheren van toegang met:
  * [PowerShell](../role-based-access-control/role-assignments-powershell.md)
  * [Azure-CLI](../role-based-access-control/role-assignments-cli.md)
  * [REST API](../role-based-access-control/role-assignments-rest.md)
* [Role-Based Access Control probleemoplossing](../role-based-access-control/troubleshooting.md): Profiteer van tips voor veelvoorkomende problemen oplossen.
