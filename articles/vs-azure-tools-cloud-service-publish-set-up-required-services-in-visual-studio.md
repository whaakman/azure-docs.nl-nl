---
title: Voorbereiden om te publiceren of een Cloud Service implementeren vanuit Visual Studio | Microsoft Docs
description: Meer informatie over de procedures voor het instellen van de cloud en opslag-Accountservices en configureren uw Azure-toepassing.
services: visual-studio-online
author: ghogen
manager: douge
ms.assetid: 92ee2f9e-ec49-4c7a-900d-620abe5e9d8a
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: ghogen
ms.openlocfilehash: 084ba64389e2f3f8d62b77697df368dac7e09eac
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42060635"
---
# <a name="prepare-to-publish-or-deploy-a-cloud-service-from-visual-studio"></a>Voorbereiden om te publiceren of een cloudservice implementeren vanuit Visual Studio

Als u een cloudserviceproject publiceren, moet u de volgende services instellen zoals beschreven in dit artikel:

* Een **cloudservice** voor het uitvoeren van de rollen in de Azure-omgeving, en 
* Een **opslagaccount** dat toegang biedt tot de services Blob, wachtrijen en tabellen.

## <a name="create-a-cloud-service"></a>Een cloudservice maken

Een cloudservice wordt uitgevoerd de rollen in de Azure-omgeving. Kunt u een cloudservice in Visual Studio of via de [Azure-portal](https://portal.azure.com/) zoals beschreven in de volgende secties.

### <a name="create-a-cloud-service-from-visual-studio"></a>Een cloudservice maken vanuit Visual Studio

1. Met een eerder gemaakte Cloud Service-project met de rechtermuisknop op het project selecteren **publiceren**.
1. Indien nodig, meld u aan met de Microsoft-account of organisatieaccount dat is gekoppeld aan uw Azure-abonnement en selecteer vervolgens **volgende** om door te gaan naar de **instellingen** pagina.
1. Een **Cloudservice maken en Storage-Account** dialoogvenster wordt weergegeven (als dit niet het geval is, selecteert u **nieuw** uit de **Cloudservice** lijst).
1. Voer een niet-hoofdlettergevoelige naam voor uw cloudservice, die deel uitmaakt van de URL van uw en moet uniek zijn. Kies een regio of Affiniteitsgroep ook en selecteer een replicatieoptie voor.

### <a name="create-a-cloud-service-through-the-azure-portal"></a>Een cloudservice via de Azure-portal maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Selecteer **Cloudservices (klassiek)** aan de linkerkant van de pagina.
1. Selecteer **+ toevoegen**, geeft u de vereiste gegevens (DNS-naam, abonnement, resourcegroep en locatie). Het is niet nodig voor het uploaden van een pakket op dit moment omdat u dat later in Visual Studio doet.
1. Selecteer **maken** om het proces te voltooien.

## <a name="create-a-storage-account"></a>Create a storage account

Een storage-account biedt toegang tot de services Blob, wachtrijen en tabellen. U kunt een storage-account via Visual Studio maken of de [Azure-portal](https://portal.azure.com/).

### <a name="create-a-storage-account-from-visual-studio"></a>Een storage-account maken vanuit Visual Studio

1. In **Solution Explorer** met een eerder gemaakte Cloud Service-project, zoek de **Connected Services** knooppunt binnen een rolproject, klik met de rechtermuisknop en selecteer **Add Connected Service**. (In Visual Studio 2015 met de rechtermuisknop op de **opslag** knooppunt en selecteert u **Storage-Account maken**.)
1. In de **Connected Services** lijst die wordt weergegeven, selecteer **opslag in de Cloud met Azure Storage**.
1. Selecteer in het dialoogvenster Azure Storage **+ nieuw Opslagaccount maken**, die zorgt voor een dialoogvenster waarin u uw abonnement, met een naam opgeven voor het account, een pricing tier, resourcegroep en locatie.
1. Selecteer **maken** wanneer u klaar bent. Het nieuwe opslagaccount wordt weergegeven in de lijst van beschikbare opslag-accounts in uw abonnement.
1. Selecteer dat account en selecteert u **toevoegen**.

### <a name="create-a-storage-account-through-the-azure-portal"></a>Een storage-account via Azure portal maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Selecteer **+ nieuw** links bovenin.
1. Selecteer **opslag** onder 'Azure Marketplace,"vervolgens **opslagaccount - blob, bestand, tabel, wachtrij** vanaf de rechterkant.
1. Geef de vereiste gegevens (naam, implementatiemodel, enzovoort.
1. Selecteer **maken** om het proces te voltooien.

## <a name="configure-your-app-to-use-the-storage-account"></a>Uw app voor het gebruik van het storage-account configureren

Nadat u een opslagaccount hebt gemaakt, verbinding mee te maken vanuit Visual Studio automatisch de serviceconfiguraties voor het project, inclusief URL's en toegangssleutels bijgewerkt.

Als u een cloudservice vanuit Visual Studio met gemaakt de **Add Connected Service**, kunt u de verbindingen controleren door het openen van `ServiceConfiguration.Cloud.cscfg` en `ServiceConfiguration.Local.cscfg`.

Als u een cloudservice via de Azure-portal hebt gemaakt, volgt u dezelfde stappen in [een storage-account maken vanuit Visual Studio](#create-a-storage-account-from-visual-studio) maar selecteert u het bestaande account in plaats van een nieuwe te maken. Visual Studio werkt vervolgens de configuratie voor u.

Het configureren van instellingen handmatig, gebruikt u de eigenschappenpagina's in Visual Studio voor de toepasselijke rol in uw cloud service-project (met de rechtermuisknop op de rol en selecteer **eigenschappen**). Zie voor meer informatie, [configureren van een verbindingsreeks naar een opslagaccount](https://docs.microsoft.com/azure/vs-azure-tools-multiple-services-project-configurations#configuring-a-connection-string-to-a-storage-account).

### <a name="about-access-keys"></a>Over toegangssleutels

De Azure-portal ziet u de URL's die u gebruiken kunt voor toegang tot resources in elk van de Azure storage-services, en de primaire en secundaire toegangssleutel voor uw account. U gebruikt deze sleutels voor verificatie van aanvragen voor de storage-services.

De secundaire toegangssleutel biedt dezelfde toegang tot uw storage-account als de primaire toegangssleutel en wordt gegenereerd als een back-up moet de primaire toegangssleutel worden geknoeid. Bovendien is het raadzaam dat u de toegangssleutels regelmatig. Een instelling voor de verbindingsreeks gebruiken van de secundaire sleutel terwijl u de primaire sleutel opnieuw genereren en vervolgens kunt u deze voor het gebruik van de opnieuw gegenereerd primaire sleutel terwijl u de secundaire sleutel opnieuw genereren wijzigen, kunt u wijzigen.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het publiceren van apps naar Azure vanuit Visual Studio, [publiceren van een Cloudservice met de Azure-hulpprogramma's](vs-azure-tools-publishing-a-cloud-service.md).
