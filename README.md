# datalens_wordle

[Дашборд в DataLens](https://datalens.yandex/3ni4lu5720msq)

## Пример расчета поля "01"

```
/*в поле try указан номер попытки
в полях [1]-[5] - буквы в слове по порядку для удобства
поле [word] - слово
поле [date] - дата
[w1]-[w6] - параметра для приема попыток
*/

case [try] 
    when 1 then case left([w1],1)
        when '*' then '*'
        when [1]+[2] then [1]
        when [1] then '+'
        when [2] then '~'
        when [3] then '~'
        when [4] then '~'
        when [5] then '~'
        else '-' end
    when 2 then case left([w2],1)
        when '*' then '*'
        when [1]+[2] then [1]
        when [1] then '+'
        when [2] then '~'
        when [3] then '~'
        when [4] then '~'
        when [5] then '~'
        else '-' end
    when 3 then case left([w3],1)
        when '*' then '*'
        when [1]+[2] then [1]
        when [1] then '+'
        when [2] then '~'
        when [3] then '~'
        when [4] then '~'
        when [5] then '~'
        else '-' end
    when 4 then case left([w4],1)
        when '*' then '*'
        when [1]+[2] then [1]
        when [1] then '+'
        when [2] then '~'
        when [3] then '~'
        when [4] then '~'
        when [5] then '~'
        else '-' end
    when 5 then case left([w5],1)
        when '*' then '*'
        when [1]+[2] then [1]
        when [1] then '+'
        when [2] then '~'
        when [3] then '~'
        when [4] then '~'
        when [5] then '~'
        else '-' end
    when 6 then case left([w6],1)
        when '*' then '*'
        when [1]+[2] then [1]
        when [1] then '+'
        when [2] then '~'
        when [3] then '~'
        when [4] then '~'
        when [5] then '~'
        else '-' end
end
```

<details>
<summary>Код подготовки csv</summary>  
 
``` python
import pandas as pd
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

df.to_csv('datalens_wordle.csv',index=False)
```
</details>


<details>  
<summary>Код подготовки формулы для отображения списка введенных букв</summary>   

``` python  
import pyperclip

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

pyperclip.copy(txt)
```
</details>


<details>  
<summary>Итоговый код формулы для отображения списка введенных букв</summary> 
 
```
MARKUP('| ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='а') or
            (left(right([w2],5),1) = [1] and [1]='а') or
            (left(right([w3],5),1) = [1] and [1]='а') or
            (left(right([w4],5),1) = [1] and [1]='а') or
            (left(right([w5],5),1) = [1] and [1]='а') or
            (left(right([w6],5),1) = [1] and [1]='а') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='а') or
            (left(right([w2],4),1) = [2] and [2]='а') or
            (left(right([w3],4),1) = [2] and [2]='а') or
            (left(right([w4],4),1) = [2] and [2]='а') or
            (left(right([w5],4),1) = [2] and [2]='а') or
            (left(right([w6],4),1) = [2] and [2]='а') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='а') or
            (left(right([w2],3),1) = [3] and [3]='а') or
            (left(right([w3],3),1) = [3] and [3]='а') or
            (left(right([w4],3),1) = [3] and [3]='а') or
            (left(right([w5],3),1) = [3] and [3]='а') or
            (left(right([w6],3),1) = [3] and [3]='а') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='а') or
            (left(right([w2],2),1) = [4] and [4]='а') or
            (left(right([w3],2),1) = [4] and [4]='а') or
            (left(right([w4],2),1) = [4] and [4]='а') or
            (left(right([w5],2),1) = [4] and [4]='а') or
            (left(right([w6],2),1) = [4] and [4]='а') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='а') or
            (left(right([w2],1),1) = [5] and [5]='а') or
            (left(right([w3],1),1) = [5] and [5]='а') or
            (left(right([w4],1),1) = [5] and [5]='а') or
            (left(right([w5],1),1) = [5] and [5]='а') or
            (left(right([w6],1),1) = [5] and [5]='а')
            ,bold(color(upper('а'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'а') and  contains([word],'а')) or
            (contains([w2],'а') and  contains([word],'а')) or
            (contains([w3],'а') and  contains([word],'а')) or
            (contains([w4],'а') and  contains([word],'а')) or
            (contains([w5],'а') and  contains([word],'а')) or
            (contains([w6],'а') and  contains([word],'а'))
            ,bold(color(upper('а'),'#ffa500')),
    /*red*/if(
            (contains([w1],'а') and  not contains([word],'а')) or
            (contains([w2],'а') and  not contains([word],'а')) or
            (contains([w3],'а') and  not contains([word],'а')) or
            (contains([w4],'а') and  not contains([word],'а')) or
            (contains([w5],'а') and  not contains([word],'а')) or
            (contains([w6],'а') and  not contains([word],'а'))
            ,bold(color(upper('а'),'#ff0000')),
        bold(color(upper('а'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='б') or
            (left(right([w2],5),1) = [1] and [1]='б') or
            (left(right([w3],5),1) = [1] and [1]='б') or
            (left(right([w4],5),1) = [1] and [1]='б') or
            (left(right([w5],5),1) = [1] and [1]='б') or
            (left(right([w6],5),1) = [1] and [1]='б') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='б') or
            (left(right([w2],4),1) = [2] and [2]='б') or
            (left(right([w3],4),1) = [2] and [2]='б') or
            (left(right([w4],4),1) = [2] and [2]='б') or
            (left(right([w5],4),1) = [2] and [2]='б') or
            (left(right([w6],4),1) = [2] and [2]='б') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='б') or
            (left(right([w2],3),1) = [3] and [3]='б') or
            (left(right([w3],3),1) = [3] and [3]='б') or
            (left(right([w4],3),1) = [3] and [3]='б') or
            (left(right([w5],3),1) = [3] and [3]='б') or
            (left(right([w6],3),1) = [3] and [3]='б') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='б') or
            (left(right([w2],2),1) = [4] and [4]='б') or
            (left(right([w3],2),1) = [4] and [4]='б') or
            (left(right([w4],2),1) = [4] and [4]='б') or
            (left(right([w5],2),1) = [4] and [4]='б') or
            (left(right([w6],2),1) = [4] and [4]='б') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='б') or
            (left(right([w2],1),1) = [5] and [5]='б') or
            (left(right([w3],1),1) = [5] and [5]='б') or
            (left(right([w4],1),1) = [5] and [5]='б') or
            (left(right([w5],1),1) = [5] and [5]='б') or
            (left(right([w6],1),1) = [5] and [5]='б')
            ,bold(color(upper('б'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'б') and  contains([word],'б')) or
            (contains([w2],'б') and  contains([word],'б')) or
            (contains([w3],'б') and  contains([word],'б')) or
            (contains([w4],'б') and  contains([word],'б')) or
            (contains([w5],'б') and  contains([word],'б')) or
            (contains([w6],'б') and  contains([word],'б'))
            ,bold(color(upper('б'),'#ffa500')),
    /*red*/if(
            (contains([w1],'б') and  not contains([word],'б')) or
            (contains([w2],'б') and  not contains([word],'б')) or
            (contains([w3],'б') and  not contains([word],'б')) or
            (contains([w4],'б') and  not contains([word],'б')) or
            (contains([w5],'б') and  not contains([word],'б')) or
            (contains([w6],'б') and  not contains([word],'б'))
            ,bold(color(upper('б'),'#ff0000')),
        bold(color(upper('б'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='в') or
            (left(right([w2],5),1) = [1] and [1]='в') or
            (left(right([w3],5),1) = [1] and [1]='в') or
            (left(right([w4],5),1) = [1] and [1]='в') or
            (left(right([w5],5),1) = [1] and [1]='в') or
            (left(right([w6],5),1) = [1] and [1]='в') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='в') or
            (left(right([w2],4),1) = [2] and [2]='в') or
            (left(right([w3],4),1) = [2] and [2]='в') or
            (left(right([w4],4),1) = [2] and [2]='в') or
            (left(right([w5],4),1) = [2] and [2]='в') or
            (left(right([w6],4),1) = [2] and [2]='в') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='в') or
            (left(right([w2],3),1) = [3] and [3]='в') or
            (left(right([w3],3),1) = [3] and [3]='в') or
            (left(right([w4],3),1) = [3] and [3]='в') or
            (left(right([w5],3),1) = [3] and [3]='в') or
            (left(right([w6],3),1) = [3] and [3]='в') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='в') or
            (left(right([w2],2),1) = [4] and [4]='в') or
            (left(right([w3],2),1) = [4] and [4]='в') or
            (left(right([w4],2),1) = [4] and [4]='в') or
            (left(right([w5],2),1) = [4] and [4]='в') or
            (left(right([w6],2),1) = [4] and [4]='в') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='в') or
            (left(right([w2],1),1) = [5] and [5]='в') or
            (left(right([w3],1),1) = [5] and [5]='в') or
            (left(right([w4],1),1) = [5] and [5]='в') or
            (left(right([w5],1),1) = [5] and [5]='в') or
            (left(right([w6],1),1) = [5] and [5]='в')
            ,bold(color(upper('в'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'в') and  contains([word],'в')) or
            (contains([w2],'в') and  contains([word],'в')) or
            (contains([w3],'в') and  contains([word],'в')) or
            (contains([w4],'в') and  contains([word],'в')) or
            (contains([w5],'в') and  contains([word],'в')) or
            (contains([w6],'в') and  contains([word],'в'))
            ,bold(color(upper('в'),'#ffa500')),
    /*red*/if(
            (contains([w1],'в') and  not contains([word],'в')) or
            (contains([w2],'в') and  not contains([word],'в')) or
            (contains([w3],'в') and  not contains([word],'в')) or
            (contains([w4],'в') and  not contains([word],'в')) or
            (contains([w5],'в') and  not contains([word],'в')) or
            (contains([w6],'в') and  not contains([word],'в'))
            ,bold(color(upper('в'),'#ff0000')),
        bold(color(upper('в'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='г') or
            (left(right([w2],5),1) = [1] and [1]='г') or
            (left(right([w3],5),1) = [1] and [1]='г') or
            (left(right([w4],5),1) = [1] and [1]='г') or
            (left(right([w5],5),1) = [1] and [1]='г') or
            (left(right([w6],5),1) = [1] and [1]='г') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='г') or
            (left(right([w2],4),1) = [2] and [2]='г') or
            (left(right([w3],4),1) = [2] and [2]='г') or
            (left(right([w4],4),1) = [2] and [2]='г') or
            (left(right([w5],4),1) = [2] and [2]='г') or
            (left(right([w6],4),1) = [2] and [2]='г') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='г') or
            (left(right([w2],3),1) = [3] and [3]='г') or
            (left(right([w3],3),1) = [3] and [3]='г') or
            (left(right([w4],3),1) = [3] and [3]='г') or
            (left(right([w5],3),1) = [3] and [3]='г') or
            (left(right([w6],3),1) = [3] and [3]='г') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='г') or
            (left(right([w2],2),1) = [4] and [4]='г') or
            (left(right([w3],2),1) = [4] and [4]='г') or
            (left(right([w4],2),1) = [4] and [4]='г') or
            (left(right([w5],2),1) = [4] and [4]='г') or
            (left(right([w6],2),1) = [4] and [4]='г') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='г') or
            (left(right([w2],1),1) = [5] and [5]='г') or
            (left(right([w3],1),1) = [5] and [5]='г') or
            (left(right([w4],1),1) = [5] and [5]='г') or
            (left(right([w5],1),1) = [5] and [5]='г') or
            (left(right([w6],1),1) = [5] and [5]='г')
            ,bold(color(upper('г'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'г') and  contains([word],'г')) or
            (contains([w2],'г') and  contains([word],'г')) or
            (contains([w3],'г') and  contains([word],'г')) or
            (contains([w4],'г') and  contains([word],'г')) or
            (contains([w5],'г') and  contains([word],'г')) or
            (contains([w6],'г') and  contains([word],'г'))
            ,bold(color(upper('г'),'#ffa500')),
    /*red*/if(
            (contains([w1],'г') and  not contains([word],'г')) or
            (contains([w2],'г') and  not contains([word],'г')) or
            (contains([w3],'г') and  not contains([word],'г')) or
            (contains([w4],'г') and  not contains([word],'г')) or
            (contains([w5],'г') and  not contains([word],'г')) or
            (contains([w6],'г') and  not contains([word],'г'))
            ,bold(color(upper('г'),'#ff0000')),
        bold(color(upper('г'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='д') or
            (left(right([w2],5),1) = [1] and [1]='д') or
            (left(right([w3],5),1) = [1] and [1]='д') or
            (left(right([w4],5),1) = [1] and [1]='д') or
            (left(right([w5],5),1) = [1] and [1]='д') or
            (left(right([w6],5),1) = [1] and [1]='д') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='д') or
            (left(right([w2],4),1) = [2] and [2]='д') or
            (left(right([w3],4),1) = [2] and [2]='д') or
            (left(right([w4],4),1) = [2] and [2]='д') or
            (left(right([w5],4),1) = [2] and [2]='д') or
            (left(right([w6],4),1) = [2] and [2]='д') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='д') or
            (left(right([w2],3),1) = [3] and [3]='д') or
            (left(right([w3],3),1) = [3] and [3]='д') or
            (left(right([w4],3),1) = [3] and [3]='д') or
            (left(right([w5],3),1) = [3] and [3]='д') or
            (left(right([w6],3),1) = [3] and [3]='д') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='д') or
            (left(right([w2],2),1) = [4] and [4]='д') or
            (left(right([w3],2),1) = [4] and [4]='д') or
            (left(right([w4],2),1) = [4] and [4]='д') or
            (left(right([w5],2),1) = [4] and [4]='д') or
            (left(right([w6],2),1) = [4] and [4]='д') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='д') or
            (left(right([w2],1),1) = [5] and [5]='д') or
            (left(right([w3],1),1) = [5] and [5]='д') or
            (left(right([w4],1),1) = [5] and [5]='д') or
            (left(right([w5],1),1) = [5] and [5]='д') or
            (left(right([w6],1),1) = [5] and [5]='д')
            ,bold(color(upper('д'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'д') and  contains([word],'д')) or
            (contains([w2],'д') and  contains([word],'д')) or
            (contains([w3],'д') and  contains([word],'д')) or
            (contains([w4],'д') and  contains([word],'д')) or
            (contains([w5],'д') and  contains([word],'д')) or
            (contains([w6],'д') and  contains([word],'д'))
            ,bold(color(upper('д'),'#ffa500')),
    /*red*/if(
            (contains([w1],'д') and  not contains([word],'д')) or
            (contains([w2],'д') and  not contains([word],'д')) or
            (contains([w3],'д') and  not contains([word],'д')) or
            (contains([w4],'д') and  not contains([word],'д')) or
            (contains([w5],'д') and  not contains([word],'д')) or
            (contains([w6],'д') and  not contains([word],'д'))
            ,bold(color(upper('д'),'#ff0000')),
        bold(color(upper('д'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='е') or
            (left(right([w2],5),1) = [1] and [1]='е') or
            (left(right([w3],5),1) = [1] and [1]='е') or
            (left(right([w4],5),1) = [1] and [1]='е') or
            (left(right([w5],5),1) = [1] and [1]='е') or
            (left(right([w6],5),1) = [1] and [1]='е') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='е') or
            (left(right([w2],4),1) = [2] and [2]='е') or
            (left(right([w3],4),1) = [2] and [2]='е') or
            (left(right([w4],4),1) = [2] and [2]='е') or
            (left(right([w5],4),1) = [2] and [2]='е') or
            (left(right([w6],4),1) = [2] and [2]='е') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='е') or
            (left(right([w2],3),1) = [3] and [3]='е') or
            (left(right([w3],3),1) = [3] and [3]='е') or
            (left(right([w4],3),1) = [3] and [3]='е') or
            (left(right([w5],3),1) = [3] and [3]='е') or
            (left(right([w6],3),1) = [3] and [3]='е') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='е') or
            (left(right([w2],2),1) = [4] and [4]='е') or
            (left(right([w3],2),1) = [4] and [4]='е') or
            (left(right([w4],2),1) = [4] and [4]='е') or
            (left(right([w5],2),1) = [4] and [4]='е') or
            (left(right([w6],2),1) = [4] and [4]='е') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='е') or
            (left(right([w2],1),1) = [5] and [5]='е') or
            (left(right([w3],1),1) = [5] and [5]='е') or
            (left(right([w4],1),1) = [5] and [5]='е') or
            (left(right([w5],1),1) = [5] and [5]='е') or
            (left(right([w6],1),1) = [5] and [5]='е')
            ,bold(color(upper('е'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'е') and  contains([word],'е')) or
            (contains([w2],'е') and  contains([word],'е')) or
            (contains([w3],'е') and  contains([word],'е')) or
            (contains([w4],'е') and  contains([word],'е')) or
            (contains([w5],'е') and  contains([word],'е')) or
            (contains([w6],'е') and  contains([word],'е'))
            ,bold(color(upper('е'),'#ffa500')),
    /*red*/if(
            (contains([w1],'е') and  not contains([word],'е')) or
            (contains([w2],'е') and  not contains([word],'е')) or
            (contains([w3],'е') and  not contains([word],'е')) or
            (contains([w4],'е') and  not contains([word],'е')) or
            (contains([w5],'е') and  not contains([word],'е')) or
            (contains([w6],'е') and  not contains([word],'е'))
            ,bold(color(upper('е'),'#ff0000')),
        bold(color(upper('е'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='ж') or
            (left(right([w2],5),1) = [1] and [1]='ж') or
            (left(right([w3],5),1) = [1] and [1]='ж') or
            (left(right([w4],5),1) = [1] and [1]='ж') or
            (left(right([w5],5),1) = [1] and [1]='ж') or
            (left(right([w6],5),1) = [1] and [1]='ж') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='ж') or
            (left(right([w2],4),1) = [2] and [2]='ж') or
            (left(right([w3],4),1) = [2] and [2]='ж') or
            (left(right([w4],4),1) = [2] and [2]='ж') or
            (left(right([w5],4),1) = [2] and [2]='ж') or
            (left(right([w6],4),1) = [2] and [2]='ж') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='ж') or
            (left(right([w2],3),1) = [3] and [3]='ж') or
            (left(right([w3],3),1) = [3] and [3]='ж') or
            (left(right([w4],3),1) = [3] and [3]='ж') or
            (left(right([w5],3),1) = [3] and [3]='ж') or
            (left(right([w6],3),1) = [3] and [3]='ж') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='ж') or
            (left(right([w2],2),1) = [4] and [4]='ж') or
            (left(right([w3],2),1) = [4] and [4]='ж') or
            (left(right([w4],2),1) = [4] and [4]='ж') or
            (left(right([w5],2),1) = [4] and [4]='ж') or
            (left(right([w6],2),1) = [4] and [4]='ж') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='ж') or
            (left(right([w2],1),1) = [5] and [5]='ж') or
            (left(right([w3],1),1) = [5] and [5]='ж') or
            (left(right([w4],1),1) = [5] and [5]='ж') or
            (left(right([w5],1),1) = [5] and [5]='ж') or
            (left(right([w6],1),1) = [5] and [5]='ж')
            ,bold(color(upper('ж'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'ж') and  contains([word],'ж')) or
            (contains([w2],'ж') and  contains([word],'ж')) or
            (contains([w3],'ж') and  contains([word],'ж')) or
            (contains([w4],'ж') and  contains([word],'ж')) or
            (contains([w5],'ж') and  contains([word],'ж')) or
            (contains([w6],'ж') and  contains([word],'ж'))
            ,bold(color(upper('ж'),'#ffa500')),
    /*red*/if(
            (contains([w1],'ж') and  not contains([word],'ж')) or
            (contains([w2],'ж') and  not contains([word],'ж')) or
            (contains([w3],'ж') and  not contains([word],'ж')) or
            (contains([w4],'ж') and  not contains([word],'ж')) or
            (contains([w5],'ж') and  not contains([word],'ж')) or
            (contains([w6],'ж') and  not contains([word],'ж'))
            ,bold(color(upper('ж'),'#ff0000')),
        bold(color(upper('ж'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='з') or
            (left(right([w2],5),1) = [1] and [1]='з') or
            (left(right([w3],5),1) = [1] and [1]='з') or
            (left(right([w4],5),1) = [1] and [1]='з') or
            (left(right([w5],5),1) = [1] and [1]='з') or
            (left(right([w6],5),1) = [1] and [1]='з') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='з') or
            (left(right([w2],4),1) = [2] and [2]='з') or
            (left(right([w3],4),1) = [2] and [2]='з') or
            (left(right([w4],4),1) = [2] and [2]='з') or
            (left(right([w5],4),1) = [2] and [2]='з') or
            (left(right([w6],4),1) = [2] and [2]='з') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='з') or
            (left(right([w2],3),1) = [3] and [3]='з') or
            (left(right([w3],3),1) = [3] and [3]='з') or
            (left(right([w4],3),1) = [3] and [3]='з') or
            (left(right([w5],3),1) = [3] and [3]='з') or
            (left(right([w6],3),1) = [3] and [3]='з') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='з') or
            (left(right([w2],2),1) = [4] and [4]='з') or
            (left(right([w3],2),1) = [4] and [4]='з') or
            (left(right([w4],2),1) = [4] and [4]='з') or
            (left(right([w5],2),1) = [4] and [4]='з') or
            (left(right([w6],2),1) = [4] and [4]='з') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='з') or
            (left(right([w2],1),1) = [5] and [5]='з') or
            (left(right([w3],1),1) = [5] and [5]='з') or
            (left(right([w4],1),1) = [5] and [5]='з') or
            (left(right([w5],1),1) = [5] and [5]='з') or
            (left(right([w6],1),1) = [5] and [5]='з')
            ,bold(color(upper('з'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'з') and  contains([word],'з')) or
            (contains([w2],'з') and  contains([word],'з')) or
            (contains([w3],'з') and  contains([word],'з')) or
            (contains([w4],'з') and  contains([word],'з')) or
            (contains([w5],'з') and  contains([word],'з')) or
            (contains([w6],'з') and  contains([word],'з'))
            ,bold(color(upper('з'),'#ffa500')),
    /*red*/if(
            (contains([w1],'з') and  not contains([word],'з')) or
            (contains([w2],'з') and  not contains([word],'з')) or
            (contains([w3],'з') and  not contains([word],'з')) or
            (contains([w4],'з') and  not contains([word],'з')) or
            (contains([w5],'з') and  not contains([word],'з')) or
            (contains([w6],'з') and  not contains([word],'з'))
            ,bold(color(upper('з'),'#ff0000')),
        bold(color(upper('з'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='и') or
            (left(right([w2],5),1) = [1] and [1]='и') or
            (left(right([w3],5),1) = [1] and [1]='и') or
            (left(right([w4],5),1) = [1] and [1]='и') or
            (left(right([w5],5),1) = [1] and [1]='и') or
            (left(right([w6],5),1) = [1] and [1]='и') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='и') or
            (left(right([w2],4),1) = [2] and [2]='и') or
            (left(right([w3],4),1) = [2] and [2]='и') or
            (left(right([w4],4),1) = [2] and [2]='и') or
            (left(right([w5],4),1) = [2] and [2]='и') or
            (left(right([w6],4),1) = [2] and [2]='и') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='и') or
            (left(right([w2],3),1) = [3] and [3]='и') or
            (left(right([w3],3),1) = [3] and [3]='и') or
            (left(right([w4],3),1) = [3] and [3]='и') or
            (left(right([w5],3),1) = [3] and [3]='и') or
            (left(right([w6],3),1) = [3] and [3]='и') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='и') or
            (left(right([w2],2),1) = [4] and [4]='и') or
            (left(right([w3],2),1) = [4] and [4]='и') or
            (left(right([w4],2),1) = [4] and [4]='и') or
            (left(right([w5],2),1) = [4] and [4]='и') or
            (left(right([w6],2),1) = [4] and [4]='и') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='и') or
            (left(right([w2],1),1) = [5] and [5]='и') or
            (left(right([w3],1),1) = [5] and [5]='и') or
            (left(right([w4],1),1) = [5] and [5]='и') or
            (left(right([w5],1),1) = [5] and [5]='и') or
            (left(right([w6],1),1) = [5] and [5]='и')
            ,bold(color(upper('и'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'и') and  contains([word],'и')) or
            (contains([w2],'и') and  contains([word],'и')) or
            (contains([w3],'и') and  contains([word],'и')) or
            (contains([w4],'и') and  contains([word],'и')) or
            (contains([w5],'и') and  contains([word],'и')) or
            (contains([w6],'и') and  contains([word],'и'))
            ,bold(color(upper('и'),'#ffa500')),
    /*red*/if(
            (contains([w1],'и') and  not contains([word],'и')) or
            (contains([w2],'и') and  not contains([word],'и')) or
            (contains([w3],'и') and  not contains([word],'и')) or
            (contains([w4],'и') and  not contains([word],'и')) or
            (contains([w5],'и') and  not contains([word],'и')) or
            (contains([w6],'и') and  not contains([word],'и'))
            ,bold(color(upper('и'),'#ff0000')),
        bold(color(upper('и'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='й') or
            (left(right([w2],5),1) = [1] and [1]='й') or
            (left(right([w3],5),1) = [1] and [1]='й') or
            (left(right([w4],5),1) = [1] and [1]='й') or
            (left(right([w5],5),1) = [1] and [1]='й') or
            (left(right([w6],5),1) = [1] and [1]='й') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='й') or
            (left(right([w2],4),1) = [2] and [2]='й') or
            (left(right([w3],4),1) = [2] and [2]='й') or
            (left(right([w4],4),1) = [2] and [2]='й') or
            (left(right([w5],4),1) = [2] and [2]='й') or
            (left(right([w6],4),1) = [2] and [2]='й') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='й') or
            (left(right([w2],3),1) = [3] and [3]='й') or
            (left(right([w3],3),1) = [3] and [3]='й') or
            (left(right([w4],3),1) = [3] and [3]='й') or
            (left(right([w5],3),1) = [3] and [3]='й') or
            (left(right([w6],3),1) = [3] and [3]='й') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='й') or
            (left(right([w2],2),1) = [4] and [4]='й') or
            (left(right([w3],2),1) = [4] and [4]='й') or
            (left(right([w4],2),1) = [4] and [4]='й') or
            (left(right([w5],2),1) = [4] and [4]='й') or
            (left(right([w6],2),1) = [4] and [4]='й') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='й') or
            (left(right([w2],1),1) = [5] and [5]='й') or
            (left(right([w3],1),1) = [5] and [5]='й') or
            (left(right([w4],1),1) = [5] and [5]='й') or
            (left(right([w5],1),1) = [5] and [5]='й') or
            (left(right([w6],1),1) = [5] and [5]='й')
            ,bold(color(upper('й'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'й') and  contains([word],'й')) or
            (contains([w2],'й') and  contains([word],'й')) or
            (contains([w3],'й') and  contains([word],'й')) or
            (contains([w4],'й') and  contains([word],'й')) or
            (contains([w5],'й') and  contains([word],'й')) or
            (contains([w6],'й') and  contains([word],'й'))
            ,bold(color(upper('й'),'#ffa500')),
    /*red*/if(
            (contains([w1],'й') and  not contains([word],'й')) or
            (contains([w2],'й') and  not contains([word],'й')) or
            (contains([w3],'й') and  not contains([word],'й')) or
            (contains([w4],'й') and  not contains([word],'й')) or
            (contains([w5],'й') and  not contains([word],'й')) or
            (contains([w6],'й') and  not contains([word],'й'))
            ,bold(color(upper('й'),'#ff0000')),
        bold(color(upper('й'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='к') or
            (left(right([w2],5),1) = [1] and [1]='к') or
            (left(right([w3],5),1) = [1] and [1]='к') or
            (left(right([w4],5),1) = [1] and [1]='к') or
            (left(right([w5],5),1) = [1] and [1]='к') or
            (left(right([w6],5),1) = [1] and [1]='к') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='к') or
            (left(right([w2],4),1) = [2] and [2]='к') or
            (left(right([w3],4),1) = [2] and [2]='к') or
            (left(right([w4],4),1) = [2] and [2]='к') or
            (left(right([w5],4),1) = [2] and [2]='к') or
            (left(right([w6],4),1) = [2] and [2]='к') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='к') or
            (left(right([w2],3),1) = [3] and [3]='к') or
            (left(right([w3],3),1) = [3] and [3]='к') or
            (left(right([w4],3),1) = [3] and [3]='к') or
            (left(right([w5],3),1) = [3] and [3]='к') or
            (left(right([w6],3),1) = [3] and [3]='к') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='к') or
            (left(right([w2],2),1) = [4] and [4]='к') or
            (left(right([w3],2),1) = [4] and [4]='к') or
            (left(right([w4],2),1) = [4] and [4]='к') or
            (left(right([w5],2),1) = [4] and [4]='к') or
            (left(right([w6],2),1) = [4] and [4]='к') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='к') or
            (left(right([w2],1),1) = [5] and [5]='к') or
            (left(right([w3],1),1) = [5] and [5]='к') or
            (left(right([w4],1),1) = [5] and [5]='к') or
            (left(right([w5],1),1) = [5] and [5]='к') or
            (left(right([w6],1),1) = [5] and [5]='к')
            ,bold(color(upper('к'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'к') and  contains([word],'к')) or
            (contains([w2],'к') and  contains([word],'к')) or
            (contains([w3],'к') and  contains([word],'к')) or
            (contains([w4],'к') and  contains([word],'к')) or
            (contains([w5],'к') and  contains([word],'к')) or
            (contains([w6],'к') and  contains([word],'к'))
            ,bold(color(upper('к'),'#ffa500')),
    /*red*/if(
            (contains([w1],'к') and  not contains([word],'к')) or
            (contains([w2],'к') and  not contains([word],'к')) or
            (contains([w3],'к') and  not contains([word],'к')) or
            (contains([w4],'к') and  not contains([word],'к')) or
            (contains([w5],'к') and  not contains([word],'к')) or
            (contains([w6],'к') and  not contains([word],'к'))
            ,bold(color(upper('к'),'#ff0000')),
        bold(color(upper('к'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='л') or
            (left(right([w2],5),1) = [1] and [1]='л') or
            (left(right([w3],5),1) = [1] and [1]='л') or
            (left(right([w4],5),1) = [1] and [1]='л') or
            (left(right([w5],5),1) = [1] and [1]='л') or
            (left(right([w6],5),1) = [1] and [1]='л') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='л') or
            (left(right([w2],4),1) = [2] and [2]='л') or
            (left(right([w3],4),1) = [2] and [2]='л') or
            (left(right([w4],4),1) = [2] and [2]='л') or
            (left(right([w5],4),1) = [2] and [2]='л') or
            (left(right([w6],4),1) = [2] and [2]='л') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='л') or
            (left(right([w2],3),1) = [3] and [3]='л') or
            (left(right([w3],3),1) = [3] and [3]='л') or
            (left(right([w4],3),1) = [3] and [3]='л') or
            (left(right([w5],3),1) = [3] and [3]='л') or
            (left(right([w6],3),1) = [3] and [3]='л') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='л') or
            (left(right([w2],2),1) = [4] and [4]='л') or
            (left(right([w3],2),1) = [4] and [4]='л') or
            (left(right([w4],2),1) = [4] and [4]='л') or
            (left(right([w5],2),1) = [4] and [4]='л') or
            (left(right([w6],2),1) = [4] and [4]='л') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='л') or
            (left(right([w2],1),1) = [5] and [5]='л') or
            (left(right([w3],1),1) = [5] and [5]='л') or
            (left(right([w4],1),1) = [5] and [5]='л') or
            (left(right([w5],1),1) = [5] and [5]='л') or
            (left(right([w6],1),1) = [5] and [5]='л')
            ,bold(color(upper('л'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'л') and  contains([word],'л')) or
            (contains([w2],'л') and  contains([word],'л')) or
            (contains([w3],'л') and  contains([word],'л')) or
            (contains([w4],'л') and  contains([word],'л')) or
            (contains([w5],'л') and  contains([word],'л')) or
            (contains([w6],'л') and  contains([word],'л'))
            ,bold(color(upper('л'),'#ffa500')),
    /*red*/if(
            (contains([w1],'л') and  not contains([word],'л')) or
            (contains([w2],'л') and  not contains([word],'л')) or
            (contains([w3],'л') and  not contains([word],'л')) or
            (contains([w4],'л') and  not contains([word],'л')) or
            (contains([w5],'л') and  not contains([word],'л')) or
            (contains([w6],'л') and  not contains([word],'л'))
            ,bold(color(upper('л'),'#ff0000')),
        bold(color(upper('л'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='м') or
            (left(right([w2],5),1) = [1] and [1]='м') or
            (left(right([w3],5),1) = [1] and [1]='м') or
            (left(right([w4],5),1) = [1] and [1]='м') or
            (left(right([w5],5),1) = [1] and [1]='м') or
            (left(right([w6],5),1) = [1] and [1]='м') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='м') or
            (left(right([w2],4),1) = [2] and [2]='м') or
            (left(right([w3],4),1) = [2] and [2]='м') or
            (left(right([w4],4),1) = [2] and [2]='м') or
            (left(right([w5],4),1) = [2] and [2]='м') or
            (left(right([w6],4),1) = [2] and [2]='м') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='м') or
            (left(right([w2],3),1) = [3] and [3]='м') or
            (left(right([w3],3),1) = [3] and [3]='м') or
            (left(right([w4],3),1) = [3] and [3]='м') or
            (left(right([w5],3),1) = [3] and [3]='м') or
            (left(right([w6],3),1) = [3] and [3]='м') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='м') or
            (left(right([w2],2),1) = [4] and [4]='м') or
            (left(right([w3],2),1) = [4] and [4]='м') or
            (left(right([w4],2),1) = [4] and [4]='м') or
            (left(right([w5],2),1) = [4] and [4]='м') or
            (left(right([w6],2),1) = [4] and [4]='м') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='м') or
            (left(right([w2],1),1) = [5] and [5]='м') or
            (left(right([w3],1),1) = [5] and [5]='м') or
            (left(right([w4],1),1) = [5] and [5]='м') or
            (left(right([w5],1),1) = [5] and [5]='м') or
            (left(right([w6],1),1) = [5] and [5]='м')
            ,bold(color(upper('м'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'м') and  contains([word],'м')) or
            (contains([w2],'м') and  contains([word],'м')) or
            (contains([w3],'м') and  contains([word],'м')) or
            (contains([w4],'м') and  contains([word],'м')) or
            (contains([w5],'м') and  contains([word],'м')) or
            (contains([w6],'м') and  contains([word],'м'))
            ,bold(color(upper('м'),'#ffa500')),
    /*red*/if(
            (contains([w1],'м') and  not contains([word],'м')) or
            (contains([w2],'м') and  not contains([word],'м')) or
            (contains([w3],'м') and  not contains([word],'м')) or
            (contains([w4],'м') and  not contains([word],'м')) or
            (contains([w5],'м') and  not contains([word],'м')) or
            (contains([w6],'м') and  not contains([word],'м'))
            ,bold(color(upper('м'),'#ff0000')),
        bold(color(upper('м'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='н') or
            (left(right([w2],5),1) = [1] and [1]='н') or
            (left(right([w3],5),1) = [1] and [1]='н') or
            (left(right([w4],5),1) = [1] and [1]='н') or
            (left(right([w5],5),1) = [1] and [1]='н') or
            (left(right([w6],5),1) = [1] and [1]='н') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='н') or
            (left(right([w2],4),1) = [2] and [2]='н') or
            (left(right([w3],4),1) = [2] and [2]='н') or
            (left(right([w4],4),1) = [2] and [2]='н') or
            (left(right([w5],4),1) = [2] and [2]='н') or
            (left(right([w6],4),1) = [2] and [2]='н') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='н') or
            (left(right([w2],3),1) = [3] and [3]='н') or
            (left(right([w3],3),1) = [3] and [3]='н') or
            (left(right([w4],3),1) = [3] and [3]='н') or
            (left(right([w5],3),1) = [3] and [3]='н') or
            (left(right([w6],3),1) = [3] and [3]='н') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='н') or
            (left(right([w2],2),1) = [4] and [4]='н') or
            (left(right([w3],2),1) = [4] and [4]='н') or
            (left(right([w4],2),1) = [4] and [4]='н') or
            (left(right([w5],2),1) = [4] and [4]='н') or
            (left(right([w6],2),1) = [4] and [4]='н') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='н') or
            (left(right([w2],1),1) = [5] and [5]='н') or
            (left(right([w3],1),1) = [5] and [5]='н') or
            (left(right([w4],1),1) = [5] and [5]='н') or
            (left(right([w5],1),1) = [5] and [5]='н') or
            (left(right([w6],1),1) = [5] and [5]='н')
            ,bold(color(upper('н'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'н') and  contains([word],'н')) or
            (contains([w2],'н') and  contains([word],'н')) or
            (contains([w3],'н') and  contains([word],'н')) or
            (contains([w4],'н') and  contains([word],'н')) or
            (contains([w5],'н') and  contains([word],'н')) or
            (contains([w6],'н') and  contains([word],'н'))
            ,bold(color(upper('н'),'#ffa500')),
    /*red*/if(
            (contains([w1],'н') and  not contains([word],'н')) or
            (contains([w2],'н') and  not contains([word],'н')) or
            (contains([w3],'н') and  not contains([word],'н')) or
            (contains([w4],'н') and  not contains([word],'н')) or
            (contains([w5],'н') and  not contains([word],'н')) or
            (contains([w6],'н') and  not contains([word],'н'))
            ,bold(color(upper('н'),'#ff0000')),
        bold(color(upper('н'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='о') or
            (left(right([w2],5),1) = [1] and [1]='о') or
            (left(right([w3],5),1) = [1] and [1]='о') or
            (left(right([w4],5),1) = [1] and [1]='о') or
            (left(right([w5],5),1) = [1] and [1]='о') or
            (left(right([w6],5),1) = [1] and [1]='о') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='о') or
            (left(right([w2],4),1) = [2] and [2]='о') or
            (left(right([w3],4),1) = [2] and [2]='о') or
            (left(right([w4],4),1) = [2] and [2]='о') or
            (left(right([w5],4),1) = [2] and [2]='о') or
            (left(right([w6],4),1) = [2] and [2]='о') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='о') or
            (left(right([w2],3),1) = [3] and [3]='о') or
            (left(right([w3],3),1) = [3] and [3]='о') or
            (left(right([w4],3),1) = [3] and [3]='о') or
            (left(right([w5],3),1) = [3] and [3]='о') or
            (left(right([w6],3),1) = [3] and [3]='о') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='о') or
            (left(right([w2],2),1) = [4] and [4]='о') or
            (left(right([w3],2),1) = [4] and [4]='о') or
            (left(right([w4],2),1) = [4] and [4]='о') or
            (left(right([w5],2),1) = [4] and [4]='о') or
            (left(right([w6],2),1) = [4] and [4]='о') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='о') or
            (left(right([w2],1),1) = [5] and [5]='о') or
            (left(right([w3],1),1) = [5] and [5]='о') or
            (left(right([w4],1),1) = [5] and [5]='о') or
            (left(right([w5],1),1) = [5] and [5]='о') or
            (left(right([w6],1),1) = [5] and [5]='о')
            ,bold(color(upper('о'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'о') and  contains([word],'о')) or
            (contains([w2],'о') and  contains([word],'о')) or
            (contains([w3],'о') and  contains([word],'о')) or
            (contains([w4],'о') and  contains([word],'о')) or
            (contains([w5],'о') and  contains([word],'о')) or
            (contains([w6],'о') and  contains([word],'о'))
            ,bold(color(upper('о'),'#ffa500')),
    /*red*/if(
            (contains([w1],'о') and  not contains([word],'о')) or
            (contains([w2],'о') and  not contains([word],'о')) or
            (contains([w3],'о') and  not contains([word],'о')) or
            (contains([w4],'о') and  not contains([word],'о')) or
            (contains([w5],'о') and  not contains([word],'о')) or
            (contains([w6],'о') and  not contains([word],'о'))
            ,bold(color(upper('о'),'#ff0000')),
        bold(color(upper('о'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='п') or
            (left(right([w2],5),1) = [1] and [1]='п') or
            (left(right([w3],5),1) = [1] and [1]='п') or
            (left(right([w4],5),1) = [1] and [1]='п') or
            (left(right([w5],5),1) = [1] and [1]='п') or
            (left(right([w6],5),1) = [1] and [1]='п') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='п') or
            (left(right([w2],4),1) = [2] and [2]='п') or
            (left(right([w3],4),1) = [2] and [2]='п') or
            (left(right([w4],4),1) = [2] and [2]='п') or
            (left(right([w5],4),1) = [2] and [2]='п') or
            (left(right([w6],4),1) = [2] and [2]='п') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='п') or
            (left(right([w2],3),1) = [3] and [3]='п') or
            (left(right([w3],3),1) = [3] and [3]='п') or
            (left(right([w4],3),1) = [3] and [3]='п') or
            (left(right([w5],3),1) = [3] and [3]='п') or
            (left(right([w6],3),1) = [3] and [3]='п') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='п') or
            (left(right([w2],2),1) = [4] and [4]='п') or
            (left(right([w3],2),1) = [4] and [4]='п') or
            (left(right([w4],2),1) = [4] and [4]='п') or
            (left(right([w5],2),1) = [4] and [4]='п') or
            (left(right([w6],2),1) = [4] and [4]='п') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='п') or
            (left(right([w2],1),1) = [5] and [5]='п') or
            (left(right([w3],1),1) = [5] and [5]='п') or
            (left(right([w4],1),1) = [5] and [5]='п') or
            (left(right([w5],1),1) = [5] and [5]='п') or
            (left(right([w6],1),1) = [5] and [5]='п')
            ,bold(color(upper('п'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'п') and  contains([word],'п')) or
            (contains([w2],'п') and  contains([word],'п')) or
            (contains([w3],'п') and  contains([word],'п')) or
            (contains([w4],'п') and  contains([word],'п')) or
            (contains([w5],'п') and  contains([word],'п')) or
            (contains([w6],'п') and  contains([word],'п'))
            ,bold(color(upper('п'),'#ffa500')),
    /*red*/if(
            (contains([w1],'п') and  not contains([word],'п')) or
            (contains([w2],'п') and  not contains([word],'п')) or
            (contains([w3],'п') and  not contains([word],'п')) or
            (contains([w4],'п') and  not contains([word],'п')) or
            (contains([w5],'п') and  not contains([word],'п')) or
            (contains([w6],'п') and  not contains([word],'п'))
            ,bold(color(upper('п'),'#ff0000')),
        bold(color(upper('п'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='р') or
            (left(right([w2],5),1) = [1] and [1]='р') or
            (left(right([w3],5),1) = [1] and [1]='р') or
            (left(right([w4],5),1) = [1] and [1]='р') or
            (left(right([w5],5),1) = [1] and [1]='р') or
            (left(right([w6],5),1) = [1] and [1]='р') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='р') or
            (left(right([w2],4),1) = [2] and [2]='р') or
            (left(right([w3],4),1) = [2] and [2]='р') or
            (left(right([w4],4),1) = [2] and [2]='р') or
            (left(right([w5],4),1) = [2] and [2]='р') or
            (left(right([w6],4),1) = [2] and [2]='р') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='р') or
            (left(right([w2],3),1) = [3] and [3]='р') or
            (left(right([w3],3),1) = [3] and [3]='р') or
            (left(right([w4],3),1) = [3] and [3]='р') or
            (left(right([w5],3),1) = [3] and [3]='р') or
            (left(right([w6],3),1) = [3] and [3]='р') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='р') or
            (left(right([w2],2),1) = [4] and [4]='р') or
            (left(right([w3],2),1) = [4] and [4]='р') or
            (left(right([w4],2),1) = [4] and [4]='р') or
            (left(right([w5],2),1) = [4] and [4]='р') or
            (left(right([w6],2),1) = [4] and [4]='р') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='р') or
            (left(right([w2],1),1) = [5] and [5]='р') or
            (left(right([w3],1),1) = [5] and [5]='р') or
            (left(right([w4],1),1) = [5] and [5]='р') or
            (left(right([w5],1),1) = [5] and [5]='р') or
            (left(right([w6],1),1) = [5] and [5]='р')
            ,bold(color(upper('р'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'р') and  contains([word],'р')) or
            (contains([w2],'р') and  contains([word],'р')) or
            (contains([w3],'р') and  contains([word],'р')) or
            (contains([w4],'р') and  contains([word],'р')) or
            (contains([w5],'р') and  contains([word],'р')) or
            (contains([w6],'р') and  contains([word],'р'))
            ,bold(color(upper('р'),'#ffa500')),
    /*red*/if(
            (contains([w1],'р') and  not contains([word],'р')) or
            (contains([w2],'р') and  not contains([word],'р')) or
            (contains([w3],'р') and  not contains([word],'р')) or
            (contains([w4],'р') and  not contains([word],'р')) or
            (contains([w5],'р') and  not contains([word],'р')) or
            (contains([w6],'р') and  not contains([word],'р'))
            ,bold(color(upper('р'),'#ff0000')),
        bold(color(upper('р'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='с') or
            (left(right([w2],5),1) = [1] and [1]='с') or
            (left(right([w3],5),1) = [1] and [1]='с') or
            (left(right([w4],5),1) = [1] and [1]='с') or
            (left(right([w5],5),1) = [1] and [1]='с') or
            (left(right([w6],5),1) = [1] and [1]='с') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='с') or
            (left(right([w2],4),1) = [2] and [2]='с') or
            (left(right([w3],4),1) = [2] and [2]='с') or
            (left(right([w4],4),1) = [2] and [2]='с') or
            (left(right([w5],4),1) = [2] and [2]='с') or
            (left(right([w6],4),1) = [2] and [2]='с') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='с') or
            (left(right([w2],3),1) = [3] and [3]='с') or
            (left(right([w3],3),1) = [3] and [3]='с') or
            (left(right([w4],3),1) = [3] and [3]='с') or
            (left(right([w5],3),1) = [3] and [3]='с') or
            (left(right([w6],3),1) = [3] and [3]='с') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='с') or
            (left(right([w2],2),1) = [4] and [4]='с') or
            (left(right([w3],2),1) = [4] and [4]='с') or
            (left(right([w4],2),1) = [4] and [4]='с') or
            (left(right([w5],2),1) = [4] and [4]='с') or
            (left(right([w6],2),1) = [4] and [4]='с') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='с') or
            (left(right([w2],1),1) = [5] and [5]='с') or
            (left(right([w3],1),1) = [5] and [5]='с') or
            (left(right([w4],1),1) = [5] and [5]='с') or
            (left(right([w5],1),1) = [5] and [5]='с') or
            (left(right([w6],1),1) = [5] and [5]='с')
            ,bold(color(upper('с'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'с') and  contains([word],'с')) or
            (contains([w2],'с') and  contains([word],'с')) or
            (contains([w3],'с') and  contains([word],'с')) or
            (contains([w4],'с') and  contains([word],'с')) or
            (contains([w5],'с') and  contains([word],'с')) or
            (contains([w6],'с') and  contains([word],'с'))
            ,bold(color(upper('с'),'#ffa500')),
    /*red*/if(
            (contains([w1],'с') and  not contains([word],'с')) or
            (contains([w2],'с') and  not contains([word],'с')) or
            (contains([w3],'с') and  not contains([word],'с')) or
            (contains([w4],'с') and  not contains([word],'с')) or
            (contains([w5],'с') and  not contains([word],'с')) or
            (contains([w6],'с') and  not contains([word],'с'))
            ,bold(color(upper('с'),'#ff0000')),
        bold(color(upper('с'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='т') or
            (left(right([w2],5),1) = [1] and [1]='т') or
            (left(right([w3],5),1) = [1] and [1]='т') or
            (left(right([w4],5),1) = [1] and [1]='т') or
            (left(right([w5],5),1) = [1] and [1]='т') or
            (left(right([w6],5),1) = [1] and [1]='т') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='т') or
            (left(right([w2],4),1) = [2] and [2]='т') or
            (left(right([w3],4),1) = [2] and [2]='т') or
            (left(right([w4],4),1) = [2] and [2]='т') or
            (left(right([w5],4),1) = [2] and [2]='т') or
            (left(right([w6],4),1) = [2] and [2]='т') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='т') or
            (left(right([w2],3),1) = [3] and [3]='т') or
            (left(right([w3],3),1) = [3] and [3]='т') or
            (left(right([w4],3),1) = [3] and [3]='т') or
            (left(right([w5],3),1) = [3] and [3]='т') or
            (left(right([w6],3),1) = [3] and [3]='т') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='т') or
            (left(right([w2],2),1) = [4] and [4]='т') or
            (left(right([w3],2),1) = [4] and [4]='т') or
            (left(right([w4],2),1) = [4] and [4]='т') or
            (left(right([w5],2),1) = [4] and [4]='т') or
            (left(right([w6],2),1) = [4] and [4]='т') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='т') or
            (left(right([w2],1),1) = [5] and [5]='т') or
            (left(right([w3],1),1) = [5] and [5]='т') or
            (left(right([w4],1),1) = [5] and [5]='т') or
            (left(right([w5],1),1) = [5] and [5]='т') or
            (left(right([w6],1),1) = [5] and [5]='т')
            ,bold(color(upper('т'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'т') and  contains([word],'т')) or
            (contains([w2],'т') and  contains([word],'т')) or
            (contains([w3],'т') and  contains([word],'т')) or
            (contains([w4],'т') and  contains([word],'т')) or
            (contains([w5],'т') and  contains([word],'т')) or
            (contains([w6],'т') and  contains([word],'т'))
            ,bold(color(upper('т'),'#ffa500')),
    /*red*/if(
            (contains([w1],'т') and  not contains([word],'т')) or
            (contains([w2],'т') and  not contains([word],'т')) or
            (contains([w3],'т') and  not contains([word],'т')) or
            (contains([w4],'т') and  not contains([word],'т')) or
            (contains([w5],'т') and  not contains([word],'т')) or
            (contains([w6],'т') and  not contains([word],'т'))
            ,bold(color(upper('т'),'#ff0000')),
        bold(color(upper('т'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='у') or
            (left(right([w2],5),1) = [1] and [1]='у') or
            (left(right([w3],5),1) = [1] and [1]='у') or
            (left(right([w4],5),1) = [1] and [1]='у') or
            (left(right([w5],5),1) = [1] and [1]='у') or
            (left(right([w6],5),1) = [1] and [1]='у') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='у') or
            (left(right([w2],4),1) = [2] and [2]='у') or
            (left(right([w3],4),1) = [2] and [2]='у') or
            (left(right([w4],4),1) = [2] and [2]='у') or
            (left(right([w5],4),1) = [2] and [2]='у') or
            (left(right([w6],4),1) = [2] and [2]='у') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='у') or
            (left(right([w2],3),1) = [3] and [3]='у') or
            (left(right([w3],3),1) = [3] and [3]='у') or
            (left(right([w4],3),1) = [3] and [3]='у') or
            (left(right([w5],3),1) = [3] and [3]='у') or
            (left(right([w6],3),1) = [3] and [3]='у') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='у') or
            (left(right([w2],2),1) = [4] and [4]='у') or
            (left(right([w3],2),1) = [4] and [4]='у') or
            (left(right([w4],2),1) = [4] and [4]='у') or
            (left(right([w5],2),1) = [4] and [4]='у') or
            (left(right([w6],2),1) = [4] and [4]='у') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='у') or
            (left(right([w2],1),1) = [5] and [5]='у') or
            (left(right([w3],1),1) = [5] and [5]='у') or
            (left(right([w4],1),1) = [5] and [5]='у') or
            (left(right([w5],1),1) = [5] and [5]='у') or
            (left(right([w6],1),1) = [5] and [5]='у')
            ,bold(color(upper('у'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'у') and  contains([word],'у')) or
            (contains([w2],'у') and  contains([word],'у')) or
            (contains([w3],'у') and  contains([word],'у')) or
            (contains([w4],'у') and  contains([word],'у')) or
            (contains([w5],'у') and  contains([word],'у')) or
            (contains([w6],'у') and  contains([word],'у'))
            ,bold(color(upper('у'),'#ffa500')),
    /*red*/if(
            (contains([w1],'у') and  not contains([word],'у')) or
            (contains([w2],'у') and  not contains([word],'у')) or
            (contains([w3],'у') and  not contains([word],'у')) or
            (contains([w4],'у') and  not contains([word],'у')) or
            (contains([w5],'у') and  not contains([word],'у')) or
            (contains([w6],'у') and  not contains([word],'у'))
            ,bold(color(upper('у'),'#ff0000')),
        bold(color(upper('у'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='ф') or
            (left(right([w2],5),1) = [1] and [1]='ф') or
            (left(right([w3],5),1) = [1] and [1]='ф') or
            (left(right([w4],5),1) = [1] and [1]='ф') or
            (left(right([w5],5),1) = [1] and [1]='ф') or
            (left(right([w6],5),1) = [1] and [1]='ф') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='ф') or
            (left(right([w2],4),1) = [2] and [2]='ф') or
            (left(right([w3],4),1) = [2] and [2]='ф') or
            (left(right([w4],4),1) = [2] and [2]='ф') or
            (left(right([w5],4),1) = [2] and [2]='ф') or
            (left(right([w6],4),1) = [2] and [2]='ф') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='ф') or
            (left(right([w2],3),1) = [3] and [3]='ф') or
            (left(right([w3],3),1) = [3] and [3]='ф') or
            (left(right([w4],3),1) = [3] and [3]='ф') or
            (left(right([w5],3),1) = [3] and [3]='ф') or
            (left(right([w6],3),1) = [3] and [3]='ф') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='ф') or
            (left(right([w2],2),1) = [4] and [4]='ф') or
            (left(right([w3],2),1) = [4] and [4]='ф') or
            (left(right([w4],2),1) = [4] and [4]='ф') or
            (left(right([w5],2),1) = [4] and [4]='ф') or
            (left(right([w6],2),1) = [4] and [4]='ф') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='ф') or
            (left(right([w2],1),1) = [5] and [5]='ф') or
            (left(right([w3],1),1) = [5] and [5]='ф') or
            (left(right([w4],1),1) = [5] and [5]='ф') or
            (left(right([w5],1),1) = [5] and [5]='ф') or
            (left(right([w6],1),1) = [5] and [5]='ф')
            ,bold(color(upper('ф'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'ф') and  contains([word],'ф')) or
            (contains([w2],'ф') and  contains([word],'ф')) or
            (contains([w3],'ф') and  contains([word],'ф')) or
            (contains([w4],'ф') and  contains([word],'ф')) or
            (contains([w5],'ф') and  contains([word],'ф')) or
            (contains([w6],'ф') and  contains([word],'ф'))
            ,bold(color(upper('ф'),'#ffa500')),
    /*red*/if(
            (contains([w1],'ф') and  not contains([word],'ф')) or
            (contains([w2],'ф') and  not contains([word],'ф')) or
            (contains([w3],'ф') and  not contains([word],'ф')) or
            (contains([w4],'ф') and  not contains([word],'ф')) or
            (contains([w5],'ф') and  not contains([word],'ф')) or
            (contains([w6],'ф') and  not contains([word],'ф'))
            ,bold(color(upper('ф'),'#ff0000')),
        bold(color(upper('ф'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='х') or
            (left(right([w2],5),1) = [1] and [1]='х') or
            (left(right([w3],5),1) = [1] and [1]='х') or
            (left(right([w4],5),1) = [1] and [1]='х') or
            (left(right([w5],5),1) = [1] and [1]='х') or
            (left(right([w6],5),1) = [1] and [1]='х') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='х') or
            (left(right([w2],4),1) = [2] and [2]='х') or
            (left(right([w3],4),1) = [2] and [2]='х') or
            (left(right([w4],4),1) = [2] and [2]='х') or
            (left(right([w5],4),1) = [2] and [2]='х') or
            (left(right([w6],4),1) = [2] and [2]='х') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='х') or
            (left(right([w2],3),1) = [3] and [3]='х') or
            (left(right([w3],3),1) = [3] and [3]='х') or
            (left(right([w4],3),1) = [3] and [3]='х') or
            (left(right([w5],3),1) = [3] and [3]='х') or
            (left(right([w6],3),1) = [3] and [3]='х') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='х') or
            (left(right([w2],2),1) = [4] and [4]='х') or
            (left(right([w3],2),1) = [4] and [4]='х') or
            (left(right([w4],2),1) = [4] and [4]='х') or
            (left(right([w5],2),1) = [4] and [4]='х') or
            (left(right([w6],2),1) = [4] and [4]='х') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='х') or
            (left(right([w2],1),1) = [5] and [5]='х') or
            (left(right([w3],1),1) = [5] and [5]='х') or
            (left(right([w4],1),1) = [5] and [5]='х') or
            (left(right([w5],1),1) = [5] and [5]='х') or
            (left(right([w6],1),1) = [5] and [5]='х')
            ,bold(color(upper('х'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'х') and  contains([word],'х')) or
            (contains([w2],'х') and  contains([word],'х')) or
            (contains([w3],'х') and  contains([word],'х')) or
            (contains([w4],'х') and  contains([word],'х')) or
            (contains([w5],'х') and  contains([word],'х')) or
            (contains([w6],'х') and  contains([word],'х'))
            ,bold(color(upper('х'),'#ffa500')),
    /*red*/if(
            (contains([w1],'х') and  not contains([word],'х')) or
            (contains([w2],'х') and  not contains([word],'х')) or
            (contains([w3],'х') and  not contains([word],'х')) or
            (contains([w4],'х') and  not contains([word],'х')) or
            (contains([w5],'х') and  not contains([word],'х')) or
            (contains([w6],'х') and  not contains([word],'х'))
            ,bold(color(upper('х'),'#ff0000')),
        bold(color(upper('х'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='ц') or
            (left(right([w2],5),1) = [1] and [1]='ц') or
            (left(right([w3],5),1) = [1] and [1]='ц') or
            (left(right([w4],5),1) = [1] and [1]='ц') or
            (left(right([w5],5),1) = [1] and [1]='ц') or
            (left(right([w6],5),1) = [1] and [1]='ц') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='ц') or
            (left(right([w2],4),1) = [2] and [2]='ц') or
            (left(right([w3],4),1) = [2] and [2]='ц') or
            (left(right([w4],4),1) = [2] and [2]='ц') or
            (left(right([w5],4),1) = [2] and [2]='ц') or
            (left(right([w6],4),1) = [2] and [2]='ц') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='ц') or
            (left(right([w2],3),1) = [3] and [3]='ц') or
            (left(right([w3],3),1) = [3] and [3]='ц') or
            (left(right([w4],3),1) = [3] and [3]='ц') or
            (left(right([w5],3),1) = [3] and [3]='ц') or
            (left(right([w6],3),1) = [3] and [3]='ц') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='ц') or
            (left(right([w2],2),1) = [4] and [4]='ц') or
            (left(right([w3],2),1) = [4] and [4]='ц') or
            (left(right([w4],2),1) = [4] and [4]='ц') or
            (left(right([w5],2),1) = [4] and [4]='ц') or
            (left(right([w6],2),1) = [4] and [4]='ц') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='ц') or
            (left(right([w2],1),1) = [5] and [5]='ц') or
            (left(right([w3],1),1) = [5] and [5]='ц') or
            (left(right([w4],1),1) = [5] and [5]='ц') or
            (left(right([w5],1),1) = [5] and [5]='ц') or
            (left(right([w6],1),1) = [5] and [5]='ц')
            ,bold(color(upper('ц'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'ц') and  contains([word],'ц')) or
            (contains([w2],'ц') and  contains([word],'ц')) or
            (contains([w3],'ц') and  contains([word],'ц')) or
            (contains([w4],'ц') and  contains([word],'ц')) or
            (contains([w5],'ц') and  contains([word],'ц')) or
            (contains([w6],'ц') and  contains([word],'ц'))
            ,bold(color(upper('ц'),'#ffa500')),
    /*red*/if(
            (contains([w1],'ц') and  not contains([word],'ц')) or
            (contains([w2],'ц') and  not contains([word],'ц')) or
            (contains([w3],'ц') and  not contains([word],'ц')) or
            (contains([w4],'ц') and  not contains([word],'ц')) or
            (contains([w5],'ц') and  not contains([word],'ц')) or
            (contains([w6],'ц') and  not contains([word],'ц'))
            ,bold(color(upper('ц'),'#ff0000')),
        bold(color(upper('ц'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='ч') or
            (left(right([w2],5),1) = [1] and [1]='ч') or
            (left(right([w3],5),1) = [1] and [1]='ч') or
            (left(right([w4],5),1) = [1] and [1]='ч') or
            (left(right([w5],5),1) = [1] and [1]='ч') or
            (left(right([w6],5),1) = [1] and [1]='ч') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='ч') or
            (left(right([w2],4),1) = [2] and [2]='ч') or
            (left(right([w3],4),1) = [2] and [2]='ч') or
            (left(right([w4],4),1) = [2] and [2]='ч') or
            (left(right([w5],4),1) = [2] and [2]='ч') or
            (left(right([w6],4),1) = [2] and [2]='ч') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='ч') or
            (left(right([w2],3),1) = [3] and [3]='ч') or
            (left(right([w3],3),1) = [3] and [3]='ч') or
            (left(right([w4],3),1) = [3] and [3]='ч') or
            (left(right([w5],3),1) = [3] and [3]='ч') or
            (left(right([w6],3),1) = [3] and [3]='ч') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='ч') or
            (left(right([w2],2),1) = [4] and [4]='ч') or
            (left(right([w3],2),1) = [4] and [4]='ч') or
            (left(right([w4],2),1) = [4] and [4]='ч') or
            (left(right([w5],2),1) = [4] and [4]='ч') or
            (left(right([w6],2),1) = [4] and [4]='ч') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='ч') or
            (left(right([w2],1),1) = [5] and [5]='ч') or
            (left(right([w3],1),1) = [5] and [5]='ч') or
            (left(right([w4],1),1) = [5] and [5]='ч') or
            (left(right([w5],1),1) = [5] and [5]='ч') or
            (left(right([w6],1),1) = [5] and [5]='ч')
            ,bold(color(upper('ч'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'ч') and  contains([word],'ч')) or
            (contains([w2],'ч') and  contains([word],'ч')) or
            (contains([w3],'ч') and  contains([word],'ч')) or
            (contains([w4],'ч') and  contains([word],'ч')) or
            (contains([w5],'ч') and  contains([word],'ч')) or
            (contains([w6],'ч') and  contains([word],'ч'))
            ,bold(color(upper('ч'),'#ffa500')),
    /*red*/if(
            (contains([w1],'ч') and  not contains([word],'ч')) or
            (contains([w2],'ч') and  not contains([word],'ч')) or
            (contains([w3],'ч') and  not contains([word],'ч')) or
            (contains([w4],'ч') and  not contains([word],'ч')) or
            (contains([w5],'ч') and  not contains([word],'ч')) or
            (contains([w6],'ч') and  not contains([word],'ч'))
            ,bold(color(upper('ч'),'#ff0000')),
        bold(color(upper('ч'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='ш') or
            (left(right([w2],5),1) = [1] and [1]='ш') or
            (left(right([w3],5),1) = [1] and [1]='ш') or
            (left(right([w4],5),1) = [1] and [1]='ш') or
            (left(right([w5],5),1) = [1] and [1]='ш') or
            (left(right([w6],5),1) = [1] and [1]='ш') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='ш') or
            (left(right([w2],4),1) = [2] and [2]='ш') or
            (left(right([w3],4),1) = [2] and [2]='ш') or
            (left(right([w4],4),1) = [2] and [2]='ш') or
            (left(right([w5],4),1) = [2] and [2]='ш') or
            (left(right([w6],4),1) = [2] and [2]='ш') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='ш') or
            (left(right([w2],3),1) = [3] and [3]='ш') or
            (left(right([w3],3),1) = [3] and [3]='ш') or
            (left(right([w4],3),1) = [3] and [3]='ш') or
            (left(right([w5],3),1) = [3] and [3]='ш') or
            (left(right([w6],3),1) = [3] and [3]='ш') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='ш') or
            (left(right([w2],2),1) = [4] and [4]='ш') or
            (left(right([w3],2),1) = [4] and [4]='ш') or
            (left(right([w4],2),1) = [4] and [4]='ш') or
            (left(right([w5],2),1) = [4] and [4]='ш') or
            (left(right([w6],2),1) = [4] and [4]='ш') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='ш') or
            (left(right([w2],1),1) = [5] and [5]='ш') or
            (left(right([w3],1),1) = [5] and [5]='ш') or
            (left(right([w4],1),1) = [5] and [5]='ш') or
            (left(right([w5],1),1) = [5] and [5]='ш') or
            (left(right([w6],1),1) = [5] and [5]='ш')
            ,bold(color(upper('ш'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'ш') and  contains([word],'ш')) or
            (contains([w2],'ш') and  contains([word],'ш')) or
            (contains([w3],'ш') and  contains([word],'ш')) or
            (contains([w4],'ш') and  contains([word],'ш')) or
            (contains([w5],'ш') and  contains([word],'ш')) or
            (contains([w6],'ш') and  contains([word],'ш'))
            ,bold(color(upper('ш'),'#ffa500')),
    /*red*/if(
            (contains([w1],'ш') and  not contains([word],'ш')) or
            (contains([w2],'ш') and  not contains([word],'ш')) or
            (contains([w3],'ш') and  not contains([word],'ш')) or
            (contains([w4],'ш') and  not contains([word],'ш')) or
            (contains([w5],'ш') and  not contains([word],'ш')) or
            (contains([w6],'ш') and  not contains([word],'ш'))
            ,bold(color(upper('ш'),'#ff0000')),
        bold(color(upper('ш'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='щ') or
            (left(right([w2],5),1) = [1] and [1]='щ') or
            (left(right([w3],5),1) = [1] and [1]='щ') or
            (left(right([w4],5),1) = [1] and [1]='щ') or
            (left(right([w5],5),1) = [1] and [1]='щ') or
            (left(right([w6],5),1) = [1] and [1]='щ') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='щ') or
            (left(right([w2],4),1) = [2] and [2]='щ') or
            (left(right([w3],4),1) = [2] and [2]='щ') or
            (left(right([w4],4),1) = [2] and [2]='щ') or
            (left(right([w5],4),1) = [2] and [2]='щ') or
            (left(right([w6],4),1) = [2] and [2]='щ') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='щ') or
            (left(right([w2],3),1) = [3] and [3]='щ') or
            (left(right([w3],3),1) = [3] and [3]='щ') or
            (left(right([w4],3),1) = [3] and [3]='щ') or
            (left(right([w5],3),1) = [3] and [3]='щ') or
            (left(right([w6],3),1) = [3] and [3]='щ') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='щ') or
            (left(right([w2],2),1) = [4] and [4]='щ') or
            (left(right([w3],2),1) = [4] and [4]='щ') or
            (left(right([w4],2),1) = [4] and [4]='щ') or
            (left(right([w5],2),1) = [4] and [4]='щ') or
            (left(right([w6],2),1) = [4] and [4]='щ') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='щ') or
            (left(right([w2],1),1) = [5] and [5]='щ') or
            (left(right([w3],1),1) = [5] and [5]='щ') or
            (left(right([w4],1),1) = [5] and [5]='щ') or
            (left(right([w5],1),1) = [5] and [5]='щ') or
            (left(right([w6],1),1) = [5] and [5]='щ')
            ,bold(color(upper('щ'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'щ') and  contains([word],'щ')) or
            (contains([w2],'щ') and  contains([word],'щ')) or
            (contains([w3],'щ') and  contains([word],'щ')) or
            (contains([w4],'щ') and  contains([word],'щ')) or
            (contains([w5],'щ') and  contains([word],'щ')) or
            (contains([w6],'щ') and  contains([word],'щ'))
            ,bold(color(upper('щ'),'#ffa500')),
    /*red*/if(
            (contains([w1],'щ') and  not contains([word],'щ')) or
            (contains([w2],'щ') and  not contains([word],'щ')) or
            (contains([w3],'щ') and  not contains([word],'щ')) or
            (contains([w4],'щ') and  not contains([word],'щ')) or
            (contains([w5],'щ') and  not contains([word],'щ')) or
            (contains([w6],'щ') and  not contains([word],'щ'))
            ,bold(color(upper('щ'),'#ff0000')),
        bold(color(upper('щ'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='ъ') or
            (left(right([w2],5),1) = [1] and [1]='ъ') or
            (left(right([w3],5),1) = [1] and [1]='ъ') or
            (left(right([w4],5),1) = [1] and [1]='ъ') or
            (left(right([w5],5),1) = [1] and [1]='ъ') or
            (left(right([w6],5),1) = [1] and [1]='ъ') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='ъ') or
            (left(right([w2],4),1) = [2] and [2]='ъ') or
            (left(right([w3],4),1) = [2] and [2]='ъ') or
            (left(right([w4],4),1) = [2] and [2]='ъ') or
            (left(right([w5],4),1) = [2] and [2]='ъ') or
            (left(right([w6],4),1) = [2] and [2]='ъ') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='ъ') or
            (left(right([w2],3),1) = [3] and [3]='ъ') or
            (left(right([w3],3),1) = [3] and [3]='ъ') or
            (left(right([w4],3),1) = [3] and [3]='ъ') or
            (left(right([w5],3),1) = [3] and [3]='ъ') or
            (left(right([w6],3),1) = [3] and [3]='ъ') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='ъ') or
            (left(right([w2],2),1) = [4] and [4]='ъ') or
            (left(right([w3],2),1) = [4] and [4]='ъ') or
            (left(right([w4],2),1) = [4] and [4]='ъ') or
            (left(right([w5],2),1) = [4] and [4]='ъ') or
            (left(right([w6],2),1) = [4] and [4]='ъ') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='ъ') or
            (left(right([w2],1),1) = [5] and [5]='ъ') or
            (left(right([w3],1),1) = [5] and [5]='ъ') or
            (left(right([w4],1),1) = [5] and [5]='ъ') or
            (left(right([w5],1),1) = [5] and [5]='ъ') or
            (left(right([w6],1),1) = [5] and [5]='ъ')
            ,bold(color(upper('ъ'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'ъ') and  contains([word],'ъ')) or
            (contains([w2],'ъ') and  contains([word],'ъ')) or
            (contains([w3],'ъ') and  contains([word],'ъ')) or
            (contains([w4],'ъ') and  contains([word],'ъ')) or
            (contains([w5],'ъ') and  contains([word],'ъ')) or
            (contains([w6],'ъ') and  contains([word],'ъ'))
            ,bold(color(upper('ъ'),'#ffa500')),
    /*red*/if(
            (contains([w1],'ъ') and  not contains([word],'ъ')) or
            (contains([w2],'ъ') and  not contains([word],'ъ')) or
            (contains([w3],'ъ') and  not contains([word],'ъ')) or
            (contains([w4],'ъ') and  not contains([word],'ъ')) or
            (contains([w5],'ъ') and  not contains([word],'ъ')) or
            (contains([w6],'ъ') and  not contains([word],'ъ'))
            ,bold(color(upper('ъ'),'#ff0000')),
        bold(color(upper('ъ'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='ы') or
            (left(right([w2],5),1) = [1] and [1]='ы') or
            (left(right([w3],5),1) = [1] and [1]='ы') or
            (left(right([w4],5),1) = [1] and [1]='ы') or
            (left(right([w5],5),1) = [1] and [1]='ы') or
            (left(right([w6],5),1) = [1] and [1]='ы') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='ы') or
            (left(right([w2],4),1) = [2] and [2]='ы') or
            (left(right([w3],4),1) = [2] and [2]='ы') or
            (left(right([w4],4),1) = [2] and [2]='ы') or
            (left(right([w5],4),1) = [2] and [2]='ы') or
            (left(right([w6],4),1) = [2] and [2]='ы') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='ы') or
            (left(right([w2],3),1) = [3] and [3]='ы') or
            (left(right([w3],3),1) = [3] and [3]='ы') or
            (left(right([w4],3),1) = [3] and [3]='ы') or
            (left(right([w5],3),1) = [3] and [3]='ы') or
            (left(right([w6],3),1) = [3] and [3]='ы') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='ы') or
            (left(right([w2],2),1) = [4] and [4]='ы') or
            (left(right([w3],2),1) = [4] and [4]='ы') or
            (left(right([w4],2),1) = [4] and [4]='ы') or
            (left(right([w5],2),1) = [4] and [4]='ы') or
            (left(right([w6],2),1) = [4] and [4]='ы') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='ы') or
            (left(right([w2],1),1) = [5] and [5]='ы') or
            (left(right([w3],1),1) = [5] and [5]='ы') or
            (left(right([w4],1),1) = [5] and [5]='ы') or
            (left(right([w5],1),1) = [5] and [5]='ы') or
            (left(right([w6],1),1) = [5] and [5]='ы')
            ,bold(color(upper('ы'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'ы') and  contains([word],'ы')) or
            (contains([w2],'ы') and  contains([word],'ы')) or
            (contains([w3],'ы') and  contains([word],'ы')) or
            (contains([w4],'ы') and  contains([word],'ы')) or
            (contains([w5],'ы') and  contains([word],'ы')) or
            (contains([w6],'ы') and  contains([word],'ы'))
            ,bold(color(upper('ы'),'#ffa500')),
    /*red*/if(
            (contains([w1],'ы') and  not contains([word],'ы')) or
            (contains([w2],'ы') and  not contains([word],'ы')) or
            (contains([w3],'ы') and  not contains([word],'ы')) or
            (contains([w4],'ы') and  not contains([word],'ы')) or
            (contains([w5],'ы') and  not contains([word],'ы')) or
            (contains([w6],'ы') and  not contains([word],'ы'))
            ,bold(color(upper('ы'),'#ff0000')),
        bold(color(upper('ы'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='ь') or
            (left(right([w2],5),1) = [1] and [1]='ь') or
            (left(right([w3],5),1) = [1] and [1]='ь') or
            (left(right([w4],5),1) = [1] and [1]='ь') or
            (left(right([w5],5),1) = [1] and [1]='ь') or
            (left(right([w6],5),1) = [1] and [1]='ь') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='ь') or
            (left(right([w2],4),1) = [2] and [2]='ь') or
            (left(right([w3],4),1) = [2] and [2]='ь') or
            (left(right([w4],4),1) = [2] and [2]='ь') or
            (left(right([w5],4),1) = [2] and [2]='ь') or
            (left(right([w6],4),1) = [2] and [2]='ь') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='ь') or
            (left(right([w2],3),1) = [3] and [3]='ь') or
            (left(right([w3],3),1) = [3] and [3]='ь') or
            (left(right([w4],3),1) = [3] and [3]='ь') or
            (left(right([w5],3),1) = [3] and [3]='ь') or
            (left(right([w6],3),1) = [3] and [3]='ь') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='ь') or
            (left(right([w2],2),1) = [4] and [4]='ь') or
            (left(right([w3],2),1) = [4] and [4]='ь') or
            (left(right([w4],2),1) = [4] and [4]='ь') or
            (left(right([w5],2),1) = [4] and [4]='ь') or
            (left(right([w6],2),1) = [4] and [4]='ь') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='ь') or
            (left(right([w2],1),1) = [5] and [5]='ь') or
            (left(right([w3],1),1) = [5] and [5]='ь') or
            (left(right([w4],1),1) = [5] and [5]='ь') or
            (left(right([w5],1),1) = [5] and [5]='ь') or
            (left(right([w6],1),1) = [5] and [5]='ь')
            ,bold(color(upper('ь'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'ь') and  contains([word],'ь')) or
            (contains([w2],'ь') and  contains([word],'ь')) or
            (contains([w3],'ь') and  contains([word],'ь')) or
            (contains([w4],'ь') and  contains([word],'ь')) or
            (contains([w5],'ь') and  contains([word],'ь')) or
            (contains([w6],'ь') and  contains([word],'ь'))
            ,bold(color(upper('ь'),'#ffa500')),
    /*red*/if(
            (contains([w1],'ь') and  not contains([word],'ь')) or
            (contains([w2],'ь') and  not contains([word],'ь')) or
            (contains([w3],'ь') and  not contains([word],'ь')) or
            (contains([w4],'ь') and  not contains([word],'ь')) or
            (contains([w5],'ь') and  not contains([word],'ь')) or
            (contains([w6],'ь') and  not contains([word],'ь'))
            ,bold(color(upper('ь'),'#ff0000')),
        bold(color(upper('ь'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='э') or
            (left(right([w2],5),1) = [1] and [1]='э') or
            (left(right([w3],5),1) = [1] and [1]='э') or
            (left(right([w4],5),1) = [1] and [1]='э') or
            (left(right([w5],5),1) = [1] and [1]='э') or
            (left(right([w6],5),1) = [1] and [1]='э') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='э') or
            (left(right([w2],4),1) = [2] and [2]='э') or
            (left(right([w3],4),1) = [2] and [2]='э') or
            (left(right([w4],4),1) = [2] and [2]='э') or
            (left(right([w5],4),1) = [2] and [2]='э') or
            (left(right([w6],4),1) = [2] and [2]='э') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='э') or
            (left(right([w2],3),1) = [3] and [3]='э') or
            (left(right([w3],3),1) = [3] and [3]='э') or
            (left(right([w4],3),1) = [3] and [3]='э') or
            (left(right([w5],3),1) = [3] and [3]='э') or
            (left(right([w6],3),1) = [3] and [3]='э') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='э') or
            (left(right([w2],2),1) = [4] and [4]='э') or
            (left(right([w3],2),1) = [4] and [4]='э') or
            (left(right([w4],2),1) = [4] and [4]='э') or
            (left(right([w5],2),1) = [4] and [4]='э') or
            (left(right([w6],2),1) = [4] and [4]='э') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='э') or
            (left(right([w2],1),1) = [5] and [5]='э') or
            (left(right([w3],1),1) = [5] and [5]='э') or
            (left(right([w4],1),1) = [5] and [5]='э') or
            (left(right([w5],1),1) = [5] and [5]='э') or
            (left(right([w6],1),1) = [5] and [5]='э')
            ,bold(color(upper('э'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'э') and  contains([word],'э')) or
            (contains([w2],'э') and  contains([word],'э')) or
            (contains([w3],'э') and  contains([word],'э')) or
            (contains([w4],'э') and  contains([word],'э')) or
            (contains([w5],'э') and  contains([word],'э')) or
            (contains([w6],'э') and  contains([word],'э'))
            ,bold(color(upper('э'),'#ffa500')),
    /*red*/if(
            (contains([w1],'э') and  not contains([word],'э')) or
            (contains([w2],'э') and  not contains([word],'э')) or
            (contains([w3],'э') and  not contains([word],'э')) or
            (contains([w4],'э') and  not contains([word],'э')) or
            (contains([w5],'э') and  not contains([word],'э')) or
            (contains([w6],'э') and  not contains([word],'э'))
            ,bold(color(upper('э'),'#ff0000')),
        bold(color(upper('э'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='ю') or
            (left(right([w2],5),1) = [1] and [1]='ю') or
            (left(right([w3],5),1) = [1] and [1]='ю') or
            (left(right([w4],5),1) = [1] and [1]='ю') or
            (left(right([w5],5),1) = [1] and [1]='ю') or
            (left(right([w6],5),1) = [1] and [1]='ю') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='ю') or
            (left(right([w2],4),1) = [2] and [2]='ю') or
            (left(right([w3],4),1) = [2] and [2]='ю') or
            (left(right([w4],4),1) = [2] and [2]='ю') or
            (left(right([w5],4),1) = [2] and [2]='ю') or
            (left(right([w6],4),1) = [2] and [2]='ю') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='ю') or
            (left(right([w2],3),1) = [3] and [3]='ю') or
            (left(right([w3],3),1) = [3] and [3]='ю') or
            (left(right([w4],3),1) = [3] and [3]='ю') or
            (left(right([w5],3),1) = [3] and [3]='ю') or
            (left(right([w6],3),1) = [3] and [3]='ю') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='ю') or
            (left(right([w2],2),1) = [4] and [4]='ю') or
            (left(right([w3],2),1) = [4] and [4]='ю') or
            (left(right([w4],2),1) = [4] and [4]='ю') or
            (left(right([w5],2),1) = [4] and [4]='ю') or
            (left(right([w6],2),1) = [4] and [4]='ю') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='ю') or
            (left(right([w2],1),1) = [5] and [5]='ю') or
            (left(right([w3],1),1) = [5] and [5]='ю') or
            (left(right([w4],1),1) = [5] and [5]='ю') or
            (left(right([w5],1),1) = [5] and [5]='ю') or
            (left(right([w6],1),1) = [5] and [5]='ю')
            ,bold(color(upper('ю'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'ю') and  contains([word],'ю')) or
            (contains([w2],'ю') and  contains([word],'ю')) or
            (contains([w3],'ю') and  contains([word],'ю')) or
            (contains([w4],'ю') and  contains([word],'ю')) or
            (contains([w5],'ю') and  contains([word],'ю')) or
            (contains([w6],'ю') and  contains([word],'ю'))
            ,bold(color(upper('ю'),'#ffa500')),
    /*red*/if(
            (contains([w1],'ю') and  not contains([word],'ю')) or
            (contains([w2],'ю') and  not contains([word],'ю')) or
            (contains([w3],'ю') and  not contains([word],'ю')) or
            (contains([w4],'ю') and  not contains([word],'ю')) or
            (contains([w5],'ю') and  not contains([word],'ю')) or
            (contains([w6],'ю') and  not contains([word],'ю'))
            ,bold(color(upper('ю'),'#ff0000')),
        bold(color(upper('ю'),'#000000')))))
        , ' | ',if(   
    /*green*/ 
    /*first letter*/
            (left(right([w1],5),1) = [1] and [1]='я') or
            (left(right([w2],5),1) = [1] and [1]='я') or
            (left(right([w3],5),1) = [1] and [1]='я') or
            (left(right([w4],5),1) = [1] and [1]='я') or
            (left(right([w5],5),1) = [1] and [1]='я') or
            (left(right([w6],5),1) = [1] and [1]='я') or
    /*second letter*/
            (left(right([w1],4),1) = [2] and [2]='я') or
            (left(right([w2],4),1) = [2] and [2]='я') or
            (left(right([w3],4),1) = [2] and [2]='я') or
            (left(right([w4],4),1) = [2] and [2]='я') or
            (left(right([w5],4),1) = [2] and [2]='я') or
            (left(right([w6],4),1) = [2] and [2]='я') or
    /*third letter*/
            (left(right([w1],3),1) = [3] and [3]='я') or
            (left(right([w2],3),1) = [3] and [3]='я') or
            (left(right([w3],3),1) = [3] and [3]='я') or
            (left(right([w4],3),1) = [3] and [3]='я') or
            (left(right([w5],3),1) = [3] and [3]='я') or
            (left(right([w6],3),1) = [3] and [3]='я') or
    /*fourth letter*/
            (left(right([w1],2),1) = [4] and [4]='я') or
            (left(right([w2],2),1) = [4] and [4]='я') or
            (left(right([w3],2),1) = [4] and [4]='я') or
            (left(right([w4],2),1) = [4] and [4]='я') or
            (left(right([w5],2),1) = [4] and [4]='я') or
            (left(right([w6],2),1) = [4] and [4]='я') or
    /*fifth letter*/
            (left(right([w1],1),1) = [5] and [5]='я') or
            (left(right([w2],1),1) = [5] and [5]='я') or
            (left(right([w3],1),1) = [5] and [5]='я') or
            (left(right([w4],1),1) = [5] and [5]='я') or
            (left(right([w5],1),1) = [5] and [5]='я') or
            (left(right([w6],1),1) = [5] and [5]='я')
            ,bold(color(upper('я'),'#00ff00')),
    /*yellow*/if(
            (contains([w1],'я') and  contains([word],'я')) or
            (contains([w2],'я') and  contains([word],'я')) or
            (contains([w3],'я') and  contains([word],'я')) or
            (contains([w4],'я') and  contains([word],'я')) or
            (contains([w5],'я') and  contains([word],'я')) or
            (contains([w6],'я') and  contains([word],'я'))
            ,bold(color(upper('я'),'#ffa500')),
    /*red*/if(
            (contains([w1],'я') and  not contains([word],'я')) or
            (contains([w2],'я') and  not contains([word],'я')) or
            (contains([w3],'я') and  not contains([word],'я')) or
            (contains([w4],'я') and  not contains([word],'я')) or
            (contains([w5],'я') and  not contains([word],'я')) or
            (contains([w6],'я') and  not contains([word],'я'))
            ,bold(color(upper('я'),'#ff0000')),
        bold(color(upper('я'),'#000000')))))
        , ' | ')
```

</details>
