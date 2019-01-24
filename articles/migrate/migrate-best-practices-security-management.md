---
title: Aanbevolen procedures voor het beveiligen en beheren van werkbelastingen naar Azure gemigreerd | Microsoft Docs
description: Na de migratie naar Azure, get-aanbevolen procedures voor het besturingssysteem, beheren en beveiligen van uw gemigreerde workloads.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/08/2018
ms.author: raynew
ms.openlocfilehash: b2b5d62c54fbfdef8a5e448a089800eedcb66d07
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54827496"
---
# <a name="best-practices-for-securing-and-managing-workloads-migrated-to-azure"></a>Aanbevolen procedures voor het beveiligen en beheren van werkbelastingen naar Azure gemigreerd

Als u plannen en ontwerpen voor migratie, naast nadenken over de migratie zelf, moet u rekening houden met het model voor beveiliging en beheer in Azure na de migratie. Dit artikel beschrijft de planning en aanbevolen procedures voor het beveiligen van uw Azure-implementatie te migreren, en voor lopende taken om uw implementatie uitgevoerd op het niveau van een optimale. 

> [!IMPORTANT]
> De aanbevolen procedures en meningen in dit artikel beschreven zijn gebaseerd op het Azure-platform en service-functies die beschikbaar zijn op het moment van schrijven. Functies en mogelijkheden veranderen verloop van tijd.

## <a name="secure-migrated-workloads"></a>Beveiliging van de gemigreerde workloads

Na de migratie is de belangrijkste taak voor het beveiligen van de gemigreerde werkbelastingen van interne en externe bedreigingen. Deze aanbevolen procedures helpen u om dat te doen:

