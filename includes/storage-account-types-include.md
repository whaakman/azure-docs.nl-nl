---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 08/20/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: f60c23e34962396d4ea6e030912d1ca3f3e4571b
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2018
ms.locfileid: "40260897"
---
Er zijn twee typen opslagaccounts:

### <a name="general-purpose-storage-accounts"></a>Opslagaccounts voor algemeen gebruik
Een opslagaccount voor algemeen gebruik biedt onder één account toegang tot Azure Storage-services zoals Tables, Queues, Files, Blobs en virtuele-machineschijven van Azure. Dit type opslagaccount heeft twee prestatielagen:

* Een laag voor standaardopslagprestaties, waarmee u Tables, Queues, Files, Blobs en virtuele-machineschijven van Azure kunt opslaan.
* Een Premium-opslagaccount, dat op dit moment alleen virtuele-machineschijven van Azure ondersteunt. Zie [Premium Storage: krachtige opslag voor Azure Virtual Machine-werkbelasting](../articles/virtual-machines/windows/premium-storage.md) voor een gedetailleerd overzicht van Premium-opslag.

### <a name="blob-storage-accounts"></a>Blob Storage-accounts
Een Blob Storage-account is een gespecialiseerd opslagaccount voor het opslaan van ongestructureerde gegevens als blobs (objecten) in Azure Storage. Blob Storage-accounts zijn vergelijkbaar met de bestaande opslagaccounts voor algemeen gebruik en bieden dezelfde hoogwaardige kenmerken op het gebied van duurzaamheid, beschikbaarheid, schaalbaarheid en prestaties waarover u nu al beschikt, inclusief 100 procent API-consistentie voor blok-blobs en toevoeg-blobs. Voor toepassingen die alleen blok- of toevoeg-blob-opslag nodig hebben, wordt het gebruik van Blob-opslagaccounts aangeraden.

> [!NOTE]
> Blob Storage-accounts ondersteunen alleen blok-blobs en toevoeg-blobs. Pagina-blobs worden niet ondersteund.
> 
> 

Blob-opslagaccounts hebben het kenmerk **Toegangslaag**, dat kan worden opgegeven wanneer het account wordt gemaakt en later naar behoefte kan worden gewijzigd. Er zijn twee soorten toegangslagen die u op basis van uw gegevenstoegangspatroon kunt opgeven:

* De toegangslaag **Hot**, waarmee wordt aangegeven dat de objecten in het opslagaccount vaker worden gebruikt. Hiermee kunt u gegevens tegen lagere toegangskosten gebruiken.
* De toegangslaag **Cool**, waarmee wordt aangegeven dat de objecten in het opslagaccount minder vaak worden gebruikt. Hiermee kunt u gegevens tegen lagere gegevensopslagkosten opslaan.

Als er een wijziging optreedt in het gebruikspatroon van de gegevens, kunt u op elk gewenst moment schakelen tussen deze toegangslagen. Aan het wijzigen van de toegangslaag kunnen extra kosten zijn verbonden. Zie [Blob Storage-accounts: prijzen en facturering](../articles/storage/common/storage-account-options.md#pricing-and-billing) voor meer informatie.

Zie [Azure Blob Storage: Cool Storage-laag en Hot Storage-laag](../articles/storage/blobs/storage-blob-storage-tiers.md) voor meer informatie.

Voordat u een opslagaccount kunt maken, moet u een Azure-abonnement hebben, een abonnement dat u toegang geeft tot diverse Azure-services. Met een [gratis account](https://azure.microsoft.com/pricing/free-trial/) kunt u direct aan de slag met Azure. Als u besluit om een abonnement aan te schaffen, kunt u kiezen uit een aantal [aanschafopties](https://azure.microsoft.com/pricing/purchase-options/). Als u een [MSDN-abonnee](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) bent, krijgt u gratis maandelijkse tegoeden die u kunt gebruiken met Azure-services, waaronder Azure Storage. Zie [Prijzen van Azure Storage](https://azure.microsoft.com/pricing/details/storage/) voor meer informatie over volumeprijzen.

Zie [Een opslagaccount maken](../articles/storage/common/storage-quickstart-create-account.md) voor informatie over het maken van een opslagaccount. Met één abonnement kunt u maximaal 200 opslagaccounts met een unieke naam maken. Zie [Azure Storage Scalability and Performance Targets](../articles/storage/common/storage-scalability-targets.md) (Schaalbaarheids- en prestatiedoeleinden in Azure Storage) voor meer informatie over opslagaccountlimieten.

