---
title: Threat Modeling Tool Releases - Microsoft Threat Modeling Tool - Azure | Microsoft Docs
description: Waarin u de releaseopmerkingen voor de threat modeling tool
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: jegeib
ms.openlocfilehash: 55c781d0c6284de1382c9d3e614621c40963194b
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55303072"
---
# <a name="threat-modeling-tool-update-release-71601261---1292019"></a>Threat Modeling Tool release van update 7.1.60126.1 - 1/29/2019

Versie 7.1.60126.1 van de Microsoft Threat Modeling Tool is uitgebracht op 29 januari-2019 en bevat de volgende wijzigingen:

- De minimaal vereiste versie van .NET is verhoogd tot [.Net 4.7.1](http://go.microsoft.com/fwlink/?LinkId=863262).
- De minimaal vereiste versie van Windows is verhoogd tot [Windows 10 Verjaardag Update](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) vanwege de afhankelijkheid .NET.
- Een functie model validatie in-/ uitschakelen is toegevoegd aan het menu Opties van het hulpprogramma.
- Meerdere koppelingen in de eigenschappen van bedreigingen zijn bijgewerkt.
- Secundaire UX wijzigingen in het beginscherm van het hulpprogramma.
- De Threat Modeling Tool nu neemt over van de TLS-instellingen van het besturingssysteem van de host en wordt ondersteund in omgevingen waarvoor TLS 1.2 of hoger.

## <a name="feature-changes"></a>Functiewijzigingen

### <a name="model-validation-option"></a>Model validatieoptie

Op basis van feedback van klanten, is een optie is toegevoegd aan het hulpprogramma de validatie uit te schakelen. Voorheen als de sjabloon een gegevensstroom één Unidirectioneel tussen twee objecten gebruikt, hebt u ontvangen een foutbericht weergegeven in de berichten frame waarin staat: ObjectsName vereist ten minste één 'Any'. Validatie uit te schakelen wordt voorkomen dat deze waarschuwingen zien in de weergave.

De optie om uit te schakelen van validatie van in- en uitschakelen vindt u in het bestand -> Instellingen -> menu opties. De standaardwaarde voor deze instelling is uitgeschakeld.

![Model validatieoptie](./media/azure-security-threat-modeling-tool-releases-71601261/tmt_model_validation_option.png)

## <a name="system-requirements"></a>Systeemvereisten

- Ondersteunde besturingssystemen
  - [Microsoft Windows 10 Verjaardag Update](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) of hoger
- .NET-versie vereist
  - [.NET 4.7.1](http://go.microsoft.com/fwlink/?LinkId=863262) of hoger
- Aanvullende vereisten
  - Een internetverbinding is vereist voor het ontvangen van updates voor het hulpprogramma, evenals de sjablonen.

## <a name="known-issues"></a>Bekende problemen

### <a name="unsupported-systems"></a>Niet-ondersteunde systemen

#### <a name="issue"></a>Probleem

Gebruikers van Windows 10-systemen die zijn kan niet worden geïnstalleerd .NET 4.7.1 of hoger, zoals Windows 10 Enterprise LTSB (versie 1507), kunnen geen open het hulpprogramma voor na de upgrade. Deze oudere versies van Windows zijn niet langer ondersteunde platforms voor het Threat Modeling Tool en moeten de meest recente update niet installeren.

#### <a name="workaround"></a>Tijdelijke oplossing

Gebruikers van Windows 10 Enterprise LTSB (versie 1507) die de nieuwste update hebt geïnstalleerd, kunnen terugkeren naar de vorige versie van de Threat Modeling Tool via het dialoogvenster verwijderen in Apps en functies.

## <a name="documentation-and-feedback"></a>Documentatie en feedback

- Documentatie voor de Threat Modeling Tool bevindt zich op [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool), en informatie gegeven [over het gebruik van het hulpprogramma](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started).

## <a name="next-steps"></a>Volgende stappen

Download de nieuwste versie van de [Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool).