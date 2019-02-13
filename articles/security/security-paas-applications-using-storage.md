---
title: PaaS-toepassingen beveiligen met Azure Storage | Microsoft Docs
description: Meer informatie over de beveiliging voor Azure Storage aanbevolen procedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen.
services: security
documentationcenter: na
author: TomShinder
manager: barbkess
editor: ''
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2018
ms.author: TomShinder
ms.openlocfilehash: 3ad97c7adb5901c1da1d174d12d5d6a91831cc74
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56108922"
---
# <a name="best-practices-for-securing-paas-web-and-mobile-applications-using-azure-storage"></a>Aanbevolen procedures voor het beveiligen van PaaS-webtoepassingen en mobiele toepassingen met behulp van Azure Storage
In dit artikel wordt besproken hoe een verzameling van Azure Storage aanbevolen beveiligingsprocedures voor het beveiligen van uw platform-as-a-service (PaaS) web en mobiele toepassingen. Deze aanbevolen procedures zijn afgeleid van onze ervaring met Azure en de ervaringen van klanten zoals zelf.

Azure maakt het mogelijk om te implementeren en gebruiken van opslag op een manier niet eenvoudig haalbare on-premises. Met Azure storage, kunt u de hoge mate van schaalbaarheid en beschikbaarheid met relatief veel moeite bereiken. Azure Storage niet alleen vormt de basis voor Windows en Linux Azure Virtual Machines, kan deze ook grote gedistribueerde toepassingen ondersteunen.

Azure Storage biedt de volgende vier services: BLOB storage, Table storage, Queue storage en File storage. Zie voor meer informatie, [Inleiding tot Microsoft Azure Storage](../storage/storage-introduction.md).

De [Azure Storage-beveiligingshandleiding](../storage/common/storage-security-guide.md) is een fantastische bron voor gedetailleerde informatie over Azure Storage en beveiliging. Aanbevolen procedures voor deze adressen op hoog niveau enkele concepten gevonden in de handleiding voor beveiliging en koppelingen naar de beveiligingshandleiding, evenals andere bronnen, voor meer informatie.

In dit artikel komen de volgende aanbevolen procedures:

- Shared Access Signatures (SAS)
- Toegangsbeheer op basis van rollen (RBAC)
- Versleuteling aan de clientzijde voor hoge waarde-gegevens
- Storage Service-versleuteling


## <a name="use-a-shared-access-signature-instead-of-a-storage-account-key"></a>Een shared access signature gebruiken in plaats van een opslagaccountsleutel
Toegangsbeheer is essentieel. Toegang tot Azure Storage, Azure genereert twee 512-bits sleutels van opslagaccount (SAKs) om u te helpen bij het maken van een storage-account. Het niveau van de belangrijkste redundantie maakt het mogelijk voor u om serviceonderbrekingen te voorkomen tijdens routinematige rouleren van de sleutel. 

Toegangssleutels voor opslag zijn essentiële geheimen en moeten alleen toegankelijk is voor die verantwoordelijk is voor toegangsbeheer voor opslag. Als de juiste personen toegang tot deze sleutels krijgen, kan ze hebben volledige controle over opslag en vervangen, verwijderen, of bestanden toevoegen aan opslag. Dit omvat malware en andere typen inhoud die u mogelijk in gevaar van uw organisatie of voor uw klanten brengen kunt.

U moet wel een manier om toegang tot objecten in de opslag te bieden. Voor gedetailleerdere toegang kunt u profiteren van de handtekening voor gedeelde toegang (SAS). De SAS maakt het mogelijk dat u specifieke objecten in de opslag voor een vooraf gedefinieerde periode en met specifieke machtigingen delen. Een shared access signature kunt u definiëren:

- Het interval op waarover de SAS geldig is, met inbegrip van de begin- en het verlooptijdstip is.
- De machtigingen verleend door de SAS. Bijvoorbeeld kan een SAS voor een blob verlenen van een gebruiker lezen en schrijfmachtigingen heeft tot de blob, maar de machtigingen niet verwijderen.
- Een optionele IP-adres of het bereik van IP-adressen van waaruit de Azure Storage de SAS accepteert. Bijvoorbeeld, kunt u een bereik van IP-adressen die horen bij uw organisatie opgeven. Dit biedt een andere mate van beveiliging voor uw SAS.
- Het protocol waarvoor Azure Storage de SAS accepteert. U kunt deze optionele parameter gebruiken om toegang te beperken met clients met behulp van HTTPS.

SAS kunt u de manier waarop u wilt delen zonder uw storage-accountsleutels weggeven van inhoud delen. Altijd met behulp van SAS in uw toepassing is een veilige manier voor het delen van uw opslagresources zonder verlies van de sleutels van uw storage-account.

