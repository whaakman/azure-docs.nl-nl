---
title: 'Azure Storage-Service: versleuteling voor gegevens in rust | Microsoft Docs'
description: 'De functie Azure Storage-Service: versleuteling gebruiken voor het versleutelen van uw Azure-blobopslag aan de kant van de service bij het opslaan van de gegevens en bij het ophalen van de gegevens worden gedecodeerd.'
services: storage
documentationcenter: .net
author: tamram
manager: timlt
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: tamram
ms.openlocfilehash: 32f622c39583a25a7bc53ffcb6d9be779459badc
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure Storage Service-versleuteling voor inactieve gegevens
Azure Storage Service versleuteling (SSE) voor gegevens in rust helpt u bij het beveiligen en bescherming van uw gegevens om te voldoen aan de beveiliging van de organisatie en de naleving verplichtingen. Met deze functie wordt Azure Storage automatisch versleutelt uw gegevens voordat het behouden blijven van opslag en ontsleutelt vóór ophalen. De versleuteling, ontsleuteling en sleutelbeheer zijn volledig transparant voor gebruikers.

De volgende secties vindt gedetailleerde richtlijnen over het gebruik van de versleuteling van opslag-functies, evenals de ondersteunde scenario's en de gebruiker optreedt.

## <a name="overview"></a>Overzicht
Azure Storage biedt een uitgebreide reeks beveiligingsmogelijkheden die samen kunnen ontwikkelaars om beveiligde toepassingen te bouwen. Gegevens onderweg tussen een toepassing en Azure kunnen worden beveiligd met behulp van [versleuteling van de Client-Side](../storage-client-side-encryption.md), HTTPs- of SMB 3.0. Versleuteling van opslag-Service biedt versleuteling in rust, verwerking versleuteling, ontsleuteling en Sleutelbeheer op een compleet transparante wijze. Alle gegevens worden versleuteld met behulp van 256-bits [AES-versleuteling](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), een van de sterkste blok coderingen beschikbaar.

SSE werkt wanneer de gegevens worden versleuteld wanneer deze wordt geschreven naar Azure Storage en kan worden gebruikt voor Azure Blob Storage en File Storage. De Tool werkt voor het volgende:

* Standard-opslag: Algemeen opslagaccounts die voor Blobs en File storage en Blob storage-accounts
* Premium Storage 
* Alle redundantie niveaus (LRS, ZRS, GRS en RA-GRS)
* Azure Resource Manager opslagaccounts (maar niet klassiek) 
* Alle regio's.

Raadpleeg de veelgestelde vragen voor meer informatie.

