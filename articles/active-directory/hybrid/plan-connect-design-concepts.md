---
title: 'Azure AD Connect: Ontwerpconcepten | Microsoft Docs'
description: In dit onderwerp wordt uitgelegd bepaalde gebieden van implementatie-ontwerp
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 4114a6c0-f96a-493c-be74-1153666ce6c9
ms.service: active-directory
ms.custom: azure-ad-connect
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 08/10/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01cc85f7eba2aefd08192c4e3f4e5151e7645238
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56269107"
---
# <a name="azure-ad-connect-design-concepts"></a>Azure AD Connect: Ontwerpconcepten
Het doel van dit document is om te beschrijven van de gebieden die moeten worden beschouwd door tijdens het implementatieontwerp van de van Azure AD Connect. Dit document is een gedetailleerde informatie over bepaalde gebieden en deze concepten worden kort beschreven in andere documenten.

## <a name="sourceanchor"></a>sourceAnchor
Het kenmerk sourceAnchor is gedefinieerd als *een kenmerk onveranderbaar tijdens de levensduur van een object*. Dit is de unieke identificatie van een object dat de dezelfde object on-premises en in Azure AD. Het kenmerk wordt ook wel genoemd **immutableId** en de namen van de twee uitwisselbaar worden gebruikt.

Het woord onveranderbare, dat is 'kan niet worden gewijzigd', is belangrijk dat u dit document. Omdat de waarde van dit kenmerk kan niet worden gewijzigd nadat deze is ingesteld, is het belangrijk om te kiezen van een ontwerp die ondersteuning biedt voor uw scenario.

Het kenmerk wordt gebruikt voor de volgende scenario's:

* Wanneer een nieuwe synchronisatie-engine-server is gebouwd, of opnieuw worden opgebouwd na een noodherstelscenario, wordt dit kenmerk wordt de bestaande objecten in Azure AD met objecten on-premises gekoppeld.
* Als u van de identiteit van een alleen-cloud naar een identiteitsmodel gesynchroniseerde, kan dit kenmerk objecten 'harde match' bestaande objecten in Azure AD met on-premises-objecten.
* Als u Federatie, wordt dit kenmerk samen met de **userPrincipalName** in de claim wordt gebruikt voor het aanduiden van een gebruiker.

In dit onderwerp alleen vertelt sourceAnchor als deze zich tot gebruikers verhoudt. De dezelfde regels gelden voor alle objecttypen, maar het is alleen voor gebruikers van dat dit probleem is meestal een probleem.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Een goede sourceAnchor-kenmerk selecteren
De kenmerkwaarde moet de volgende regels volgen:

* Minder dan 60 tekens lang
  * Tekens die niet wordt a-z, A-Z en 0-9 zijn gecodeerd en geteld als 3 tekens
* Een speciaal teken niet bevatten: &#92; ! # $ % & * + / = ? ^ &#96; { } | ~ < > ( ) ' ; : , [ ] " \@ _
* Moet globaal uniek zijn
* Moet een tekenreeks, een geheel getal of een binair bestand
* Niet moet worden gebaseerd op de naam van gebruiker omdat deze kunnen wijzigen
* Niet mogen worden hoofdlettergevoelig en waarden die per aanvraag variëren kunnen voorkomen
* Moet worden toegewezen wanneer het object wordt gemaakt

Als het geselecteerde sourceAnchor is niet van het typetekenreeks, wordt Azure AD Connect Base64Encode de waarde van het kenmerk om te controleren of er geen speciale tekens weergegeven. Als u een andere federatieserver dan AD FS gebruikt, zorg ervoor dat de server kan ook Base64Encode het kenmerk.

Het kenmerk sourceAnchor is hoofdlettergevoelig. Een waarde van "Jandevries" is niet gelijk zijn aan "jandevries". Maar u moet geen twee verschillende objecten met alleen een verschil in geval.

