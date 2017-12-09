---
title: 'Azure Storage-Service: versleuteling met behulp van de klant beheerde sleutels in Azure Sleutelkluis | Microsoft Docs'
description: 'De functie Azure Storage-Service: versleuteling gebruiken voor het versleutelen van uw Azure-blobopslag aan de kant van de service bij het opslaan van de gegevens en ontsleutelen wanneer sleutels bij het ophalen van de gegevens met behulp van de klant beheerd.'
services: storage
documentationcenter: .net
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: lakasa
ms.openlocfilehash: 0a05a0d28899cc3db11f8fda8aec5bd6ed9bd5f8
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Versleuteling van opslag-Service met behulp van de klant beheerde sleutels in Azure Sleutelkluis

Microsoft Azure is zeer belangrijk dat u kunt beschermen en bescherming van uw gegevens om te voldoen aan de beveiliging van de organisatie en de naleving verplichtingen.  Een manier die u kunt uw gegevens in rust beveiligen is het gebruik van opslag Service versleuteling (SSE), die automatisch worden uw gegevens worden versleuteld wanneer het schrijven naar de opslag en uw gegevens ontsleutelt bij het ophalen van het. De versleuteling en ontsleuteling is automatische, volledig transparant en maakt gebruik van 256-bits [AES-versleuteling](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), een van de sterkste blok coderingen beschikbaar.

U kunt Microsoft beheerde versleutelingssleutels SSE of kunt u uw eigen versleutelingssleutels. Dit artikel worden de laatste. Zie voor meer informatie over het gebruik van door Microsoft beheerde sleutels of over SSE in het algemeen [Service versleuteling van opslag voor gegevens in rust](../storage-service-encryption.md).

Als u de mogelijkheid om met uw eigen versleutelingssleutels, is de SSE voor Blob storage Azure van met Azure Key Vault (Sleutelkluis) geïntegreerd. U kunt uw eigen versleutelingssleutels maken en op te slaan in Azure Sleutelkluis, of kunt u API's van Azure Sleutelkluis coderingssleutels wilt genereren. Niet alleen Azure Sleutelkluis in staat om te beheren en uw sleutels beheren, ook kunt u het gebruik van uw sleutel controleren. 

Waarom zou u wilt uw eigen sleutels maken? Dit biedt u meer flexibiliteit, zodat u kunt maken, draaien, uitschakelen en toegangsbeheer definiëren. Ook kunt u de versleutelingssleutels die wordt gebruikt voor het beveiligen van uw gegevens te controleren.

## <a name="sse-with-customer-managed-keys-preview"></a>SSE met de klant beheerde sleutels preview

Deze functie is momenteel beschikbaar als preview-product. Deze functie wilt gebruiken, moet u een nieuw opslagaccount maken. U kunt een nieuwe sleutelkluis maken en sleutel of u een bestaande sleutelkluis en sleutel kunt gebruiken. Het opslagaccount en de sleutelkluis moet in dezelfde regio, maar ze kunnen zich in verschillende abonnementen behoren.

Als u wilt deelnemen aan de preview, neem contact op met [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com). We bieden een speciale koppeling als u wilt deelnemen aan de Preview-versie.

