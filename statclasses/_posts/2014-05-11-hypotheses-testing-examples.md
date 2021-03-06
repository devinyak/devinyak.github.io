---
title: "Перевірка статистичних гіпотез - приклади"
author: Девіняк Олег
tags: [R, р-величина, основи статистики, перевірка гіпотез]
date: "2014-05-08"
layout: post
category: statclasses
--- 

##ПРИКЛАД 1.
Відомо, що бензодіазепінові транквілізатори несприятливо впливають на м’язовий тонус. Тому пошук нових транквілізаторів із зменшенням цієї побічної дії є перспективним. В експерименті досліджувався вплив новосинтезованого спіроциклічного похідного індолу на м’язовий тонус мишей  у порівнянні з інтактною контрольною групою та препаратом порівняння діазепамом за допомогою тесту стрижня, що обертається <a href="#shtrygol">\[1\]</a>. Результати експерименту (що тривав 1 хв для кожної миші), переведені у більш зручний для аналізу формат (порівняно з оригіналом; дана таблиця – таблиця спряженості), наступна:

<table>
<tr><td></td><td>контроль</td><td>діазепам(10мг/кг)</td><td>сполука І (0.5 мг/кг)</td><td>сполука І (5 мг/кг)</td></tr>
<tr><td>Упали</td><td>4</td><td>8</td><td>4</td><td>4</td></tr>
<tr><td>Не упали</td><td>6</td><td>0</td><td>4</td><td>7</td></tr>
</table>

Так як об’єктом інтересу є сполука І, можна сформулювати наступні нульові гіпотези:

1. Поведінка мишей після прийому досліджуваної сполуки не відрізняються від поведінки мишей у контрольній групі. Якщо ця гіпотеза буде відхилена, то ми зможемо стверджувати про негативний вплив досліджуваної сполуки на м’язовий тонус. Якщо ця гіпотеза не буде відхилена, то підстави для ствердження відсутності впливу на м’язовий тонус все одно немає. 
2. Поведінка мишей після прийому досліджуваної сполуки не відрізняються від поведінки після прийому препарату порівняння (діазепаму). Якщо ця гіпотеза буде відхилена, то ми зможемо стверджувати про відмінність між діазепамом та досліджуваною сполукою у впливі на м’язовий тонус, а точніше – про слабший вплив досліджуваної сполуки порівняно з діазепамом.

Оскільки у таблиці трапляються числа < 5 (і навіть нуль) варто використовувати точний критерій Фішера. 

Гіпотеза 1:
Введемо таблицю спряженості по рядкам за допомогою функції `rbind()`, аналізуємо стовпчики 1, 3 і 4.

```r
data <- rbind(c(4, 4, 4), c(6, 4, 7))
fisher.test(data)  #виконаємо точний тест Фішера
```

```r
## 
## 	Fisher's Exact Test for Count Data
## 
## data:  data
## p-value = 0.8953
## alternative hypothesis: two.sided
```

Отримана p-величина > 0.05, тобто при рівні значимості `α=0.05` ми не можемо відхилити нульову гіпотезу. Таким чином залишилась  невизначеність – чи впливає досліджувана сполука на м’язовий тонус, чи ні.

Гіпотеза 2:
Замінюємо стовпчик результатів інтактної групи на стовпчик діазепаму.

```r
data[, 1] <- c(8, 0)
fisher.test(data)
```

```r
## 
## 	Fisher's Exact Test for Count Data
## 
## data:  data
## p-value = 0.01592
## alternative hypothesis: two.sided
```

Отримана p-величина <0.05, тобто при рівні значимості `α=0.05` нульова гіпотеза відкидається. Отже статистично значимо кількість упавших мишей залежить від природи (чи дози) прийнятого препарату. З огляду на результати, можна стверджувати, що вплив досліджуваного препарату на м’язовий тонус є меншим.

Висновок: у результаті експерименту зв’язок між прийомом препарату у досліджуваних дозах і негативним впливом на м’язовий тонус не виявлено. Також вплив на м’язовий тонус досліджуваного препарату статистично значимо відрізняється від ефекту, спричиненого діазепамом.

