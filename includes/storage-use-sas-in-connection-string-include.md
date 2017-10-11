Als u een shared access signature (SAS)-URL die u toegang tot bronnen in een opslagaccount verleent beschikt, kunt u de SAS in een verbindingsreeks. Omdat de SAS de benodigde informatie om te verifiëren van de aanvraag bevat, biedt een verbindingsreeks met een SAS het protocol, het service-eindpunt en de benodigde referenties voor toegang tot de bron.

Geef voor het maken van een verbindingsreeks die een shared access signature bevat de tekenreeks in de volgende indeling:

```
BlobEndpoint=myBlobEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
FileEndpoint=myFileEndpoint;
SharedAccessSignature=sasToken
```

Elk service-eindpunt is optioneel, hoewel de verbindingsreeks moet ten minste één bevatten.

> [!NOTE]
> Gebruik van HTTPS met een SAS wordt aanbevolen als aanbevolen procedure.
>
> Als u een SAS in een verbindingsreeks in een configuratiebestand opgeeft, moet u wellicht de speciale tekens in de URL's coderen.
>
>

### <a name="service-sas-example"></a>Voorbeeld van de service-SAS
Hier volgt een voorbeeld van een verbindingsreeks met een service-SAS voor Blob-opslag:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&sr=b&si=tutorial-policy-635959936145100803&sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

En Hier volgt een voorbeeld van de verbindingsreeks met codering van speciale tekens bevatten:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&amp;sr=b&amp;si=tutorial-policy-635959936145100803&amp;sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

### <a name="account-sas-example"></a>Voorbeeld van account-SAS
Hier volgt een voorbeeld van een verbindingsreeks waarin een SAS-account voor Blob en File storage. Houd er rekening mee dat eindpunten voor beide services worden opgegeven:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&spr=https&st=2016-04-12T03%3A24%3A31Z&se=2016-04-13T03%3A29%3A31Z&srt=s&ss=bf&sp=rwl
```

En Hier volgt een voorbeeld van de verbindingsreeks met URL-codering:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&amp;sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&amp;spr=https&amp;st=2016-04-12T03%3A24%3A31Z&amp;se=2016-04-13T03%3A29%3A31Z&amp;srt=s&amp;ss=bf&amp;sp=rwl
```

