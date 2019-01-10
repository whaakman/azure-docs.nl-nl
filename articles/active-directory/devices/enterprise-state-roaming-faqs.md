---
title: Instellingen en gegevensroaming Veelgestelde vragen over | Microsoft Docs
description: Vindt u antwoorden op enkele vragen die IT-beheerders mogelijk over de instellingen en app-gegevens synchroniseren.
services: active-directory
keywords: Enterprise state roaming-instellingen, windows-cloud, veelgestelde vragen over enterprise state roaming
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: curtand
ms.component: devices
ms.assetid: c0824f5c-129b-4240-969f-921f6a64eae7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: markvi
ms.openlocfilehash: 9f3d171a8f0e353d5860f410a8c32149f8872338
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54189385"
---
# <a name="settings-and-data-roaming-faq"></a>Veelgestelde vragen over instellingen en gegevensroaming
In dit artikel vindt u antwoorden op enkele vragen die IT-beheerders mogelijk over de instellingen en app-gegevens synchroniseren.

## <a name="what-data-roams"></a>Welke gegevens roamt?
**Instellingen voor Windows**: de PC-instellingen die zijn ingebouwd in het Windows-besturingssysteem. Over het algemeen zijn instellingen die van uw PC aanpassen en ze de volgende categorieën bevatten:

* *Thema*, waaronder functies zoals bureaubladinstellingen thema en de taakbalk.
* *Instellingen van Internet Explorer*, met inbegrip van onlangs geopende tabbladen en Favorieten.
* *Microsoft Edge-browserinstellingen*, zoals Favorieten en leeslijst.
* *Wachtwoorden*, met inbegrip van wachtwoorden voor Internet-, Wi-Fi-profielen en anderen.
* *Taalvoorkeuren*, waaronder instellingen voor de toetsenbord-indelingen, systeemtaal, datum en tijd en meer.
* *Gebruiksgemak toegangsfuncties*, zoals thema met hoog contrast en Narrator, Vergrootglas.
* *Andere Windows-instellingen*, zoals muisinstellingen.