##ПРИКЛАД 2. 
Повернемось до *Student’s Sleep Data* <a href="#student">\[2\]</a>. Встановимо тепер, чи статистично значимо відрізняють результати випробувань двох снодійних препаратів.
Для цього нам потрібно використати критерій зсуву. Але для того, щоб вибрати між параметричним та непараметричним критерієм потрібно провести попередню перевірку нормальності розподілу. Варто пам’ятати, що при малих об’ємах вибірок будь-який критерій має досить низьку потужність, і може бути не здатен виявити реально існуючі відмінності (чи зв’язок). З іншого боку, вимога щодо нормальності розподілу даних насправді є вимогою щодо нормальності відхилень від центрів вибірок. Ці вимоги є тотожними, оскільки дії додавання/віднімання, множення/ділення не змінюють розподілу випадкової величини. Тому замість того, щоб перевіряти нормальність розподілу двох вибірок по-окремо, раціонально буде об’єднати їх відхилення від центрів і перевірити нормальність відхилень. Відхилення для однієї групи можна обчислити, віднявши від кожного значення середнє арифметичне групи. Для цього використаємо функцію масштабування `scale()`, в яку передаємо значення досліджуваної групи, логічне `ТАК` - щоб здійснювалось віднімання середнього арифметичного (центрування), і наступне логічне `НІ` - щоб залишити масштаб вибірки без змін. Отож:

```r
data(sleep)
fix(sleep)
dev1 <- scale(sleep$extra[sleep$group == 1], TRUE, FALSE)  #відхилення для першої групи 
dev2 <- scale(sleep$extra[sleep$group == 2], TRUE, FALSE)  #відхилення для другої групи 
shapiro.test(c(dev1, dev2))
```

```r
## 
## 	Shapiro-Wilk normality test
## 
## data:  c(dev1, dev2)
## W = 0.9195, p-value = 0.09687
```

Оскільки p-величина > 0.05, можемо застосовувати *двовибірковий парний* (адже вибірки залежні) критерій Стьюдента.

```r
t.test(sleep$extra ~ sleep$group, paired = TRUE)
```

```r
## 
## 	Paired t-test
## 
## data:  sleep$extra by sleep$group
## t = -4.062, df = 9, p-value = 0.002833
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -2.4599 -0.7001
## sample estimates:
## mean of the differences 
##                   -1.58
```

Бачимо, що p-величина <0.05, отже різниця у ефективності препаратів статистично значима при `α=0.05`.
А що було б якби експеримент з препаратами проводили на різних пацієнтах (незалежні вибірки)?

```r
t.test(sleep$extra ~ sleep$group)
```

```r
## 
## 	Welch Two Sample t-test
## 
## data:  sleep$extra by sleep$group
## t = -1.861, df = 17.78, p-value = 0.07939
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -3.3655  0.2055
## sample estimates:
## mean in group 1 mean in group 2 
##            0.75            2.33
```

p-величина > 0.05! Таким чином різниця між препаратами у такому досліді уже не є статистично значимою.

І взагалі, 

```r
t.test(sleep$extra[sleep$group == 1])
```

```r
## 
## 	One Sample t-test
## 
## data:  sleep$extra[sleep$group == 1]
## t = 1.326, df = 9, p-value = 0.2176
## alternative hypothesis: true mean is not equal to 0
## 95 percent confidence interval:
##  -0.5298  2.0298
## sample estimates:
## mean of x 
##      0.75
```

 \- збільшення тривалості сну під дією препарату під номером 1 не відрізняється (статистично значимо) від нуля.

##ПРИКЛАД 3. 
Визначимо чи відноситься лікарський препарат до препаратів сезонного попиту. Приймемо у якості сезонів часові інтервали "зима", "весна", "літо", "осінь". Проаналізуємо дані щодо кількості проданих упаковок вентоліну та сальбутамолу напротязі трьох років у деякій аптеці. Теоретичне підгрунтя: препарат віднесемо до препаратів сезонного попиту, якщо розподіл кількості проданих упаковок по сезонах статистично значимо (приймемо `α=0.05`) відрізняється від рівномірного розподілу. Отож слід використати один із критеріїв узгодженості для номінальних величин.
Введемо дані: 

```r
ventolin <- c(185, 155, 142, 168)
```

Оскільки усі значення > 5, використаємо критерій хі квадрат:

```r
chisq.test(ventolin)
```

```r
## 
## 	Chi-squared test for given probabilities
## 
## data:  ventolin
## X-squared = 6.234, df = 3, p-value = 0.1008
```

p>0.05, отже недостатньо підстав для віднесення цього препарату до сезонних.

```r
milistan <- c(392, 314, 207, 317)
chisq.test(milistan)
```

```r
## 
## 	Chi-squared test for given probabilities
## 
## data:  milistan
## X-squared = 56.5, df = 3, p-value = 3.29e-12
```

Вигляд p-величини `"3.29е-12"` є одною з формою запису чисел, `е` у цій формі позначає експоненту, тобто саме число рівне \\(3.29*10^{-12}\\), що набагато менше ніж 0.05. Тому даний препарат є препаратом сезонного попиту.

