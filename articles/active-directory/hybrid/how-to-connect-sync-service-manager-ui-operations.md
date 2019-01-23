---
title: Azure AD Connect Synchronization Service Manager Operations | Microsoft Docs
description: Meer informatie over de Operations-tabblad in de Synchronization Service Manager voor Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 97a26565-618f-4313-8711-5925eeb47cdc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.component: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 57d5dd17a180c946043c307e31e1c89e91f1219e
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54467686"
---
# <a name="using-the-sync-service-manager-operations-tab"></a>Met behulp van het tabblad Synchronisatie Service Manager-bewerkingen

![Synchronisatie van Service Manager](./media/how-to-connect-sync-service-manager-ui-operations/operations.png)

De operations-tabblad toont de resultaten van de meest recente bewerkingen. Op dit tabblad is essentieel om te begrijpen en oplossen van problemen.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>Meer informatie over de gegevens zichtbaar zijn in de operations-tabblad
Het bovenste gedeelte toont alle uitvoeringen in chronologische volgorde. Standaard Registreer de bewerkingen houdt informatie over de afgelopen zeven dagen, maar deze instelling kan worden gewijzigd met de [scheduler](how-to-connect-sync-feature-scheduler.md). U wilt zoeken naar alle uitvoeren die niet de status voltooid wordt weergegeven. U kunt het sorteren door te klikken op de headers.

De **Status** kolom zijn de belangrijkste gegevens en het meest ernstige probleem voor het uitvoeren van wordt weergegeven. Hier volgt een snel overzicht van de meest voorkomende statussen in volgorde van prioriteit voor het onderzoeken van (waarbij * verschillende mogelijke fout tekenreeksen geven).

| Status | Opmerking |
| --- | --- |
| gestopt-\* |De uitvoering kan niet worden voltooid. Bijvoorbeeld, als het externe systeem niet actief is en is niet bereikbaar. |
| gestopt-fout-limiet |Er zijn meer dan 5000 fouten. De uitvoering is automatisch gestopt vanwege het grote aantal fouten. |
| voltooide -\*-fouten |De uitvoering is voltooid, maar er zijn fouten (minder dan 5000) die moeten worden onderzocht. |
| voltooide -\*-waarschuwingen |De uitvoering is voltooid, maar sommige gegevens is niet de verwachte status. Als u fouten hebt, klikt u vervolgens dit bericht is meestal alleen een symptoom zijn. Totdat u fouten hebt opgelost, moet u geen waarschuwingen onderzoeken. |
| voltooid |Geen problemen. |

Wanneer u een rij selecteert, wordt onder bijgewerkt om weer te geven van de details van die worden uitgevoerd. Aan de linkerkant van de onderste, mogelijk hebt u een lijst met de melding **stap #**. Deze lijst wordt alleen weergegeven als u meerdere domeinen in uw forest waar elk domein wordt vertegenwoordigd door een stap. Naam van het domein kunt u vinden onder de kop **partitie**. Onder **Synchronisatiestatistieken**, vindt u meer informatie over het aantal wijzigingen dat is verwerkt. U kunt klikken op de koppelingen een lijst van de gewijzigde objecten te krijgen. Als u objecten met fouten hebt, die fouten weergegeven op **synchronisatiefouten**.

Zie voor meer informatie, [oplossen van een object dat niet kan worden gesynchroniseerd](tshoot-connect-object-not-syncing.md)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](whatis-hybrid-identity.md).
