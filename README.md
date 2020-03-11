## In costruzione ...

## Perché questo spazio

- **ITA** : Progetto QGIS per la visualizzazione dei dati COVID-19 attraverso un atlas con grafici dinamici - regioni ISTAT - fonte : https://github.com/pcm-dpc/COVID-19
- **ENG** : QGIS project for the visualization of COVID-19 data through an atlas with dynamic graphs - regions ISTAT - source : https://github.com/pcm-dpc/COVID-19

## Cosa c'è in questo repo

- cartella `imgs` contiene le immagini utilizzate nel progetto .qgs;
- cartella `risorse` contiene i file utilizzati nel progetto, come:
  - `nroVerdeEmergenzaCOVID19.csv` è una tabella con i numeri verdi regionali per emergenza sanitaria;
  - `nroVerdeEmergenzaCOVID19.csvt` file di servizio per definire la tipologia di campi;
  - shapefile `reg_istat3857.*` limiti amministrativi regionali ISTAT 2019, EPSG:3857;
- file `COVID19_3857_noVL.qgs` è il file di progetto QGIS in formato `.qgs` (senza usare Virtual layer), EPSG:3857 (IN LAVORAZIONE);
- file `COVID19_3857.qgs` è il file di progetto QGIS in formato `.qgs` (usa Virtual layer), EPSG:3857;
- file `license` è il file che definisce la licenza del repository;
- file `README.md` è questo file, con le info.

## Espressione per calcolo valori incrementali giornalieri

```
with_variable( 'my_exp', 
                array_find(  
                array_agg( 
                expression:= "data" ,
                order_by:=  "data"),"data" ),
if( @my_exp = 0,  -- condizione
               (array_agg( 
                expression:= "tot_att_pos"  , 
                order_by:=  "data"  )[0]), -- se vero
                     ("tot_att_pos"  -
                     (array_agg( 
                      expression:=  "tot_att_pos"  , 
                      order_by:=  "data"  )[@my_exp-1])) -- altrimenti
                )
              )
```

PS: per maggiori info sull'espressione: <https://pigrecoinfinito.com/2020/03/10/qgis-creare-grafici-con-incrementi-giornalieri/>

![](https://pigrecoinfinito.files.wordpress.com/2020/03/image-25.png)