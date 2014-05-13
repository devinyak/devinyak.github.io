---
title: Зв'язок між віком провізорів та їх поглядами на фармопіку - непараметричне тестування відмінностей між групами
author: Девіняк Олег
tags: [R, основи статистики, р-величина, множинні порівняння, непараметричні методи, критерій Уілкоксона, критерій Краскела-Уолліса]
date: "2014-05-17"
layout: post
category: myblog
--- 

Фармацевтична опіка - ключова функція працівника аптеки. Узявши на себе відповідальність з надання фармацевтичної опіки, провізор стає важливою ланкою системи охорони здоров'я. Однак процедура надання фармацевтичної опіки досі не має чіткої регламентації та юридичного базису. У цьому напрямку ведуться наукові роботи, зокрема вивчається питання створення клініко-фармацевтичної служби в аптечних закладах (<a href="http://stat.org.ua/blog/stories/age-and-pharm-care/#boretska">Борецька О.Б.</a>). В контексті цього дослідження проведено анкетування працівників аптек стосовно їхньої думки щодо різних аспектів фармацевтичної опіки. Давайте розглянемо фрагмент аналізу результатів анкетування: дослідження зв'язку між віком провізорів та їхніми відповідями на ряд питань.

Вйо до діла: завантажимо дані.


```r
anketa = read.csv("http://stat.org.ua/data/pharmcare-survey.csv")
names(anketa)  #подивимось що там за стовпчики 
```

```
##  [1] "X"                                                
##  [2] "вік"                                              
##  [3] "як.часто.ви.проводите.моніторинг"                 
##  [4] "чи.конс..Ви.пацієнта.з.питань.правильного.заст.ЛЗ"
##  [5] "зберігання.ЛЗ"                                    
##  [6] "Інформація.рац..Заст.1"                           
##  [7] "Інформація.рац..Заст.2"                           
##  [8] "Інформація.рац..Заст.3"                           
##  [9] "Інформація.рац..Заст.4"                           
## [10] "Інформація.рац..Заст.5"                           
## [11] "Інформація.рац..Заст.6"
```


Отже, перший стовпчик - вік, а інші містять відповіді на запитання анкети. Питання "Звідки Ви отримуєте інформацію, щодо раціонального застосування лікарських засобів?" мало можливість множинного вибору відповідей, тому кожен з варіантів має свій стовпчик. Завдання дослідження зв'язку між віком провізорів та відповідями на питання зводиться до порівняння вікових груп, утворених різними відповідями. Щоб вибрати між параметричними та непараметричними методами дослідження відмінностей між групами, слід перевірити, чи розподіл віку подібний до нормального розподілу. Для цього подивимось на квантиль-квантильний нормальний графік (*інспекція графіку*):


```r
qqnorm(anketa$вік)
```

