---
title: Azure AD Connect - AD FS-vertrouwensrelatie met Azure AD met behulp van Azure AD Connect beheren | Microsoft Docs
description: Operationele gegevens van Azure AD-vertrouwensrelatie verwerking door Azure AD connect.
keywords: AD FS, ADFS, AD FS-beheer, AAD Connect, Connect, Azure AD-vertrouwensrelatie AAD, claim, claim, aanspraak op regels, uitgifte, transformatie, regels, back-up, herstel
services: active-directory
documentationcenter: ''
ms.reviewer: anandyadavmsft
manager: mtillman
ms.component: hybrid
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2018
ms.author: billmath
author: billmath
ms.custom: ''
ms.openlocfilehash: 4eeb716e4eeae8dc560f9588c6c3ce252daf741e
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46314662"
---
# <a name="manage-ad-fs-trust-with-azure-ad-using-azure-ad-connect"></a>AD FS-vertrouwensrelatie met Azure AD met behulp van Azure AD Connect beheren

## <a name="overview"></a>Overzicht

Azure AD Connect kunt beheren federatie tussen on-premises Active Directory Federation Service (AD FS) en Azure AD. In dit artikel biedt een overzicht van:

* De verschillende instellingen geconfigureerd op de vertrouwensrelatie met Azure AD Connect
* De transformatieregels voor uitgifte (claimregels) instellen met Azure AD Connect
* Back-up en herstellen van de claim regels tussen upgrades en configuratie van updates. 

## <a name="settings-controlled-by-azure-ad-connect"></a>Instellingen die worden beheerd door Azure AD Connect

Azure AD Connect beheert **alleen** instellingen met betrekking tot Azure AD-vertrouwensrelatie. Azure AD Connect wijzigt niet alle instellingen op andere vertrouwensrelaties met relying party's in AD FS. De volgende tabel staan de instellingen die worden beheerd door Azure AD Connect.

| Instelling | Beschrijving |
| :--- | :--- |
| Certificaat voor token-ondertekening | Azure AD Connect kan worden gebruikt om te herstellen en opnieuw maken van de vertrouwensrelatie met Azure AD. Azure AD Connect biedt een eenmalige direct rollover van certificaten voor tokenondertekening voor AD FS en updates van de federatie-instellingen van Azure AD-domein.|
| Token ondertekeningsalgoritme | Microsoft adviseert als het token-ondertekening algoritme SHA-256 aan. Azure AD Connect kan worden gedetecteerd als het algoritme voor token-ondertekening is ingesteld op een waarde minder veilig dan het SHA-256. Deze wordt de instelling worden bijgewerkt naar SHA-256 in de volgende mogelijke configuratie-bewerking. |
| Azure AD-id voor vertrouwensrelatie | Azure AD Connect stelt de juiste id-waarde voor de Azure AD-vertrouwensrelatie. AD FS identificatie unieke van de Azure AD-vertrouwensrelatie met de id-waarde. |
| Azure AD-eindpunten | Azure AD Connect zorgt ervoor dat de eindpunten zijn geconfigureerd voor de Azure AD-vertrouwensrelatie altijd aan de hand van de meest recente aanbevolen waarden voor tolerantie en prestaties wordt. |
| Transformatieregels voor uitgifte | Er zijn getallen van de claimregels die nodig zijn voor optimale prestaties van de functies van Azure AD in een federatieve omgeving. Azure AD Connect zorgt ervoor dat de Azure AD-vertrouwensrelatie altijd is geconfigureerd met de juiste set met aanbevolen claimregels. |
| Alternatieve-id | Als synchronisatie is geconfigureerd voor het gebruik van alternatieve-id, configureert Azure AD Connect AD FS voor het uitvoeren van verificatie met behulp van alternatieve-id. |
| De metagegevens van de automatische update | Vertrouwensrelatie met Azure AD is geconfigureerd voor de metagegevens van de automatische update. AD FS wordt regelmatig gecontroleerd van de metagegevens van de Azure AD-vertrouwensrelatie en bijgehouden in het geval er aan de Azure AD wijzigingen. |
| Geïntegreerde Windows-verificatie (IWA) | Tijdens de hybride Azure AD join-bewerking, is IWA ingeschakeld voor device Registration service om Hybrid Azure AD join voor downlevel-apparaten mogelijk te maken |

## <a name="execution-flows-and-federation-settings-configured-by-azure-ad-connect"></a>Uitvoering van stromen en federatie-instellingen die zijn geconfigureerd door Azure AD Connect

