---
title: 'Azure AD Connect: Ontwerpen concepten | Microsoft Docs'
description: In dit onderwerp beschrijft bepaalde gebieden van implementatie ontwerpen
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 4114a6c0-f96a-493c-be74-1153666ce6c9
ms.service: active-directory
ms.custom: azure-ad-connect
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 510efc4ae6674a3987c2bb5d7cd155ea8c710c83
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-design-concepts"></a>Azure AD Connect: Ontwerpconcepten
Het doel van dit onderwerp is om gebieden die moeten worden beschouwd door bij het implementatieontwerp van Azure AD Connect te beschrijven. In dit onderwerp is een deep dive op bepaalde gebieden en deze concepten worden kort beschreven in andere onderwerpen ook.

## <a name="sourceanchor"></a>sourceAnchor
Het kenmerk sourceAnchor is gedefinieerd als *een kenmerk onveranderbaar tijdens de levensduur van een object*. Dit is de unieke identificatie van een object dat de dezelfde object on-premises en in Azure AD. Het kenmerk is een afkorting **onveranderbare id genoemd** en de twee namen uitwisselbaar worden gebruikt.

De niet-wijzigbaar, word die 'kan niet worden gewijzigd', is belangrijk dat u dit onderwerp. Omdat de waarde van dit kenmerk kan niet worden gewijzigd nadat deze is ingesteld, is het belangrijk dat u een ontwerp die ondersteuning biedt voor uw scenario selecteren.

Het kenmerk wordt gebruikt voor de volgende scenario's:

* Wanneer een nieuwe server van de synchronisatie-engine is gemaakt of opnieuw worden opgebouwd na een noodherstelscenario, koppelt u dit kenmerk bestaande objecten in Azure AD met objecten on-premises.
* Als u van een identiteit alleen in de cloud naar een identiteitsmodel gesynchroniseerde verplaatst, kan dit kenmerk objecten 'harde match' bestaande objecten in Azure AD met lokale objecten.
* Als u gebruikmaakt van Federatie en vervolgens dit kenmerk samen met de **userPrincipalName** in de claim wordt gebruikt als unieke identificatie van een gebruiker.

In dit onderwerp alleen wordt gesproken over sourceAnchor met betrekking tot de gebruikers. De dezelfde regels gelden voor alle objecttypen, maar dit is alleen bestemd voor gebruikers dat dit probleem is meestal een belangrijk.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Een goede sourceAnchor kenmerk selecteren
De kenmerkwaarde moet de volgende regels volgen:

* Minder dan 60 tekens bestaan
  * Tekens die niet wordt a-z, A-Z en 0-9 zijn gecodeerd en geteld als 3 tekens
* Speciale tekens niet bevatten: &#92;! # $ % & * + / = ? ^ &#96; { } | ~ < > ( ) ' ; : , [ ] " @ _
* Globaal uniek zijn
* Moet een tekenreeks, geheel getal of binair
* Moet niet zijn gebaseerd op de naam van gebruiker deze wijzigingen
* Niet mogen worden hoofdlettergevoelig en te voorkomen dat de waarden die per aanvraag verschillen kunnen
* Moet worden toegewezen als het object is gemaakt

Als de geselecteerde sourceAnchor is niet van het type string, vervolgens Azure AD Connect Base64Encode de waarde van het kenmerk om te controleren of er geen speciale tekens worden weergegeven. Als u een andere federatieserver dan de AD FS gebruikt, controleert u of de server kan ook Base64Encode het kenmerk.

Het kenmerk sourceAnchor is hoofdlettergevoelig. Een waarde van 'Jandevries' is niet hetzelfde als 'jandevries'. Maar u mag geen twee verschillende objecten met alleen een verschil in geval.

Als u één forest op locatie, klikt u vervolgens het kenmerk moet u is **objectGUID**. Dit is ook het kenmerk wordt gebruikt wanneer u snelle instellingen in Azure AD Connect gebruiken en ook het kenmerk wordt gebruikt door DirSync.

Als u meerdere forests hebt en niet gebruikers tussen forests en domeinen, klikt u vervolgens verplaatsen **objectGUID** een goed kenmerk kiezen zelfs in dit geval is.

