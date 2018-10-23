---
title: Valideren van de oorspronkelijke leveranciers (OEM)-pakketten in de Azure Stack-validatie als een Service | Microsoft Docs
description: Informatie over het valideren van de oorspronkelijke leveranciers (OEM)-pakketten met validatie uit als een Service.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: bcfc4cb65c94e34e9f6056ada53726f88489fefb
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49646648"
---
# <a name="validate-oem-packages"></a>OEM-pakketten valideren

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Wanneer er een wijziging in de firmware of stuurprogramma's voor de validatie van een voltooide oplossing is, kunt u een nieuw pakket voor OEM testen. Wanneer de test van het pakket is verstreken, wordt het ondertekend door Microsoft. Uw test moet het bijgewerkte OEM-uitbreidingspakket met de stuurprogramma's en firmware die zijn geslaagd voor Windows Server-logo en pc's tests bevatten.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

> [!IMPORTANT]
> Bekijk voordat u uploaden of het verzenden van pakketten, [een OEM-pakket maken](azure-stack-vaas-create-oem-package.md) voor de indeling van de verwachte pakket en de inhoud.

## <a name="managing-packages-for-validation"></a>Beheren van pakketten voor validatie

Wanneer u de **validatie van het pakket** werkstroom voor het valideren van een pakket, moet u een URL naar een **Azure Storage-blob**. Deze blob is het OEM-pakket dat op de oplossing is geïnstalleerd tijdens de implementatie. Maken van de blob met behulp van de Azure Storage-Account die u hebt gemaakt tijdens de installatie (Zie [instellen dat uw gevalideerd als een serviceresources](azure-stack-vaas-set-up-resources.md)).

### <a name="prerequisite-provision-a-storage-container"></a>Voorwaarde: Inrichten van een storage-container

Maak een container in uw storage-account voor pakket-blobs. Deze container kan worden gebruikt voor al uw validatie van het pakket wordt uitgevoerd.

1. In de [Azure Portal](https://portal.azure.com), gaat u naar het storage-account hebt gemaakt in [instellen dat uw gevalideerd als een serviceresources](azure-stack-vaas-set-up-resources.md).
2. Op de blade links onder **Blob-Service**selecteert op **Containers**.
3. Selecteer **+ Container** in het menu van de balk en geef een naam voor de container, bijvoorbeeld `vaaspackages`.

### <a name="upload-package-to-storage-account"></a>Pakket uploaden naar storage-account

1. Bereid het pakket dat u wilt valideren. Als het pakket meerdere bestanden heeft, comprimeren in een `.zip` bestand.
2. In de [Azure Portal](https://portal.azure.com), selecteert u de pakket-container en uploaden van het pakket door te selecteren op **uploaden** in de menubalk.
3. Selecteer het pakket `.zip` dat u wilt uploaden. Laat de standaardwaarden staan **Blobtype** (dat wil zeggen, **blok-Blob**) en **blokgrootte**.

> [!NOTE]
> Zorg ervoor dat de `.zip` inhoud worden geplaatst in de hoofdmap van de `.zip` bestand. Er mogen zich geen submappen in het pakket.

### <a name="generate-package-blob-url-for-vaas"></a>URL van het pakket blob voor VaaS genereren

Bij het maken van een **validatie van het pakket** werkstroom in de portal VaaS, moet u een URL naar de Azure Storage-blob met uw pakket opgeven.

#### <a name="option-1-generating-an-account-sas-url"></a>Optie 1: Genereren van een account-SAS-URL

1. [!INCLUDE [azure-stack-vaas-sas-step_navigate](includes/azure-stack-vaas-sas-step_navigate.md)]

1. Selecteer **Blob** van **toegestane Services opties**. Schakel alle overige opties uit.

1. Selecteer **Container** en **Object** van **toegestane resourcetypen**. Schakel alle overige opties uit.

1. Selecteer **lezen** en **lijst** van **toegestane machtigingen**. Schakel alle overige opties uit.

1. Stel **begintijd** op de huidige tijd en **eindtijd** naar één uur van de huidige tijd.

1. [!INCLUDE [azure-stack-vaas-sas-step_generate](includes/azure-stack-vaas-sas-step_generate.md)]
    Dit is hoe de indeling moet worden weergegeven: `https://storageaccountname.blob.core.windows.net/?sv=2016-05-31&ss=b&srt=co&sp=rl&se=2017-05-11T21:41:05Z&st=2017-05-11T13:41:05Z&spr=https`

1. Wijzigen van de gegenereerde SAS-URL om op te nemen van de container pakket `{containername}`, en de naam van uw blob pakket `{mypackage.zip}`, als volgt:  `https://storageaccountname.blob.core.windows.net/{containername}/{mypackage.zip}?sv=2016-05-31&ss=b&srt=co&sp=rl&se=2017-05-11T21:41:05Z&st=2017-05-11T13:41:05Z&spr=https`

    Deze waarde gebruiken bij het starten van een nieuwe **validatie van het pakket** werkstroom in de portal VaaS.

#### <a name="option-2-using-public-read-container"></a>Optie 2: Met behulp van openbare lezen container

> [!CAUTION]
> Deze optie, opent u de container voor anonieme toegang voor alleen-lezen.

1. Verleen **openbare leestoegang voor blobs alleen** naar de container van het pakket door de instructies in de sectie [anonieme gebruikersmachtigingen verlenen voor containers en blobs](https://docs.microsoft.com/azure/storage/storage-manage-access-to-resources#grant-anonymous-users-permissions-to-containers-and-blobs).

2. Selecteer op de pakket-blob in de container in de container pakket om het eigenschappendeelvenster te openen.

3. Kopieer de **URL**. Deze waarde gebruiken bij het starten van een nieuwe **validatie van het pakket** werkstroom in de portal VaaS.

## <a name="apply-monthly-update"></a>Maandelijkse update van toepassing

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

## <a name="create-a-package-validation-workflow"></a>Een werkstroom voor validatie van het pakket maken

1. Aanmelden bij de [VaaS portal](https://azurestackvalidation.com).

2. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]

3. Selecteer **Start** op de **validatie van het pakket** tegel.

    ![Pakket validaties werkstroom tegel](media/tile_validation-package.png)

4. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]

5. Voer de URL van de Azure-opslag-blob naar de OEM-pakket dat is geïnstalleerd op de oplossing tijdens de implementatie. Zie voor instructies [pakket blob URL genereren voor VaaS](#generate-package-blob-url-for-vaas).

6. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

7. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Omgeving parameters kunnen niet worden gewijzigd na het maken van een werkstroom.

8. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]

9. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    U wordt omgeleid naar de pagina overzicht van tests.

## <a name="run-package-validation-tests"></a>Pakket validatietests uit te voeren

In de **pakket validatietests samenvatting** pagina, ziet u een lijst van de tests voor het voltooien van de validatie is vereist. Tests in deze werkstroom wordt uitgevoerd voor ongeveer 24 uur.

[!INCLUDE [azure-stack-vaas-workflow-validation-section_schedule](includes/azure-stack-vaas-workflow-validation-section_schedule.md)]

Wanneer alle tests zijn voltooid, stuurt u de naam van uw oplossing VaaS en validatie van het pakket naar [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com) om aan te vragen pakket ondertekenen.

## <a name="next-steps"></a>Volgende stappen

- [Controleren en beheren van tests in de portal VaaS](azure-stack-vaas-monitor-test.md)