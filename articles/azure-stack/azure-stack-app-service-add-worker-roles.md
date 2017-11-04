---
title: Scale-out werkrollen in App Services - Stack in Azure | Microsoft Docs
description: Gedetailleerde richtlijnen voor het schalen van Azure Stack App Services
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: slinehan
editor: anwestg
ms.assetid: 3cbe87bd-8ae2-47dc-a367-51e67ed4b3c0
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: anwestg
ms.openlocfilehash: 30ab325488684a26a6ef442e7c8241526a66aa4c
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/14/2017
---
# <a name="app-service-on-azure-stack-add-more-infrastructure-or-worker-roles"></a>App-Service op Azure-Stack: meer infrastructuur of worker rollen toevoegen

Dit document bevat instructies over het schalen van App Service voor Azure-Stack infrastructuur- en werkrollen rollen. Deze bevat stappen voor het maken van aanvullende werkrollen ter ondersteuning van toepassingen van elke grootte.

> [!NOTE]
> Als uw Azure-Stack-omgeving geen meer dan 96 GB RAM-geheugen heeft mogelijk hebt u problemen bij het toevoegen van extra capaciteit.

App-Service op Azure-Stack standaard ondersteunt gratis en gedeelde worker lagen. Als u wilt toevoegen andere worker-laag, moet u meer werkrollen toevoegen.

Als u niet zeker wat is geïmplementeerd met de standaard-App Service op Azure-Stack-installatie weet, kunt u meer informatie over in controleren de [App-Service op Azure-Stack overzicht](azure-stack-app-service-overview.md).

Azure App Service op Azure-Stack alle rollen met behulp van de virtuele-Machineschaalsets implementeert en als zodanig wordt gebruikgemaakt van de mogelijkheden voor vergroten/verkleinen van deze workload. Daarom wordt alle schalen van de werknemer lagen uitgevoerd via de App Service-beheerder.

Toevoegen van extra werknemers rechtstreeks vanuit de App Service-beheerder met Resource-Provider.

1. Meld u aan bij de Azure-Stack-beheerportal als de servicebeheerder.

2. Blader naar **App Services**.

    ![](media/azure-stack-app-service-add-worker-roles/image01.png)
  
3. Klik op **rollen**. Hier ziet u de verdeling van alle rollen in App Service geïmplementeerd.

4. Klik met de rechtermuisknop op de rij van het type dat u wilt schalen en klik vervolgens op **ScaleSet**.

    ![](media/azure-stack-app-service-add-worker-roles/image02.png)
  
5. Klik op **schaal**, selecteer het aantal exemplaren dat u wilt aanpassen aan en klik vervolgens op **opslaan**.
    
    ![](media/azure-stack-app-service-add-worker-roles/image03.png)

6. App-Service op Azure-Stack wordt nu de extra virtuele machines toevoegen, om ze te configureren, de vereiste software installeren en gemarkeerd als gereed wanneer dit proces voltooid is. Dit kan ongeveer 80 minuten duren.

7. U kunt de voortgang van de gereedheid van de nieuwe rollen aan de hand van de werknemers in de **rollen** blade.

Nadat ze volledig geïmplementeerde en klaar bent, worden de werknemers beschikbaar voor gebruikers van hun werkbelasting naar ze te implementeren. Hieronder ziet u een voorbeeld van de beschikbare meerdere Prijscategorieën standaard. Als er geen beschikbare werknemers voor een bepaalde worker-laag, is de optie voor het kiezen van de bijbehorende prijscategorie is niet beschikbaar.

![](media/azure-stack-app-service-add-worker-roles/image04.png)

>[!NOTE]
> Als u wilt uitbreiden Management, toevoegen front-end- of Publisher rollen dat moet u de bijbehorende VM-schaalset uitbreiden. We zullen de mogelijkheid voor het schalen van deze rollen via de App Service-beheer in een toekomstige release toevoegen.

Als u wilt uitbreiden Management, Front-End of Publisher rollen, volgt u dezelfde stappen selecteren van het type van de juiste rol. Domeincontrollers worden niet geïmplementeerd als Schaalsets en dus twee moeten worden geïmplementeerd tijdens de installatie voor alle productie-implementaties.

### <a name="next-steps"></a>Volgende stappen

[Implementatiebronnen configureren](azure-stack-app-service-configure-deployment-sources.md)
