---
title: Foutopsporing in een gepubliceerde een Azure-cloudservice met Visual Studio en IntelliTrace | Microsoft Docs
description: Meer informatie over het fouten opsporen in een cloudservice met Visual Studio en IntelliTrace
services: visual-studio-online
documentationcenter: n/a
author: mikejo
manager: douge
editor: ''
ms.assetid: 5e6662fc-b917-43ea-bf2b-4f2fc3d213dc
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 03/21/2017
ms.author: mikejo
ms.openlocfilehash: 1deec539d3dfe65a2ac02a9f4cd2b94d9a78274a
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444339"
---
# <a name="debugging-a-published-azure-cloud-service-with-visual-studio-and-intellitrace"></a>Foutopsporing in een gepubliceerde Azure-cloudservice met Visual Studio en IntelliTrace
Met IntelliTrace, kunt u uitgebreide informatie over foutopsporing voor een rolinstantie registreren wanneer deze wordt uitgevoerd in Azure. Als u de oorzaak van een probleem wilt, kunt u de IntelliTrace-Logboeken kunt doorlopen uw code vanuit Visual Studio alsof deze worden uitgevoerd in Azure. In feite IntelliTrace-records de belangrijkste uitvoering van code en omgevingsgegevens wanneer uw Azure-toepassing wordt uitgevoerd als een cloudservice in Azure en kunt u de vastgelegde gegevens vanuit Visual Studio opnieuw afspelen. 

U kunt IntelliTrace hebt u Visual Studio Enterprise geïnstalleerd en de doelen van uw Azure-toepassing .NET Framework 4 of hoger gebruiken. IntelliTrace verzamelt gegevens voor uw Azure-rollen. De virtuele machines voor deze rollen worden altijd uitgevoerd 64-bits besturingssystemen.

Als alternatief kunt u [foutopsporing op afstand](http://go.microsoft.com/fwlink/p/?LinkId=623041) om rechtstreeks aan een cloudservice die wordt uitgevoerd in Azure te koppelen.

> [!IMPORTANT]
> IntelliTrace is bedoeld voor alleen debug-scenario's, en mag niet worden gebruikt voor een productie-implementatie.
> 

## <a name="configure-an-azure-application-for-intellitrace"></a>Een Azure-toepassing configureren voor IntelliTrace
Om in te schakelen IntelliTrace voor een Azure-toepassing, moet u maken en publiceren van de toepassing in een Azure van Visual Studio-project. Voordat u deze naar Azure publiceren, moet u de IntelliTrace configureren voor uw Azure-toepassing. Als u uw toepassing zonder IntelliTrace configureren, moet u het project opnieuw publiceren. Zie voor meer informatie, [publiceren van een Azure-cloud services-projecten met Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. Wanneer u klaar bent voor uw Azure-toepassing implementeren, moet u controleren of uw doelen van de build project zijn ingesteld op **Debug**.

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer in het contextmenu **publiceren**.
   
1. In de **Publish Azure Application** dialoogvenster, selecteer het Azure-abonnement en selecteert u **volgende**.

1. In de **instellingen** weergeeft, schakelt de **geavanceerde instellingen** tabblad.

1. Schakel de **IntelliTrace inschakelen** optie voor het verzamelen van IntelliTrace-logboeken voor uw toepassing wanneer het wordt gepubliceerd in de cloud.
   
1. Selecteer voor het aanpassen van de basisconfiguratie van IntelliTrace **instellingen** naast **inschakelen IntelliTrace**.

    ![Koppeling van IntelliTrace-instellingen](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/intellitrace-settings-link.png)
   
1. In de **IntelliTrace instellingen** dialoogvenster kunt u opgeven welke gebeurtenissen u wilt het logboek, of voor het verzamelen van informatie, welke modules en -processen voor het verzamelen van Logboeken voor en hoeveel ruimte toe te wijzen aan de opname. Zie voor meer informatie over IntelliTrace [foutopsporing met IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).
   
    ![IntelliTrace-instellingen](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

De IntelliTrace-logboek is een circulaire logboekbestand van de maximale grootte die is opgegeven in de IntelliTrace-instellingen (de standaardgrootte is 250 MB). IntelliTrace-logboeken worden verzameld in een bestand in het bestandssysteem van de virtuele machine. Wanneer u de logboeken aanvraagt, wordt een momentopname die zijn uitgevoerd op dat punt in tijd en gedownload naar uw lokale computer.

Nadat de Azure-cloud-service is gepubliceerd naar Azure, kunt u bepalen als IntelliTrace is ingeschakeld vanaf de Azure-knooppunt in **Server Explorer**, zoals wordt weergegeven in de volgende afbeelding:

![Server Explorer - IntelliTrace ingeschakeld](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="download-intellitrace-logs-for-a-role-instance"></a>IntelliTrace-logboeken voor een rolinstantie downloaden
U kunt met behulp van Visual Studio, IntelliTrace-logboeken voor een rolinstantie downloaden via de volgende stappen:

1. In **Server Explorer**, vouw de **Cloudservices** knooppunt, en Ga naar de rolinstantie waarvan logboeken die u wilt downloaden. 

1. Met de rechtermuisknop op de rolinstantie en selecteer in het contextmenu s **IntelliTrace-logboeken weergeven**. 

    ![Menuoptie voor IntelliTrace-logboeken weergeven](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/view-intellitrace-logs.png)

1. De IntelliTrace-logboeken worden gedownload naar een bestand in een map op uw lokale computer. Elke keer dat u aanvraagt dat de IntelliTrace-Logboeken, wordt een nieuwe momentopname gemaakt. Terwijl de logboeken worden gedownload, Visual Studio geeft de voortgang van de bewerking in de **Azure Activity Log** venster. Zoals u in de volgende afbeelding, kunt u het regelitem voor de bewerking voor meer details kunt uitbreiden.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

U kunt blijven werken in Visual Studio, terwijl de IntelliTrace-logboeken worden gedownload. Als het logboek voor het downloaden is voltooid, wordt deze geopend in Visual Studio.

> [!NOTE]
> De IntelliTrace-logboeken bevatten mogelijk uitzonderingen die het framework genereert en daarna worden verwerkt. Interne framework-code genereert deze uitzonderingen als een normaal onderdeel van het starten van een rol, zodat u ze veilig negeren.
> 
> 

## <a name="next-steps"></a>Volgende stappen
- [Opties voor het opsporen van fouten in Azure cloudservices](vs-azure-tools-debugging-cloud-services-overview.md)
- [Publiceren van een Azure-cloudservice met Visual Studio](vs-azure-tools-publishing-a-cloud-service.md)