---
title: Aanbevolen procedures voor beveiliging van gegevens en versleuteling | Microsoft Docs
description: In dit artikel biedt een set met aanbevolen procedures voor beveiliging van gegevens en ingebouwde mogelijkheden van Azure met behulp van versleuteling.
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 17ba67ad-e5cd-4a8f-b435-5218df753ca4
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: yurid
ms.openlocfilehash: 0cebc7ae5279b720e8fd0d6c986e1706d944476f
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="azure-data-security-and-encryption-best-practices"></a>Best Practices voor beveiliging van gegevens van Azure en versleuteling
Een van de sleutels voor bescherming van gegevens in de cloud is accounting voor de mogelijke statussen in die uw gegevens zich kunnen voordoen, en welke besturingselementen beschikbaar zijn voor die status. Omwille van de Azure data zijn versleuteling best practices voor beveiliging en de aanbevelingen om de statussen van de volgende gegevens:

* In rust: Dit omvat alle informatie opslagobjecten, containers en typen die statisch bestaan op de fysieke media, worden deze magnetische of optische schijf.
* In Transit: Wanneer gegevens worden overgebracht tussen onderdelen, locaties of programma's, zoals via het netwerk via een servicebus (van on-premises naar cloud en vice versa, inclusief hybride verbindingen zoals ExpressRoute) of tijdens een i/o, dit wordt beschouwd als worden in beweging.

In dit artikel wordt een verzameling van Azure data encryption best practices voor beveiliging en besproken. Deze aanbevolen procedures zijn afgeleid van onze ervaring met beveiliging van gegevens van Azure en versleuteling en de ervaringen van klanten zoals zelf.

Voor elke aanbevolen procedure wordt uitgelegd:

* Wat de beste handelswijze is
* Waarom u wilt dat het beste inschakelen
* Wat zijn het resultaat als u niet de aanbevolen procedure inschakelen
* Mogelijke alternatieven voor de aanbevolen procedure
* Hoe u kunt informatie over het inschakelen van de aanbevolen procedure

In dit artikel Azure-gegevensbeveiliging en Best Practices voor versleuteling is gebaseerd op een advies consensus en mogelijkheden van Azure-platform en functiesets, als deze bestaan in de tijd die in dit artikel is geschreven. Adviezen en technologieën op den duur veranderen en dit artikel wordt regelmatig gecontroleerd op basis van deze wijzigingen worden bijgewerkt.

Azure data beveiligings- en aanbevolen procedures in dit artikel wordt beschreven, zijn onder andere:

* Multi-factor authentication afdwingen
* Gebruik rollen gebaseerd toegangsbeheer (RBAC)
* Virtuele machines van Azure versleutelen
* Hardware security modellen gebruiken
* Met beveiligde werkstations beheren
* SQL-gegevensversleuteling inschakelt
* De gegevens onderweg te beschermen
* Bestand niveau gegevensversleuteling afdwingen

## <a name="enforce-multi-factor-authentication"></a>Multi-factor Authentication afdwingen
De eerste stap bij de toegang tot gegevens en beheer in Microsoft Azure is de gebruiker te verifiëren. [Azure multi-factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md) is een methode voor het verifiëren van de identiteit van gebruiker met een andere methode dan alleen een gebruikersnaam en wachtwoord. Deze verificatie methode helpt beveiliging toegang tot gegevens en toepassingen en te voldoen aan de behoeften van de gebruiker voor een eenvoudig proces aanmelden.

Door Azure MFA inschakelen voor uw gebruikers, kunt u een tweede beveiligingslaag wilt toevoegen aan de gebruikersaanmeldingen en transacties. In dit geval een transactie kan toegang krijgen tot een document dat zich in een bestandsserver of op uw SharePoint Online. Azure MFA ook helpt IT verminderen de kans dat een verdachte referenties toegang tot gegevens van de organisatie hebben.

