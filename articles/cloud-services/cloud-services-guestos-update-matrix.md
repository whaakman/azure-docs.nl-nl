---
title: Meer informatie over de meest recente Azure Guest OS Releases | Microsoft Docs
description: De meest recente release nieuws en de compatibiliteit van de SDK voor Azure Cloud Services-Gastbesturingssysteem.
services: cloud-services
documentationcenter: na
author: raiye
editor: ''
ms.assetid: 6306cafe-1153-44c7-8554-623b03d59a34
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 2/20/2019
ms.author: raiye
ms.openlocfilehash: 063834af9b56e9959883c00bb58c70f0d333c455
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56588565"
---
# <a name="azure-guest-os-releases-and-sdk-compatibility-matrix"></a>Azure Guest OS releases en SDK compatibiliteitsmatrix
Biedt dat u met actuele informatie over de meest recente Azure Guest OS releases voor Cloud Services. Deze informatie helpt u bij het plannen van uw upgradepad voordat een Gastbesturingssysteem wordt uitgeschakeld. Als u uw rollen gebruiken configureert *automatische* Guest OS-updates zoals beschreven in [Azure Gast OS Update-instellingen][Azure Guest OS Update Settings], niet is het essentieel dat u deze pagina hebt gelezen.

> [!IMPORTANT]
> Deze pagina is van toepassing op Cloud Services-web- en werkrollen rollen, die boven op een gast-besturingssysteem worden uitgevoerd. Dit gebeurt **niet van toepassing** naar IaaS Virtual Machines.
>
>


> [!TIP]
>  Abonneer u op de [Gast Update van het besturingssysteem RSS-Feed] voor het ontvangen van de meest recente melding over alle wijzigingen aan Gastbesturingssystemen.
>
>

> [!IMPORTANT]
> Alleen de meest recente 2 versies van het Gastbesturingssysteem worden ondersteund en beschikbaar in de Azure portal.
>
>

Weet over het bijwerken van het Gastbesturingssysteem? Controleer [dit] [ cloud updates] uit.

## <a name="news-updates"></a>Nieuws

###### <a name="february-5-2019"></a>**5 februari 2019**
Het Gastbesturingssysteem januari is uitgebracht.

###### <a name="january-24-2019"></a>**24 januari 2019**
Het Gastbesturingssysteem Family 6 (Windows Server 2019) heeft uitgegeven.

###### <a name="january-7-2019"></a>**7 januari 2019**
Het Gastbesturingssysteem December heeft uitgegeven.

###### <a name="december-14-2018"></a>**14 december 2018**
Het Gastbesturingssysteem November heeft uitgegeven.

###### <a name="november-8-2018"></a>**8 november 2018**
Het Gastbesturingssysteem oktober is uitgebracht.

###### <a name="october-12-2018"></a>**12 oktober 2018**
Het Gastbesturingssysteem September heeft uitgegeven.

###### <a name="september-12-2018"></a>**Op 12 september 2018**
Het Gastbesturingssysteem augustus heeft uitgegeven.

###### <a name="august-3-2018"></a>**3 augustus 2018**
Het Gastbesturingssysteem juli is uitgebracht.

## <a name="releases"></a>Releases

## <a name="family-6-releases"></a>Familie 6-versies
**Windows Server 2019**

.NET framework is geïnstalleerd: 3.5, 4.7.2

> [!NOTE]
> De Windows Azure SDK voor .NET - 3.0 kan worden gedownload [hier][Windows Azure SDK].
>
>De stappen van de installatie:
>1. Verwijder alle oudere versies van MicrosoftAzureAuthoringTools*.msi
>2. Installeer de [Azure SDK voor .NET - 3.0][Windows Azure SDK]
>3. Start uw computer opnieuw op
>4. Een nieuw project voor een Cloudservice maken en toevoegen van een enkele Werkrol
>5. Wijzigen van het type besturingssysteem op 6 en bouwen van een pakket
>6. Het pakket implementeren op Azure met behulp van de Azure portal of Visual Studio
>


| Configuratietekenreeks | Releasedatum | Datum uitschakelen |
| --- | --- | --- |
| WA-GUEST-OS-6.3_201901-01 |5 februari 2019 |Post 6.5 |
| WA-GUEST-OS-6.2_201812-01 |24 januari 2019 |Post 6.4 |
|~~WA-GUEST-OS-6.1_201811-01~~ |24 januari 2019 |5 februari 2019 |

## <a name="family-5-releases"></a>Familie 5-versies
**Windows Server 2016**

.NET framework is geïnstalleerd: 3.5, 4.6.2

