---
title: Uw on-premises infrastructuur voor identiteiten in de cloud controleren.
description: Dit is de Azure AD Connect Health-pagina waar wordt beschreven wat dit is en waarvoor u het kunt gebruiken.
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 82798ea6-5cd3-4f30-93ae-d56536f8d8e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/18/2016
ms.author: vakarand
translationtype: Human Translation
ms.sourcegitcommit: 87c73981c74fc763fd1aec6c283e934c77008441
ms.openlocfilehash: 5a7a740ddac54e9b908d17e64b4590aecdc34956


---
# <a name="monitor-your-onpremises-identity-infrastructure-and-synchronization-services-in-the-cloud"></a>Uw on-premises infrastructuur voor identiteiten en synchronisatieservices in de cloud controleren
Azure AD Connect Health helpt u om inzicht te verkrijgen in uw on-premises infrastructuur voor identiteiten en de synchronisatieservices.  Hiermee kunt u een betrouwbare verbinding met Office 365 en Microsoft Online Services onderhouden door controlemogelijkheden te bieden voor uw belangrijkste identiteitsonderdelen, zoals AD FS-servers, Azure AD Connect-servers (ook bekend Sync Engine), Active Directory Domain Controllers enzovoort. Ook worden de belangrijkste gegevenspunten over deze onderdelen gemakkelijk toegankelijk gemaakt, zodat het gebruik en het verkrijgen van belangrijke informatie gemakkelijker worden en u weloverwogen beslissingen kunt nemen.

Deze informatie vindt u in de [portal voor Azure AD Connect Health](https://aka.ms/aadconnecthealth). In de portal voor Azure AD Connect Health kunt u waarschuwingen bekijken, de prestaties controleren, het gebruik analyseren en nog veel meer. Met Azure AD Connect Health beschikt u in een oogopslag over de status van uw belangrijkste identiteitsonderdelen.

![Wat is Azure AD Connect Health?](./media/active-directory-aadconnect-health/aadconnecthealth2.png)

Toekomstige updates voor Azure AD Connect Health bevatten extra controle- en inzichtmogelijkheden voor aanvullende identiteitsonderdelen. Zo beschikt u over één dashboard als het gaat om identiteiten, waardoor u een meer robuuste, stabiele en geïntegreerde omgeving hebt waarvan uw gebruikers kunnen profiteren bij hun werk.

## <a name="why-use-azure-ad-connect-health"></a>Waarom Azure AD Connect Health gebruiken?
Wanneer u uw on-premises directory's integreert met Azure AD, worden uw gebruikers productiever omdat zij één identiteit hebben voor toegang tot zowel cloud als on-premises resources. Bij deze integratie moet er echter wel voor worden gezorgd dat deze omgeving stabiel is, zodat gebruikers op een betrouwbare manier toegang hebben tot zowel on-premises als cloudresources vanaf elk apparaat. Azure AD Connect Health biedt een gemakkelijke, cloudgebaseerde benadering voor controle van en inzicht in uw on-premises infrastructuur, die wordt gebruikt voor toegang tot Office 365 of andere Azure AD-toepassingen. Het is net zo eenvoudig als het installeren van een agent op een van uw on-premises identiteitsservers.

## <a name="azure-ad-connect-health-for-ad-fsactivedirectoryaadconnecthealthadfsmd"></a>[Azure AD Connect Health voor AD FS](active-directory-aadconnect-health-adfs.md)
Azure AD Connect Health voor AD FS ondersteunt AD FS 2.0 op Windows Server 2008 R2, AD FS in Windows Server 2012 en Windows Server 2012R2. Het ondersteunt ook de bewaking van AD FS Proxy- of Web Application Proxy-servers die verificatie-ondersteuning bieden voor extranettoegang. Met een eenvoudige en voordelige installatie van de statusagent, biedt Azure AD Connect Health voor AD FS de volgende set kernfuncties:

* Controle met waarschuwingen over de status van AD FS- en AD FS Proxy-servers
* E-mailmeldingen voor kritieke waarschuwingen
* Weergave van trends wat betreft prestatiegegevens, hetgeen nuttig is voor de capaciteitsplanning van AD FS
* Gebruiksanalyse voor AD FS-aanmeldingen met een andere draaigrafiek (apps, gebruikers, netwerklocatie, enzovoort), wat handig is voor een beter begrip van de manier waarop AD FS wordt gebruikt.
* Rapporten voor AD FS, zoals 50 belangrijkste gebruikers die aanmeldingspogingen ondernemen met een ongeldige gebruikersnaam/ongeldig wachtwoord, met laatste IP-adres

De volgende video biedt een overzicht van Azure AD Connect Health voor AD FS

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health--Monitor-you-identity-bridge/player]
>
>

## <a name="azure-ad-connect-health-for-syncactivedirectoryaadconnecthealthsyncmd"></a>[Azure AD Connect Health for Sync](active-directory-aadconnect-health-sync.md)
Met Azure AD Connect Health voor synchroniseren wordt informatie gecontroleerd en geboden over de synchronisaties die worden uitgevoerd tussen uw on-premises Active Directory en Azure Active Directory. Azure AD Connect Health voor synchroniseren biedt de volgende set kernfuncties:

* Controle met waarschuwingen over de status van Azure AD Connect-servers of Sync Engine
* E-mailmeldingen voor kritieke waarschuwingen
* Operationele synchronisatie-inzichten, inclusief latentiegrafieken voor synchronisatiebewerkingen en trends in verschillende bewerkingen zoals toevoegingen, updates en verwijderingen.
* In een oogopslag informatie over synchronisatie-eigenschappen en de laatste succesvolle export naar Azure AD
* Voor rapporten over synchronisatiefouten op objectniveau \(is Azure AD Premium niet vereist\)

De volgende video biedt een overzicht van Azure AD Connect Health for Sync

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Health-Monitoring-the-sync-engine/player]
>
>

## <a name="azure-ad-connect-health-for-ad-ds-previewactivedirectoryaadconnecthealthaddsmd"></a>[Azure AD Connect Health voor AD DS (preview)](active-directory-aadconnect-health-adds.md)
Azure AD Connect Health voor AD DS biedt bewaking voor domeincontrollers die geïnstalleerd zijn in Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2. Een eenvoudige en voordelige statusagent-installatie, waarmee u uw on-premises AD DS-omgeving rechtstreeks vanuit de cloud kunt bewaken. Azure AD Connect Health voor AD DS biedt de volgende set kernfuncties:

* Bewakingswaarschuwingen om te detecteren wanneer domeincontrollers niet in orde zijn, samen met e-mailmeldingen voor kritieke waarschuwingen.
* Dashboard voor domeincontrollers, waarmee u snel inzicht krijgt in de (operationele) status van uw domeincontrollers.
* Dashboard voor replicatiestatus, met meest recente replicatiegegevens, samen met koppelingen naar gidsen voor het oplossen van problemen wanneer er fouten worden gedetecteerd.
* Snel overal toegang tot grafieken met prestatiegegevens van populaire prestatiemeteritems, vereist voor het oplossen van problemen en bewakingsdoeleinden.

De volgende video biedt een overzicht van Azure AD Connect Health voor AD DS

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health-monitors-on-premises-AD-Domain-Services/player]
>
>

## <a name="get-started-with-azure-ad-connect-health"></a>Aan de slag met Azure AD Connect Health
Het is heel eenvoudig om aan de slag te gaan met Azure AD Connect Health. Volg de onderstaande stappen:

1. [Installeer Azure AD Premium](active-directory-get-started-premium.md) of [start een proefversie](https://azure.microsoft.com/trial/get-started-active-directory/).
2. [Download en installeer Azure AD Connect Health-agents](#download-and-install-azure-ad-connect-health-agent) op uw servers voor identiteiten.
3. Ga naar [https://aka.ms/aadconnecthealth](https://aka.ms/aadconnecthealth) om het Azure AD Connect Health-dashboard te bekijken

> [!NOTE]
> Houd er rekening mee dat u de Azure AD Connect Health-agents moet installeren op uw doelservers, voordat u gegevens ziet in uw Azure AD Connect Health-dashboard.
>
>

## <a name="download-and-install-azure-ad-connect-health-agent"></a>Azure AD Connect Health-agent downloaden en installeren
* Zorg dat u voldoet aan de vereisten voor Azure AD Connect Health
* Als u aan de slag wilt gaan met Azure AD Connect Health voor AD FS, kunt u de nieuwste versie van de agent hier downloaden: [Azure AD Connect Health-agent voor AD FS downloaden.](http://go.microsoft.com/fwlink/?LinkID=518973)
  [](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs)
* Als u aan de slag wilt gaan met Azure AD Connect Health voor synchroniseren, downloadt en installeert u de [nieuwste versie van Azure AD Connect](http://go.microsoft.com/fwlink/?linkid=615771).  De statusagent wordt geïnstalleerd als onderdeel van de Azure AD Connect-installatie (versie 1.0.9125.0 of hoger).  Azure AD Connect ondersteunt een upgrade die u in plaats van eerdere versies kunt installeren.
* Als u aan de slag wilt gaan met Azure AD Connect Health voor AD DS, kunt u de nieuwste versie van de agent hier downloaden: [Download Azure AD Connect Health Agent for AD DS (De Azure AD Connect Health-agent voor AD DS downloaden).](http://go.microsoft.com/fwlink/?LinkID=820540)
  [](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs)

## <a name="azure-ad-connect-health-portal"></a>Portal voor Azure AD Connect Health
In de portal voor Azure AD Connect Health kunt u waarschuwingen weergeven, de prestaties controleren en het gebruik analyseren. Via de koppeling https://aka.ms/aadconnecthealth gaat u naar de hoofdblade van Azure AD Connect Health.  Een blade kunt u zien als een venster. Op de hoofdblade ziet u Snel starten, services in Azure AD Connect Health en extra configuratie-opties. Hieronder vindt u een schermafbeelding met een korte uitleg van elk van deze onderdelen.  Nadat u de agents hebt geïmplementeerd, identificeert de Health-service automatisch de services die met Azure AD Connect Health worden bewaakt.

![Portal voor Azure AD Connect Health](./media/active-directory-aadconnect-health/portal4.png)

* **Snel starten** - hiermee opent u de blade Snel starten. Hier kunt u de Azure AD Connect Health-agent downloaden door Hulpmiddelen ophalen te selecteren. Ook kunt u de documentatie openen en feedback geven.
* **Active Directory Federation Services** - dit verwijst naar alle AD FS-services die momenteel worden gecontroleerd door Azure AD Connect Health. Door een van de exemplaren te selecteren, wordt er een blade geopend met informatie over dat servicesexemplaar.  Deze informatie omvat een overzicht, eigenschappen, waarschuwingen, controle en gebruiksanalyses. U kunt [hier](active-directory-aadconnect-health-adfs.md) meer lezen over de functionaliteit.
* **Azure Active Directory Connect (Sync)** - dit verwijst naar de Azure AD Connect- servers die momenteel door Azure AD Connect Health worden gecontroleerd. Als u het item selecteert, wordt er een blade geopend met informatie over uw Azure AD Connect-servers. U kunt [hier](active-directory-aadconnect-health-sync.md) meer lezen over de functionaliteit.
* **Active Directory Domain Services** - dit verwijst naar alle AD DS-forests die momenteel door Azure AD Connect Health worden bewaakt. Als u een van de forests selecteert, wordt een blade geopend met informatie over dat forest.  Deze informatie bevat een overzicht van essentiële gegevens, een dashboard voor domeincontrollers, een dashboard voor replicatiestatus, waarschuwingen en bewaking. U kunt [hier](active-directory-aadconnect-health-adds.md) meer lezen over de functionaliteit.
* **Configureren** - hiermee kunt u de volgende opties in- of uitschakelen:

  1. Automatische updates zodat de Azure AD Connect Health-agent automatisch wordt bijgewerkt naar de nieuwste versie. Dit betekent dat uw systeem automatisch wordt bijgewerkt naar de nieuwste versie van Azure AD Connect Health Agent zodra deze beschikbaar is. Deze optie is standaard ingeschakeld.
  2. Microsoft alleen toegang geven tot de statusgegevens van uw Azure AD-directory voor het oplossen van problemen. Dit betekent dat als deze optie is ingeschakeld, Microsoft dezelfde gegevens kan zien als u. Dit kan handig zijn bij het oplossen van problemen en het verlenen van assistentie. Deze optie is standaard uitgeschakeld.

## <a name="related-links"></a>Verwante koppelingen
* [De Azure AD Connect Health-agent installeren](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health Operations](active-directory-aadconnect-health-operations.md) (Azure AD Connect Health-bewerkingen)
* [Azure AD Connect Health gebruiken met AD FS](active-directory-aadconnect-health-adfs.md)
* [Azure AD Connect Health for Sync gebruiken](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health gebruiken met AD DS](active-directory-aadconnect-health-adds.md)
* [Veelgestelde vragen over Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
* [Versiegeschiedenis van Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)



<!--HONumber=Nov16_HO2-->


