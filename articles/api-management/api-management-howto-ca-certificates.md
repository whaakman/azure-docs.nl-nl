---
title: Toevoegen van een aangepaste CA-certificaat - Azure API Management | Microsoft Docs
description: Informatie over het toevoegen van een aangepaste CA-certificaat in Azure API Management.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/20/2018
ms.author: apimpm
ms.openlocfilehash: 9d3399ba6ee724d91117486744ad1431f53edbce
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43053833"
---
# <a name="how-to-add-a-custom-ca-certificate-in-azure-api-management"></a>Een aangepaste CA-certificaat toevoegen in Azure API Management

Azure API Management kunnen CA-certificaten installeren op de computer in de vertrouwde basiscertificeringsinstanties en tussenliggende gebruikerscertificaatarchieven bevinden. Deze functionaliteit moet worden gebruikt als voor uw services is een aangepaste CA-certificaat vereist.

Het artikel wordt beschreven hoe u voor het beheren van CA-certificaten van een exemplaar van de Azure API Management-service in Azure portal.

## <a name="step1"> </a>Een CA-certificaat uploaden

![CA-certificaten toevoegen](media/api-management-howto-ca-certificates/00.png)

De volgende stappen om een nieuwe CA-certificaat te uploaden. Als u nog geen exemplaar van API Management-service hebt gemaakt, Zie de zelfstudie [API Management service-exemplaar maken](get-started-create-service-instance.md).

1. Navigeer naar uw Azure API Management service-exemplaar in de Azure-portal.

2. Selecteer **CA-certificaten** in het menu.

3. Klik op de **+ toevoegen** knop.  

    ![CA-certificaten toevoegen](media/api-management-howto-ca-certificates/01.png)  

4. Zoeken naar het certificaat en Bepaal in het certificaatarchief. Alleen de openbare sleutel is vereist, zodat het wachtwoord niet vereist is.

    ![CA-certificaten toevoegen](media/api-management-howto-ca-certificates/02.png)  

5. Klik op **Opslaan**. Deze bewerking kan enkele minuten duren.

    ![CA-certificaten toevoegen](media/api-management-howto-ca-certificates/03.png)  

> [!NOTE]
> U kunt uploaden een CA-certificaat met de `New-AzureRmApiManagementSystemCertificate` Powershell-opdracht.

## <a name="step1a"> </a>Verwijderen van een clientcertificaat

Als u wilt een certificaat wilt verwijderen, klikt u op in het contextmenu **...**  en selecteer **verwijderen** naast het certificaat.

![CA-certificaten verwijderen](media/api-management-howto-ca-certificates/04.png)  

[Upload a CA certificate]: #step1
[Delete a CA certificate]: #step1a