Als u gebruikers tussen forests en domeinen verplaatst, moet vervolgens u een kenmerk dat niet verandert of kan worden verplaatst met de gebruikers tijdens het verplaatsen. Een aanbevolen aanpak is te integreren van een synthetische kenmerk. Een kenmerk kan waarin iets dat op een GUID lijkt zijn geschikt is. Tijdens het maken van het object is een nieuwe GUID gemaakt en geautoriseerd op de gebruiker. Een aangepaste synchronisatieregel kan worden gemaakt in de synchronisatie-engine-server voor het maken van deze waarde op basis van de **objectGUID** en het geselecteerde kenmerk in ADDS bijwerken. Wanneer u het object verplaatst, zorg ervoor dat de inhoud van deze waarde ook gekopieerd.

Er is een andere oplossing kiest u een bestaand kenmerk weet u niet gewijzigd. Gangbare kenmerken zijn **werknemer-id**. Als u een kenmerk dat letters bevat overweegt, zorg ervoor dat er zonder dat het geval (hoofdletters en kleine letters) voor de waarde van het kenmerk kunt wijzigen. Ongeldige kenmerken die mag niet worden gebruikt, zijn deze kenmerken met de naam van de gebruiker. In een huwelijk of echtscheiding, wordt de naam wilt wijzigen, verwacht dat niet is toegestaan voor dit kenmerk. Dit is ook een reden waarom kenmerken zoals **userPrincipalName**, **mail**, en **targetAddress** zijn niet ook mogelijk om in de Azure AD Connect-installatiewizard te selecteren. Deze kenmerken bevatten ook de ' @ ' is niet toegestaan in de sourceAnchor teken.

### <a name="changing-the-sourceanchor-attribute"></a>Het kenmerk sourceAnchor wijzigen
De waarde van het kenmerk sourceAnchor kan niet worden gewijzigd nadat het object is gemaakt in Azure AD en de identiteit is gesynchroniseerd.

Daarom gelden de volgende beperkingen naar Azure AD Connect:

* Het kenmerk sourceAnchor kan alleen worden ingesteld tijdens de eerste installatie. Als u de installatiewizard opnieuw, wordt deze optie is alleen-lezen. Als u moet deze instelling te wijzigen, moet u verwijderen en opnieuw installeren.
* Als u een andere Azure AD Connect-server installeert, moet u hetzelfde sourceAnchor kenmerk zoals eerder gebruikt selecteren. Als u eerder hebt gebruikt als DirSync en verplaatsen naar Azure AD Connect, dan moet u **objectGUID** omdat dat het kenmerk wordt gebruikt door DirSync.
* Als de waarde voor sourceAnchor is gewijzigd na werd het object geëxporteerd naar Azure AD, vervolgens Azure AD Connect sync een fout genereert en niet in staat geen wijzigingen meer op dat object voordat u het probleem is opgelost en de sourceAnchor terug in de bronmap is gewijzigd.