##ПРИКЛАД 4. 
Необхідно порівняти стабільність продажів препаратів А і Б. Для цього було встановлено кількості проданих упаковок препаратів у деякій аптеці  напротязі 12 тижнів.

<table>
<tr><td></td><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td><td>6</td><td>7</td><td>8</td><td>9</td><td>10</td><td>11</td><td>12</td></tr>
<tr><td>A</td><td>0</td><td>2</td><td>3</td><td>0</td><td>3</td><td>4</td><td>4</td><td>3</td><td>0</td><td>2</td><td>4</td><td>3</td></tr>
<tr><td>Б</td><td>9</td><td>10</td><td>15</td><td>11</td><td>7</td><td>5</td><td>5</td><td>4</td><td>7</td><td>9</td><td>18</td><td>13</td></tr>
</table>

Оскільки кількість упаковок є дискретною величиною, а також у одній з вибірок (препарат А) присутні граничні значення – нулі (продати меншу кількість препарату не можливо. У той же час, нормальний розподіл передбачає відсутність границь), слід надати перевагу непараметричному методам порівняння масштабів. Оскільки медіани продажів відрізняються (можете це перевірити), то критерій Ансарі-Бредлі застосовувати не варто. Застосуємо одну розумну статистичну техніку: ресемплінг (*resampling*). Для цього побудуємо графік "ящик з вусами" для цих препаратів, і протиставимо йому вісім аналогічних графіків, отриманих з-під нульової гіпотези (про випадковість різниці у масштабах).

```r
a <- c(0, 2, 3, 0, 3, 4, 4, 3, 0, 2, 4, 3)  #продажі препарату А
b <- c(9, 10, 15, 11, 7, 5, 5, 4, 7, 9, 18, 13)  #продажі препарату Б
```

Для того, щоб вивести дев'ять графіків разом, використаємо графічний параметр `mfrow` – в даному випадку задаємо розташування у три рядки і три стовпці

```r
par(mfrow = c(3, 3))  #
boxplot(a, b)
```

