---
title: Verifiëren van toegang tot Azure Storage met Azure Active Directory (Preview) | Microsoft Docs
description: Verifiëren van toegang tot Azure Storage met Azure Active Directory (Preview).
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 06/01/2018
ms.author: tamram
ms.openlocfilehash: 9a0782b96b45d27c9b7e603959ecadf5b2632064
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34737643"
---
# <a name="authenticate-access-to-azure-storage-using-azure-active-directory-preview"></a>Verifiëren van toegang tot Azure Storage met Azure Active Directory (Preview)

Azure Storage biedt ondersteuning voor verificatie en autorisatie met Azure Active Directory (AD) voor de services Blob en Queue. U kunt met Azure AD op rollen gebaseerde toegangsbeheer (RBAC) gebruiken om toegang te verlenen aan gebruikers, groepen of service-principals van toepassing. 

Autoriseren van toepassingen die toegang hebben tot Azure Storage met Azure AD biedt betere beveiliging en eenvoudig te gebruiken ten opzichte van andere opties voor autorisatie. Terwijl u kunt doorgaan naar de gedeelde sleutel autorisatie met uw toepassingen gebruiken, gevolg met behulp van Azure AD de noodzaak voor het opslaan van uw toegangssleutel rekening met uw code. Op deze manier kunt u blijven gebruiken van handtekeningen voor gedeelde toegang (SAS) fijnmazig toegang tot bronnen in uw opslagaccount te verlenen, maar Azure AD biedt vergelijkbare mogelijkheden zonder de noodzaak voor het beheren van SAS-tokens of zorgen over het intrekken van een SAS waarmee is geknoeid.

## <a name="about-the-preview"></a>Over de evaluatieversie

Houd de volgende punten over de Preview-versie:

- Azure AD-integratie is beschikbaar voor de services Blob en wachtrij alleen in de Preview-versie.
- Azure AD-integratie is beschikbaar voor GPv1 GPv2 en Blob storage-accounts in alle openbare regio's. 
- Alleen de opslagaccounts die zijn gemaakt met het Resource Manager-implementatiemodel worden ondersteund. 
- Ondersteuning voor de aanroeper identiteitsgegevens in Azure Storage Analytics-logboekregistratie is binnenkort beschikbaar.
- Azure AD-autorisatie van toegang tot bronnen in de standard-opslagaccounts wordt momenteel ondersteund. Autorisatie van toegang tot de pagina-blobs in premium storage-accounts zal binnenkort worden ondersteund.
- Azure Storage ondersteunt ingebouwde en aangepaste RBAC-rollen. U kunt binnen het bereik van het abonnement, de resourcegroep, het storage-account of een afzonderlijke container of de wachtrij rollen toewijzen.
- De clientbibliotheken van Azure Storage die momenteel ondersteuning bieden voor Azure AD-integratie zijn onder andere:
    - [.NET](https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0)
    - [Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) (7.1.x-Preview gebruiken)
    - Python
        - [Blob](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-blob)
        - [Wachtrij](https://github.com/Azure/azure-storage-python/releases/tag/v1.2.0rc1-queue)
    - [Node.js](https://www.npmjs.com/package/azure-storage)
    - [JavaScript](https://aka.ms/downloadazurestoragejs))

> [!IMPORTANT]
> Deze preview is bedoeld voor gebruik van niet-productieve alleen. Productie service level agreements (Sla's) worden pas beschikbaar Azure AD-integratie voor Azure Storage is gedeclareerd in het algemeen beschikbaar. Als u Azure AD-integratie is nog niet ondersteund voor uw scenario, blijven de gedeelde sleutel autorisatie of SAS-tokens gebruiken in uw toepassingen.
>
> Tijdens de preview kunnen de RBAC-roltoewijzingen doorgeven maximaal vijf minuten duren.
>
> Azure AD-integratie met Azure Storage vereist het gebruik van HTTPS voor Azure Storage-bewerkingen.

## <a name="get-started-with-azure-ad-for-storage"></a>Aan de slag met Azure AD voor opslag

De eerste stap bij het gebruik van Azure AD-integratie met Azure Storage is RBAC rollen op voor storage-gegevens voor uw service-principal (een gebruiker, groep of toepassingsservice-principal) of beheerd Service identiteit (MSI) toewijzen. RBAC functies omvatten algemene sets met machtigingen voor containers en -wachtrijen. Zie voor meer informatie over de rollen RBAC voor Azure Storage, [beheren rechten voor het storage-gegevens met RBAC (Preview)](storage-auth-aad-rbac.md).

Voor het gebruik van Azure AD toegang verlenen aan de storage-resources in uw toepassingen, moet u een OAuth 2.0-toegangstoken aanvragen vanuit uw code. Zie voor informatie over het aanvragen van een toegangstoken en deze gebruiken voor het autoriseren van aanvragen naar Azure Storage, [verifiëren met Azure AD van een Azure Storage-toepassing (Preview)](storage-auth-aad-app.md). Als u een Azure Managed Service identiteit (MSI) gebruikt, raadpleegt u [verifiëren met Azure AD van een Azure VM beheerde Service-identiteit (Preview)](storage-auth-aad-msi.md).

Azure CLI en PowerShell ondersteunen nu aangemeld met een Azure AD-identiteit. Nadat u met een Azure AD-identiteit aanmelden, is uw sessie wordt uitgevoerd onder de identiteit. Zie voor meer informatie, [gebruiken van een Azure AD-identiteit voor toegang tot Azure Storage met CLI of PowerShell (Preview)](storage-auth-aad-script.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure AD-integratie voor Azure Blobs en wachtrijen, het Azure Storage-team blogbericht, [aankondigen van de Preview van Azure AD-verificatie voor Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