Azure AD connect niet alle instellingen voor Azure AD-vertrouwensrelatie bijgewerkt tijdens de configuratie van stromen. De instellingen die zijn gewijzigd, is afhankelijk van welke taak of het uitvoeren van de stroom wordt uitgevoerd. De volgende tabel bevat de instellingen die in andere stromen beïnvloed.

| Uitvoering van flow | Instellingen die worden beïnvloed |
| :--- | :--- |
| Eerste pass-installatie (snelle) | Geen |
| Eerst geven installatie (nieuwe AD FS-farm) | Een nieuwe AD FS-farm is gemaakt en een vertrouwensrelatie met Azure AD helemaal is gemaakt. |
| Eerst geven installatie (bestaande AD FS-farm en bestaande Azure AD-vertrouwensrelatie) | Azure AD-id voor vertrouwensrelatie, transformatieregels voor uitgifte, Azure AD-eindpunten, alternatieve-id (indien nodig), de metagegevens van de automatische update |
| Azure AD-vertrouwensrelatie opnieuw instellen | Token handtekeningcertificaat voor Token-ondertekening algoritme, Azure AD-id voor vertrouwensrelatie, transformatie uitgifte regels, Azure AD-eindpunten, alternatieve-id (indien nodig), de metagegevens van de automatische update |
| Federatieve server toevoegen | Geen |
| WAP-server toevoegen | Geen |
| Apparaatopties | Transformatieregels voor uitgifte, IWA voor device Registration service |
| Federatief domein toevoegen | Als het domein wordt toegevoegd voor de eerste keer, dat wil zeggen, de installatie van Federatie met één domein wordt gewijzigd in meerdere domeinen federation: Azure AD Connect maakt de vertrouwensrelatie helemaal opnieuw. Als de vertrouwensrelatie met Azure AD al is geconfigureerd voor meerdere domeinen, worden alleen transformatieregels voor uitgifte gewijzigd |
| Bijwerken van SSL | Geen |

Tijdens alle bewerkingen, waarin elke instelling die is gewijzigd, Azure AD Connect maakt een back-up van de huidige instellingen voor vertrouwen op **%ProgramData%\AADConnect\ADFS**

![Azure AD Connect pagina weergegeven bericht over bestaande Azure AD-vertrouwensrelatie back-up](./media/how-to-connect-azure-ad-trust/backup2.png)

> [!NOTE]
> Voorafgaand aan versie 1.1.873.0, de back-up bestond uit alleen transformatieregels voor uitgifte en ze zijn back-ups in de wizard traceringslogboekbestand.

## <a name="issuance-transform-rules-set-by-azure-ad-connect"></a>Transformatieregels voor uitgifte ingesteld door Azure AD Connect

Azure AD Connect zorgt ervoor dat de Azure AD-vertrouwensrelatie altijd is geconfigureerd met de juiste set met aanbevolen claimregels. Microsoft adviseert het gebruik van Azure AD connect voor het beheren van uw Azure AD-vertrouwensrelatie. Deze sectie vindt u de set uitgifte transformatie regels en de bijbehorende beschrijvingen.

