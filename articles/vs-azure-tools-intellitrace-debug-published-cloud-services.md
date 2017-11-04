---
title: Een gepubliceerde foutopsporing een Azure-cloudservice met Visual Studio en IntelliTrace | Microsoft Docs
description: Meer informatie over fouten opsporen in een cloudservice met Visual Studio en IntelliTrace
services: visual-studio-online
documentationcenter: n/a
author: mikejo
manager: ghogen
editor: 
ms.assetid: 5e6662fc-b917-43ea-bf2b-4f2fc3d213dc
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/21/2017
ms.author: mikejo
ms.openlocfilehash: d33a8338b37f7479196449238388c7dbf391bbb6
ms.sourcegitcommit: b83781292640e82b5c172210c7190cf97fabb704
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="debugging-a-published-azure-cloud-service-with-visual-studio-and-intellitrace"></a>Een gepubliceerde Azure-cloudservice met Visual Studio en IntelliTrace-foutopsporing
U kunt uitgebreide foutopsporingsinformatie voor een rolexemplaar van met IntelliTrace registreren wanneer deze wordt uitgevoerd in Azure. Als u nodig hebt om de oorzaak van een probleem te achterhalen, kunt u de IntelliTrace-logboeken doorlopen uw code vanuit Visual Studio als in Azure werd uitgevoerd. In feite key IntelliTrace records-uitvoering van code en omgevingsgegevens wanneer uw Azure-toepassing wordt uitgevoerd als een cloudservice in Azure en kunt u de vastgelegde gegevens vanuit Visual Studio opnieuw afspelen. 

U kunt IntelliTrace hebt u Visual Studio Enterprise geïnstalleerd en de doelen van uw Azure-toepassing .NET Framework 4 of hoger gebruiken. IntelliTrace verzamelt gegevens voor uw Azure-functies. De virtuele machines voor deze rollen wordt altijd uitgevoerd 64-bits besturingssystemen.

Als alternatief kunt u [foutopsporing op afstand](http://go.microsoft.com/fwlink/p/?LinkId=623041) koppelen rechtstreeks aan een cloudservice die wordt uitgevoerd in Azure.

> [!IMPORTANT]
> IntelliTrace is bedoeld voor foutopsporing scenario's alleen en mag niet worden gebruikt voor een productie-implementatie.
> 

## <a name="configure-an-azure-application-for-intellitrace"></a>Een Azure-toepassing voor IntelliTrace configureren
Om IntelliTrace voor een Azure-toepassing, moet u maken en publiceren van de toepassing van een Azure met Visual Studio-project. Voordat u deze naar Azure publiceert, moet u de IntelliTrace configureren voor uw Azure-toepassing. Als u uw toepassing publiceert zonder IntelliTrace configureren, moet u het project opnieuw te publiceren. Zie voor meer informatie [publiceren van een Azure cloud services-projecten met Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. Wanneer u klaar bent voor uw Azure-toepassing implementeren, moet u controleren of de doelen van uw project build zijn ingesteld op **Debug**.

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer in het contextmenu **publiceren**.
   
1. In de **Publish Azure Application** dialoogvenster, selecteer het Azure-abonnement en selecteert u **volgende**.

1. In de **instellingen** pagina de **geavanceerde instellingen** tabblad.

1. Schakel de **IntelliTrace inschakelen** optie voor het verzamelen van IntelliTrace-logboeken voor uw toepassing wanneer het wordt gepubliceerd in de cloud.
   
1. Selecteer voor het aanpassen van de basisconfiguratie van IntelliTrace **instellingen** naast **IntelliTrace inschakelen**.

    ![Koppeling van IntelliTrace-instellingen](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/intellitrace-settings-link.png)
   
1. In de **IntelliTrace instellingen** dialoogvenster kunt u opgeven welke gebeurtenissen worden logboek, of moeten worden verzameld oproepgegevens, welke modules en -processen voor het verzamelen van Logboeken voor en hoeveel ruimte toe te wijzen aan de opname. Zie voor meer informatie over IntelliTrace [foutopsporing met IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).
   
    ![IntelliTrace-instellingen](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

De IntelliTrace-logboek is een kringverwijzing logbestand van de maximale grootte die is opgegeven in de IntelliTrace-instellingen (de standaardgrootte is 250 MB). IntelliTrace-logboeken worden bijgehouden in een bestand in het bestandssysteem van de virtuele machine. Wanneer u de logboeken aanvraagt, wordt er een momentopname gemaakt op dat moment en wordt deze gedownload op uw lokale computer.

Nadat de Azure-cloud-service is gepubliceerd naar Azure, kunt u bepalen als IntelliTrace is ingeschakeld via het Azure-knooppunt in **Server Explorer**, zoals wordt weergegeven in de volgende afbeelding:

![Server Explorer - IntelliTrace ingeschakeld](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="download-intellitrace-logs-for-a-role-instance"></a>IntelliTrace-logboeken voor een rolexemplaar van downloaden
U kunt met behulp van Visual Studio IntelliTrace-logboeken voor een rolexemplaar van downloaden met de volgende stappen:

1. In **Server Explorer**, vouw de **Cloudservices** knooppunt, en zoek rolinstantie waarvan logboeken die u wilt downloaden. 

1. Met de rechtermuisknop op de rolinstantie en selecteer in het contextmenu s **IntelliTrace-logboeken bekijken**. 

    ![Menuoptie voor IntelliTrace-logboeken weergeven](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/view-intellitrace-logs.png)

1. De IntelliTrace-logboeken worden gedownload naar een bestand in een map op uw lokale computer. Elke keer dat u de IntelliTrace aanvragen registreert, wordt een nieuwe momentopname gemaakt. Terwijl de logboeken worden gedownload, Visual Studio geeft de voortgang van de bewerking in de **Azure Activity Log** venster. Zoals u in de volgende afbeelding, kunt u het artikel voor meer details van de bewerking kunt uitbreiden.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

U kunt blijven werken in Visual Studio, terwijl de IntelliTrace-logboeken worden gedownload. Wanneer het logboek is gedownload, wordt het geopend in Visual Studio.

> [!NOTE]
> De IntelliTrace-logboeken bevatten mogelijk uitzonderingen die het framework genereert en daarna worden verwerkt. Interne framework code genereert deze uitzonderingen als een normaal onderdeel van het opstarten van een rol, zodat u ze kunt negeren.
> 
> 

## <a name="next-steps"></a>Volgende stappen
- [Opties voor het opsporen van Azure-cloudservices](vs-azure-tools-debugging-cloud-services-overview.md)
- [Publiceren van een Azure-cloudservice met Visual Studio](vs-azure-tools-publishing-a-cloud-service.md)