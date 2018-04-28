---
title: 'Azure Storage-Service: versleuteling voor gegevens in rust | Microsoft Docs'
description: 'De functie Azure Storage-Service: versleuteling gebruiken voor het versleutelen van Azure Blob-opslag aan de kant van de service bij het opslaan van de gegevens en bij het ophalen van de gegevens worden gedecodeerd.'
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 03/14/2018
ms.author: lakasa
ms.openlocfilehash: 5e4df176104111f44ca95df2b2d5d1c81ed3a4e3
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure Storage Service-versleuteling voor inactieve gegevens

Azure Storage-versleuteling voor gegevens in rust kunt u uw gegevens om te voldoen aan de beveiliging van de organisatie en de naleving verbintenissen beschermen. Met deze functie wordt Azure Storage automatisch de gegevens versleuteld voordat behouden blijven van Azure Storage en ontsleutelt de gegevens voordat ophalen. De verwerking van versleuteling en versleuteling op rest, ontsleuteling en sleutelbeheer in Storage-Service: versleuteling is transparant voor gebruikers. Alle gegevens geschreven naar Azure Storage worden versleuteld via 256-bits [AES-versleuteling](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), een van de sterkste blok coderingen beschikbaar.

Storage-Service: versleuteling is ingeschakeld voor alle nieuwe en bestaande opslagaccounts en kan niet worden uitgeschakeld. Omdat uw gegevens zijn standaard beveiligd, hoeft u niet te wijzigen van uw code of toepassingen kunnen profiteren van de versleuteling van de opslagruimte.

De functie versleutelt automatisch gegevens in:

- Beide prestatielagen (standaard en Premium).
- Beide implementatiemodellen (Azure Resource Manager en classic).
- Alle Azure Storage-services (Blob storage, Queue storage, Table storage en Azure-bestanden). 

Versleuteling van opslag-Service heeft geen invloed op prestaties van de Azure-opslag.

U kunt Microsoft beheerde versleutelingssleutels versleuteling van de opslagruimte of kunt u uw eigen versleutelingssleutels. Zie voor meer informatie over het gebruik van uw eigen sleutels [Service versleuteling van opslag met behulp van de klant beheerd sleutels in Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Versleutelingsinstellingen voor weergave in de Azure portal

Als u wilt weergeven van instellingen voor de versleuteling van de opslagruimte, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com) en selecteer een opslagaccount. In de **instellingen** deelvenster, selecteer de **versleuteling** instelling.

![Portal schermafbeelding van de instelling voor wachtwoordversleuteling](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>Veelgestelde vragen over de versleuteling van opslag-Service

**V: ik heb een klassieke storage-account. Kan ik versleuteling van de opslagruimte op het inschakelen?**

A: Storage-Service: versleuteling is standaard ingeschakeld voor alle opslagaccounts (klassiek en Resource Manager).

**V: hoe kan ik gegevens coderen in mijn klassieke storage-account?**

A: met versleuteling is standaard ingeschakeld, versleutelt Azure Storage automatisch de nieuwe gegevens. 

**V: ik heb een Resource Manager-opslagaccount. Kan ik versleuteling van de opslagruimte op het inschakelen?**

A: storage-Service: versleuteling is standaard ingeschakeld op alle bestaande opslagaccounts van Resource Manager. Dit wordt ondersteund voor Blob storage, Table storage, Queue storage en Azure-bestanden. 

**V: hoe de gegevens in een opslagaccount van Resource Manager versleutelen?**

A: Storage-Service: versleuteling is standaard ingeschakeld voor alle opslagaccounts--klassieke en het Resource Manager worden alle bestaande bestanden in de storage-account maken voordat de versleuteling is ingeschakeld wordt met terugwerkende kracht ophalen versleuteld met een achtergrondproces voor versleuteling.

**V: kan ik storage-accounts maken met de versleuteling van opslag-Service is ingeschakeld met behulp van Azure PowerShell en Azure CLI**

A: Storage-Service: versleuteling is standaard ingeschakeld op het moment van het maken van een opslagaccount (klassieke of Resource Manager). U kunt eigenschappen van account controleren met behulp van zowel Azure PowerShell en Azure CLI.

**V: hoe veel meer kost Azure Storage als versleuteling van opslag-Service is ingeschakeld?**

A: Er is geen extra kosten.

**V: kan ik mijn eigen versleutelingssleutels gebruiken?**

A: Ja, kunt u uw eigen versleutelingssleutels. Zie voor meer informatie [Service versleuteling van opslag met behulp van de klant beheerd sleutels in Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

**V: kan ik de toegang tot de versleutelingssleutels intrekken?**

A: Ja, als u [gebruik van uw eigen versleutelingssleutels](storage-service-encryption-customer-managed-keys.md) in Azure Sleutelkluis.

**V: is de versleuteling van opslag-Service is standaard ingeschakeld wanneer ik een opslagaccount maken?**

A: Ja, versleuteling van de opslagruimte standaard voor alle opslagaccounts en voor alle Azure Storage-services is ingeschakeld.

**V: hoe verschilt dit van Azure Disk Encryption?**

A: azure Disk Encryption wordt gebruikt voor het versleutelen van besturingssysteem en gegevensschijven in IaaS VM's. Zie voor meer informatie de [opslag beveiligingshandleiding](../storage-security-guide.md).

**V: Wat gebeurt er als ik Azure Disk Encryption in mijn gegevensschijven inschakelen?**

A: dit werkt naadloos. Beide methoden worden uw gegevens te versleutelen.

**V: Mijn storage-account is ingesteld geo-redundante worden gerepliceerd. Met Storage-Service Encryption mijn redundante kopie ook worden versleuteld?**

A: Ja, alle exemplaren van het opslagaccount zijn versleuteld. Alle redundantie opties worden ondersteund--lokaal redundante opslag, zone-redundante opslag, geografisch redundante opslag en geografisch redundante opslag met leestoegang.

**V: kan ik versleuteling uitschakelen op mijn storage-account?**

A: versleuteling is standaard ingeschakeld en er zijn geen versleuteling voor uw opslagaccount uitschakelen. 

**V: is versleuteling van opslag Service alleen toegestaan in specifieke gebieden?**

A: storage-Service: versleuteling is beschikbaar in alle regio's voor alle services. 

**V: hoe ik contact op met iemand als ik problemen hebt of feedback wilt geven?**

A: Neem contact op met [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) voor problemen of feedback over versleuteling van de opslagruimte.

## <a name="next-steps"></a>Volgende stappen
Azure Storage biedt dat een uitgebreide reeks beveiligingsmogelijkheden die ontwikkelaars samen help build beveiligde toepassingen. Zie voor meer informatie de [opslag beveiligingshandleiding](../storage-security-guide.md).
