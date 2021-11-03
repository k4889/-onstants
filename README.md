# Решение "Прикладные константы"

## Описание

### Назначение
Позволяет разработчику создавать константы для хранения редактируемых значений, которые могут использоваться при разработке прикладного кода.

### Состав
Справочник "Группы констант"
Справочник "Константы"

### Права доступа
Права доступа (справочник Группы констант): роль "Администраторы" - редактирование.  
Права доступа (справочник Константы): роль "Администраторы" - редактирование, роль "Все пользователи" - просмотр.  

`Внимание:` Создание записей запрещено для всех пользователей системы!  
            Удаление разрешено пользователю Integration Service.

### Доступные типы:
- Строка;
- Целочисленное значение;
- Вещественное значение;
- Логическое значение;
- Целочисленный диапазон;
- Вещественный диапазон;
- Строковый список;
- Целочисленный список;
- Вещественный список;
- Текст;
- Значение Base64.

### Порядок работы
Инициализация констант в модуле EditableConstants:
```C#
// Создание группы констант (инициализация из модуля EditableConstants)
CreateGroup("Имя", "Примечание");

// Создание константы (инициализация из модуля EditableConstants)
CreateConstants("Имя", Значение, "Примечание", "Имя группы");
```
Инициализация констант из стороннего модуля:
```C#
// Создание группы констант (инициализация  из стороннего модуля)
EditableConstants.PublicInitializationFunctions.Module.CreateGroup("Имя", "Примечание");

// Создание константы (инициализация из стороннего модуля)
EditableConstants.PublicInitializationFunctions.Module.CreateConstants("Имя", Значение, "Примечание", "Имя группы");
```
`Внимание:` Поскольку в разработке модуля не используются Sid, имена констант должны быть уникальны!

### Примеры
Примеры инициализации констант в модуле EditableConstants:
```C#
// Создание группы
CreateGroup("Тестовая группа", "Тестовое примечание");

// Создание констант
CreateConstants("Строковое значение", "Тест", "Примечание", "Тестовая группа");
CreateConstants("Целочисленное значение", 1, "Примечание", "Тестовая группа");
CreateConstants("Вещественное значение", 1,57, "Примечание", "Тестовая группа");
CreateConstants("Логическое значение", true, "Примечание", "Тестовая группа");
CreateConstants("Целочисленный диапазон", 1, 5, "Примечание", "Тестовая группа");
CreateConstants("Вещественный диапазон", 1,45, 8,49, "Примечание", "Тестовая группа");
CreateConstants("Строковый список", new List<string> {"Строка 1", "Строка 2", "Строка 3"}, "Примечание", "Тестовая группа");
CreateConstants("Целочисленный список", new List<int> {1, 2, 3, 4, 5}, "Примечание", "Тестовая группа");
CreateConstants("Вещественный список", new List<double> {1.56, 2.33, 3.45, 4.75, 5.12}, "Примечание", "Тестовая группа");
CreateConstants("Текстовое значение", "Тест", true, "Примечание", "Тестовая группа");
CreateBase64Constants("Значение Base64", "Тест", "Примечание", "Тестовая группа");
``` 

Примеры чтения и перезаписи значения констант:
```C#
using ConstantsFunc = finex.EditableConstants.PublicFunctions.Module.Remote; 

// Пример получения значения константы с генерацией исключения в случае ошибки
Int? intValue = ConstantsFunc.GetValueIntByName("Имя константы");
List<string> listString = ConstantsFunc.GetValueListStringByName("Имя константы");

// Пример получения значения константы без генерации исключения в случае ошибки
Int? intValue = ConstantsFunc.GetValueIntByName("Имя константы", false);
List<string> listString = ConstantsFunc.GetValueListStringByName("Имя константы", false);

// Пример программного изменения значения константы без генерации исключения в случае ошибки
int intValue = 10; 
ConstantsFunc.SetValueIntByName("Имя константы", intValue, false);
List<string> listString = new List<string> {"Строка 1", "Строка 2", "Строка 3"};
ConstantsFunc.SetValueListStringByName("Имя константы", listString, false);

// Пример получения значения текстовой константы с генерацией исключения в случае ошибки
ConstantsFunc.GetValueTextByName("Имя константы");

// Пример получения значения константы Base64 в формате Base64
ConstantsFunc.GetValueBase64ByName("Имя константы");
// Пример получения значения константы Base64 преобразованного в строку
ConstantsFunc.GetValueBase64ByName("Имя константы", true)
// Пример получения значения константы Base64 преобразованного в строку без генерации исключения
ConstantsFunc.GetValueBase64ByName("Имя константы", true, false);
```


## Порядок установки
Для работы требуется установленный Directum Development studio версии 3.2 и выше.

### Установка для ознакомления
1. Склонировать репозиторий Constants в папку (например C:\WorkFolder).
2. Указать в _ConfigSettings.xml DDS:
```xml
<block name="REPOSITORIES">
  <repository folderName="Base" solutionType="Base" url="<адрес локального репозитория>" />
  <repository folderName="Work" solutionType="Work" url="<адрес локального репозитория>" />
  <repository folderName="<Папка из п.1>" solutionType="Work" url="https://github.com/k4889/Constants" />
</block>
```

### Установка для использования на проекте
Возможные варианты:

#### A. Fork репозитория
1. Сделать fork репозитория Constants для своей учетной записи.
2. Склонировать созданный в п. 1 репозиторий в папку.
3. Указать в _ConfigSettings.xml DDS:
```xml
<block name="REPOSITORIES">
  <repository folderName="Base" solutionType="Base" url="<адрес локального репозитория>" />
  <repository folderName="Work" solutionType="Work" url="<адрес локального репозитория>" />
  <repository folderName="<Папка из п.2>" solutionType="Work" url="<Адрес репозитория gitHub учетной записи пользователя из п. 1>" />
</block>
```

#### B. Подключение на базовый слой.
Вариант не рекомендуется:
* так как при выходе новой версии шаблона разработки не гарантируется обратная совместимость;
* потеряется возможность изменения или доработки функционала под собственые требования;


1. Склонировать репозиторий Settings в папку.
2. Указать в _ConfigSettings.xml DDS:
```xml
<block name="REPOSITORIES">
  <repository folderName="Base" solutionType="Base" url="" /> 
  <repository folderName="<Папка из п.1>" solutionType="Base" url="<Адрес репозитория gitHub>" />
  <repository folderName="<Папка для рабочего слоя>" solutionType="Work" url="https://github.com/k4889/Constants" />
</block>
```

#### C. Копирование репозитория в систему контроля версий.
Рекомендуемый вариант для проектов внедрения.

1. В системе контроля версий с поддержкой git создать новый репозиторий.
2. Склонировать репозиторий Settings в папку с ключом ```--mirror```.
3. Перейти в папку из п. 2.
4. Импортировать клонированный репозиторий в систему контроля версий командой:
```
git push –mirror <Адрес репозитория из п. 1>
```
