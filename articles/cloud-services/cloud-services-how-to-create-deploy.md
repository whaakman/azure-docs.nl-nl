---
title: Het maken en implementeren van een service in de cloud | Microsoft Docs
description: Informatie over het maken en implementeren van een cloudservice met de methode Snelle invoer in Azure.
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 0ea78ccc-5e7d-40f8-bdb6-478c0eb0e265
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 2a2172a78bfd3ac923edbc9de366b035629dd27b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-create-and-deploy-a-cloud-service"></a>Het maken en implementeren van een Cloudservice
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-create-deploy-portal.md)
> * [Klassieke Azure Portal](cloud-services-how-to-create-deploy.md)
> 
> 

De klassieke Azure portal biedt twee manieren maken en implementeren van een cloudservice: **snelle invoer** en **aangepast maken**.

In dit onderwerp wordt uitgelegd hoe u een nieuwe cloudservice maken en vervolgens gebruiken met de methode Snelle invoer **uploaden** om te uploaden en implementeren van een cloud service-pakket in Azure. Wanneer u deze methode gebruikt, wordt de klassieke Azure portal beschikbare handige koppelingen voor het voltooien van alle vereisten die u gaat. Als u klaar bent voor het implementeren van uw cloudservice bij het maken, kunt u doen beide op hetzelfde moment gebruikt **aangepast maken**.

> [!NOTE]
> Als u van plan bent voor het publiceren van uw cloudservice vanuit Visual Studio Team Services (VSTS), gebruikt u **snelle invoer**, en stel VSTS publiceren vanaf **Quick Start** of het dashboard.
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

* Als u wilt implementeren van een cloudservice die gebruikmaakt van Secure Sockets Layer (SSL) om gegevens te coderen, [uw toepassing configureren](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) voor SSL.
* Als u wilt configureren van extern bureaublad-verbindingen met rolinstanties, [configureren van de rollen](cloud-services-role-enable-remote-desktop.md) voor extern bureaublad.
* Als u wilt voor het configureren van uitgebreide bewaking voor uw cloudservice, moet u Azure diagnostische gegevens inschakelen voor de cloudservice. *Minimale bewaking* (de standaardinstelling niveau bewaking) maakt gebruik van prestatiemeteritems die afkomstig zijn van de host-besturingssystemen voor rolexemplaren (virtuele machines). ' Uitgebreide bewaking * aanvullende gegevens op basis van prestatiegegevens binnen de rolexemplaren om in te schakelen dichter analyse van problemen die tijdens de verwerking van de toepassing optreden moeten worden verzameld. Zie voor meer informatie over het inschakelen van Azure Diagnostics, [diagnostische gegevens inschakelen in Azure](cloud-services-dotnet-diagnostics.md).

