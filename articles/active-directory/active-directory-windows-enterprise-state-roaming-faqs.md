---
title: Instellingen en veelgestelde vragen voor dataroaming | Microsoft Docs
description: Biedt antwoorden op enkele vragen IT-beheerders wellicht over de instellingen en synchroniseren van app-gegevens.
services: active-directory
keywords: Enterprise state roaming instellingen windows cloud, veelgestelde vragen op enterprise state roaming
documentationcenter: 
author: tanning
manager: mtillman
editor: curtand
ms.assetid: c0824f5c-129b-4240-969f-921f6a64eae7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: markvi
ms.openlocfilehash: 054705e802867fda666c80217396db197c60f50e
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="settings-and-data-roaming-faq"></a>Veelgestelde vragen over instellingen en gegevensroaming
In dit onderwerp worden enkele vragen beantwoord IT-beheerders over de instellingen en synchroniseren van app-gegevens hebben kunnen.

## <a name="what-data-roams"></a>Welke gegevens roamt?
**Windows-instellingen**: de PC-instellingen die zijn ingebouwd in de Windows-besturingssysteem. Over het algemeen dit zijn instellingen die uw PC aanpassen, en bestaan uit de volgende categorieën:

* *Thema*, waaronder functies zoals bureaubladinstellingen thema en de taakbalk.
* *Instellingen van Internet Explorer*, met inbegrip van onlangs geopende tabbladen en Favorieten.
* *Edge-browserinstellingen*, zoals Favorieten en leeslijst zijn vermeld.
* *Wachtwoorden*, zoals wachtwoorden voor Internet-, Wi-Fi-profielen en anderen.
* *Taalvoorkeuren*, waaronder instellingen voor toetsenbordindeling, systeemtaal, datum en tijd en meer.
* *Gebruiksgemak toegangsfuncties*, zoals thema met hoog contrast en Narrator, Vergrootglas.
* *Andere Windows-instellingen*, zoals instellingen voor de opdrachtprompt en de lijst met toepassingen.

