### Hexlet tests and linter status:
[![Actions Status](https://github.com/fedorovaea18/java-project-78/actions/workflows/hexlet-check.yml/badge.svg)](https://github.com/fedorovaea18/java-project-78/actions)
[![Maintainability](https://api.codeclimate.com/v1/badges/f98370da14866d304cd0/maintainability)](https://codeclimate.com/github/fedorovaea18/java-project-78/maintainability)
[![Test Coverage](https://api.codeclimate.com/v1/badges/f98370da14866d304cd0/test_coverage)](https://codeclimate.com/github/fedorovaea18/java-project-78/test_coverage)
[![GitHub Actions Status](https://github.com/fedorovaea18/java-project-78/actions/workflows/github-actions.yml/badge.svg)](https://github.com/fedorovaea18/java-project-78/actions)

# **Валидатор данных**

Данный проект реализует функцию создания собственной библиотеки для проверки валидации данных в зависимости от их типа. Проверяемые типы данных: String, Number и Map.

## **Валидация строк**

В проекте реализованы следующие методы проверки строк:

- _required()_ — строка должна быть заполненной(непустой);

- _minLength()_ — строка должна быть равна или длиннее указанного числа;

- _contains()_ — cтрока должна содержать определённую подстроку.
  
После настройки схемы валидации необходимо вызвать метод _isValid()_ для проверки данных.

Пример использования:
```java
Validator v = new Validator();
StringSchema schema = v.string();

schema.required().isValid(""); //false
schema.minLength(6).isValid("fox"); //false
schema.contains("what").isValid("what does the fox say"); // true
```
## **Валидация чисел**

В проекте реализованы следующие методы проверки чисел:

- _required()_ — требуется наличие любого числа;

- _positive()_ — число должно быть положительным;

- _range()_ — значение числа должно попадать в диапазон, включая границы.

После настройки схемы валидации необходимо вызвать метод _isValid()_ для проверки данных.

Пример использования:
```java
Validator v = new Validator();
NumberSchema schema = v.number();

schema.required().isValid(-10); //false
schema.positive().isValid(5); // true
schema.range(5, 10).isValid(4); //false
```
## **Валидация объектов типа Map**

В проекте реализованы следующие валидаторы проверки объектов Map:

- _required()_ — требуется тип данных Map;

- _sizeof()_ — количество пар ключ-значений в объекте Map должно быть равно заданному.

После настройки схемы валидации необходимо вызвать метод _isValid()_ для проверки данных.

Пример использования:
```java
Validator v = new Validator();
MapSchema schema = v.map();

schema.required().isValid(new HashMap<>()); //true

Map<String, String> data = new HashMap<>();
data.put("key1", "value1");
schema.isValid(data); // true

schema.sizeof(2);

schema.isValid(data);  // false
data.put("key2", "value2");
schema.isValid(data); // true
```

## **Вложенная валидация**

В проекте реализована проверка данных внутри объектов Map с помощью валидатора _shape()_.
После настройки схемы валидации необходимо вызвать метод _isValid()_ для проверки данных.

Пример использования:
```java
Validator v = new Validator();
MapSchema schema = v.map();

Map<String, BaseSchema> schemas = new HashMap<>();
schemas.put("firstName", v.string().required());
schemas.put("lastName", v.string().required().minLength(2));
schemas.put("age", v.number().required().positive());

schema.shape(schemas);

Map<String, Object> human1 = new HashMap<>();
human1.put("firstName", "John");
human1.put("lastName", "Smith");
human1.put("age", 30);
schema.isValid(human1); //true

Map<String, Object> human2 = new HashMap<>();
human2.put("firstName", "Anna");
human2.put("lastName", "B");
human2.put("age", -5);
schema.isValid(human2); //false

Map<String, Object> human3 = new HashMap<>();
human3.put("firstName", "Bob");
human3.put("lastName", null);
human3.put("age", 45);
schema.isValid(human3); //false
```