Zie voor meer informatie over de handtekening voor gedeelde toegang, [voor gedeelde toegangshandtekeningen](../storage/common/storage-dotnet-shared-access-signature-part-1.md). 

## <a name="use-role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer gebruiken
Een andere manier om toegang te beheren, is met [op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md) (RBAC). Met RBAC u richten op uw werknemers de exacte machtigingen die ze nodig hebben, op basis van de noodzaak om te weten en minimale bevoegdheden beveiligingsprincipes. Te veel machtigingen kunnen een account voor aanvallen worden blootgesteld. Te weinig machtigingen betekent dat werknemers hun werk efficiënter kunnen niet ophalen. Met RBAC kunt dit probleem oplossen door het aanbieden van verfijnd toegangsbeheer voor Azure. Dit is van cruciaal belang voor organisaties die willen beveiligingsbeleid voor toegang tot gegevens afdwingen.

U kunt ingebouwde RBAC-rollen in Azure machtigingen toewijzen aan gebruikers. Inzender voor Opslagaccounts bijvoorbeeld gebruiken voor cloudoperators die nodig hebt voor het beheren van storage-accounts en de rol Inzender voor klassieke Storage-Account voor het beheren van klassieke opslagaccounts. Voor cloudoperators die nodig hebt voor het beheren van virtuele machines, maar niet het virtuele netwerk of opslagaccount waarmee ze zijn verbonden, kunt u ze toevoegen aan de rol Inzender voor virtuele machines.

Organisaties die gegevenstoegangsbeheer niet afdwingen met behulp van mogelijkheden zoals RBAC mogelijk meer bevoegdheden dan nodig is voor hun gebruikers geven. Dit kan leiden tot inbreuk op gegevens door toe te staan van sommige gebruikers toegang tot gegevens die ze in de eerste plaats al dan niet mogen hebben.

Zie voor meer informatie over RBAC:

- [Toegang beheren met op rollen gebaseerd toegangsbeheer en Azure Portal](../role-based-access-control/role-assignments-portal.md)
- [Ingebouwde rollen voor Azure-resources](../role-based-access-control/built-in-roles.md)
- [Veiligheidsgids voor Azure Storage](../storage/common/storage-security-guide.md) 

## <a name="use-client-side-encryption-for-high-value-data"></a>Client-side-versleuteling gebruiken voor hoge waarde-gegevens
Client-side encryption kunt u de gegevens die onderweg zijn programmatisch te versleutelen voordat u uploadt naar Azure Storage en ontsleutelen van gegevens via een programma bij het ophalen van het. Dit biedt versleuteling van gegevens die onderweg zijn, maar het biedt ook versleuteling van data-at-rest. Clientversleuteling is de veiligste methode van het versleutelen van uw gegevens, maar het vereist dat u programmatische wijzigingen aanbrengen in uw toepassing en sleutelbeheer processen opstellen.

Clientversleuteling kunt u hebt enige controle over uw versleutelingssleutels. U kunt genereren en beheren van uw eigen versleutelingssleutels. Het maakt gebruik van een techniek envelop waar de Azure storage-clientbibliotheek genereert een versleuteling van inhoud sleutel (CEK) dat vervolgens is verpakt (versleuteld) met behulp van de sleutel van versleutelingssleutel (KEK-sleutel). De KEK wordt aangeduid met een sleutel-id en kunt een asymmetrisch sleutelpaar of een symmetrische sleutel en kan worden lokaal beheerd of die zijn opgeslagen in [Azure Key Vault](../key-vault/key-vault-whatis.md).

Clientversleuteling is ingebouwd in de Java en de opslagclientbibliotheken voor .NET. Zie [Client-side-versleuteling en Microsoft Azure Storage in Azure Key Vault](../storage/storage-client-side-encryption.md) voor meer informatie over versleuteling van gegevens binnen clienttoepassingen en genereren en beheren van uw eigen versleutelingssleutels.

## <a name="enable-storage-service-encryption-for-data-at-rest"></a>Inschakelen van Storage-Serviceversleuteling voor data-at-rest
Wanneer [Storage Service Encryption](../storage/storage-service-encryption.md) voor File storage is ingeschakeld, worden de gegevens versleuteld automatisch met behulp van AES-256-codering. Microsoft verwerkt alle versleuteling, ontsleuteling en sleutelbeheer. Deze functie is beschikbaar voor LRS- als GRS redundantietypen.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u kennisgemaakt met een verzameling van Azure Storage aanbevolen beveiligingsprocedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen. Zie voor meer informatie over het beveiligen van uw PaaS-implementaties:

- [PaaS-implementaties beveiligen](security-paas-deployments.md)
- [Beveiliging van de PaaS-webtoepassingen en mobiele toepassingen met behulp van Azure App Services](security-paas-applications-using-app-services.md)
- [PaaS-databases in Azure beveiligen](security-paas-applications-using-sql.md)
