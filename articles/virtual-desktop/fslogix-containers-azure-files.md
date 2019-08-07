---
title: FSLogix-profiel containers en-Azure Files in Windows virtueel bureau blad-Azure
description: In dit artikel worden FSLogix-profiel containers in virtueel bureau blad van Windows en Azure files beschreven.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: helohr
ms.openlocfilehash: c01e138c8afcdd59fcb0c87f189d98bec10e16d7
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68828135"
---
# <a name="fslogix-profile-containers-and-azure-files"></a>FSLogix-profielcontainers en Azure Files

De Windows-preview-service voor virtuele Bureau bladen beveelt FSLogix-profiel containers aan als een gebruikers profiel oplossing. FSLogix is ontworpen om profielen te roamen in omgevingen met externe computers, zoals Windows virtueel bureau blad. Er wordt een volledig gebruikers profiel opgeslagen in één container. Bij het aanmelden wordt deze container dynamisch gekoppeld aan de computer omgeving met behulp van systeem eigen ondersteunde virtuele harde schijven (VHD) en Hyper-V virtuele harde schijf (VHDX). Het gebruikers profiel is onmiddellijk beschikbaar en wordt in het systeem weer gegeven op dezelfde manier als een systeem eigen gebruikers profiel.

In dit artikel worden FSLogix-profiel containers beschreven die worden gebruikt met Azure Files. De informatie bevindt zich in de context van het virtuele bureau blad van Windows, dat werd [aangekondigd op 3/21](https://www.microsoft.com/microsoft-365/blog/2019/03/21/windows-virtual-desktop-public-preview/).

## <a name="user-profiles"></a>Gebruikersprofielen

Een gebruikers profiel bevat gegevens elementen over een individu, inclusief configuratie gegevens zoals bureaublad instellingen, permanente netwerk verbindingen en toepassings instellingen. Windows maakt standaard een lokaal gebruikers profiel dat nauw geïntegreerd is met het besturings systeem.

Een profiel voor externe gebruikers biedt een partitie tussen gebruikers gegevens en het besturings systeem. Hiermee kan het besturings systeem worden vervangen of gewijzigd zonder dat dit van invloed is op de gebruikers gegevens. In Extern bureaublad Session Host (RDSH) en Virtual Desktop Infrastructure (VDI) kan het besturings systeem om de volgende redenen worden vervangen:

- Een upgrade van het besturings systeem
- Een bestaande virtuele machine (VM) vervangen
- Een gebruiker die deel uitmaakt van een gegroepeerde (niet-permanente) RDSH-of VDI-omgeving

Micro soft-producten kunnen worden gebruikt voor verschillende technologieën voor externe gebruikers profielen, waaronder deze technologieën:
- Zwervende gebruikers profielen (ZWERVENDE gebruikers profielen)
- Gebruikers profiel schijven (UDP)
- Enter prise State roaming (ESR)

UPD en ZWERVENDE gebruikers profielen zijn de meest gebruikte technologieën voor gebruikers profielen in de omgevingen met Extern bureaublad sessiehost en virtuele harde schijven (VHD).

### <a name="challenges-with-previous-user-profile-technologies"></a>Uitdagingen met eerdere gebruikers profiel technologieën

Bestaande en verouderde micro soft-oplossingen voor gebruikers profielen zijn met diverse uitdagingen gearriveerd. Geen van de voor gaande oplossingen heeft alle gebruikers profielen afgehandeld die bij een RDSH-of VDI-omgeving worden geleverd. Bijvoorbeeld, UPD kan geen grote OST-bestanden verwerken en ZWERVENDE gebruikers profielen de moderne instellingen niet behouden.

#### <a name="functionality"></a>Functionaliteit

In de volgende tabel worden de voor delen en beperkingen van eerdere gebruikers profiel technologieën weer gegeven.

| Technologie | Moderne instellingen | Win32-instellingen | BESTURINGSSYSTEEM instellingen | Gebruikersgegevens | Ondersteund op de server-SKU | Back-end-opslag in azure | On-premises back-end-opslag | Versie ondersteuning | Volgende aanmeldings tijd |Opmerkingen|
| ---------- | :-------------: | :------------: | :---------: | --------: | :---------------------: | :-----------------------: | :--------------------------: | :-------------: | :---------------------: |-----|
| **Gebruikers profiel schijven (UDP)** | Ja | Ja | Ja | Ja | Ja | Nee | Ja | Win 7+ | Ja | |
| **Zwervend gebruikers profiel (ZWERVENDE gebruikers profielen), onderhouds modus** | Nee | Ja | Ja | Ja | Ja| Nee | Ja | Win 7+ | Nee | |
| **Enterprise State Roaming (ESR)** | Ja | Nee | Ja | Nee | Opmerkingen weer geven | Ja | Nee | Win 10 | Nee | Functies op de server-SKU, maar geen ondersteunende gebruikers interface |
| **User Experience Virtualization (UE-V)** | Ja | Ja | Ja | Nee | Ja | Nee | Ja | Win 7+ | Nee |  |
| **OneDrive-Cloud bestanden** | Nee | Nee | Nee | Ja | Opmerkingen weer geven | Opmerkingen weer geven  | Opmerkingen weer geven | Win 10 RS3 | Nee | Niet getest op server-SKU. Back-end-opslag in Azure is afhankelijk van de synchronisatieclient. On-premises back-end-opslag moet een synchronisatieclient hebben. |

#### <a name="performance"></a>Prestaties

UPD vereist [opslagruimten direct (S2D)](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment) om prestatie vereisten te verhelpen. UPD maakt gebruik van het SMB-protocol (Server Message Block). Het profiel wordt gekopieerd naar de virtuele machine waarin de gebruiker wordt geregistreerd. UPD met S2D is de oplossing die het RDS-team adviseert voor Windows Virtual Desktop tijdens de preview-versie van de service.  

#### <a name="cost"></a>Kosten

Terwijl S2D-clusters de benodigde prestaties behaalden, zijn de kosten duur voor zakelijke klanten, maar met name kostbaar voor klanten met een kleine en middel grote onderneming (SMB). Voor deze oplossing betalen bedrijven voor opslag schijven, samen met de kosten van de virtuele machines die gebruikmaken van de schijven voor een share.

#### <a name="administrative-overhead"></a>Administratieve overhead

S2D-clusters vereisen een besturings systeem dat met een patch in een beveiligde status wordt bijgewerkt en onderhouden. Deze processen en de complexiteit van het instellen van S2D-nood herstel, maken S2D alleen voor ondernemingen met een speciaal IT-personeel.

## <a name="fslogix-profile-containers"></a>FSLogix-profiel containers

Op 19 november 2018 heeft [micro soft FSLogix aangeschaft](https://blogs.microsoft.com/blog/2018/11/19/microsoft-acquires-fslogix-to-enhance-the-office-365-virtualization-experience/). FSLogix is een oplossing voor veel uitdagingen met een profiel container. De sleutel is onder andere:

- **Nemen** De [FSLogix-profiel containers](https://fslogix.com/products/profile-containers) zijn hoge prestaties en kunnen prestatie problemen oplossen die de Exchange-modus met een historische cache hebben geblokkeerd.
- **OneDrive:** Zonder FSLogix-profiel containers wordt OneDrive voor bedrijven niet ondersteund in niet-permanente RDSH-of VDI-omgevingen. In de [Best practices voor OneDrive voor bedrijven en FSLogix](https://fslogix.com/products/technical-faqs/284-onedrive-for-business-and-fslogix-best-practices) wordt beschreven hoe ze communiceren. Zie [de Synchronisatieclient gebruiken op virtuele Bureau bladen](https://docs.microsoft.com/deployoffice/rds-onedrive-business-vdi)voor meer informatie.
- **Aanvullende mappen:** FSLogix biedt de mogelijkheid om gebruikers profielen uit te breiden voor het toevoegen van extra mappen.

Sinds de aanschaf is micro soft begonnen met het vervangen van bestaande gebruikers profiel oplossingen, zoals UPD, met FSLogix-profiel containers.

## <a name="azure-files-integration-with-azure-active-directory"></a>Integratie met Azure Active Directory Azure Files

De prestaties en functies van het FSLogix-profiel containers profiteren van de Cloud. Op Sept. 24 2018 Microsoft Azure bestanden een open bare preview van [Azure files ondersteunen Azure Active Directory verificatie](https://azure.microsoft.com/blog/azure-active-directory-integration-for-smb-access-now-in-public-preview/). Door zowel kosten als administratieve overhead te adresseren, wordt Azure Files met Azure Active Directory-verificatie een Premium-oplossing voor gebruikers profielen in de nieuwe virtueel bureau blad-service van Windows.

## <a name="best-practices-for-windows-virtual-desktop"></a>Aanbevolen procedures voor virtueel bureau blad van Windows

Virtueel bureau blad van Windows biedt volledige controle over de grootte, het type en het aantal Vm's dat door klanten wordt gebruikt. Zie [Wat is Windows virtueel bureau blad-preview?](https://docs.microsoft.com/azure/virtual-desktop/overview)voor meer informatie.

Om ervoor te zorgen dat uw virtuele Windows-bureau blad-omgeving de best practices volgt:

- Azure Files Storage-account moet zich in dezelfde regio bevinden als de virtuele machines van de host.
- Azure Files machtigingen moeten overeenkomen met de machtigingen die zijn beschreven in [vereisten-profiel containers](https://docs.fslogix.com/display/20170529/Requirements+-+Profile+Containers).
- Elke hostgroep moet zijn gebaseerd op hetzelfde type en dezelfde grootte VM op basis van dezelfde hoofd installatie kopie.
- Elke VM van een hostgroep moet zich in dezelfde resource groep beheersen voor het beheren, schalen en bijwerken.
- Voor optimale prestaties moeten de opslag oplossing en de FSLogix-profiel container zich op dezelfde locatie in het Data Center bevinden.
- Het opslag account met de master installatie kopie moet zich in dezelfde regio en hetzelfde abonnement bevinden als de virtuele machines die worden ingericht.

## <a name="next-steps"></a>Volgende stappen

Gebruik de volgende instructies om een virtuele Windows-desktop omgeving in te stellen.

- Zie [een Tenant maken in Windows virtueel bureau blad](https://docs.microsoft.com/azure/virtual-desktop/tenant-setup-azure-active-directory)om te beginnen met het bouwen van de oplossing voor desktop virtualisatie.
- Zie [een hostgroep maken met Azure Marketplace](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace)voor meer informatie over het maken van een hostgroep in uw virtuele Windows-bureau blad-Tenant.
- Zie [Azure Files share instellen](https://docs.microsoft.com/azure/storage/files/storage-files-active-directory-enable)voor meer informatie over het instellen van volledig beheerde bestands shares in de Cloud.
- Zie [een gebruikers profiel share instellen voor een hostgroep voor](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-user-profile)meer informatie over het configureren van FSLogix-profiel containers.
- Zie [app-groepen beheren voor Windows virtueel bureau blad](https://docs.microsoft.com/azure/virtual-desktop/manage-app-groups)om gebruikers toe te wijzen aan een hostgroep.
- Zie [verbinding maken met het virtuele bureau blad van Windows](https://docs.microsoft.com/azure/virtual-desktop/connect-web)voor toegang tot uw virtuele bureau blad-resources via een webbrowser.
