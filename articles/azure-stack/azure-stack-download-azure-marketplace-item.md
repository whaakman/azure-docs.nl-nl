---
title: Downloaden van marketplace-items van Azure | Microsoft Docs
description: Ik kan de marketplace-items van Azure downloaden aan mijn Azure-Stack-implementatie.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2017
ms.author: erikje
ms.openlocfilehash: 4d7c335a3c68cc9bb8cb0c823883716a3dd6620a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Marketplace-items van Azure naar Azure Stack downloaden

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Als u welke inhoud besluit wilt opnemen in uw Azure-Stack marketplace, moet u rekening houden met de inhoud die beschikbaar zijn vanuit Azure marketplace. U kunt downloaden van een geselecteerde lijst met Azure marketplace-items die vooraf geteste zijn worden uitgevoerd op Azure-Stack. Nieuwe items worden regelmatig toegevoegd aan deze lijst, dus zorg ervoor dat selectievakje terug voor nieuwe inhoud.

Als u wilt downloaden marketplace-items, moet u eerst [Azure Stack registreren bij Azure](azure-stack-register.md). 

## <a name="download"></a>Downloaden
1. Aanmelden bij de Azure-Stack-beheerdersportal (https://portal.local.azurestack.external).
2. Sommige marketplace-items kunnen erg groot worden.  Controleer of er voldoende schijfruimte op het systeem door te klikken op **Resourceproviders** > **opslag**.

    ![](media/azure-stack-download-azure-marketplace-item/image01.png)

3. Klik op **meer Services** > **Marketplace Management**.

    ![](media/azure-stack-download-azure-marketplace-item/image02.png)

4. Klik op **toevoegen uit Azure** voor een overzicht van de items beschikbaar voor downloaden. U kunt op elk item in de lijst weergeven van de beschrijving en downloadgrootte klikken.

    ![](media/azure-stack-download-azure-marketplace-item/image03.png)

5. Selecteer het item dat u wilt dat in de lijst en klik vervolgens op **downloaden**. Hiermee start u downloaden van de VM-installatiekopie voor het geselecteerde item. Downloadtijden variëren.

    ![](media/azure-stack-download-azure-marketplace-item/image04.png)

6. Nadat het downloaden is voltooid, kunt u uw nieuwe marketplace-item als een Azure-Stack operator of een gebruiker kunt implementeren. Klik op **+ nieuw**zoeken in de categorieën voor het nieuwe marketplace-item en selecteer vervolgens het item.
7. Klik op **maken** om de ervaring maken voor de zojuist gedownloade item te openen. Volg de stapsgewijze instructies voor het implementeren van uw item.

## <a name="next-steps"></a>Volgende stappen

[Een Marketplace-item maken en publiceren](azure-stack-create-and-publish-marketplace-item.md)
