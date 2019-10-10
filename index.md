# Tiedonhakujärjestelmän kyselyrajapintakuvaus

*Dokumentin versio 0.12*

## Versiohistoria

Versio|Päivämäärä|Kuvaus|Tekijä
---|---|---|---
0.3|26.4.2019|Alustava versio|AP,TV
0.4|9.5.2019|Sanomarakenne päivitetty alustavasti ISO 20022 mukaillen|AP|
0.5|22.5.2019|Päivityksiä standardinmukaisuuteen liittyen|AP|
0.6|31.5.2019|Tarkennuksia rajapintakuvauksen käyttöohjeistukseen|AP|
0.7|10.6.2019|Tarkennus virtuaalivaluutan tarjoajien osalta|TV|
0.8|11.9.2019|Rajapintakuvaus jaettu kahdeksi dokumentiksi|AL|
0.9|25.9.2019|Tarkennuksia auth.002 käyttöön|AP|
0.10|27.9.2019|Päivitetty kuvaus pankki- ja maksutilitietojen kyselystä tiedonhakujärjestelmästä|AP|
0.11|1.10.2019|Lisätty kuvaus oikeushenkilön rekisteröitymispäivän palauttamisesta hakutulosten yhteydessä.|AP|
0.12|8.10.2019|Lisätty ohje XML-allekirjoituksen muodotamiseen|AP|

## Sisällysluettelo

