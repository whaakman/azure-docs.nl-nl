---
title: Een Azure Media Services-account maken via Azure Portal | Microsoft Docs
description: In deze zelfstudie wordt stapsgewijs uitgelegd hoe u een Azure Media Services-account maakt via Azure Portal.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: c551e158-aad6-47b4-931e-b46260b3ee4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: ddc1c7f2dd207cba18a8c080c8b14cc53c149a39
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61463313"
---
# <a name="create-a-media-services-account-using-the-azure-portal"></a>Een Media Services-account maken via de Azure-portal

> [!NOTE]
> Er worden geen nieuwe functies of functionaliteit meer aan Media Services v2. toegevoegd. <br/>Maak kennis met de nieuwste versie, [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/). Zie ook [hulp bij de migratie van v2 naar v3](../latest/migrate-from-v2-to-v3.md)

Azure Portal biedt een manier om snel een Azure Media Services-account (AMS) te maken. U kunt uw account gebruiken voor toegang tot Media Services waarmee u media-inhoud in Azure kunt opslaan, versleutelen, coderen, beheren en streamen. Op het moment dat u een Media Services-account maakt, maakt u ook een bijbehorend opslagaccount (of gebruikt u een bestaand account). Als u een Media Services-account verwijdert, worden de blobs in uw gerelateerde opslagaccount niet verwijderd.

Als uw primaire opslagaccount kunt u Algemeen gebruik v1 of Algemeen gebruik v2 hebben. Op dit moment kunt u in Azure Portal alleen v1 kiezen, maar u kunt v2 toevoegen als u uw account maakt met behulp van de API of Powershell. Zie [Azure-opslagaccounts](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) voor meer informatie over opslagtypen.

Het Media Services-account en alle gekoppelde opslagaccounts moeten zich in hetzelfde Azure-abonnement bevinden. Het wordt sterk aangeraden opslagaccounts te gebruiken op dezelfde locatie als het Media Services-account om aanvullende kosten voor latentie en uitgaande data te vermijden.

In dit artikel wordt uitgelegd hoe u een Media Services-account maakt met de Azure Portal.

> [!NOTE]
> Zie [Scenarios and availability of Media Services features across datacenters](scenarios-and-availability.md#availability) (Scenario's en beschikbaarheid van Media Services-functies via datacenters) voor meer informatie over de beschikbaarheid van Azure Media Services-functies in verschillende regio's.

## <a name="prerequisites"></a>Vereisten

U hebt een Azure-account nodig om deze zelfstudie te voltooien. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie. 

## <a name="create-an-ams-account"></a>Een AMS-account maken

In de stappen in deze sectie wordt uitgelegd hoe u een AMS-account maakt.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **+Nieuw** > **Web en mobiel** > **Media Services**.
   
    ![Media Services-account maken](./media/media-services-create-account/media-services-new1.png)
3. Voer bij **MEDIA SERVICES-ACCOUNT MAKEN** de vereiste waarden in.
   
    ![Media Services-account maken](./media/media-services-create-account/media-services-new3.png)
   
   1. Voer in **Accountnaam** de naam van het nieuwe AMS-account in. Voor de naam van een Media Services-account mogen alleen cijfers en kleine letters worden gebruikt. Spaties zijn niet toegestaan. De naam mag 3 tot 24 tekens lang zijn.
   2. Selecteer in Abonnement een van de verschillende Azure-abonnementen waartoe u toegang hebt.
   3. Selecteer in **Resourcegroep** de nieuwe of bestaande resource.  Een resourcegroep is een verzameling resources met dezelfde levenscyclus, dezelfde machtigingen en hetzelfde beleid. Klik [hier](../../azure-resource-manager/resource-group-overview.md#resource-groups) voor meer informatie.
   4. Selecteer in **Locatie** de geografische regio die wordt gebruikt om de media en metagegevensrecords voor uw Media Services-account op te slaan. Deze regio wordt gebruikt om uw media te verwerken en te streamen. Alleen de beschikbare Media Services-regio's worden in de vervolgkeuzelijst weergegeven. 
   5. Selecteer bij **Opslagaccount** een opslagaccount om Blob Storage van de media-inhoud vanaf uw Media Services-account te leveren. U kunt een bestaand opslagaccount selecteren in dezelfde geografische regio als uw Media Services-account of u kunt een opslagaccount maken. Een nieuw opslagaccount wordt in dezelfde regio gemaakt. De regels voor opslagaccountnamen zijn hetzelfde als voor Media Services-accounts.
      
       Klik [hier](../../storage/common/storage-introduction.md) voor meer informatie over opslag.
   6. Selecteer **Vastmaken aan dashboard** om de voortgang van de implementatie van het account te bekijken.
4. Klik op **Maken** onder in het formulier.
   
    Wanneer het account is gemaakt, worden de overzichtspagina's geladen. In de tabel met het streaming-eindpunt heeft het account een standaardstreaming-eindpunt met de status **Gestopt**. 

    >[!NOTE]
    >Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 
   
## <a name="to-manage-your-ams-account"></a>Uw AMS-account beheren

Selecteer **Instellingen** aan de linkerkant van de portal als u uw AMS-account wilt beheren, bijvoorbeeld om via programmacode verbinding te maken met de AMS-API, video's te uploaden, assets te coderen, beveiliging van inhoud te configureren of de taakvoortgang te bekijken. Ga vanuit **Instellingen** naar een van de beschikbare blades (zoals: **API-toegang**, **Assets**, **Taken**, **Inhoudsbeveiliging**).


## <a name="next-steps"></a>Volgende stappen

U kunt nu bestanden uploaden naar uw AMS-account. Zie [Bestanden uploaden](media-services-portal-upload-files.md) voor meer informatie.

Als u van plan bent om via programmacode toegang te krijgen tot de AMS-API, raadpleegt u [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md) (Toegang tot de API van Azure Media Services met Azure AD-verificatie).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

