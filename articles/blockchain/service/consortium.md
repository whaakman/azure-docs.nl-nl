---
title: Azure Blockchain Service Consortium
description: ''
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: e745a4ee4789ef46a61b5cb0bbf806c41ef631ec
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027913"
---
# <a name="azure-blockchain-service-consortium"></a>Azure Blockchain Service Consortium

Met Azure Blockchain-Service, kunt u particuliere consortium blockchain netwerken maken waarin elke blockchain-netwerk beperkt tot specifieke deelnemers in het netwerk worden kan. Alleen de deelnemers in het persoonlijke consortium blockchain-netwerk kunnen zien en gebruiken met de blockchain. Consortium netwerken in Azure Blockchain-Service kunnen twee soorten lid deelnemer rollen bevatten:

* **Beheerder** -deelnemers die consortium beheeracties kunnen duren en kunnen deelnemen in blockchain-transacties in beschermde modus.

* **Gebruiker** -deelnemers die u kunnen geen consortium management-actie ondernemen, maar kunnen deelnemen in blockchain-transacties.

Consortium netwerken kunnen een combinatie van deelnemer rollen en een willekeurig aantal elk Roltype kunnen hebben. Er moet ten minste één beheerder.

Het volgende diagram toont een consortium-netwerk met meerdere deelnemers:

![Persoonlijke consortium-netwerkdiagram](./media/consortium/network-diagram.png)

Met consortium management in Azure Blockchain-Service, kunt u deelneemt aan het netwerk consortium beheren. Beheer van het consortium is gebaseerd op het consensus-model van het netwerk. In de huidige preview-versie biedt Azure Blockchain-Service een gecentraliseerde consensus-model voor het beheer van consortium. Een bevoegde deelnemer aan een rol beheren kan duren voordat consortium acties, zoals het toevoegen of verwijderen van deelnemers uit een netwerk.

## <a name="roles"></a>Rollen

Deelnemers aan een consortium kunnen personen of organisaties en een gebruikersrol of een beheerdersrol kunnen worden toegewezen. De volgende tabel bevat de belangrijke verschillen tussen de twee rollen:

| Bewerking | Gebruikersrol | Rol van beheerder
|--------|:----:|:------------:|
| Nieuw lid maken | Ja | Ja |
| Nieuwe leden uitnodigen | Nee | Ja |
| Instellen of wijzigen van de deelnemer rol lid | Nee | Ja |
| Weergavenaam van wijziging lid | Alleen voor eigen lid | Alleen voor eigen lid |
| Leden verwijderen | Alleen voor eigen lid | Ja |
| Deelnemen aan blockchain-transacties | Ja | Ja |

### <a name="user-role"></a>Gebruikersrol

Gebruikers zijn geen beheerdermogelijkheden consortium deelnemers. Ze kunnen niet deelnemen aan leden met betrekking tot het consortium beheren. Gebruikers hun weergavenaam lid kunnen wijzigen en zichzelf van een consortium kunnen verwijderen.

### <a name="administrator"></a>Beheerder

Een beheerder kan leden binnen het consortium beheren. Een beheerder kan leden uitnodigen, leden verwijderen of bijwerken van leden rollen binnen het consortium.
Er moet altijd ten minste één beheerder binnen een consortium. De laatste beheerder moet een andere deelnemer opgeven als een beheerdersrol vóór het verlaten van een consortium.

## <a name="managing-members"></a>Beheren van leden

Alleen beheerders kunnen andere deelnemers aan het consortium uitnodigen. Beheerders uitnodigen deelnemers met hun Azure-abonnement-ID.

Wanneer uitgenodigd, kunnen deelnemers aan de blockchain-consortium toevoegen door het implementeren van een nieuw lid in de Azure Blockchain-Service. Als u wilt weergeven en deelnemen aan het uitgenodigde consortium, moet u dezelfde Azure-abonnement-ID in de uitnodiging gebruikt door de netwerkbeheerder.

Beheerders kunnen een deelnemer verwijderen uit het consortium, met inbegrip van andere beheerders. Leden kunnen zich alleen verwijderen uit een consortium.

## <a name="consortium-management-smart-contract"></a>Consortium management slimme contract

Consortium management in Azure Blockchain-Service wordt uitgevoerd via consortium management slimme contracten. De slimme contracten worden automatisch geïmplementeerd naar uw knooppunten wanneer u een nieuw lid van de blockchain implementeert.

Het adres van de hoofdmap consortium management slimme contract kan worden weergegeven in de Azure-portal. De **RootContract adres** is in de sectie overzicht van blockchain-lid.

![RootContract adres](./media/consortium/rootcontract-address.png)

U kunt werken met de consortium management slimme overeenkomst met de management consortium [PowerShell-module](manage-consortium-powershell.md), Azure portal of rechtstreeks met de slimme contract met behulp van de Service van Azure Blockchain Ethereum gegenereerd -account.

## <a name="ethereum-account"></a>Ethereum-account

Wanneer een lid wordt gemaakt, wordt een Ethereum-accountsleutel gemaakt. Azure Blockchain-Service maakt gebruik van de sleutel om transacties met betrekking tot consortium management te maken. De sleutel van het Ethereum wordt automatisch beheerd door Azure Blockchain-Service.

Het lidaccount kan worden weergegeven in de Azure-portal. Het lidaccount is in de sectie overzicht van blockchain-lid.

![LEDENACCOUNT](./media/consortium/member-account.png)

U kunt uw Ethereum-account opnieuw door te klikken op uw lidaccount en een nieuw wachtwoord invoert. Zowel het adres Ethereum-account en het wachtwoord wordt opnieuw worden ingesteld.  

## <a name="next-steps"></a>Volgende stappen

[Over het beheren van leden in Azure Blockchain-Service met behulp van PowerShell](manage-consortium-powershell.md)
