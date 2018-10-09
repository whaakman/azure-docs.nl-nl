---
title: Verifiëren van toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory (Preview) | Microsoft Docs
description: Verifiëren van toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory (Preview).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/07/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: ef9fdb348a80c050e8c47217f2f5158200ae2f30
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48868671"
---
# <a name="authenticate-access-to-azure-blobs-and-queues-using-azure-active-directory-preview"></a>Verifiëren van toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory (Preview)

Azure Storage biedt ondersteuning voor verificatie en autorisatie met Azure Active Directory (AD) voor de services Blob en wachtrij. U kunt met Azure AD, op rollen gebaseerd toegangsbeheer (RBAC) gebruiken om toegang te verlenen aan gebruikers, groepen of service-principals van toepassing. 

Verifiëren van gebruikers of toepassingen die gebruikmaken van Azure AD-referenties biedt uitstekende beveiliging en gebruiksgemak ten opzichte van andere middelen van autorisatie. Terwijl u kunt echter ook doorgaan met de gedeelde sleutel autorisatie met uw toepassingen, heeft met behulp van Azure AD de noodzaak voor het opslaan van uw toegangssleutel voor uw code. U kunt ook blijven gebruiken van handtekeningen voor gedeelde toegang (SAS) om te voorzien in specifieke toegang tot resources in uw opslagaccount verlenen, maar Azure AD biedt vergelijkbare mogelijkheden zonder de noodzaak om te beheren van SAS-tokens of zorgen over het intrekken van een SAS waarmee is geknoeid. Microsoft raadt u aan met behulp van Azure AD-verificatie voor uw Azure Storage-toepassingen indien mogelijk.

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
    - [Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)
    - Python
        - [BLOB, Queue en bestanden](https://github.com/Azure/azure-storage-python)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs)

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="get-started-with-azure-ad-for-storage"></a>Aan de slag met Azure AD voor opslag

De eerste stap bij het gebruik van Azure AD-integratie met Azure Storage is RBAC-rollen voor gegevens in de opslag toewijzen aan uw service-principal (een gebruiker, groep of toepassing service-principal) of de beheerde identiteiten voor Azure-resources. RBAC-rollen omvatten algemene sets machtigingen voor containers en wachtrijen. Zie voor meer informatie over RBAC-rollen voor Azure Storage, [beheren-toegangsrechten aan opslag van gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).

Voor het gebruik van Azure AD toegang verlenen aan de storage-resources in uw toepassingen, moet u een OAuth 2.0-toegangstoken aanvragen vanuit uw code. Zie voor informatie over het aanvragen van een toegangstoken en voor het toestaan van aanvragen voor Azure Storage, [verifiëren met Azure AD vanuit een Azure Storage-toepassing (Preview)](storage-auth-aad-app.md). Als u van een beheerde identiteit gebruikmaakt, Zie [verifiëren toegang tot blobs en wachtrijen met Azure beheerde identiteiten voor Azure-Resources (Preview)](storage-auth-aad-msi.md).

Azure CLI en PowerShell bieden nu ondersteuning voor aangemeld met een Azure AD-identiteit. Nadat u zich aanmelden met een Azure AD-identiteit, is uw sessie wordt uitgevoerd onder die identiteit. Zie voor meer informatie, [gebruiken van een Azure AD-identiteit voor toegang tot Azure Storage met CLI of PowerShell (Preview)](storage-auth-aad-script.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure AD-integratie voor Azure-Blobs en wachtrijen, de blog van het Azure Storage-team plaatst, [aankondiging van de Preview-versie van Azure AD-verificatie voor Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
