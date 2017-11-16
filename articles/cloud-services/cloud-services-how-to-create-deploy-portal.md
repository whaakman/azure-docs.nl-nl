---
title: Het maken en implementeren van een service in de cloud | Microsoft Docs
description: Informatie over het maken en implementeren van een cloudservice met de Azure portal.
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 56ea2f14-34a2-4ed9-857c-82be4c9d0579
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 020e2996f40ed4a48affd3776e44e382c40cb3c0
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="how-to-create-and-deploy-a-cloud-service"></a>Het maken en implementeren van een cloudservice
De Azure portal biedt twee manieren maken en implementeren van een cloudservice: *snelle invoer* en *aangepast maken*.

In dit artikel wordt uitgelegd hoe u een nieuwe cloudservice maken en vervolgens gebruiken met de methode Snelle invoer **uploaden** om te uploaden en implementeren van een cloud service-pakket in Azure. Wanneer u deze methode gebruikt, kunt u de Azure-portal beschikbaar handige koppelingen voor het voltooien van alle vereisten die u gaat. Als u klaar bent om uw cloudservice implementeren wanneer u dit hebt gemaakt, kunt u beide op hetzelfde moment met behulp van aangepast maken kunt doen.

> [!NOTE]
> Als u van plan bent voor het publiceren van uw cloudservice vanuit Visual Studio Team Services (VSTS), snel kunt maken en vervolgens publiceren VSTS instellen van de Azure Quickstart of het dashboard. Zie voor meer informatie [continue levering naar Azure met behulp van Visual Studio Team Services][TFSTutorialForCloudService], of Zie help voor de **Quick Start** pagina.
>
>

## <a name="concepts"></a>Concepten
Drie onderdelen zijn vereist voor het implementeren van een toepassing als een cloudservice in Azure:

* **Servicedefinitie**  
  Het cloud-servicedefinitiebestand (.csdef) definieert het servicemodel, inclusief het aantal rollen.
* **Configuratie van de service**  
  Het configuratiebestand (.cscfg) van een cloud-service bevat configuratie-instellingen voor de cloud service en afzonderlijke rollen, met inbegrip van het aantal rolinstanties.
* **Servicepakket**  
  Het servicepakket (.cspkg) bevat de toepassingscode en configuraties en het servicedefinitiebestand.

U kunt meer informatie over deze en het maken van een pakket [hier](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Voorbereiden van uw app
Voordat u een cloudservice implementeren kunt, moet u het pakket in de cloud-service (.cspkg) maken van uw toepassingscode en een cloud service-configuratiebestand (.cscfg). De Azure SDK biedt hulpprogramma's voor het voorbereiden van deze bestanden vereiste implementatie. U kunt installeren de SDK van de [Azure downloadt](https://azure.microsoft.com/downloads/) pagina in de taal waarin u wilt ontwikkelen van uw toepassingscode.

Drie functies van de cloud zijn speciale configuraties vereist voordat u een servicepakket exporteren:

* Als u wilt implementeren van een cloudservice die gebruikmaakt van Secure Sockets Layer (SSL) om gegevens te coderen, [uw toepassing configureren](cloud-services-configure-ssl-certificate-portal.md#modify) voor SSL.
* Als u wilt configureren van extern bureaublad-verbindingen met rolinstanties, [configureren van de rollen](cloud-services-role-enable-remote-desktop-new-portal.md) voor extern bureaublad.
* Als u wilt voor het configureren van uitgebreide bewaking voor uw cloudservice, moet u Azure diagnostische gegevens inschakelen voor de cloudservice. *Minimale bewaking* (de standaardinstelling niveau bewaking) maakt gebruik van prestatiemeteritems die afkomstig zijn van de host-besturingssystemen voor rolexemplaren (virtuele machines). *Uitgebreide bewaking* aanvullende gegevens op basis van prestatiegegevens binnen de rolexemplaren om in te schakelen dichter analyse van problemen die tijdens de verwerking van de toepassing optreden moeten worden verzameld. Zie voor meer informatie over het inschakelen van Azure Diagnostics, [inschakelen van diagnostische gegevens in Azure](cloud-services-dotnet-diagnostics.md).

Maakt een cloudservice met implementaties van webrollen of werkrollen, moet u [maken het servicepakket](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Voordat u begint
* Als u de Azure SDK nog niet hebt geïnstalleerd, klikt u op **Azure SDK installeren** openen de [pagina Azure Downloads](https://azure.microsoft.com/downloads/), en downloadt u de SDK voor de taal waarin u liever uw code te ontwikkelen. (U hebt een kans om dit later doen.)
* Als alle rolinstanties een certificaat vereist, maakt u de certificaten. Cloudservices moeten een pfx-bestand met een persoonlijke sleutel. Bij het maken en implementeren van de cloudservice, kunt u de certificaten uploaden naar Azure.

## <a name="create-and-deploy"></a>Maken en implementeren
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **Nieuw > berekenen**, en schuif vervolgens omlaag naar en klikt u op **Cloudservice**.

    ![Uw cloudservice publiceren](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)
3. In het nieuwe **Cloudservice** blade, voer een waarde voor de **DNS-naam**.
4. Maak een nieuwe **resourcegroep** of een bestaande set selecteren.
5. Selecteer een **locatie**.
6. Klik op **pakket**. Hiermee opent u de **uploaden van een pakket** blade. Vul de vereiste velden in. Als een van de rollen één exemplaar bevatten, zorg er dan **toch implementeren als een of meer rollen met één exemplaar** is geselecteerd.
7. Zorg ervoor dat **implementatie Start** is geselecteerd.
8. Klik op **OK** die wordt gesloten de **uploaden van een pakket** blade.
9. Als u geen certificaten toevoegen, klikt u op **maken**.

    ![Uw cloudservice publiceren](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Een certificaat uploaden
Als uw implementatiepakket [geconfigureerd voor het gebruik van certificaten](cloud-services-configure-ssl-certificate-portal.md#modify), kunt u het certificaat nu uploaden.

1. Selecteer **certificaten**, en klik op de **certificaten toevoegen** blade, selecteert u het .pfx-bestand van de SSL-certificaat en geef vervolgens de **wachtwoord** voor het certificaat
2. Klik op **Attach certificaat**, en klik vervolgens op **OK** op de **certificaten toevoegen** blade.
3. Klik op **maken** op de **Cloudservice** blade. Wanneer de implementatie heeft bereikt de **gereed** status, kunt u doorgaan met de volgende stappen.

    ![Uw cloudservice publiceren](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)

## <a name="verify-your-deployment-completed-successfully"></a>Controleer of de implementatie is voltooid
1. Klik op het cloud service-exemplaar.

    De status moet worden weergegeven dat de service is **met**.
2. Onder **Essentials**, klikt u op de **Site-URL** uw cloudservice openen in een webbrowser.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)

[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Volgende stappen
* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure-portal.md).
* Configureer een [aangepaste domeinnaam](cloud-services-custom-domain-name-portal.md).
* [Beheren van uw cloudservice](cloud-services-how-to-manage-portal.md).
* Configureer [ssl-certificaten](cloud-services-configure-ssl-certificate-portal.md).
