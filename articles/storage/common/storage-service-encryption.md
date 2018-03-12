---
title: 'Azure Storage-Service: versleuteling voor gegevens in rust | Microsoft Docs'
description: 'De functie Azure Storage-Service: versleuteling gebruiken voor het versleutelen van uw Azure-blobopslag aan de kant van de service bij het opslaan van de gegevens en bij het ophalen van de gegevens worden gedecodeerd.'
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 03/06/2018
ms.author: lakasa
ms.openlocfilehash: 29f6a38f7a7f0f107dab8c99e750a8dec803f6f0
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure Storage Service-versleuteling voor inactieve gegevens

Azure Storage Service versleuteling (SSE) voor gegevens in rust helpt u bij het beveiligen en bescherming van uw gegevens om te voldoen aan de beveiliging van de organisatie en de naleving verplichtingen. Met deze functie van Azure Storage automatisch uw gegevens voorafgaand aan versleutelt behouden blijven van Azure Storage en ontsleuteld voordat ophalen. De verwerking van versleuteling en versleuteling op rest, ontsleuteling en geleverd door SSE Sleutelbeheer NLB is zichtbaar voor gebruikers. Alle gegevens die worden geschreven naar Azure Storage is versleuteld met 256-bits [AES-versleuteling](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), een van de sterkste blok coderingen beschikbaar.

SSE is ingeschakeld voor alle nieuwe en bestaande opslagaccounts en kan niet worden uitgeschakeld. Omdat uw gegevens zijn standaard beveiligd, hoeft u niet om uw code of toepassingen kunnen profiteren van SSE te wijzigen.

 SSE versleutelt automatisch gegevens in alle prestatielagen (standaard en Premium), alle implementatiemodellen (Azure Resource Manager en het klassieke model) en alle services van Azure Storage (Blob, Queue, Table en -bestand). 

U kunt Microsoft beheerde versleutelingssleutels SSE of kunt u uw eigen versleutelingssleutels. Zie voor meer informatie over het gebruik van uw eigen sleutels [Service versleuteling van opslag met behulp van de klant beheerde sleutels in Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Versleutelingsinstellingen voor weergave in de Azure portal

Als u wilt weergeven van instellingen voor de versleuteling van de opslagruimte, meld u aan bij de [Azure-portal](https://portal.azure.com) en selecteer een opslagaccount. Op de **instellingen** blade, klik op de instelling voor wachtwoordversleuteling.

![De optie versleuteling Portal schermopname](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>Veelgestelde vragen over de versleuteling van opslag-Service

**V: ik heb een bestaande klassieke storage-account. Kan ik SSE deze functie inschakelen?**

A: SSE is standaard ingeschakeld voor alle opslagaccounts (klassiek en Resource Manager storage-accounts).

**V: hoe kan ik gegevens coderen in mijn klassieke storage-account?**

A: met versleuteling is standaard ingeschakeld, wordt de nieuwe gegevens automatisch versleuteld door de storage-service. 

**V: ik heb een bestaand Resource Manager-opslagaccount. Kan ik SSE deze functie inschakelen?**

A: SSE is standaard ingeschakeld op alle bestaande opslagaccounts van Resource Manager. Dit wordt ondersteund voor Blobs, tabellen, bestand en Queue storage. 

**V: ik wil de huidige gegevens in een bestaand Resource Manager-opslagaccount te versleutelen?**

A: met SSE standaard ingeschakeld voor alle opslagaccounts - klassieke en het Resource Manager storage-accounts. Gegevens die al aanwezig waren wordt echter niet versleuteld. Bestaande om gegevens te versleutelen, kunt u deze kopiëren naar een andere naam of een andere container en verwijder vervolgens de niet-versleutelde versies. 

**V: kan ik nieuwe storage-accounts maken met de SSE ingeschakeld met behulp van Azure PowerShell en Azure CLI**

A: SSE is standaard ingeschakeld op het moment van het maken van een opslagaccount (klassieke en het Resource Manager-accounts). U kunt met behulp van Azure PowerShell en Azure CLI accounteigenschappen controleren.

**V: hoe veel meer kost Azure Storage als SSE is ingeschakeld?**

A: Er is geen extra kosten.

**V: die de versleutelingssleutels beheert?**

A: de sleutels worden beheerd door Microsoft.

**V: kan ik mijn eigen versleutelingssleutels gebruiken?**

A: Wij werken op het bieden van mogelijkheden voor klanten om hun eigen versleutelingssleutels.

**V: kan ik de toegang tot de versleutelingssleutels intrekken?**

A: niet op dit moment; de sleutels zijn volledig beheerd door Microsoft.

**V: is SSE standaard ingeschakeld wanneer ik een nieuw opslagaccount maken?**

A: Ja, met behulp van Microsoft Managed sleutels SSE is standaard ingeschakeld voor alle opslagaccounts - Azure Resource Manager en klassieke opslagaccounts. Het is ingeschakeld voor alle services ook - Blob-, tabel, in de wachtrij en File storage.

**V: hoe verschilt dit van Azure Disk Encryption?**

A: azure Disk Encryption wordt gebruikt voor het versleutelen van besturingssysteem en gegevensschijven in IaaS VM's. Ga voor meer informatie naar onze [opslag beveiligingshandleiding](../storage-security-guide.md).

**V: Wat gebeurt er als ik Azure Disk Encryption in mijn gegevensschijven inschakelen?**

A: dit werkt naadloos. Uw gegevens worden versleuteld door beide methoden.

**V: Mijn storage-account is ingesteld geo-redundante worden gerepliceerd. Met SSE, worden mijn redundante kopie ook versleuteld?**

A: Ja, alle exemplaren van het opslagaccount zijn versleuteld en alle redundantieopties voor, Zone-redundante opslag (ZRS) lokaal redundante opslag (LRS) Geo-Redundant Storage (GRS) en geografisch redundante opslag met leestoegang (RA-GRS), worden ondersteund.

**V: ik kan geen versleuteling uitschakelen op mijn storage-account.**

A: versleuteling is standaard ingeschakeld en er zijn geen versleuteling voor uw opslagaccount uitschakelen. 

**V: is SSE alleen toegestaan in specifieke gebieden?**

A: de SSE is beschikbaar in alle regio's voor alle services. 

**V: hoe neem ik iemand als ik problemen hebt of feedback wilt geven?**

A: Neem contact op met [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) van eventuele problemen met versleuteling van de opslagruimte.

## <a name="next-steps"></a>Volgende stappen
Azure Storage biedt een uitgebreide reeks beveiligingsmogelijkheden die samen kunnen ontwikkelaars om beveiligde toepassingen te bouwen. Voor meer informatie gaat u naar de [opslag beveiligingshandleiding](../storage-security-guide.md).
