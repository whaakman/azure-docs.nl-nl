---
title: Verifiëren van toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory (Preview) | Microsoft Docs
description: Verifiëren van toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory (Preview).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/13/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: dcf7d237c8cfbf52a804e428d84fff0bb328c7c8
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58012101"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory-preview"></a>Verifiëren van toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory (Preview)

Azure Storage biedt ondersteuning voor verificatie en autorisatie met Azure Active Directory (AD) voor de services Blob en wachtrij. U kunt met Azure AD, op rollen gebaseerd toegangsbeheer (RBAC) gebruiken om toegang te verlenen aan gebruikers, groepen of service-principals van toepassing. 

Verifiëren van gebruikers of toepassingen die gebruikmaken van Azure AD-referenties biedt uitstekende beveiliging en gebruiksgemak ten opzichte van andere middelen van autorisatie. Terwijl u kunt echter ook doorgaan met de gedeelde sleutel autorisatie met uw toepassingen, heeft met behulp van Azure AD de noodzaak voor het opslaan van uw toegangssleutel voor uw code. U kunt ook blijven gebruiken van handtekeningen voor gedeelde toegang (SAS) om te voorzien in specifieke toegang tot resources in uw opslagaccount verlenen, maar Azure AD biedt vergelijkbare mogelijkheden zonder de noodzaak om te beheren van SAS-tokens of zorgen over het intrekken van een SAS waarmee is geknoeid. Microsoft raadt u aan met behulp van Azure AD-verificatie voor uw Azure Storage-toepassingen indien mogelijk.

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="about-the-preview"></a>Over de Preview-versie

Houd rekening met de volgende punten over de Preview-versie:

