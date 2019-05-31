---
title: FSLogix profiel containers en Azure-bestanden in Windows virtueel bureaublad - Azure
description: Dit artikel wordt beschreven FSLogix profiel containers binnen virtuele Windows-bureaublad en Azure-bestanden.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: v-chjenk
ms.openlocfilehash: c3f31e8d260ea5e462e8782fadd9f61f34d03add
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66307270"
---
# <a name="fslogix-profile-containers-and-azure-files"></a>FSLogix profiel containers en Azure files

De service Windows Virtual Desktop Preview raadt FSLogix profiel containers als een oplossing voor het profiel van gebruiker. FSLogix is ontworpen om u te laten zwerven-profielen in externe computeromgevingen, zoals virtuele Windows-bureaublad. Het profiel voor een volledige gebruiker worden opgeslagen in een enkele container. Bij het aanmelden, is de container dynamisch gekoppeld aan de computeromgeving systeemeigen, in de Gast virtuele hardeschijf (VHD) en Hyper-V virtuele harde schijf (VHDX) met services van Microsoft. Het gebruikersprofiel is onmiddellijk beschikbaar en wordt weergegeven in het systeem exact zo als een systeemeigen gebruikersprofiel.

In dit artikel wordt beschreven FSLogix profiel containers gebruikt in combinatie met Azure Files. De informatie is in de context van Windows virtuele bureaublad, die was [aangekondigd op 3/21](https://www.microsoft.com/microsoft-365/blog/2019/03/21/windows-virtual-desktop-public-preview/).

## <a name="user-profiles"></a>Gebruikersprofielen

Een gebruikersprofiel bevat gegevenselementen die over een persoon, met inbegrip van configuratie-informatie zoals bureaubladinstellingen, permanente netwerkverbindingen en toepassingsinstellingen. Windows maakt standaard een lokaal gebruikersprofiel dat is nauw geïntegreerd met het besturingssysteem.

Een extern gebruikersprofiel biedt een partitie tussen gebruikersgegevens en het besturingssysteem. Hierdoor kan het besturingssysteem moet worden vervangen of gewijzigd zonder negatieve gevolgen voor de gebruikersgegevens. In het Remote Desktop Session Host (RDSH) en virtuele bureaublad infrastructuur (VDI), worden het besturingssysteem vervangen om de volgende redenen:

- Een upgrade van het besturingssysteem
- Vervanging van een bestaande virtuele Machine (VM)
- Een gebruiker die deelneemt aan een gegroepeerde (niet-permanente) RDSH of VDI-omgeving

Microsoft-producten werken met verschillende technologieën voor externe gebruikersprofielen, met inbegrip van deze technologieën:
- Zwervende gebruikersprofielen (zwervende Gebruikersprofielen)
- Gebruikersprofielschijven (UPD)
- Enterprise state roaming (ESR)

UPD en zwervende Gebruikersprofielen vindt u de meest gebruikte technologieën voor gebruikersprofielen in omgevingen met Remote Desktop Session Host (RDSH) en virtuele hardeschijf (VHD).

### <a name="challenges-with-previous-user-profile-technologies"></a>Problemen met technologieën van vorige gebruiker profiel

Bestaande en oudere Microsoft-oplossingen voor gebruikersprofielen afkomstig zijn verschillende uitdagingen met zich mee. Er is geen vorige oplossing verwerkt alle de gebruiker profiel nodig heeft die worden geleverd met een RDSH of VDI-omgeving. Bijvoorbeeld, UPD grote OST-bestanden kan niet worden verwerkt en zwervende Gebruikersprofielen gaat verloren moderne instellingen.

#### <a name="functionality"></a>Functionaliteit

De volgende tabel bevat de voordelen en beperkingen van de vorige gebruiker profiel technologieën.

| Technologie | Moderne instellingen | Win32-instellingen | Instellingen van besturingssysteem | Gebruikersgegevens | Ondersteund op SKU-server | Back-end-opslag in Azure | Back-end opslag on-premises | Ondersteuning voor versie | Voor volgende aanmeldingen in de tijd |Opmerkingen|
| ---------- | :-------------: | :------------: | :---------: | --------: | :---------------------: | :-----------------------: | :--------------------------: | :-------------: | :---------------------: |-----|
| **Gebruikersprofielschijven (UPD)** | Ja | Ja | Ja | Ja | Ja | Nee | Ja | Win 7+ | Ja | |
| **Zwervende gebruikers profiel (RUP), in de onderhoudsmodus** | Nee | Ja | Ja | Ja | Ja| Nee | Ja | Win 7+ | Nee | |
| **Enterprise State Roaming (ESR)** | Ja | Nee | Ja | Nee | Zie Opmerkingen bij de | Ja | Nee | Win 10 | Nee | Functies worden uitgevoerd op server SKU, maar geen ondersteunende gebruikersinterface |
| **Gebruikerservaring virtualisatie (UE-V)** | Ja | Ja | Ja | Nee | Ja | Nee | Ja | Win 7+ | Nee |  |
| **OneDrive-bestanden voor cloud** | Nee | Nee | Nee | Ja | Zie Opmerkingen bij de | Zie Opmerkingen bij de  | Zie Opmerkingen bij de | Win 10 RS3 | Nee | Niet getest op server SKU. Back-end-opslag in Azure, is afhankelijk van sync-client. Back-end-opslag on-premises moet een sync-client. |

#### <a name="performance"></a>Prestaties

UPD vereist [Storage Spaces Direct (S2D)](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment) voor prestatievereisten. UPD maakt gebruik van Server Message Block (SMB)-protocol. Het profiel worden gekopieerd naar de virtuele machine waarbij de gebruiker is geregistreerd. UPD met S2D is de oplossing die het team extern bureaublad-services wordt aanbevolen voor virtuele Windows-bureaublad tijdens de Preview-versie van de service.  

#### <a name="cost"></a>Kosten

Terwijl het S2D-clusters bereiken van de prestaties die nodig zijn, worden de kosten zijn duur voor zakelijke klanten, maar vooral dure voor kleine en middelgrote bedrijven (SMB)-klanten. Voor deze oplossing bedrijven betalen voor premium storage-schijven, samen met de kosten van de virtuele machines die gebruikmaken van de schijven voor een share.

#### <a name="administrative-overhead"></a>Administratieve overhead

S2D-clusters vereist een besturingssysteem dat is gevuld, bijgewerkt en onderhouden in een beveiligde status. Deze processen en de complexiteit van het instellen van S2D-noodherstel kunt S2D haalbaar is alleen voor ondernemingen met een toegewezen IT-personeel.

## <a name="fslogix-profile-containers"></a>FSLogix profiel containers

19 November 2018 [Microsoft verkregen FSLogix](https://blogs.microsoft.com/blog/2018/11/19/microsoft-acquires-fslogix-to-enhance-the-office-365-virtualization-experience/). FSLogix adressen veel profiel container uitdagingen, sleutel daartussen zijn:

- **Prestaties:** De [FSLogix profiel containers](https://fslogix.com/products/profile-containers) zijn hoge prestaties en oplossen van prestatieproblemen die in het verleden geblokkeerd in de cache opgeslagen exchange-modus.
- **OneDrive:** Zonder FSLogix profiel containers, wordt OneDrive voor bedrijven niet ondersteund in niet-permanente RDSH of VDI-omgevingen. [OneDrive voor bedrijven en FSLogix aanbevolen procedures](https://fslogix.com/products/technical-faqs/284-onedrive-for-business-and-fslogix-best-practices) wordt beschreven hoe ze werken. Zie voor meer informatie, [de synchronisatieclient gebruiken op virtuele bureaubladen](https://docs.microsoft.com/deployoffice/rds-onedrive-business-vdi).
- **Extra mappen:** FSLogix biedt de mogelijkheid om uit te breiden gebruikersprofielen om op te nemen van extra mappen.

Microsoft sinds het ophalen, aan de slag bestaande gebruiker profiel oplossingen, zoals UPD, vervangen door FSLogix profiel containers.

## <a name="azure-files-integration-with-azure-active-directory"></a>Integratie van Azure bestanden met Azure Active Directory

Functies voor FSLogix profiel containers prestaties en profiteer van de cloud. Op 24 september 2018, Microsoft Azure Files een openbare preview van aangekondigd [Azure Files ondersteunt Azure Active Directory-verificatie](https://azure.microsoft.com/blog/azure-active-directory-integration-for-smb-access-now-in-public-preview/). Azure Files met Azure Active Directory-verificatie is door zowel de kosten en de administratieve overhead, een premium-oplossing voor gebruikersprofielen in de nieuwe virtuele Windows-bureaublad-service.

## <a name="best-practices-for-windows-virtual-desktop"></a>Aanbevolen procedures voor virtuele Windows-bureaublad

Virtuele Windows-bureaublad biedt volledige controle over de grootte, soort en aantal VM's die worden gebruikt door klanten. Zie voor meer informatie, [wat is Windows Virtual Desktop Preview?](https://docs.microsoft.com/azure/virtual-desktop/overview).

Om ervoor te zorgen van uw virtuele Windows-bureaublad omgeving met de volgende aanbevolen procedures:

- Azure Files storage-account moet zich in dezelfde regio als de host-VM's van de sessie.
- Azure-bestandsmachtigingen moeten overeenkomen met de machtigingen die worden beschreven in [vereisten - profiel Containers](https://docs.fslogix.com/display/20170529/Requirements+-+Profile+Containers).
- Elke groep host moet van hetzelfde type en grootte van virtuele machine op basis van de dezelfde hoofdinstallatiekopie worden gebouwd.
- Elke groep host virtuele machine moet zich in dezelfde resourcegroep bevinden om te helpen beheer, schalen en bijwerken.
- Voor optimale prestaties moet de oplossing en de profiel-container moet zich in dezelfde gegevens FSLogix locatie voor het datacenter.
- Het opslagaccount met de master-image moet zich in dezelfde regio en hetzelfde abonnement waar de virtuele machines worden ingericht.

## <a name="next-steps"></a>Volgende stappen

Gebruik de volgende instructies voor het instellen van een virtuele Windows-bureaublad-omgeving.

- Zie het starten van het bouwen van uw oplossing bureaubladvirtualisatie [een tenant maken in een virtuele Windows-bureaublad](https://docs.microsoft.com/azure/virtual-desktop/tenant-setup-azure-active-directory).
- Zie voor het maken van een groep host binnen uw tenant virtuele Windows-bureaublad, [een host-pool maken met Azure Marketplace](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace).
- Voor het instellen van volledig beheerde bestandsshares in de cloud, Zie [instellen van Azure Files-share](https://docs.microsoft.com/azure/storage/files/storage-files-active-directory-enable).
- Zie configureren van FSLogix profiel containers [instellen van een share van de gebruiker profiel voor een groep host](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-user-profile).
- Als u wilt gebruikers toewijzen aan een host-groep, Zie [app-groepen beheren voor virtuele Windows-bureaublad](https://docs.microsoft.com/azure/virtual-desktop/manage-app-groups).
- Voor toegang tot uw virtuele Windows-bureaublad-resources via een webbrowser, Zie [verbinding maken met virtuele Windows-bureaublad](https://docs.microsoft.com/azure/virtual-desktop/connect-web).
