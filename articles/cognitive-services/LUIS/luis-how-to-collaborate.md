---
title: Werk samen met andere inzenders aan apps in Azure LUIS | Microsoft Docs
description: Meer informatie over het samenwerken met andere inzenders aan Language Understanding (LUIS)-toepassingen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/05/2018
ms.author: v-geberr
ms.openlocfilehash: 55c50f5276e2cf97ac98d75b8fc2bd56f1c5d644
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37340410"
---
# <a name="collaborate-with-others-on-language-understanding-luis-apps"></a>Samenwerken met anderen aan Language Understanding (LUIS)-apps  

U kunt samenwerken met anderen in uw LUIS-app samen. 

## <a name="owner-and-collaborators"></a>Eigenaar en samenwerkers
Een app is eigenaar van een enkele maar veel deelnemers kan hebben. 

## <a name="add-collaborator"></a>De samenwerker toevoegen

Om toe te staan samenwerkers om te bewerken van uw LUIS-app op de **instellingen** pagina van uw LUIS-app, voer de e-mailadres van de samenwerker en klik op **toevoegen samenwerker**.

![De samenwerker toevoegen](./media/luis-how-to-collaborate/add-collaborator.png)

* Deelnemers kunnen aanmelden en uw LUIS-app op hetzelfde moment als die u in de app werkt bewerken. <!--If a collaborator edits the LUIS app, you see a notification at the top of the browser.-->
* Samenwerkers kunnen geen andere deelnemers toevoegen.

Zie [Azure Active Directory-tenant gebruiker](luis-how-to-account-settings.md#azure-active-directory-tenant-user) voor meer informatie over Active Directory-gebruikersaccounts. 

## <a name="set-application-as-public"></a>Set-toepassing als openbare
Zie [toegang tot het eindpunt van de openbare app](luis-concept-security.md#public-app-endpoint-access) voor meer informatie.

### <a name="transfer-of-ownership"></a>Overdracht van eigendom
Hoewel LUIS momenteel geen overdracht van eigendom ondersteunt, kunt u uw app exporteren en een andere LUIS gebruiker de app kunt importeren. Er zijn kleine verschillen in LUIS scores tussen de twee toepassingen. 