Bijvoorbeeld: als u Azure MFA voor uw gebruikers afdwingen en configureren voor het gebruik van een telefoongesprek of SMS-bericht als verificatie als referentie voor de gebruiker is geknoeid, toegang tot alle bronnen omdat hij geen toegang tot het telefoonnummer van de gebruiker de aanvaller niet mogelijk. Organisaties die deze extra beschermingslaag identiteit niet toevoegt zijn vatbaarder zijn voor de aanval diefstal van referenties, wat tot inbreuk op gegevens leiden kan.

Een alternatief voor organisaties die u behouden van de verificatie besturingselement wilt on-premises is [Azure multi-factor Authentication-Server](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), ook wel MFA lokale. Met deze methode zich u nog steeds multi-factor authentication, terwijl de MFA-server on-premises afdwingen.

Lees het artikel voor meer informatie over Azure MFA [aan de slag met Azure multi-factor Authentication in de cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Gebruik rollen gebaseerd toegangsbeheer (RBAC)
Toegang beperken op basis de [moet weten](https://en.wikipedia.org/wiki/Need_to_know) en [minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) beveiligingsprincipes. Dit is noodzakelijk voor organisaties die willen beveiligingsbeleid instellen voor toegang tot gegevens. Azure op rollen gebaseerde toegangsbeheer (RBAC) kan worden gebruikt om machtigingen te wijzen aan gebruikers, groepen en toepassingen op een bepaalde scope. Het bereik van een roltoewijzing kan dit een abonnement, resourcegroep of één resource.

U kunt gebruikmaken van [ingebouwde RBAC-rollen](../active-directory/role-based-access-built-in-roles.md) in Azure rechten toewijzen aan gebruikers. Overweeg het gebruik van *Storage Account Inzender* voor cloudoperators die nodig zijn voor het beheren van storage-accounts en *klassieke Storage Account Inzender* rol voor het beheren van klassieke opslagaccounts. Voor cloudoperators die nodig zijn voor het beheren van virtuele machines en storage-account, kunt toevoegen aan *Virtual Machine Contributor* rol.

Organisaties die niet afgedwongen door toegangsbeheer gegevens dankzij het gebruik van mogelijkheden, zoals RBAC mogelijk meer bevoegdheden beschikt dan nodig is voor hun gebruikers geven. Dit kan leiden tot inbreuk op gegevens door sommige gebruikers toegang hebben tot gegevens die ze in eerste instantie mag geen te laten.

U kunt meer informatie over Azure RBAC lezen van het artikel [rollen gebaseerd toegangsbeheer](../active-directory/role-based-access-control-configure.md).

## <a name="encrypt-azure-virtual-machines"></a>Virtuele Machines van Azure versleutelen
Voor veel organisaties [gegevensversleuteling in rust](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) is een verplichte stap naar privacy, naleving en gegevens onafhankelijkheid van gegevens. Azure Disk Encryption kunnen IT-beheerders voor het versleutelen van de schijven voor Windows en Linux IaaS virtuele Machine (VM). Azure Disk Encryption maakt gebruik van de branche standaard BitLocker-functie van Windows en de functie DM-Crypt van Linux voor volumeversleuteling voor het besturingssysteem en de gegevensschijven.

U kunt gebruikmaken van Azure Disk Encryption om te helpen beveiligen en bescherming van uw gegevens om te voldoen aan uw organisatie beveiligings- en nalevingsvereisten. Organisaties moeten ook rekening houden met behulp van versleuteling op risico's met betrekking tot niet-geautoriseerde gegevenstoegang verminderen. Het is ook raadzaam stations voordat gevoelige gegevens schrijven naar ze te versleutelen.

Zorg ervoor dat voor het versleutelen van uw VM gegevensvolumes en opstartvolume beveiligen van gegevens in rust in uw Azure storage-account. De versleutelingssleutels en geheimen beveiligen door gebruik te [Azure Key Vault](../key-vault/key-vault-whatis.md).

Overweeg de versleuteling van de volgende aanbevolen procedures voor de lokale Windows-Servers:

* Gebruik [BitLocker](https://technet.microsoft.com/library/dn306081.aspx) voor gegevensversleuteling
* Sla de herstelgegevens in AD DS.
* Als er zorgen zijn dat BitLocker-sleutels zijn aangetast, is het raadzaam dat u ofwel formatteren van de schijf verwijderen van alle exemplaren van de BitLocker-metagegevens uit het station of ontsleutelen en te versleutelen van het hele station opnieuw.

Organisaties die niet gegevensversleuteling afdwingen vaker worden blootgesteld aan problemen met de gegevensintegriteit, zoals schadelijke of rogue gebruikers gegevens stelen en onbevoegde toegang tot gegevens in een versleutelde indeling accounts waarmee is geknoeid. Naast deze risico's, moeten bedrijven die voldoen aan de regelgeving vanuit de sector, bewijzen dat zij zijn toegewijd en de juiste beveiligingscontroles gebruikt voor het verbeteren van de beveiliging van gegevens.

U kunt meer informatie over Azure Disk Encryption lezen van het artikel [Azure Disk Encryption for Windows en Linux IaaS VM's](azure-security-disk-encryption.md).

## <a name="use-hardware-security-modules"></a>Gebruik van Hardware Security Modules
Geheime codes versleuteling-oplossingen gebruiken om gegevens te versleutelen. Het is daarom essentieel dat deze sleutels veilig worden opgeslagen. Sleutelbeheer wordt een integraal onderdeel van gegevensbeveiliging, aangezien deze zal worden gebruikt voor het opslaan van de geheime codes die worden gebruikt om gegevens te versleutelen.

Maakt gebruik van Azure schijfversleuteling [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) om te controleren en beheren van schijf versleutelingssleutels en geheimen in uw abonnement sleutelkluis, terwijl u ervoor te zorgen dat alle gegevens in de virtuele machine-schijven zijn versleuteld in rust in uw Azure-opslag. Gebruik Azure Sleutelkluis sleutels en gebruik van beleidsregels controleren.

Er zijn veel intrinsieke risico's die betrekking hebben op de juiste beveiligingscontroles niet hoeft in voor het beveiligen van de geheime codes die zijn gebruikt om uw gegevens te versleutelen. Als aanvallers toegang tot de geheime codes hebt, worden ze kunnen de gegevens ontsleutelen en mogelijk toegang hebben tot vertrouwelijke informatie.

U meer informatie over algemene aanbevelingen voor certificaatbeheer in Azure door te lezen van het artikel [Certificate Management in Azure: tips](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Lees voor meer informatie over Azure Sleutelkluis [aan de slag met Azure Key Vault](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>Met beveiligde werkstations beheren
Aangezien de meeste van deze aanvallen als doel de eindgebruiker, wordt het eindpunt in een van de primaire punten van een aanval. Als een aanvaller wordt het eindpunt aangetast, kan hij gebruikmaken van referenties om toegang krijgen tot gegevens van de organisatie van de gebruiker. De meeste eindpunt aanvallen zijn kunnen profiteren van het feit dat eindgebruikers beheerders in hun lokale werkstations.

U kunt deze risico's verminderen met behulp van een beveiligd beheerwerkstation. Het is raadzaam dat u een [Privileged Access Workstations (PAW)](https://technet.microsoft.com/library/mt634654.aspx) om de kwetsbaarheid in werkstations te beperken. Deze beveiligde beheerwerkstations kunnen helpen u bij het beperken van enkele van deze aanvallen zodat uw gegevens is veiliger. Zorg ervoor dat PAW gebruiken om te beperken en uw werkstation vergrendelen. Dit is een belangrijke stap in het hoge beveiliging garantie voor gevoelige accounts, taken en gegevensbeveiliging te bieden.

Gebrek van endpoint protection mogelijk risico voor uw gegevens, zorg ervoor dat u het beveiligingsbeleid afdwingen op alle apparaten die worden gebruikt voor gegevens, ongeacht de locatie van gegevens (cloud of on-premises) gebruiken.

U kunt meer informatie over de juiste rechten toegang tot werkstation met het lezen van het artikel [bevoegde toegang beveiligen](https://technet.microsoft.com/library/mt631194.aspx).

## <a name="enable-sql-data-encryption"></a>SQL-gegevensversleuteling inschakelt
[Azure SQL Database transparante gegevensversleuteling](https://msdn.microsoft.com/library/dn948096.aspx) (TDE) beschermt tegen de dreiging van schadelijke activiteiten door te voeren realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogbestanden in rust zonder wijzigingen in de toepassing.  TDE versleutelt de opslag van een volledige database met behulp van een symmetrische sleutel, naam van de databaseversleutelingssleutel.

Zelfs als de volledige opslag wordt gecodeerd, is het erg belangrijk voor het versleutelen van uw database zelf ook. Dit is een implementatie van de verdediging in de diepte benadering voor gegevensbescherming. Als u [Azure SQL Database](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) en bescherming van gevoelige gegevens zoals creditcard of burgerservicenummers wilt, kunt u databases met FIPS 140-2-gevalideerde 256 bits AES-versleuteling dat voldoet aan de vereisten van veel industrienormen (bijv, HIPAA, PCI) coderen.

Het is belangrijk te begrijpen of bestanden betrekking heeft op [uitbreiding van de buffer](https://msdn.microsoft.com/library/dn133176.aspx) (BPE) worden niet versleuteld wanneer een database worden versleuteld met TDE. Moet u versleuteling op bestandsniveau hulpprogramma's zoals BitLocker of de [Encrypting File System](https://technet.microsoft.com/library/cc700811.aspx) (EFS) voor de BPE gerelateerde bestanden.

Sinds een geautoriseerde gebruiker zoals een beveiligingsbeheerder of een databasebeheerder toegang tot de gegevens zelfs als de database is versleuteld met TDE, moet u ook Volg de onderstaande aanbevelingen:

* SQL-verificatie op het databaseniveau van de
* Azure AD-verificatie met behulp van RBAC-rollen
* Gebruikers en toepassingen moeten afzonderlijke accounts voor verificatie gebruiken. Deze manier kunt u de machtigingen voor gebruikers en toepassingen beperken en de risico's van schadelijke activiteiten te verminderen
* Beveiliging van de database op gebruikersniveau implementeren met behulp van de vaste databaserollen (zoals db_datareader of db_datawriter), of u kunt aangepaste rollen voor uw toepassing expliciete machtigen om objecten in de geselecteerde database maken

Organisaties die geen gebruik van niveau versleuteling van de database maakt mogelijk gevoeliger voor aanvallen kunnen leiden die gegevens in SQL-databases.

U meer informatie over SQL TDE versleuteling door te lezen van het artikel [Transparent Data Encryption met Azure SQL Database](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="protect-data-in-transit"></a>De gegevens onderweg te beschermen
Beveiligen van gegevens die onderweg moet essentieel onderdeel van uw strategie voor gegevensbescherming. Omdat de gegevens wordt heen en weer worden verplaatst vanaf verschillende locaties, is de algemene aanbeveling dat u altijd SSL/TLS-protocollen gebruiken voor het uitwisselen van gegevens op verschillende locaties. In sommige gevallen wilt u mogelijk het hele communicatiekanaal tussen uw on-premises en cloud isoleren infrastructuur met behulp van een virtueel particulier netwerk (VPN).

Gegevens verplaatsen tussen uw on-premises infrastructuur en Azure, moet u passende beveiligingsmaatregelen zoals HTTPS- of VPN.

Voor organisaties die nodig zijn om de toegang van meerdere werkstations die zich on-premises naar Azure te beveiligen, gebruikt u [Azure site-naar-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md).

Voor organisaties die nodig zijn om de toegang van een werkstation zich on-premises naar Azure te beveiligen, gebruikt u [punt-naar-Site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Grotere gegevenssets kan worden verplaatst via een speciale snelle WAN-verbinding, zoals [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Als u kiest voor ExpressRoute, kunt u ook de gegevens op het niveau van de toepassing met behulp van coderen [SSL/TLS](https://support.microsoft.com/kb/257591) of andere protocollen voor extra beveiliging.

Als u met Azure Storage via de Azure-Portal communiceert, worden alle transacties plaatsvinden via HTTPS. [REST API voor Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx) via HTTPS kan ook worden gebruikt om te communiceren met [Azure Storage](https://azure.microsoft.com/services/storage/) en [Azure SQL Database](https://azure.microsoft.com/services/sql-database/).

Organisaties die niet voldoen aan het beveiligen van gegevens die onderweg zijn vatbaar voor [man-in-the-middle-aanvallen](https://technet.microsoft.com/library/gg195821.aspx), [afgeluisterd](https://technet.microsoft.com/library/gg195641.aspx) en sessiehijacking. Deze aanvallen kunnen worden de eerste stap bij het toegang krijgen tot vertrouwelijke gegevens.

U kunt meer informatie over Azure VPN-optie lezen van het artikel [Planning en ontwerp voor VPN-Gateway](../vpn-gateway/vpn-gateway-plan-design.md).

## <a name="enforce-file-level-data-encryption"></a>Bestand niveau gegevensversleuteling afdwingen
Een andere beschermingslaag die het niveau van beveiliging voor uw gegevens kunt verhogen met het versleutelen van het bestand zelf, ongeacht de locatie van het bestand.

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) maakt gebruik van beleidsregels voor versleuteling, identiteit en verificatie om te helpen uw bestanden en e-mail te beveiligen. Azure RMS werkt op meerdere apparaten, telefoons, tablets en pc's voor beveiliging binnen uw organisatie en buiten uw organisatie. Deze mogelijkheid is mogelijk omdat Azure RMS wordt toegevoegd een beveiligingsniveau dat aan de gegevens gekoppeld blijft, zelfs wanneer deze de fysieke grenzen van uw organisatie.

Wanneer u Azure RMS gebruikt om uw bestanden te beveiligen, gebruikt u industriestandaard cryptografie met volledige ondersteuning van [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Wanneer u Azure RMS-gegevensbeveiliging gebruikmaken, hebt u de zekerheid dat de beveiliging blijft van toepassing op het bestand, zelfs als deze wordt gekopieerd naar opslag die niet onder het beheer van IT, zoals een cloudopslagservice. Dezelfde vindt plaats voor bestanden die worden gedeeld via e-mail, het bestand is beveiligd als bijlage aan een e-mailbericht met instructies voor het openen van de beveiligde bijlage.

Bij het plannen van de overstap op Azure RMS raden we het volgende:

* Installeer de [app RMS sharing](https://technet.microsoft.com/library/dn339006.aspx). Deze app kan worden geïntegreerd met Office-toepassingen door installatie van een Office-invoegtoepassing zodat gebruikers kunnen bestanden eenvoudig rechtstreeks beveiligen.
* Configureer toepassingen en services voor ondersteuning van Azure RMS
* Maak [aangepaste sjablonen](https://technet.microsoft.com/library/dn642472.aspx) die overeenstemming met uw zakelijke vereisten. Bijvoorbeeld: een sjabloon voor bovenste geheime gegevens die moet worden toegepast op alle bovenste geheim gerelateerde e-mailberichten.

Organisaties die zich op zwakke [gegevensclassificatie](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) en bestandsbeveiliging vatbaar voor lekken van gegevens. Zonder de juiste bestandsbeveiliging niet organisaties kunnen zakelijke inzichten te verkrijgen, op misbruik kunt controleren en te voorkomen dat schadelijke toegang tot bestanden.

U meer informatie over Azure RMS door te lezen van het artikel [aan de slag met Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx).