Als u heb één forest is het on-premises, klikt u vervolgens het kenmerk moet u **objectGUID**. Dit is ook het kenmerk wordt gebruikt wanneer u express-instellingen in Azure AD Connect en ook het kenmerk wordt gebruikt door DirSync.

Als u meerdere forests hebt en niet gebruikers tussen forests en domeinen, klikt u vervolgens verplaatsen **objectGUID** te gebruiken, zelfs in dit geval een goed kenmerk is.

Als u gebruikers tussen forests en domeinen verplaatst, moet klikt u vervolgens u een kenmerk dat niet wordt gewijzigd of kan worden verplaatst met de gebruikers tijdens de verplaatsing. Een aanbevolen aanpak is om te introduceren een synthetische kenmerk. Een kenmerk dat kan bevatten iets dat lijkt op een GUID zijn geschikt. Tijdens het maken van databaseobject is een nieuwe GUID gemaakt en tijdstempel op de gebruiker. Een aangepaste synchronisatieregel kan worden gemaakt in de synchronisatie-engine-server te maken van deze waarde op basis van de **objectGUID** en bijwerken van het geselecteerde kenmerk in AD DS. Wanneer u het object verplaatst, zorg ervoor dat de inhoud van deze waarde ook kopiëren.

Een andere oplossing is om op te halen van een bestaand kenmerk dat u weet niet wordt gewijzigd. Gangbare kenmerken zijn **employeeID**. Als u een kenmerk dat letters bevat overweegt, zorg ervoor dat er dat geen kans van de aanvraag (hoofdletters en kleine letters) voor de waarde van het kenmerk kunt wijzigen. Ongeldige kenmerken die niet mogen worden gebruikt zijn deze kenmerken met de naam van de gebruiker. In een huwelijk of echtscheiding, de naam verwacht te wijzigen, dit is niet toegestaan voor dit kenmerk. Dit is ook een van de redenen waarom kenmerken zoals **userPrincipalName**, **e-mail**, en **targetAddress** zijn niet ook mogelijk om te selecteren in de Azure AD Connect-installatie de wizard. Deze kenmerken bevatten ook de "\@"-teken, wat niet is toegestaan in het sourceAnchor.

### <a name="changing-the-sourceanchor-attribute"></a>Het kenmerk sourceAnchor wijzigen
De waarde van het kenmerk sourceAnchor kan niet worden gewijzigd nadat het object is gemaakt in Azure AD en de identiteit is gesynchroniseerd.

Daarom gelden de volgende beperkingen naar Azure AD Connect:

* Het kenmerk sourceAnchor kan alleen worden ingesteld tijdens de eerste installatie. Als u de installatiewizard opnieuw uitvoeren, wordt deze optie is alleen-lezen. Als u nodig hebt om deze instelling te wijzigen, moet u verwijderen en opnieuw installeren.
* Als u een andere Azure AD Connect-server installeert, moet u de dezelfde sourceAnchor-kenmerk zoals eerder gebruikt selecteren. Als u eerder hebt gebruikt als DirSync en verplaatsen naar Azure AD Connect, dan moet u **objectGUID** omdat dat het kenmerk wordt gebruikt door DirSync.
* Als de waarde voor sourceAnchor wordt gewijzigd nadat is het object geëxporteerd naar Azure AD, klikt u vervolgens Azure AD Connect sync genereert een fout en kan geen wijzigingen meer op dat object voordat het probleem is opgelost en het sourceAnchor is gewijzigd in de bron-directeur y.