| Regelnaam | Beschrijving |
| --- | --- |
| Probleem UPN | Deze regel worden de waarde van userprincipalname-02-het kenmerk dat is geconfigureerd in de synchronisatie-instellingen voor de userprincipalname in een query uitgevoerd.|
| Query objectguid en msdsconsistencyguid voor aangepaste ImmutableId-claim | Deze regel wordt een tijdelijke waarde in de pijplijn voor objectguid en msdsconsistencyguid waarde als deze bestaat |
| Controleren of er sprake van msdsconsistencyguid | Afhankelijk van of de waarde voor msdsconsistencyguid of niet bestaat, wordt een tijdelijke vlag om te leiden van wat u wilt gebruiken als ImmutableId instellen |
| Msdsconsistencyguid uitgeven als onveranderbare ID, indien aanwezig | Msdsconsistencyguid uitgeven als ImmutableId als de waarde bestaat |
| ObjectGuidRule uitgeven als msdsConsistencyGuid regel niet bestaat | Als de waarde voor msdsconsistencyguid niet bestaat, worden de waarde van objectguid uitgegeven als ImmutableId |
| Uitgeven van nameidentifier | Deze regel geeft de waarde voor de nameidentifier-claim.|
| Probleem accounttype voor domeincomputers | Als de entiteit die wordt geverifieerd een domein gekoppelde apparaat is, deze regel geeft het accounttype als DJ wat inhoudt van een domein gekoppelde apparaat |
| AccountType probleem met de waarde van de gebruiker wanneer het is niet een computeraccount | Als de entiteit die wordt geverifieerd een gebruiker is, geeft deze regel het accounttype als gebruiker |
| Issuerid uitgeven als dit niet een computeraccount | Deze regel geeft de issuerId-waarde wanneer de verificatie entiteit niet een apparaat is. De waarde wordt gemaakt via een reguliere expressie die is geconfigureerd door Azure AD Connect. De reguliere expressie wordt gemaakt nadat rekening houdend met alle domeinen gefedereerd met Azure AD Connect. |
| Probleem issuerid voor DJ computer auth | Deze regel geeft de issuerId-waarde als de verificatie entiteit een apparaat is |
| Probleem onpremobjectguid voor domeincomputers | Als de entiteit die wordt geverifieerd een domein gekoppelde apparaat is, problemen met deze regel met de on-premises objectguid voor het apparaat |
| Primaire SID doorgeven | Deze regel geeft de primaire SID van de verificatie-entiteit |
| Claim - insideCorporateNetwork doorgeven | Problemen met deze regel een claim waarmee Azure AD kent als de verificatie afkomstig is van binnen bedrijfsnetwerk of extern |
| Claim – Psso doorgeven |   |
| Wachtwoord verlopen Claims uitgeven | Deze regel problemen drie claims voor Wachtwoordverlooptijd, het aantal dagen voor het wachtwoord voor het verlopen van de entiteit die wordt geverifieerd en de URL waar u de route voor het wijzigen van het wachtwoord.|
| Claim – authnmethodsreferences doorgeven | De waarde in de claim uitgegeven onder deze regel geeft aan welk type verificatie is uitgevoerd voor de entiteit |
| Claim - multifactorauthenticationinstant doorgeven | De waarde van deze claim geeft de tijd in UTC, wanneer de gebruiker verificatie op basis van meerdere factoren voor het laatst uitgevoerd. |
| Claim - AlternateLoginID doorgeven | Deze regel verleent de claim AlternateLoginID als de verificatie is uitgevoerd met behulp van alternatieve aanmeldings-ID. |

> [!NOTE]
> De claimregels voor probleem UPN en ImmutableId zullen verschillen als u niet-standaard keuze tijdens het configureren van Azure AD Connect gebruiken

## <a name="restore-issuance-transform-rules"></a>Herstellen van de transformatieregels voor uitgifte

Azure AD Connect versie 1.1.873.0 of hoger maakt vertrouwt een back-up van de Azure AD instellingen wanneer er een update is aangebracht in de instellingen van Azure AD-vertrouwensrelatie. De instellingen van Azure AD-vertrouwensrelatie back-ups op **%ProgramData%\AADConnect\ADFS**. Naam van het bestand is in de volgende indeling AadTrust -&lt;datum&gt;-&lt;tijd&gt;.txt, bijvoorbeeld - AadTrust-20180710-150216.txt

![Een sanpshot van voorbeeld van de back-ups van Azure AD-vertrouwensrelatie](./media/how-to-connect-azure-ad-trust/backup.png)

U kunt de transformatieregels voor uitgifte met behulp van de voorgestelde stappen hieronder herstellen

1. Open de gebruikersinterface van de AD FS-beheer in Serverbeheer
2. De eigenschappen van de Azure AD-vertrouwensrelatie openen door te gaan **AD FS &gt; Relying Party Trusts &gt; Identiteitsplatform van Microsoft Office 365 &gt; Claims uitgiftebeleid bewerken**
3. Klik op **regel toevoegen**
4. In de claimregelsjabloon, selecteert u verzenden Claims met behulp van een aangepaste regel en klikt u op **volgende**
5. De naam van de claimregel van back-upbestand Kopieer en plak deze in het veld **naam van Claimregel**
6. De claimregel kopiëren van back-upbestand in het tekstveld voor **aangepaste regel** en klikt u op **voltooien**

> [!NOTE]
> Zorg ervoor dat uw extra regels niet in strijd is met de regels die door Azure AD Connect geconfigureerd.

## <a name="next-steps"></a>Volgende stappen
* [Beheren en aanpassen van Active Directory Federation Services met Azure AD Connect](how-to-connect-fed-management.md)