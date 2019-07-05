---
title: Gegevens kopiëren of verplaatsen naar Azure Storage met behulp van AzCopy v10 | Microsoft Docs
description: AzCopy is een opdrachtregelprogramma die u gebruiken kunt om gegevens te kopiëren naar, uit of tussen opslagaccounts. Dit artikel helpt u AzCopy downloaden, verbinding maken met uw storage-account en vervolgens bestanden over te dragen.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 94aca33b2f12c1c39297221a856296dcca052b0f
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565803"
---
# <a name="get-started-with-azcopy"></a>Aan de slag met AzCopy

AzCopy is een opdrachtregelprogramma waarmee u kunt blobs of bestanden kopiëren naar of van een storage-account. Dit artikel helpt u AzCopy downloaden, verbinding maken met uw storage-account en vervolgens bestanden over te dragen.

> [!NOTE]
> AzCopy **V10** is de momenteel ondersteunde versie van AzCopy.
>
> Als u wilt gebruiken van AzCopy **v8.1**, Zie de [gebruikt u de vorige versie van AzCopy](#previous-version) sectie van dit artikel.

<a id="download-and-install-azcopy" />

## <a name="download-azcopy"></a>AzCopy downloaden

Download eerst het uitvoerbare bestand van AzCopy V10 naar een map op uw computer. 

- [Windows](https://aka.ms/downloadazcopy-v10-windows) (zip)
- [Linux](https://aka.ms/downloadazcopy-v10-linux) (tar)
- [MacOS](https://aka.ms/downloadazcopy-v10-mac) (zip)

> [!NOTE]
> Als u wilt kopiëren van gegevens naar en uit uw [Azure Table storage](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) -service en vervolgens installeren [AzCopy versie 7.3](https://aka.ms/downloadazcopynet).

## <a name="run-azcopy"></a>AzCopy uitvoert

Voor het gemak, houd rekening met de Active directory-locatie van het uitvoerbare bestand van AzCopy toe te voegen aan het systeempad staat voor gebruiksgemak. Op die manier kunt u typen `azcopy` uit een map op uw systeem.

Als u niet de AzCopy-map toevoegen aan uw path, hebt u mappen wijzigen naar de locatie van het uitvoerbare bestand van AzCopy en hetzelfde type `azcopy` of `.\azcopy` in Windows PowerShell-opdracht wordt gevraagd.

Een lijst met opdrachten wilt bekijken, typt u `azcopy -h` en druk vervolgens op de ENTER-toets.

Voor meer informatie over een specifieke opdracht, de naam van de opdracht NET bevatten (bijvoorbeeld: `azcopy list -h`).

![Inline-hulp](media/storage-use-azcopy-v10/azcopy-inline-help.png)

> [!NOTE] 
> Als een eigenaar van uw Azure Storage-account, worden niet u automatisch machtigingen voor toegang tot gegevens toegewezen. Voordat u iets zinvolle met AzCopy doen kunt, moet u bepalen hoe u autorisatiereferenties om de storage-service te bieden. 

## <a name="choose-how-youll-provide-authorization-credentials"></a>Kies hoe u autorisatiereferenties bieden

U kunt autorisatiereferenties opgeven met behulp van Azure Active Directory (AD) of met behulp van een Shared Access Signature (SAS)-token.

Deze tabel als richtlijn gebruiken:

| Opslagtype | Momenteel ondersteunde methode van autorisatie |
|--|--|
|**Blob Storage** | Azure AD & SAS |
|**BLOB-opslag (hiërarchische naamruimte)** | Azure AD & SAS |
|**Bestandsopslag** | Alleen SAS |

### <a name="option-1-use-azure-ad"></a>Optie 1: Gebruik Azure AD

Met behulp van Azure AD, kunt u de referenties eenmaal in plaats van een SAS-token toevoegen aan elke opdracht opgeven.  

Het niveau van de machtiging die u nodig hebt, is gebaseerd op of u wilt bestanden uploaden of ze alleen te downloaden.

Als u alleen wilt om bestanden te downloaden, klikt u vervolgens controleren of de [gegevenslezer voor Opslagblob](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader) is toegewezen aan uw gebruikers-id of service-principal. 

> [!NOTE]
> Gebruikers-id's en service-principals zijn elk een type *beveiligings-principal*, daarom we de term gebruiken *beveiligings-principal* voor de rest van dit artikel.

Als u uploaden van bestanden wilt, controleert u of dat een van deze rollen is toegewezen aan uw beveiligings-principal:

- [Gegevensbijdrager voor Blob](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor)
- [De eigenaar van een opslag-Blob-gegevens](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)

Deze rollen kunnen worden toegewezen aan uw identiteit in een van deze bereiken:

- Container (bestandssysteem)
- Storage-account
- Resourcegroep
- Abonnement

Zie voor meer informatie over het controleren en toewijzen van rollen, [toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC in Azure portal](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

> [!NOTE] 
> Houd er rekening mee dat RBAC-roltoewijzingen maximaal vijf minuten duurt worden doorgegeven.

U hoeft niet beschikken over een van deze rollen toegewezen aan uw beveiligings-principal als uw beveiligings-principal wordt toegevoegd aan de toegangsbeheerlijst (ACL) van de doelcontainer of map. In de ACL moet uw beveiligings-principal schrijftoegang op de doelmap en voert u de machtigingen op de container en de bovenliggende map.

Zie voor meer informatie, [toegangsbeheer in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

#### <a name="authenticate-a-user-identity"></a>De identiteit van een gebruiker verifiëren

Nadat u hebt gecontroleerd dat de identiteit van de gebruiker het benodigde autorisatieniveau is toegewezen, open een opdrachtprompt, typ de volgende opdracht en druk vervolgens op de ENTER-toets.

```azcopy
azcopy login
```

Als u werkzaam bent bij meer dan één organisatie, opgenomen in de tenant-ID van de organisatie waarvan het opslagaccount dat deel uitmaakt.

```azcopy
azcopy login --tenant-id=<tenant-id>
```

Vervang de `<tenant-id>` tijdelijke aanduiding door de tenant-ID van de organisatie waarvan het opslagaccount dat deel uitmaakt. Als u wilt zoeken in de tenant-ID, selecteer **Azure Active Directory > Eigenschappen > map-ID** in Azure portal.

Met deze opdracht retourneert een verificatiecode op te geven en de URL van een website. Open de website, de op te geven en kies vervolgens de **volgende** knop.

![Een container maken](media/storage-use-azcopy-v10/azcopy-login.png)

Er wordt een venster Aanmelden weergegeven. In dit venster, meld u aan bij uw Azure-account met behulp van de referenties van uw Azure-account. Nadat u hebt aangemeld, kunt u het browservenster sluiten en beginnen met behulp van AzCopy.

<a id="service-principal" />

#### <a name="authenticate-a-service-principal"></a>Een service-principal verifiëren

Dit is een goede optie als u van plan bent AzCopy gebruiken binnen een script dat wordt uitgevoerd zonder tussenkomst van de gebruiker. 

Voordat u het script uitvoert, hebt u aanmelden bij interactief ten minste één keer zodat u AzCopy met de referenties van uw service-principal opgeven kunt.  Deze referenties worden opgeslagen in een bestand beveiligd en versleuteld, zodat uw script beschikt niet over die gevoelige informatie te verstrekken.

U kunt zich aanmelden bij uw account met behulp van een clientgeheim of met behulp van het wachtwoord van een certificaat dat is gekoppeld aan app-registratie van uw service-principal. 

Zie voor meer informatie over service-principal maken, [het: Gebruik de portal voor het maken van een Azure AD-toepassing en service-principal die toegang hebben tot resources](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

Zie voor meer informatie over service-principals in het algemeen, [toepassing en service-principalobjecten in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals)

##### <a name="using-a-client-secret"></a>Met behulp van een clientgeheim in

Beginnen met het instellen de `AZCOPY_SPA_CLIENT_SECRET` omgevingsvariabele van het clientgeheim van uw service-principal de app-registratie. 

> [!NOTE]
> Zorg ervoor dat u deze waarde instellen vanaf de opdrachtprompt en niet in de omgeving variabele instellingen van het besturingssysteem. Op die manier kunnen de waarde is alleen beschikbaar voor de huidige sessie.

In dit voorbeeld laat zien hoe u dit kunt doen in PowerShell.

```azcopy
$env:AZCOPY_SPA_CLIENT_SECRET="$(Read-Host -prompt "Enter key")"
```

> [!NOTE]
> Overweeg het gebruik van een prompt, zoals wordt weergegeven in dit voorbeeld. Op die manier het clientgeheim wordt niet weergegeven in de opdrachtgeschiedenis van de console. 

Vervolgens typt u de volgende opdracht en druk vervolgens op de ENTER-toets.

```azcopy
azcopy login --service-principal --application-id <application-id>
```

Vervang de `<application-id>` tijdelijke aanduiding met de toepassings-ID van uw service-principal de app-registratie.

##### <a name="using-a-certificate"></a>Met behulp van een certificaat

Als u liever uw eigen referenties gebruiken voor autorisatie, kunt u een certificaat uploaden naar uw app-registratie, en vervolgens met dit certificaat om aan te melden.

Naast het uploaden van uw certificaat naar uw app-registratie, moet u ook voor een kopie van het certificaat dat is opgeslagen op de machine of virtuele machine waarop u AzCopy wordt uitgevoerd. Deze kopie van het certificaat moet zich in. PFX of. PEM-indeling en de persoonlijke sleutel moet bevatten. De persoonlijke sleutel moet worden beveiligd met een wachtwoord. Als u Windows gebruikt en uw certificaat alleen aanwezig is in een certificaatarchief, zorg er dan voor dat certificaat exporteren naar een PFX-bestand (met inbegrip van de persoonlijke sleutel). Zie voor instructies [Export-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate?view=win10-ps)

Vervolgens stelt de `AZCOPY_SPA_CERT_PASSWORD` omgevingsvariabele wachtwoord voor het certificaat.

> [!NOTE]
> Zorg ervoor dat u deze waarde instellen vanaf de opdrachtprompt en niet in de omgeving variabele instellingen van het besturingssysteem. Op die manier kunnen de waarde is alleen beschikbaar voor de huidige sessie.

In dit voorbeeld laat zien hoe u dit kunt doen in PowerShell.

```azcopy
$env:AZCOPY_SPA_CERT_PASSWORD="$(Read-Host -prompt "Enter key")"
```

Vervolgens typt u de volgende opdracht en druk vervolgens op de ENTER-toets.

```azcopy
azcopy login --service-principal --certificate-path <path-to-certificate-file>
```

Vervang de `<path-to-certificate-file>` tijdelijke aanduiding met het relatieve of volledig gekwalificeerde pad naar het certificaatbestand. AzCopy het pad naar dit certificaat opgeslagen, maar deze niet Sla een kopie van het certificaat, dus zorg ervoor dat dit certificaat in plaats.

> [!NOTE]
> Overweeg het gebruik van een prompt, zoals wordt weergegeven in dit voorbeeld. Op die manier kunnen uw wachtwoord wordt niet weergegeven in de opdrachtgeschiedenis van de console. 

### <a name="option-2-use-a-sas-token"></a>Optie 2: Een SAS-token gebruiken

U kunt een SAS-token toevoegen aan elke bron of doel-URL die gebruikmaken van in de AzCopy-opdrachten.

In dit voorbeeld van de opdracht recursief kopieert gegevens van een lokale map naar een blob-container. Een fictieve SAS-token is toegevoegd aan het einde van de van de container-URL.

```azcopy
azcopy cp "C:\local\path" "https://account.blob.core.windows.net/mycontainer1/?sv=2018-03-28&ss=bjqt&srt=sco&sp=rwddgcup&se=2019-05-01T05:01:17Z&st=2019-04-30T21:01:17Z&spr=https&sig=MGCXiyEzbtttkr3ewJIh2AR8KrghSy1DGM9ovN734bQF4%3D" --recursive=true
```

Zie voor meer informatie over SAS-tokens en het verkrijgen van een [Using shared access signatures (SAS)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="transfer-files"></a>Overdracht van bestanden

Nadat u hebt uw identiteit wordt geverifieerd of een SAS-token verkregen, kunt u beginnen met het overdragen van bestanden.

Als u Voorbeeldopdrachten, bezoekt u een van deze artikelen.

- [Gegevens overdragen met AzCopy en blob-opslag](storage-use-azcopy-blobs.md)

- [Gegevens overdragen met AzCopy en bestandsopslag](storage-use-azcopy-files.md)

- [Gegevens overdragen met AzCopy en Amazon S3 buckets](storage-use-azcopy-s3.md)

- [Gegevens overdragen met AzCopy en Azure Stack-opslag](https://docs.microsoft.com/azure-stack/user/azure-stack-storage-transfer#azcopy)

## <a name="use-azcopy-in-a-script"></a>AzCopy gebruiken in een script

Voordat u het script uitvoert, hebt u aanmelden bij interactief ten minste één keer zodat u AzCopy met de referenties van uw service-principal opgeven kunt.  Deze referenties worden opgeslagen in een bestand beveiligd en versleuteld, zodat uw script beschikt niet over die gevoelige informatie te verstrekken. Zie voor voorbeelden van de [verifiëren van uw service-principal](#service-principal) sectie van dit artikel.

Na verloop van tijd, de AzCopy [downloadkoppeling](#download-and-install-azcopy) verwijst naar de nieuwe versies van AzCopy. Als uw script AzCopy downloadt, kan het script mogelijk niet meer werkt als een nieuwere versie van AzCopy functies die afhankelijk van uw script wijzigt. 

Om te voorkomen dat deze problemen, verkrijgen van een statische (niet wijzigen) koppelen aan de huidige versie van AzCopy. Op die manier uw script wordt gedownload exact dezelfde versie van AzCopy elke keer dat deze wordt uitgevoerd.

Als u de koppeling, kunt u deze opdracht uitvoeren:

| Besturingssysteem  | Opdracht |
|--------|-----------|
| **Linux** | `curl -v https://aka.ms/downloadazcopy-v10-linux` |
| **Windows** | `(curl https://aka.ms/downloadazcopy-v10-windows -MaximumRedirection 0 -ErrorAction silentlycontinue).RawContent` |

> [!NOTE]
> Voor Linux, `--strip-components=1` op de `tar` opdracht verwijdert u de op het hoogste niveau map met de naam van de en haalt u in plaats daarvan het binaire bestand rechtstreeks in de huidige map. Hiermee wordt het script dat moet worden bijgewerkt met een nieuwe versie van `azcopy` door bij te werken alleen de `wget` URL.

De URL wordt weergegeven in de uitvoer van deze opdracht. Het script kunt vervolgens AzCopy downloaden met behulp van deze URL.

| Besturingssysteem  | Opdracht |
|--------|-----------|
| **Linux** | `wget -O azcopyv10.tar https://azcopyvnext.azureedge.net/release20190301/azcopy_linux_amd64_10.0.8.tar.gz tar -xf azcopyv10.tar --strip-components=1 ./azcopy` |
| **Windows** | `Invoke-WebRequest https://azcopyvnext.azureedge.net/release20190517/azcopy_windows_amd64_10.1.2.zip -OutFile azcopyv10.zip <<Unzip here>>` |

## <a name="use-azcopy-in-storage-explorer"></a>AzCopy gebruiken in Storage Explorer

Als u wilt en biedt daardoor de prestatievoordelen van AzCopy, maar u liever met Storage Explorer in plaats van de opdrachtregel om te communiceren met uw bestanden, schakelt u vervolgens AzCopy in Storage Explorer. 

Kies in Storage Explorer **Preview**->**AzCopy gebruiken voor verbeterde Blob uploaden en downloaden**.

![AzCopy inschakelen als een engine voor overdracht in Azure Storage Explorer](media/storage-use-azcopy-v10/enable-azcopy-storage-explorer.jpg)

> [!NOTE]
> U hebt geen Schakel deze instelling als u een hiërarchische naamruimte hebt ingeschakeld op uw storage-account. Dat komt doordat Opslagverkenner maakt automatisch gebruik van AzCopy op storage-accounts waarvoor een hiërarchische naamruimte.  

Storage Explorer uw accountsleutel gebruikt voor het uitvoeren van bewerkingen, dus nadat u zich bij de Storage Explorer aanmeldt, hoeft u niet meer autorisatiereferenties op te geven.

<a id="previous-version" />

## <a name="use-the-previous-version-of-azcopy"></a>Gebruik de vorige versie van AzCopy

Als u wilt gebruiken van de vorige versie van AzCopy (AzCopy v8.1), ziet u een van de volgende koppelingen:

- [AzCopy in Windows (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy)

- [AzCopy in Linux (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy-linux)

## <a name="configure-optimize-and-troubleshoot-azcopy"></a>Configureer, optimaliseren en oplossen van AzCopy

Zie [configureren, te optimaliseren en oplossen van AzCopy](storage-use-azcopy-configure.md)

## <a name="next-steps"></a>Volgende stappen

Als u vragen of problemen met algemene feedback te sturen, dient u ze [op GitHub](https://github.com/Azure/azure-storage-azcopy) pagina.