## <a name="using-ms-ds-consistencyguid-as-sourceanchor"></a>Met behulp van ms-DS-ConsistencyGuid as sourceAnchor
Standaard Azure AD Connect (versie 1.1.486.0 en ouder) objectGUID gebruikt als het kenmerk sourceAnchor. ObjectGUID is door het systeem gegenereerd. U kunt de waarde bij het maken van on-premises AD-objecten niet opgeven. Zoals uitgelegd in de sectie [sourceAnchor](#sourceanchor), er zijn scenario's waarbij u moet de waarde sourceAnchor op te geven. Als de scenario's op u van toepassing zijn, moet u een configureerbare AD-kenmerk (bijvoorbeeld, ms-DS-ConsistencyGuid) als het kenmerk sourceAnchor.

Azure AD Connect (versie 1.1.524.0 en hoger en na) nu vereenvoudigt het uitvoeren van het gebruik van ms-DS-ConsistencyGuid as sourceAnchor-kenmerk. Wanneer u deze functie gebruikt, configureert Azure AD Connect automatisch de synchronisatieregels op:

1. Ms-DS-ConsistencyGuid gebruiken als het kenmerk sourceAnchor voor gebruikersobjecten. ObjectGUID wordt gebruikt voor andere objecttypen.

2. Voor een gegeven on-premises AD-gebruiker object waarvan het kenmerk ms-DS-ConsistencyGuid niet is ingevuld, Azure AD Connect schrijft de waarde objectGUID back-ups maken met het kenmerk ms-DS-ConsistencyGuid in on-premises Active Directory. Nadat het kenmerk ms-DS-ConsistencyGuid is ingevuld, wordt het object vervolgens geëxporteerd met Azure AD Connect naar Azure AD.

>[!NOTE]
> Eenmaal een on-premises AD-object is geïmporteerd in Azure AD Connect (die is geïmporteerd in het AD-Connectorgebied en in de metaverse geprojecteerd), u kunt de waarde sourceAnchor niet meer wijzigen. De sourceAnchor op waarde opgeven voor een gegeven on-premises AD-object, configureren van het kenmerk ms-DS-ConsistencyGuid voordat het wordt geïmporteerd naar Azure AD Connect.

### <a name="permission-required"></a>Machtiging is vereist
Voor deze functie te gebruiken, moet de machtiging schrijven naar het kenmerk ms-DS-ConsistencyGuid in on-premises Active Directory op de AD DS-account gebruikt om te synchroniseren met on-premises Active Directory worden verleend.

### <a name="how-to-enable-the-consistencyguid-feature---new-installation"></a>Het inschakelen van de functie ConsistencyGuid - nieuwe installatie
U kunt het gebruik van ConsistencyGuid as sourceAnchor inschakelen tijdens de installatie van nieuwe. In deze sectie bevat informatie over zowel Express en aangepaste installatie in details.

  >[!NOTE]
  > Alleen nieuwere versies van Azure AD Connect (1.1.524.0 en hoger en na) ondersteuning voor het gebruik van ConsistencyGuid as sourceAnchor tijdens de installatie van nieuwe.

### <a name="how-to-enable-the-consistencyguid-feature"></a>De functie ConsistencyGuid inschakelen
De functie kan op dit moment alleen tijdens de nieuwe Azure AD Connect-installatie alleen worden ingeschakeld.

#### <a name="express-installation"></a>Snelle installatie
Bij het installeren van Azure AD Connect met Express-modus, bepaalt de Azure AD Connect-wizard automatisch de meest geschikte AD-kenmerk te gebruiken als het kenmerk sourceAnchor met behulp van de volgende logica:

* De Azure AD Connect-wizard vraagt eerst uw Azure AD-tenant om op te halen van de AD-kenmerk dat wordt gebruikt als het kenmerk sourceAnchor in de vorige installatie van Azure AD Connect (indien aanwezig). Als deze informatie beschikbaar is, wordt in Azure AD Connect maakt gebruik van hetzelfde AD-kenmerk.

  >[!NOTE]
  > Alleen nieuwere versies van Azure AD Connect (1.1.524.0 en hoger en na) informatie over het kenmerk sourceAnchor store in uw Azure AD-tenant die tijdens de installatie wordt gebruikt. Oudere versies van Azure AD Connect niet.

* Als u informatie over het kenmerk sourceAnchor gebruikt niet beschikbaar is, controleert de wizard de status van het kenmerk ms-DS-ConsistencyGuid in uw on-premises Active Directory. Als het kenmerk is niet geconfigureerd op een object in de directory, gebruikt de wizard ms-DS-ConsistencyGuid als het kenmerk sourceAnchor. Als het kenmerk is geconfigureerd op een of meer objecten in de map, wordt de wizard is het kenmerk wordt gebruikt door andere toepassingen en is niet geschikt als sourceAnchor-kenmerk...

* In dat geval de wizard terugvalt op het gebruik objectGUID als het kenmerk sourceAnchor.

* Als het kenmerk sourceAnchor is besloten, slaat de wizard de informatie in uw Azure AD-tenant. De informatie die wordt gebruikt door toekomstige installatie van Azure AD Connect.

Wanneer de Express-installatie is voltooid, de wizard geeft aan welk kenmerk is geselecteerd als het kenmerk Sourceanchor.

![Wizard geeft aan AD-kenmerk voor sourceAnchor verzameld](./media/plan-connect-design-concepts/consistencyGuid-01.png)

#### <a name="custom-installation"></a>Aangepaste installatie
Als u Azure AD Connect installeert met de aangepaste modus, biedt de Azure AD Connect-wizard twee opties bij het configureren van sourceAnchor-kenmerk:

![Aangepaste installatie - sourceAnchor-configuratie](./media/plan-connect-design-concepts/consistencyGuid-02.png)

| Instelling | Description |
| --- | --- |
| Azure het bronanker voor mij laten beheren | Selecteer deze optie als u wilt dat Azure AD het kenmerk voor u selecteert. Als u deze optie selecteert, Azure AD Connect-wizard geldt dezelfde [sourceAnchor-kenmerk selectie logica wordt gebruikt tijdens de snelle installatie](#express-installation). Net als bij de Express-installatie, de wizard geeft aan welk kenmerk is geselecteerd als het kenmerk Sourceanchor nadat de aangepaste installatie is voltooid. |
| Een specifiek kenmerk | Selecteer deze optie als u een bestaand AD-kenmerk opgeeft als het kenmerk sourceAnchor. |

### <a name="how-to-enable-the-consistencyguid-feature---existing-deployment"></a>De functie ConsistencyGuid - bestaande implementatie inschakelen
Hebt u een bestaande Azure AD Connect-implementatie die objectGUID als het kenmerk Bronanker wordt gebruikt, kunt u het overstappen naar ConsistencyGuid in plaats daarvan.

>[!NOTE]
> Alleen nieuwere versies van Azure AD Connect (1.1.552.0 en na) ondersteunen overschakelen van ObjectGuid naar ConsistencyGuid als het kenmerk Bronanker.

Overschakelen van objectGUID naar ConsistencyGuid als het kenmerk Bronanker:

1. Start de wizard Azure AD Connect en klikt u op **configureren** om naar het scherm taken te gaan.

2. Selecteer de **Bronanker configureren** taak optie en klik op **volgende**.

   ![ConsistencyGuid inschakelen voor bestaande implementatie - stap 2](./media/plan-connect-design-concepts/consistencyguidexistingdeployment01.png)

3. Voer de referenties van uw Azure AD-beheerder en klik op **volgende**.

4. Azure AD Connect-wizard analyseert de status van het kenmerk ms-DS-ConsistencyGuid in uw on-premises Active Directory. Als het kenmerk niet is geconfigureerd op een object in de directory, Azure AD Connect dat er geen andere toepassing momenteel wordt gebruikt door het kenmerk en is veilig om te gebruiken als het kenmerk Bronanker wordt afgesloten. Klik op **volgende** om door te gaan.

   ![ConsistencyGuid inschakelen voor bestaande implementatie - stap 4](./media/plan-connect-design-concepts/consistencyguidexistingdeployment02.png)

5. In de **klaar om te configureren** scherm, klikt u op **configureren** om de configuratie wijzigen.

   ![ConsistencyGuid inschakelen voor bestaande implementatie - stap 5](./media/plan-connect-design-concepts/consistencyguidexistingdeployment03.png)

6. Nadat de configuratie is voltooid, wordt de wizard geeft aan dat ms-DS-ConsistencyGuid nu als het kenmerk Bronanker wordt gebruikt.

   ![ConsistencyGuid inschakelen voor bestaande implementatie - stap 6](./media/plan-connect-design-concepts/consistencyguidexistingdeployment04.png)

Tijdens de analyse (stap 4), als het kenmerk is geconfigureerd op een of meer objecten in de map en concludeert de wizard het kenmerk wordt gebruikt door een andere toepassing en een foutmelding zoals wordt geïllustreerd in het onderstaande diagram. Deze fout kan ook optreden als u eerder de ConsistencyGuid-functie hebt ingeschakeld op uw primaire Azure AD Connect-server en u probeert te doen op uw server met tijdelijke bestanden hetzelfde.

![ConsistencyGuid inschakelen voor bestaande implementatie - fout](./media/plan-connect-design-concepts/consistencyguidexistingdeploymenterror.png)

 Als u er zeker van zijn dat het kenmerk wordt niet door andere bestaande toepassingen gebruikt, kunt u de fout onderdrukken door opnieuw starten van de Azure AD Connect-wizard met de **/SkipLdapSearchcontact** opgegeven. Voer de volgende opdracht in de opdrachtprompt om dit te doen:

```
"c:\Program Files\Microsoft Azure Active Directory Connect\AzureADConnect.exe" /SkipLdapSearch
```

### <a name="impact-on-ad-fs-or-third-party-federation-configuration"></a>Gevolgen voor de AD FS of van derden federation-configuratie
Als u gebruikmaakt van Azure AD Connect voor het beheren van on-premises AD FS-implementatie, de Azure AD Connect de claimregels voor het gebruik van hetzelfde AD-kenmerk als sourceAnchor automatisch bijgewerkt. Dit zorgt ervoor dat de ImmutableID-claim die worden gegenereerd door de AD FS consistent met de waarden sourceAnchor is geëxporteerd naar Azure AD is.

Als u AD FS buiten Azure AD Connect beheert of als u externe federation-servers voor verificatie gebruikt, moet u handmatig de claimregels voor ImmutableID-claim zodat deze overeenkomt met de waarden sourceAnchor is geëxporteerd naar Azure AD, zoals beschreven in bijwerken sectie artikel [wijzigen AD FS claim regels](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-management#modclaims). Nadat de installatie is voltooid, retourneert de wizard de volgende waarschuwing:

![Externe federation-configuratie](./media/plan-connect-design-concepts/consistencyGuid-03.png)

### <a name="adding-new-directories-to-existing-deployment"></a>Nieuwe mappen toevoegen aan bestaande implementatie
Stel dat u Azure AD Connect hebt geïmplementeerd met de functie ConsistencyGuid is ingeschakeld, en nu u wilt een andere map toevoegen aan de implementatie. Wanneer u probeert toe te voegen van de map, controleert Azure AD Connect-wizard in de status van het kenmerk ms-DS-ConsistencyGuid in de map. Als het kenmerk is geconfigureerd op een of meer objecten in de map, wordt de wizard concludeert het kenmerk wordt gebruikt door andere toepassingen en een foutmelding zoals wordt geïllustreerd in het onderstaande diagram. Als u er zeker van zijn dat het kenmerk wordt niet door bestaande toepassingen gebruikt, kunt u de fout onderdrukken door opnieuw starten van de Azure AD Connect-wizard met de **/SkipLdapSearchcontact** , zoals hierboven beschreven opgegeven of moet u contact opnemen met Ondersteuning voor meer informatie.

![Nieuwe mappen toevoegen aan bestaande implementatie](./media/plan-connect-design-concepts/consistencyGuid-04.png)

## <a name="azure-ad-sign-in"></a>Aanmelding bij Azure AD
Bij het integreren van uw on-premises directory met Azure AD, is het belangrijk om te begrijpen hoe de synchronisatie-instellingen kunnen invloed is op de manier waarop-gebruiker wordt geverifieerd. Azure AD maakt gebruik van userPrincipalName (UPN) om de gebruiker te verifiëren. Als u uw gebruikers synchroniseert, moet u echter het kenmerk moet worden gebruikt voor de waarde van de userPrincipalName zorgvuldig kiezen.

### <a name="choosing-the-attribute-for-userprincipalname"></a>Het kenmerk voor de userPrincipalName kiezen
Bij het selecteren van het kenmerk voor het leveren van de waarde van de UPN moet worden gebruikt in Azure een moet ervoor zorgen

* De waarden in overeenstemming zijn met de UPN-syntaxis (RFC 822), die is het kenmerk moet de indeling gebruikersnaam\@domein
* Het achtervoegsel van de waarden komt overeen met een van de gecontroleerde aangepaste domeinen in Azure AD

In de express-instellingen wordt de veronderstelde keuze voor het kenmerk userPrincipalName. Als het kenmerk userPrincipalName niet de waarde bevat u wilt uw gebruikers zich aanmelden bij Azure, dan moet u **aangepaste installatie**.

### <a name="custom-domain-state-and-upn"></a>Status van het aangepaste domein- en UPN
Het is belangrijk om ervoor te zorgen dat er een geverifieerd domein voor het UPN-achtervoegsel.

John is een gebruiker in contoso.com. U wilt dat Jan gebruik van de on-premises UPN john\@contoso.com zich aanmeldt bij Azure, nadat u er gebruikers zijn gesynchroniseerd met uw Azure AD-directory contoso.onmicrosoft.com. Om dit te doen, die u wilt toevoegen en controleren van contoso.com als een aangepast domein in Azure AD voordat u kunt beginnen met het synchroniseren van de gebruikers. Als het UPN-achtervoegsel van John, bijvoorbeeld contoso.com is, komt niet overeen met een geverifieerd domein in Azure AD, klikt u vervolgens vervangen Azure AD door het UPN-achtervoegsel contoso.onmicrosoft.com.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Niet-routeerbare on-premises domeinen en UPN voor Azure AD
Sommige organisaties hebben niet-routeerbare domeinen, zoals contoso.local, of domeinen met een eenvoudige één label, zoals contoso. U bent niet kunnen verifiëren van een niet-routeerbare domein in Azure AD. Azure AD Connect kunt synchroniseren met alleen een geverifieerd domein in Azure AD. Wanneer u een Azure AD-directory maken, maakt het een routeerbare domein dat standaarddomein voor uw Azure AD bijvoorbeeld contoso.onmicrosoft.com wordt. Het wordt daarom, die nodig zijn om te controleren of andere routeerbare domeinen in een dergelijk scenario in het geval u niet wilt synchroniseren met het standaarddomein onmicrosoft.com.

Lezen [uw aangepaste domeinnaam toevoegen aan Azure Active Directory](../active-directory-domains-add-azure-portal.md) voor meer informatie over het toevoegen en verifiëren van domeinen.

Azure AD Connect detecteert als u in een omgeving met niet-routeerbare domeinen worden uitgevoerd en u op de juiste wijze waarschuwt dan verder kunnen gaan met express-instellingen. Als u in een niet-routeerbare domein werkt, is het waarschijnlijk dat de UPN, van de gebruikers, niet-routeerbare achtervoegsels te hebben. Bijvoorbeeld, als u worden uitgevoerd onder contoso.local, stelt Azure AD Connect u de aangepaste instellingen in plaats van met expresinstellingen gebruiken. Met aangepaste instellingen voor kan u zijn om op te geven het kenmerk dat moet worden gebruikt als de UPN voor het aanmelden bij Azure, nadat de gebruikers zijn gesynchroniseerd met Azure AD.

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).
