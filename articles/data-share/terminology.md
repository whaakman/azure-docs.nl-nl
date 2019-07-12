---
title: Terminologie voor Azure Data Share Preview-versie
description: Terminologie voor Azure Data Share Preview-versie
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 4e7db84666b9d3786c3fc25e3653d24d0b95f2e4
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789226"
---
# <a name="azure-data-share-preview-concepts"></a>Azure Data Share Preview concepten 

Azure Data Share Preview introduceert nieuwe terminologie die betrekking hebben op het delen van gegevens. In dit artikel wordt uitgelegd dat enkele veelgebruikte termen die mogelijk worden weergegeven die de service worden gebruikt. 

## <a name="data-provider"></a>Gegevensprovider

Een gegevens-Provider is de organisatie die gegevens met hun gebruikers deelt. De gegevensprovider is doorgaans een eigenaar of een beheerder van de gegevens. Gegevensproviders wilt delen van gegevens van verschillende typen. Enkele voorbeelden van gegevens die een gegevensprovider kan wilt delen zijn onbewerkte gegevens, zoals punt van verkoop- of time series-gegevens. Gegevensprovider kunt ook vooraf verwerkt, wordt een samengestelde gegevens al met analyses en inzichten te delen. 

## <a name="data-consumer"></a>Gegevensverbruiker 

Een Consumer gegevens is de organisatie die gegevens van een gegevensprovider ontvangt. De gegevensverbruiker kan willen deelnemen aan de gedeelde gegevens met hun eigen gegevens om inzicht te. In sommige gevallen kan kan de gegevensverbruiker worden ontvangen van gegevens die al is verwerkt. 

## <a name="data-share"></a>Het delen van gegevens

Een gegevensshare is een groep van gegevenssets die worden gedeeld als één entiteit. Gegevenssets zijn uit een aantal Azure-gegevensbronnen die worden ondersteund door Azure het delen van gegevens. Op dit moment ondersteunt Azure het delen van gegevens Azure Blob Storage en Azure Data Lake Store. 

## <a name="share-subscription"></a>Delen van abonnement 

Een abonnement delen wordt gemaakt wanneer een gegevensgebruiker de uitnodiging voor een gegevens-share van een gegevensprovider accepteert. Gegevensproviders active gedeelde abonnementen kunnen bekijken door te navigeren naar **verzonden Shares** account in hun Azure-gegevens delen en het selecteren van **abonnementen delen**.

Een gegevensgebruiker kunt controleren als ze door te navigeren naar een actieve share-abonnement hebben **ontvangen Shares** de status van hun ontvangen shares weer te geven. 

## <a name="snapshot"></a>Momentopname

Een momentopname kan worden gemaakt door een consument gegevens wanneer ze een uitnodiging voor het delen van gegevens accepteren. Wanneer ze een uitnodiging accepteren, kunnen ze een volledige momentopname van de gegevens die worden gedeeld met hen activeren. De momentopname is een kopie van de gegevens op het moment dat de gegevensverbruiker de momentopname gegenereerd. 

Er zijn twee soorten momentopnamen: volledige en incrementele. Een volledige momentopname bevat alle gegevens in het delen van de gegevens. Een momentopname van een incrementele bevat alle gegevens die is bijgewerkt/toegevoegd sinds de laatste momentopname is geactiveerd. 

## <a name="snapshot-settings-in-azure-data-share"></a>Instellingen voor de momentopnamen in Azure het delen van gegevens
 
Gegevensprovider kunt een momentopname-instelling voor het delen van gegevens inschakelen. Deze instelling kunt gebruikers van de gegevens voor het ontvangen van incrementele updates wanneer deze zich voordoen. Deze instelling moet worden ingeschakeld als de gegevensprovider gebruikers van de gegevens dat wilt voor het ontvangen van updates van gegevens die is gedeeld. 

Als een gegevensprovider kunt deze instelling, kan een terugkeerpatroon worden geselecteerd. De terugkeerinterval kan worden per uur of dagelijks. 

Een gegevensverbruiker heeft de optie voor het aanmelden voor deze momentopname planning voor het ontvangen van incrementele updates, waaronder alle gegevens die zijn gewijzigd sinds ze eerst een nieuwe momentopname gegenereerd. 

## <a name="invitation"></a>Uitnodiging

Gegevensprovider kunt uitnodigen meerdere ontvangers voor het delen van hun gegevens. Ze kunnen dit doen door de ontvangers toe te voegen aan het delen van de gegevens. Uitnodigingen kunnen ook worden toegevoegd nadat een gegevensshare is gemaakt. 

Gegevensprovider kan een uitnodiging niet verwijderen nadat het is verzonden. Houd er rekening mee dat als een gegevensprovider een uitnodiging verwijdert nadat deze is goedgekeurd, de gegevensverbruiker kunt nog steeds een actieve share abonnement hebt. Als de gegevensprovider Hiermee verwijdert u een uitnodiging en nog niet zijn geaccepteerd, wordt de gegevensverbruiker wordt pas weer te accepteren. 

## <a name="recipient"></a>Ontvanger

Een ontvanger is iemand die een uitnodiging naar een gegevensshare ontvangt. Normaal gesproken wordt een gegevensprovider geadresseerden toevoegen aan het delen van gegevens die ze maken. Zodra de ontvanger van een uitnodiging voor de uitnodiging accepteert, worden ze een gegevensverbruiker.  

## <a name="next-steps"></a>Volgende stappen

Als u wilt meer informatie over het delen van gegevens, blijven de [delen van uw gegevens](share-your-data.md) zelfstudie.