**Toepassingsgegevens**: Universele Windows-apps instellingsgegevens naar een map voor zwervende schrijft en alle gegevens die naar deze map geschreven automatisch worden gesynchroniseerd. Het is aan de afzonderlijke app-ontwikkelaar een app om te profiteren van deze mogelijkheid te ontwerpen. Zie voor meer informatie over het ontwikkelen van een universele Windows-app die gebruikmaakt van roaming, het [appdata-API van storage](https://msdn.microsoft.com/library/windows/apps/mt299098.aspx) en de [Windows 8 appdata roaming ontwikkelaarsblog](https://blogs.msdn.com/b/windowsappdev/archive/2012/07/17/roaming-your-app-data.aspx).

## <a name="what-account-is-used-for-settings-sync"></a>Welk account wordt gebruikt voor synchronisatie van instellingen?
In Windows 8.1, instellingen synchroniseren is altijd gebruikt voor consumenten-Microsoft-accounts. Zakelijke gebruikers heeft de mogelijkheid om te verbinden met hun Active Directory-domeinaccount toegang te krijgen tot de synchronisatie van instellingen voor een Microsoft-account. In Windows 10 verbonden deze functionaliteit wordt vervangen met een primaire/secundaire account-framework voor Microsoft-account.

Het hoofdaccount is gedefinieerd als het account waarmee zich aanmeldt bij Windows. Dit kan een Microsoft-account, Azure Active Directory (Azure AD)-account, een on-premises Active Directory-account of een lokaal account zijn. Windows 10-gebruikers kunnen een of meer secundaire cloudaccounts toevoegen aan hun apparaat naast het hoofdaccount. Een secundaire-account is doorgaans een Microsoft-account, een Azure AD-account of een ander account, zoals Gmail of Facebook. Deze secundaire accounts geven toegang tot extra services zoals eenmalige aanmelding en de Windows Store, maar ze zijn niet geschikt voor die instellingen synchroniseren.

In Windows 10, het hoofdaccount voor het apparaat kan worden gebruikt voor synchronisatie van instellingen (Zie [hoe voer ik een upgrade van Microsoft-account-instellingen synchroniseren in Windows 8 naar Azure AD-instellingen synchroniseren is in Windows 10?](enterprise-state-roaming-faqs.md#how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10)).

Gegevens wordt nooit gemengd tussen de verschillende gebruikersaccounts op het apparaat. Er zijn twee regels voor synchronisatie van instellingen:

* Windows-instellingen wordt altijd beschikbaar zijn met het hoofdaccount.
* App-gegevens worden, gemarkeerd met het account voor het verkrijgen van de app. Alleen apps die zijn gemarkeerd met het hoofdaccount worden gesynchroniseerd. App-eigenaar tagging wordt bepaald wanneer een app ' side-loaded via de Windows Store of beheer van mobiele apparaten (MDM is).

Als de eigenaar van een app kan niet worden geïdentificeerd, wordt deze kunnen worden gebruikt met het hoofdaccount. Als een apparaat is een upgrade uitgevoerd van Windows 8 of Windows 8.1 naar Windows 10, worden alle apps in het label dat u hebt aangeschaft door de Microsoft-account. Dit is omdat de meeste gebruikers apps via de Windows Store aanschaffen, en er geen ondersteuning voor Windows Store voor Azure AD-accounts voordat u Windows 10 is. Als een app via een offline-licentie is geïnstalleerd, wordt de app worden gelabeld met behulp van het hoofdaccount op het apparaat.

> [!NOTE]
> Windows 10-apparaten die eigendom zijn van enterprise en zijn verbonden met Azure AD kunnen niet meer verbinding met het maken van hun Microsoft-accounts met een domeinaccount. De mogelijkheid om te verbinden met een Microsoft-account een domeinaccount hebben van de gebruiker gegevens synchroniseren met het Microsoft-account (dat wil zeggen, het Microsoft-account roaming via het Microsoft-account verbonden en de functionaliteit van Active Directory) wordt verwijderd uit Windows 10 apparaten die lid zijn van een verbonden Active Directory of Azure AD-omgeving.
>
>

## <a name="how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10"></a>Hoe voer ik een upgrade van Microsoft-account-instellingen synchroniseren in Windows 8 naar Azure AD in Windows 10-instellingen synchroniseren?
Als u lid zijn van Active Directory-domein met Windows 8.1 met een Microsoft-account verbonden, wordt u instellingen synchroniseren via uw Microsoft-account. Na de upgrade naar Windows 10, blijft u synchronisatie van gebruikersinstellingen via Microsoft-account als u een domein-gebruiker bent en de Active Directory-domein geen verbinding met Azure AD maken.

Als de on-premises Active Directory-domein maakt verbinding met Azure AD, uw apparaat probeert te synchroniseren met behulp van de gekoppelde instellingen Azure AD-account. Als de Azure AD-beheerder is niet ingeschakeld voor het Enterprise State Roaming, het verbonden Azure AD-account wordt stoppen van de synchronisatie van instellingen. Als u een Windows 10-gebruiker bent en u zich aanmelden met een Azure AD-identiteit, begint u windows-instellingen synchroniseren zodra de beheerder kan instellingen synchroniseren via Azure AD.

Als u geen persoonlijke gegevens op uw zakelijke apparaat opgeslagen, moet u rekening mee dat Windows-besturingssysteem en toepassing zullen worden gesynchroniseerd met Azure AD. Dit heeft de volgende implicaties:

* De instellingen van uw persoonlijke Microsoft-account wordt drift naast de instellingen op uw werk- of schoolaccount van Azure AD-accounts. Dit komt doordat de Microsoft-account en Azure AD-instellingen synchroniseren nu verschillende accounts gebruikt.
* Persoonlijke gegevens, zoals Wi-Fi-wachtwoorden, web-referenties en Favorieten in Internet Explorer die eerder zijn gesynchroniseerd via een gekoppelde Microsoft-account worden via Azure AD gesynchroniseerd.

## <a name="how-do-microsoft-account-and-azure-ad-enterprise-state-roaming-interoperability-work"></a>Hoe Microsoft-account en Azure AD Enterprise State Roaming interoperabiliteit werk?
Bij de versies van November 2015 of hoger van Windows 10 wordt Enterprise State Roaming alleen ondersteund voor één account tegelijkertijd. Als u zich aanmelden bij Windows met behulp van een werk- of schoolaccount van Azure AD-account, worden alle gegevens worden gesynchroniseerd via Azure AD. Als u bij Windows aanmelden met behulp van een persoonlijk Microsoft-account, worden alle gegevens worden gesynchroniseerd via het Microsoft-account. Universele appdata wordt kunnen worden gebruikt met alleen het primaire-aanmelden-account op het apparaat en wordt deze alleen als eigenaar van de app-licentie is het hoofdaccount roamen. Universele appdata voor de apps die eigendom zijn van een secundaire accounts worden niet gesynchroniseerd.

## <a name="do-settings-sync-for-azure-ad-accounts-from-multiple-tenants"></a>Synchroniseer instellingen voor Azure AD-accounts van meerdere tenants?
Wanneer meerdere Azure AD-accounts van andere Azure AD-tenants op hetzelfde apparaat, moet u het apparaatregister om te communiceren met de Azure Rights Management-service voor elke Azure AD-tenant bijwerken.  

1. De GUID voor elke Azure AD-tenant niet vinden. Open de Azure-portal en selecteert u een Azure AD-tenant. De GUID voor de tenant is op de pagina Eigenschappen voor de geselecteerde tenant (https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties), gelabelde **map-ID**. 
2. Nadat u de GUID hebt, moet u de registersleutel toevoegen **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\SettingSync\WinMSIPC\<tenant-ID GUID >**.
   Uit de **tenant-ID GUID** sleutel, maakt u een nieuwe waarde met meerdere tekenreeksen (REG-MULTI-SZ) te gebruiken met de naam **AllowedRMSServerUrls**. Geef de licentieverlening distribution point URL's van de andere Azure-tenants die het apparaat toegang heeft tot voor de gegevens.
3. U kunt de distributie punt URL's voor licentieverlening vinden door te voeren de **Get-AadrmConfiguration** cmdlet uit de AADRM-module. Als de waarden voor de **LicensingIntranetDistributionPointUrl** en **LicensingExtranetDistributionPointUrl** verschillend zijn, worden beide waarden opgeven. Als de waarden gelijk zijn, geeft u de waarde slechts één keer in.

## <a name="what-are-the-roaming-settings-options-for-existing-windows-desktop-applications"></a>Wat zijn de opties voor roaming voor bestaande Windows-bureaubladtoepassingen?
Roaming werkt alleen voor universele Windows-apps. Er zijn twee opties beschikbaar voor het inschakelen van roaming op een bestaande Windows desktop-toepassing:

* De [Desktop Bridge](https://aka.ms/desktopbridge) helpt u bij uw bestaande Windows-desktopapps brengen voor het Universal Windows Platform. Hier worden wijzigingen in de minimale code vereist om te profiteren van Azure AD app dataroaming. De Desktop-brug biedt uw apps met een app-id, die nodig is om in te schakelen app data roaming voor bestaande bureaublad-apps.
* [Gebruiker ervaring Virtualization (UE-V)](https://technet.microsoft.com/library/dn458947.aspx) helpt u bij het maken van een sjabloon voor aangepaste instellingen voor bestaande Windows-bureaublad-apps en zwervende voor Win32-apps inschakelen. Deze optie is niet vereist voor de app-ontwikkelaar code van de app te wijzigen. UE-V is beperkt tot on-premises Active Directory roaming voor klanten die Microsoft Desktop Optimization Pack hebt aangeschaft.

Beheerders kunnen configureren UE-V voor Windows desktop-app-gegevens door het veranderen van roaming van instellingen voor Windows-besturingssysteem en de universele app-gegevens via roamen [UE-V-groepsbeleid](https://technet.microsoft.com/itpro/mdop/uev-v2/configuring-ue-v-2x-with-group-policy-objects-both-uevv2), waaronder:

* Groepsbeleid voor Windows-instellingen kunnen worden gebruikt
* Groepsbeleid voor Windows-Apps niet synchroniseren
* Internet Explorer in de sectie toepassingen roaming

Microsoft kan in de toekomst manieren UE-V is nauw geïntegreerd in Windows maken en uitbreiden van UE-V voor Roaming van instellingen via de cloud met Azure AD onderzoeken.

## <a name="can-i-store-synced-settings-and-data-on-premises"></a>Kan ik gesynchroniseerde instellingen en gegevens on-premises opslaan?
Enterprise State Roaming dat alle gesynchroniseerde gegevens worden opgeslagen in de Microsoft-cloud. UE-V biedt een on-premises oplossing roaming.

## <a name="who-owns-the-data-thats-being-roamed"></a>Wie is eigenaar van de gegevens die wordt wordt zwerven?
De eigen ondernemingen de gegevens zwerven via Enterprise State Roaming. Gegevens worden opgeslagen in een Azure-datacenter. Alle gegevens van de gebruiker is versleuteld, zowel in-transit en in rust in de cloud met behulp van de Azure Rights Management-service van Azure Information Protection. Dit is een verbetering ten opzichte van Microsoft-account gebaseerde instellingen sync, waarmee alleen bepaalde gevoelige gegevens, zoals gebruikersreferenties worden versleuteld voordat het apparaat verlaat.

Microsoft waakt bescherming van gegevens van de klant. Gegevens van de instellingen van de ondernemingsgebruiker van een is automatisch versleuteld door de Azure Rights Management-service voordat het verlaten van een Windows 10-apparaat, zodat er geen andere gebruiker deze gegevens kan lezen. Als uw organisatie een betaald abonnement voor de Azure Rights Management-service heeft, u kunt gebruiken van andere functies voor gegevensbeveiliging, zoals bijhouden en intrekken van documenten, automatisch e-mailberichten met gevoelige informatie beveiligen en beheren van uw eigen sleutels (de 'bring Your own key'-oplossing, ook wel BYOK). Zie voor meer informatie over deze functies en de werking van deze beveiligingsservice [wat is Azure Rights Management](/azure/information-protection/what-is-information-protection).

## <a name="can-i-manage-sync-for-a-specific-app-or-setting"></a>Kan ik synchronisatie uit voor de instelling van een specifieke app of beheren?
In Windows 10 is er geen MDM of Groepsbeleid-instelling uitschakelen van roaming voor een afzonderlijke toepassing. Tenantbeheerders appdata synchronisatie uit voor alle apps op een beheerd apparaat kunnen uitschakelen, maar er is geen mate van controle op het niveau van een per-app of in-app.

## <a name="how-can-i-enable-or-disable-roaming"></a>Hoe kan ik inschakelen of uitschakelen van roaming?
In de **instellingen** app, gaat u naar **Accounts** > **uw instellingen synchroniseren**. Op deze pagina kunt u zien welk account wordt gebruikt om de instellingen kunnen worden gebruikt en u kunt inschakelen of uitschakelen van afzonderlijke groepen van instellingen om te worden zwerven.

## <a name="what-is-microsofts-recommendation-for-enabling-roaming-in-windows-10"></a>Wat is de aanbeveling van Microsoft voor het inschakelen van roaming in Windows 10?
Microsoft heeft een aantal andere instellingen mobiele oplossingen beschikbaar zijn, met inbegrip van zwervende gebruikersprofielen, UE-V en Enterprise State Roaming.  Microsoft waakt te maken van een investering in Enterprise State Roaming in toekomstige versies van Windows. Als uw organisatie niet is gereed of vertrouwd bent met het verplaatsen van gegevens naar de cloud, klikt u vervolgens het beste UE-V te gebruiken als uw primaire zwervende technologie. Als uw organisatie vereist ondersteuning voor bestaande Windows-bureaubladtoepassingen roaming, maar er is door wilt gaan naar de cloud, raden wij Enterprise State Roaming en UE-V te gebruiken. Hoewel UE-V- en Enterprise State Roaming vergelijkbare technologieën zijn, zijn ze niet sluiten elkaar wederzijds uit. Ze vullen elkaar om ervoor te zorgen dat uw organisatie biedt de zwervende services die uw gebruikers nodig hebben.  

Wanneer u Enterprise State Roaming en UE-V, gelden de volgende regels:

* Enterprise State Roaming is de primaire zwervende agent op het apparaat. UE-V wordt gebruikt om te voorzien in de 'Win32 kloof."
* UE-V voor Windows-instellingen en moderne UWP-app data roaming moet worden uitgeschakeld wanneer de groep UE-V met beleidsregels. Deze zijn al gedekt door Enterprise State Roaming.

## <a name="how-does-enterprise-state-roaming-support-virtual-desktop-infrastructure-vdi"></a>Hoe Enterprise State Roaming ondersteunt virtuele desktopinfrastructuur (VDI)?
Enterprise State Roaming wordt ondersteund op Windows 10 client-SKU's, maar niet op server SKU's. Als een client virtuele machine wordt gehost op een hypervisor-machine en u extern bij de virtuele machine aanmelden, worden uw gegevens kunnen roamen. Als meerdere gebruikers de dezelfde OS delen en gebruikers zich extern bij een server voor een volledige Bureaubladervaring aanmelden, roaming werkt mogelijk niet. Het tweede scenario op basis van een sessie wordt niet officieel ondersteund.

## <a name="what-happens-when-my-organization-purchases-a-subscription-that-includes-azure-rights-management-after-using-roaming"></a>Wat gebeurt er als mijn organisatie een abonnement dat aanschaft Azure Rights Management bevat nadat u roaming?
Als uw organisatie al met zwervende in Windows 10 met het gratis abonnement voor Azure Rights Management-beperkt gebruik, aanschaffen van een [betaald abonnement](https://azure.microsoft.com/pricing/details/information-protection/) waarin de Azure Rights Management-protection-service niet heeft is opgenomen dit van invloed is op de functionaliteit van de functie voor roaming en er zijn geen wijzigingen in de configuratie wordt door uw IT-beheerder worden vereist.

## <a name="known-issues"></a>Bekende problemen
Raadpleeg de documentatie in de [probleemoplossing](enterprise-state-roaming-troubleshooting.md) sectie voor een lijst met bekende problemen. 

## <a name="next-steps"></a>Volgende stappen 

Zie voor een overzicht [enterprise state roaming overzicht](enterprise-state-roaming-overview.md)
