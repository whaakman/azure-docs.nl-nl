---
title: Meer informatie over de nieuwste Azure Gast OS Releases | Microsoft Docs
description: De meest recente release nieuws en SDK compatibiliteit voor Azure Cloud Services-Gastbesturingssysteem.
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: 
ms.assetid: 6306cafe-1153-44c7-8554-623b03d59a34
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 1/4/2018
ms.author: raiye
ms.openlocfilehash: e645d57dfff040eb05f4d395fac058b2b485933b
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="azure-guest-os-releases-and-sdk-compatibility-matrix"></a>Azure Gast OS releases en SDK compatibiliteit matrix
Biedt de meest recente informatie over de nieuwste Azure Gast OS releases voor Cloud-Services. Deze informatie helpt u bij het plannen van uw upgradepad voordat een Gastbesturingssysteem wordt uitgeschakeld. Als u uw rollen gebruiken configureert *automatische* Gastbesturingssysteem updates zoals beschreven in [Update-instellingen van Azure Gast OS][Azure Guest OS Update Settings], niet is het essentieel dat u deze pagina hebt gelezen.

> [!IMPORTANT]
> Deze pagina geldt voor Cloud Services-web- en werkrollen rollen, dat boven op een Gastbesturingssysteem wordt uitgevoerd. Dit gebeurt **niet van toepassing** IaaS virtuele Machines.
>
>


> [!TIP]
>  Abonneren op de [Gast OS Update RSS-Feed] voor het ontvangen van de meest recente melding op alle wijzigingen van het Gastbesturingssysteem.
>
>

> [!IMPORTANT]
> Vanaf de November-implementatie, alleen de meest recente 2 versies van het Gastbesturingssysteem worden ondersteund en beschikbaar zijn in de Azure-portal.
>
>