- [Werken met Azure Security Center](#best-practice-follow-azure-security-center-recommendations): Meer informatie over het werken met de bewaking, evaluaties en aanbevelingen die door Azure Security Center
- [Uw gegevens versleutelen](#best-practice-encrypt-data): Get-aanbevolen procedures voor het versleutelen van uw gegevens in Azure.
- [Instellen van anti-malware](#best-practice-protect-vms-with-antimalware): Beveilig uw virtuele machines tegen schadelijke software en aanvallen.
- [Web-apps beveiligen](#best-practice-secure-web-apps): Gevoelige gegevens in gemigreerde web-apps veilig te houden.
- [Bekijk de abonnementen](#best-practice-review-subscriptions-and-resource-permissions): Controleer of wie toegang heeft tot uw Azure-abonnementen en resources na de migratie.
- [Werken met Logboeken](#best-practice-review-audit-and-security-logs): Bekijk uw Azure controlebeleid en beveiligingslogboek Logboeken regelmatig.
- [Bekijk andere beveiligingsfuncties](#best-practice-evaluate-other-security-features): Begrijpen en beoordelen van geavanceerde beveiligingsfuncties die Azure biedt.

## <a name="best-practice-follow-azure-security-center-recommendations"></a>Aanbevolen: Volg Azure Security Center aanbevelingen

Microsoft werkt hard om ervoor te zorgen dat Azure tenantbeheerders kunnen de informatie die nodig is om in te schakelen van beveiligingsfuncties die workloads te tegen aanvallen beschermen.  Azure Security Center biedt geïntegreerd beveiligingsbeheer. In het Security Center, kunt u beveiligingsbeleid toepassen voor workloads, beperken blootstelling aan bedreigingen, en detecteren en reageren op aanvallen. Security Center analyseert resources en configuraties voor tenants van Azure en kunt u aanbevelingen voor beveiliging, met inbegrip van:

- **Gecentraliseerd beheer van beleid**: zorg voor naleving van bedrijfsspecifieke en wettelijke beveiligingsvereisten door beveiligingsbeleid voor al uw hybride cloudworkloads centraal te beheren.
- **Continue beveiligingsevaluatie** – Bewaak de beveiligingsstatus van machines, netwerken, opslag- en gegevensservices en toepassingen voor het detecteren van mogelijke beveiligingsproblemen.
- **Uitvoerbare aanbevelingen**: los beveiligingsproblemen op voordat ze kunnen worden misbruikt door aanvallers met op prioriteit gebaseerde en uitvoerbare beveiligingsaanbevelingen.
- **Op prioriteit gebaseerde waarschuwingen en incidenten**: richt u eerst op de meest kritieke bedreigingen met op prioriteit gebaseerde beveiligingswaarschuwingen en -incidenten.

Naast de evaluaties en aanbevelingen biedt het Security Center een aantal andere beveiligingsfuncties die kan worden ingeschakeld voor specifieke resources.

- **Alleen In tijd (JIT) toegang**: Verminder het oppervlak voor netwerkaanvallen met just-in-time, gecontroleerde toegang tot beheerpoorten op Azure Virtual machines.
    - Met VM-RDP-poort 3389 openen op het internet wordt aangegeven dat virtuele machines aan voortdurende slecht actor-activiteit. Azure IP-adressen zijn bekende en hackers voortdurend test ze voor aanvallen op 3389 poorten openen. 
    - Just-in-tijd gebruikt netwerkbeveiliging groepen (nsg's) en binnenkomende regels die grens de hoeveelheid tijd die een specifieke poort geopend is.
    - Met just-in-time is ingeschakeld, controleert Security Center dat een gebruiker op basis van rollen (RBAC) toegang voor schrijven met machtigingen voor toegangsbeheer voor een virtuele machine heeft. Daarnaast Geef regels op voor hoe gebruikers verbinding met virtuele machines maken kunnen. Als machtigingen in orde zijn, een aanvraag voor toegang tot is goedgekeurd en Security Center configureert u nsg's om toe te staan van inkomend verkeer op de geselecteerde poorten voor de hoeveelheid tijd die u opgeeft. Nsg's zijn Ga terug naar de vorige status wanneer het tijd is verlopen.
- **Besturingselementen voor adaptieve toepassingen**: Software en schadelijke software uit de virtuele machines door te bepalen welke apps worden uitgevoerd op deze Blijf gebruiken in de whitelist aan dynamische app.
    - Besturingselementen voor adaptieve toepassingen kunnen u witte lijst apps en voorkomen dat rogue-gebruikers of beheerders software voor niet-goedgekeurde of gaan apps installeren op uw virtuele machines.
    - U kunt blokkeren of waarschuwing wordt geprobeerd schadelijke apps worden uitgevoerd, te voorkomen dat ongewenste of schadelijke apps en voldoen aan de app-beveiligingsbeleid van uw organisatie.
- **Bestandsintegriteit controleren**: Zorg ervoor dat de integriteit van de bestanden die worden uitgevoerd op virtuele machines.
    - U hoeft niet te installeren software om te leiden tot problemen met de virtuele machine.  Wijzigen van een bestand kan ook leiden tot verslechtering van de virtuele machine mislukt of prestaties.  Het bestand integriteit bewaking onderzoekt systeembestanden en instellingen van het register voor wijzigingen en waarschuwt u als er iets is bijgewerkt.
    - Security Center aangeraden welke bestanden u moeten controleren.


**Meer informatie:**

- [Meer informatie](https://docs.microsoft.com/azure/security-center/security-center-intro) over Azure Security Center.
- [Meer informatie](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) over just-in-time-VM-toegang.
- [Meer informatie over](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application) toepassen van besturingselementen voor adaptieve toepassingen.
- [Aan de slag](https://docs.microsoft.com/azure/security-center/security-center-file-integrity-monitoring) met bestandsintegriteit controleren.


## <a name="best-practice-encrypt-data"></a>Aanbevolen: Gegevens versleutelen 

Versleuteling is een belangrijk onderdeel van de procedures voor beveiliging op Azure. Ervoor te zorgen dat versleuteling is ingeschakeld, op alle niveaus helpt te voorkomen dat onbevoegden toegang krijgen tot gevoelige gegevens, met inbegrip van gegevens in-transit en in rust. 

### <a name="encryption-for-iaas"></a>Versleuteling voor IaaS

- **Virtuele machines**: U kunt Azure Disk Encryption voor virtuele machines gebruiken voor het versleutelen van uw Windows- en Linux IaaS-VM-schijven.
    - Schijfversleuteling maakt gebruik van BitLocker voor Windows en DM-Crypt voor Linux om volumeversleuteling voor het besturingssysteem en gegevensschijven.
    - Kunt u een versleutelingssleutel gemaakt door Azure, of kunt u uw eigen versleutelingssleutels, beveiligd in Azure Key Vault opgeven. 
    - Met schijfversleuteling, IaaS VM-gegevens in rust (op schijf) is beveiligd en tijdens het opstarten van de virtuele machine. 
    - Azure Security Center wordt u gewaarschuwd als er virtuele machines die niet zijn versleuteld.
- **Opslag**: Beveiligen aan de rest-gegevens opgeslagen in Azure storage.
    - Gegevens die zijn opgeslagen in Azure storage-accounts kan worden versleuteld met Microsoft gegenereerde AES-sleutels die FIPS 140-2-compatibele zijn, of u kunt uw eigen sleutels gebruiken.
    - Storage-Serviceversleuteling is ingeschakeld voor alle nieuwe en bestaande opslagaccounts en kan niet worden uitgeschakeld.


### <a name="encryption-for-paas"></a>Versleuteling voor PaaS

In tegenstelling tot IaaS waar u uw eigen virtuele machines en infrastructuur beheren, in een PaaS wordt model platform en de infrastructuur beheerd door de provider, zodat u zich kunt richten op de kernlogica van de app en mogelijkheden. Met veel verschillende soorten PaaS-services, worden elke service afzonderlijk om veiligheidsredenen geëvalueerd. Bijvoorbeeld: we gaan kijken hoe we kunnen versleuteling inschakelen voor Azure SQL Database.

- **Altijd versleuteld**: Wizard altijd versleuteld in SQL Server Management Studio gebruiken om gegevens in rust te beveiligen.
    - U maken Always Encrypted-sleutel voor het versleutelen van gegevens van afzonderlijke kolom.
    - Altijd versleutelde sleutels kunnen worden opgeslagen als gecodeerde in de metagegevens van een database of opgeslagen in vertrouwde sleutel, zoals Azure Key Vault.
    - App-wijzigingen waarschijnlijk nodig om deze functie te gebruiken.
- **Transparante gegevensversleuteling (TDE)**: De Azure SQL Database met realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden in rust beveiligen.
    - TDE kunt versleutelingsactiviteiten moet plaatsvinden zonder wijzigingen in de app-laag.
    - TDE versleutelingssleutels geleverd door Microsoft kunt gebruiken, of u uw eigen sleutels met Bring Your Own Key-ondersteuning kunt bieden.


**Meer informatie:**
- [Meer informatie over](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) Azure Disk Encryption voor IaaS-VM's.
- [Schakel](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-windows) versleuteling voor IaaS Windows-VM's.
- [Meer informatie over](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) Azure Storage-Serviceversleuteling voor data-at-rest.
- [Lezen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) een overzicht van altijd versleuteld.
- [Meer informatie over](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?view=sql-server-2017) TDE voor Azure SQL-Database.
- [Meer informatie over](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql) TDE met Bring Your Own Key.

## <a name="best-practice-protect-vms-with-antimalware"></a>Aanbevolen: Virtuele machines beveiligen met anti-malware

Oudere Azure gemigreerde virtuele machines mogelijk is met name niet het juiste niveau van anti-malware geïnstalleerd.  Azure biedt een gratis endpoint-oplossing waarmee u kunt virtuele machines beschermen tegen virussen, spyware en andere schadelijke software.
- Microsoft Antimalware voor Azure genereert waarschuwingen wanneer bekende schadelijke of ongewenste software probeert zichzelf te installeren.
- Het is een afzonderlijke agent-oplossing die wordt uitgevoerd op de achtergrond zonder menselijke tussenkomst.
- U kunt eenvoudig virtuele machines die geen endpoint Protection in uitgevoerd en Microsoft Antimalware installeren indien nodig in Azure Security Center identificeren.

![Anti-malware voor virtuele machines](./media/migrate-best-practices-security-management/antimalware.png)
*anti-malware voor virtuele machines*

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/security/azure-security-antimalware) Microsoft Antimalware.

## <a name="best-practice-secure-web-apps"></a>Aanbevolen: Beveiligde web-apps

Gemigreerde web-apps te maken krijgt een aantal problemen:

- De meeste verouderde webtoepassingen vaak dat gevoelige informatie in de configuratiebestanden.  Bestanden met dergelijke informatie kunnen beveiligingsproblemen met zich mee voor opleveren wanneer apps worden back-ups, of wanneer de app-code in of buiten het besturingselement is ingeschakeld.
- Bovendien wanneer u web-apps die zich bevinden in een virtuele machine migreert, verplaatst u waarschijnlijk die machine uit een on-premises netwerk en firewall-beveiligde omgeving naar een omgeving die geconfronteerd van het internet. U moet ervoor zorgen dat u een oplossing die hetzelfde als uw on-premises resources van de beveiliging doet instelt.


Azure biedt een aantal oplossingen:

- **Azure Key Vault**: Web app-ontwikkelaars duurt vandaag nog om ervoor te zorgen dat gevoelige informatie van deze bestanden is niet gelekt. Een methode voor het beveiligen van gegevens is het uitpakken van bestanden en plaatsen deze in een Azure Key Vault.
    - U kunt Key Vault gebruiken voor het opslaan van appgeheimen centraliseren en de verdeling ervan bepalen. Dit voorkomt de noodzaak voor het opslaan van informatie over beveiliging in app-bestanden.
    - Apps kunnen toegang tot informatie over beveiliging in de kluis met behulp van URI's, zonder aangepaste code.
    - Azure Key Vault kunt u toegang via Azure beveiligingsmechanismen vergrendelen en een naadloos implementeren 'rolling sleutels'. Microsoft niet kan zien of extraheren van uw gegevens.
- **App Service-omgeving**: Als een app die u migreert extra beveiliging moet, kunt u overwegen een App Service-omgeving en de Web Application Firewall ter bescherming van de app-resources toe te voegen.
    - De Azure App Service-omgeving biedt een volledig geïsoleerde en toegewezen omgeving waarin aan de actieve App Service-apps, zoals Windows en Linux-web-apps, Docker-containers, mobiele apps en functies.
    - Dit is handig voor apps die zeer grote schaal zijn, isolatie en beveiligde netwerk toegang tot of hoog geheugengebruik vereisen
- **Web Application Firewall**: Een functie van Application Gateway die gecentraliseerde beveiliging voor web-apps biedt.
    - Het beveiligt web-apps zonder codewijzigingen back-end.
    - Deze beschermt meerdere webtoepassingen op hetzelfde moment achter een application gateway.
    - De Web application firewall kan worden bewaakt met behulp van Azure Monitor en is geïntegreerd in Azure Security Center.



![Web-apps beveiligen](./media/migrate-best-practices-security-management/web-apps.png)
*Azure Key Vault*

**Meer informatie:**

- [Bekijk een overzicht](https://docs.microsoft.com/azure/key-vault/key-vault-overview) van Azure Key Vault.
- [Meer informatie over](https://docs.microsoft.com/azure/application-gateway/waf-overview) Web application firewall.
- [Een inleiding](https://docs.microsoft.com/azure/app-service/environment/intro) naar App Service-omgevingen.
- [Meer informatie over het](https://docs.microsoft.com/azure/key-vault/tutorial-web-application-keyvault) configureren van een web-app voor het lezen van geheimen uit Key Vault.
- [Meer informatie over](https://docs.microsoft.com/azure/application-gateway/waf-overview) Web Application Firewall

## <a name="best-practice-review-subscriptions-and-resource-permissions"></a>Aanbevolen: Controle-abonnementen en -machtigingen voor resources

Migreer uw workloads en voer ze uit in Azure, wordt de medewerkers met toegang tot de workload verplaatsen. Uw beveiligingsteam moet controleren of de toegang tot uw Azure-tenant en resource-groepen op gezette tijden. Azure biedt een aantal oplossingen voor identiteits- en toegangsbeveiliging, met inbegrip van op rollen gebaseerd toegangsbeheer (RBAC) voor het verlenen van machtigingen voor toegang tot Azure-resources.

- RBAC toegewezen toegangsmachtigingen voor beveiligings-principals. Beveiligings-principals stellen gebruikers, groepen (een set van gebruikers), service-principals (identiteit die wordt gebruikt door apps en -services) en beheerde identiteiten (een Azure Active Directory-identiteit automatisch beheerd door Azure).
- RBAC kunt rollen toewijzen aan beveiligingsprincipes, zoals eigenaar, Inzender en lezer en rol definities (een verzameling van machtigingen) die de bewerkingen die kunnen worden uitgevoerd door de rollen worden gedefinieerd.
- RBAC kunt bereiken die de grens voor een rol ook instellen. Bereik kan worden ingesteld op verschillende niveaus, met inbegrip van een beheergroep, abonnement, resourcegroep of resource
- Zorg ervoor dat beheerders met toegang tot Azure alleen toegang tot bronnen die u wilt toestaan.  Als de vooraf gedefinieerde rollen in Azure niet gedetailleerd genoeg zijn, kunt u aangepaste rollen om te scheiden en beperken van machtigingen voor toegang.

![Toegangsbeheer](./media/migrate-best-practices-security-management/subscription.png)
*Access control - IAM*

**Meer informatie:**

- [Over](https://docs.microsoft.com/azure/role-based-access-control/overview) RBAC.
- [Informatie over](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) voor het beheren van toegang met RBAC en Azure portal.
- [Meer informatie over](https://docs.microsoft.com/azure/role-based-access-control/custom-roles) aangepaste rollen.

## <a name="best-practice-review-audit-and-security-logs"></a>Aanbevolen: Logboeken voor controle en beveiliging controleren

Azure Active Directory (AD) biedt activiteitenlogboeken die worden weergegeven in Azure Monitor. De logboeken vastleggen van de bewerkingen die worden uitgevoerd in Azure tenants, wanneer deze zijn opgetreden en wie ze uitgevoerd. 

- Logboeken voor controle van weergeven de geschiedenis van taken in de tenant. Aanmelden activiteitenlogboeken weergeven die de taken uitgevoerd. 
- Toegang tot beveiligingsrapporten, is afhankelijk van uw Azure AD-licentie. In de gratis en Basic dat u een lijst met riskante gebruikers en -aanmeldingen. U kunt informatie over de gebeurtenis ophalen onderliggende in Premium 1 en 2 van de Premium-edities.
- U kunt activiteitenlogboeken versturen naar een aantal eindpunten voor langetermijnretentie en inzicht in gegevens.
- Kunt u een gebruikelijk om te controleren van de logboeken of integreren van uw security information en event management (SIEM) hulpprogramma's om te controleren automatisch afwijkingen.  Als u Premium 1 of 2 niet gebruikt, moet u een hoop analysewerk doen uzelf of uw SIEM-systeem.  Analyse bevat riskante aanmeldingen en evenementen en andere aanvalspatronen gebruiker zoekt.


![Gebruikers en groepen](./media/migrate-best-practices-security-management/azure-ad.png)
*Azure AD-gebruikers en groepen*

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor) Azure AD-activiteitenlogboeken in Azure Monitor.
- [Meer informatie over het](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs) controleactiviteitenrapporten in de Azure AD-portal.

## <a name="best-practice-evaluate-other-security-features"></a>Aanbevolen: Andere beveiligingsfuncties evalueren

Azure biedt een aantal andere beveiligingsfuncties die opties zijn voor geavanceerde beveiliging. Sommige van deze aanbevolen procedures moet invoegtoepassingslicenties en premium-opties.

- **Azure AD-beheereenheden (AU) implementeren**: Beheertaken voor de ondersteuning van medewerkers delegeren lastig om met alleen eenvoudige toegangsbeheer in Azure.  Geeft toegang tot het personeel van ondersteuning voor het beheer van alle groepen in Azure AD mogelijk niet de ideale methode voor de beveiliging van de organisatie.  Met behulp van Australië, kunt u Azure-resources scheiden in containers op dezelfde manier naar on-premises organisatie-eenheden (OE).  De AU-beheerder moet een Azure AD premium-licentie hebben voor het gebruik van AU. [Meer informatie](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-administrative-units).
- **Gebruik multi-factor authentication (MFA)**: Als u een premium Azure AD-licentie kunt u inschakelen en afdwingen van MFA op uw beheerdersaccounts hebt. Phishing is de meest voorkomende manier dat accounts referenties zijn aangetast.  Zodra een actor slecht account beheerdersreferenties heeft, moet u er is geen stoppen van de vérstrekkend acties, zoals het verwijderen van al uw resourcegroepen. U kunt MFA instellen op een aantal manieren, inclusief e-mail, authenticator-app en tekstberichten van de telefoon. Als beheerder kunt u de minste firewallverbinding optie selecteren. MFA kan worden geïntegreerd met threat analytics en beleid voor voorwaardelijke toegang om te vereisen willekeurig een MFA-controle reageren. Meer informatie over [beveiligingsrichtlijnen](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication-security-best-practices), en [over het instellen van](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication-security-best-practices) MFA.
- **Implementeren van voorwaardelijke toegang**: In de meeste grootte voor kleine en middelgrote organisaties zich Azure-beheerders en het ondersteuningsteam bevindt waarschijnlijk in een één geografisch gebied. De meeste aanmeldingen, in dit geval zijn afkomstig van de dezelfde gebieden. Als de IP-adressen van deze locaties vrij statisch zijn, is het handig dat u aanmeldingen van de beheerder van buiten deze gebieden al dan niet mogen zien. Zelfs in een gebeurtenis waarbij een externe slecht actor wordt aangetast referenties van een beheerder, kunt u beveiligingsfuncties, zoals voorwaardelijke toegang in combinatie met MFA om te voorkomen dat aanmelding vanaf externe locaties of vervalste locaties van willekeurige IP-adressen te implementeren. [Meer informatie](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) over voorwaardelijke toegang en [aanbevolen procedures](https://docs.microsoft.com/azure/active-directory/conditional-access/best-practices) voor voorwaardelijke toegang in Azure AD.
- **Bekijk bedrijfstoepassing machtigingen**: Na verloop van tijd beheerders klikt u op Microsoft en derden koppelingen zonder te weten de invloed ervan op de organisatie. Koppelingen kunnen presenteren toestemming schermen die machtigingen toewijzen aan Azure-apps, en mogelijk om toegang tot Azure AD-gegevens lezen of zelfs volledige toegang tot het beheren van uw hele Azure-abonnement. De apps waarop uw beheerders en gebruikers hebben toegang hebben tot Azure-resources, moet u regelmatig controleren. U moet ervoor zorgen dat deze apps hebben alleen de machtigingen die nodig zijn. Bovendien, per kwartaal of jaar door u kan verzenden naar gebruikers met een koppeling naar app-pagina's zodat ze op de hoogte van de apps waartoe ze toegang tot hun gegevens van de organisatie hebt toegestaan. [Meer informatie](https://docs.microsoft.com/azure/active-directory/manage-apps/application-types) over de toepassingstypen, en [over het besturingselement](https://docs.microsoft.com/azure/active-directory/manage-apps/remove-user-or-group-access-portal) app-toewijzingen in Azure AD.



## <a name="managed-migrated-workloads"></a>Beheerde gemigreerde workloads

In deze sectie we adviseren u enkele aanbevolen procedures voor het beheer van Azure, met inbegrip van:

- [Bronnen beheren](#best-practice-name-resource-groups): Aanbevolen procedures voor Azure-resourcegroepen en resources, met inbegrip van slimme naamgeving, zo wordt voorkomen dat onbedoelde verwijdering, beheer van machtigingen voor resources en effectieve resource tagging.
- [Gebruik blauwdrukken](#best-practice-implement-blueprints): Bekijk een kort overzicht over het gebruik van blauwdrukken voor het ontwikkelen en beheren van uw implementatieomgevingen.
- [Bekijk architecturen](#best-practice-review-azure-reference-architectures): Bekijk het voorbeeld van Azure-architecturen voor meer informatie over uit tijdens het ontwikkelen van uw implementaties na de migratie.
- [Instellen van beheergroepen](#best-practice-manage-resources-with-management-groups): Als u meerdere abonnementen hebt, kunt u ze in beheergroepen te verzamelen en governance-instellingen toepassen op deze groepen.
- [Instellen van toegangsbeleid](#best-practice-deploy-azure-policy): Nalevingsbeleid toepassen op uw Azure-resources.
- [Implementeren van een BCDR-strategie](#best-practice-implement-a-bcdr-strategy): Samengesteld een bedrijfscontinuïteit en noodherstel (BCDR) strategie voor zakelijke gegevens veilig, uw omgeving robuuste en resources zodat u kunt blijven en worden uitgevoerd als er uitval optreedt.
- [Virtuele machines beheren](#best-practice-use-managed-disks-and-availability-sets): Groep virtuele machines in beschikbaarheidsgroepen voor tolerantie en hoge beschikbaarheid. Beheerde schijven gebruiken voor het gebruiksgemak van VM-schijf en beheer.
- [Gebruik van resources bewaken](#best-practice-monitor-resource-usage-and-performance): Diagnostische logboekregistratie inschakelen voor Azure-resources, waarschuwingen en playbooks voor het oplossen van proactieve bouwen en gebruiken van het Azure-dashboard voor een overzicht van uw implementatie integriteit en status.
- [Ondersteuning en -updates beheren](#best-practice-manage-updates): Inzicht in uw Azure-ondersteuningsplan en hoe u deze implementeert, aanbevolen procedures voor het beveiligen van virtuele machines up-to-date en put processen voor wijzigingsbeheer ophalen.


## <a name="best-practice-name-resource-groups"></a>Aanbevolen: De naam van resourcegroepen

Ervoor te zorgen dat uw resourcegroepen betekenisvolle namen die beheerders hebben en ondersteuning teamleden eenvoudig kunnen herkennen en navigeer zal zijn aanzienlijk verbeterd, productiviteit en efficiëntie.
- Het is raadzaam om de volgende naamgevingsregels voor Azure.
- Als u bij het synchroniseren van uw on-premises AD DS naar Azure AD met behulp van AD Connect, kunt u die overeenkomt met de namen van de beveiliging groepen on-premises naar de namen van resourcegroepen in Azure.

![Naamgeving van](./media/migrate-best-practices-security-management/naming.png)
*naamgeving van de Resource-groep*


**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) naamconventies

## <a name="best-practice-implement-delete-locks-for-resource-groups"></a>Aanbevolen: Implementatie verwijderen vergrendelingen voor resourcegroepen

Het laatste wat dat u nodig is voor een resourcegroep niet meer zichtbaar omdat het per ongeluk is verwijderd. Het is raadzaam om de delete-vergrendelingen te implementeren, zodat dit gebeurt.


![Verwijderen van vergrendelingen](./media/migrate-best-practices-security-management/locks.png)
*vergrendelingen verwijderen*

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) vergrendelen resources om onverwachte wijzigingen te voorkomen.


## <a name="best-practice-understand-resource-access-permissions"></a>Aanbevolen: Informatie over machtigingen voor toegang tot resources 

De eigenaar van een abonnement heeft toegang tot alle resourcegroepen en resources in uw abonnement.
- Personen spaarzaam toevoegen aan deze waardevolle toewijzing. Informatie over de implicaties van deze typen machtigingen is belangrijk in uw omgeving veilig en stabiel blijven.
- Zorg ervoor dat u plaatst resources in groepen van de juiste resources:
    - Stem resources met een vergelijkbare levensduur samen. In het ideale geval moet u mag niet een resource verplaatsen wanneer u nodig hebt om een hele resourcegroep te verwijderen.
    - Resources die ondersteuning bieden voor een functie of een werkbelasting moeten samen worden geplaatst voor vereenvoudigd beheer.

**Meer informatie:**

- [Meer informatie over](https://azure.microsoft.com/blog/organizing-subscriptions-and-resource-groups-within-the-enterprise/) ordenen van abonnementen en resourcegroepen.

## <a name="best-practice-tag-resources-effectively"></a>Aanbevolen: Toevoegen aan resources effectief

Vaak wordt niet met behulp van alleen een Resourcegroepnaam met betrekking tot de resources behoren voldoende metagegevens voor een effectieve implementatie van de mechanismen zoals interne facturering of informatiebeveiligingsbeheer binnen een abonnement.
- Als een best practice, moet u Azure-tags toe te voegen nuttige metagegevens die kan worden opgevraagd en gerapporteerd op. 
- Tags bieden een manier logisch om resources te organiseren met eigenschappen die u definieert.  Labels kunnen rechtstreeks worden toegepast op resourcegroepen of resources.
- Labels kunnen worden toegepast op een resourcegroep of afzonderlijke resources. Groep resourcetags worden niet overgenomen door de resources in de groep.
- U kunt automatiseren met behulp van Powershell of Azure Automation, of tag afzonderlijke groepen en resources taggen. -benadering of een selfservice-tagging.  Als u een aanvraag en huidige beheersysteem wijzigen, kunt u eenvoudig de gegevens in de aanvraag voor het vullen van uw bedrijfsspecifieke resourcetags gebruiken.


![Labels](./media/migrate-best-practices-security-management/tagging.png)
*Tagging*

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) tagging en beperkingen taggen.
- [Beoordeling](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#powershell) PowerShell en CLI-voorbeelden voor het instellen van tags en tags wilt toepassen vanuit een resourcegroep op de bijbehorende resources.
- [Lezen](http://www.azurefieldnotes.com/2016/07/18/azure-resource-tagging-best-practices/) codering van best practices voor Azure.


## <a name="best-practice-implement-blueprints"></a>Aanbevolen: Blauwdrukken implementeren

Net zoals blauwdruk kan technici en -architecten van een project ontwerpparameters schetsen, inschakelen blauwdrukken voor Azure cloud-architecten en centrale IT-groepen voor het definiëren van een herhaalbare set Azure-resources die worden geïmplementeerd en voldoet aan de standaarden van een organisatie, patronen en vereisten. Met behulp van Azure blauwdrukken development-teams kan snel bouwen en maken van nieuwe omgevingen die voldoen aan compliancevereisten en waarvoor een set ingebouwde onderdelen, zoals netwerken, om de ontwikkeling en levering te versnellen.

- Blauwdrukken gebruiken voor het indelen van de implementatie van resourcegroepen, Azure Resource Manager-sjablonen en toewijzingen van beleid en de rol.
- Azure blauwdrukken worden opgeslagen in een wereldwijd gedistribueerde Azure Cosmos DB. Blauwdrukobjecten worden naar meerdere Azure-regio's gerepliceerd. Replicatie biedt een lage latentie, hoge beschikbaarheid en consistente toegang tot de blauwdruk, ongeacht de regio waaraan een blauwdruk resources implementeert.

**Meer informatie:**

- [Lezen](https://docs.microsoft.com/azure/governance/blueprints/overview) over blauwdrukken.
- [Beoordeling](https://azure.microsoft.com/blog/customizing-azure-blueprints-to-accelerate-ai-in-healthcare/) een voorbeeld van een blauwdruk gebruikt voor het versnellen van AI in de gezondheidszorg.

## <a name="best-practice-review-azure-reference-architectures"></a>Aanbevolen: Bekijk de Azure-referentiearchitecturen

Het bouwen van veilige, schaalbare en beheerbare workloads in Azure kunnen uitdaging zijn.  Met continu wijzigingen, kan het lastig zijn om te blijven met verschillende functies voor een optimale omgeving. Met een verwijzing naar het leren van kan nuttig zijn bij het ontwerpen en migreren van uw workloads.  Azure en Azure partners verschillende voorbeeld-referentiearchitecturen voor verschillende soorten omgevingen hebt gemaakt. Deze voorbeelden zijn bedoeld ideeën die u kunt leren en bouwen op. 

Referentiearchitecturen zijn geordend op scenario. Ze bevatten de aanbevolen procedures en advies voor beheer, beschikbaarheid, schaalbaarheid en beveiliging.
De Azure App Service-omgeving biedt een volledig geïsoleerde en toegewezen omgeving waarin u voor het uitvoeren van App Service, apps, waaronder Windows en Linux-web-apps, Docker-containers, mobiele apps en functies. App Service voegt de kracht van Azure toe aan uw toepassing, met beveiliging, taakverdeling, automatisch schalen en geautomatiseerd beheer. U kunt ook profiteren van de DevOps-mogelijkheden, zoals continue implementatie van Azure DevOps en GitHub, Pakketbeheer, omgevingen, het aangepaste domein en SSL-certificaten. App Service is handig voor apps waarvoor isolatie en beveiligde netwerktoegang en opdrachten die gebruikmaken van grote hoeveelheden geheugen en andere bronnen die nodig zijn om te schalen.
**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/architecture/reference-architectures/) Azure-referentiearchitecturen.
- [Beoordeling](https://docs.microsoft.com/azure/architecture/example-scenario/) voorbeeld van een Azure-scenario's.

## <a name="best-practice-manage-resources-with-management-groups"></a>Aanbevolen: Resources beheren met beheergroepen

Als uw organisatie meerdere abonnementen heeft, moet u voor het beheren van toegang, beleid en naleving voor hen. Azure-beheergroepen bieden een scopeniveau boven abonnementen.

- U organiseren van abonnementen in containers beheergroepen genoemd en governance voorwaarden op hen van toepassing.
- Alle abonnementen in een beheergroep worden automatisch de voorwaarden van de groep management overnemen.
- Beheergroepen bieden geavanceerde beheer op grote schaal, ongeacht welk type abonnementen hebt.
- U kunt bijvoorbeeld een beleid voor beheergroep die beperkt de regio's waarin virtuele machines kunnen worden gemaakt toepassen. Dit beleid wordt vervolgens toegepast op alle beheergroepen, abonnementen en resources onder deze beheergroep.
- U kunt een flexibele structuur van de beheergroepen en abonnementen, uw om resources te organiseren in een hiërarchie voor geïntegreerde policy and access management kunt bouwen.

Het volgende diagram laat een voorbeeld zien van hoe een hiërarchie voor governance kan worden gemaakt met behulp van beheergroepen.

![Beheergroepen](./media/migrate-best-practices-security-management/management-groups.png)
*beheergroepen*

**Meer informatie:**
- [Meer informatie](https://docs.microsoft.com/azure/governance/management-groups/index) over het organiseren van resources in beheergroepen.

## <a name="best-practice-deploy-azure-policy"></a>Aanbevolen: Azure-beleid implementeren

Azure Policy is een service in Azure die u gebruikt om beleidsregels te maken, toe te wijzen en te beheren.

- Beleid afdwingen voor verschillende regels en effecten voor uw resources, zodat deze resources voldoen aan uw bedrijfsnormen en serviceovereenkomsten blijven.
- Azure Policy evalueert uw resources, scannen op die niet voldoen aan uw beleid.
- U kunt bijvoorbeeld een beleid waarmee alleen een specifieke SKU-grootte voor virtuele machines in uw omgeving kunt maken. Azure Policy evalueren deze instelling bij het maken en bijwerken van resources, en bij het scannen van bestaande resources. 
- Azure biedt een aantal ingebouwde beleidsregels die u kunt toewijzen of kunt u uw eigen maken.


![Azure Policy](./media/migrate-best-practices-security-management/policy.png)
*Azure Policy*

**Meer informatie:**
- [Bekijk een overzicht](https://docs.microsoft.com/azure/governance/policy/overview) van Azure Policy.
- [Meer informatie over](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage) maken en beheren van beleidsregels voor het afdwingen van naleving.


## <a name="best-practice-implement-a-bcdr-strategy"></a>Aanbevolen: Implementeer een BCDR-strategie

Plan voor bedrijfscontinuïteit en herstel na noodgevallen (BCDR), is een kritieke oefening die u voltooien moet tijdens de planning voor migratie naar Azure. Juridische voorwaarden bevat uw contract een overmacht-component die Kom allemaal verkleed verplichtingen vanwege een groter kracht zoals orkanen of aardbevingen. Echter, u hebt ook verplichtingen rond een mogelijkheid om ervoor te zorgen dat de services uit te voeren en indien nodig herstellen wanneer na noodgevallen strike, blijven. De mogelijkheid om dit te doen kunt maken of kraken toekomst van uw bedrijf.

Ruime zin is uw BCDR-strategie rekening moet houden:
- **De back-up**: Klik hier voor meer informatie over het beveiligen van uw gegevens zodat u deze eenvoudig herstellen kunt als er uitval optreedt.
- **Herstel na noodgevallen**: Klik hier voor meer informatie over het behouden van uw apps, flexibel en beschikbaar als er uitval optreedt. 

### <a name="azure-resiliency-features"></a>Flexibiliteitsfuncties van Azure
Het Azure-platform biedt een aantal tolerantiefuncties.

- **Regio koppelen**: Azure-regio's voor regionale beveiliging binnen de grenzen van de gegevenslocatie gegevens paren. Azure zorgt ervoor dat de fysieke isolatie tussen gekoppelde regio's, bepaalt de volgorde van het herstel van een gebied in het paar in het geval van een grootschalige storing, systeemupdates afzonderlijk in elke regio implementeert en kunnen functies zoals Azure geografisch redundante opslag te repliceren naar de regioparen.
- **Beschikbaarheidszones**: Beschikbaarheidszones beter beschermd tegen uitval van een hele Azure-datacenter tot stand brengen van fysieke afzonderlijke zones met een Azure-regio. Elke zone heeft een onderscheidende stroomvoorziening, netwerkinfrastructuur en koeling mechanisme.
- **Beschikbaarheidssets**: Beschikbaarheidssets beschermen tegen storingen in een datacenter. U kunt virtuele machines groeperen in beschikbaarheidssets dat ze voor maximaal beschikbare. Binnen elke beschikbaarheidsset, de implementatie van Azure meerdere domeinen met fouten die samen hardware met een gemeenschappelijke voedingsbron en netwerkswitch en updatedomeinen die groep onderliggende hardware die onderhoud kan ondergaan of opnieuw worden opgestart, de onderliggende groep op hetzelfde moment. Bijvoorbeeld: wanneer een werkbelasting verdeeld over virtuele machines van Azure kunt u plaatsen twee of meer virtuele machines voor elke app-laag in een set. Bijvoorbeeld, kunt u de front-end VM's in één set en gegevenslaag virtuele machines in een ander plaatsen. Omdat er slechts één updatedomein wordt elke tegelijk in een set opnieuw opgestart en Azure zorgt ervoor dat virtuele machines in een set worden verdeeld over foutdomeinen, zorgt u ervoor dat niet alle virtuele machines in een set niet op hetzelfde moment.

### <a name="set-up-bcdr"></a>BCDR instellen

Wanneer u migreert naar Azure, is het belangrijk om te begrijpen dat maar het Azure-platform deze mogelijkheden ingebouwde tolerantie biedt, u uw Azure-implementatie moet om te profiteren van Azure-functies en services die zorgen voor hoge beschikbaarheid ontwerpen herstel na noodgevallen en back-up.

- Uw BCDR-oplossing, hangt af van de doelstellingen van uw bedrijf en beïnvloed door uw strategie voor Azure-implementatie. Infrastructure as a Service (IaaS) en Platform als een Service (PaaS)-implementaties aanwezig zijn verschillende uitdagingen voor BCDR.
- Eenmaal aanwezig is, uw BCDR-oplossingen moeten regelmatig worden getest om te controleren dat uw strategie voor levensvatbare blijft.


## <a name="best-practice-back-up-your-data"></a>Aanbevolen: Een back-up maken van uw gegevens

In de meeste gevallen een on-premises-workload is buiten gebruik gesteld na de migratie en uw on-premises-strategie voor back-ups van gegevens moet worden verlengd of vervangen. Als u uw hele datacenter naar Azure migreert, moet u voor het ontwerpen en implementeren van een volledige back-up-oplossing met behulp van Azure-technologieën of geïntegreerde oplossingen van derden. 


### <a name="back-up-an-iaas-deployment"></a>Back-up van een IaaS-implementatie

Voor workloads die worden uitgevoerd op Azure IaaS VM's, kunt u overwegen deze back-upoplossingen:

- **Azure Backup**: Biedt toepassingsconsistente back-ups voor Windows Azure en Linux-machines.
- **Storage-momentopnamen**: Momentopnamen van blob-opslag.

#### <a name="azure-backup"></a>Azure Backup

Azure Backup back-ups maakt herstelpunten van gegevens die zijn opgeslagen in Azure storage. Azure Backup een back-up Azure VM-schijven, en Azure Files (preview). Azure Files bieden bestandsshares in de cloud, die toegankelijk zijn via SMB.
   
Azure Backup kunt u back-up van virtuele machines in een aantal manieren.

- **Directe back-ups van VM-instellingen**: U kunt back-up van virtuele machines met Azure Backup rechtstreeks vanuit de VM-opties in de Azure-portal. U kunt back-up van de virtuele machine eenmaal en de dag en de VM-schijf herstellen indien nodig. App-bewuste momentopnamen (VSS) worden Azure back-ups, geen agent is geïnstalleerd op de virtuele machine.
- **Directe back-ups in een Recovery Services-kluis**: U kunt een back-up van uw IaaS-VM's maken door het implementeren van een Azure Backup Recovery Services-kluis. Dit biedt één locatie als u wilt bijhouden en beheren van back-ups en biedt gedetailleerde opties voor back-up en herstel. Back-up is maximaal drie keer per dag, op het niveau van het bestand/map. Het app-bewuste niet en Linux wordt niet ondersteund. U moet de Microsoft Azure Recovery Services agent (MARS) installeren op elke virtuele machine die u back wilt-up.
- **Azure Backup Server: Beveiligen van de virtuele machine naar Azure back-upserver**: Azure Backup Server wordt het gratis geleverd met Azure Backup. De virtuele machine is back-ups op lokale opslag van Azure Backup Server. U hebt vervolgens maakt u een back-up van de Azure Backup-Server naar Azure in een kluis. Back-up is app-bewust is, met volledige granulatie via vaak-up en retentie. U kunt een back-up maken op het niveau van de app. Bijvoorbeeld door de back-ups van SQL Server of SharePoint.

Voor beveiliging, back-up Azure gegevens die onderweg zijn versleuteld met AES-256 en verzendt ze naar Azure via HTTPS. Back-upgegevens in rust in Azure is versleuteld met behulp van [Storage Service Encryption (SSE)](https://docs.microsoft.com/azure/storage/common/storage-service-encryption?toc=%2fazure%2fstorage%2fqueues%2ftoc.json), en gegevens voor de overdracht en opslag.


![Azure Backup](./media/migrate-best-practices-security-management/iaas-backup.png)
*Azure Backup*

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) verschillende typen back-ups.
- [Een back-upinfrastructuur plannen](https://docs.microsoft.com/azure/backup/backup-azure-vms-introduction) voor virtuele Azure-machines.

#### <a name="storage-snapshots"></a>Storage-momentopnamen

Azure-VM's worden opgeslagen als pagina-blobs in Azure Storage. 

- Momentopnamen vastleggen de status van de blob op een bepaald tijdstip.
- Als een alternatieve back-upmethode voor Azure VM-schijven, kunt u een momentopname van de storage-blobs en kopieer deze naar een ander opslagaccount. 
- U kunt een hele blob kopiëren of een momentopname van de incrementele kopie van exemplaar alleen deltawijzigingen gebruiken en opslagruimte te verminderen.
- Als een extra voorzorgsmaatregel, kunt u voorlopig verwijderen voor blob storage-accounts inschakelen. Met deze functie is ingeschakeld, is een blob die wordt verwijderd gemarkeerd voor verwijdering maar niet direct worden opgeschoond. Tijdens de periode kan de blob worden hersteld.

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) Azure blob-opslag.
- [Meer informatie over het](https://docs.microsoft.com/azure/storage/blobs/storage-blob-snapshots) een blob-momentopname maken.
- [Bekijk een voorbeeldscenario](https://azure.microsoft.com/blog/microsoft-azure-block-blob-storage-backup) voor back-up van blob-opslag.
- [Meer informatie over](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) voorlopig verwijderen.
- [Beoordeling](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) wat te doen als een Azure Storage-storing?


#### <a name="third-party-backup"></a>Back-up van derden

Bovendien kunt u oplossingen van derden naar het back-up van virtuele Azure-machines en storage-containers naar de lokale opslag of andere cloudproviders. [Meer informatie](https://azuremarketplace.microsoft.com/marketplace/apps?search=backup&page=1) over back-upoplossingen in de Azure marketplace. 


### <a name="back-up-a-paas-deployment"></a>Back-up van een PaaS-implementatie


In tegenstelling tot IaaS waar u uw eigen virtuele machines en infrastructuur beheren, in een PaaS wordt model platform en de infrastructuur beheerd door de provider, zodat u zich kunt richten op de kernlogica van de app en mogelijkheden. Met veel verschillende soorten PaaS-services, wordt elke service afzonderlijk worden geëvalueerd voor de doeleinden van back-up. We kijken naar twee veelvoorkomende Azure PaaS-services - Azure SQL Database en Azure Functions.

#### <a name="back-up-azure-sql-database"></a>Back-up van Azure SQL Database

Azure SQL Database is een volledig beheerde PaaS-Database-Engine. Het biedt een aantal van de functies voor bedrijfscontinuïteit, met inbegrip van back-up automatiseren.

- SQL-Database voert automatisch wekelijkse volledige back-ups en differentiële back-ups elke 12 uur. Transactielogboekback-ups wordt elke vijf tot tien minuten doorgeleid naar de database te beschermen tegen gegevensverlies.
- Back-ups zijn transparant en geen extra kosten in rekening worden gebracht.
- Back-ups worden opgeslagen in de RA-GRS-opslag voor geografisch redundante en gerepliceerd naar de gekoppelde geografische regio.
- Back-upretentie, is afhankelijk van de aankopen model. Servicelagen op basis van DTU Ga van zeven dagen voor Basic-laag naar 35 dagen voor de andere lagen.
- U kunt een database herstellen naar een punt-in-time binnen de bewaarperiode liggen. U kunt ook een verwijderde database herstellen naar een andere geografische regio of herstellen vanuit een langetermijnback-up als de database een beleid met een langetermijnbewaarperiode (LTR heeft).


![Azure SQL-back-up](./media/migrate-best-practices-security-management/sql-backup.png)
*Azure SQL-back-up*

**Meer informatie:**
- [Geautomatiseerde back-ups](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups) voor SQL-Database.
- [Een database herstellen](https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups) met behulp van geautomatiseerde back-ups.

 
#### <a name="back-up-azure-functions"></a>Back-up van Azure Functions

Omdat Azure Functions meer of minder als code werkt, u moet een back-up met behulp van dezelfde methoden die u gebruiken voor het beveiligen van code, zoals broncodebeheer in GitHub of Azure DevOps-Services

**Meer informatie:**

- [Gegevensbeveiliging](https://docs.microsoft.com/azure/devops/articles/team-services-security-whitepaper?view=vsts) voor Azure DevOps.

## <a name="best-practice-set-up-disaster-recovery"></a>Aanbevolen: Herstel na noodgeval instellen 

Naast het beveiligen van gegevens, BCDR planning moet rekening houden met hoe u apps en workloads beschikbaar zijn in geval van nood. 

### <a name="set-up-disaster-recovery-for-iaas-apps"></a>Herstel na noodgevallen voor IaaS-apps instellen

Houd rekening met deze oplossingen voor workloads die worden uitgevoerd op Azure IaaS VM's en Azure storage:

- **Azure Site Recovery**: Organiseert de replicatie van virtuele Azure-machines in een primaire naar een secundaire regio. Als er uitval optreedt, schakelt u over van de primaire regio naar de secundaire en gebruikers toegang houden tot apps. Als er dingen zijn weer in de normale, schakelt u terug naar de primaire regio.
- **Azure-opslag**: Azure biedt in-build veerkracht en hoge beschikbaarheid voor verschillende soorten opslag:

#### <a name="azure-site-recovery"></a>Azure Site Recovery 

Azure Site Recovery is de primaire Azure-service om ervoor te zorgen dat Azure-VM's kunnen online worden gebracht en VM-apps die beschikbaar zijn gesteld wanneer er uitval optreedt. Site Recovery repliceert virtuele machines van een primaire naar secundaire Azure-regio. Wanneer noodgeval voordoet, moet u virtuele machines failover van de primaire regio en blijven toegang houden tot deze zoals gebruikelijk in de secundaire regio. Als er operations terugkeren naar de normale, schakelt u virtuele machines terug naar de primaire regio.


![Azure Site Recovery](./media/migrate-best-practices-security-management/site-recovery.png)
*Site Recovery*

**Meer informatie:**
- [Beoordeling](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-disaster-recovery-guidance) herstel na noodgevallen voor Azure VM's.
- [Meer informatie over het](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-replicate-after-migration) herstel na noodgevallen instellen voor een virtuele Azure-machine na de migratie.

#### <a name="azure-storage"></a>Azure Storage

Azure storage wordt gerepliceerd voor de ingebouwde tolerantie en hoge beschikbaarheid.

-   **Geografisch redundante opslag (GRS)**: Beschermt tegen regiobrede uitval, met ten minste 99,99999999999999% (16 9's) duurzaamheid van objecten in een bepaald jaar.
    - Gegevens in de opslag wordt gerepliceerd naar de secundaire regio waaraan uw primaire regio is gekoppeld.
    - Als de primaire regio uitvalt, en Microsoft een failover naar de secundaire regio initieert, hebt u leestoegang tot uw gegevens.
- **Geografisch redundante opslag met leestoegang (RA-GRS)**: Beschermt tegen regiobrede uitval.
    - Gegevens in de opslag wordt gerepliceerd naar de secundaire regio.
    - U hebt alleen toegang tot de gerepliceerde gegevens in de secundaire regio, ongeacht of er een failover wordt gestart in Microsoft gegarandeerd. Wanneer twee of meer datacenters in dezelfde regio kan er een probleem is, maar uw gegevens zich nog steeds beschikbaar in een geografisch gescheiden regio.
-   **Zone-redundante opslag (ZRS)**:  Beschermt tegen fouten in datacenter.
    - ZRS repliceert gegevens synchroon tussen drie opslagclusters in één regio. Clusters en fysiek van elkaar gescheiden en elk zich in een eigen binnen een beschikbaarheidszone.
    - Als er zich een noodgeval voordoet, wordt uw opslag nog steeds beschikbaar zijn. ZRS moet het minimale doel voor uw essentiële workloads.



**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/storage/common/storage-redundancy) Azure storage-replicatie.


### <a name="set-up-disaster-recovery-for-paas-workloads"></a>Instellen van herstel na noodgevallen voor werkbelastingen voor PaaS

Laten we eens na noodgevallen herstelopties voor onze voorbeelden van de werkbelasting PaaS.

#### <a name="disaster-recovery-of-azure-sql-server"></a>Herstel na noodgevallen van Azure SQL-Server

Er zijn een aantal van de verschillende opties voor elk die invloed hebben op verlies van gegevens, hersteltijd en kosten.

U kunt failover-groepen en actieve geo-replicatie gebruiken om te bieden tolerantie tegen regionale uitval en onherstelbare fouten

- **Actieve geo-replicatie**: Actieve geo-replicatie voor snelle noodherstel implementeren als een storing in het datacenter of een verbinding kan niet worden gemaakt met een primaire database.
    - Geo-replicatie wordt voortdurend leesbare replica's van uw database gemaakt in maximaal vier secundaire databases in de dezelfde of verschillende regio's.
    - In een storing, kunt u een failover uitvoeren naar een van de secundaire regio's en uw database weer online brengen.
- **Automatische failovergroepen**: Automatische failover-groepen uitbreiden actieve geo-replicatie met transparante failover van meerdere databases.
    - Een automatische failover-groep biedt een krachtige abstractie van actieve geo-replicatie met groep op database-replicatie en automatische failover.
    - U maakt een failovergroep met een primaire server die als host fungeert een of meer primaire databases, een secundaire server die als host fungeert voor alleen-lezen replica's van de primaire databases, listeners die naar elke server en een automatische failover-beleid verwijzen.
    - De opgegeven listener-eindpunten voor de noodzaak om te wijzigen van de SQL-verbindingsreeks na een failover.
- **Geo-restore**: 
    -   Geo-restore kunt u de database naar een andere regio te herstellen. De automatische back-up van alle Azure-databases worden, gerepliceerd naar een secundaire regio op de achtergrond. Het wordt altijd de database herstellen van de kopie van de back-upbestanden die zijn opgeslagen in de secundaire regio.
-   **Zone-redundante databases** bieden ingebouwde ondersteuning voor Azure-beschikbaarheidszones.
    - Zone-redundante databases voor het verbeteren van hoge beschikbaarheid voor Azure SQL-Server in het geval van een fout data center.
    - Met zone-redundantie, kunt u redundante database-replica's in verschillende beschikbaarheidszones in een regio te plaatsen. 



![Geo-replicatie](./media/migrate-best-practices-security-management/geo-replication.png)
*Geo-replicatie*

**Meer informatie:**
- [Meer informatie over](https://docs.microsoft.com/azure/sql-database/sql-database-high-availability) hoge beschikbaarheid voor Azure SQL-Server.
- [Lezen](https://azure.microsoft.com/blog/azure-sql-databases-disaster-recovery-101/) 101 van Azure SQL-Databases voor herstel na noodgevallen.
- [Bekijk een overzicht](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) van actieve geo-replicatie en failover-groepen.
- [Meer informatie over](https://docs.microsoft.com/azure/sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery) ontwerpen voor herstel na noodgevallen.
- [Aanbevolen procedures ophalen](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview#best-practices-of-using-failover-groups-for-business-continuity) voor failover-groepen.
- [Aanbevolen procedures ophalen](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-security-config) voor beveiliging na geografisch herstel en failover.
- [Meer informatie over](https://docs.microsoft.com/azure/sql-database/sql-database-high-availability#zone-redundant-configuration) zone redundantie
- [Meer informatie over het](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery-drills) een Dr-herstelanalyse uitvoeren voor SQL-database.

### <a name="disaster-recovery-for-azure-functions"></a>Herstel na noodgevallen voor Azure Functions

Als de beheerinfrastructuur in Azure is mislukt, kan een Azure-functie-app niet meer beschikbaar.

- Als u wilt de mogelijkheid van dergelijke downtime minimaliseren, gebruikt u twee functie-apps geïmplementeerd naar verschillende regio's.
- Met Azure Traffic Manager kan worden geconfigureerd voor detectie van problemen op de primaire functie-app en automatisch verkeer omleiden naar de functie-app in de secundaire regio
- Traffic Manager met geografisch redundante opslag kunt u dezelfde functie hebben in meerdere regio's, bij een regionale storing


![Traffic Manager](./media/migrate-best-practices-security-management/traffic-manager.png)
*Traffic Manager*

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications) herstel na noodgevallen voor Azure-apps.
- [Meer informatie over](https://docs.microsoft.com/azure/azure-functions/durable/durable-functions-disaster-recovery-geo-distribution) herstel na noodgevallen en geo-distributie voor duurzame Azure functions.


## <a name="best-practice-use-managed-disks-and-availability-sets"></a>Aanbevolen: Gebruik beheerde schijven en beschikbaarheidssets

Azure maakt gebruik van beschikbaarheidssets voor virtuele machines logisch groeperen en voor het isoleren van virtuele machines in een set van andere bronnen. Virtuele machines in een beschikbaarheidsset worden verdeeld over meerdere foutdomeinen met afzonderlijke subsystemen, ter bescherming tegen lokale stroomstoringen, en ook worden verdeeld over meerdere updatedomeinen dat niet alle virtuele machines in een set opnieuw op hetzelfde moment opstarten.

Azure Managed Disks vereenvoudigen Schijfbeheer voor Azure IaaS VM's door het beheer van de storage-accounts die zijn gekoppeld aan de VM-schijven.

- U wordt aangeraden dat u beheerde schijven gebruiken, indien mogelijk. U hoeft alleen te geven van het type opslag die u wilt gebruiken en de grootte van de schijf moet u, en Azure maakt en beheert de schijf voor u, achter de schermen.
- U kunt bestaande schijven naar beheerd converteren.
- U moet de virtuele machines maken in beschikbaarheidssets voor een hoge tolerantie en beschikbaarheid. Als er geplande of niet-geplande uitval optreedt, beschikbaarheidssets zorgen ervoor dat ten minste één van uw virtuele machines in de set nog steeds beschikbaar.


![Beheerde schijven](./media/migrate-best-practices-security-management/managed-disks.png)
*beheerde schijven*

**Meer informatie:**
- [Bekijk een overzicht](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) van beheerde schijven.
- [Meer informatie over](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks) schijven converteren naar beheerd.
- [Meer informatie over het](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) de beschikbaarheid van Windows-VM's in Azure beheren.


## <a name="best-practice-monitor-resource-usage-and-performance"></a>Aanbevolen: Monitor Resourcegebruik en prestaties 

Mogelijk hebt u uw workloads naar Azure verplaatst voor de gigantische schaal mogelijkheden. Echter, het verplaatsen van uw workload betekent niet dat Azure automatisch implementeert schalen zonder dat uw invoer. Als u een voorbeeld:

- Als uw organisatie marketing pushes een nieuwe aankondiging tv-programma's die 300% meer verkeer aanstuurt, kan deze site beschikbaarheidsproblemen veroorzaken. De gemigreerde werkbelasting mogelijk bereikt toegewezen limieten en foutrapporten.
- Een ander voorbeeld is mogelijk een gedistribueerde denial-of-service (DDoS)-aanval op de gemigreerde werkbelasting. In dit geval raadzaam niet om te schalen, maar om te voorkomen dat de bron van de aanvallen bereikt uw resources.

Deze twee gevallen hebben verschillende resoluties voor beide u moeten echter een beter inzicht in wat met het gebruik en de bewaking van toepassingsprestaties gebeurt er.

- Azure Monitor kunt u deze metrische gegevens over het oppervlak en antwoord voorzien van waarschuwingen, automatisch schalen, eventhubs, logic apps en meer.
- Naast Azure monitoring, kunt u uw SIEM-toepassing van derden voor het bewaken van de Azure-logboeken voor controle en de prestaties gebeurtenissen integreren.


![Azure Monitor](./media/migrate-best-practices-security-management/monitor.png)
*Azure Monitor*

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/azure-monitor/overview) Azure Monitor.
- [Aanbevolen procedures ophalen](https://docs.microsoft.com/azure/architecture/best-practices/monitoring) voor controle en diagnostiek.
- [Meer informatie over](https://docs.microsoft.com/azure/architecture/best-practices/auto-scaling) automatisch schalen.
- [Meer informatie over het](https://docs.microsoft.com/azure/security-center/security-center-export-data-to-siem) Azure gegevens routeren naar een SIEM-hulpprogramma.

## <a name="best-practice-enable-diagnostic-logging"></a>Aanbevolen: Bijhouden van diagnostische gegevens inschakelen

Azure-resources een geoorloofd aantal metrische gegevens en telemetrie-gegevens in een logboek gegenereerd.

- De meeste resourcetypen geen standaard Diagnostische logboekregistratie is ingeschakeld.
- Door Diagnostische logboekregistratie inschakelen voor uw resources, kunt u logboekregistratie van gegevens op te vragen en bouwen van waarschuwingen en playbooks op basis van dit.
- Wanneer u Diagnostische logboekregistratie inschakelt, wordt elke resource een specifieke set categorieën hebben. Selecteert u een of meer categorieën voor logboekregistratie en een locatie voor de logboekgegevens. Logboeken kunnen worden verzonden naar een opslagaccount, de event hub, of naar Log Analytics. 


![Registratie in diagnoselogboek](./media/migrate-best-practices-security-management/diagnostics.png)
*registratie in diagnoselogboek*

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) verzamelen en gebruiken van logboekgegevens.
- [Informatie over wat er wordt ondersteund](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-diagnostic-logs-schema) voor registratie in diagnoselogboek.


## <a name="best-practice-set-up-alerts-and-playbooks"></a>Aanbevolen: Instellen van waarschuwingen en playbooks

Met Diagnostische logboekregistratie is ingeschakeld voor Azure-resources, kunt u gegevens voor logboekregistratie gebruiken om te maken van aangepaste waarschuwingen starten.

- Waarschuwingen waarschuwen u proactief te wanneer er voorwaarden in uw bewakingsgegevens worden gevonden. U kunt vervolgens problemen kunt oplossen voordat gebruikers ziet u deze. U kunt waarschuwingen op dingen zoals metrische waarden, log zoekquery's, gebeurtenissen in het activiteitenlogboek, platform-status en beschikbaarheid van de website.
- Wanneer waarschuwingen worden geactiveerd, kunt u een logische App Playbook uitvoeren. Een playbook helpt u bij het automatiseren en organiseren van een reactie op een specifieke waarschuwing. Playbooks zijn gebaseerd op Azure Logic Apps. Playbooks maken of maak uw eigen kunt u sjablonen voor logische Apps.
- Als een eenvoudig voorbeeld kunt u een waarschuwing die wordt geactiveerd wanneer een scan poort op basis van een netwerkbeveiligingsgroep gebeurt maken.  U kunt instellen wanneer er een playbook die wordt uitgevoerd en het IP-adres van de oorsprong van de scan wordt vergrendeld.
- Een ander voorbeeld is mogelijk een app met een geheugenlek.  Als het geheugengebruik op een bepaald moment, kan het proces door een playbook recyclen.


![Waarschuwingen](./media/migrate-best-practices-security-management/alerts.png)
*waarschuwingen*

**Meer informatie:**
- [Meer informatie over](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-alerts) waarschuwingen.
- [Meer informatie over](https://docs.microsoft.com/azure/security-center/security-center-playbooks) beveiligingsplaybooks die op waarschuwingen van Beveiligingscentrum reageren.

## <a name="best-practice-use-the-azure-dashboard"></a>Aanbevolen: Gebruik de Azure-dashboard

De Azure portal is een geïntegreerde webconsole waarmee u kunt bouwen, beheren en bewaken van alles, van eenvoudige web-apps tot complexe cloudtoepassingen. De portal bevat aanpasbare dashboards en toegankelijkheidsopties.
- U kunt meerdere dashboards maken en delen met anderen die toegang tot uw Azure-abonnementen hebben.
- Met deze gedeelde model heeft uw team inzicht in de Azure-omgeving, zodat ze proactieve worden bij het beheer van systemen in de cloud.


![Azure-dashboard](./media/migrate-best-practices-security-management/dashboard.png)
*Azure-dashboard*

**Meer informatie:**

- [Meer informatie over het](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards) een dashboard maken.
- [Meer informatie over](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-structure) dashboard structuur.


## <a name="best-practice-understand-support-plans"></a>Aanbevolen: Ondersteuningsabonnementen begrijpen

Op een bepaald moment moet u samenwerken met uw helpdesk of de medewerkers van Microsoft ondersteuning. Met een set beleidsregels en procedures voor ondersteuning tijdens de scenario's, zoals herstel na noodgevallen is het essentieel. Daarnaast moet uw beheerders en ondersteunend personeel worden getraind op het implementeren van beleid.

- In het onwaarschijnlijke geval dat een Azure-service-probleem gevolgen heeft voor uw werkbelasting, moeten beheerders weten hoe u kunt een ondersteuningsticket naar Microsoft verzenden in de juiste en meest efficiënte manier.
- Maak uzelf bekend met de verschillende ondersteuningsplannen aangeboden voor Azure. Ze variëren van reactietijden toegewezen Developer-exemplaren aan bij Premier support met een reactietijd van minder dan 15 minuten.


![Ondersteuningsabonnementen](./media/migrate-best-practices-security-management/support.png)
*Ondersteuningsabonnementen*

**Meer informatie:**
- [Bekijk een overzicht](https://azure.microsoft.com/support/options/) van Azure-ondersteuningsabonnementen.
- [Meer informatie over](https://azure.microsoft.com/support/legal/sla/) service level agreements (Sla's).

## <a name="best-practice-manage-updates"></a>Aanbevolen: Updates beheren

Azure-VM's te houden bijgewerkt met de meest recente versie van het besturingssysteem en software-updates is een zeer grote taken uit te voeren. De mogelijkheid om het oppervlak van alle virtuele machines, om te bepalen welke updates die ze nodig hebben, en automatisch push die updates is zeer waardevol zijn.

- U kunt updatebeheer in Azure Automation gebruiken voor het beheren van updates van het besturingssysteem voor computers met Windows en Linux-computers die zijn geïmplementeerd in Azure, on-premises en in andere cloudproviders. 
- Met updatebeheer snel de status van de beschikbare updates op alle agentcomputers beoordelen en beheren van de installatie van de update.
- U kunt updatebeheer inschakelen voor virtuele machines rechtstreeks vanuit een Azure Automation-account. U kunt ook een enkele virtuele machine op de pagina van de virtuele machine in Azure portal bijwerken.
- Met System Center Configuration Manager kan bovendien aan Azure-VM's worden geregistreerd. U kunt vervolgens migreren van de Configuration Manager-workload naar Azure en rapportage- en software-updates uitvoeren vanuit een één webinterface.


![Updates van de virtuele machine](./media/migrate-best-practices-security-management/updates.png)
*Updates*

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/automation/automation-update-management) updatebeheer in Azure.
- [Meer informatie over het](https://docs.microsoft.com/azure/automation/oms-solution-updatemgmt-sccmintegration) Configuration Manager integreren met updatebeheer.
- [Veelgestelde vragen over](https://docs.microsoft.com/sccm/core/understand/configuration-manager-on-azure) over Configuration Manager in Azure.


## <a name="implement-a-change-management-process"></a>Implementeer een proces voor het beheren van wijzigen

Net als bij elk productiesysteem, waardoor elk type wijziging kan invloed hebben op uw omgeving. Een proces voor het beheer van wijzigen waarvoor aanvragen worden ingediend om de wijzigingen aanbrengen in productiesystemen is een waardevolle toevoeging in uw omgeving gemigreerd.

- U kunt best practice-frameworks voor wijzigingsbeheer, bewust maken in de groep beheerders en ondersteunend personeel op te bouwen.
- U kunt Azure Automation gebruiken om te helpen met Configuratiebeheer en bijhouden van wijzigingen voor uw gemigreerde werkstromen.
- Bij het afdwingen van proces voor het beheren van wijzigen, kunt u auditlogboeken te koppelen van Azure wijziging logboeken aan consistentiefout (of niet) bestaande wijzigingsaanvragen. Zo dat als u een wijziging aangebracht zonder een bijbehorende wijzigingsaanvraag ziet, u wat is een fout opgetreden in het proces onderzoeken kunt.

Azure heeft een oplossing voor wijzigingen bijhouden in Azure automation:

- De oplossing houdt de wijzigingen in Windows en Linux-software en bestanden, Windows-registersleutels, services voor Windows en Linux-daemons.
- Wijzigingen in de bewaakte servers worden verzonden naar de Log Analytics-service in de cloud voor verwerking.
- Logica wordt toegepast op de ontvangen gegevens en de cloudservice registreert de gegevens.
- Op het dashboard wijzigingen bijhouden kunt kunt u gemakkelijk zien de wijzigingen die zijn aangebracht in uw serverinfrastructuur.


![Wijzigingsbeheer](./media/migrate-best-practices-security-management/change.png)
*wijzigingsbeheer*

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/automation/automation-change-tracking) wijzigingen bijhouden.
- [Meer informatie over](https://docs.microsoft.com/azure/automation/automation-intro) mogelijkheden van Azure Automation.




## <a name="next-steps"></a>Volgende stappen 

Bekijk de andere aanbevolen procedures:


- [Aanbevolen procedures](migrate-best-practices-networking.md) voor netwerken na de migratie.
- [Aanbevolen procedures](migrate-best-practices-costs.md) voor kostenbeheer na de migratie.








