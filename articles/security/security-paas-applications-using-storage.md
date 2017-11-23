---
title: Beveiliging van PaaS-toepassingen met behulp van Azure Storage | Microsoft Docs
description: " Meer informatie over Azure Storage security best practices voor het beveiligen van uw PaaS-web- en mobiele toepassingen. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomShinder
ms.openlocfilehash: 16ee6d9d2f02c758d7682626a8b71a3ff17f841c
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="securing-paas-web-and-mobile-applications-using-azure-storage"></a>Beveiligen van PaaS-webtoepassingen en mobiele toepassingen met behulp van Azure Storage
In dit artikel bespreken we een verzameling van Azure Storage aanbevolen beveiligingsprocedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen. Deze aanbevolen procedures zijn afgeleid van onze ervaring met Azure en de ervaringen van klanten zoals zelf.

De [Azure Storage-beveiligingshandleiding](../storage/common/storage-security-guide.md) bevat een schat voor gedetailleerde informatie over Azure Storage en beveiliging.  Dit artikel heeft betrekking op een hoog niveau enkele concepten gevonden in de handleiding voor beveiliging en koppelingen naar de beveiligingshandleiding, evenals andere bronnen voor meer informatie.

## <a name="azure-storage"></a>Azure Storage
Azure maakt het mogelijk te implementeren en gebruiken van opslag op manieren niet eenvoudig haalbare lokale. U kunt een hoge mate van schaalbaarheid en beschikbaarheid met relatief weinig moeite bereiken met Azure storage. Niet alleen Azure storage vormt de basis voor Windows en Linux Azure Virtual Machines, kan het ook grote gedistribueerde toepassingen ondersteunen.

Azure Storage biedt de volgende vier services: Blob Storage, Table Storage, Queue Storage en File Storage. Zie voor meer informatie, [Inleiding tot Microsoft Azure Storage](../storage/storage-introduction.md).

## <a name="best-practices"></a>Aanbevolen procedures
In dit artikel worden de volgende aanbevolen procedures:

- Beveiliging:
   - Shared Access Signatures (SAS)
   - Beheerde schijven
   - RBAC (op rollen gebaseerd toegangsbeheer)

