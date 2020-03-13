<!-- TOC -->

- [Perché questo spazio](#perch%c3%a9-questo-spazio)
- [File di progetto QGIS](#file-di-progetto-qgis)
- [Cosa c'è in questo repo](#cosa-c%c3%a8-in-questo-repo)
- [Espressione usata](#espressione-usata)
- [Virtual layer](#virtual-layer)
- [Virtual File Format di GDAL/OGR](#virtual-file-format-di-gdalogr)
- [Atlas](#atlas)
- [Riferimenti utili](#riferimenti-utili)

<!-- /TOC -->

[![GitHub license](https://img.shields.io/badge/License-Creative%20Commons%20Attribution%204.0%20International-blue)](https://github.com/pigreco/COVID-19_ITA/blob/master/license)
[![GitHub commit](https://img.shields.io/github/last-commit/pcm-dpc/COVID-19)](https://github.com/pigreco/COVID-19_ITA/commits/master)

## Perché questo spazio

- **ITA** : Progetto QGIS per la visualizzazione dei dati COVID-19 attraverso un atlas con grafici dinamici - regioni ISTAT - fonte : https://github.com/pcm-dpc/COVID-19
- **ENG** : QGIS project for the visualization of COVID-19 data through an atlas with dynamic graphs - regions ISTAT - source : https://github.com/pcm-dpc/COVID-19

## File di progetto QGIS

I file di progetto **QGIS** (`COVID19_3857.qgs` e `COVID19_3857.qgs_noVL`) utilizzano come fonte dati il file [`dpc-covid19-ita-regioni.csv`](https://raw.githubusercontent.com/pcm-dpc/COVID-19/master/dati-regioni/dpc-covid19-ita-regioni.csv) presente nel repository ufficiale del [`PCM-DPC`](https://github.com/pcm-dpc/COVID-19) tramite Protocollo `HTTPS`, quindi il file si aggiorna automaticamente:

![](imgs/https.png)

Il file di progetto (`COVID19_3857_noVL_ogrVRT.qgs`) utilizza come fonte dati il file [`dpc-covid19-ita-regioni.csv`](https://raw.githubusercontent.com/pcm-dpc/COVID-19/master/dati-regioni/dpc-covid19-ita-regioni.csv) presente nel repository ufficiale del [`PCM-DPC`](https://github.com/pcm-dpc/COVID-19) tramite file `codid19-regioni_noWKT.vrt` (source un [Virtual File Format](https://gdal.org/drivers/vector/vrt.html#virtual-file-format) di GDAL/OGR), quindi il file si aggiorna automaticamente.

**NB:** il file di progetto è stato realizzato con `QGIS 3.12 București` e Plugin `DataPlotly 3.5`

## Cosa c'è in questo repo

- cartella `imgs` contiene le immagini utilizzate nel progetto .qgs;
- cartella `risorse` contiene i file utilizzati nel progetto, come:
  - `nroVerdeEmergenzaCOVID19.csv` è una tabella con i numeri verdi regionali per emergenza sanitaria;
  - `nroVerdeEmergenzaCOVID19.csvt` file di servizio per definire la tipologia di campi;
  - shapefile `reg_istat3857.*` limiti amministrativi regionali ISTAT 2019, EPSG:3857;
  - file `codid19-regioni.vrt` Virtual File Format GDAL/OGR con file CSV raw da GitHub, con geometry Point;
  - file `codid19-regioni_noWKT.vrt` Virtual File Format GDAL/OGR con file CSV raw da GitHub, no geometry;
  - file `codid19-regioni_dw.vrt` collegato a data.word, ma non funziona in QGIS;
- file `COVID19_3857_noVL.qgs` è il file di progetto QGIS in formato `.qgs` (senza usare Virtual layer), EPSG:3857;
- file `COVID19_3857.qgs` è il file di progetto QGIS in formato `.qgs` (usa Virtual layer), EPSG:3857;
- file `COVID19_3857_noVL_ogrVRT.qgs` è il file di progetto QGIS in formato `.qgs`, EPSG:3857 (`main`), usa OGRVRT;
- file `license` è il file che definisce la licenza del repository;
- file `README.md` è questo file, con le info.

[↑ torna su ↑](#perch%c3%a9-questo-spazio)

## Espressione usata

Per calcolo valori incrementali giornalieri è stata usata la seguente espressione nel Campo Y dei grafici `Scatter Plot`

```
with_variable(
'my_exp', 
array_find(  
array_agg( 
expression:= "data" , group_by:= "codice_regione" ,
order_by:="data"),"data"),
if( 
to_int(@my_exp) = 0, 
(array_agg( 
            expression:=  "terapia_intensiva" , 
            group_by:= "codice_regione" ,
            order_by:=  "data"  )[0]),
("terapia_intensiva"  -
(array_agg( 
expression:= "terapia_intensiva", 
group_by:= "codice_regione" ,
order_by:=  "data"  )[to_int(@my_exp)-1]))))
```

PS: per maggiori info sull'espressione: <https://pigrecoinfinito.com/2020/03/10/qgis-creare-grafici-con-incrementi-giornalieri/>

![](https://pigrecoinfinito.files.wordpress.com/2020/03/image-25.png)

[↑ torna su ↑](#perch%c3%a9-questo-spazio)

## Virtual layer

Layer : `virtual layer`

```sql
SELECT "codice_regione",
substr(data,1,10) as "data", 
sum(CAST ("ricoverati_con_sintomi" AS INT)) AS ricoverati,
sum(CAST ("deceduti" AS INT)) AS deceduti,
sum(CAST ("terapia_intensiva" AS INT)) AS terapia,
sum(CAST ("isolamento_domiciliare" AS INT)) AS isolamento,
sum(CAST ("dimessi_guariti" AS INT)) AS dimessi,
sum(CAST ("tamponi" AS INT)) AS tamponi,
sum(CAST("totale_casi" AS INT)) AS totale, count(*) as nro
FROM "dpc-covid19-ita-regioni"
GROUP BY 1,2;
```

Layer : `virtual layer complessivo`

```sql
SELECT 
substr(data,1,10) as "data", 
sum(CAST("totale_casi" AS INT)) AS totale,
sum(CAST("totale_attualmente_positivi" AS INT)) AS tot_att_pos,
sum(CAST("deceduti" AS INT)) AS deceduti,
sum(CAST("dimessi_guariti" AS INT)) AS guariti,
sum(CAST("terapia_intensiva" AS INT)) AS terapia,
sum(CAST("tamponi" AS INT)) AS tamponi
FROM "dpc-covid19-ita-regioni"
GROUP BY 1;
```

[↑ torna su ↑](#perch%c3%a9-questo-spazio)

## Virtual File Format di GDAL/OGR

link utile: <https://gdal.org/drivers/vector/vrt.html#virtual-file-format>

```xml
<OGRVRTDataSource>
<OGRVRTLayer name="dpc-covid19-ita-regioni">
    <SrcDataSource relativeToVRT="0">/vsicurl/https://raw.githubusercontent.com/pcm-dpc/COVID-19/master/dati-regioni/dpc-covid19-ita-regioni.csv</SrcDataSource>
    <Field name="data" type="String" />
    <Field name="lat" type="Real" />
    <Field name="long" type="Real" />
    <Field name="stato" type="String" />
    <Field name="codice_regione" type="String" />
    <Field name="denominazione_regione" type="String" />
	  <Field name="ricoverati_con_sintomi" type="Integer" />
    <Field name="terapia_intensiva" type="Integer" />
    <Field name="totale_ospedalizzati" type="Integer" />
    <Field name="isolamento_domiciliare" type="Integer" />
    <Field name="totale_attualmente_positivi" type="Integer" />
    <Field name="nuovi_attualmente_positivi" type="Integer" />
    <Field name="dimessi_guariti" type="Integer" />
    <Field name="deceduti" type="Integer" />
    <Field name="totale_casi" type="Integer" />
    <Field name="tamponi" type="Integer" />
</OGRVRTLayer>
</OGRVRTDataSource>
```

- per un quadro sinottico del file

```
ogrinfo codid19-regioni_dw.vrt dpc-covid19-ita-regioni -summary
```

- per leggere il file con OGR:

```
ogrinfo codid19-regioni_dw.vrt dpc-covid19-ita-regioni
```

- per usarlo in QGIS:

![](imgs/https_vrt.png)

## Atlas

Vettore di copertura : layer `reg_istat3857`

![](imgs/atlas_vl_01.png)

**Gif animata:**

![](./imgs/covid12_atlas.gif)

[↑ torna su ↑](#perch%c3%a9-questo-spazio)

## Riferimenti utili

- **QGIS** : <https://qgis.org/it/site/>
- **Plugin DataPlotly** : <https://plugins.qgis.org/plugins/DataPlotly/>
- **Fonti dati PCM-DPC** : <https://github.com/pcm-dpc/COVID-19>
- **CONFINI DELLE UNITÀ AMMINISTRATIVE A FINI STATISTICI AL 1 GENNAIO 2019** : <https://www.istat.it/it/archivio/222527>

[↑ torna su ↑](#perch%c3%a9-questo-spazio)
