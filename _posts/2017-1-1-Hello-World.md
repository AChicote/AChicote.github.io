---
layout: post
title: BarcelonaAccidents, Coming soon!!!
---

es, I let myself go and get sucked in by my origins. I just had to work on a database that is from my hometown (Barcelona) as a modest way to show my support to the fellow Catalans in these sad days.

![nofrauds](/images/BCN01.jpg){:height="300px" width="400px"}


## Brief description of Fraud Detection.
  1. Dataset contains all the accidents that the local police attended.
  2. THe information is gathered is spread in different csv: accidents, causes of the accident, type of accident and finaly the vehicles involved.
  3. It has been a hard work to combine all different csv due to different encodings and column's names.
  4. Once I have all data combined, I will decide what course of action to take.
  4. The link in github for a more detailed and technical explanation is partialy available.
  
## Non-technical description of BarcelonaAccidents.

## Gathering the data.
  
```
def combinant_csv(llista, year_0, year_1):

    suma = 0
    summa = 0
    files = 0
    llista1 = llista
    llista = pd.DataFrame()
    
    for it in range(year_0, 2016):
        file = '{}-files/{}_accidents_{}_gu_bcn_{}.csv'.format(it, it, llista1, it)
        encoding = 'ISO-8859-15'
        df = pd.read_csv(file, encoding = encoding)
        df.columns = [str(x).strip() for x in df.columns]
        df.columns = [c.replace(' ', '_').lower() for c in list(df)]
        new_columns = []
        for item in df.columns:
            nou_item = posant_accents(item)
            if '_de_' in nou_item:
                nou_item = nou_item.replace('_de_', "_")
            new_columns.append(nou_item)
        df.columns = new_columns
        df=df.rename(columns = {'coordenada_utm_(x)':'point_x', 'coordenada_utm_x':'point_x',\
                                         'coordenada_utm_(y)':'point_y', 'coordenada_utm_y':'point_y',\
                                           'nom_mes': 'month', 'numero_morts': 'amount_deceased',\
                               'codi_carrer': 'street_code', 'hora_dia': 'hour_day', 'nk_any': 'year',\
                               'nom_barri': 'neighborhood', 'numero_lesionats_greus': 'severely_injured',\
                               'numero_lesionats_lleus': 'minor_injures'})
        for item in df.columns:
            if item.startswith('long') == True or item.startswith('lat') == True:
                df.drop(item, axis=1, inplace=True)
        if 'mes_any' in df.columns:
            if 'month' in df.columns:
                df.drop('mes_any', axis=1, inplace= True)
            else:
                df['mes_any'] = df['mes_any'].fillna(0)
                df['month'] = df['mes_any'].apply(mesos_a_lletres)

        if 'dia_setmana' in df.columns:
            if 'descripcio_dia_setmana' in df.columns:
                df.drop('dia_setmana', axis=1, inplace=True)
            else:
                df['descripcio_dia_setmana'] = df['dia_setmana'].apply(setmana_a_lletres)
                df.drop('dia_setmana', axis=1, inplace=True)
        df.columns = ['vehicle_color' if 'color' in c else c for c in list(df)]
        df.columns = ['vehicle_brand_name' if 'marca' in c else c for c in list(df)]
        df.columns = ['vehicle_model' if 'model' in c else c for c in list(df)]
        df.columns = ["driver's_experience" if c == 'antiguitat_carnet' else c for c in list(df)]
        df.columns = ['day_month' if 'dia_mes' in c else c for c in list(df)]
        df.columns = ['perpetrator' if c.endswith('situacio') else c for c in list(df)]
        df.columns = ['type_vehicle' if c.endswith('vehicle_implicat')  or 'tipus_vehicle' in c else c for c in list(df)]
        df.columns = ['type_drivers_license' if c == 'descripcio_carnet' else c for c in list(df)]
        df.columns = ['age' if c == 'edat' else c for c in list(df)]
        df.columns = ['gender' if c.endswith('sexe') else c for c in list(df)]
        df.columns = ['district' if 'nom_districte' in c else c for c in list(df)]
        df.columns = ['week_day' if 'setmana' in c else c for c in list(df)]        
        df.columns = ['shift' if c.endswith('torn')else c for c in list(df)]
        df.columns = ['case_file' if c.endswith('pedient') else c for c in list(df)]
        df.columns = ['mediate_cause' if c.endswith('mediata')else c for c in list(df)]
        df.columns = ["pedestrian_cause" if c.endswith('vianant')else c for c in list(df)]
        df.columns = ['amount_vehicles_involved' if c.endswith('implicats') else c for c in list(df)]
        df.columns = ['type_day' if c.endswith('tipus_dia') else c for c in list(df)]
        df.columns = ['type_people' if c.endswith('persona') else c for c in list(df)]
        df.columns = ['dregree_injuries' if 'victimit' in c else c for c in list(df)]
        df.columns = ['amount_victims' if c.endswith('times') else c for c in list(df)]
        df.columns = ['accident_details' if c == 'descripcio_tipus_accident'else c for c in list(df)]
        df.columns = ['initial_street' if c.endswith('original') else c for c in list(df)]
        #df['nom_barri'] = df['nom_barri'].apply(posant_accents)
        df['case_file'] = df['case_file'].apply(lambda x: str(x).strip())
        suma = suma + len(df['case_file'].unique())
        llista = llista.append(df, ignore_index=True)
        
    for it in range(2016, year_1 + 1):
        
        file = '{}-files/{}_accidents_{}_gu_bcn_{}.csv'.format(it, it, llista1, it)
        encoding = 'utf-8'
        data = pd.read_csv(file, encoding = encoding)
        data.columns = [str(x).strip() for x in data.columns]
        data.columns = [c.replace(' ', '_').lower() for c in list(data)]
        new_columns = []

        for item in data.columns:
            nou_item = posant_accents(item)
            if '_de_' in nou_item:
                nou_item = nou_item.replace('_de_', "_")
            new_columns.append(nou_item)
        data.columns = new_columns
        data=data.rename(columns = {'coordenada_utm_(x)':'point_x', 'coordenada_utm_x':'point_x',\
                                         'coordenada_utm_(y)':'point_y', 'coordenada_utm_y':'point_y',\
                                   'nom_mes': 'month', 'numero_morts': 'amount_deceased',\
                                   'codi_carrer': 'street_code', 'hora_dia': 'hour_day',\
                                   'nk_any': 'year', 'nom_barri': 'neighborhood', 'numero_lesionats_greus': 'severely_injured',
                                   'numero_lesionats_lleus': 'minor_injures'})
        for item in data.columns:
            if item.startswith('long') == True or item.startswith('lat') == True:
                data.drop(item, axis=1, inplace=True)
        if 'mes_any' in data.columns:
            if 'month' in data.columns:
                data.drop('mes_any', axis=1, inplace= True)
            else:
                data['mes_any'] = data['mes_any'].fillna(0)
                data['month'] = data['mes_any'].apply(mesos_a_lletres)
            #data.drop('mes_any', axis=1, inplace=True)
        if 'dia_setmana' in data.columns:
            if 'descripcio_dia_setmana' in data.columns:
                data.drop('dia_setmana', axis=1, inplace=True)
            else:
                data['descripcio_dia_setmana'] = data['dia_setmana'].apply(setmana_a_lletres)
                data.drop('dia_setmana', axis=1, inplace=True)
        data.columns = ['vehicle_color' if 'color' in c else c for c in list(data)]
        data.columns = ['vehicle_brand_name' if 'marca' in c else c for c in list(data)]
        data.columns = ['vehicle_model' if 'model' in c else c for c in list(data)]
        data.columns = ['type_drivers_license' if c == 'descripcio_carnet' else c for c in list(data)]
        data.columns = ["driver's_experience" if c == 'antiguitat_carnet' else c for c in list(data)]
        data.columns = ['day_month' if 'dia_mes' in c else c for c in list(data)]
        data.columns = ['type_vehicle' if c.endswith('vehicle_implicat') or 'tipus_vehicle' in c else c for c in list(data)]
        data.columns = ['perpetrator' if c.endswith('situacio') else c for c in list(data)]
        data.columns = ['age' if 'edat' in c else c for c in list(data)]
        data.columns = ['gender' if c.endswith('sexe') else c for c in list(data)]
        data.columns = ['district' if 'nom_districte' in c else c for c in list(data)]
        data.columns = ['week_day' if 'setmana' in c else c for c in list(data)]
        data.columns = ['shift' if c.endswith('torn')else c for c in list(data)]
        data.columns = ['case_file' if c.endswith('pedient') else c for c in list(data)]
        data.columns = ['mediate_cause' if c.endswith('mediata')else c for c in list(data)]
        data.columns = ["pedestrian_cause" if c.endswith('vianant')else c for c in list(data)]
        data.columns = ['amount_vehicles_involved' if c.endswith('implicats') else c for c in list(data)]
        data.columns = ['type_day' if c.endswith('tipus_dia') else c for c in list(data)]
        data.columns = ['type_people' if c.endswith('persona') else c for c in list(data)]
        data.columns = ['dregree_injuries' if 'victimit' in c else c for c in list(data)]
        data.columns = ['amount_victims' if c.endswith('times') else c for c in list(data)]
        data.columns = ['accident_details' if c == 'descripcio_tipus_accident'else c for c in list(data)]
        data.columns = ['initial_street' if c.endswith('original') else c for c in list(data)]
        data['case_file'] = data['case_file'].apply(lambda x: str(x).strip())
        summa = summa + len(data['case_file'].unique())
        resuma = suma + summa
        files = files + data.shape[0]
        
        llista = llista.append(data, ignore_index=True)
    for item in llista.columns:
    
        llista[item] = llista[item].apply(posant_accents)
        llista[item] = llista[item].apply(traduir_castella)
    for c in llista.columns:
        if c in ['codi_districte', 'nk_barri', 'codi_barri', 'nom_carrer', 'num_postal_caption']:
            llista.drop(c, axis=1, inplace=True)
        if 'shift' in c:
            llista.drop(llista[llista['shift'] == '13:25:52'].index, inplace=True)
        
    return llista
#####Merging dataframes
def merging_df(name1, aggregated_df):
    adding = set(name1.columns) - set(aggregated_df.columns)
    adding = list(adding)
    adding.append('case_file')
    aggregated_df = pd.merge(aggregated_df, name1[adding], on = 'case_file', how ='outer')
    return aggregated_df

```