![plot of chunk unnamed-chunk-10](http://stat.org.ua/figures/hypotheses-ex1.png) 

Отриманий графік не закриваємо, продовжуємо ввід команд. Ініціалізуємо генератор випадкових чисел заданим ключем – це робиться для того, щоб отримати однакові результати при усіх виконаннях наступного коду:

```r
set.seed(29)  
```

Створювати вибірки, що слідують із нульової гіпотези будемо наступним чином: об’єднуємо всі числа разом, переставляємо їх місцями випадковим чином (випадкові індекси створюємо функцією `sample()`), і тоді ділимо отриману сукупність навпіл. Щоб поділити навпіл, додатково створюємо послідовність із половинної кількості одиничок і половинної кількості двійок. У функцію `boxplot` передаємо формулу "випадкова перестановка залежить від  послідовності одиничок і двійок". Повторяємо усі процедури тричі (щоб вивести три графіки з-під нульової гіпотези) – за допомогою функції (оператора циклу) `for()`. Функцію `for()` найчастіше будемо використовувати у такому написанні: `for(i in 1:number)`, де `number` – необхідна кількість повторень (дослівно: "для і від одиниці до вказаного числа, виконай:"). Таким чином `R` записує в змінну і одиницю, і виконує наступну після `for()` команду. Тоді записує в і двійку, і знову виконує наступну після `for()` команду. Тоді – трійку, і так поки не досягне вказаного нами числа `(number)`.

```r
for (i in 1:3) boxplot(sample(c(a, b))~ c(rep(1, length(a)), 
    rep(2, length(b))))
```

![plot of chunk unnamed-chunk-12](http://stat.org.ua/figures/hypotheses-ex2.png)

Щоб порівняти стабільності продажів, на графіках ми порівнюємо різниці в міжквартильній відстані. Ця різниця для першого графіку (на ньому продажі препаратів А і Б) майже не відрізняється (і навіть менша) від різниці на п'ятому та дев'ятому графіках. Тому робимо висновок, що досліджувані лікарські препарати не відрізняються стабільністю продажів. Крім того, помітно що різниця між медіанами на першому графіку достатньо сильно відрізняється від різниці на інших графіках. Виконавши тест Уілкоксона (чому саме Уілкоксона?),

```r
wilcox.test(a, b)
```

```r
## Warning: cannot compute exact p-value with ties
```

```r
## 
## 	Wilcoxon rank sum test with continuity correction
## 
## data:  a and b
## W = 1.5, p-value = 4.777e-05
## alternative hypothesis: true location shift is not equal to 0
```

отримуємо р<0.05 (різниця між медіанами статистично значима, тобто продажі препарату Б більші), а "Warning message" - застереження, що обчислена р-величина є наближеною, оскільки у вибірках присутні повторення (одинакові числа).

##ПРИКЛАД 5. 
Повернемось до даних щодо зміни кількості проданих лікарських препаратів для 20 вітчизняних та 20 іноземних топ-виробників за період з січня до серпня 2011 року відносно аналогічного періоду 2010 року (див. <a href="http://stat.org.ua/blog/statclasses/descriptive-stat-examples/">приклад 2 з описової статистики</a>). Візуально було встановлено значну близькість показників приросту для іноземних компаній і досить сильну різницю між показниками приросту у групі вітчизняних виробників. Перевіримо чи статистично значимо відрізняються масштаби змін (як опосередковані показники маркетингової стабільності).

```r
adress <- "http://stat.org.ua/data/pharmcomp_aug2011.csv"
data <- read.csv(adress, sep = ";")
fix(data)  #перевіримо чи успішно завантажились дані, дивимось назви #змінних
```

Перевіримо нормальність розподілу даних двох вибірок:

```r
dev1 <- scale(data$PPG[data$ORIGIN == "UA"], TRUE, FALSE)  #відхилення для вітчизняних виробників 
dev2 <- scale(data$PPG[data$ORIGIN == "FOREIGN"], TRUE, FALSE)  #відхилення для #іноземних виробників 
shapiro.test(c(dev1, dev2))
```

```r
## 
## 	Shapiro-Wilk normality test
## 
## data:  c(dev1, dev2)
## W = 0.9641, p-value = 0.2299
```

Оскільки розподіл результатів близький нормального, можна застосувати більш потужний, параметричний критерій для порівняння масштабів – F-критерій Фішера.

```r
var.test(data$PPG ~ data$ORIGIN)  #критерій Фішера
```

```r
## 
## 	F test to compare two variances
## 
## data:  data$PPG by data$ORIGIN
## F = 0.7367, num df = 19, denom df = 19, p-value = 0.5118
## alternative hypothesis: true ratio of variances is not equal to 1
## 95 percent confidence interval:
##  0.2916 1.8612
## sample estimates:
## ratio of variances 
##             0.7367
```

Висновок: досліджувані показники приросту не вказують на різницю між стабільностями вітчизняних та іноземних виробників.
Однак згадаємо, що серед іноземних компаній раніше нами були виявлені два викиди – фірми *Reckitt Benckiser* i *Nycomed* мали значно більший приріст порівняно із всіма іншими у групі. На жаль, ми не володіємо додатковою інформацією, яка дозволила би визначити їх як "особливі", або ж як "частину розподілу". Якщо ці компанії є частиною розподілу для генеральної сукупності, то отриманий висновок є вірним. Якщо ж ці компанії є "особливими", то раціонально буде вилучити їх із досліджуваної вибірки. Так як вони мали найбільші значення приросту, то виконуємо сортування, причому вибираємо тільки ті значення `PPG`, що належать іноземним компаніям, другим параметром змінюємо порядок сортування на спадаючий.

```r
PPG2 <- sort(data$PPG[data$ORIGIN == "FOREIGN"], TRUE)
PPG2 <- PPG2[c(-1, -2)]  #видалимо два перші (найбільші) значення – адже це і є викиди.
var.test(PPG2, data$PPG[data$ORIGIN == "UA"])
```

```r
## 
## 	F test to compare two variances
## 
## data:  PPG2 and data$PPG[data$ORIGIN == "UA"]
## F = 0.3432, num df = 17, denom df = 19, p-value = 0.03111
## alternative hypothesis: true ratio of variances is not equal to 1
## 95 percent confidence interval:
##  0.1337 0.9038
## sample estimates:
## ratio of variances 
##             0.3432
```

Таким чином, якщо вилучити викиди, то при заданому рівні значимості α=0.05 різниця у дисперсіях приростів вітчизняних та іноземних компаній статистично значима. Тобто, в такому разі досліджуваний показник `data$PPG` для іноземних компаній є статистично значимо стабільніший.

___

<div class="nohover">
<a name="shtrygol" id="anchor">[1] Цубанова Н.А., Штриголь С.Ю. Дослідження антидепресивної та анксіолітичної дії спіроциклічного похідного оксіндолу. Вісник фармації. – 2011. - №1. – С. 77-80.</a><br>
<a name="student" id="anchor">[2] Student The probable error of the mean // Biometrika. -1908. - 6. - P. 20.</a>
</div>
