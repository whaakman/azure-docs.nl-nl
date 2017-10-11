---
title: Een Azure storage-account integreren met Azure CDN | Microsoft Docs
description: Informatie over het gebruik van Azure inhoud Delivery Network (CDN) hoge bandbreedte inhoud leveren door blobs uit Azure Storage cache te plaatsen.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: cbc2ff98-916d-4339-8959-622823c5b772
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 511076935d06ed0908341044e37069e74530be49
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a>Een Azure storage-account integreren met Azure CDN
CDN worden ingeschakeld voor het cache-inhoud van uw Azure-opslag. Het biedt ontwikkelaars een globale oplossing voor hoge bandbreedte inhoud leveren door blobs en statische inhoud van de compute-exemplaren op fysieke knooppunten in de Verenigde Staten, Europa, Azië, Australië en Zuid-Amerika cache te plaatsen.

## <a name="step-1-create-a-storage-account"></a>Stap 1: Een opslagaccount maken
Gebruik de volgende procedure voor het maken van een nieuw opslagaccount voor een Azure-abonnement. Een opslagaccount biedt toegang tot Azure storage-services. Het opslagaccount vertegenwoordigt het hoogste niveau van de naamruimte voor het openen van elk van de onderdelen van de Azure storage-service: Blob-services, wachtrijservices en services van de tabel. Raadpleeg voor meer informatie de [Inleiding tot Microsoft Azure Storage](../storage/common/storage-introduction.md).

Voor het maken van een opslagaccount, moet u de servicebeheerder of medebeheerder voor het gekoppelde abonnement.

> [!NOTE]
> Er zijn verschillende methoden die u gebruiken kunt voor het maken van een opslagaccount, met inbegrip van de Azure Portal en Powershell.  Voor deze zelfstudie worden gebruikt de Azure-Portal.  
> 
> 

**Een opslagaccount voor een Azure-abonnement maken**