> [!NOTE]
> De RDP-wachtwoord voor de OS-familie 5 moet minimaal 10 tekens.
>


| Configuratietekenreeks | Releasedatum | Datum uitschakelen |
| --- | --- | --- |
| WA-GUEST-OS-5.27_201901-01 |5 februari 2019 |Post 5.29 |
| WA-GUEST-OS-5.26_201812-01 |7 januari 2019 |Post 5.28 |
|~~WA-GUEST-OS-5.25_201811-01~~ |14 december 2018 |5 februari 2019 |
|~~WA-GUEST-OS-5.24_201810-01~~ |8 november 2018 |7 januari 2019 |
|~~WA-GUEST-OS-5.23_201809-01~~ |12 oktober 2018 |14 december 2018 |
|~~WA-GUEST-OS-5.22_201808-01~~ |Op 12 september 2018 |8 november 2018 |
|~~WA-GUEST-OS-5.21_201807-02~~ |3 augustus 2018 |12 oktober 2018 |

## <a name="family-4-releases"></a>Family 4-versies
**Windows Server 2012 R2**

.NET framework is geïnstalleerd: 3.5, 4.5.1

| Configuratietekenreeks | Releasedatum | Datum uitschakelen |
| --- | --- | --- |
| WA-GUEST-OS-4.62_201901-01 |5 februari 2019 |Post 4.64 |
| WA-GUEST-OS-4.61_201812-01 |7 januari 2019 |Post 4.63 |
|~~WA-GUEST-OS-4.60_201811-01~~ |14 december 2018 |5 februari 2019 |
|~~WA-GUEST-OS-4.59_201810-01~~ |8 november 2018 |7 januari 2019 |
|~~WA-GUEST-OS-4.58_201809-01~~ |12 oktober 2018 |14 december 2018 |
|~~WA-GUEST-OS-4.57_201808-01~~ |Op 12 september 2018 |8 november 2018 |
|~~WA-GUEST-OS-4.56_201807-02~~ |3 augustus 2018 |12 oktober 2018 |

## <a name="family-3-releases"></a>Type 3-versies
**Windows Server 2012**

.NET framework is geïnstalleerd: 3.5, 4.5

| Configuratietekenreeks | Releasedatum | Datum uitschakelen |
| --- | --- | --- |
| WA-GUEST-OS-3.69_201901-01 |5 februari 2019 |Post 3.71 |
| WA-GUEST-OS-3.68_201812-01 |7 januari 2019 |Post 3.70 |
|~~WA-GUEST-OS-3.67_201811-01~~ |14 december 2018 |5 februari 2019 |
|~~WA-GUEST-OS-3.66_201810-01~~ |8 november 2018 |7 januari 2019 |
|~~WA-GUEST-OS-3.65_201809-01~~ |12 oktober 2018 |14 december 2018 |
|~~WA-GUEST-OS-3.64_201808-01~~ |Op 12 september 2018 |8 november 2018 |
|~~WA-GUEST-OS-3.63_201807-02~~ |3 augustus 2018 |12 oktober 2018 |

## <a name="family-2-releases"></a>Familie 2-versies
**Windows Server 2008 R2 SP1**

.NET framework is geïnstalleerd: 3.5 (inclusief 2.0 en 3.0)

| Configuratietekenreeks | Releasedatum | Datum uitschakelen |
| --- | --- | --- |
| WA-GUEST-OS-2.82_201901-01 |5 februari 2019 |Post 2.84 |
| WA-GUEST-OS-2.81_201812-01 |7 januari 2019 |Post 2,83 |
|~~WA-GUEST-OS-2.80_201811-01~~ |14 december 2018 |5 februari 2019 |
|~~WA-GUEST-OS-2.79_201810-01~~ |8 november 2018 |7 januari 2019 |
|~~WA-GUEST-OS-2.78_201809-01~~ |12 oktober 2018 |14 december 2018 |
|~~WA-GUEST-OS-2.77_201808-01~~ |Op 12 september 2018 |8 november 2018 |
|~~WA-GUEST-OS-2.76_201807-02~~ |3 augustus 2018 |12 oktober 2018 |
|~~WA-GUEST-OS-2.75_201806-01~~ |3 juli 2018 |Op 12 september 2018 |

## <a name="msrc-patch-updates"></a>MSRC-patch-updates
De lijst met patches die opgenomen in elke maandelijkse Guest OS-versie zijn beschikbaar is [hier][patches].

