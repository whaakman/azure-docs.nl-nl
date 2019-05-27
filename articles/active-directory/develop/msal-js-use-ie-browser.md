---
title: Gebruik Internet Explorer (Microsoft Authentication Library voor JavaScript) | Azure
description: Meer informatie over het gebruik van de Microsoft Authentication Library voor JavaScript (MSAL.js) met Internet Explorer-browser.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8cf8c84120f4c90d3943cfc31ffbf9aafcec0ba3
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873913"
---
# <a name="known-issues-on-internet-explorer-and-microsoft-edge-browsers-with-msaljs"></a>Bekende problemen in Internet Explorer en Microsoft Edge browsers met MSAL.js

Microsoft Authentication Library voor JavaScript (MSAL.js) is gegenereerd voor [JavaScript ES5](https://fr.wikipedia.org/wiki/ECMAScript#ECMAScript_Edition_5_.28ES5.29) zodat het kan worden uitgevoerd in Internet Explorer. Er zijn echter enkele dingen weten.

## <a name="run-an-app-in-internet-explorer"></a>Een app uitvoeren in Internet Explorer
Als u van plan bent MSAL.js te gebruiken in toepassingen die kunnen worden uitgevoerd in Internet Explorer, moet u een verwijzing naar een promise kód Pro toevoegen voordat u het script MSAL.js ernaar verwijst.

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js" class="pre"></script>
```

Dit komt doordat JavaScript beloften biedt geen systeemeigen ondersteuning voor Internet Explorer.

## <a name="debugging-an-application-running-in-internet-explorer"></a>Foutopsporing in een toepassing die wordt uitgevoerd in Internet Explorer

### <a name="running-in-production"></a>Wordt uitgevoerd in de productieomgeving
Normaal gesproken implementeren van uw toepassing naar productie (bijvoorbeeld in een Azure-Web-apps) werkt prima, voor zover dat de eindgebruiker pop-ups heeft geaccepteerd. We hebben het met Internet Explorer 11 getest.

### <a name="running-locally"></a>Lokaal worden uitgevoerd
Als u wilt uitvoeren en fouten opsporen in lokaal uw toepassing in Internet Explorer wordt uitgevoerd, moet u rekening houden met de volgende overwegingen (wordt ervan uitgegaan dat u wilt uitvoeren van uw toepassing als *http://localhost:1234*):

- Internet Explorer is een beveiligingsmechanisme met de naam 'beveiligde modus', waarmee wordt voorkomen dat MSAL.js correct werkt. Tussen de symptomen nadat u zich hebt aangemeld, de pagina kan worden omgeleid naar http://localhost:1234/null.

- Als u fouten opsporen in uw toepassing lokaal wilt uitvoeren, moet u deze 'beveiligde modus' uitschakelen. Voor deze:

    1. Klik op Internet Explorer **extra** (het tandwielpictogram).
    1. Selecteer **Internetopties** en vervolgens de **Security** tabblad.
    1. Klik op de **Internet** zone en schakel het selectievakje **beveiligde modus inschakelen (vereist opnieuw opstarten van Internet Explorer)**. Internet Explorer wordt gewaarschuwd dat de computer niet meer is beveiligd. Klik op **OK**.
    1. Start Internet Explorer opnieuw.
    1. Uitvoeren en fouten opsporen in uw toepassing.

Wanneer u klaar bent, herstelt u de instellingen voor Internet Explorer.  Selecteer **instellingen** -> **Internetopties** -> **Security** -> **alle zones op het standaardniveauinstellen**.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [bekende problemen bij het gebruik van MSAL.js in Internet Explorer](msal-js-use-ie-browser.md).