Maakt een cloudservice met implementaties van webrollen of werkrollen, moet u [maken het servicepakket](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Voordat u begint
* Als u de Azure SDK nog niet hebt geïnstalleerd, klikt u op **Azure SDK installeren** openen de [pagina Azure Downloads](https://azure.microsoft.com/downloads/), en downloadt u de SDK voor de taal waarin u liever uw code te ontwikkelen. (U hebt een kans om dit later doen.)
* Als alle rolinstanties een certificaat vereist, maakt u de certificaten. Cloudservices moeten een pfx-bestand met een persoonlijke sleutel. U kunt [de certificaten te uploaden naar Azure](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) bij het maken en implementeren van de cloudservice.
* Als u van plan bent de cloudservice implementeren in een affiniteitsgroep, maakt u de affiniteitsgroep. U kunt een affiniteitsgroep gebruiken voor het implementeren van uw cloudservice en andere Azure-services naar dezelfde locatie in een regio. De affiniteitsgroep in kunt u de **netwerken** gebied van de klassieke Azure portal op de **Affiniteitsgroepen** pagina.

## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Hoe: Maak een cloudservice met snelle invoer
1. In de [klassieke Azure-portal](http://manage.windowsazure.com/), klikt u op **nieuw**>**Compute**>**Cloudservice** > **Snelle invoer**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)
2. In **URL**, voer de subdomeinnaam van een moet worden gebruikt in de openbare URL voor toegang tot uw cloudservice in productie-implementaties. De URL-indeling voor productie-implementaties: http://*myURL*. cloudapp.net.
3. In **regio of Affiniteitsgroep**, selecteer de geografische regio of affiniteitsgroep voor het implementeren van de cloudservice. Selecteer een affiniteitsgroep als u wilt uw cloudservice implementeren op dezelfde locatie als andere Azure-services binnen een regio.
4. Klik op **Cloudservice maken**.
   
    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)
   
    U kunt de status van het proces in het berichtgedeelte aan de onderkant van het venster bewaken.
   
    De **Cloudservices** gebied wordt geopend, met de nieuwe cloudservice weergegeven. Wanneer de status Created, is cloud services te maken voltooid.
   
    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)

## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Procedure: een certificaat voor een cloudservice uploaden
1. In de [klassieke Azure-portal](http://manage.windowsazure.com/), klikt u op **Cloudservices**, klik op de naam van de cloudservice en klik vervolgens op **certificaten**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)
2. Klik op **een certificaat uploaden** of **uploaden**.
3. In **bestand**, gebruik **Bladeren** om het certificaat (.pfx-bestand) te selecteren.
4. In **wachtwoord**, de persoonlijke sleutel voor het certificaat opgeven.
5. Klik op **OK** (vinkje).
   
    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)
   
    U kunt de voortgang van het uploaden in het berichtgedeelte hieronder bekijken. Wanneer het uploaden is voltooid, wordt het certificaat is toegevoegd aan de tabel. Klik in het berichtgedeelte op OK om het bericht te sluiten.
   
    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Procedure: een cloudservice implementeren
1. In de [klassieke Azure-portal](http://manage.windowsazure.com/), klikt u op **Cloudservices**, klik op de naam van de cloudservice en klik vervolgens op **Dashboard**.
2. Klik op **uploaden van een nieuwe implementatie in productie** of **uploaden**.
3. In **implementatielabel**, voer een naam voor de nieuwe implementatie - bijvoorbeeld MyCloudServicev4.
4. In **pakket**, gebruik **Bladeren** selecteren van het service-pakketbestand (.cspkg) te gebruiken.
5. In **configuratie**, gebruik **Bladeren** selecteren van het bestand configureren (.cscfg) te gebruiken.
6. Als de cloudservice geen rollen met slechts één exemplaar bevat, selecteert u de **toch implementeren als een of meer rollen met één exemplaar** selectievakje in zodat de implementatie om door te gaan.
   
    Azure kan alleen garanderen van 99,95% toegang tot de cloudservice tijdens het onderhoud en service-updates als ten minste twee exemplaren van elke rol heeft. Indien nodig, kunt u aanvullende rolinstanties toevoegen op de **Scale** pagina nadat u de cloudservice hebt geïmplementeerd. Zie voor meer informatie [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).
7. Klik op **OK** (ingeschakeld) om te beginnen met de cloud service-implementatie.
   
    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)
   
    U kunt de status van de implementatie in het berichtgedeelte bewaken. Klik op OK als u wilt verbergen van het bericht.
   
    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Controleer of de implementatie is voltooid
1. Klik op **Dashboard**.
   
    De status moet worden weergegeven dat de service is **met**.
2. Onder **snelle weergave**, klikt u op de site-URL voor uw cloudservice openen in een webbrowser.
   
    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


## <a name="next-steps"></a>Volgende stappen
* [Algemene configuratie van uw cloudservice](cloud-services-how-to-configure.md).
* Configureer een [aangepaste domeinnaam](cloud-services-custom-domain-name.md).
* [Beheren van uw cloudservice](cloud-services-how-to-manage.md).
* Configureer [ssl-certificaten](cloud-services-configure-ssl-certificate.md).