Als u wilt in- of uitschakelen van versleuteling van de opslagruimte voor een opslagaccount, meld u aan bij de [Azure-portal](https://portal.azure.com) en selecteer een opslagaccount. Op de blade instellingen, zoekt u naar de sectie Blob-Service zoals weergegeven in deze schermafbeelding en versleuteling op.

![De optie versleuteling Portal schermopname](./media/storage-service-encryption/image1.png)
<br/>*Afbeelding 1: SSE inschakelen voor Blob-Service (stap 1)*

![De optie versleuteling Portal schermopname](./media/storage-service-encryption/image3.png)
<br/>*Afbeelding 2: SSE inschakelen voor File-Service (stap 1)*

Nadat u de instelling voor wachtwoordversleuteling op, kunt u in- of uitschakelen van de versleuteling van de opslagruimte.

![Eigenschappen van de portal schermopname van versleuteling](./media/storage-service-encryption/image2.png)
<br/>*Afbeelding 3: SSE inschakelen voor Blob en File-Service (stap 2)*

## <a name="encryption-scenarios"></a>Versleuteling scenario 's
Versleuteling van opslag-Service kan worden ingeschakeld op een niveau storage-account. Eenmaal is ingeschakeld, wordt de klanten ervoor kiezen welke services voor het versleutelen. Ondersteunt de volgende klant-scenario's:

* Versleuteling van Blob Storage en File Storage in Resource Manager-accounts.
* Versleuteling van Blob en File-Service in de klassieke opslagaccounts eenmaal is gemigreerd naar Resource Manager storage-accounts.

SSE heeft de volgende beperkingen:

* Versleuteling van klassieke opslagaccounts wordt niet ondersteund.
* Bestaande gegevens - SSE alleen nieuwe gegevens worden gecodeerd nadat de codering is ingeschakeld. Als u bijvoorbeeld een nieuwe Resource Manager-opslagaccount maken maar versleuteling niet inschakelen en vervolgens blobs of gearchiveerde VHD uploaden naar dit opslagaccount en schakelt u SSE, wordt deze blobs niet versleuteld tenzij ze zijn herschreven of gekopieerd.
* Ondersteuning voor Marketplace - versleuteling inschakelen van virtuele machines gemaakt op basis van de Marketplace met behulp van de [Azure-portal](https://portal.azure.com), PowerShell en Azure CLI. De VHD-basisinstallatiekopie blijft niet-versleutelde; maar wordt er geen schrijfbewerkingen uitgevoerd nadat de virtuele machine heeft ingezette versleuteld.
* Tabel en gegevens wachtrijen wordt niet versleuteld.

## <a name="getting-started"></a>Aan de slag
### <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Stap 1: [maken van een nieuw opslagaccount](../storage-create-storage-account.md).
### <a name="step-2-enable-encryption"></a>Stap 2: Versleuteling inschakelen.
U kunt inschakelen met behulp van versleuteling de [Azure-portal](https://portal.azure.com).

> [!NOTE]
> Als u programmatisch wilt inschakelen of uitschakelen van de Storage-Service: versleuteling op een storage-account, kunt u de [REST API van Azure Storage Resource Provider](https://msdn.microsoft.com/library/azure/mt163683.aspx), wordt de [Storage Resource Provider-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Azure PowerShell](/powershell/azureps-cmdlets-docs), of de [Azure CLI](../storage-azure-cli.md).
> 
> 

### <a name="step-3-copy-data-to-storage-account"></a>Stap 3: Gegevens kopiëren naar de storage-account
Als u SSE voor de Blob-service inschakelt, worden alle bestaande blobs geschreven naar dit opslagaccount wordt versleuteld. Alle blobs die zich al in de betreffende storage-account worden niet versleuteld totdat ze opnieuw worden geschreven. U kunt de gegevens kopiëren van een opslagaccount naar een met SSE versleuteld, of zelfs SSE- en de blobs naar de andere kopiëren uit een container op ervoor dat de eerder opgeslagen gegevens worden versleuteld. U kunt een van de volgende hulpprogramma's gebruiken om dit te bereiken. Dit is hetzelfde gedrag voor File Storage ook.

#### <a name="using-azcopy"></a>Met behulp van AzCopy
AzCopy is een Windows-opdrachtregelprogramma ontworpen voor het kopiëren van gegevens naar en van Microsoft Azure Blob-, bestands- en tabel opslag met behulp van eenvoudige opdrachten met optimale prestaties. U kunt deze gebruiken uw blobs of bestanden vanuit een opslagaccount kopiëren naar een ander subnet met SSE ingeschakeld. 

Voor meer informatie gaat u naar [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md).

#### <a name="using-smb"></a>Gebruik van SMB-
Azure Files biedt bestandsshares in de cloud met behulp van het standaard SMB-protocol. U kunt een bestandsshare koppelen van een client on-premises of in Azure. Zodra gekoppeld, kunnen hulpprogramma's zoals Robocopy worden gebruikt om bestanden te kopiëren via naar Azure-bestandsshares. Zie voor meer informatie [het koppelen van de Azure-bestandsshare op Windows](../files/storage-how-to-use-files-windows.md) en [het koppelen van Azure-bestandsshare op Linux](../files/storage-how-to-use-files-linux.md).


#### <a name="using-the-storage-client-libraries"></a>Met behulp van de clientbibliotheken Storage
U kunt gegevens voor blob of een bestand kopiëren naar en van blob-opslag of tussen opslagaccounts met behulp van onze uitgebreide set Opslagclientbibliotheken waaronder .NET, C++, Java, Android, Node.js, PHP, Python en Ruby.

Voor meer informatie gaat u naar onze [aan de slag met Azure Blob storage met .NET](../blobs/storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Met behulp van een Opslagverkenner
U kunt een Opslagverkenner storage-accounts maken, uploaden en downloaden van gegevens, inhoud van BLOB's weergeven en navigeren door mappen. U kunt een van deze gebruiken voor het uploaden van BLOB's naar uw storage-account met versleuteling ingeschakeld. Met sommige Opslagverkenner kunt u ook gegevens uit bestaande blob storage kopiëren naar een andere container in het opslagaccount of een nieuw opslagaccount met SSE ingeschakeld.

Voor meer informatie gaat u naar [Azure Storage Explorers](../storage-explorers.md).

### <a name="step-4-query-the-status-of-the-encrypted-data"></a>Stap 4: De status van de versleutelde gegevens opvragen
Een bijgewerkte versie van de opslag-clientbibliotheken er is geïmplementeerd waarmee u de status van een object om te bepalen of ze zijn versleuteld of niet. Dit is momenteel alleen beschikbaar voor Blob-opslag. Ondersteuning voor File storage is op het overzicht. 

U kunt in de tussentijd aanroepen [accounteigenschappen ophalen](https://msdn.microsoft.com/library/azure/mt163553.aspx) Controleer of het opslagaccount heeft versleuteling ingeschakeld of de eigenschappen van het opslagaccount weergeven in de Azure portal.

## <a name="encryption-and-decryption-workflow"></a>Versleuteling en ontsleuteling werkstroom
Hier volgt een korte beschrijving van de werkstroom tokenversleuteling /-ontsleuteling:

* De klant schakelt u versleuteling in de storage-account.
* Wanneer de klant nieuwe gegevens (plaatsen Blob, plaatsen blok plaatsen pagina, enz. het bestand plaatsen) schrijft naar Blob of bestandsopslag; elke schrijfbewerking is versleuteld met 256-bits [AES-versleuteling](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), een van de sterkste blok coderingen beschikbaar.
* Wanneer de klant moet voor toegang tot gegevens (Blob ophalen, enzovoort), worden gegevens automatisch ontsleuteld voordat u terugkeert naar de gebruiker.
* Als versleuteling is uitgeschakeld, nieuwe schrijfbewerkingen zijn niet langer gecodeerd en bestaande versleutelde gegevens blijven versleuteld totdat herschreven door de gebruiker. Terwijl versleuteling is ingeschakeld, worden schrijfbewerkingen naar de opslag-Blob of het bestand wordt versleuteld. De status van gegevens wordt niet gewijzigd met de gebruiker schakelen tussen in-/ uitschakelen versleuteling voor het opslagaccount.
* Alle versleutelingssleutels zijn opgeslagen, gecodeerd en beheerd door Microsoft.

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Veelgestelde vragen over Service-versleuteling van opslag voor gegevens in rust
**V: ik heb een bestaande klassieke storage-account. Kan ik SSE deze functie inschakelen?**

A: Nee, SSE wordt alleen ondersteund op Resource Manager storage-accounts.

**V: hoe kan ik gegevens coderen in mijn klassieke storage-account?**

A: u kunt een nieuwe Resource Manager-opslagaccount maken en kopieer uw gegevens met [AzCopy](storage-use-azcopy.md) van uw bestaande klassieke storage-account aan uw zojuist gemaakte Resource Manager-opslagaccount. 

Als u uw klassieke opslagaccount migreert naar een Resource Manager-storage-account, deze onmiddellijk wordt, wordt het type van uw account, maar heeft geen invloed op uw bestaande gegevens. Kan geen nieuwe gegevens geschreven wordt versleuteld alleen na het inschakelen van versleuteling. Zie voor meer informatie [Platform ondersteund van IaaS bronnen voor migratie van klassiek naar Resource Manager](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/). Houd er rekening mee dat dit wordt alleen ondersteund voor Blob- en -services.

**V: ik heb een bestaand Resource Manager-opslagaccount. Kan ik SSE deze functie inschakelen?**

Ja, maar alleen nieuwe gegevens geschreven A: wordt versleuteld. Deze niet teruggaan en gegevens versleutelen die al aanwezig is. Dit is nog niet ondersteund voor de Preview van File Storage.

**V: ik wil de huidige gegevens in een bestaand Resource Manager-opslagaccount te versleutelen?**

A: u kunt de SSE inschakelen op elk gewenst moment in een Resource Manager-opslagaccount. Gegevens die al aanwezig waren wordt echter niet versleuteld. Bestaande om gegevens te versleutelen, kunt u deze kopiëren naar een andere naam of een andere container en verwijder vervolgens de niet-versleutelde versies.

**V: ik gebruik Premium-opslag; kan ik SSE gebruiken?**

A: Ja, de SSE op zowel Standard-opslag- en Premium-opslag wordt ondersteund.  Premium-opslag wordt niet ondersteund voor de File-Service.

**V: als ik een nieuw opslagaccount maken en inschakelen van SSE en vervolgens een nieuwe virtuele machine maken met dit opslagaccount, betekent dit dat mijn VM is versleuteld?**

A: Ja. Alle schijven gemaakt die gebruikmaken van het nieuwe opslagaccount worden versleuteld, zolang ze worden gemaakt nadat de SSE is ingeschakeld. Als de virtuele machine is gemaakt met behulp van Azure-marktplaats, blijft de basisinstallatiekopie van het VHD niet-versleutelde; maar wordt er geen schrijfbewerkingen uitgevoerd nadat de virtuele machine heeft ingezette versleuteld.

**V: kan ik nieuwe storage-accounts maken met de SSE ingeschakeld met behulp van Azure PowerShell en Azure CLI**

A: Ja.

**V: hoe veel meer kost Azure Storage als SSE is ingeschakeld?**

A: Er is geen extra kosten.

**V: die de versleutelingssleutels beheert?**

A: de sleutels worden beheerd door Microsoft.

**V: kan ik mijn eigen versleutelingssleutels gebruiken?**

A: Wij werken op het bieden van mogelijkheden voor klanten om hun eigen versleutelingssleutels.

**V: kan ik de toegang tot de versleutelingssleutels intrekken?**

A: niet op dit moment; de sleutels zijn volledig beheerd door Microsoft.

**V: is SSE standaard ingeschakeld wanneer ik een nieuw opslagaccount maken?**

A: het Azure Storage-team is aan het inschakelen van versleuteling standaard door Microsoft beheerde sleutels voor alle gegevens die worden geschreven naar Azure Storage (Blob, bestand, Table en Queue storage) en voor alle opslagaccounts (Azure Resource Manager en Classic opslag accounts), zowel nieuwe als bestaande.

**V: hoe verschilt dit van Azure Disk Encryption?**

A: deze functie wordt gebruikt voor het versleutelen van gegevens in Azure Blob storage. De Azure Disk Encryption wordt gebruikt voor het versleutelen van besturingssysteem en gegevensschijven in IaaS VM's. Ga voor meer informatie naar onze [opslag beveiligingshandleiding](../storage-security-guide.md).

**V: Wat gebeurt er als ik SSE, inschakelen en vervolgens in te gaan en Azure Disk Encryption inschakelen op de schijven?**

A: dit werkt naadloos. Uw gegevens worden versleuteld door beide methoden.

**V: Mijn storage-account is ingesteld geo-redundante worden gerepliceerd. Als ik SSE inschakelt, worden mijn redundante kopie ook versleuteld?**

A: Ja, alle exemplaren van het opslagaccount zijn versleuteld en alle redundantieopties voor, Zone-redundante opslag (ZRS) lokaal redundante opslag (LRS) Geo-Redundant Storage (GRS) en geografisch redundante opslag met leestoegang (RA-GRS), worden ondersteund.

**V: ik inschakelen niet codering voor mijn storage-account.**

A: is het een Resource Manager-storage-account? Klassieke opslagaccounts worden niet ondersteund. 

**V: is SSE alleen toegestaan in specifieke gebieden?**

A: de SSE is beschikbaar in alle regio's voor Blob-opslag. Controleer of de sectie beschikbaarheid voor File storage. 

**V: hoe neem ik iemand als ik problemen hebt of feedback wilt geven?**

A: neemt contact op met [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) van eventuele problemen met versleuteling van de opslagruimte.

## <a name="next-steps"></a>Volgende stappen
Azure Storage biedt een uitgebreide reeks beveiligingsmogelijkheden die samen kunnen ontwikkelaars om beveiligde toepassingen te bouwen. Voor meer informatie gaat u naar de [opslag beveiligingshandleiding](../storage-security-guide.md).

