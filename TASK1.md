# Задача "Понимание JVM"

## Код для исследования
```java

public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}

```

## Описание

Первым делом работает ClassLoader загружая основные классы для нашего проекта: Object, Integer, String. Также, следом обрабатывается наш класс JvmComprehension с записью в Metaspace. 
Далее JVM вызывает метод main(String[] args) как основную точку входа, не забывая создать фрейм в Stack.
По пунктам:
1. Во фрейм метода main записывается примитив int i = 1
2. В Heap создается новый объект o класса Object, а ссылка на этот объект записывается в фрейм метода main в Stack Memory
3. Аналогичное создание объекта ii класса Integer в Heap с ссылкой в фрейме метода main в Stack Memory 
4. Происодит создание нового фрейма в Stack Memory для метода printAll. Примитив i запишитеся в Stack в этот фрейм, также сюда попадут ссылки на объекты o и ii
5. Создание объекта uselessVar класса Integer в Heap и записванием ссылки в Stack в фрейм метода printAll
6. Результат выполнения выражения o.toString() + i + ii попадет в Heap в пул строк, ссылка на этот результат передатся в созданный очереднйо фрейм под println в Stack Memory. При последующей работе сборщика мусора будет очищен.
7. Аналогично пункту 6 строка "finished" попадет в Heap в пул строк во фрейме метода main. и будт очищен при следующей работе сборщика мусора

