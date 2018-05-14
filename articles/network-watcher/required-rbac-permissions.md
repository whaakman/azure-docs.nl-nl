---
title: Vereiste machtigingen voor het gebruik van de mogelijkheden van Azure-netwerk-Watcher | Microsoft Docs
description: Meer informatie over welke machtigingen voor het beheer van Azure op rollen gebaseerde toegang nodig zijn om te werken met de netwerk-Watcher-mogelijkheden.
services: network-watcher
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: network-watcher
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: jdial
ms.openlocfilehash: 09f3a1e1d9c6796cb55ae8f0ab18bf8e1b3fa198
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="role-based-access-control-permissions-required-to-use-network-watcher-capabilities"></a>Op rollen gebaseerde machtigingen voor toegangsbeheer vereist voor het gebruik van netwerk-Watcher-mogelijkheden

Azure op rollen gebaseerde toegangsbeheer (RBAC) kunt u alleen de specifieke acties toewijzen aan leden van uw organisatie die zij nodig hebben om hun toegewezen taken worden voltooid. Voor het gebruik van netwerk-Watcher-mogelijkheden, moet het account dat u zich aanmelden bij Azure met, worden toegewezen aan de [eigenaar](/role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#owner), [Inzender](/role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#contributor), of [Network contributor](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#network-contributor) ingebouwde rollen of toegewezen aan een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) die de acties die worden vermeld voor elk netwerk-Watcher-mogelijkheden in de volgende secties is toegewezen. Zie voor meer informatie over de mogelijkheden van netwerk-Watcher, [wat is er netwerk-Watcher?](network-watcher-monitoring-overview.md).

## <a name="network-watcher"></a>Network Watcher

| Bewerking                                                              | Naam                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/read                              | Ophalen van een netwerk-watcher                                          |
| Microsoft.Network/networkWatchers/write                             | Maken of bijwerken van een netwerk-watcher                             |
| Microsoft.Network/networkWatchers/delete                            | Verwijderen van een netwerk-watcher                                       |

## <a name="nsg-flow-logs"></a>NSG-flow-Logboeken

| Bewerking                                                              | Naam                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/configureFlowLog/action           | Configureren van een stroom logboek                                           |
| Microsoft.Network/networkWatchers/queryFlowLogStatus/action         | De status opvragen voor een stroom-logboek                                    |

## <a name="connection-troubleshoot"></a>Verbinding oplossen

| Bewerking                                                              | Naam                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/queryTroubleshootResult/action    | De resultaten van de query van een verbinding oplossen test                |
| Microsoft.Network/networkWatchers/troubleshoot/action               | Voer een verbinding test oplossen                             |

## <a name="connection-monitor"></a>Monitor voor verbinding

| Bewerking                                                              | Naam                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectionMonitors/start/action   | Een monitor verbinding starten                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/stop/action    | Een monitor verbinding stoppen                                      |
| Microsoft.Network/networkWatchers/connectionMonitors/query/action   | Query uitvoeren op een monitor verbinding                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/read           | Een monitor verbinding ophalen                                       |
| Microsoft.Network/networkWatchers/connectionMonitors/write          | Een monitor verbinding maken                                    |
| Microsoft.Network/networkWatchers/connectionMonitors/delete         | Een monitor verbinding verwijderen                                    |

## <a name="packet-capture"></a>Pakketopname

| Bewerking                                                              | Naam                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | De status van een pakketopname opvragen                           |
| Microsoft.Network/networkWatchers/packetCaptures/stop/action        | Stop de pakketopname van een                                          |
| Microsoft.Network/networkWatchers/packetCaptures/read               | Ophalen van een pakketopname                                           |
| Microsoft.Network/networkWatchers/packetCaptures/write              | maken van een pakketopname                                        |
| Microsoft.Network/networkWatchers/packetCaptures/delete             | Het vastleggen van een pakket verwijderen                                        |

## <a name="ip-flow-verify"></a>IP-stroom controleren

| Bewerking                                                              | Naam                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/ipFlowVerify/action               | Controleer of een IP-stroom                                              |

## <a name="next-hop"></a>Volgende hop

| Bewerking                                                              | Naam                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/nextHop/action                    | De volgende hop van een virtuele machine ophalen                                     |

## <a name="network-security-group-view"></a>Netwerkbeveiligingsgroep weergeven

| Bewerking                                                              | Naam                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/securityGroupView/action          | Weergave-beveiligingsgroepen                                           |

## <a name="topology"></a>Topologie

| Bewerking                                                              | Naam                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/topology/action                   | Topologie ophalen                                                   |

## <a name="reachability-report"></a>Bereikbaarheid rapport

| Bewerking                                                              | Naam                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/azureReachabilityReport/action    | Een rapport Azure bereikbaarheid ophalen                               |

## <a name="additional-actions"></a>Aanvullende acties

Watcher netwerkmogelijkheden moeten de volgende acties:

- Microsoft.Storage/Read
- Microsoft.Authorization/Read
- Microsoft.Resources/subscriptions/resourceGroups/Read
- Microsoft.Storage/storageAccounts/listServiceSas/Action
- Microsoft.Storage/storageAccounts/listAccountSas/Action
- Microsoft.Storage/storageAccounts/listKeys/Action
- Microsoft.Compute/virtualMachines/Read
- Microsoft.Compute/virtualMachines/Write
- Microsoft.Compute/virtualMachineScaleSets/Read
- Microsoft.Compute/virtualMachineScaleSets/Write
- Microsoft.Insights/alertRules/*
- Microsoft.Support/*