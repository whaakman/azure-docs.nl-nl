#### <a name="create-an-a-record-set-with-single-record"></a>Een A-recordset met één record maken

Gebruik `azure network dns record-set create` om een recordset te maken. Geef de resourcegroep, zonenaam, de relatieve naam van de recordset, het recordtype en de TTL (Time to Live) op.

```azurecli
    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300
```

Nadat u de A-recordset hebt gemaakt, voegt u het IPv4-adres aan de recordset toe met `azure network dns record-set add-record`.

```azurecli
    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1
```

#### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Een AAAA-recordset met één record maken

```azurecli
    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"
```

#### <a name="create-a-cname-record-set-with-a-single-record"></a>Een CNAME-recordset met één record maken

CNAME-records staan slechts één tekenreekswaarde toe.

```azurecli
    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"
```

#### <a name="create-an-mx-record-set-with-a-single-record"></a>Een MX-recordset met één record maken

In dit voorbeeld gebruiken we de naam van de recordset "@" om de MX-record te maken in het toppunt van de zone (in dit geval 'contoso.com'). Dit is gebruikelijk voor MX-records.

```azurecli
    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5
```

#### <a name="create-an-ns-record-set-with-a-single-record"></a>Een NS-recordset met één record maken

```azurecli
    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"
```

#### <a name="create-a-ptr-record-set-with-a-single-record"></a>Een PTR-recordset met één record maken

In dit geval vertegenwoordigt 'my-arpa-zone.com' de ARPA-zone voor uw IP-bereik.  Elke PTR-recordset die is ingesteld in deze zone komt overeen met een IP-adres in dit IP-bereik.

```azurecli
    azure network dns record-set add-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"
```

#### <a name="create-an-srv-record-set-with-a-single-record"></a>Een SRV-recordset met één record maken

Als u een SRV-record in de zonebasis maakt, kunt u '_service' en '_protocol' in de recordnaam opgeven. Het is niet nodig om "@" op te nemen in de recordnaam.

```azurecli
    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"
```

#### <a name="create-a-txt-record-set-with-single-record"></a>Een TXT-recordset met één record maken

```azurecli
    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"
```


<!--HONumber=Nov16_HO2-->


