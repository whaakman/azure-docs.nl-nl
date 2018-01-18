---
title: 'Azure AD Connect: Functies in preview | Microsoft Docs'
description: Dit onderwerp wordt beschreven in meer detail-onderdelen in het voorbeeld in Azure AD Connect.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 7173c87dec980130992438954650227c16ad7292
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="more-details-about-features-in-preview"></a>Meer informatie over functies in preview
Dit onderwerp wordt beschreven hoe u functies die momenteel in preview.

## <a name="group-writeback"></a>Groep terugschrijven
De optie voor write-back van groep in optionele functies kunt u Write-back van **Office 365-groepen** naar een forest met Exchange geïnstalleerd. Dit is een groep die altijd is gemaakt in de cloud. Als u Exchange lokale hebt, kunt klikt u vervolgens u schrijven terug deze groepen met on-premises zodat gebruikers met een lokale Exchange-postvak kunnen verzenden en ontvangen van e-mailberichten van deze groepen.

Meer informatie over Office 365-groepen en het gebruik ervan vindt [hier](http://aka.ms/O365g).

Een Office 365-groep wordt weergegeven als een distributiegroep op on-premises AD DS. Uw lokale Exchange-server moet zich op Exchange 2013 cumulatieve update 8 (uitgebracht in maart 2015) of Exchange 2016 dit nieuwe groepstype kan herkennen.

**Opmerkingen bij de tijdens de preview**

* Het kenmerk van het adresboek adres momenteel niet is ingevuld in het voorbeeld. De groep is niet zichtbaar in de algemene Adreslijst zonder dit kenmerk. De eenvoudigste manier om het vullen van dit kenmerk is met de Exchange-PowerShell-cmdlet `update-recipient`.
* Alleen-forests met het Exchange-schema zijn geldige doelen voor groepen. Als er geen Exchange is gedetecteerd, vervolgens is Write-back van groep niet mogelijk om in te schakelen.
* Alleen één forest Exchange organisatie implementaties worden momenteel ondersteund. Als u meer dan één Exchange organisatie lokale hebt, moet u een on-premises GALSync oplossing voor deze groepen worden weergegeven in de andere forests.
* Het onderdeel van de Write-back van groep verwerkt beveiligingsgroepen of distributiegroepen niet.

> [!NOTE]
> Een abonnement op Azure AD Premium is vereist voor write-back van groep.
> 
>

## <a name="user-writeback"></a>Write-back van gebruiker
> [!IMPORTANT]
> De gebruiker terugschrijven preview-functie is verwijderd in de update augustus 2015 naar Azure AD Connect. Als u deze hebt ingeschakeld, moet u deze functie uitschakelen.
>
>

## <a name="next-steps"></a>Volgende stappen
Blijven uw [aangepaste installatie van Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).
