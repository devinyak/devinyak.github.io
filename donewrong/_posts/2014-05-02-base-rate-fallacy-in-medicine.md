---
layout: post
tags: [основи статистики, р-величина, базова частота]
title: "&nbsp&nbsp&nbsp&nbspПомилка базової частоти у медицині"
author: Алекс Рейнхарт (переклав Олег Девіняк)
date: "2014-05-02" 
category: donewrong
---

Тивалий час існують дебати щодо використання мамографії для скринінгу раку молочної залози. Дехто аргументує, що шкода від хибно позитивних результатів, така як зайві біопсії, хірургія та хіміотерапія, переважує позитив раннього виявлення раку. Давайте дослідимо це питання статистично.

Припустимо, 0.8% жінок, які проходять мамографію, мають рак грудей. З тих, що мають рак, мамограма ідентифікує не всіх, а 90%. (Це є потужність тесту. Це приблизна оцінка, так як непросто сказати, скільки випадків раку пропущено, якщо ми не знаємо, власне, в кого є рак.) А серед тих жінок, що не мають раку, близько 7% отримають позитивний висновок з мамограми і будуть направлені на подальші дослідження, біопсію і т.д. Тепер питання: якщо ви отримали позитивний результат мамограми, яка імовірність, що у вас рак? 

Якщо не враховувати, що ви, читач, можете бути чоловіком (у чоловіків, до речі, також буває рак грудей, однак значно рідше), то відповідь буде 9% <a href="#kramer">\[12\]</a>. 

Не зважаючи на те, що частка хибнопозитивних результатів склаадє лише 7%, що відповідає тестуванню з р<0.07, 91% позитивних результатів - хибнопозитивні.

Як це можна порахувати? Так само, як і у випадку з протипухлинними препаратами. Уявимо 1000 випадково вибраних жінок, які пройшли мамографію. Вісім з них (0.8%) - мають рак. Мамограма коректно ідентифікує 90% випадків раку, тому приблизно сім з восьми випадків раку будуть виявлені. Але ще залишилось 992 жінок без раку, і 7% шанс отримати хибнопозитивний результат, що разом дає нам 70 жінок, яким хибно ставиться діагноз раку.

Загалом, маємо 77 жінок з позитивним результатом мамограми, з яких лише 7 дійсно мають рак. Отже, лише 9% жінок з позитивним результатом мамограми матимуть рак.

Якщо ж таке питання задати одному із студентів спеціальності "статистика", або одному з наукових керівників, більше третини дають неправильні відповіді (це в США <a href="#kramer">\[12\]</a>, в Україні буде ще гірше). Якщо спитати у лікарів - помиляться дві третини <a href="#Bramwell">\[13\]</a>. Вони роблять хибний висновок, що \\(р<0.05\\) означає 95% імовірність, що результати правдиві - але, як ви могли побачити на прикладах, вірогідність того, щоб позитивний результат був вірним, залежить від того, *яка частка із тестованих гіпотез справджується*. На щастя, на момент скринінгу лише маленька частка жінок має рак. 

Таку помилку можна знайти навіть у посібниках з початків статистики. Р-величини не є інтуїтивно зрозумілими, і помилка базової частоти набула характеру епідемії.

___

<div class="nohover">
<a name="kramer", id="anchor">[12] W. Krämer, G. Gigerenzer. How to Confuse with Statistics or: The Use and Misuse of Conditional Probabilities. Statistical Science, 20:223–230, 2005. </a><br>
<a name="Bramwell", id="anchor">[13] R. Bramwell, H. West. Health professionals’ and service users’ interpretation of screening test results: experimental study. BMJ, 2006.</a>
</div>

