# Коллекции для параллельной работы

## Задача 1 (обязательная)

Возьмём следующий генератор случайного текста:

```java
    public static String generateText(String letters, int length) {
        Random random = new Random();
        StringBuilder text = new StringBuilder();
        for (int i = 0; i < length; i++) {
            text.append(letters.charAt(random.nextInt(letters.length())));
        }
        return text.toString();
    }
```

Вам стало интересно, если генерировать строки из символов "abc" размером 100_000 каждая, то из 10_000 текстов как выглядела бы та, у которой максимальное количество символов 'a'? символов 'b'? символов 'c'?

Пусть мы хотим попробовать решить эту задачу многопоточно: чтобы за анализ строк на предмет максимального количества каждого из трёх символов отвечал отдельный поток. Т.е. за поиск строки с самым большим количеством символов 'a' отвечал бы один поток, за поиск с самым большим количеством 'b' - второй поток и за 'c' третий поток.

Однако сгенерировать все тексты, сохранить в массив и затем пройтись по ним было бы неправильно, тк суммарно в текстах было бы под 1 млрд. символов, что привело бы к огромнейшему расходу памяти. Мы можем пойти другим путём и распараллелить этап создания строк и этапы их анализа.

Для этого у вас строки будут генерироваться в отдельном потоке и заполнять блокирующие очереди, максимальный размер которых ограничьте в 100 строк.
Очереди нужно будет сделать по одной для каждого из трёх анализирующих потоков, тк строка должна быть обработана каждым таким потоком.

Подсказка: воспользуйтесь `ArrayBlockingQueue`.

В итоге:
1. Создайте в статических полях три потокобезопасные блокирующие очереди
2. Создайте поток, который наполнял бы их текстами
3. Создайте по потоку для каждого из трёх символов 'a', 'b' и 'c', которые разбирали бы свою очередь и делали бы подсчёты

На проверку отправьте ссылку на репозиторий с решением.
