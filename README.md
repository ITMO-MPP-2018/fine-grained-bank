# Синхронизация с помощью тонкой блокировки

Задание включает в себя следующие исходные файлы:

* [`src/Bank.kt`](src/Bank.kt) содержит интерфейс для гипотетического банка.
* [`src/BankImpl.kt`](src/BankImpl.kt) содержит реализацию операций банка для однопоточного случая.
  Данная реализация небезопасна для использования из нескольких потоков одновременно.
  * Альтернативная реализация на Java дана в [`java/BankImpl.java`](java/BankImpl.java).   

Необходимо доработать реализую `BackImpl` так, чтобы она стала безопасной для использования из множества потоков одновременно. 

* Для реализации необходимо использовать тонкую блокировку. 
  * Синхронизация должна осуществляться для каждого счета по отдельности. 
  * Добавьте поле `lock` типа `java.util.concurrent.locks.ReentrantLock` (класс для примитива блокировки) в класс `BankImpl.Account`
    и используйте его для блокировки операций над соответствующим счетом.
  * **Hint:** В Kotlin можно использовать [`lock.withLock { ... }`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.concurrent/java.util.concurrent.locks.-lock/with-lock.html).   
* Для обеспечения линеаризуемости операций должна использоваться двухфазная блокировка для всех операций.
* Для избегания ситуации взаимной блокировки (deadlock) необходимо использовать иерархическую блокировку.
* Весь код должен содержаться в файле `src/BankImpl.kt` или `src/BankImpl.java`. 
  * В случае использования Java, удалите `src/BankImpl.kt` и скопируйте `java/BankImpl.java` в `src/BankImpl.java` в качестве шаблона кода. 
* **В заголовке строке файла, в строке `@author` впишите вашу фамилию и имя**.
   
## Сборка и тестирование

Проект должен собираться и успешно проходить тестирование с помощью команды `gradlew test`. 
При этом автоматически будут запущены следующие тесты:

* [`FunctionalTest`](test/FunctionalTest.kt) проверяет базовую корректность работы операций банка.
* [`MTStressTest`](test/MTStressTest.kt) проверяет основные аспекты корректности работы банка при множестве одновременно работающих потоков исполнения.
* [`LinearizabilityTest`](test/LinearizabilityTest.kt) проверяет все аспекты корректности работы банка и линеаризуемость операций, 
  сравнивая различные варианты одновременного выполнения операций с различными вариантами их перестановки на модельной однопоточной реализации.
* [`CodeTest`](test/CodeTest.kt) делает базовую проверку структуры кода на соответствие заданию. 

Исходная реализация проходит только `FunctionalTest`, но не проходит многопоточные тесты и тест на
структуру кода (нет поля `lock`). Все тесты кроме `CodeTest` будут проходить, если у каждого метода класса `BankImpl` написать 
аннотацию `@Synchronized` (ключевое слово `synchronized` в Java), тем самым сделав _грубую блокировку_. 
Однако задание требует применение тонкой, а не грубой, блокировки.

## Формат сдачи

Выполняйте задание в этом репозитории. Инструкции по сдаче заданий находятся в 
[этом документе](https://docs.google.com/document/d/1GQ0OI_OBkj4kyOvhgRXfacbTI9huF4XJDMOct0Lh5og). 
