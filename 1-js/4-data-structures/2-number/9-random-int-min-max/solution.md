# Очевидное неверное решение (round)

Самый простой, но неверный способ -- это сгенерировать значение в интервале `min..max` и округлить его `Math.round`, вот так:

```js
//+ run
function randomInteger(min, max) {
  var rand = min + Math.random() * (max - min)
  rand = Math.round(rand);
  return rand;
}

alert( randomInteger(1, 3) );
```

Эта функция работает. Но при этом она некорректна: вероятность получить крайние значения `min` и `max` будет в два раза меньше, чем любые другие. 

При многократном запуске этого кода вы легко заметите, что `2` выпадает чаще всех.

Это происходит из-за того, что `Math.round()`  получает разнообразные случайные числа из интервала от `1` до `3`, но при округлении до ближайшего целого получится, что:

```js
//+ no-beautify
значения из диапазона 1   ... 1.49999..  станут 1
значения из диапазона 1.5 ... 2.49999..  станут 2 
значения из диапазона 2.5 ... 2.99999..  станут 3
```

Отсюда явно видно, что в `1` (как и `3`) попадает диапазон значений в два раза меньший, чем в `2`. Из-за этого такой перекос.

# Верное решение с round

Правильный способ: `Math.round(случайное от min-0.5 до max+0.5)`

```js
//+ run
*!*
function randomInteger(min, max) {
    var rand = min - 0.5 + Math.random() * (max - min + 1)
    rand = Math.round(rand);
    return rand;
  }
*/!*

alert( randomInteger(5, 10) );
```

В этом случае диапазон будет тот же (`max-min+1`), но учтена механика округления `round`.

# Решение с floor

Альтернативный путь - применить округление `Math.floor()` к случайному числу от `min` до `max+1`. 

Например, для генерации целого числа от `1` до `3`, создадим вспомогательное случайное значение от `1` до `4` (не включая `4`).

Тогда `Math.floor()` округлит их так:

```js
//+ no-beautify
1 ... 1.999+ станет 1
2 ... 2.999+ станет 2
3 ... 3.999+ станет 3
```

Все диапазоны одинаковы.
Итак, код:

```js
//+ run
*!*
function randomInteger(min, max) {
    var rand = min + Math.random() * (max + 1 - min);
    rand = Math.floor(rand);
    return rand;
  }
*/!*

alert( randomInteger(5, 10) );
```