- Versleuteling van opslag:
   - Versleuteling aan de clientzijde voor waardevolle gegevens
   - Azure Disk Encryption voor virtuele machines (VM's)
   - Storage Service-versleuteling

## <a name="access-protection"></a>Netwerktoegangsbeveiliging
### <a name="use-shared-access-signature-instead-of-a-storage-account-key"></a>Shared Access Signature gebruiken in plaats van de sleutel van een opslagaccount

In een IaaS-oplossing, meestal met Windows Server of Linux virtuele machines met bestanden beveiligd tegen openbaarmaking en beveiligingsactiviteiten bedreigingen met behulp van de toegangsmechanismen. U wilt gebruiken op Windows [toegangsbeheerlijsten (ACL)](../virtual-network/virtual-networks-acl.md) en op Linux, u waarschijnlijk gebruikt [type chmod](https://en.wikipedia.org/wiki/Chmod). In wezen, dit is precies wat u zou doen als u bestanden op een server in uw eigen datacenter vandaag zijn beveiligt.

PaaS verschilt. Een van de meest voorkomende manieren voor het opslaan van bestanden in Microsoft Azure is het gebruik [Azure Blob storage](../storage/storage-dotnet-how-to-use-blobs.md). Een verschil tussen de Blob storage en andere opslag van bestanden is het bestand i/o- en beveiligingsmethoden die worden geleverd met i/o bestand.

Toegangsbeheer is essentieel. Om u te helpen toegang tot Azure-opslag, het systeem wordt gegenereerd twee 512-bits opslagaccountsleutels (SAKs) wanneer u [een opslagaccount maken](../storage/common/storage-create-storage-account.md). Het niveau van redundantie voor belangrijke maakt het mogelijk is voor u om te voorkomen dat de interrupt service tijdens het routinematig sleutel draaien.

Toegangssleutels voor opslag moeten hoge prioriteit geheimen zijn en alleen toegankelijk is voor die verantwoordelijk is voor toegangsbeheer voor opslag. Als de juiste mensen toegang tot deze sleutels krijgen, kan ze hebben volledige controle over opslag en vervangen, verwijderen of bestanden toevoegen aan opslag. Dit omvat malware en andere typen inhoud die u kunt niet optimaal van uw organisatie of voor uw klanten.

U moet nog steeds een manier om toegang tot objecten in de opslag te bieden. Voor meer gedetailleerd toegang kunt u profiteren van [Shared Access Signature](../storage/common/storage-dotnet-shared-access-signature-part-1.md) (SAS). De SAS maakt het mogelijk dat u specifieke objecten in de opslag voor een vooraf gedefinieerde tijdsinterval en met specifieke machtigingen delen. Een Shared Access Signature kunt u definiëren:

- De tijdsinterval waarover de SAS geldig zijn, inclusief de begintijd en de verlooptijd is.
- De machtigingen verleend door de SAS. Een SAS voor een blob kan bijvoorbeeld, verleent u een gebruiker lees en schrijfmachtigingen voor blob maar niet verwijderen machtigingen.
- Een optionele IP-adres of het bereik van IP-adressen waaruit Azure Storage de SAS accepteert. U kunt bijvoorbeeld een bereik met IP-adressen die horen bij uw organisatie opgeven. Dit biedt een andere meting van beveiliging voor uw SAS.
- Het protocol waarvoor Azure Storage de SAS accepteert. U kunt deze optionele parameter gebruiken om toegang te beperken op clients met behulp van HTTPS.

SAS kunt u de manier waarop u wilt delen zonder dat opslag uw toegangscodes voor opslag van inhoud delen. Altijd via SAS in uw toepassing is een veilige manier voor het delen van uw opslagresources uw toegangscodes voor opslag in gevaar.

Zie voor meer informatie, [handtekeningen voor gedeelde toegang met behulp van](../storage/common/storage-dotnet-shared-access-signature-part-1.md) (SAS). Zie voor meer informatie over mogelijke risico's en aanbevelingen die risico's te beperken, [Best practices wanneer via SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

### <a name="use-managed-disks-for-vms"></a>Beheerde schijven voor virtuele machines gebruiken

Wanneer u de optie [Azure beheerd schijven](../storage/storage-managed-disks-overview.md), Azure beheert de storage-accounts die u voor uw VM-schijven gebruikt. U hoeft te doen is Kies het type schijf (Premium of standaard) en de schijfgrootte; Azure-opslag doet de rest. U hoeft niet te hoeven maken over de limieten voor schaalbaarheid die zijn anders u naar meerdere accounts voor opslag moet mogelijk.

Zie voor meer informatie, [Veelgestelde vragen over beheerde en onbeheerde premium-schijven](../storage/storage-faq-for-disks.md).

### <a name="use-role-based-access-control"></a>Op rollen gebaseerde toegangsbeheer gebruiken

Eerder besproken Shared Access Signature (SAS) met beperkte toegang tot objecten in uw opslagaccount verlenen aan andere clients zonder dat de sleutel van uw account-opslagaccount. Soms is de risico's die zijn gekoppeld aan een bepaalde bewerking op basis van uw opslagaccount opwegen tegen de voordelen van SAS. Soms is het eenvoudiger om toegang op een andere manier te beheren.

Een andere manier om toegang te beheren is met [rollen gebaseerd toegangsbeheer](../active-directory/role-based-access-control-what-is.md) (RBAC). Met RBAC, richten op uw werknemers de exacte machtigingen die ze nodig hebben, op basis van de noodzaak om te weten en minimale bevoegdheden beveiligingsprincipes. Te veel machtigingen kunnen een account dat kwaadwillende personen worden blootgesteld. Te weinig machtigingen betekent dat werknemers hun werk efficiënt kunnen niet ophalen. RBAC kunt u dit probleem oplossen door het aanbieden van Geavanceerd toegangsbeheer voor Azure. Dit is noodzakelijk voor organisaties die willen beveiligingsbeleid instellen voor toegang tot gegevens.

U kunt gebruikmaken van ingebouwde RBAC-rollen in Azure rechten toewijzen aan gebruikers. Overweeg het gebruik van Storage Account Inzender voor cloudoperators die nodig zijn voor het beheren van storage-accounts en de rol Inzender van de Account klassieke opslag voor het beheren van klassieke opslagaccounts. Voor cloudoperators die moeten beheren van virtuele machines, maar niet het virtuele netwerk of opslag account waaraan ze zijn verbonden, kunt u deze toevoegt aan de rol Inzender van de virtuele Machine.

Organisaties die niet afgedwongen door toegangsbeheer gegevens dankzij het gebruik van mogelijkheden, zoals RBAC mogelijk meer bevoegdheden beschikt dan nodig is voor hun gebruikers geven. Dit kan leiden tot inbreuk op gegevens doordat sommige gebruikers toegang tot gegevens die ze in eerste instantie mag geen.

Zie voor meer informatie over RBAC:

- [Op rollen gebaseerde toegangsbeheer van Azure](../active-directory/role-based-access-control-configure.md)
- [Ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure](../active-directory/role-based-access-built-in-roles.md)
- [Azure Storage-beveiligingshandleiding](../storage/common/storage-security-guide.md) voor details over het beveiligen van uw opslagaccount met RBAC

## <a name="storage-encryption"></a>Storage-versleuteling
### <a name="use-client-side-encryption-for-high-value-data"></a>Client-side '-versleuteling gebruiken voor waardevolle gegevens

Versleuteling aan clientzijde kunt u programmatisch gegevens onderweg voordat u uploadt naar Azure Storage te versleutelen en ontsleutelen van gegevens via een programma bij het ophalen van het uit de opslag.  Dit biedt versleuteling van gegevens die worden verzonden, maar het biedt ook versleuteling van gegevens in rust.  Versleuteling aan clientzijde is de veiligste methode van het versleutelen van uw gegevens, maar hoeft u programmatische wijzigingen aanbrengen in uw toepassing en processen voor sleutelbeheer plaatsen in plaats.

Versleuteling aan clientzijde ook kunt u enige controle over uw versleutelingssleutels.  U kunt genereren en beheren van uw eigen versleutelingssleutels.  Versleuteling aan clientzijde gebruikt een techniek envelop waarin de Azure storage-clientbibliotheek genereert een inhoud versleutelingssleutel (CEK) die wordt vervolgens verpakt (versleuteld) met behulp van de belangrijkste versleutelingssleutel (KEK-sleutel). De KEK wordt aangeduid met een sleutel-id en kan een asymmetrisch sleutelpaar of een symmetrische sleutel en kunnen worden beheerd lokaal of opgeslagen [Azure Key Vault](../key-vault/key-vault-whatis.md).

Versleuteling aan clientzijde is ingebouwd in de Java en de opslagclientbibliotheken voor .NET.  Zie [Client-Side-versleuteling en Azure Key Vault voor Microsoft Azure Storage](../storage/storage-client-side-encryption.md) voor informatie over het coderen van gegevens binnen clienttoepassingen en genereren en beheren van uw eigen versleutelingssleutels.

### <a name="azure-disk-encryption-for-vms"></a>Azure Disk Encryption voor virtuele machines
Azure Disk Encryption is een functie waarmee u uw Windows- en Linux IaaS-schijven voor virtuele machine versleutelen. Azure Disk Encryption maakt gebruik van de branche standaard BitLocker-functie van Windows en de functie DM-Crypt van Linux voor volumeversleuteling voor het besturingssysteem en de gegevensschijven. De oplossing is geïntegreerd met Azure Key Vault om te controleren en beheren van de schijfversleuteling sleutels en geheimen in uw abonnement sleutelkluis. De oplossing zorgt er ook voor dat alle gegevens op de virtuele machine-schijven zijn versleuteld in rust in uw Azure-opslag.

Zie [Azure Disk Encryption for Windows and Linux IaaS VM's](azure-security-disk-encryption.md).

### <a name="storage-service-encryption"></a>Storage Service-versleuteling
Wanneer [Service Opslagversleuteling](../storage/storage-service-encryption.md) voor File storage is ingeschakeld, worden de gegevens versleuteld automatisch met AES-256-versleuteling. Microsoft verwerkt de versleuteling, ontsleuteling en sleutelbeheer. Deze functie is beschikbaar voor LRS- en GRS redundantie typen.

## <a name="next-steps"></a>Volgende stappen
In dit artikel is geïntroduceerd aan een verzameling van Azure Storage aanbevolen beveiligingsprocedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen. Zie voor meer informatie over het beveiligen van uw PaaS-implementaties:

- [PaaS-implementaties beveiligen](security-paas-deployments.md)
- [PaaS-webtoepassingen en mobiele toepassingen met Azure App Services beveiligen](security-paas-applications-using-app-services.md)
- [PaaS-databases in Azure beveiligen](security-paas-applications-using-sql.md)
