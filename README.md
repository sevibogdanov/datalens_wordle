# datalens_wordle

<details>
<summary>Код подготовки csv</summary>  
 
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

</details>


<details>
<summary>Код подготовки формулы для отображения списка введенных букв</summary>

```import pyperclip

#для каждой буквы алфавита выполняется проверка была ли она введена и на какой позиции стоит, есть ли она в слове
#в зависимости от результата буква красится в цвет
#итоговый код получается на 1700 строк, что обуславливает его долгую работу

alphabet = 'абвгдежзийклмнопрстуфхцчшщъыьэюя'

txt = "MARKUP('| ',"
for i in alphabet:
    text = f"""if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='{i}') or
            (left(right([w2],5),1) = [1] and [1]='{i}') or
            (left(right([w3],5),1) = [1] and [1]='{i}') or
            (left(right([w4],5),1) = [1] and [1]='{i}') or
            (left(right([w5],5),1) = [1] and [1]='{i}') or
            (left(right([w6],5),1) = [1] and [1]='{i}') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='{i}') or
            (left(right([w2],4),1) = [2] and [2]='{i}') or
            (left(right([w3],4),1) = [2] and [2]='{i}') or
            (left(right([w4],4),1) = [2] and [2]='{i}') or
            (left(right([w5],4),1) = [2] and [2]='{i}') or
            (left(right([w6],4),1) = [2] and [2]='{i}') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='{i}') or
            (left(right([w2],3),1) = [3] and [3]='{i}') or
            (left(right([w3],3),1) = [3] and [3]='{i}') or
            (left(right([w4],3),1) = [3] and [3]='{i}') or
            (left(right([w5],3),1) = [3] and [3]='{i}') or
            (left(right([w6],3),1) = [3] and [3]='{i}') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='{i}') or
            (left(right([w2],2),1) = [4] and [4]='{i}') or
            (left(right([w3],2),1) = [4] and [4]='{i}') or
            (left(right([w4],2),1) = [4] and [4]='{i}') or
            (left(right([w5],2),1) = [4] and [4]='{i}') or
            (left(right([w6],2),1) = [4] and [4]='{i}') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='{i}') or
            (left(right([w2],1),1) = [5] and [5]='{i}') or
            (left(right([w3],1),1) = [5] and [5]='{i}') or
            (left(right([w4],1),1) = [5] and [5]='{i}') or
            (left(right([w5],1),1) = [5] and [5]='{i}') or
            (left(right([w6],1),1) = [5] and [5]='{i}')
            ,bold(color(upper('{i}'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'{i}') and  contains([word],'{i}')) or
            (contains([w2],'{i}') and  contains([word],'{i}')) or
            (contains([w3],'{i}') and  contains([word],'{i}')) or
            (contains([w4],'{i}') and  contains([word],'{i}')) or
            (contains([w5],'{i}') and  contains([word],'{i}')) or
            (contains([w6],'{i}') and  contains([word],'{i}'))
            ,bold(color(upper('{i}'),'#ffa500')),
    /*red*/if(
            (contains([w1],'{i}') and  not contains([word],'{i}')) or
            (contains([w2],'{i}') and  not contains([word],'{i}')) or
            (contains([w3],'{i}') and  not contains([word],'{i}')) or
            (contains([w4],'{i}') and  not contains([word],'{i}')) or
            (contains([w5],'{i}') and  not contains([word],'{i}')) or
            (contains([w6],'{i}') and  not contains([word],'{i}'))
            ,bold(color(upper('{i}'),'#ff0000')),
        bold(color(upper('{i}'),'#000000')))))
        , ' | ',"""
    txt+=text
txt = txt[:-1]+')'

pyperclip.copy(txt)```

</details>