## <a name="sdk-support"></a>SDK-ondersteuning
Hoewel de [stopzetting van beleid voor de Azure SDK] [ retire policy sdk] geeft aan dat alleen versies hierboven 2.2 ondersteunde zijn gastbesturingssystemen kunnen u gebruikmaken van een eerdere versie. U moet altijd de nieuwste ondersteunde SDK gebruiken.

| Gast-OS-familie | Compatibele SDK-versies |
| --- | --- |
| 6 |Versie 2.9.6+ |
| 5 |Versie 2.9.5.1+ |
| 4 |Versie 2.1 + |
| 3 |Versie 1.8 + |
| 2 |Versie 1.3 + |
| 1 |Versie 1.0 + |

## <a name="guest-os-release-information"></a>Gastbesturingssysteem release-informatie
Er zijn drie datums die belangrijk voor Guest OS releases zijn: **release** datum, **uitgeschakeld** datum, en **verlopen** datum. Een Gastbesturingssysteem wordt aangemerkt als beschikbaar wanneer het wordt in de Portal en kan worden geselecteerd als het doel Gastbesturingssysteem. Als een Gastbesturingssysteem bereikt de **uitgeschakeld** datum, wordt deze verwijderd uit Azure. Elke Cloudservice die gericht is op dat Gastbesturingssysteem wordt echter nog steeds uitgevoerd die normaal werken.

Het venster tussen de **uitgeschakeld** datum en de **verlopen** datum biedt u een buffer eenvoudig overstappen van een gast-besturingssysteem naar een nieuwere. Als u *automatische* als het Gastbesturingssysteem altijd, moet u de meest recente versie en u geen zorgen te hoeven maken over het verlopen.

Wanneer de **verlopen** datum is geslaagd, elke Cloudservice nog steeds met dat Gast-besturingssysteem wordt gestopt, verwijderd of gedwongen om bij te werken. U kunt meer lezen over de stopzetting van beleid [hier][retirepolicy].

## <a name="guest-os-family-version-explanation"></a>Uitleg van Guest OS family-versie
De Gast-OS-families zijn gebaseerd op uitgebrachte versies van Microsoft Windows Server. Het Gastbesturingssysteem is het onderliggende besturingssysteem dat op Azure Cloud Services wordt uitgevoerd. Elk Gastbesturingssysteem is een familie, versie en release getal.

* **Gast-OS-familie**  
  De release van een Windows Server-besturingssysteem die een Gastbesturingssysteem is gebaseerd op. Bijvoorbeeld, *type 3* is gebaseerd op Windows Server 2012.
* **Gast-OS-versie**  
  Specifiek is voor een afbeelding van Guest OS-familie plus relevante [Microsoft Security Response Center (MSRC)] [ msrc] patches die beschikbaar zijn op de datum waarop de nieuwe versie van de Gast-OS wordt geproduceerd. Niet alle patches kunnen worden opgenomen.

    Cijfers beginnen bij 0 en verhoogd met 1 telkens wanneer die een nieuwe set van updates wordt toegevoegd. Volgnullen worden alleen weergegeven als belangrijk. Versie 2.10 is een ander, veel hogere versie dan versie 2.1.
* **Gastbesturingssysteemversies**  
  Een nieuwe versie van een gast-OS-versie. Een nieuwe versie doet zich voor als Microsoft problemen gevonden tijdens het testen; die moeten worden gewijzigd. De meest recente versie altijd vervangt alle eerdere releases, openbare of niet. De Azure-portal kunnen alleen gebruikers om op te halen van de meest recente versie voor een bepaalde versie. Implementaties die worden uitgevoerd op een eerdere versie zijn meestal niet force bijgewerkt, afhankelijk van de ernst van de fout.

In het onderstaande voorbeeld 2 is de familie, 12, is de versie en 'rel2' is de versie.

**Release van Guest OS** - 2,12 rel2

**Configuratietekenreeks voor deze release** -WA-GUEST-OS-2.12_201208-02

De configuratietekenreeks voor een Gastbesturingssysteem is ingesloten in, samen met een datum wordt weergegeven welke patches MSRC werden meegenomen voor die versie van dezelfde informatie. In dit voorbeeld zijn MSRC patches geproduceerd voor Windows Server 2008 R2 tot en met inbegrip van: augustus 2012 overweging genomen voor opname. Alleen patches specifiek toe te passen op die versie van Windows Server worden opgenomen. Bijvoorbeeld, als een MSRC-patch is van toepassing op Microsoft Office, deze niet worden opgenomen omdat dat product geen deel uitmaakt van de basisinstallatiekopie van het Windows Server.