1. Meld u aan bij [Azure-portal](https://portal.azure.com).
2. Selecteer in de linkerbovenhoek **nieuw**. In de **nieuw** dialoogvenster **gegevens en opslag**, klikt u vervolgens op **opslagaccount**.
    
    De **storage-account maken** blade wordt weergegeven.   

    ![Storage-Account maken][create-new-storage-account]  

3. In de **naam** veld, typt u de subdomeinnaam. Deze vermelding kan 3 tot 24 kleine letters en cijfers bevatten.
   
    Deze waarde wordt de hostnaam van de in de URI die wordt gebruikt om Blob, wachtrij of tabel bronnen voor het abonnement. Om een container-bron in de Blob-service op te lossen, zou u een URI gebruikt in de volgende indeling waar  *&lt;StorageAccountLabel&gt;*  verwijst naar de waarde die u hebt opgegeven in **Voer een URL**:
   
    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*
   
    **Belangrijk:** de URL label formulieren het subdomein van de URI van het opslagaccount en moet uniek zijn alle gehoste services in Azure.
   
    Deze waarde wordt ook gebruikt als de naam van dit opslagaccount in de portal of bij het openen van dit account via een programma.
4. Laat de standaardwaarden voor **implementatiemodel**, **Account kind**, **prestaties**, en **replicatie**. 
5. Selecteer de **abonnement** die met de storage-account worden gebruikt.
6. Selecteer of maak een **resourcegroep**.  Zie [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md#resource-groups) (Overzicht van Azure Resource Manager) voor meer informatie over resourcegroepen.
7. Selecteer een locatie voor uw opslagaccount.
8. Klik op **Create**. Het proces van het maken van het opslagaccount kan enkele minuten duren.

## <a name="step-2-enable-cdn-for-the-storage-account"></a>Stap 2: CDN inschakelen voor het opslagaccount

Met de nieuwste integratie kunt u nu CDN inschakelen voor uw opslagaccount zonder de uitbreiding van uw storage-portal. 

1. Selecteer het opslagaccount, zoeken naar 'CDN' of schuif omlaag in het menu linkernavigatievenster aan en klik vervolgens op 'Azure CDN'.
    
    De **Azure CDN** blade wordt weergegeven.

    ![CDN inschakelen navigatie][cdn-enable-navigation]
    
2. Maken van een nieuw eindpunt door te voeren van de vereiste informatie
    - **CDN-profiel**: U kunt een nieuwe maken of een bestaand profiel gebruiken.
    - **Prijscategorie**: U hoeft alleen een prijscategorie selecteren als u een nieuw CDN-profiel maakt.
    - **De naam van de CDN-eindpunt**: Voer de naam van een eindpunt per uw keuze.

    > [!TIP]
    > Het gemaakte CDN-eindpunt wordt standaard de hostnaam van uw storage-account gebruikt als oorsprong.

    ! [cdn-eindpunt maken van nieuwe] [cdn--eindpunt-maken van nieuwe]

3. Na het maken, wordt het nieuwe eindpunt weergegeven in de bovenstaande endpoint-lijst.

    ![storage-nieuwe CDN-eindpunt][cdn-storage-new-endpoint]

> [!NOTE]
> U kunt ook gaan naar de extensie Azure CDN CDN inschakelen. [Zelfstudie](#Tutorial-cdn-create-profile).
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]  

## <a name="step-3-enable-additional-cdn-features"></a>Stap 3: Extra CDN-functies inschakelen

Blade 'Azure CDN' opslagaccount, klik op het CDN-eindpunt in de lijst om CDN configuratie blade te openen. U kunt aanvullende CDN-functies inschakelen voor uw levering, zoals compressie, queryreeks geo filteren. U kunt ook aangepaste domein toewijzing aan uw CDN-eindpunt toevoegen en inschakelen van aangepast domein HTTPS.
    
![CDN-opslagconfiguratie cdn][cdn-storage-cdn-configuration]

## <a name="step-4-access-cdn-content"></a>Stap 4: Toegang tot CDN-inhoud
Gebruik voor toegang tot de inhoud in cache op de CDN moet de CDN-URL in de portal. Het adres van een blob in de cache is vergelijkbaar met het volgende:

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\>

> [!NOTE]
> Wanneer u CDN toegang tot een opslagaccount hebt ingeschakeld, zijn alle openbaar beschikbare objecten in aanmerking komen voor CDN edge cache. Als u een object dat momenteel in cache in de CDN wijzigt, de nieuwe inhoud wordt niet meer beschikbaar via de CDN totdat de CDN wordt de inhoud vernieuwd wanneer het in cache opgeslagen inhoud time-to-live-periode is verstreken.
> 
> 

## <a name="step-5-remove-content-from-the-cdn"></a>Stap 5: De inhoud van het CDN verwijderen
Als u niet langer in de cache van een object in de Azure Content Delivery Network (CDN) wenst, kunt u een van de volgende stappen uitvoeren:

* Kunt u de container private in plaats van openbaar. Zie [Anonieme leestoegang tot containers en blobs beheren](../storage/blobs/storage-manage-access-to-resources.md) voor meer informatie.
* U kunt uitschakelen of verwijderen van het CDN-eindpunt met behulp van de beheerportal.
* U kunt uw gehoste service niet meer reageren op aanvragen voor het object wijzigen.

Een object dat al in de cache in de CDN blijft in de cache totdat de time-to-live voor het object zijn verstreken of het eindpunt wordt verwijderd. Wanneer de time-to-live-periode is verstreken, wordt de CDN gecontroleerd om te zien of het CDN-eindpunt nog geldig is en het object nog steeds anoniem toegankelijk. Als dit niet het geval is, klikt u vervolgens het object wordt niet langer in de cache opgeslagen.

## <a name="additional-resources"></a>Aanvullende bronnen
* [CDN-inhoud toewijzen aan een aangepast domein](cdn-map-content-to-custom-domain.md)
* [HTTPS inschakelen voor uw aangepaste domein](cdn-custom-ssl.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png
[cdn-enable-navigation]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png
[cdn-storage-new-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png
[cdn-storage-cdn-configuration]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png 