**Toepassingsgegevens**: universele Windows-apps instellingsgegevens naar een map voor zwervende kunnen schrijven en geen gegevens geschreven naar deze map worden automatisch gesynchroniseerd. Het is aan de ontwikkelaar van de afzonderlijke app voor het ontwerpen van een app om te profiteren van deze mogelijkheid. Zie voor meer informatie over het ontwikkelen van een universele Windows-app die gebruikmaakt van roaming de [appdata opslag API](https://msdn.microsoft.com/library/windows/apps/mt299098.aspx) en de [Windows 8 appdata roaming blog developer](http://blogs.msdn.com/b/windowsappdev/archive/2012/07/17/roaming-your-app-data.aspx).

## <a name="what-account-is-used-for-settings-sync"></a>Welk account wordt gebruikt voor synchronisatie van instellingen?
Synchronisatie van instellingen in Windows 8 en Windows 8.1, altijd consumer Microsoft-accounts gebruikt. Zakelijke gebruikers heeft de mogelijkheid verbinding maken met een Microsoft-account van hun account Active Directory-domein toegang te krijgen tot de synchronisatie van instellingen. In Windows 10 verbonden dit Microsoft-account met een account van de primaire en secundaire framework functionaliteit wordt vervangen.

De primaire account wordt gedefinieerd als het account waarmee zich aanmeldt bij Windows. Dit is een Microsoft-account, een account met Azure Active Directory (Azure AD), een lokale Active Directory-account of een lokale account. Naast het primaire account, kunnen Windows 10-gebruikers een of meer secundaire cloud-accounts op hun apparaat toevoegen. Een secundaire account wordt doorgaans een Microsoft-account, een Azure AD-account of enige andere account zoals Gmail of Facebook. Deze secundaire accounts bieden toegang tot extra services, zoals het eenmalige aanmelding en de Windows Store, maar ze zijn niet geschikt is voor het aansturen van de synchronisatie van instellingen.

In Windows 10 is het hoofdaccount voor het apparaat kan worden gebruikt voor synchronisatie van instellingen (Zie [hoe voer ik een upgrade van Microsoft-account instellingen synchroniseren in Windows 8 naar Azure AD sync instellingen in Windows 10?](active-directory-windows-enterprise-state-roaming-faqs.md#how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10)).

Gegevens is nooit gemengd tussen de verschillende gebruikersaccounts op het apparaat. Er zijn twee regels voor synchronisatie van instellingen:

* Windows-instellingen wordt altijd beschikbaar zijn met het hoofdaccount.
* App-gegevens worden gemarkeerd met het account voor het verkrijgen van de app. Alleen apps die zijn gemarkeerd met het hoofdaccount worden gesynchroniseerd. App-eigendom tagging wordt bepaald wanneer een app ' side-loaded via de Windows Store of beheer van mobiele apparaten (MDM).

Als de eigenaar van een app kan niet worden geïdentificeerd, wordt deze met het hoofdaccount kunnen roamen. Als een apparaat is geüpgraded van Windows 8 of Windows 8.1 naar Windows 10, worden alle apps label dat u hebt aangeschaft door de Microsoft-account. Dit is omdat de meeste gebruikers apps via de Windows Store verkrijgen en er geen ondersteuning voor Windows Store voor Azure AD-accounts voordat Windows 10 is. Als een app is geïnstalleerd via een offline licentie, worden de app gemarkeerd met het hoofdaccount op het apparaat.

> [!NOTE]
> Windows 10-apparaten die eigendom van het enterprise en zijn verbonden met Azure AD kunnen niet meer verbinding maken met hun Microsoft-account aan een domeinaccount. De mogelijkheid verbinding maken met een Microsoft-account een domeinaccount en synchroniseren van gegevens van de gebruiker bij de Microsoft-account (dat wil zeggen, het Microsoft-account roaming via de gekoppelde Microsoft-account en de functionaliteit van Active Directory) hebben wordt verwijderd uit Windows 10 apparaten die lid zijn van een verbonden Active Directory of Azure AD-omgeving.
>
>

## <a name="how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10"></a>Hoe voer ik een upgrade van Microsoft-account instellingen synchroniseren in Windows 8 naar Azure AD sync in Windows 10-instellingen?
Als u lid zijn van de Active Directory-domein met Windows 8 of Windows 8.1 met een gekoppelde Microsoft-account, kunt u instellingen worden gesynchroniseerd via uw Microsoft-account. Na de upgrade naar Windows 10, blijft u gebruikersinstellingen via Microsoft-account als u een gebruiker lid van een domein en de Active Directory-domein geen verbinding met Azure AD maken synchroniseren.

Als de lokale Active Directory-domein maakt verbinding met Azure AD, het apparaat zal proberen te synchroniseren instellingen met behulp van de verbonden Azure AD-account. Als de Azure AD-beheerder niet Enterprise State Roaming, uw verbonden kunnen Azure AD-account wordt stoppen van de synchronisatie van instellingen. Als u een Windows 10-gebruiker bent en u zich met een Azure AD-identiteit aanmeldt, gaat u windows-instellingen worden gesynchroniseerd als uw beheerder instellingen synchroniseren via Azure AD activeert.

Als u geen persoonlijke gegevens op uw zakelijke apparaat opgeslagen, moet u zich bewust zijn dat het Windows-besturingssysteem en toepassingsgegevens begint worden gesynchroniseerd met Azure AD. Dit heeft gevolgen voor de volgende:

* De instellingen van uw persoonlijke Microsoft-account wordt netjes afgezien van de instellingen op uw werk of school Azure AD-accounts. Dit komt doordat de Microsoft-account en Azure AD-instellingen synchroniseren nu afzonderlijke accounts gebruikt.
* Persoonlijke gegevens, zoals Wi-Fi-wachtwoorden, web-referenties en Favorieten in Internet Explorer die eerder zijn gesynchroniseerd via een gekoppelde Microsoft-account worden via Azure AD gesynchroniseerd.

## <a name="how-do-microsoft-account-and-azure-ad-enterprise-state-roaming-interoperability-work"></a>Hoe Microsoft-account en Azure AD Enterprise status Roaming interoperabiliteit werk?
In de versies November 2015 of hoger van Windows 10 wordt Enterprise State Roaming alleen ondersteund voor één account op een tijdstip. Als u zich aanmeldt bij Windows met een werk- of Azure AD-account schoolaccount, worden alle gegevens worden gesynchroniseerd via Azure AD. Als u zich aanmeldt bij Windows met behulp van een persoonlijk Microsoft-account, worden alle gegevens worden gesynchroniseerd via het Microsoft-account. Universele appdata wordt roamen met alleen het primaire-in-account op het apparaat en wordt deze alleen als de app-licentie is eigendom van het hoofdaccount roamen. Universele appdata voor de apps die eigendom zijn van een secundaire accounts worden niet gesynchroniseerd.

## <a name="do-settings-sync-for-azure-ad-accounts-from-multiple-tenants"></a>Synchroniseer instellingen voor Azure AD-accounts van meerdere tenants?
Wanneer meerdere Azure AD-accounts van verschillende Azure AD-tenants, zijn op hetzelfde apparaat, moet u de registersleutel van het apparaat om te communiceren met Azure Rights Management (Azure RMS) voor elke Azure AD-tenant bijwerken.  

1. De GUID voor elke Azure AD-tenant niet vinden. De Azure portal openen en selecteer een Azure AD-tenant. De GUID voor de tenant is op de pagina Eigenschappen voor de geselecteerde tenant (https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties) met het label **map-ID**. 
2. Nadat u de GUID hebt, moet u de registersleutel toevoegen **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\SettingSync\WinMSIPC\<tenant-ID GUID >**.
   Van de **tenant-ID GUID** sleutel, maakt u een nieuwe waarde met meerdere tekenreeksen (REG-MULTI-SZ) te gebruiken met de naam **AllowedRMSServerUrls**. Geef de licentieverlening distribution point URL's van de andere Azure-tenants die het apparaat toegang heeft tot voor de gegevens.
3. U vindt de distribution point URL's voor licentieverlening door het uitvoeren van de **Get-AadrmConfiguration** cmdlet. Als de waarden voor de **LicensingIntranetDistributionPointUrl** en **LicensingExtranetDistributionPointUrl** verschillend zijn, beide waarden opgeven. Als de waarden hetzelfde zijn, maar één keer de waarde opgeven.

## <a name="what-are-the-roaming-settings-options-for-existing-windows-desktop-applications"></a>Wat zijn de opties voor roaming voor een bestaande Windows-bureaubladtoepassingen?
Roaming werkt alleen voor universele Windows-apps. Er zijn twee opties beschikbaar voor het inschakelen van roaming op een bestaande Windows desktop-toepassing:

* De [bureaublad Bridge](http://aka.ms/desktopbridge) kunt u ervoor zorgt dat uw bestaande Windows desktop-apps op het Universal Windows Platform. Hier kunt minimale codewijzigingen worden om te profiteren van Azure AD app dataroaming. De brug bureaublad biedt uw apps met een app-id, die is vereist voor roaming voor bestaande desktop-apps van app-gegevens.
* [Gebruiker ervaring Virtualization (UE-V)](https://technet.microsoft.com/library/dn458947.aspx) kunt u een sjabloon aangepaste instellingen voor bestaande Windows desktop-apps maken en zwervende voor Win32-apps inschakelen. Deze optie is niet vereist voor de ontwikkelaar van de app om de code van de app te wijzigen. UE-V is beperkt tot de lokale Active Directory zwervende voor klanten die Microsoft Desktop Optimization Pack hebt aangeschaft.

Beheerders kunnen configureren UE-V voor Windows desktop app-gegevens door te wijzigen van de instellingen van het Windows-besturingssysteem en universele app-gegevens via roaming roamen [UE-V-groepsbeleid](https://technet.microsoft.com/itpro/mdop/uev-v2/configuring-ue-v-2x-with-group-policy-objects-both-uevv2), waaronder:

* Windows-instellingen voor Groepsbeleid kunnen worden gebruikt
* Groepsbeleid voor Windows-Apps niet synchroniseren
* Internet Explorer in de sectie toepassingen roaming

Microsoft kan in de toekomst manieren UE-V is nauw geïntegreerd in Windows maken en uitbreiden UE-V-instellingen via de Azure AD cloud roamen onderzoek.

## <a name="can-i-store-synced-settings-and-data-on-premises"></a>Kan ik gesynchroniseerde instellingen en gegevens die on-premises opslaan?
Enterprise State Roaming, worden alle gesynchroniseerde gegevens opgeslagen in de Azure-cloud. UE-V biedt een on-premises oplossing roaming.

## <a name="who-owns-the-data-thats-being-roamed"></a>Wie is eigenaar van de gegevens die wordt wordt zwerven?
De eigen ondernemingen de gegevens zwerven via Enterprise State Roaming. Gegevens worden opgeslagen in een Azure-datacenter. Alle gebruikersgegevens worden versleuteld onderweg zowel in rust in de cloud met Azure RMS. Dit is een verbetering ten opzichte van Microsoft-account gebaseerde instellingen sync, waarmee alleen bepaalde gevoelige gegevens, zoals de gebruikersreferenties worden versleuteld voordat deze het apparaat zal verlaten.

Microsoft hecht veel belang aan het beveiligen van gegevens van de klant. Een enterprise-gebruiker instellingsgegevens worden automatisch versleuteld door Azure RMS voordat u het Windows 10-apparaat, zodat er geen andere gebruiker deze gegevens kan lezen. Als uw organisatie beschikt over een betaald abonnement voor Azure RMS, u kunt gebruiken van andere Azure RMS-functies, zoals bijhouden en intrekken van documenten, automatisch e-mailberichten met gevoelige informatie te beveiligen en uw eigen sleutels beheren (de 'bring your own key'-oplossing ook ook wel BYOK). Zie voor meer informatie over deze functies en hoe Azure RMS werkt [wat is Azure Rights Management](https://technet.microsoft.com/jj585026.aspx).

## <a name="can-i-manage-sync-for-a-specific-app-or-setting"></a>Kan ik synchronisatie voor een specifieke app of -instellingen beheren?
In Windows 10 is er geen MDM of Groepsbeleid-instelling uitschakelen roaming voor een afzonderlijke toepassing. Tenantbeheerders kunnen uitschakelen appdata synchronisatie voor alle apps op een beheerd apparaat, maar er is geen mate van controle op een per-app of in-app-niveau.

## <a name="how-can-i-enable-or-disable-roaming"></a>Hoe kan ik in- of uitschakelen roaming?
In de **instellingen** app, gaat u naar **Accounts** > **uw instellingen synchroniseren**. Op deze pagina kunt u zien welk account wordt gebruikt voor Roaming van instellingen, en u kunt inschakelen of uitschakelen van afzonderlijke groepen van instellingen om te worden zwerven.

## <a name="what-is-microsofts-recommendation-for-enabling-roaming-in-windows-10"></a>Wat is de aanbeveling van Microsoft voor het inschakelen van roaming in Windows 10?
Microsoft heeft een aantal andere instellingen oplossingen beschikbaar zijn, inclusief zwervende gebruikersprofielen, UE-V en Enterprise status zwervende roaming.  Microsoft hecht te maken van een investering in Enterprise State Roaming in toekomstige versies van Windows. Als uw organisatie niet is gereed of vertrouwd met de verplaatsing van gegevens naar de cloud, vervolgens raden wij u aan UE-V te gebruiken als uw primaire zwervende technologie. Als uw organisatie vereist ondersteuning voor bestaande Windows-bureaubladtoepassingen roaming maar graag verplaatsen naar de cloud, wordt u aangeraden Enterprise status zwervende en UE-V te gebruiken. Hoewel UE-V- en Enterprise State Roaming zeer soortgelijke technologieën zijn, zijn ze niet sluiten elkaar wederzijds uit. Ze vormen een aanvulling elkaar om ervoor te zorgen dat uw organisatie de verzorgt zwervende die uw gebruikers nodig hebben.  

Wanneer u Enterprise State Roaming en UE-V, gelden de volgende regels:

* Enterprise State Roaming is de primaire zwervende agent op het apparaat. UE-V wordt gebruikt om te voorzien in de 'Win32 gap."
* UE-V voor Windows-instellingen en moderne UWP-app-gegevens voor roaming moet worden uitgeschakeld wanneer het beleid met behulp van de groep UE-V. Deze vallen al Enterprise State Roaming.

## <a name="how-does-enterprise-state-roaming-support-virtual-desktop-infrastructure-vdi"></a>Hoe Enterprise State Roaming ondersteunt virtuele desktopinfrastructuur (VDI)?
Enterprise State Roaming wordt ondersteund op SKU's voor Windows 10-client, maar niet op server SKU's. Als een client virtuele machine wordt gehost op een hypervisor-machine en u extern bij de virtuele machine aanmelden, wordt uw gegevens kunnen roamen. Als gebruikers zich extern aanmelden bij een server voor een volledige Bureaubladervaring meerdere gebruikers delen de dezelfde OS roaming werkt mogelijk niet. Het tweede scenario op basis van een sessie wordt niet officieel ondersteund.

## <a name="what-happens-when-my-organization-purchases-azure-rms-after-using-roaming"></a>Wat gebeurt er als mijn organisatie Azure RMS koopt na gebruik van zwervende?
Als uw organisatie al wordt gebruikt in Windows 10 met het Azure RMS beperkt gebruik gratis abonnement, zwervende aanschaf van een betaald abonnement voor Azure RMS heeft geen invloed op de functionaliteit van de functie voor zwervende en geen configuratiewijzigingen vereist zijn door uw IT-beheerder.

## <a name="known-issues"></a>Bekende problemen
Zie de documentatie in de [probleemoplossing](active-directory-windows-enterprise-state-roaming-troubleshooting.md) sectie voor een lijst met bekende problemen. 

## <a name="related-topics"></a>Verwante onderwerpen
* [Overzicht van zwervende Enterprise](active-directory-windows-enterprise-state-roaming-overview.md)
* [Enterprise-status roaming in Azure Active Directory inschakelen](active-directory-windows-enterprise-state-roaming-enable.md)
* [Groep beleid en de MDM-instellingen voor synchronisatie van instellingen](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Windows 10 zwervende naslaginformatie](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Problemen oplossen](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