Onzekerheid over het Gastbesturingssysteem is of hoe de Gast OS releases werk? Lees [dit](#how-it-works) sectie.

## <a name="news-updates"></a>Nieuws
###### <a name="january-4-2018"></a>**4 januari 2018**
Het Gastbesturingssysteem januari is vrijgegeven voor OS-familie 4 (WA-GUEST-OS-4.50_201801-01) & 5 (WA-GUEST-OS-5.15_201801-01) en bevat belangrijke beveiligingspatches.  

###### <a name="january-4-2018"></a>**4 januari 2018**
Het Gastbesturingssysteem december heeft uitgegeven.

###### <a name="december-14-2017"></a>**14 december 2017**
Het Gastbesturingssysteem november heeft uitgegeven.

###### <a name="november-8-2017"></a>**8 november 2017**
Het Gastbesturingssysteem oktober heeft uitgegeven.

###### <a name="october-6-2017"></a>**6 oktober 2017**
Het Gastbesturingssysteem september heeft uitgegeven. Voor de release van Windows Server 2016 September is netfx3 standaard ingeschakeld. Klanten toe te voegen, dism / online/Disable-Feature /featurename:netfx3' in hun OnStart als hun werkstroom ze nodig heeft om een .NET 2.x-app uitvoeren met een runtime 4.x of als ze een .NET 2.x-app uitgevoerd verwerkt een fout en vervolgens een .NET-app 4.x is uitgevoerd.

###### <a name="september-14-2017"></a>**14 september 2017**
Implementatie van het Gastbesturingssysteem september 14 September wordt gestart en heeft een verwachte release van oktober 9.

###### <a name="august-24-2017"></a>**24 augustus 2017**
Het Gastbesturingssysteem augustus heeft uitgegeven.

###### <a name="august-3-2017"></a>**3 augustus 2017**
Het Gastbesturingssysteem juli heeft uitgegeven.

###### <a name="july-19-2017"></a>**19 juli 2017**
Implementatie van het Gastbesturingssysteem juli juli 19 wordt gestart en heeft een verwachte release van 8 augustus.

###### <a name="july-7-2017"></a>**7 juli 2017**
Het Gastbesturingssysteem juni heeft uitgegeven.

###### <a name="june-16-2017"></a>**16 juni 2017**
Implementatie van het Gastbesturingssysteem juni juni 16 wordt gestart en heeft een verwachte release van juli 11.


## <a name="releases"></a>Versies
## <a name="family-5-releases"></a>Familie 5-versies
**WindowsServer 2016**

.NET framework is geïnstalleerd: 4.0, 4.5, 4.5.1, 4.5.2, 4.6, 4.6.1, 4.6.2

> [!NOTE]
> Datums met een * zijn kan worden gewijzigd.
>
> Het RDP-wachtwoord voor OS-familie 5 moet minimaal 10 tekens.
>

| Configuratietekenreeks | Releasedatum | Datum uitschakelen | Verloopdatum |
| --- | --- | --- | --- |
| WA-GUEST-OS-5.15_201801-01 |4 januari 2018 |Post 5.17 |NOG TE BEPALEN |
| WA-GUEST-OS-5.14_201712-01 |4 januari 2018 |Post 5.16 |NOG TE BEPALEN |
| WA-GUEST-OS-5.13_201711-01 |14 december 2017 |Post 5.15 |NOG TE BEPALEN |
|~~WA-GUEST-OS-5.12_201710-02~~ |8 november 2017 |4 januari 2018 |NOG TE BEPALEN |
|~~WA-GUEST-OS-5.11_201709-01~~ |6 oktober 2017 |14 december 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-5.10_201708-01~~ |24 augustus 2017 |14 december 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-5.9_201707-01~~ |3 augustus 2017 |8 november 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-5.8_201706-01~~ |7 juli 2017 |6 oktober 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-5.7_201705-01~~ |5 juni 2017 |24 augustus 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-5.6_201704-01~~ |9 mei 2017 |3 augustus 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-5.5_201703-01~~ |10 april 2017 |7 juli 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-5.4_201612-01~~ |10 januari 2017 |5 juni 2017|NOG TE BEPALEN |

## <a name="family-4-releases"></a>Familie 4-versies
**Windows Server 2012 R2**

.NET framework is geïnstalleerd: 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> Datums met een * zijn kan worden gewijzigd
>
>

| Configuratietekenreeks | Releasedatum | Datum uitschakelen | Verloopdatum |
| --- | --- | --- | --- |
| WA-GUEST-OS-4.50_201801-01 |4 januari 2018 |Post 4.52 |NOG TE BEPALEN |
| WA-GUEST-OS-4.49_201712-01 |4 januari 2018 |Post 4,51 |NOG TE BEPALEN |
| WA-GUEST-OS-4.48_201711-01 |14 december 2017 |Post 4.50 |NOG TE BEPALEN |
|~~WA-GUEST-OS-4.47_201710-02~~ |8 november 2017 |4 januari 2018 |NOG TE BEPALEN |
|~~WA-GUEST-OS-4.46_201709-01~~ |6 oktober 2017 |14 december 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-4.45_201708-01~~ |24 augustus 2017 |14 december 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-4.44_201707-01~~ |3 augustus 2017 |8 november 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-4.43_201706-01~~ |7 juli 2017 |6 oktober 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-4.42_201705-01~~ |5 juni 2017 |24 augustus 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-4.41_201704-01~~ |9 mei 2017 |3 augustus 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-4.40_201703-01~~ |10 april 2017 |7 juli 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-4.39_201612-01~~ |10 januari 2017 |5 juni 2017 |NOG TE BEPALEN |

## <a name="family-3-releases"></a>Familie 3-versies
**WindowsServer 2012**

.NET framework is geïnstalleerd: 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> Datums met een * zijn kan worden gewijzigd
>
>

| Configuratietekenreeks | Releasedatum | Datum uitschakelen | Verloopdatum |
| --- | --- | --- | --- |
| WA-GUEST-OS-3.56_201712-01 |4 januari 2018 |Post 3,58 |NOG TE BEPALEN |
| WA-GUEST-OS-3.55_201711-01 |14 december 2017 |Post 3.57 |NOG TE BEPALEN |
|~~WA-GUEST-OS-3.54_201710-02~~ |8 november 2017 |4 januari 2018 |NOG TE BEPALEN |
|~~WA-GUEST-OS-3.53_201709-01~~ |6 oktober 2017 |14 december 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-3.52_201708-01~~ |24 augustus 2017 |14 december 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-3.51_201707-01~~ |3 augustus 2017 |8 november 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-3.50_201706-01~~ |7 juli 2017 |6 oktober 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-3.49_201705-01~~ |5 juni 2017 |24 augustus 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-3.48_201704-01~~ |9 mei 2017 |3 augustus 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-3.47_201703-01~~ |10 april 2017 |7 juli 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-3.46_201612-01~~ |10 januari 2017 |5 juni 2017 |NOG TE BEPALEN |

## <a name="family-2-releases"></a>2-familie releases
**Windows Server 2008 R2 SP1**

.NET framework is geïnstalleerd: 3.5, 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> Datums met een * zijn kan worden gewijzigd
>
>

| Configuratietekenreeks | Releasedatum | Datum uitschakelen | Verloopdatum |
| --- | --- | --- | --- |
| WA-GUEST-OS-2.69_201712-01 |4 januari 2018 |Post 2.71 |NOG TE BEPALEN |
| WA-GUEST-OS-2.68_201711-01 |14 december 2017 |Post 2,70 |NOG TE BEPALEN |
|~~WA-GUEST-OS-2.67_201710-02~~ |8 november 2017 |4 januari 2018 |NOG TE BEPALEN |
|~~WA-GUEST-OS-2.66_201709-01~~ |6 oktober 2017 |14 december 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-2.65_201708-01~~ |24 augustus 2017 |14 december 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-2.64_201707-01~~ |3 augustus 2017 |8 november 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-2.63_201706-01~~ |7 juli 2017 |6 oktober 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-2.62_201705-01~~ |5 juni 2017 |24 augustus 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-2.61_201704-01~~ |9 mei 2017 |3 augustus 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-2.60_201703-01~~ |10 april 2017 |7 juli 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-2.59_201701-01~~ |10 januari 2017 |5 juni 2017 |NOG TE BEPALEN |
|~~WA-GUEST-OS-2.58_201612-01~~ |10 januari 2017 |9 mei 2017|NOG TE BEPALEN |


## <a name="msrc-patch-updates"></a>MSRC patchupdates
De lijst met patches die opgenomen in elke versie van de maandelijkse Gastbesturingssysteem zijn is beschikbaar [hier][patches].

## <a name="sdk-support"></a>SDK-ondersteuning
Hoewel de [buiten gebruik stellen beleid voor de Azure SDK] [ retire policy sdk] geeft aan dat alleen versies hierboven 2.2 ondersteunde, specifieke zijn Gastbesturingssysteem families kunnen u oudere versies. U moet altijd de meest recente ondersteunde SDK gebruiken.

| Gast OS-familie | Compatibel SDK-versies |
| --- | --- |
| 5 |Versie 2.9.5.1+ |
| 4 |Versie 2.1 + |
| 3 |Versie 1.8 + |
| 2 |Versie 1.3 + |
| 1 |Versie 1.0 + |

## <a name="guest-os-release-information"></a>Gast OS Release-informatie
Er zijn drie datums die belangrijk voor de Gast OS releases zijn: **release** datum, **uitgeschakeld** datum, en **verlopen** datum. Een Gastbesturingssysteem wordt beschouwd als beschikbaar wanneer het wordt in de Portal en kan worden geselecteerd als het doel Gastbesturingssysteem. Wanneer een Gastbesturingssysteem bereikt de **uitgeschakeld** datum is, wordt deze verwijderd uit Azure. Elke Cloud-Service die gericht is op dat Gastbesturingssysteem wordt echter nog steeds werken normaal.

Het venster tussen de **uitgeschakeld** datum en de **verlopen** datum vindt u een buffer naar eenvoudig overgang van een Gastbesturingssysteem naar een nieuwere. Als u *automatische* als het Gastbesturingssysteem altijd, moet u over de nieuwste versie en u hoeft te hoeven maken over verloopt.

Wanneer de **verlopen** datum verstrijkt, elke Cloud-Service nog steeds met dat Gastbesturingssysteem worden gestopt, verwijderd of gedwongen om bij te werken. Meer informatie over het buiten gebruik stellen beleid [hier][retirepolicy].

## <a name="guest-os-family-version-explanation"></a>Uitleg van Gast OS-familie-versie
De Gast OS-familie zijn gebaseerd op uitgebrachte versies van Microsoft Windows Server. Het Gastbesturingssysteem is het onderliggende besturingssysteem dat op Azure Cloud Services wordt uitgevoerd. Elk Gastbesturingssysteem heeft een familie, versie en release nummer.

* **Gast OS-familie**  
  De release van een Windows Server-besturingssysteem die een Gastbesturingssysteem is gebaseerd op. Bijvoorbeeld: *familie 3* is gebaseerd op Windows Server 2012.
* **Versie van het besturingssysteem van de Gast**  
  Specifiek zijn voor een Gastbesturingssysteem familie installatiekopie plus relevante [Microsoft Security Response Center (MSRC)] [ msrc] patches die beschikbaar zijn op de datum waarop de nieuwe versie van de Gast OS wordt geproduceerd. Niet alle patches kunnen worden opgenomen.

    Nummers beginnen bij 0 en verhogen door 1 telkens wanneer die een nieuwe set van updates wordt toegevoegd. Volgnullen worden alleen weergegeven als belangrijk. Versie 2.10 is een versie afwijken, veel hoger dan versie 2.1.
* **Gastbesturingssysteem release**  
  Een nieuwe versie van de versie van een Gastbesturingssysteem. Een nieuwe versie Deze gebeurtenis treedt op als Microsoft problemen gevonden tijdens het testen; die moeten worden gewijzigd. De meest recente versie vervangt altijd alle vorige versies, openbare of niet. De Azure-portal kunnen alleen gebruikers om op te halen van de meest recente release voor een bepaalde versie. Implementaties die worden uitgevoerd op een eerdere versie zijn meestal niet geforceerd bijgewerkt, afhankelijk van de ernst van de fout.

In het onderstaande voorbeeld 2 is de familie, 12, is de versie en 'rel2' is de release.

**Gastbesturingssysteem release** - 2,12 rel2

**De configuratietekenreeks voor deze release** -WA-GUEST-OS-2.12_201208-02

De configuratietekenreeks voor een Gastbesturingssysteem heeft dezelfde informatie in dit ingesloten samen met een datum wordt weergegeven welke patches MSRC zijn van belang is voor deze release. In dit voorbeeld zijn MSRC patches geproduceerd voor Windows Server 2008 R2 tot en met inbegrip van augustus 2012 overwogen om opgenomen. Alleen patches toepassen van specifiek voor die versie van Windows Server zijn opgenomen. Bijvoorbeeld, als een patch MSRC voor Microsoft Office geldt, kan deze niet worden opgenomen omdat dat product geen deel uit van de basisinstallatiekopie van Windows Server maakt.

## <a name="guest-os-system-update-process"></a>Gast OS systeemproces Update
Deze pagina bevat informatie over toekomstige Gast OS Releases. Klanten hebt aangegeven dat ze willen weten wanneer een release treedt op omdat de rollen van hun cloud-service opnieuw opgestart wordt als ze zijn ingesteld op 'Automatisch' update. Gast OS releases optreden doorgaans ten minste vijf (5) dagen na het MSRC update versie die wordt uitgevoerd op de tweede dinsdag van elke maand. Nieuwe releases omvatten de relevante MSRC patches voor elke gast OS-familie.

Microsoft Azure brengt voortdurend updates. Het Gastbesturingssysteem is slechts één update in de pijplijn. Een release kan worden beïnvloed door diverse factoren te groot om weer te geven hier. Bovendien Azure wordt uitgevoerd op letterlijk honderden of duizenden-of-machines. Dit betekent dat is het onmogelijk geven een exacte datum en tijd waarop de rollen opnieuw wordt opgestart. We werken aan een plan te beperken of tijd opnieuw wordt opgestart.

Wanneer een nieuwe versie van het Gastbesturingssysteem wordt gepubliceerd, kan het duren naar Azure volledig is doorgegeven. Als de services zijn bijgewerkt naar de nieuwe Gastbesturingssysteem, worden ze opgestart naleven van update-domeinen. Services die zijn ingesteld op "Automatisch" updates krijgt een release eerste. Nadat de update ziet u de nieuwe Gast OS-versie die wordt vermeld voor uw service in de Azure portal. Opnieuw uitgebrachte versies optreden tijdens deze periode. Sommige versies gedurende langere tijd kunnen worden geïmplementeerd en Automatische upgrade herstarts niet plaatsvindt voor veel weken na de releasedatum van de officiële. Zodra een Gastbesturingssysteem beschikbaar is, klikt u vervolgens kunt expliciet u die versie van de portal of in uw configuratiebestand.

Voor een grote hoeveelheid waardevolle informatie over het opnieuw is opgestart en koppelingen naar meer informatie technische details van de Gast en Host-OS-updates, Zie de MSDN-blog post met de titel [rol exemplaar opnieuw wordt opgestart vanwege Upgrades voor het besturingssysteem] [ restarts].

Als u uw Gastbesturingssysteem handmatig bijwerken, raadpleegt u de [Gastbesturingssysteem buiten gebruik stellen beleid] [ retirepolicy] voor meer informatie.

## <a name="guest-os-supportability-and-retirement-policy"></a>Gast OS ondersteuningsmogelijkheden en buiten gebruik stellen beleid
Het Gastbesturingssysteem ondersteuningsmogelijkheden en buiten gebruik stellen beleid wordt uitgelegd [hier][retirepolicy].

[Gast OS Update RSS-Feed]: https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/master/GuestOS/GuestOSFeed.xml
[Install .NET on a Cloud Service Role]: https://azure.microsoft.com/en-us/documentation/articles/cloud-services-dotnet-install-dotnet/?WT.mc_id=azurebg_email_Trans_963_RevisedNET_Update
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
[msrc]: http://www.microsoft.com/security/msrc/default.aspx
[update guest os portal]: https://msdn.microsoft.com/library/gg433101.aspx
[update guest os svc]: https://msdn.microsoft.com/library/gg456324.aspx
[restarts]: http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx
[patches]: cloud-services-guestos-msrc-releases.md
[retirepolicy]: cloud-services-guestos-retirement-policy.md
[fam1retire]: cloud-services-guestos-family1-retirement.md
[fix]: https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
