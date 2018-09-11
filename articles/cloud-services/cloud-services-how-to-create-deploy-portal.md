---
title: Over het maken en implementeren van een service in de cloud | Microsoft Docs
description: Informatie over het maken en implementeren van een cloudservice met de Azure-portal.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 56ea2f14-34a2-4ed9-857c-82be4c9d0579
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeconnoc
ms.openlocfilehash: ece3511d0d3c12be13a649385ea9b6dd5c75bfe0
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44303706"
---
# <a name="how-to-create-and-deploy-a-cloud-service"></a>Over het maken en implementeren van een service in de cloud
De Azure portal biedt twee manieren om u te maken en implementeren van een service in de cloud: *snelle invoer* en *aangepast maken*.

In dit artikel wordt uitgelegd hoe u gebruik van de methode Snelle invoer aan een nieuwe cloudservice maken en gebruik vervolgens **uploaden** om te uploaden en implementeren van een cloud service-pakket in Azure. Wanneer u deze methode gebruikt, maakt de Azure-portal beschikbaar handige koppelingen voor het voltooien van alle vereisten die u gaat. Als u klaar bent om uw cloudservice implementeren wanneer u deze maakt, kunt u beide op hetzelfde moment met behulp van aangepast maken kunt doen.

> [!NOTE]
> Als u van plan bent voor het publiceren van uw cloudservice van Azure DevOps, snel kunt maken en vervolgens Azure DevOps-publicatie instellen vanuit de Azure-Quickstart of het dashboard. Zie voor meer informatie, [onafgebroken levering naar Azure met behulp van Azure DevOps][TFSTutorialForCloudService], of Zie help voor de **Quick Start** pagina.
>
>

## <a name="concepts"></a>Concepten
Drie onderdelen zijn vereist voor het implementeren van een toepassing als een cloudservice in Azure:

* **De definitie van service**  
  Het cloud-servicedefinitiebestand (.csdef) definieert het servicemodel, met inbegrip van het aantal rollen.
* **Configuratie van de service**  
  Het configuratiebestand (.cscfg) van een cloud-service bevat configuratie-instellingen voor de cloud service en afzonderlijke rollen, met inbegrip van het aantal rolinstanties.
* **Service-pakket**  
  De servicepakket (.cspkg) bevat de toepassingscode en configuraties en het servicedefinitiebestand.

U kunt meer informatie over deze en over het maken van een pakket [hier](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Uw app voorbereiden
Voordat u een cloudservice implementeren kunt, moet u de cloud service-pakket (.cspkg) maken vanuit de code van uw toepassing en een cloud service-configuratiebestand (.cscfg). De Azure SDK biedt hulpprogramma's voor het voorbereiden van deze bestanden vereiste implementatie. U kunt de SDK van installeren de [Azure Downloads](https://azure.microsoft.com/downloads/) pagina in de taal waarin u de voorkeur aan het ontwikkelen van uw toepassingscode.

Drie cloud service-functies zijn speciale configuraties vereist voordat u een servicepakket exporteren:

* Als u wilt implementeren, een cloudservice die gebruikmaakt van Secure Sockets Layer (SSL) voor gegevensversleuteling, [configureren van uw toepassing](cloud-services-configure-ssl-certificate-portal.md#modify) voor SSL.
* Als u wilt configureren van extern bureaublad-verbindingen met de rolinstanties, [configureren van de rollen](cloud-services-role-enable-remote-desktop-new-portal.md) voor extern bureaublad.
* Als u wilt configureren van uitgebreide bewaking voor uw cloudservice, moet u Azure diagnostische gegevens inschakelen voor de cloudservice. *Minimale bewaking* (de standaardinstelling niveau bewaking) maakt gebruik van prestatiemeteritems die zijn verzameld van de host-besturingssystemen voor rolexemplaren (virtuele machines). *Uitgebreide bewaking* aanvullende metrische gegevens op basis van van prestatiegegevens binnen de rolinstanties om in te schakelen nadere analyse van de problemen die tijdens de verwerking van de toepassing optreden worden verzameld. Zie voor meer informatie over het inschakelen van Azure Diagnostics, [inschakelen van diagnostische gegevens in Azure](cloud-services-dotnet-diagnostics.md).

Voor het maken van een cloudservice met implementaties van webrollen of werkrollen, moet u [maken het servicepakket](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Voordat u begint
* Als u de Azure SDK nog niet hebt geïnstalleerd, klikt u op **Azure SDK installeren** openen de [pagina Azure Downloads](https://azure.microsoft.com/downloads/), en download de SDK voor de taal waarin u de voorkeur aan het ontwikkelen van uw code. (U hebt een kans om dit te doen later opnieuw.)
* Als alle instanties van een certificaat vereist, maakt u de certificaten. Cloudservices moet een pfx-bestand met een persoonlijke sleutel. Bij het maken en implementeren van de cloudservice, kunt u de certificaten uploaden naar Azure.

## <a name="create-and-deploy"></a>Maken en implementeren
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **een resource maken > Compute**, en schuif vervolgens omlaag naar en klikt u op **Cloudservice**.

    ![Uw cloudservice publiceren](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)
3. In de nieuwe **Cloudservice** deelvenster, voer een waarde voor de **DNS-naam**.
4. Maak een nieuwe **resourcegroep** of Selecteer een bestaande resourcegroep.
5. Selecteer een **locatie**.
6. Klik op **pakket**. Hiermee opent u de **uploaden van een pakket** deelvenster. Vul de vereiste velden in. Als een van de rollen één exemplaar bevat, controleert u of **toch implementeren als een of meer rollen met één exemplaar** is geselecteerd.
7. Zorg ervoor dat **implementatie starten** is geselecteerd.
8. Klik op **OK** die wordt gesloten de **uploaden van een pakket** deelvenster.
9. Als u geen certificaten om toe te voegen, klikt u op **maken**.

    ![Uw cloudservice publiceren](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Een certificaat uploaden
Als uw implementatiepakket is [geconfigureerd voor het gebruik van certificaten](cloud-services-configure-ssl-certificate-portal.md#modify), kunt u nu het certificaat uploaden.

1. Selecteer **certificaten**, en klik op de **certificaten toevoegen** deelvenster, selecteert u het .pfx-bestand van het SSL-certificaat en geef vervolgens de **wachtwoord** voor het certificaat
2. Klik op **Attach certificaat**, en klik vervolgens op **OK** op de **certificaten toevoegen** deelvenster.
3. Klik op **maken** op de **Cloudservice** deelvenster. Wanneer de implementatie heeft bereikt de **gereed** status, kunt u doorgaan met de volgende stappen.

    ![Uw cloudservice publiceren](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)

## <a name="verify-your-deployment-completed-successfully"></a>Controleer of de implementatie is voltooid
1. Klik op de cloud service-exemplaar.

    De status moet worden weergegeven dat de service is **met**.
2. Onder **Essentials**, klikt u op de **Site-URL** uw cloudservice openen in een webbrowser.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)

[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Volgende stappen
* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure-portal.md).
* Configureer een [aangepaste domeinnaam](cloud-services-custom-domain-name-portal.md).
* [Uw cloudservice beheren](cloud-services-how-to-manage-portal.md).
* Configureer [ssl-certificaten](cloud-services-configure-ssl-certificate-portal.md).
