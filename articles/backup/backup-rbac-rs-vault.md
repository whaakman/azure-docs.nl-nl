---
title: Back-ups met op rollen gebaseerd toegangsbeheer in Azure beheren '
description: Toegangsbeheer op basis van rollen gebruiken voor het beheren van toegang tot de back-upbeheer bewerkingen in Recovery Services-kluis.
services: backup
author: trinadhk
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 12/09/2018
ms.author: trinadhk
ms.openlocfilehash: e86595ceb940ebcfa702823e9c9b8ad3ef50bb45
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56674630"
---
# <a name="use-role-based-access-control-to-manage-azure-backup-recovery-points"></a>Toegangsbeheer op basis van rollen gebruiken voor het beheren van Azure Backup-herstelpunten
Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over geavanceerd toegangsbeheer voor Azure. Met op rollen gebaseerd toegangsbeheer kunt u taken scheiden binnen uw team en alleen de mate van toegang verlenen aan gebruikers die nodig is om de taken uit te voeren.

> [!IMPORTANT]
> Geleverd door Azure Backup zijn beperkt tot de acties die kunnen worden uitgevoerd in Azure portal of via REST API of PowerShell of CLI-cmdlets voor Recovery Services-kluis. Acties die worden uitgevoerd in Azure backup Agent Client-gebruikersinterface of System center Data Protection Manager UI of de gebruikersinterface van Azure Backup Server zijn buiten het besturingselement van deze rollen.

Azure Backup biedt 3 ingebouwde rollen voor het beheren van back-beheerbewerkingen. Meer informatie over [ingebouwde Azure RBAC-rollen](../role-based-access-control/built-in-roles.md)

* [Back-Inzender](../role-based-access-control/built-in-roles.md#backup-contributor) -deze rol heeft alle machtigingen voor het maken en beheren van back-up met uitzondering van de Recovery Services-kluis maken en het verlenen van toegang aan anderen. Stel dat u deze rol als beheerder van de back-upbeheer die elke back-upbeheer bewerking kunt uitvoeren.
* [Back-up van Operator](../role-based-access-control/built-in-roles.md#backup-operator) -deze rol heeft machtigingen voor alles wat een inzender met uitzondering van back-up en beheren van back-upbeleidsregels verwijderen. Deze rol is gelijk aan Inzender, behalve het destructieve bewerkingen uitvoeren kunt, zoals back-up stoppen met gegevens of verwijderen van registratie van on-premises resources.
* [Back-up lezer](../role-based-access-control/built-in-roles.md#backup-reader) -deze rol heeft machtigingen voor het weergeven van alle bewerkingen van de back-upbeheer. Stel dat u deze rol niet de bewaking van een persoon.

Als u op zoek bent voor het definiëren van uw eigen rollen voor nog meer controle, Zie hoe u [bouwen van aangepaste rollen](../role-based-access-control/custom-roles.md) in Azure RBAC.



## <a name="mapping-backup-built-in-roles-to-backup-management-actions"></a>Back-up ingebouwde rollen toewijzen aan back-upbeheer acties
De volgende tabel bevat de acties voor back-up en de bijbehorende minimale RBAC-rol om uit te voeren die voor deze bewerking.

| Bewerking voor het beheer | Minimale RBAC-rol vereist | Bereik vereist |
| --- | --- | --- |
| Een Recovery Services-kluis maken | Inzender | Resourcegroep met de kluis |
| Back-up van virtuele Azure-machines inschakelen | Back-upoperator | Resourcegroep met de kluis |
| | Inzender voor virtuele machines | VM-resource |
| On-demand back-up van virtuele machine | Back-upoperator | Recovery vault-resource |
| VM herstellen | Back-upoperator | Recovery Services-kluis |
| | Inzender | Resourcegroep waarin u de virtuele machine wordt geïmplementeerd |
| | Inzender voor virtuele machines | Bron-VM waarvan een back-up is gemaakt |
| Niet-beheerde schijven VM back-up herstellen | Back-upoperator | Recovery vault-resource |
| | Inzender voor virtuele machines | Bron-VM waarvan een back-up is gemaakt |
| | Inzender voor opslagaccounts | Opslagaccountresource waar opstartcyclus schijven kunnen worden hersteld |
| Beheerde schijven terugzetten van back-up van virtuele machine | Back-upoperator | Recovery vault-resource |
| | Inzender voor virtuele machines | Bron-VM waarvan een back-up is gemaakt |
| | Inzender voor opslagaccounts | Tijdelijke Opslagaccount dat is geselecteerd als onderdeel van herstellen voor het opslaan van gegevens uit de kluis voordat u deze te converteren naar beheerde schijven |
| | Inzender | Resourcegroep waartoe de beheerde schijven worden hersteld |
| Afzonderlijke bestanden herstellen vanaf back-up van virtuele machine | Back-upoperator | Recovery vault-resource |
| | Inzender voor virtuele machines | Bron-VM waarvan een back-up is gemaakt |
| Back-upbeleid voor back-up van virtuele Azure-machine maken | Back-upinzender | Recovery vault-resource |
| Back-upbeleid van Azure VM backup wijzigen | Back-upinzender | Recovery vault-resource |
| Back-upbeleid van Azure VM backup | Back-upinzender | Recovery vault-resource |
| Back-up stoppen (met behoud van gegevens of verwijderen van gegevens) op virtuele machine back-up | Back-upinzender | Recovery vault-resource |
| On-premises Windows Server/client-/ SCDPM of Azure Backup-Server registreren | Back-upoperator | Recovery vault-resource |
| Geregistreerde on-premises Windows Server/client-/ SCDPM of Azure Backup-Server verwijderen | Back-upinzender | Recovery vault-resource |

> [!IMPORTANT]
> Als u VM-Inzender bij een VM-resource-bereik opgeven en klik op back-up als onderdeel van de VM-instellingen, wordt deze 'Back-up inschakelen' scherm geopend, zelfs als de virtuele machine is al back-up gemaakt als de aanroep om te controleren of de status van de back-up werkt alleen op abonnementsniveau. Om dit te voorkomen, hetzij gaat u naar de kluis en opent u de back-upitem weergave van de virtuele machine of geef de rol Inzender voor virtuele machine op een abonnementsniveau. 

## <a name="next-steps"></a>Volgende stappen
* [Op rollen gebaseerd toegangsbeheer](../role-based-access-control/role-assignments-portal.md): Aan de slag met RBAC in Azure portal.
* Informatie over het beheren van toegang met:
  * [PowerShell](../role-based-access-control/role-assignments-powershell.md)
  * [Azure-CLI](../role-based-access-control/role-assignments-cli.md)
  * [REST API](../role-based-access-control/role-assignments-rest.md)
* [Role-Based Access Control probleemoplossing](../role-based-access-control/troubleshooting.md): Profiteer van tips voor veelvoorkomende problemen oplossen.