![квантиль-квантильний нормальний графік](http://stat.org.ua/figures/age-pharmcare-qqnorm.png) 


При нормальному розподілі точки на графіку лежали би на одній прямій. У даному випадку розподіл сильно відрізняється від нормального. Тому слід використовувати непараметричні методи: критерій Уілкоксона-Манна-Уітні для порівняння двох груп та критерій Краскела-Уолліса для порівняння багатьох груп.

Почнемо по порядку, із зв'язку між віком та відповіддю на питання "Як часто Ви проводите моніторинг (перевірку) призначення лікарських засобів на предмет їх взаємодії?". Подивимось спочатку на кількість варіантів та їх розподіл.


```r
table(anketa$як.часто.ви.проводите.моніторинг)
```

```
## 
##                            завжди лише окремим категоріям пацієнтів 
##                               143                               128 
##                            ніколи                             рідко 
##                                21                                57 
##      тільки за проханням пацієнта 
##                               112
```


Більшість працівників стверджують, що завжди проводять моніторинг. Всього є 5 різних варіантів відповіді, тому використовуєм критерій Краскела-Уолліса:


```r
kruskal.test(anketa$вік ~ anketa$як.часто.ви.проводите.моніторинг)
```

```
## 
## 	Kruskal-Wallis rank sum test
## 
## data:  anketa$вік by anketa$як.часто.ви.проводите.моніторинг
## Kruskal-Wallis chi-squared = 4.037, df = 4, p-value = 0.401
```


Зв'язок не є статистично значимим. Враховуючи, що розмір вибірки досить великий:


```r
nrow(anketa)
```

```
## [1] 468
```


то можна казати, що зв'язку немає як такого. Частота проведення моніторингу взаємодій лікарських засобів не залежить від віку працівника.

Ідем далі: питання "Чи консультуєте Ви пацієнта з питань правильного застосування  лікарських засобів?"


```r
table(anketa$чи.конс..Ви.пацієнта.з.питань.правильного.заст.ЛЗ)
```

```
## 
##                        дуже рідко                  практично ніколи 
##                                16                                 5 
##                       так, завжди      тільки за проханням пацієнта 
##                               290                               109 
## тільки окремі категорії пацієнтів 
##                                45
```

5 варіантів, критерій Краскела-Уолліса:


```r
kruskal.test(anketa$вік ~ anketa$чи.конс..Ви.пацієнта.з.питань.правильного.заст.ЛЗ)
```

```
## 
## 	Kruskal-Wallis rank sum test
## 
## data:  anketa$вік by anketa$чи.конс..Ви.пацієнта.з.питань.правильного.заст.ЛЗ
## Kruskal-Wallis chi-squared = 8.795, df = 4, p-value = 0.06643
```


\\(p\approx0.066\\), тому також кажемо про відсутність статистичної значимості. Однак \\(р\\) не таке вже й велике, отже підстав щоб заперечувати зв'язок небагато. Залишаємось із висновком, що гіпотеза про зв'язок між віком та цим питанням у даному дослідженні не підтверджена, але й не спростована.

Наступне питання звучало так: "Чи часто звертаєте увагу пацієнтів на правила зберігання лікарських засобів?"


```r
table(anketa$зберігання.ЛЗ)
```

```
## 
##                             дуже рідко 
##                                     26 
##                                 завжди 
##                                    205 
##                       практично ніколи 
##                                     15 
##           тільки за проханням пацієнта 
##                                     57 
## тільки окремі групи лікарських засобів 
##                                    163
```

5 варіантів, критерій Краскела-Уолліса:


```r
kruskal.test(anketa$вік ~ anketa$зберігання.ЛЗ)
```

```
## 
## 	Kruskal-Wallis rank sum test
## 
## data:  anketa$вік by anketa$зберігання.ЛЗ
## Kruskal-Wallis chi-squared = 8.2, df = 4, p-value = 0.08451
```


Аналогічно, статистична значимість відсутня.

Ну і залишився блок стовпчиків стосовно питання про джерела інформації. Тут кожен стовпчик може мати два значення: формулювання вибраного варіанту, або пустий рядок, якщо цей варіант не було вибрано. Ну ось наприклад:


```r
table(anketa$Інформація.рац..Заст.1)
```

```
## 
##               із довідників 
##           173           295
```


Над 173 пусто - це означає, що 173 респонденти цей варіант **не** вибрали. Отже, для довідників:


```r
wilcox.test(anketa$вік ~ anketa$Інформація.рац..Заст.1)
```

```
## 
## 	Wilcoxon rank sum test with continuity correction
## 
## data:  anketa$вік by anketa$Інформація.рац..Заст.1
## W = 25426, p-value = 0.6283
## alternative hypothesis: true location shift is not equal to 0
```


p-величина велика, люди різних вікових категорій користуються довідниками з однаковою частотою.

Наступне:


```r
table(anketa$Інформація.рац..Заст.2)
```

```
## 
##                       користуюсь Інтернетом 
##                   287                   181
```

```r
wilcox.test(anketa$вік ~ anketa$Інформація.рац..Заст.2)
```

```
## 
## 	Wilcoxon rank sum test with continuity correction
## 
## data:  anketa$вік by anketa$Інформація.рац..Заст.2
## W = 30515, p-value = 0.0002352
## alternative hypothesis: true location shift is not equal to 0
```


\\(р=0.000235\\) - отже, користування інтернетом залежить від віку працівника. Подивимось на цю залежність - виведемо графік "ящик з вусами":


```r
boxplot(anketa$вік ~ anketa$Інформація.рац..Заст.2)
```

![Зв'язок між віком і користування інтернетом для інформації про ЛЗ](http://stat.org.ua/figures/age-pharmcare-internet.png) 

Помітно, що інтернетом користуються більше молодші працівники. Що ж, нічого дивного в цьому немає. Котимось до наступного цікавого варіанту: хто читає огляди клінічних досліджень:


```r
table(anketa$Інформація.рац..Заст.3)
```

```
## 
##                                                                   
##                                                               375 
## на основі результатів клінічних досліджень,надрукованих в оглядах 
##                                                                93
```

```r
wilcox.test(anketa$вік ~ anketa$Інформація.рац..Заст.3)
```

```
## 
## 	Wilcoxon rank sum test with continuity correction
## 
## data:  anketa$вік by anketa$Інформація.рац..Заст.3
## W = 14065, p-value = 0.007049
## alternative hypothesis: true location shift is not equal to 0
```


Ви може думаєте, що р<0.05, а значить - статистична значимість. Не все так просто. Якщо Ви так подумали - почитайте про [множинні порівняння](http://stat.org.ua/blog/donewrong/try-try-again/). Ми тут проводили 3 критерії Краскела-Уолліса, це третій критерій Уілкоксона-Манна-Уітні, і ще збираємось зробити три Уілкоксони. Разом 9. Якщо брати найпростіший метод для роботи з множинними порівняннями - корекцію Бонферроні, то наш рівень \\(\alpha\\), з яким треба порівнювати р-величину, не 0.05, а \\(0.05/9=0.00556\\). Получається трохи більше. Але якщо застосувати хитріший метод - [контроль частки хибних відкриттів](http://stat.org.ua/blog/donewrong/the-controlling-of-false-discovery-rate/) - то ми впишемось (бо ми вже знайшли один статистично значимий результат, і це буде другий) і зможем стверджувати статистичну значимість. З іншого боку, наша вибірка велика, тому використання критерія Стьюдента - не такий вже і гріх (нас милує теорема про центральну границю):


```r
t.test(anketa$вік ~ anketa$Інформація.рац..Заст.3)
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  anketa$вік by anketa$Інформація.рац..Заст.3
## t = -2.859, df = 130.9, p-value = 0.004947
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -5.3318 -0.9708
## sample estimates:
##                                                                  mean in group  
##                                                                           28.90 
## mean in group на основі результатів клінічних досліджень,надрукованих в оглядах 
##                                                                           32.05
```


І маємо таке р, яке виконує вимоги корекції Бонферроні. От з таким важким виправданням говоримо, що зв'язок статистично значимий. Дивимось на розподіл:



```r
boxplot(anketa$вік ~ anketa$Інформація.рац..Заст.3)
```

![вік та огляди клінічних досліджень](http://stat.otg.ua/figures/age-pharmcare-reviews.png) 


Старші люди ніби-то частіше читають огляди клінічних досліджень (це другий ящик, він не підписаний бо підпис дуже великий). Ви в це вірите? Я ні. Просто такі респонденти більше хочуть здаватись розумними.

ОК, їдем далі:


```r
table(anketa$Інформація.рац..Заст.4)
```

```
## 
##                        від представників фірм 
##                    250                    218
```

```r
wilcox.test(anketa$вік ~ anketa$Інформація.рац..Заст.4)
```

```
## 
## 	Wilcoxon rank sum test with continuity correction
## 
## data:  anketa$вік by anketa$Інформація.рац..Заст.4
## W = 24212, p-value = 0.09716
## alternative hypothesis: true location shift is not equal to 0
```


Медичні-фармацевтичні представники. Їх слухає біля половини провізорів. Зв'язок із віком не простежується.

А тепер щодо варіанту "проходжу навчання щодо заданої проблеми"


```r
table(anketa$Інформація.рац..Заст.5)
```

```
## 
##                                         
##                                     397 
## проходжу навчання щодо заданої проблеми 
##                                      71
```

```r
wilcox.test(anketa$вік ~ anketa$Інформація.рац..Заст.5)
```

```
## 
## 	Wilcoxon rank sum test with continuity correction
## 
## data:  anketa$вік by anketa$Інформація.рац..Заст.5
## W = 11664, p-value = 0.04517
## alternative hypothesis: true location shift is not equal to 0
```


Ну що ж, навчання проходять небагато провізорів - принаймні чесно. Зв'язок не є статистично значимим (пам'ятаєте про множинні порівняння?).

І останній варіант - "покладаюсь на власний досвід"


```r
table(anketa$Інформація.рац..Заст.6)
```

```
## 
##                              покладаюсь на власний досвід 
##                          398                           70
```

```r
wilcox.test(anketa$вік ~ anketa$Інформація.рац..Заст.6)
```

```
## 
## 	Wilcoxon rank sum test with continuity correction
## 
## data:  anketa$вік by anketa$Інформація.рац..Заст.6
## W = 9911, p-value = 0.000207
## alternative hypothesis: true location shift is not equal to 0
```


Таких людей небагато, але подивіться, яка хороша статистична значимість. Подивимось:


```r
boxplot(anketa$вік ~ anketa$Інформація.рац..Заст.6)
```

![вік - досвід](http://stat.org.ua/figures/age-pharmcare-experience.png) 


Старші люди частіше покладаються на власний досвід. Ну, це справделиво - в них того досвіду більше.

На такій оптимістичній ноті дозвольте відкланятись. Радий буду коментарям.

___

<div class="nohover">
<a name="boretska", id="anchor">Організаційно-методичні засади створення та діяльності клініко-фармацевтичної служби в аптечних закладах України : автореф. дис. на здобуття наук. ступеня канд. фармац. наук : [спец.] 15.00.01 “Технологія ліків, організація фармацевтичної справи та судова фармація” / О. Б. Борецька ; Львів. нац. мед. ун-т ім. Данила Галицького. – Львів, 2013. – 24 с.</a>
</div>
