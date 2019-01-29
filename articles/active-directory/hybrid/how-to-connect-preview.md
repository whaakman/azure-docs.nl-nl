---
title: 'Azure AD Connect: Functies in preview | Microsoft Docs'
description: Dit onderwerp wordt beschreven in meer detail-functies die zich in de Preview-versie in Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: b501bcefc1126c3e68a6f10ff0ef85740663aaa4
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55167867"
---
# <a name="more-details-about-features-in-preview"></a>Meer informatie over functies in preview
In dit onderwerp wordt beschreven hoe u het gebruik van functies die momenteel in preview.

## <a name="group-writeback"></a>Groep terugschrijven
De optie voor write-back van groep in optionele functies kunt u Write-back van **Office 365-groepen** naar een forest met Exchange geïnstalleerd. Dit is een groep die altijd is gemaakt in de cloud. Als u Exchange on-premises hebt, kunt klikt u vervolgens u terug deze groepen met on-premises gebruikers met een on-premises Exchange-postvak kunnen verzenden en ontvangen van e-mailberichten van deze groepen.

Meer informatie over Office 365-groepen en het gebruik ervan kan worden gevonden [hier](https://aka.ms/O365g).

Een Office 365-groep wordt weergegeven als een distributiegroep in de on-premises AD DS. Uw on-premises Exchange-server moet zich op Exchange 2013 cumulatieve update 8 (uitgebracht in maart 2015) of Exchange 2016 voor het herkennen van dit nieuwe groepstype.

**Opmerkingen bij de tijdens de Preview-versie**

* Het kenmerk van het adresboek momenteel niet is ingevuld in de Preview-versie. De groep is niet zichtbaar in de GAL zonder dat dit kenmerk. De eenvoudigste manier voor het vullen van dit kenmerk is het gebruik van de Exchange PowerShell-cmdlet `update-recipient`.
* Alleen forests met de Exchange-schema zijn geldige doelen voor groepen. Als er geen Exchange is gedetecteerd, klikt u vervolgens is Write-back van groep niet mogelijk om in te schakelen.
* Slechts één forest organisatie implementaties van Exchange worden momenteel ondersteund. Als u meer dan één organisatie on-premises Exchange hebt, moet u een on-premises GALSync oplossing voor deze groepen worden weergegeven in de andere forests.
* De functie voor write-back van groep verwerkt beveiligingsgroepen of distributiegroepen niet.

> [!NOTE]
> Een abonnement op Azure AD Premium is vereist voor write-back van groep.
> 
>

## <a name="user-writeback"></a>Write-back van gebruiker
> [!IMPORTANT]
> De gebruiker Write-back van preview-functie is verwijderd in de update van augustus 2015 naar Azure AD Connect. Als u deze hebt ingeschakeld, moet u deze functie uitschakelen.
>
>

## <a name="next-steps"></a>Volgende stappen
Doorgaan met uw [aangepaste installatie van Azure AD Connect](how-to-connect-install-custom.md).

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](whatis-hybrid-identity.md).