## <a name="guest-os-system-update-process"></a>Het updateproces voor Guest OS-systeem
Deze pagina bevat informatie over toekomstige Guest OS Releases. Klanten hebben aangegeven dat ze willen weten wanneer een release treedt op omdat de cloudrollen van de service opnieuw opgestart wordt als ze zijn ingesteld op 'Automatisch' update. Guest OS releases optreden doorgaans 2 tot 3 weken na de release die wordt uitgevoerd op de tweede dinsdag van elke maand van het MSRC-update. Nieuwe releases bevatten onder meer de relevante MSRC patches voor elke Guest OS family.

Microsoft Azure wordt voortdurend vrijgeven van updates. Het Gastbesturingssysteem is slechts één update in de pijplijn. Een release kan worden beïnvloed door vele factoren te groot om hier weer te geven. Bovendien Azure wordt uitgevoerd op letterlijk honderden duizenden machines. Dit betekent dat het niet mogelijk om op te geven van een exacte datum en tijd waarop de rollen zijn wordt opnieuw opgestart. We werken op een abonnement om te beperken of wanneer opnieuw wordt opgestart.

Wanneer een nieuwe versie van het Gastbesturingssysteem wordt gepubliceerd, kan het even volledig aan Azure doorgegeven. Als services zijn bijgewerkt naar de nieuwe Gast-besturingssysteem, worden ze opnieuw opgestart naleven van update-domeinen. Services die zijn ingesteld voor het gebruik van 'Automatisch' updates krijgt een release eerste. Nadat de update ziet u de nieuwe Guest OS-versie die wordt vermeld voor uw service in Azure portal. Opnieuw uitgebrachte versies kunnen zich voordoen tijdens deze periode. Sommige versies gedurende langere perioden kunnen worden geïmplementeerd en Automatische upgrade opnieuw wordt opgestart niet veel weken na de releasedatum van de officiële kunnen optreden. Als een Gastbesturingssysteem beschikbaar is, klikt u vervolgens kunt expliciet u die versie van de portal of in uw configuratiebestand.

Voor een grote hoeveelheid waardevolle informatie over het opnieuw is opgestart en verwijzingen naar meer informatie technische details van de Gast en Host-OS-updates, Zie de MSDN-blog post met de titel [rol exemplaar wordt opnieuw opgestart vanwege de Besturingssysteemupgrades] [ restarts].

Als u handmatig uw gast-besturingssysteem bijwerken, raadpleegt u de [Guest OS buitengebruikstellingsbeleid] [ retirepolicy] voor meer informatie.

## <a name="guest-os-supportability-and-retirement-policy"></a>Beleid voor ondersteuning en buiten gebruik stellen van Gastbesturingssysteem
Het beleid voor ondersteuning en buiten gebruik stellen van Guest OS wordt uitgelegd [hier][retirepolicy].

[cloud updates]: https://docs.microsoft.com/azure/cloud-services/cloud-services-update-azure-service
[Gast Update van het besturingssysteem RSS-Feed]: https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/master/GuestOS/GuestOSFeed.xml
[Install .NET on a Cloud Service Role]: https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-install-dotnet/?WT.mc_id=azurebg_email_Trans_963_RevisedNET_Update
[Azure Guest OS Update Settings]: cloud-services-how-to-configure-portal.md
[ssl3 announcement]: http://azure.microsoft.com/blog/2014/12/09/azure-security-ssl-3-0-update/
[Microsoft Security Advisory 3009008]: https://technet.microsoft.com/library/security/3009008.aspx
[ssl3-fixit]: http://go.microsoft.com/?linkid=9863266
[MS14-066]: https://technet.microsoft.com/library/security/ms14-066.aspx
[MS14-046]: https://technet.microsoft.com/library/security/ms14-046.aspx
[retire policy sdk]: https://msdn.microsoft.com/library/dn479282.aspx
[server and gos]: https://msdn.microsoft.com/library/dn775043.aspx
[azuresupport]: http://azure.microsoft.com/support/options/
[net install pkg]: http://www.microsoft.com/download/details.aspx?id=42643
[msrc]: https://technet.microsoft.com/security/dn440717.aspx
[update guest os portal]: https://msdn.microsoft.com/library/gg433101.aspx
[update guest os svc]: https://msdn.microsoft.com/library/gg456324.aspx
[restarts]: http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx
[patches]: cloud-services-guestos-msrc-releases.md
[retirepolicy]: cloud-services-guestos-retirement-policy.md
[fam1retire]: cloud-services-guestos-family1-retirement.md
[fix]: https://technet.microsoft.com/library/security/ms17-010.aspx
[Windows Azure SDK]: https://www.microsoft.com/en-us/download/details.aspx?id=54917
