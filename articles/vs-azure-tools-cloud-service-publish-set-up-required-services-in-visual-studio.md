---
title: Voorbereiden om te publiceren of een Cloud Service implementeren vanuit Visual Studio | Microsoft Docs
description: Meer informatie over de procedures voor services voor cloud- en storage-account instellen en configureren van uw Azure-toepassing.
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 92ee2f9e-ec49-4c7a-900d-620abe5e9d8a
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/10/2017
ms.author: kraigb
ms.openlocfilehash: 5b2043ada193e5a08957656e153b58a61e7eba9e
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="prepare-to-publish-or-deploy-a-cloud-service-from-visual-studio"></a>Voorbereiden om te publiceren of een cloudservice implementeren vanuit Visual Studio

Als u wilt een cloudserviceproject publiceren, moet u de volgende services instellen zoals beschreven in dit artikel:

* Een **cloudservice** uw rollen uitvoeren in de Azure-omgeving, en 
* Een **opslagaccount** die toegang biedt tot de services Blob, wachtrijen en tabellen.

## <a name="create-a-cloud-service"></a>Een cloudservice maken

Een cloudservice kan uw rollen in de Azure-omgeving. Kunt u een cloudservice in Visual Studio of via de [Azure-portal](https://portal.azure.com/) zoals beschreven in de volgende secties.

### <a name="create-a-cloud-service-from-visual-studio"></a>Maak een cloudservice vanuit Visual Studio

1. Met een eerder gemaakte Cloud Services-project met de rechtermuisknop op het project selecteren **publiceren**.
1. Indien nodig, meld u aan met de Microsoft of -organisatieaccount die zijn gekoppeld aan uw Azure-abonnement en selecteer vervolgens **volgende** om door te gaan naar de **instellingen** pagina.
1. Een **Cloudservice maken en Storage-Account** dialoogvenster wordt weergegeven (zo niet, selecteer **nieuw** van de **Cloudservice** lijst).
1. Voer een niet-hoofdlettergevoelige naam voor uw cloudservice, die deel uitmaakt van uw URL en moeten uniek zijn. Ook kiezen een regio of Affiniteitsgroep en selecteer een replicatieoptie voor.

### <a name="create-a-cloud-service-through-the-azure-portal"></a>Maak een cloudservice via de Azure portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Selecteer **Cloudservices (klassiek)** aan de linkerkant van de pagina.
1. Selecteer **+ toevoegen**, geef de vereiste gegevens (DNS-naam, abonnement, resourcegroep en locatie). Het is niet nodig zijn voor het uploaden van een pakket op dit moment omdat u dat verderop in Visual Studio doen.
1. Selecteer **maken** om het proces te voltooien.

## <a name="create-a-storage-account"></a>Een opslagaccount maken

Een opslagaccount biedt toegang tot de services Blob, wachtrijen en tabellen. U kunt een opslagaccount met Visual Studio maken of de [Azure-portal](https://portal.azure.com/).

### <a name="create-a-storage-account-from-visual-studio"></a>Een opslagaccount maken vanuit Visual Studio

1. In **Solution Explorer** met een eerder gemaakte Cloud Service-project, zoek de **verbonden Services** knooppunt binnen een rolproject, klik met de rechtermuisknop en selecteer **verbonden Service toevoegen**. (In Visual Studio 2015 met de rechtermuisknop op de **opslag** uit en selecteer **Storage-Account maken**.)
1. In de **verbonden Services** lijst die wordt weergegeven, selecteer **Cloud-opslag met Azure Storage**.
1. Selecteer in het dialoogvenster Azure Storage **+ nieuw Opslagaccount maken**, die wordt een dialoogvenster weergegeven waarin u uw abonnement, met een naam opgeven voor het account, een prijscategorie laag, resourcegroep en locatie.
1. Selecteer **maken** wanneer u klaar bent. Het nieuwe opslagaccount wordt weergegeven in de lijst met beschikbare storage-accounts in uw abonnement.
1. Aangeven dat de account in en selecteer **toevoegen**.

### <a name="create-a-storage-account-through-the-azure-portal"></a>Een opslagaccount via de Azure portal maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Selecteer **+ nieuw** linksboven op.
1. Selecteer **opslag** onder 'Azure Marketplace,' vervolgens **opslagaccount - blob, bestand, tabel, wachtrij** vanaf de rechterkant.
1. Geef de vereiste gegevens (naam, implementatiemodel, enzovoort.
1. Selecteer **maken** om het proces te voltooien.

## <a name="configure-your-app-to-use-the-storage-account"></a>Configureer uw app om de storage-account gebruiken

Nadat u een opslagaccount hebt gemaakt, verbinding mee te maken vanuit Visual Studio automatisch de serviceconfiguraties voor het project, inclusief URL's en toegangstoetsen bijgewerkt.

Als u een cloudservice hebt gemaakt vanuit Visual Studio met de **verbonden Service toevoegen**, kunt u de verbindingen controleren door het openen van `ServiceConfiguration.Cloud.cscfg` en `ServiceConfiguration.Local.cscfg`.

Als u een cloudservice via de Azure-portal hebt gemaakt, dezelfde stappen in [een opslagaccount maken vanuit Visual Studio](#create-a-storage-account-from-visual-studio) maar het bestaande account selecteren in plaats van een nieuwe maken. Visual Studio vervolgens werkt u de configuratie voor u.

Voor het configureren van instellingen handmatig, gebruik de eigenschappenpagina's in Visual Studio voor de betreffende functie in uw cloudserviceproject (met de rechtermuisknop op de rol en selecteer **eigenschappen**). Zie voor meer informatie [configureren van een verbindingsreeks voor een opslagaccount](https://docs.microsoft.com/azure/vs-azure-tools-multiple-services-project-configurations#configuring-a-connection-string-to-a-storage-account).

### <a name="about-access-keys"></a>Over toegangstoetsen

De Azure-portal ziet u de URL's waarmee u kunt toegang krijgen tot bronnen in elk van de Azure storage-services en de primaire en secundaire sneltoetsen voor uw account. U kunt deze sleutels gebruiken om aanvragen ten opzichte van de opslagservices te verifiëren.

De secundaire toegangssleutel biedt dezelfde toegangsrechten als de primaire toegangssleutel voor uw opslagaccount en wordt gegenereerd als een back-up van uw primaire toegangssleutel verdacht. Bovendien is het raadzaam dat u de toegangssleutels regelmatig opnieuw genereert. Een instelling van de tekenreeks gebruiken van de secundaire sleutel terwijl u de primaire sleutel opnieuw genereren en vervolgens kunt u deze voor het gebruik van de primaire sleutel in opnieuw gegenereerd tijdens het genereren van de secundaire sleutel wijzigen, kunt u wijzigen.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over apps publiceren naar Azure vanuit Visual Studio, [publiceren van een Cloudservice met de Azure-hulpprogramma's](vs-azure-tools-publishing-a-cloud-service.md).