1. [Johdanto](#luku1)  
2. [Pankki- ja maksutilitietojen kysely tiedonhakujärjestelmästä](#luku2)  
3. [Tietoturva](#tietoturva)  
4. [Tiedonhakujärjestelmän kyselyrajapinta](#kyselyrajapinta)   

## 1. Johdanto <a name="luku1"></a>

### 1.1 Termit ja lyhenteet

Lyhenne tai termi|Selite
---|---
Rajapinta|Standardin mukainen käytäntö tai yhtymäkohta, joka mahdollistaa tietojen siirron laitteiden, ohjelmien tai käyttäjän välillä. 
WS (Web Service)|Verkkopalvelimessa toimiva ohjelmisto, joka tarjoaa standardoitujen internetyhteyskäytäntöjen avulla palveluja sovellusten käytettäväksi. Tiedonhakujärjestelmä tarjoaa palveluna tietojen kyselyn.
Endpoint|Rajapintapalvelu, joka on saatavilla tietyssä verkko-osoitteessa
WSDL| (Web Service Description Language) Rakenteellinen kuvauskieli, jolla kuvataan web palvelun tarjoamat toiminnallisuudet.
PKI | Julkisen avaimen tekniikka. Julkisen avaimen tekniikkaan perustuva sähköinen allekirjoitus luodaan siten, että allekirjoitettavasta tiedosta muodostetaan (tiivistealgoritmilla) tiiviste, joka salataan avainparin yksityisellä avaimella. Salattu tiiviste tallennetaan allekirjoitetun tiedon tai sähköisen asiakirjan yhteyteen tai välitetään muulla tavoin tiedon vastaanottajalle. Vastaanottaja purkaa tiivisteen salauksen avainparin julkisella avaimella, muodostaa uudelleen viestin tai asiakirjan tiedoista tiivisteen ja vertaa sitä allekirjoitukseen liitettyyn tiivisteeseen. Viestin sisältö on muuttumaton, mikäli tiivisteet ovat samat. (Sähköisen asioinnin tietoturvallisuus -ohje)|

### 1.2 Dokumentin tarkoitus ja kattavuus

Tämä dokumentti on osa Tullin julkaisemaa määräystä pankki- ja maksutilien valvontajärjestelmästä. Dokumentin tarkoitus on antaa ohjeet tiedonhakujärjestelmän kyselyrajapinnasta. Tätä dokumenttia täydentää tiedonhakujärjestelmän käyttöönoton ja ylläpidon ohje.

### 1.3 Viittaukset

[ISO 20022 External Code Sets](https://www.iso20022.org/external_code_list.page)

[ISO 20022 auth.001.001.01 InformationRequestOpeningV01 MDR](https://www.iso20022.org/documents/general/Payments_AFI_MDR_January2013.zip)

[ISO 20022 auth.002.001.01 InformationRequestResponseV01 MDR](https://www.iso20022.org/documents/general/Payments_AFI_MDR_January2013.zip)

[ISO 20022 head.001.001.01 skeema](https://www.iso20022.org/documents/messages/head/schemas/head.001.001.01.zip)

[fin.002.001.01](schemas/fin.002.001.01.xsd)

[fin.012.001.01](schemas/fin.012.001.01.xsd)

[fin.013.001.01](schemas/fin.013.001.01.xsd)

[Sähköisen asioinnin tietoturvallisuus -ohje](http://julkaisut.valtioneuvosto.fi/bitstream/handle/10024/80012/VM_25_2017.pdf)

### 1.4 Yleiskuvaus

Tulli on perustanut Tilirekisterihankkeen, joka toteuttaa (EU) 2018/843 direktiivin ja sen täytäntöön panemiseksi säädetyn, Suomen lainsäädäntöön perustuvan pankki- ja maksutilien valvontajärjestelmän. 

Tässä dokumentissa kuvataan tiedonhakujärjestelmän kyselyrajapinnat.

## 2. Pankki- ja maksutilitietojen kysely tiedonhakujärjestelmästä <a name="luku2"></a>

Tässä luvussa on kuvattu pankki- ja maksutilitietojen kysely tiedonhakujärjestelmästä.

Kuvassa 2.1 on esitetty vuokaaviona pankki- ja maksutilitietojen kysely tiedonhakujärjestelmästä.

![Pankki- ja maksutilitietojen kysely](diagrams/flowchart_query.png "Pankki- ja maksutilitietojen kysely")  
*__Kuva2.1.__ Pankki- ja maksutilitietojen kysely*  

Kuvasta nähdään, että kyselyrajapinta mahdollistaa sekä vastauksen palauttamisen heti synkronisesti, tai vaihtoehtoisesti asynkronisesti. 

Taulukossa 2.1. on esitetty vuokaavion muuttujien merkitys. 

*__Taulukko 2.1.__ Vuokaavion muuttujat*

|Muuttuja|Kuvaus|
|:--|:--|
|DELAY_1|Kyselyn request #1 sallittu maksimiviive, "välittömästi"|
|DELAY_2|Pollausväli, viive joka clientin on odotettava ennen seuraavaa kyselyä|
|RETRY_LIMIT|Kuinka monta pollausta (request #2) on sallittua tehdä|

Taulukossa 2.1. esitettyjen muuttujien kulloinkin voimassa olevat arvot kerrotaan liitedokumentaatiossa.

Kyselyn vuo kulkee seuraavasti:
1. Client lähettää kyselysanoman
2. Server joko  
  a. Palauttaa vastaussanoman, joka sisältää hakutuloksen ja koodin *COMP*, muuttujassa *DELAY_1* määritellyn viiveen sisällä ("välittömästi"), tai  
  b. palauttaa vastaussanoman, joka sisältää koodin *NRES* 
3. Client tarkistaa onko vastaussanomassa koodi *COMP* vai *NRES*
4. Jos koodi on *COMP*, siirrytään kohtaan 10.
5. Koodi on *NRES*. Client odottaa *DELAY_2* muuttujan määrittämän ajan ja tekee sen jälkeen kyselyn request #2
6. Server, joko  
  a. Palauttaa hakutuloksen ja koodin *COMP*, muuttujassa *DELAY_1* määritellyn viiveen sisällä ("välittömästi"), tai  
  b. palauttaa vastaussanoman, joka sisältää koodin *NRES* 
7. Client tarkistaa onko vastaussanomassa koodi *COMP* vai *NRES*
8. Jos koodi on *COMP*, siirrytään kohtaan 10.
9. Koodi on *NRES*. Jos *RETRY_LIMIT* ei ole saavutettu, siirrytään kohtaan 5.  
10. Loppu.
 
Taulukossa 2.2. on kuvattu StatusResponse1Code arvojen käyttö

*__Taulukko 2.2.__ StatusResponse1Code arvojen käyttö*

|Koodi|Nimi|Määritelmä|Kuvaus|
|:--|:--|:--|:--|
|COMP|CompleteResponse|Response is complete.|Vastaussanoma sisältää hakutulokset|
|NRES|NoResponseYet|Response not provided yet.|Vastaussanoma ei sisällä hakutuloksia, tee uusi kysely myöhemmin.|
|PART|PartialResponse|Response is partially provided.|Ei käytössä|

## <a name="tietoturva"></a> 3. Tietoturva
  
### 3.1 Tunnistaminen

Taulukossa 3.1. on esitetty varmenteet tiedonhakujärjestelmässä.

*__Taulukko 3.1.__ Tiedonhakujärjestelmän varmenteet*

|Standardi|Sertifikaatin nimi|Käyttötarkoitus|
|:--|:--|:--|
|X.509|Tiedonhakujärjestelmän tietoliikennesertifikaatti|Rajapinnan hyödyntäjän tunnistaminen|
|X.509|Tiedonhakujärjestelmän allekirjoitussertifikaatti|Sanoman allekirjoittaminen,sanoman muuttumattomuuden varmistaminen|

Tiedonhakujärjestelmän kyselyrajapinnan hyödyntäjät tunnistetaan X.509-sertifikaateilla (Tietoliikennesertifikaatti). Kyselyrajapinnan sanomat allekirjoitetaan XML-allekirjoituksella (Allekirjoitussertifikaatti). 

#### <a name="xml-sig"></a> XML-allekirjoituksen muodostaminen

Allekirjoituksen tyyppi on __enveloped signature__. Signature-elementti sijoitetaan [BAHin](#BusinessApplicationHeaderV01) Sgntr-elementin alle.

Esimerkki 3.1. Esimerkki SignedInfo
```
<SignedInfo>
 <CanonicalizationMethod Algorithm=”http://www.w3.org/2001/10/xml-exc-c14n#"/>
 <SignatureMethod
  Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha256"/>
 <Reference URI="">
  <Transforms>
   <Transform Algorithm=”http://www.w3.org/2001/10/xml-exc-c14n#"/>
  </Transforms>
  <DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha256"/>
  <DigestValue>...</DigestValue>
 </Reference>
</SignedInfo>
```

Allekirjoitusalgoritmi on siis RSA-SHA256 ja C14N on Exclusive XML Canonicalization. Reference URI on "", eli koko dokumentti allekirjoitetaan.

Mahdollisuus pyyntöjen IP-avaruuden rajoittamiseen tiedonhakujärjestelmässä tarkennetaan myöhemmin.

### 3.2 Tietoliikenteen suojaus

Tietoliikenne tulee suojata (salaus ja vastapuolen tunnistus) x.509 varmenteita käyttäen. Yhteydenottaja tunnistetaan asiakas(client) -varmenteen avulla. Tietojärjestelmän tulee hyväksyä yhteys toimivaltaiselta viranomaiselta seuraavin edellytyksin: <br />a) Toimivaltaisen viranomaisen asiakas(client) -varmenteen on myöntänyt VRK, <br />b) varmenne on voimassa, eikä esiinnyt VRK:n sulkulistalla ja <br />c) varmenteen Subject-kentän serialNumber attribuuttina on tunnus, joka muodostuu kirjaimista "FI" ja toimivaltaisen viranomaisen tai sen puolesta toimivan valtion palvelukeskuksen y-tunnuksen numero-osasta ilman väliviivaa (ALV tunnuksen muotoinen tunnus), esim, Tullin varmenteessa tulee lukea "Subject : CN=ws.tulli.fi, serialNumber=FI02454428, 0=Tulli, L=Helsinki"

## <a name="kyselyrajapinta"></a> 4. Tiedonhakujärjestelmän kyselyrajapinta

Kyselyrajapinta toteutetaan SOAP/XML Web Servicenä, josta julkaistaan WSDL.

Sanomissa käytetään ISO 20022 koodistoviittauksia. Koodistoviittaukset löytyvät ISO 20022 sivulta [External Code Sets](https://www.iso20022.org/external_code_list.page).

Kyselyrajapinnassa on yksi endpoint, jonka kysely- ja vastaussanomarakenne on kuvattu tässä luvussa.

### 4.1 Kyselyrajapinnan SOAP-operaatioiden sanomarakenne

SOAP body koostuu aina kahdesta osasta, ISO 20022 Business Application Headerista (BAH) ja varsinaisesta business-sanomasta.  

### 4.2 Business Application Header (BAH) 

Business Application Header sanoman tiedot on esitetty seuraavassa taulukossa.

|Sanoma-id|Sanoman nimi|Sovellusohje|
|:---|:---|:--|
|[head.001.001.01](https://www.iso20022.org/documents/messages/head/schemas/head.001.001.01.zip)|Business Application Header|[MUG](https://www.iso20022.org/documents/general/BAHMUG.zip)|

BAH on oltava aina SOAP bodyn ensimmäinen elementti.

### 4.3 Kyselyrajapinnan sanomat

Tiedonhakujärjestelmän kyselyrajapinnassa käytetään [ISO 20022 -sanomia InformationRequestOpeningV01 ja InformationRequestResponseV01](https://www.iso20022.org/full_catalogue.page), joihin liitetään tarvittavat [Supplementary Datat](https://www.iso20022.org/supplementary_data.page).

Seuraavassa taulukossa on listattu käytettävät ISO 20022 -sanomat. 

|Sanoma-id|Sanoman nimi|Käyttötarkoitus|Vastaava organisaatio|Msg Def Report|
|---|---|---|---|---| 
|auth.001.001.01|InformationRequestOpeningV01|Kyselyrajapinnan kyselysanoma|FFI|[MDR](https://www.iso20022.org/documents/general/Payments_AFI_MDR_January2013.zip)|
|auth.002.001.01|InformationRequestResponseV01|Kyselyrajapinnan vastaussanoma|FFI|[MDR](https://www.iso20022.org/documents/general/Payments_AFI_MDR_January2013.zip)|

Seuraavassa taulukossa on listattu kyselysanomaan liitettävä Supplementary Data -sanomalaajennus. Sanomalaajennusten tarkempi sisältö ja tietueiden käyttöohjeet on listattu luvussa 4.

|Sanoma-id|Sanoman nimi|Laajennettavan ISO 20022 sanoman id|Tarkoitus ja toiminnallisuus|
|---|---|---|---|
|FIN012|InformationRequestFIN012|auth.001.001.01|ISO 20022 sanomalaajennus. Kyselyrajapinnan toimivaltaiset viranomaiset käyttävät tätä sanomaa tietojen kyselyyn tiedonhakurajapinnasta. Sisältää kyselyn tekevän henkilön ja henkilön esimiehen tunnisteet. Mahdollistaa auth.001.001.01 puuttuvien hakukriteerien käytön (esim. tallelokero, toistaiseksi ei käytössä)|

Seuraavassa taulukossa on listattu vastaussanomaan liitettävät Supplementary Data -sanomalaajennukset.

|Sanoma-id|Sanoman nimi|Laajennettavan ISO 20022 sanoman id|Tarkoitus ja toiminnallisuus|
|---|---|---|---|
|supl.027.001.01|InformationResponseSD1V01|auth.002.001.01|Sisältää hakuparametreja vastaavat tilitiedot ja tileihin liittyvät edunsaajat|
|FIN002|InformationResponseFIN002|auth.002.001.01|Sisältää hakuparametreja vastaavat tallelokeroiden ja tallelokeroiden haltijoiden tiedot.|
|FIN013|InformationResponseFIN013|auth.002.001.01|Sisältää tili- ja tallelokerotiedoista erillisenä hakuparametreja vastaavat asiakkuustiedot|

Kyselyrajapinnan sanomavastauksiin sisällytetään kaikki hakukriteereitä vastaavat sanomarakenteen sellaiset tiedot, jotka liittyvät asiakkuuksiin, jotka ovat päättyneet alle 10 vuotta sitten (tietojen säilytysaika). Tileihin ja tallelokeroihin liittyvät kaikki osallisuustiedot palautetaan, eli kyselyparametrina annetun hakuehdon mukaisten henkilöiden (oikeus- tai luonnollinen) lisäksi palautetaan myös kaikki osalliset henkilöt. Osallisten henkilöiden muita kuin kyselyn hakuparametria vastaavia tilejä ja tallelokeroita sen sijaan ei palauteta, vaan niitä koskien on tehtävä uudet kyselyt lainsäädäntöperusteineen.

Seuraavassa on listattu kyselyrajapinnan sanomat tietueineen. Taulukoita luetaan siten, että sisennetyt rivit ovat ensimmäisen rivin sisällä sanomarakenteessa.

Taulukossa pakollisuus on merkitty seuraavasti:

- **R** Pakollinen (required)
- **A** Vaihtoehtoinen (alternative), valitaan yksi useammasta vaihtoehdosta
- **O** Vapaaehtoinen (optional)

### <a name="BusinessApplicationHeaderV01"></a> 4.4 BusinessApplicationHeaderV01

Seuraavassa taulukossa on esitetty BAH-elementtien käyttö. Elementtien tyypit on kuvattu [head.001.001.01-skeemassa](https://www.iso20022.org/documents/messages/head/schemas/head.001.001.01.zip).

|Nimi|Tyyppi|Käytössä|Kuvaus|
|:---|:---|:---|:---|
|BusinessApplicationHeaderV01| | | |
|CharSet|UnicodeChartsCode|kyllä|"UTF-8"|
|Fr|Party9Choice|kyllä|Käytetään seuraavasti: Elementti `Fr/OrgId/Id/OrgId/Othr/SchmeNm/Cd` sisältää arvon "Y" ja elementti `Fr/OrgId/Id/OrgId/Othr/Id` sisältää lähettäjän Y-tunnuksen.|
|To|Party9Choice|kyllä|Käytetään seuraavasti: Elementti `To/OrgId/Id/OrgId/Othr/SchmeNm/Cd` sisältää arvon "Y" ja elementti `To/OrgId/Id/OrgId/Othr/Id` sisältää vastaanottajan Y-tunnuksen (Esim. Tiedonhakujärjestelmässä Y-tunnus 0245442-8)|
|BizMsgIdr|Max35Text|kyllä|Käyttö standardin mukaisesti.|
|MsgDefIdr|Max35Text|kyllä|Sisältää sanoma-id:n. Kyselysanomassa käytetään "auth.001.001.01", vastaussanomassa sisältönä on "auth.002.001.01"|
|BizSvc||ei||
|CreDt|ISONormalisedDateTime|kyllä|BAH:n luomis päivämäärä ja aika. Normalisoitava Z-notaatiolla (UTC).|
|CpyDplct||ei| |
|PssblDplct| |ei| |
|Prty| |ei| |
|Sgntr| |kyllä|Business messagen lähettäjän muodostama XML-allekirjoitus. Ks. [XML-allekirjoituksen muodostaminen](#xml-sig)|
|Rltd|BusinessApplicationHeader1|kyllä|Käytössä vastaussanomassa, sisältää kyselysanoman sisältämän BAH:in.|

### <a name="InformationRequestOpeningV01"></a> 4.5 InformationRequestOpeningV01

Taulukossa on kuvattu sanoman tietueiden käyttö.

|Nimi|Tyyppi|Käytössä|Kuvaus|
|:---|:---|:---|:---|
|InformationRequestOpeningV01| | | |
|&nbsp;&nbsp;&nbsp;&nbsp;InvstgtnId|Max35Text|Kyllä|Tutkinnan case id|
|&nbsp;&nbsp;&nbsp;&nbsp;LglMndtBsis|LegalMandate1|Kyllä|Lainsäädäntöperuste. Arvojoukko tarkentuu.|
|&nbsp;&nbsp;&nbsp;&nbsp;CnfdtltySts|YesNoIndicator|Kyllä|Aina "true"|
|&nbsp;&nbsp;&nbsp;&nbsp;DueDt|DueDate1|Ei||
|&nbsp;&nbsp;&nbsp;&nbsp;InvstgtnPrd|DateOrDateTimePeriodChoice|Ei|Ei huomioida. Skeemassa pakollinen, joten `/Document/InfReqOpng/InvstgtnPrd/Dt/FrDt` ja `ToDt` esim. kuluva pvm|
|&nbsp;&nbsp;&nbsp;&nbsp;SchCrit|SearchCriteria1Choice|Kyllä|Hakukriteeri. Käytettävä aina mahdollisimman täsmällistä hakukriteeriä. Esim. jos ei käytetä Y-tunnusta, vaan käytetään OtherOrganisationIdentification -kenttää, haku ei kohdistu Y-tunnuksiin lainkaan. Ks. [tarkempi erittely](#SearchCriteria1Choice) alla.|
|&nbsp;&nbsp;&nbsp;&nbsp;SplmtryData|SupplementaryData1|Kyllä|Sisältää sanomalaajennuksen [InformationRequestFIN012](#InformationRequestFIN012)|

#### <a name="SearchCriteria1Choice"></a> Haku henkilötunnuksella tai henkilötodistuksen tunnistenumerolla

|Tagi|Skeeman polku InfReqOpng/SchCrit/|Kuvaus|
|:---|:---|:---|
|\<Id\>|CstmrId/Pty/Id/Prvtld/Othr|Hetu tai henkilötodistuksen tunnistenumero|
|\<Cd\>|CstmrId/Pty/Id/Prvtld/Othr/SchemeNm|"PIC" (Person Identification Code, hetu), "OTHR" (Muu henkilötodistuksen tunnistenumero)|
|\<MsgNmId\>|CstmrId/AuthrtyReq/Tp|"auth.001.001.01"|
|\<Cd\>|CstmrId/AuthrtyReq/InvstgtdRoles|"ALLP"|

#### <a name=""></a> Haku Y-tunnuksella tai muulla oikeushenkilön tunnisteella

|Tagi|Skeeman polku InfReqOpng/SchCrit/|Kuvaus|
|:---|:---|:---|
|\<Id\>|CstmrId/Pty/Id/OrgId/Othr|Y-tunnus tai muu oikeushenkilön tunniste|
|\<Cd\>|CstmrId/Pty/Id/OrgId/Othr/SchemeNm|"Y" (Y-tunnus), "PRH" (Yhdistysrekisterinumero), OTHER*|
|\<MsgNmId\>|CstmrId/AuthrtyReq/Tp|"auth.001.001.01"|
|\<Cd\>|CstmrId/AuthrtyReq/InvstgtdRoles|"ALLP"|


#### <a name=""></a> Haku yrityksen nimellä

|Tagi|Skeeman polku InfReqOpng/SchCrit/|Kuvaus|
|:---|:---|:---|
|\<Id\>|CstmrId/Pty/Nm|Yrityksen nimi|
|\<Id\>|CstmrId/Pty/Id/OrgId/Othr|Arvoksi asetetaan "1"|
|\<Cd\>|CstmrId/Pty/Id/OrgId/Othr/SchemeNm|"NAME"|
|\<MsgNmId\>|CstmrId/AuthrtyReq/Tp|"auth.001.001.01"|
|\<Cd\>|CstmrId/AuthrtyReq/InvstgtdRoles|"ALLP"|


#### <a name=""></a> Haku IBANilla

|Hakukriteeri|Tagi|Skeeman polku InfReqOpng/SchCrit/|Kuvaus|
|:---|:---|:---|:---|
|IBAN|\<IBAN\>|Acct/Id/Id||

#### <a name=""></a> Haku muulla tilin yksilöintitunnuksella

|Hakukriteeri|Tagi|Skeeman polku InfReqOpng/SchCrit/|Kuvaus|
|:---|:---|:---|:---|
|Tilin muu yksilöintitunnus|\<Id\>|Acct/Id/Id/Othr|Tunnus|
||\<Cd\>|Acct/Id/Id/Othr/SchemeNm|OTHER*|

#### <a name=""></a> Haku luonnollisen henkilön nimi, kansalaisuus ja syntymäaika -yhdistelmällä

|Tagi|Skeeman polku InfReqOpng/SchCrit/|Kuvaus|
|:---|:---|:---|
|\<Nm\>|CstmrId/Pty|Nimi|
|\<Id\>|CstmrId/Pty/Id/PrvtId/Othr|Maakoodi|
|\<Cd\>|CstmrId/Pty/Id/PrvtId/Othr/SchemeNm|"NATI"|
|\<BirthDt\>|CstmrId/Pty/Id/PrvtId/DtAndPlcOfBirth|Syntymäaika. `CtryOfBirth` arvoksi asetetaan "XX" ja `CityOfBirth` arvoksi asetetaan ”not in use”|
|\<MsgNmId\>|CstmrId/AuthrtyReq/Tp|"auth.001.001.01"|
|\<Cd\>|CstmrId/AuthrtyReq/InvstgtdRoles|"ALLP"|

*) OTHER arvojoukko kuvataan erillisessä taulukossa, Tulli lähettää taulukot Pankki- ja maksutilirekisterin
osalta yhteisen taulukon ylläpitäjälle.

### <a name="InformationRequestFIN012"></a> 4.6 Sanomalaajennus InformationRequestFIN012

Sanomalaajennus liitetään taulukossa listattuun ISO 20022 sanoman XPath-sijaintiin.

|Nimi|Pakollisuus (RAO)|[min..max]|Tyyppi|Kuvaus|Liitetään sanomaan|XPath|
|:---|:---|:---|:---|:---|:---|:---|
|InformationRequestFIN012| | | | |[auth.001](#InformationRequestOpeningV01)|`/Document/InfReqOpng/SplmtryData/Envlp`|
|&nbsp;&nbsp;&nbsp;&nbsp;AuthorityInquiry|R|[1..1]|[AuthorityInquirySet](#AuthorityInquirySet)|Kyselyyn liittyvät viranomaisen tiedot| |
|&nbsp;&nbsp;&nbsp;&nbsp;AdditionalSearchCriteria|R|[1..*]|[SearchCriteriaChoice](#SearchCriteriaChoice)|Jos tarvitaan esimerkiksi tallelokero hakukriteeriksi, voidaan käyttää tätä elementtiä.| |

#### <a name="AuthorityInquirySet"></a> AuthorityInquirySet

|Nimi|Pakollisuus (RAO)|[min..max]|Tyyppi|Kuvaus|
|:---|:---|:---|:---|:---|
|AuthorityInquirySet| | | | |
|&nbsp;&nbsp;&nbsp;&nbsp;OfficialId|R|[0..1]|Max140Text|Viranomaisen (henkilön) tunniste|
|&nbsp;&nbsp;&nbsp;&nbsp;OfficialSuperiorId|R|[0..1]|Max140Text|Esimiehen tunniste|

### <a name="InformationRequestResponseV01"></a> 4.7 InformationRequestResponseV01

Taulukossa on kuvattu sanoman tietueiden käyttö.

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|InformationRequestResponseV01| | | | |
|&nbsp;&nbsp;&nbsp;&nbsp;RspnId|Max35Text|Kyllä|[1..1]|Vastaussanoman id|
|&nbsp;&nbsp;&nbsp;&nbsp;InvstgtnId|Max35Text|Kyllä|[1..1]|Kyselysanomassa toimitettu Case Id|
|&nbsp;&nbsp;&nbsp;&nbsp;RspnSts|StatusResponse1Code|Kyllä|[1..1]|Vastaussanoman status, "COMP"|
|&nbsp;&nbsp;&nbsp;&nbsp;SchCrit|SearchCriteria1Choice|Kyllä|[1..1]|Kyselysanomassa esiintynyt Document/InfReqOpng/SchCrit sellaisenaan|
|&nbsp;&nbsp;&nbsp;&nbsp;RtrInd|ReturnIndicator1|Kyllä|[0..*]| ks. ReturnIndicator1 käyttö alla. |
|&nbsp;&nbsp;&nbsp;&nbsp;SplmtryData|SupplementaryData1|Ei|[0..0]|-|

#### ReturnIndicator1 käyttö

ReturnIndicator1 sisältää yksittäisen hakutulostyypin esiintymän. 

|XPath|Tyyppi|Kuvaus|
|:---|:---|:---|
|RtrInd/AuthrtyReqTp/MsgNmId|Max35Text|sisältää sanomalaajennuksen sanoma-id:n (supl.027.001.01, fin.013.001.01 tai fin.002.001.01)|
|RtrInd/InvstgtnRslt|InvestigationResult1Choice|palautetaan aina `Rslt` elementti tyyppiä SupplementaryDataEnvelope1, joka sisältää joko [supl.027.001.01](#supl.027.001.01), [InformationResponseFIN002](#InformationResponseFIN002) tai [InformationResponseFIN013](#InformationResponseFIN013).

Jokaista hakutulostyyppiä kohti palautetaan enintään yksi hakutulos-alisanoma (supl.027.001.01, fin.013.001.01 tai fin.002.001.01) per Y-tunnus. 

__Esimerkki 1.__  

Kyselysanomassa esiintynyttä Document/InfReqOpng/SchCrit hakutermiä vastaavia tuloksia on löytynyt kolme kappaletta: yksi asiakkuus ja kaksi tiliä.

Vastaussanomaan liitetään kaksi kappaletta RtrInd-elementtiä:

```
<!-- xmlns:n1="urn:iso:std:iso:20022:tech:xsd:auth.002.001.01" -->
<n1:RtrInd>
  <n1:AuthrtyReqTp>
    <n1:MsgNmId>supl.027.001.01</n1:MsgNmId>
  </n1:AuthrtyReqTp>
  <n1:InvstgtnRslt>
    <n1:Rslt>
      <n2:Document xmlns:n2="urn:iso:std:iso:20022:tech:xsd:supl.027.001.01" ...>
        <n2:InfRspnSD1>
          <!-- Hakutuloksen tili #1, tili #2 tiedot -->
        </n2:InfRspnSD1>
      </n2:Document>
    </n1:Rslt>
  </n1:InvstgtnRslt>
</n1:RtrInd>
<n1:RtrInd>
  <n1:AuthrtyReqTp>
    <n1:MsgNmId>fin.013.001.01</n1:MsgNmId>
  </n1:AuthrtyReqTp>
  <n1:InvstgtnRslt>
    <n1:Rslt>
      <n1:Document xmlns:n3="fin.013.001.01" ...">
      	<n1:InfRspnFin013>
          <!-- Hakutuloksen asiakkuus #1 tiedot -->
        </n1:InfRspnFin013>
      </n1:Document>
    </n1:Rslt>
  </n1:InvstgtnRslt>
</n1:RtrInd>
```

__Esimerkki 2.__

Rajapinta on koosteinen: yksi rajapinta palauttaa hakutuloksia useiden Y-tunnusten alla.

Kyselysanomassa esiintynyttä Document/InfReqOpng/SchCrit hakutermiä vastaavia tuloksia on löytynyt neljä kappaletta: yksi tili (tili #1) Y-tunnukselle 0190983-0 ja kolme tiliä (tili #2, tili #3, tili #4) Y-tunnukselle 0828972-6.

Vastaussanomaan liitetään kaksi kappaletta RtrInd-elementtiä:

```
<!-- xmlns:n1="urn:iso:std:iso:20022:tech:xsd:auth.002.001.01" -->
<n1:RtrInd>
  <n1:AuthrtyReqTp>
    <n1:MsgNmId>supl.027.001.01</n1:MsgNmId>
  </n1:AuthrtyReqTp>
  <n1:InvstgtnRslt>
    <n1:Rslt>
      <n2:Document xmlns:n2="urn:iso:std:iso:20022:tech:xsd:supl.027.001.01" ...>
        <n2:InfRspnSD1>
          <n2:InvstgtnId>a</n2:InvstgtnId>
            <n2:CreDtTm>
              <!-- -->
            </n2:CreDtTm>
            <n2:AcctSvcrId>
              <n2:FinInstnId>
                <n2:Othr>
                  <n2:Id>0190983-0</n2:Id>
                  <n2:SchmeNm>
                    <n2:Cd>Y</n2:Cd>
                  </n2:SchmeNm>
                </n2:Othr>
              </n2:FinInstnId>
            </n2:AcctSvcrId>
            <n2:AcctAndPties>
              <!-- Y-tunnuksen 0190983-0 hakutulokset, tili #1-->
            </n2:AcctAndPties>
        </n2:InfRspnSD1>
      </n2:Document>
    </n1:Rslt>
  </n1:InvstgtnRslt>
</n1:RtrInd>
<n1:RtrInd>
  <n1:AuthrtyReqTp>
    <n1:MsgNmId>supl.027.001.01</n1:MsgNmId>
  </n1:AuthrtyReqTp>
  <n1:InvstgtnRslt>
    <n1:Rslt>
      <n2:Document xmlns:n2="urn:iso:std:iso:20022:tech:xsd:supl.027.001.01" ...>
        <n2:InfRspnSD1>
          <n2:InvstgtnId>a</n2:InvstgtnId>
          <n2:CreDtTm>
            <!-- -->
          </n2:CreDtTm>
          <n2:AcctSvcrId>
            <n2:FinInstnId>
              <n2:Othr>
                <n2:Id>0828972-6</n2:Id>
                <n2:SchmeNm>
                  <n2:Cd>Y</n2:Cd>
                </n2:SchmeNm>
              </n2:Othr>
            </n2:FinInstnId>
          </n2:AcctSvcrId>
          <n2:AcctAndPties>
            <!-- Y-tunnuksen 0828972-6 hakutulokset, tili #2, tili #3, tili #4 -->
          </n2:AcctAndPties>
        </n2:InfRspnSD1>
      </n2:Document>
    </n1:Rslt>
  </n1:InvstgtnRslt>
</n1:RtrInd>
```

### <a name="InformationResponseSD1V01"></a> 4.8 InformationResponseSD1V01 supl.027.001.01

Taulukossa on kuvattu sanoman tietueiden käyttö.

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|InformationResponseSD1V01 supl.027.001.01| | | | |
|&nbsp;&nbsp;&nbsp;&nbsp;InvstgtnId|Max35Text|kyllä|[1..1]|Tutkinnan case-id|
|&nbsp;&nbsp;&nbsp;&nbsp;CreDtTm|ISODateTime|kyllä|[1..1]|Sanoman luomisaika|
|&nbsp;&nbsp;&nbsp;&nbsp;AcctSvcrId|BranchAndFinancialInstitutionIdentification4|kyllä|[1..1]|Käytetään seuraavasti: Elementti `AcctSvcrId/FinInstnId/Othr/SchmeNm/Cd` sisältää arvon "Y" ja elementti `AcctSvcrId/FinInstnId/Othr/Id` sisältää lähettäjän Y-tunnuksen.|
|&nbsp;&nbsp;&nbsp;&nbsp;AcctAndPties|AccountAndParties2|kyllä|[1..*]|Ks. taulukko alla|

#### AccountAndParties2 käyttö

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|AccountAndParties2| | | | |
|&nbsp;&nbsp;&nbsp;&nbsp;Acct|CustomerAccount1|kyllä|[1..*]|Tilin tiedot ks. CustomerAccount1 käyttö| 
|&nbsp;&nbsp;&nbsp;&nbsp;Role|AccountRole1|kyllä|[1..*]|Tiliin liittyvät roolit ks. toinen taulukko alla|
|&nbsp;&nbsp;&nbsp;&nbsp;AddtlInf|Max256Text|kyllä|[1..1]|Tilin avaamispäivämäärä merkkijonona ISODate-formaatissa|

#### CustomerAccount1 käyttö

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|CustomerAccount1| | | | |
|&nbsp;&nbsp;&nbsp;&nbsp;Id|AccountIdentification4Choice|kyllä|[1..1]|Joko IBAN tai muu tilin tunnistetieto, kts. `supl.027.001.01` -skeema|
|&nbsp;&nbsp;&nbsp;&nbsp;Nm||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;Sts||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;Tp||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;Ccy||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;MnthlyPmtVal||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;MnthlyRcvdVal||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;MnthlyTxNb||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;AvrgBal||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;AcctPurp||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;FlrNtfctnAmt||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;ClngNtfctnAmt||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;StmtCycl||ei|||
|&nbsp;&nbsp;&nbsp;&nbsp;ClsgDt|ISODate|kyllä|[0..1]|Tilin sulkemispäivämäärä|
|&nbsp;&nbsp;&nbsp;&nbsp;Rstrctn||ei|||

#### AccountRole1 käyttö

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|AccountRole1| | | | |
|&nbsp;&nbsp;&nbsp;&nbsp;Pty|PartyIdentification41|[1..*]|ks. [Id-elementin käyttö](#Id-elementin_kaytto)|
|&nbsp;&nbsp;&nbsp;&nbsp;OwnrTp|OwnerType1|[1..1]|Käytetään `OwnrTp/Prtry/SchmeNm/Cd` arvolla "RLTP", sekä `OwnrTp/Prtry/Id`, jossa arvot "POWN" (omistaja), "ACCE" (käyttöoikeus) tai "BENE" (edunsaaja)|
|&nbsp;&nbsp;&nbsp;&nbsp;StartDt|ISODate|[1..1]|Roolin alkamispäivämäärä|
|&nbsp;&nbsp;&nbsp;&nbsp;EndDt|ISODate|[0..1]|Roolin päättymispäivämäärä|

### <a name="InformationResponseFIN002"></a> 4.9 InformationResponseFIN002

Sanomalaajennus liitetään taulukossa listattuun ISO 20022 sanoman XPath-sijaintiin.

|Nimi|Pakollisuus (RAO)|[min..max]|Tyyppi|Index|Liitetään sanomaan|XPath|
|:---|:---|:---|:---|:---|:---|:---|
|InformationResponseFIN002| | | | |[auth.002](#InformationRequestResponseV01)|/Document/InfReqRspn/RtrInd/InvstgtnRslt/Rslt|
|&nbsp;&nbsp;&nbsp;&nbsp;InvstgtnId|Max35Text|kyllä|[1..1]|Tutkinnan case-id|
|&nbsp;&nbsp;&nbsp;&nbsp;CreDtTm|ISODateTime|kyllä|[1..1]|Sanoman luomisaika|
|&nbsp;&nbsp;&nbsp;&nbsp;SvcrId|BranchAndFinancialInstitutionIdentification4|kyllä|[1..1]|Käytetään seuraavasti: Elementti `SvcrId/FinInstnId/Othr/SchmeNm/Cd` sisältää arvon "Y" ja elementti `SvcrId/FinInstnId/Othr/Id` sisältää lähettäjän Y-tunnuksen.|
|&nbsp;&nbsp;&nbsp;&nbsp;SdBoxAndPties|O|[0..*]|SafetyDepositBoxAndParties|Tallelokero ja osalliset|

### <a name="InformationResponseFIN013"></a> 4.10 InformationResponseFIN013

Sanomalaajennus liitetään taulukossa listattuun ISO 20022 sanoman XPath-sijaintiin.

|Nimi|Pakollisuus (RAO)|[min..max]|Tyyppi|Index|Liitetään sanomaan|XPath|
|:---|:---|:---|:---|:---|:---|:---|
|InformationResponseFIN013| | | | |[auth.002](#InformationRequestResponseV01)|/Document/InfReqRspn/RtrInd/InvstgtnRslt/Rslt|
|&nbsp;&nbsp;&nbsp;&nbsp;InvstgtnId|Max35Text|kyllä|[1..1]|Tutkinnan case-id|
|&nbsp;&nbsp;&nbsp;&nbsp;CreDtTm|ISODateTime|kyllä|[1..1]|Sanoman luomisaika|
|&nbsp;&nbsp;&nbsp;&nbsp;SvcrId|BranchAndFinancialInstitutionIdentification4|kyllä|[1..1]|Käytetään seuraavasti: Elementti `SvcrId/FinInstnId/Othr/SchmeNm/Cd` sisältää arvon "Y" ja elementti `SvcrId/FinInstnId/Othr/Id` sisältää lähettäjän Y-tunnuksen.|
|&nbsp;&nbsp;&nbsp;&nbsp;Customer|O|[0..*]|Customer|Asiakas. Luonnollinen henkilö tai yritys. Ks. Customer-elementin käyttö taulukko alla|

#### Customer-elementin käyttö

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|Customer| | | | |
|&nbsp;&nbsp;&nbsp;&nbsp;Contract|Contract|kyllä|[1..1]| |
|&nbsp;&nbsp;&nbsp;&nbsp;Id|PartyIdentification41|kyllä|[1..1]|Ks. [Id-elementin käyttö](#Id-elementin_kaytto)|
|&nbsp;&nbsp;&nbsp;&nbsp;Beneficiaries|Beneficiaries|kyllä|[0..1]|Edunsaajat, ks. [Beneficiaries käyttö](#Beneficiaries_kaytto)|

#### <a name="Beneficiaries_kaytto"></a> Beneficiaries käyttö

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|Id|PartyIdentification41|kyllä|[1..*]|Ks. [Id-elementin käyttö](#Id-elementin_kaytto)|

### <a name="Id-elementin_kaytto"></a> 4.11 Id-elementin käyttö

Kaikissa sanomissa käytetään vastaavaa oikeushenkilön ja luonnollisen henkilön tunnistamisrakennetta Id-elementin (Party8Choice) alla. Tässä on kuvattu Id-elementin käyttö kyselyrajapinnassa.

|Nimi|Tyyppi|Käytössä|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|Id|Party8Choice|kyllä|[1..1]| |

#### Party8Choice

|Nimi|Pakollisuus (RAO)|Tyyppi|[min..max]|Kuvaus|
|:---|:---|:---|:---|:---|
|Party8Choice|| | | |
|&nbsp;&nbsp;&nbsp;&nbsp;OrgId|A|OrganisationIdentification6|[0..1]|Käytetään seuraavasti: Elementti `OrgId/Othr/SchmeNm/Cd` sisältää organisaatiotunnuksen tyyppikoodin ja elementti `OrgId/Othr/Id` sisältää tunnuksen. Ks. koodit taulukko alla. Lisäksi voidaan kyselyvastauksen yhteydessä palauttaa oikeushenkilön rekisteröitysmispäivämäärä ks. [esimerkki](#rgdt) alla|
|&nbsp;&nbsp;&nbsp;&nbsp;PrvtId|A|PersonIdentification5|[0..1]|Käytetään seuraavasti: Elementti `PrvtId/Othr/SchmeNm/Cd` sisältää henkilötunnisteen tyyppikoodin. Elementti `PrvtId/Othr/Id` sisältää tunnuksen. Ks. koodit taulukko alla.|

OrgId koodit  

|Koodi|Kuvaus|
|:---|:---|
|Y|Y-tunnus|
|PRH|Yhdistysrekisterinumero|
|*|Muu yritystunnus, lista koodeista Tullin toimittamassa erillisessä dokumentissa|

PrvtId koodit  

|Koodi|Kuvaus|
|:---|:---|
|PIC|Suomalainen henkilötunnus|
|OTHR|Muu henkilön tunnistusasiakirjan id|

#### <a name="rgdt"></a> Esimerkki oikeushenkilön rekisteröitymispäivän palauttamisesta

Oikeushenkilön rekisteröitymispäivämäärä palautetaan tunniste-elementin (esim. Y-tunnus) rinnakkaisena Othr-elementtinä:

Id-elementissä palautetaan rekisteröitymispäivämäärä. SchemeNm/Cd-elementissä palautetaan koodi RGDT, Issr-elementissä palautetaan rekisteriviranomaisen nimi.

```
<OrgId>
  <Othr>
    <Id>1234567-8</Id>
    <SchmeNm>
      <Cd>Y</Cd>
    </SchmeNm>
  </Othr>
  <Othr>
    <Id>2000-01-01</Id>
    <SchmeNm>
      <Cd>RGDT</Cd>
    </SchmeNm>
    <Issr>Verohallinto</Issr>
  </Othr>
</OrgId>
```

### 4.12 Kyselyrajapinnan WS-sanomaliikenteen skenaariot

Tässä luvussa on esitelty kyselyrajapinnan WS-sanomaliikenteen skenaariot. 

#### Skenaario 1 - OK

| | |
|---|---|  
Kuvaus|Sanoma käsiteltiin kokonaisuudessaan onnistuneesti.  
HTTP status code|202
Seuraamus|-|

#### Skenaario 3 - Virheellinen kysely

| | |
|---|---|
|Kuvaus|Kyselysanoma on virheellinen|
|HTTP status code|400|
|Seuraamus|Lähetä korjattu sanoma|

#### Skenaario 4 - Tekninen virhe

| | |
|---|---|
|Kuvaus|Tapahtui tekninen virhe|
|HTTP status code|500|
|Seuraamus|Viestiä ei ole käsitelty. Vastaussanoma sisältää virheen kuvauksen.|
