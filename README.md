# datalens_wordle

Код подготовки csv:
```import pandas as pd
import datetime

words = [
 'абзац','аванс','аврал','адепт','адрес','азарт','азиат','актер','акциз',
 'аллея','алмаз','алтын','алыча','амбар','ампер','ангар','ангел','анонс',
 'аорта','арбуз','ареал','арена','арест','армия','архив','аршин','астма',
 'астра','атака','атлет','афера','афиша','ацтек','багаж','багет','базар',
 'базис','балет','банан','банда','барак','барин','барон','басня','дозор',
 'донор','доход','дудка','дупло','дуэль','дюшес','дятел','надой','парта']

df_dates = pd.DataFrame({'word':words,
                        'date':[datetime.datetime.today().date()+datetime.timedelta(days=i) for i in range(len(words))]})
df_dates['key']=0

df_nums = pd.DataFrame({'try':[i for i in range(1,7)]})
df_nums['key']=0

df = df_dates.merge(df_nums,how='outer')
del(df['key'])

for each in ['1','2','3','4','5']:
    df[each] = df['word'].apply(lambda x: x[int(each)-1])

df.to_csv('datalens_wordle.csv',index=False)```
