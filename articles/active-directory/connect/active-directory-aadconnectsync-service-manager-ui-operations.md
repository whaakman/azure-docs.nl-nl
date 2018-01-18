---
title: Azure AD Connect Synchronization Service Manager-bewerkingen | Microsoft Docs
description: Het tabblad Operations Synchronization Service Manager begrijpen voor Azure AD Connect.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 97a26565-618f-4313-8711-5925eeb47cdc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 91210edc3306b834cbd68f0f028845a7f36dd0b5
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="using-the-sync-service-manager-operations-tab"></a>Het tabblad Synchronisatie Service Manager-bewerkingen

![Sync-Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

De operations-tabblad ziet u de resultaten van de meest recente bewerkingen. Op dit tabblad is de sleutel te begrijpen en oplossen van problemen.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>De informatie die zichtbaar zijn in het tabblad bewerkingen te begrijpen
Het bovenste gedeelte ziet u alle wordt uitgevoerd in chronologische volgorde. Standaard de bewerkingen logboekgegevens houdt van de afgelopen zeven dagen, maar deze instelling kan worden gewijzigd met de [scheduler](active-directory-aadconnectsync-feature-scheduler.md). U wilt zoeken naar eventuele uitvoeren die een status geslaagd niet weergegeven. U kunt het sorteren door te klikken op de headers.

De **Status** kolom is de belangrijkste informatie en ziet u het meest ernstige probleem voor een uitvoering. Hier volgt een korte samenvatting van de meest voorkomende statussen in volgorde van prioriteit voor het onderzoeken van (waarbij * enkele mogelijke fout tekenreeksen geven).

| Status | Opmerking |
| --- | --- |
| Stop-* |De sessie kan niet worden voltooid. Bijvoorbeeld als het externe systeem is niet actief en kan geen verbinding worden gemaakt. |
| gestopt fout limiet |Er zijn meer dan 5000 fouten. De run is automatisch gestopt vanwege het grote aantal fouten. |
| voltooide -\*-fouten |De uitvoering is voltooid, maar er zijn fouten (minder dan 5000) die moeten worden onderzocht. |
| voltooide -\*-waarschuwingen |De uitvoering is voltooid, maar sommige gegevens is niet de verwachte status. Als u fouten hebt, klikt u vervolgens dit bericht is meestal slechts een symptoom zijn. Totdat u fouten hebt opgelost, moet u de waarschuwingen niet onderzoeken. |
| voltooid |Geen problemen. |

Wanneer u een rij selecteert, wordt onder updates zodat de details van die worden uitgevoerd. Aan de linkerkant van de onderkant, moet u wellicht een lijst met de melding **stap #**. Deze lijst wordt alleen weergegeven als er meerdere domeinen in uw forest waar elk domein wordt vertegenwoordigd door een stap. Naam van het domein kan worden gevonden onder de kop **partitie**. Onder **Synchronisatiestatistieken**, vindt u meer informatie over het aantal wijzigingen dat is verwerkt. U kunt klikken op de koppelingen om een lijst van de gewijzigde objecten. Als u objecten met fouten hebt, deze fouten worden weergegeven onder **synchronisatiefouten**.

Zie voor meer informatie [oplossen van een object dat niet kan worden gesynchroniseerd](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).