- Azure AD-integratie is beschikbaar voor de Blob- en wachtrijservices services alleen in de Preview-versie.
- Azure AD-integratie is beschikbaar voor GPv1-, GPv2- en Blob storage-accounts in alle openbare regio's. 
- Alleen de opslagaccounts die zijn gemaakt met het Resource Manager-implementatiemodel worden ondersteund. 
- Ondersteuning voor de oproepende functie identiteitsgegevens in Azure Storage Analytics logboekregistratie is binnenkort beschikbaar.
- Azure AD-autorisatie van toegang tot resources in de standard-opslagaccount wordt momenteel ondersteund. Autorisatie van toegang tot pagina-blobs in premium storage-accounts zal binnenkort worden ondersteund.
- Azure Storage ondersteunt zowel ingebouwde als aangepaste RBAC-rollen. U kunt rollen binnen het bereik van het abonnement, de resourcegroep, de storage-account of een afzonderlijke container of wachtrij toewijzen.
- De Azure Storage-clientbibliotheken die momenteel ondersteuning bieden voor Azure AD-integratie zijn onder andere:
    - [.NET](https://www.nuget.org/packages/WindowsAzure.Storage)
    - [Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)
    - Python
        - [BLOB, Queue en bestanden](https://github.com/Azure/azure-storage-python)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs)

## <a name="overview-of-azure-ad-for-storage"></a>Overzicht van Azure AD voor opslag

De eerste stap bij het gebruik van Azure AD-integratie met Azure Storage is RBAC-rollen voor gegevens in de opslag toewijzen aan uw service-principal (een gebruiker, groep of toepassing service-principal) of de beheerde identiteiten voor Azure-resources. RBAC-rollen omvatten algemene sets machtigingen voor containers en wachtrijen. Zie voor meer informatie over het toewijzen van RBAC-rollen voor Azure Storage, [beheren-toegangsrechten aan opslag van gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).

Voor het gebruik van Azure AD toegang verlenen aan de storage-resources in uw toepassingen, moet u een OAuth 2.0-toegangstoken aanvragen vanuit uw code. Zie voor informatie over het aanvragen van een toegangstoken en voor het toestaan van aanvragen voor Azure Storage, [verifiëren met Azure AD vanuit een Azure Storage-toepassing (Preview)](storage-auth-aad-app.md). Als u van een beheerde identiteit gebruikmaakt, Zie [verifiëren toegang tot blobs en wachtrijen met Azure beheerde identiteiten voor Azure-Resources (Preview)](storage-auth-aad-msi.md).

Azure CLI en PowerShell bieden nu ondersteuning voor aangemeld met een Azure AD-identiteit. Nadat u zich aanmelden met een Azure AD-identiteit, is uw sessie wordt uitgevoerd onder die identiteit. Zie voor meer informatie, [gebruiken van een Azure AD-identiteit voor toegang tot Azure Storage met CLI of PowerShell (Preview)](storage-auth-aad-script.md).

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC-rollen voor blobs en wachtrijen

Azure Active Directory (Azure AD) machtigt toegangsrechten tot beveiligde bronnen via [op rollen gebaseerd toegangsbeheer (RBAC)](../../role-based-access-control/overview.md). Azure Storage definieert een aantal ingebouwde RBAC-rollen die algemene sets machtigingen die wordt gebruikt voor toegang tot containers of wachtrijen omvatten. 

Wanneer een RBAC-rol is toegewezen aan een beveiligings-principal voor Azure AD, wordt de status van Azure verleent toegang tot deze resources voor deze beveiligings-principal. Toegang kan worden gericht op het niveau van het abonnement, de resourcegroep, de storage-account of een afzonderlijke container of de wachtrij. Een beveiligings-principal voor Azure AD kan een gebruiker, een groep, een service-principal van toepassing zijn of een [beheerde identiteit voor de Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md). 

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Zie voor informatie over het toewijzen van een ingebouwde rol in Azure portal, [toegang verlenen tot Azure-containers en wachtrijen met RBAC in Azure portal (preview)](storage-auth-aad-rbac.md).

### <a name="access-permissions-granted-by-rbac-roles"></a>Machtigingen voor toegang verleend door RBAC-rollen 

De volgende tabel geeft een overzicht van de rechten verleend door de ingebouwde rollen voor verschillende niveaus van bereik:

|Bereik|Blob Data Owner|Inzender voor BLOB-gegevens|Blob Data Reader|Inzender voor wachtrij Data|Gegevenslezer voor opslagwachtrij|
|---|---|---|---|---|---|
|Abonnementsniveau|Toegang voor lezen/schrijven en POSIX toegang beheren tot alle containers en blobs in het abonnement|Lees-/ schrijftoegang tot alle containers en blobs in het abonnement| Leestoegang tot alle containers en blobs in het abonnement|Lees-/ schrijftoegang tot alle wachtrijen in het abonnement|Leestoegang tot alle wachtrijen in het abonnement|
|Het niveau van resourcegroep|Toegang voor lezen/schrijven en POSIX toegang beheren tot alle containers en blobs in de resourcegroep|Lees-/ schrijftoegang tot alle containers en blobs in de resourcegroep|Leestoegang tot alle containers en blobs in de resourcegroep|Lees-/ schrijftoegang tot alle wachtrijen in de resourcegroep|Leestoegang tot alle wachtrijen in de resourcegroep|
|Niveau van de Storage-account|Toegang voor lezen/schrijven en POSIX toegang beheren tot alle containers en blobs in de storage-account|Lees-/ schrijftoegang tot alle containers en blobs in de storage-account|Leestoegang tot alle containers en blobs in de storage-account|Lees-/ schrijftoegang tot alle wachtrijen in de storage-account|Leestoegang tot alle wachtrijen in de storage-account|
|Het niveau van de container/wachtrij|Toegang voor lezen/schrijven en POSIX toegang beheren tot de opgegeven container en de blobs.|Toegang tot de opgegeven container en de blobs voor lezen/schrijven|Leestoegang tot de opgegeven container en de blobs|Lees-/ schrijftoegang tot de opgegeven wachtrij|Leestoegang tot de opgegeven wachtrij|

Zie voor meer informatie over de vereiste machtigingen voor het aanroepen van Azure Storage-bewerkingen, [machtigingen voor het aanroepen van REST-bewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure AD-integratie voor Azure-Blobs en wachtrijen, de blog van het Azure Storage-team plaatst, [aankondiging van de Preview-versie van Azure AD-verificatie voor Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