Raadpleeg voor meer informatie de [Veelgestelde vragen over](#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).

> [!IMPORTANT]
> U moet zich registreren voor de preview voordat u de stappen in dit artikel. Zonder toegang tot het preview zich u niet inschakelen van deze functie in de portal.

## <a name="getting-started"></a>Aan de slag
## <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Stap 1: [een nieuw opslagaccount maken](../storage-create-storage-account.md)

## <a name="step-2-enable-encryption"></a>Stap 2: Enable-versleuteling
U kunt de SSE inschakelen voor de storage-account met de [Azure-portal](https://portal.azure.com). Op de blade instellingen voor het opslagaccount, zoek naar de sectie Blob-Service, zoals weergegeven in de volgende afbeelding en versleuteling op.

![De optie versleuteling Portal schermopname](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)
<br/>*SSE voor Blob-Service inschakelen*

Als u programmatisch wilt inschakelen of uitschakelen van de Storage-Service: versleuteling op een storage-account, kunt u de [REST API van Azure Storage Resource Provider](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN), wordt de [Storage Resource Provider-clientbibliotheek voor .NET](https://docs.microsoft.com/dotnet/api/?redirectedfrom=MSDN), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.0.0), of de [Azure CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli).

In dit scherm als u het selectievakje 'gebruiken your own key' niet ziet zijn u niet goedgekeurd voor de preview. Een e-mail verzenden naar [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) en goedkeuring aanvragen.

![Portal schermafbeelding versleuteling Preview](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

Standaard gebruikt SSE Microsoft beheerde sleutels. Schakel het selectievakje in voor het gebruik van uw eigen sleutels. Vervolgens kunt u uw sleutel URI opgeven, of Selecteer een sleutel en de Sleutelkluis in de objectkiezer.

## <a name="step-3-select-your-key"></a>Stap 3: Uw sleutel selecteren

![Portal schermopname Versleutelingen gebruik uw eigen sleutel optie](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)

![Portal schermafbeelding versleuteling met sleutel uri optie invoeren](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Als het storage-account geen toegang tot de Sleutelkluis heeft, kunt u de volgende opdracht met Azure Powershell toegang verlenen tot de storage-accounts aan de vereiste sleutelkluis uitvoeren.

![Portal schermafbeelding toegang is geweigerd voor sleutelkluis](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

U kunt ook toegang via de Azure-portal door te gaan naar de Azure Sleutelkluis in Azure portal en het verlenen van toegang tot het opslagaccount.

## <a name="step-4-copy-data-to-storage-account"></a>Stap 4: Gegevens kopiëren naar de storage-account
Als u gegevens overdragen naar uw nieuwe opslagaccount wilt zodat ze zijn versleuteld, raadpleegt u [stap 3 van aan de slag in Service-versleuteling van opslag voor gegevens in rust](https://docs.microsoft.com/azure/storage/storage-service-encryption#step-3-copy-data-to-storage-account).

## <a name="step-5-query-the-status-of-the-encrypted-data"></a>Stap 5: De status van de versleutelde gegevens opvragen
Als u wilt de status van de versleutelde gegevens opvragen, verwijzen naar [stap 4 van aan de slag in Service-versleuteling van opslag voor gegevens in rust](https://docs.microsoft.com/azure/storage/storage-service-encryption#step-4-query-the-status-of-the-encrypted-data).

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Veelgestelde vragen over Service-versleuteling van opslag voor gegevens in rust
**V: ik gebruik Premium-opslag; kan ik SSE gebruiken met sleutels van de klant beheerd?**

A: Ja SSE met door Microsoft beheerd en de klant beheerde sleutels op zowel Standard-opslag- en Premium-opslag wordt ondersteund. 

**V: kan ik nieuwe storage-accounts met SSE met beheer van de klant sleutels ingeschakeld met behulp van Azure PowerShell en Azure CLI maken?**

A: Ja.

**V: hoe veel meer kosten van Azure Storage biedt als SSE met klant beheerde sleutels is ingeschakeld?**

A: Er is een waarde die is gekoppeld voor het gebruik van Azure Sleutelkluis. Voor meer informatie gaat u naar [Sleutelkluis prijzen](https://azure.microsoft.com/en-us/pricing/details/key-vault/). Er is geen extra kosten voor het gebruik van SSE.

**V: kan ik de toegang tot de versleutelingssleutels intrekken?**

A: Ja, kunt u de toegang intrekken op elk gewenst moment. Er zijn verschillende manieren toegang tot uw sleutels in te trekken. Raadpleeg [Azure Key Vault PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.0.0) en [Azure Key Vault CLI](https://docs.microsoft.com/cli/azure/keyvault) voor meer informatie. Toegang intrekken wordt toegang tot alle blobs in de storage-account effectief geblokkeerd omdat de versleutelingssleutel van de Account niet toegankelijk door Azure Storage is.

**V: kan ik een opslagaccount en de sleutel in andere regio maken?**

A: Nee, de storage-account en de key vault/sleutel moeten in dezelfde regio. 

**V: kan ik SSE inschakelen met beheer van de klant sleutels bij het maken van het storage-account?**

A: Nee. Wanneer u SSE inschakelt tijdens het maken van het storage-account, kunt u alleen Microsoft beheerde sleutels. Als u wilt de klant beheerd sleutels wilt gebruiken, moet u de eigenschappen van het opslagaccount bijwerken. U kunt gebruik REST of een van de opslagclientbibliotheken programmatisch uw storage-account bijwerken of bijwerken van de eigenschappen van het opslagaccount met de Azure portal na het maken van het account.

**V: kan ik versleuteling uitschakelen tijdens het gebruik van SSE met de klant beheerde sleutels?**

A: u kunt versleuteling Nee, niet uitschakelen terwijl SSE gebruiken met de klant sleutels beheerde. Schakel codering moet u overschakelen naar door Microsoft beheerde sleutels. U kunt dit doen met de Azure-portal of PowerShell.

**V: is SSE standaard ingeschakeld wanneer ik een nieuw opslagaccount maken?**

A: SSE is niet standaard; u kunt de Azure-portal in te schakelen. U kunt deze functie met de REST-API van Storage Resource Provider ook programmatisch inschakelen. 

**V: ik inschakelen niet codering voor mijn storage-account.**

A: is het een Resource Manager-storage-account? Klassieke opslagaccounts worden niet ondersteund. SSE met sleutels voor beheer van de klant kan ook worden ingeschakeld alleen op de zojuist gemaakte Resource Manager-opslagaccounts.

**V: SSE Is met de klant beheerde sleutels alleen toegestaan in specifieke gebieden?**

A: SSE is beschikbaar in alleen bepaalde regio's voor Blob-opslag voor deze Preview. E-mailadres [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) om te controleren op de beschikbaarheid en meer informatie over de preview. 

**V: hoe neem ik iemand als ik problemen hebt of feedback wilt geven?**

A: Neem contact op met [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) van eventuele problemen met versleuteling van de opslagruimte. 

## <a name="next-steps"></a>Volgende stappen

*   Voor meer informatie over de uitgebreide set van beveiliging mogelijkheden die ontwikkelaars helpen bij het ontwikkelen van beveiligde toepassingen, Zie de [opslag beveiligingshandleiding](https://docs.microsoft.com/azure/storage/storage-security-guide).
*   Zie voor informatie over Azure Sleutelkluis [wat is Azure Sleutelkluis](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?
*   Zie voor aan de slag op Azure Sleutelkluis, [aan de slag met Azure Key Vault](../../key-vault/key-vault-get-started.md).
