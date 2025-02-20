Move Zeroes
===========

Nov 23, 2020 · 3 min read

Дан массив целых чисел. Написать функцию, которая поставит все нули в конец массива. Относительный порядок должен сохраниться.

Пример:

    Input: [0,1,0,3,12]
    Output: [1,3,12,0,0]
    

[Задача на LeetCode](https://leetcode.com/problems/move-zeroes/).

Решение [#](#решение)
---------------------

Отличная задача для phone interview. Хороша она тем, что тривиально пишется «в лоб», а дальше можно написать несколько follow-up — это и просят сделать.

Что значит без дополнительной памяти? Значит нельзя использовать промежуточный массив куда бы мы складывали все числа подряд, которые не нули. Это и есть тривиальное решение.

Давайте попробуем рассмотреть пример.

    0, 1, 0, 3, 12
    ^-- начинаем здесь
    

Ставим указательно в начало, и сразу попадаем на ноль, который должен оказаться в конце. Можно ли, скажем, поменять местами его с последним элементом?

В условии сказано, что _относительный порядок должен сохраниться_.

> Кстати, на собеседовании это могут намеренно не уточнить. Это надо сделать самостоятельно, до того как писать код.

Хорошо, с последним менять нельзя. Может тогда начать с конца?

    0, 1, 0, 3, 12
                ^-- начинаем здесь
             ^-- не ноль, идём дальше
          ^-- что делать с нулём?
    

Как только указатель оказался на нуле — отправляем его «всплывать» в конец, как в сортировке пузырьком: меняемся с соседом. В итоге, и ноль окажется в конец, и относительный порядок сохранится.

    0, 1, 0, 3, 12
          ^--^
    0, 1  3, 0, 12
             ^--^
    0, 1  3, 12, 0
    

Какая сложность у данного решения? Как и с сортировкой пузырьком — квадратная.

С каждым следующим нулём всплывать элементу всё дальше и дальше, т.е. количество перестановок будет определяться по формуле арифметической прогрессии, что даст `O(n^2)`.

Можно ли быстрее?

Для ответа на этот вопрос надо понять где совершается лишняя работа. Очевидно мы делаем слишком много перестановок пытаясь поставить ноль в конец. Точно ли нужно пытаться ставить его в конец?

Посмотрим на проблему иначе. На самом деле, нам не важно, что находится в конце, нам важно, что находится в начале. Если мы сможем поставить все не нулевые элементы в начало, то хвост можно просто занулить.

Заводим два указателя, `slow` и `fast`. Быстрый обгоняет медленный, пропуская нули, а все ненули — ставит на место медленного (продвигая его дальше). В итоге, все ненули перемещаются в начало массив, не меняя порядок.

Так как быстрый указатель пропускает нули, то `fast - slow` в итоге равен их количеству, а `slow` будет указывать на начало хвоста, который надо занулить.

Сложность данного решения `O(n)`, потому что каждый элемент мы посмотрели не более 1 раза.

Когда решение готов и расписано на доске — пора писать код.

    /**
     * @param {number[]} nums
     * @return {void} Do not return anything, modify nums in-place instead.
     */
    var moveZeroes = function(nums) {
        let slow = 0;
        let fast = 0;
    
        while (fast < nums.length) {
            if (nums[fast] !== 0) {
                nums[slow++] = nums[fast];
            }
            fast++;
        }
        while (slow < nums.length) {
            nums[slow++] = 0;
        }
    };
    

PS. Обсудить можно в [телеграм-чате](https://t.me/ctci_chat_ru) любознательных программистов. Welcome! 🤗

Подписывайтесь на [мой твитер](https://twitter.com/vitkarpov) или [канал в телеграме](https://t.me/coding_interviews), чтобы узнавать о новых разборах задач.