## <a name="using-msds-consistencyguid-as-sourceanchor"></a>Met behulp van msDS-ConsistencyGuid als sourceAnchor
Standaard wordt in Azure AD Connect (versie 1.1.486.0 en ouder) objectGUID gebruikt als het kenmerk sourceAnchor. ObjectGUID is door het systeem gegenereerd. U kunt de waarde bij het maken van lokale AD-objecten niet opgeven. Zoals wordt beschreven in de sectie [sourceAnchor](#sourceanchor), zijn er scenario's waarbij u moet de waarde sourceAnchor op te geven. Als de scenario's van toepassing op u zijn, moet u een configureerbare AD-kenmerk (bijvoorbeeld msDS-ConsistencyGuid) gebruiken als het kenmerk sourceAnchor.

Azure AD Connect (versie 1.1.524.0 en na) nu vereenvoudigt het gebruik van msDS-ConsistencyGuid als kenmerk sourceAnchor. Wanneer u deze functie gebruikt, configureert Azure AD Connect automatisch de synchronisatieregels naar:

1. Gebruik msDS-ConsistencyGuid als het kenmerk sourceAnchor voor gebruikersobjecten. ObjectGUID wordt gebruikt voor andere objecttypen.

2. Voor een gegeven on-premises AD-gebruiker object waarvan het kenmerk msDS-ConsistencyGuid niet is ingevuld, Azure AD Connect schrijfbewerkingen back-de object-GUID-waarde met het kenmerk msDS-ConsistencyGuid in de lokale Active Directory. Nadat het kenmerk msDS-ConsistencyGuid is ingevuld, wordt in Azure AD Connect daarna het object naar Azure AD exporteert.

>[!NOTE]
> Eenmaal een on-premises AD-object is geïmporteerd in Azure AD Connect (die is geïmporteerd in de ruimte van AD-Connector en de geschatte naar de Metaverse), u kunt de waarde sourceAnchor niet meer wijzigen. Om op te geven van de waarde sourceAnchor voor een opgegeven lokale AD object, het configureren van het kenmerk msDS-ConsistencyGuid voordat deze wordt geïmporteerd in Azure AD Connect.

### <a name="permission-required"></a>Vereiste machtiging
Voor deze functie te gebruiken, moet de AD DS-account gebruikt om te synchroniseren met Active Directory lokale schrijfmachtiging voor het kenmerk msDS-ConsistencyGuid in de lokale Active Directory worden verleend.

### <a name="how-to-enable-the-consistencyguid-feature---new-installation"></a>Het inschakelen van de functie ConsistencyGuid - nieuwe installatie
U kunt het gebruik van ConsistencyGuid als sourceAnchor inschakelen tijdens de installatie van nieuwe. Deze sectie worden zowel Express en aangepaste installatie meer informatie.

  >[!NOTE]
  > Alleen nieuwere versies van Azure AD Connect (1.1.524.0 en na) ondersteunt het gebruik van ConsistencyGuid als sourceAnchor tijdens de installatie van nieuwe.

### <a name="how-to-enable-the-consistencyguid-feature"></a>Het inschakelen van de functie ConsistencyGuid
Op dit moment wordt kan de functie alleen worden ingeschakeld tijdens alleen nieuwe Azure AD Connect-installatie.

#### <a name="express-installation"></a>Snelle installatie
Als u Azure AD Connect installeert met de snelle modus, wordt het meest geschikte AD-kenmerk moet worden gebruikt als het kenmerk sourceAnchor met behulp van de volgende logica automatisch bepaald door de Azure AD Connect-wizard:

* De Azure AD Connect-wizard query eerst uw Azure AD-tenant voor het ophalen van de AD-kenmerk dat wordt gebruikt als het kenmerk sourceAnchor in de vorige Azure AD Connect-installatie (indien aanwezig). Als deze informatie beschikbaar is, wordt in Azure AD Connect maakt gebruik van hetzelfde kenmerk voor AD.

  >[!NOTE]
  > Alleen nieuwere versies van Azure AD Connect (1.1.524.0 en na) slaat informatie over het kenmerk sourceAnchor in uw Azure AD-tenant die wordt gebruikt tijdens de installatie. Oudere versies van Azure AD Connect niet.

* Als u informatie over het kenmerk sourceAnchor gebruikt niet beschikbaar is, controleert de wizard de status van het kenmerk msDS-ConsistencyGuid in uw lokale Active Directory. Als het kenmerk van een object in de directory niet is geconfigureerd, gebruikt de wizard de msDS-ConsistencyGuid als het kenmerk sourceAnchor. Als het kenmerk is geconfigureerd op een of meer objecten in Active directory, concludeert de wizard het kenmerk wordt gebruikt door andere toepassingen en is niet geschikt is als het kenmerk sourceAnchor...

* De wizard valt in dat geval terug voor het gebruik van object-GUID als het kenmerk sourceAnchor.

* Als het kenmerk sourceAnchor is besloten, slaat de wizard de informatie in uw Azure AD-tenant. De informatie wordt gebruikt door toekomstige installatie van Azure AD Connect.

Wanneer de Express-installatie is voltooid, wordt de wizard informeert welk kenmerk is opgenomen als het kenmerk Bronanker.

![Wizard informeert AD-kenmerk voor sourceAnchor verzameld](./media/active-directory-aadconnect-design-concepts/consistencyGuid-01.png)

#### <a name="custom-installation"></a>Aangepaste installatie
Als u Azure AD Connect installeert met de aangepaste modus, biedt de Azure AD Connect-wizard twee opties bij het kenmerk sourceAnchor configureren:

![Aangepaste installatie - sourceAnchor configuratie](./media/active-directory-aadconnect-design-concepts/consistencyGuid-02.png)

| Instelling | Beschrijving |
| --- | --- |
| Azure het bronanker voor mij laten beheren | Selecteer deze optie als u wilt dat Azure AD het kenmerk voor u selecteert. Als u deze optie selecteert, Azure AD Connect-wizard past dezelfde [sourceAnchor kenmerk selectie logica wordt gebruikt tijdens de snelle installatie](#express-installation). Net als bij normale installatie, de wizard informeert u welk kenmerk is opgenomen als het kenmerk Bronanker nadat aangepaste installatie is voltooid. |
| Een specifiek kenmerk | Selecteer deze optie als u een bestaand AD-kenmerk opgeeft als het kenmerk sourceAnchor. |

### <a name="how-to-enable-the-consistencyguid-feature---existing-deployment"></a>Het inschakelen van de functie ConsistencyGuid - bestaande implementatie
Als u een bestaande implementatie van Azure AD Connect dit objectGUID wordt gebruikt als het kenmerk Bronanker hebt, kunt u deze kunt overschakelen naar ConsistencyGuid te gebruiken.

>[!NOTE]
> Alleen nieuwere versies van Azure AD Connect (1.1.552.0 en na) ondersteunt overschakelen van ObjectGuid naar ConsistencyGuid als het kenmerk Bronanker.

Overschakelen van objectGUID naar ConsistencyGuid als het kenmerk Bronanker:

1. De Azure AD Connect-wizard starten en klik op **configureren** naar het scherm taken.

2. Selecteer de **Bronanker configureren** taak optie en klik op **volgende**.

   ![ConsistencyGuid inschakelen voor bestaande implementatie - stap 2](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment01.png)

3. Voer uw referenties in Azure AD-beheerder en klik op **volgende**.

4. Azure AD Connect-wizard analyseert de status van het kenmerk msDS-ConsistencyGuid in uw lokale Active Directory. Als het kenmerk is niet geconfigureerd op een object in de directory, Azure AD Connect dat er geen andere toepassing momenteel wordt gebruikt door het kenmerk en is veilig om te gebruiken als het kenmerk Bronanker wordt afgesloten. Klik op **volgende** om door te gaan.

   ![ConsistencyGuid inschakelen voor bestaande implementatie - stap 4](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment02.png)

5. In de **gereed om te configureren** scherm, klikt u op **configureren** waarmee de configuratie wijzigen.

   ![ConsistencyGuid inschakelen voor bestaande implementatie - stap 5](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment03.png)

6. Zodra de configuratie is voltooid, wordt de wizard geeft aan dat msDS-ConsistencyGuid nu als het kenmerk Bronanker wordt gebruikt.

   ![ConsistencyGuid inschakelen voor bestaande implementatie - stap 6](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment04.png)

Tijdens de analyse (stap 4) als het kenmerk is geconfigureerd op een of meer objecten in de map en de wizard concludeert de domeincontroller het kenmerk wordt gebruikt door een andere toepassing en een foutmelding zoals geïllustreerd in het onderstaande diagram. Deze fout kan ook optreden als u de functie ConsistencyGuid eerder hebt ingeschakeld op uw primaire Azure AD Connect-server en u probeert te doen op de testserver hetzelfde.

![ConsistencyGuid inschakelen voor bestaande implementatie - fout](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeploymenterror.png)

 Als u er zeker van bent dat het kenmerk wordt niet door andere bestaande toepassingen gebruikt, kunt u de fout onderdrukken door opnieuw starten van de Azure AD Connect-wizard met de **/SkipLdapSearchcontact** opgegeven. Voer de volgende opdracht in de opdrachtprompt om dit te doen:

```
"c:\Program Files\Microsoft Azure Active Directory Connect\AzureADConnect.exe" /SkipLdapSearch
```

### <a name="impact-on-ad-fs-or-third-party-federation-configuration"></a>Gevolgen voor de AD FS of van derden federation-configuratie
Als u gebruikmaakt van Azure AD Connect beheren on-premises AD FS-implementatie, de Azure AD Connect de claimregels voor het gebruik van hetzelfde kenmerk voor AD als sourceAnchor automatisch bijgewerkt. Dit zorgt ervoor dat de claim onveranderbare id genoemd is gegenereerd door AD FS consistent met de waarden sourceAnchor is geëxporteerd naar Azure AD is.

Als u AD FS buiten Azure AD Connect beheert, of u federatieservers van derden voor verificatie gebruikt, moet u handmatig de claimregels voor onveranderbare id genoemd claim consistent zijn met de waarden sourceAnchor is geëxporteerd naar Azure AD, zoals beschreven in artikel sectie bijwerken [wijzigen AD FS claimregels](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-management#modclaims). Nadat de installatie voltooid is, retourneert de wizard de volgende waarschuwing:

![Federatieconfiguratie van derden](./media/active-directory-aadconnect-design-concepts/consistencyGuid-03.png)

### <a name="adding-new-directories-to-existing-deployment"></a>Nieuwe mappen toevoegen aan bestaande implementatie
Stel dat u Azure AD Connect hebt geïmplementeerd met de functie ConsistencyGuid is ingeschakeld en u wilt nu een andere directory toevoegen aan de implementatie. Wanneer u de map toevoegen, controleert met Azure AD Connect-wizard de status van het kenmerk mSDS-ConsistencyGuid in de map. Als het kenmerk is geconfigureerd op een of meer objecten in Active directory, de wizard het kenmerk wordt gebruikt door andere toepassingen en een foutmelding zoals geïllustreerd in het onderstaande diagram wordt afgesloten. Als u er zeker van bent dat het kenmerk wordt niet door bestaande toepassingen gebruikt, moet u contact op met ondersteuning voor meer informatie over het onderdrukken van de fout.

![Nieuwe mappen toevoegen aan bestaande implementatie](./media/active-directory-aadconnect-design-concepts/consistencyGuid-04.png)

## <a name="azure-ad-sign-in"></a>Aanmelding bij Azure AD
Bij het integreren van uw on-premises directory met Azure AD, is het belangrijk om te begrijpen hoe de synchronisatie-instellingen invloed hebben op de gebruiker manier wordt geverifieerd. Azure AD gebruikt userPrincipalName (User Principal Name) voor het verifiëren van de gebruiker. Wanneer u uw gebruikers synchroniseert, moet u het kenmerk moet worden gebruikt voor de waarde van userPrincipalName zorgvuldig kiezen.

### <a name="choosing-the-attribute-for-userprincipalname"></a>Het kenmerk voor userPrincipalName kiezen
Bij het selecteren van het kenmerk voor het ontwikkelen van controleren de waarde van de UPN om te worden gebruikt in Azure een

* De kenmerkwaarden weer in overeenstemming zijn met de syntaxis van de UPN (RFC 822), die dat deze moet de indelingusername@domain
* Het achtervoegsel in de waarden overeenkomt met een van de geverifieerde aangepaste domeinen in Azure AD

In de express-instellingen wordt de veronderstelde keuze is voor het kenmerk userPrincipalName. Als het kenmerk userPrincipalName geen waarde u wilt dat gebruikers aan te melden bij Azure en vervolgens moet u **aangepaste installatie**.

### <a name="custom-domain-state-and-upn"></a>Status van het aangepaste domein en UPN
Het is belangrijk om ervoor te zorgen dat er een geverifieerde domein voor het UPN-achtervoegsel is.

Johan is een gebruiker in contoso.com. U wilt dat Jeroen gebruik van de lokale UPN john@contoso.com aan te melden bij Azure nadat u gebruikers aan uw Azure AD-directory contoso.onmicrosoft.com zijn gesynchroniseerd. Om dit te doen, die u wilt toevoegen en verifiëren van contoso.com als een aangepast domein in Azure AD voordat u kunt beginnen met het synchroniseren van de gebruikers. Als de UPN-achtervoegsel van Jan, bijvoorbeeld contoso.com is, komt niet overeen met een geverifieerde domeinnaam in Azure AD, vervolgens vervangen Azure AD door het UPN-achtervoegsel contoso.onmicrosoft.com.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Niet-routeerbaar on-premises domeinen en UPN voor Azure AD
Sommige organisaties hebben niet-routeerbare domeinen, zoals contoso.local of eenvoudige enkelvoudige domeinen zoals contoso. U bent geen een niet-routeerbare domein verifiëren in Azure AD. Azure AD Connect kunt synchroniseren met alleen een geverifieerde domeinnaam in Azure AD. Wanneer u een Azure Active directory maakt, maakt u een routeerbaar domein dat standaarddomein voor uw Azure AD, bijvoorbeeld: contoso.onmicrosoft.com wordt. Het wordt daarom nodig zijn om te controleren of een ander domein in een dergelijk scenario routeerbaar als u niet wilt synchroniseren naar het standaarddomein onmicrosoft.com.

Lees [uw aangepaste domeinnaam toevoegen aan Azure Active Directory](../active-directory-domains-add-azure-portal.md) voor meer informatie over het toevoegen en verifiëren van domeinen.

Azure AD Connect detecteert als u in een niet-routeerbare domeinomgeving uitvoert en zou u op de juiste wijze van u wilt doorgaan met expresinstellingen waarschuwen. Als u in een niet-routeerbare domein werkt, is het waarschijnlijk dat de UPN van de gebruikers niet-routeerbare achtervoegsels te hebben. Bijvoorbeeld, als u worden uitgevoerd onder contoso.local, stelt Azure AD Connect u de aangepaste instellingen in plaats van met expresinstellingen gebruiken. Met aangepaste instellingen voor zijn u opgeven van het kenmerk op dat moet worden gebruikt als de UPN aan te melden bij Azure nadat de gebruikers zijn gesynchroniseerd naar Azure AD.

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](active-directory-aadconnect.md).
