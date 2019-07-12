---
title: Preview van Azure Data bestandsshare controleren
description: Preview van Azure Data bestandsshare controleren
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 869c1ed41d7f78df184461bc1d8cab6c6eb8d426
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789096"
---
# <a name="monitor-azure-data-share-preview"></a>Voorbeeld van monitor Azure gegevens delen 

In dit artikel wordt uitgelegd hoe u uw gegevens bestandsshares met behulp van Azure Data Share Preview kunt bewaken. Als een gegevensprovider bent u kunt voor het bewaken van verschillende aspecten van uw gegevens delen van relaties. Meer informatie, zoals of de gebruikers van uw gegevens hebt heeft uw uitnodiging geaccepteerd voor het delen van gegevens, ook als ze hebben of een share-abonnement hebt gemaakt en aan de slag om uw gegevens te gebruiken zijn allemaal beschikbaar om te controleren. 

U kunt de momentopnamen die zijn geactiveerd als een consument gegevens controleren in uw Azure-abonnement. 

## <a name="monitor-invitation-status"></a>Monitor status van de uitnodiging

De status van uw uitnodigingen voor het delen van gegevens door te navigeren naar verzonden shares weergeven -> uitnodigingen. 

![Status van de uitnodiging](./media/invitation-status.png "status van de uitnodiging") 

Er zijn drie statussen die uw uitnodiging kan zich in:

* In behandeling - heeft gegevens delen ontvanger nog niet geaccepteerd de uitnodiging.
* Geaccepteerd - heeft gegevens delen ontvanger de uitnodiging geaccepteerd.
* Afgekeurd - heeft gegevens delen ontvanger de uitnodiging geweigerd.

> [!IMPORTANT]
> Als u een uitnodiging verwijdert nadat deze is al geaccepteerd, is het niet gelijk aan het intrekken van toegang. Als u wilt stoppen toekomstige momentopnamen wordt gekopieerd naar uw gegevens consumenten storage-account, moet u de toegang tot en met intrekken de *abonnementen delen* tabblad. 

## <a name="monitor-share-subscriptions"></a>Monitor bestandsshare abonnementen

De status van uw abonnementen delen door te navigeren naar verzonden Shares weergeven -> abonnementen delen. Hiermee geeft u meer informatie over actieve abonnementen die zijn gemaakt door de gebruikers van uw gegevens nadat u hebt uw uitnodiging geaccepteerd. U kunt toekomstige updates aan uw gegevensverbruiker stoppen door de share-abonnement te selecteren en te selecteren *intrekken*. 

## <a name="snapshot-history"></a>Geschiedenis van de momentopname 

Op het tabblad Geschiedenis bent u de momentopnamen die zijn gekopieerd voor de tenant van de consumenten van uw gegevens te bekijken. U bent kunnen controleren of de frequentie en duur van elke momentopname-interval. 

![Geschiedenis van de momentopname](./media/sent-shares.png "Snapshot-geschiedenis") 

Hier vindt u meer informatie over elke uitvoeren door te klikken op de begindatum van de uitvoering van de momentopname. 

## <a name="next-steps"></a>Volgende stappen 

Meer informatie over [delen van gegevens van Azure-terminologie](terminology.md)