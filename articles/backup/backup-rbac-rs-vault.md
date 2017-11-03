---
title: Back-ups met op rollen gebaseerd toegangsbeheer van Azure beheren | Microsoft Docs
description: Gebruik toegangsbeheer op basis van rollen voor het beheren van toegang tot de back-beheerbewerkingen in Recovery Services-kluis.
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 3bd46b97-4b29-47a5-b5ac-ac174dd36760
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/22/2017
ms.author: trinadhk;markgal
ms.openlocfilehash: b6e4c6761e1bd5c17c9c3428491113042d3b1d31
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-role-based-access-control-to-manage-azure-backup-recovery-points"></a>Toegangsbeheer op basis van rollen voor het beheren van herstelpunten voor Azure Backup gebruiken
Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over geavanceerd toegangsbeheer voor Azure. Met RBAC kunt u taken scheiden binnen uw team en de hoeveelheid toegang verlenen aan gebruikers die ze nodig hebben voor het uitvoeren van hun taken.

> [!IMPORTANT]
> Rollen die worden geleverd door Azure Backup zijn beperkt tot de acties die kunnen worden uitgevoerd in Azure-portal of PowerShell-cmdlets voor Recovery Services-kluis. Acties die worden uitgevoerd in Azure backup Agent Client-gebruikersinterface of System center Data Protection Manager UI of de gebruikersinterface van Azure Backup-Server aanwezig zijn buiten de controle van deze rollen.

Azure Backup biedt 3 ingebouwde functies voor het beheren van de Backup-beheertaken uit te voeren. Meer informatie over [ingebouwde Azure RBAC-rollen](../active-directory/role-based-access-built-in-roles.md)

* [Back-up Inzender](../active-directory/role-based-access-built-in-roles.md#backup-contributor) -deze rol heeft alle machtigingen maken en beheren van back-up, behalve de Recovery Services-kluis maken en toegang geven aan anderen. Denk aan deze rol als beheerder van back-management die elke back-management-bewerking kunt uitvoeren.
* [Back-up Operator](../active-directory/role-based-access-built-in-roles.md#backup-operator) -deze rol machtigingen heeft voor alles Inzender behalve back-up en het beheer van back-upbeleid te verwijderen. Deze rol is gelijk aan Inzender, behalve het destructieve bewerkingen niet uitvoeren, zoals stop back-up met gegevens verwijderen of verwijder de registratie van lokale bronnen.
* [Back-up lezer](../active-directory/role-based-access-built-in-roles.md#backup-reader) -deze rol machtigingen heeft voor het weergeven van alle back-beheertaken uit te voeren. Stel dat u deze functie worden de bewaking van een persoon.

Als u op zoek bent om te definiëren van uw eigen rollen voor nog meer Zie hoe [aangepaste rollen maken](../active-directory/role-based-access-control-custom-roles.md) in Azure RBAC.



## <a name="mapping-backup-built-in-roles-to-backup-management-actions"></a>Back-up ingebouwde rollen toewijzen aan acties voor back-up
De volgende tabel worden de acties voor back-up en de bijbehorende minimale RBAC-rol vereist voor deze bewerking.

| Bewerking voor het beheer | Minimale RBAC-rol vereist |
| --- | --- |
| Recovery Services-kluis maken | Inzender voor de resourcegroep van de kluis |
| Back-up van Azure Virtual machines inschakelen | Back-upoperator op de kluis, bijdrager van de virtuele machine op virtuele machines |
| Op aanvraag back-up van de virtuele machine | Back-upoperator |
| Virtuele machine herstellen | Back-upoperator, Resource groep Inzender waarin VM en Vnets gaat geïmplementeerd |
| Schijven, afzonderlijke bestanden in de VM-back-up herstellen | Back-upoperator, bijdrager van de virtuele machine op virtuele machines |
| Back-upbeleid voor virtuele machine van Azure back-up maken | Back-up Inzender |
| Back-upbeleid van virtuele machine van Azure back-up wijzigen | Back-up Inzender |
| Back-upbeleid van virtuele machine van Azure back-up verwijderen | Back-up Inzender |
| Back-up stoppen (met behoud van gegevens of gegevens verwijderen) in VM-back-up | Back-up Inzender |
| Lokale Windows Server/client-/ SCDPM of Azure Backup-Server registreren | Back-upoperator |
| Geregistreerde lokale Windows Server/client-/ SCDPM of Azure Backup-Server verwijderen | Back-up Inzender |

## <a name="next-steps"></a>Volgende stappen
* [Op rollen gebaseerd toegangsbeheer](../active-directory/role-based-access-control-configure.md): aan de slag met RBAC in Azure portal.
* Meer informatie over het beheren van toegang met:
  * [PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md)
  * [REST API](../active-directory/role-based-access-control-manage-access-rest.md)
* [Probleemoplossing voor toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-troubleshooting.md): Profiteer van tips voor het oplossen van veelvoorkomende problemen.
