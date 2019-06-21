---
title: Overzicht van Azure Active Directory-verificatie voor Azure Files (preview) - Azure-opslag via SMB
description: Azure Files biedt ondersteuning voor verificatie op basis van identiteit via SMB (Server Message Block) (preview) via Azure Active Directory (Azure AD) Domain Services. Uw domein Windows virtuele machines (VM's) hebben vervolgens toegang tot Azure-bestandsshares met behulp van Azure AD-referenties.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 06/18/2019
ms.author: rogarana
ms.openlocfilehash: 21087424be1a7a3edfe2dddcbec830bd74559b23
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67269349"
---
# <a name="overview-of-azure-files-azure-active-directory-domain-service-aad-ds-authentication-support-for-smb-access-preview"></a>Overzicht van Azure Files Azure Active Directory Domain Services (DS AAD)-verificatieondersteuning voor SMB-toegang (Preview)
[!INCLUDE [storage-files-aad-auth-include](../../../includes/storage-files-aad-auth-include.md)]

Zie voor meer informatie over AAD Active Directory-verificatie inschakelen voor Azure Files, [inschakelen Azure Active Directory-domein-Service-verificatie via SMB voor Azure Files (Preview)](storage-files-active-directory-enable.md).

## <a name="glossary"></a>Woordenlijst 
Is het handig om te begrijpen van enkele belangrijke termen met betrekking tot Azure AD Domain-Service-verificatie via SMB voor Azure Files:

-   **Azure Active Directory (Azure AD)**  
    Azure Active Directory (Azure AD) is van Microsoft cloud-gebaseerde directory- en identiteitsbeheer management service met meerdere tenants. Azure AD combineert belangrijke directoryservices, toegangsbeheer voor toepassingen en identiteitsbeveiliging in één oplossing. Zie voor meer informatie, [wat is Azure Active Directory?](../../active-directory/fundamentals/active-directory-whatis.md)

-   **Azure AD Domain Services**  
    Azure AD Domain Services biedt beheerde domeinservices zoals domain-join, Groepsbeleid, LDAP en Kerberos/NTLM-verificatie. Deze services zijn volledig compatibel met Windows Server Active Directory. Zie voor meer informatie, [Azure Active Directory (AD) Domain Services](../../active-directory-domain-services/overview.md).

-   **Azure Role Based Access Control (RBAC)**  
    Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over geavanceerd toegangsbeheer voor Azure. Met RBAC, kunt u toegang tot resources gebruikers het minst door machtigingen te verlenen die nodig zijn om hun werk te kunnen beheren. Zie voor meer informatie over RBAC [wat is er op rollen gebaseerd toegangsbeheer (RBAC) in Azure?](../../role-based-access-control/overview.md)

-   **Kerberos-verificatie**

    Kerberos is een authenticatieprotocol dat wordt gebruikt om te controleren of de identiteit van een gebruiker of host. Zie voor meer informatie over Kerberos [overzicht van Kerberos-authenticatie](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-authentication-overview).

-  **Server Message Block (SMB)-protocol**  
    SMB is een netwerkprotocol voor de delen van bestanden van industrienormen. SMB wordt ook wel Common Internet File System- of CIFS. Zie voor meer informatie over SMB [Microsoft SMB-Protocol en overzicht van de CIFS-Protocol](https://docs.microsoft.com/windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview).

## <a name="advantages-of-azure-ad-domain-service-authentication"></a>Voordelen van Azure AD Domain-Service-verificatie
Azure AD Domain Services-verificatie voor Azure Files biedt verschillende voordelen boven het gebruik van gedeelde sleutelverificatie:

-   **Uitbreiden van de traditionele op basis van identiteit bestand gedeelde toegang ervaring naar de cloud met Azure AD en Azure AD Domain Services**  
    Als u van plan bent te 'lift and shift' referenties die uw toepassing in de cloud, traditionele bestandsservers vervangen door Azure Files, en vervolgens kunt u uw toepassing om te verifiëren met Azure AD voor toegang tot bestandsgegevens. Azure biedt ondersteuning voor bestanden met behulp van Azure AD-referenties voor toegang tot Azure Files via SMB van AAD-DS domein-Windows VM's. U kunt ook om te synchroniseren op al uw on-premises Active Directory-objecten met Azure AD voor gebruikersnamen, wachtwoorden en andere groepstoewijzingen behouden.

-   **Gebruik nauwkeurig toegangsbeheer voor Azure-bestandsshares afdwingen**  
    U kunt machtigingen voor een specifieke identiteit in de share, map of bestandsniveau toekennen. Stel bijvoorbeeld dat u hebt verschillende teams met behulp van een enkel Azure-bestandsshare voor projectsamenwerking. U kunt alle teams toegang verlenen tot niet-gevoelige mappen, terwijl het beperken van toegang tot mappen met gevoelige financiële gegevens naar uw team van Financiën alleen. 

-   **Back-up van ACL's samen met uw gegevens**  
    Azure Files kunt u back-up van uw bestaande on-premises bestandsshares. Azure Files bewaart de ACL's samen met uw gegevens wanneer u maakt u een back-up van een bestand deelt naar Azure Files via SMB.

## <a name="how-it-works"></a>Hoe werkt het?
Azure Files maakt gebruik van Azure AD Domain Services voor de ondersteuning van Kerberos-verificatie met Azure AD-referenties van een domein VM's. Voordat u Azure AD met Azure Files gebruiken kunt, moet u Azure AD Domain Services inschakelen en toegevoegd aan het domein van de virtuele machines van waaruit u plant voor toegang tot gegevens uit een bestand. Uw domein-virtuele machine moet zich bevinden in hetzelfde virtuele netwerk (VNET) als Azure AD Domain Services. 

Wanneer een identiteit die is gekoppeld aan een toepassing die wordt uitgevoerd op een virtuele machine probeert te krijgen tot gegevens in Azure Files, wordt de aanvraag verzonden naar Azure AD Domain Services de identiteit verifiëren. Als verificatie is geslaagd, wordt in Azure AD Domain Services een Kerberos-token retourneert. De toepassing verzendt een aanvraag met het Kerberos-token en Azure Files maakt gebruik van dit token te autoriseren van de aanvraag. Azure Files ontvangt alleen het token en Azure AD-referenties niet bewaard is gebleven.

![Schermopname die laat zien diagram van Azure AD-verificatie via SMB](media/storage-files-active-directory-overview/azure-active-directory-over-smb-for-files-overview.png)

### <a name="enable-azure-ad-domain-service-authentication-for-smb-access"></a>Azure AD Domain Services-verificatie voor SMB-toegang inschakelen
U kunt Azure AD Domain-Service-verificatie voor Azure-bestanden op uw nieuwe en bestaande opslagaccounts die zijn gemaakt na 24 September 2018 inschakelen. 

Voordat u deze functie inschakelt, moet u controleren dat Azure AD Domain Services is geïmplementeerd voor de primaire Azure AD-tenant die aan uw storage-account gekoppeld is. Als u nog geen Azure AD Domain Services hebt ingesteld, volgt u de stapsgewijze richtlijnen van [inschakelen Azure Active Directory Domain Services met behulp van de Azure-portal](../../active-directory-domain-services/create-instance.md).

Azure AD Domain Services-implementatie duurt doorgaans 10 tot 15 minuten. Nadat Azure AD Domain Services is geïmplementeerd, kunt u Azure AD-verificatie via SMB inschakelen voor Azure Files. Zie voor meer informatie, [inschakelen Azure Active Directory Domain Services-verificatie voor Azure Files (Preview) via SMB](storage-files-active-directory-enable.md). 

### <a name="configure-share-level-permissions-for-azure-files"></a>Machtigingen voor share-niveau voor Azure Files configureren
Nadat Azure AD Domain-Service-verificatie is ingeschakeld, kunt u configureren van aangepaste RBAC-rollen voor Azure AD-identiteiten en toegangsrechten toewijzen aan alle bestandsshares in de storage-account.

Wanneer een toepassing die wordt uitgevoerd op een domein-VM probeert te koppelen van een Azure-bestandsshare of toegang tot een bestand of map, wordt van de toepassing Azure AD-referenties worden geverifieerd om te controleren of het juiste niveau van de share en NTFS-machtigingen. Zie voor meer informatie over het configureren van machtigingen voor share-niveau [inschakelen Azure Active Directory Domain Services-verificatie via SMB (Preview)](storage-files-active-directory-enable.md).

### <a name="configure-directory--or-file-level-permissions-for-azure-files"></a>Directory - of bestandsniveau machtigingen configureren voor Azure Files 
Azure Files dwingt standaard NTFS-machtigingen op het niveau mappen en bestanden, met inbegrip van de hoofdmap. Configuratie van machtigingen voor map - of bestandsniveau wordt alleen ondersteund via SMB. De doel-bestandsshare koppelen vanuit uw virtuele machine en het configureren met behulp van de Windows [icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) of [Set-ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-acl) opdracht. 

> [!NOTE]
> Configureren van NTFS-machtigingen via Windows Verkenner wordt niet ondersteund in de Preview-versie.

### <a name="use-the-storage-account-key-for-superuser-permissions"></a>Gebruik de opslagaccountsleutel voor beheerder machtigingen 
Een gebruiker die de opslagaccountsleutel kunt toegang tot Azure Files met de juiste machtigingen beschikken. Beheerder machtigingen Overtref alle toegangscontrolebeperkingen geconfigureerd op het niveau van de share met RBAC en afgedwongen door Azure AD. Beheerder machtigingen zijn vereist voor een Azure-bestandsshare koppelen. 

> [!IMPORTANT]
> Vermijd het delen van uw storage-accountsleutels als onderdeel van best practices voor beveiliging, en gebruikmaken van Azure AD-machtigingen indien mogelijk.

### <a name="preserve-directory-and-file-acls-for-data-import-to-azure-file-shares"></a>Map- en ACL's voor het importeren van gegevens naar Azure-bestandsshares behouden
Azure Files biedt nu ondersteuning voor het bestand of map ACL's te behouden wanneer u gegevens kopiëren naar Azure-bestandsshares. U kunt de ACL's in een map of bestand kopiëren naar Azure Files. Bijvoorbeeld, kunt u [robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) met vlag `/copy:s` ACL's en gegevens kopiëren naar een Azure-bestandsshare. Behoud van de ACL is standaard ingeschakeld en moet u niet expliciet inschakelen authentication-functie van Azure AD Domain Services in uw storage-account. 

## <a name="pricing"></a>Prijzen
Er zijn geen extra kosten van de service Azure AD-verificatie inschakelen via SMB op uw storage-account. Zie voor meer informatie over prijzen [Azure Files-prijzen](https://azure.microsoft.com/pricing/details/storage/files/) en [prijzen voor Azure AD Domain Services](https://azure.microsoft.com/pricing/details/active-directory-ds/) pagina's.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over Azure Files en Azure AD-verificatie via SMB, Zie de volgende bronnen:

- [Inleiding tot Azure Files](storage-files-introduction.md)
- [Azure Active Directory-verificatie inschakelen via SMB voor Azure Files (Preview)](storage-files-active-directory-enable.md)
- [Veelgestelde vragen](storage-files-faq.md)
