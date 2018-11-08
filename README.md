Yii2 PHP Spreadsheet
==============


Экспорт PHP в Excel или импорт Excel в PHP.
Виджет Excel для создания файла Excel или для загрузки файла Excel.


Свойство
--------

string `$mode` режим экспорта или импорта. допустимые значения 'export' и 'import'  

boolean `$isMultipleSheet` для настройки экспорта excel с несколькими листами.  

array `$properties` для свойства set объекта excel.  

array `$models` Объект Model или DataProvider объект с большим количеством данных.

array `$columns` чтобы получить атрибуты из модели, это допустимое значение имеет только атрибут exist в модели. Если это не задано, то весь атрибут модели будет задан в виде столбцов.

array `$headers` чтобы установить столбец заголовка на первую строку. Если хотите, чтобы настраиваемый Заголовок. Если не задано, Заголовок получит метку атрибутов модели. 

string|array `$fileName` название для имени файла для экспорта или импорта. Несколько Имя файла использовать только для режима импорта, не работает, если вы используете режим экспорта.  

string `$savePath` является каталогом для сохранения файла или вы можете очистить его, чтобы установить файл как вложение.

string `$format` для экспорта excel. Допустимое значение 'Xls','Xlsx','Xml','Ods','Slk','Gnumeric','Csv', и 'Html'.  

boolean `$setFirstTitle` чтобы установить заголовки столбца на первой строке. Столбцы будут иметь заголовки в первой строке.

boolean `$asAttachment` чтобы установить файл excel в режим загрузки.

boolean `$setFirstRecordAsKeys` чтобы задать для первой записи в файле excel ключи массива на строку. Если вы хотите установить ключи столбца записи с первой записью, если он не установлен, Заголовок с использованием столбца алфавита в excel. 

boolean `$setIndexSheetByName` чтобы задать индекс листа по имени листа или результат массива, если лист не один.

string `$getOnlySheet` является именем листа для получения данных. Получаем лист с тем же именем.

array|Formatter `$formatter` для форматирования, используется для форматирования значений атрибутов модели в отображаемые тексты. Это может быть либо экземпляр [[Formatter]], либо массив конфигурации для создания экземпляра [[Formatter]]. Если это свойство не задано, то будет использоваться компонент приложения "formatter".


Установка
------------

Предпочтительным способом установки этого расширения является [composer](http://getcomposer.org/download/).

Ввести

```
php composer.phar require --prefer-dist alex290/yii2-spreadsheet "*"
```

или добавить

```
"alex290/yii2-spreadsheet": "*"
```

в раздел require в ваш `composer.json` файл.


Использование 
-----

### Экспортировать данные

### Экспортировать данные в excel файл.


```php
<?php

// экспорта данных только один лист.

\alex290\spreadsheet\Excel::widget([
	'models' => $allModels,
	'mode' => 'export', //значение по умолчанию как 'export'
	'columns' => ['column1','column2','column3'], //без заголовка, заголовок получит значение из метки атрибута.
	'headers' => ['column1' => 'Header Column 1','column2' => 'Header Column 2', 'column3' => 'Header Column 3'], 
]);

\alex290\spreadsheet\Excel::export([
	'models' => $allModels, 
	'columns' => ['column1','column2','column3'], //без заголовка, заголовок получит значение из метки атрибута. 
	'headers' => ['column1' => 'Header Column 1','column2' => 'Header Column 2', 'column3' => 'Header Column 3'],
]);

// экспорта данных с нескольких листов.

\alex290\spreadsheet\Excel::widget([
	'isMultipleSheet' => true, 
	'models' => [
		'sheet1' => $allModels1, 
		'sheet2' => $allModels2, 
		'sheet3' => $allModels3
	], 
	'mode' => 'export', //значение по умолчанию как 'export' 
	'columns' => [
		'sheet1' => ['column1','column2','column3'], 
		'sheet2' => ['column1','column2','column3'], 
		'sheet3' => ['column1','column2','column3']
	],
	//без заголовка, заголовок получит значение из метки атрибута. 
	'headers' => [
		'sheet1' => ['column1' => 'Header Column 1','column2' => 'Header Column 2', 'column3' => 'Header Column 3'], 
		'sheet2' => ['column1' => 'Header Column 1','column2' => 'Header Column 2', 'column3' => 'Header Column 3'], 
		'sheet3' => ['column1' => 'Header Column 1','column2' => 'Header Column 2', 'column3' => 'Header Column 3']
	],
]);

\alex290\spreadsheet\Excel::export([
	'isMultipleSheet' => true, 
	'models' => [
		'sheet1' => $allModels1, 
		'sheet2' => $allModels2, 
		'sheet3' => $allModels3
	], 'columns' => [
		'sheet1' => ['column1','column2','column3'], 
		'sheet2' => ['column1','column2','column3'], 
		'sheet3' => ['column1','column2','column3']
	], 
	//без заголовка, заголовок получит значение из метки атрибута. 
	'headers' => [
		'sheet1' => ['column1' => 'Header Column 1','column2' => 'Header Column 2', 'column3' => 'Header Column 3'],
		'sheet2' => ['column1' => 'Header Column 1','column2' => 'Header Column 2', 'column3' => 'Header Column 3'],
		'sheet3' => ['column1' => 'Header Column 1','column2' => 'Header Column 2', 'column3' => 'Header Column 3']
	],
]);

```

Новая функция для экспорта данных, Вы можете использовать это, если вы знакомы yii gridview. 
То же самое с столбцом данных gridview.
Столбцы в режиме массива допустимыми параметрами являются 'attribute', 'header', 'format', 'value', и footer (TODO).
Столбцы в строковом режиме допустимый макет 'attribute:format:header:footer(TODO)'.
  
```php
<?php
  
\alex290\spreadsheet\Excel::export([
   	'models' => Post::find()->all(),
      	'columns' => [
      		'author.name:text:Author Name',
      		[
      				'attribute' => 'content',
      				'header' => 'Content Post',
      				'format' => 'text',
      				'value' => function($model) {
      					return ExampleClass::removeText('example', $model->content);
      				},
      		],
      		'like_it:text:Reader like this content',
      		'created_at:datetime',
      		[
      				'attribute' => 'updated_at',
      				'format' => 'date',
      		],
      	],
      	'headers' => [
     		'created_at' => 'Date Created Content',
		],
]);
	  
```


### Импортировать данные

Импорт файл excel и возврат в массив.


```php
<?php

$data = \alex290\spreadsheet\Excel::import($fileName, $config); // $config это необязательный

$data = \alex290\spreadsheet\Excel::widget([
		'mode' => 'import', 
		'fileName' => $fileName, 
		'setFirstRecordAsKeys' => true, // если вы хотите установить ключи записи столбца с первой записи, если он не установлен, то заголовок с использованием алфавита столбцов в Excel. 
		'setIndexSheetByName' => true, // установить это, данные Excel с нескольких листов, индекс массива будет установлен с листа имя. Если это не задано, индекс будет использовать числовой. 
		'getOnlySheet' => 'sheet1', // это свойство можно задать, если требуется получить указанный лист из данных excel с несколькими листами.
	]);

$data = \alex290\spreadsheet\Excel::import($fileName, [
		'setFirstRecordAsKeys' => true, // если вы хотите установить ключи записи столбца с первой записи, если он не установлен, то заголовок с использованием алфавита столбцов в Excel. 
		'setIndexSheetByName' => true, // установить это, если данные Excel с нескольких листов, индекс массива будет установлен с листа имя. Если это не задано, индекс будет использовать числовой. 
		'getOnlySheet' => 'sheet1', // это свойство можно задать, если требуется получить указанный лист из данных excel с несколькими листами.
	]);

// импорт данных с нескольких файлов.

$data = \alex290\spreadsheet\Excel::widget([
	'mode' => 'import', 
	'fileName' => [
		'file1' => $fileName1, 
		'file2' => $fileName2, 
		'file3' => $fileName3,
	], 
		'setFirstRecordAsKeys' => true, // если вы хотите установить ключи записи столбца с первой записи, если он не установлен, то заголовок с использованием алфавита столбцов в Excel. 
		'setIndexSheetByName' => true, // установить это, если данные Excel с нескольких листов, индекс массива будет установлен с листа имя. Если это не задано, индекс будет использовать числовой. 
		'getOnlySheet' => 'sheet1', // это свойство можно задать, если требуется получить указанный лист из данных excel с несколькими листами.
	]);

$data = \alex290\spreadsheet\Excel::import([
	'file1' => $fileName1, 
	'file2' => $fileName2, 
	'file3' => $fileName3,
	], [
		'setFirstRecordAsKeys' => true, // если вы хотите установить ключи записи столбца с первой записи, если он не установлен, то заголовок с использованием алфавита столбцов в Excel. 
		'setIndexSheetByName' => true, // установить это, если данные Excel с нескольких листов, индекс массива будет установлен с листа имя. Если это не задано, индекс будет использовать числовой. 
		'getOnlySheet' => 'sheet1', // это свойство можно задать, если требуется получить указанный лист из данных excel с несколькими листами.
	]);

```

Пример результата из кода сверху :

```

// Только один лист или определенный лист.
Array([0] => Array([name] => Alex, [email] => alex@webgoal.ru, [framework interest] => Yii2), [1] => Array([name] => Example, [email] => alex@webgoal.ru, [framework interest] => Yii2));

// данные с нескольких листов
Array([Sheet1] => Array([0] => Array([name] => Alex, [email] => alex@webgoal.ru, [framework interest] => Yii2), [1] => Array([name] => Example, [email] => alex@webgoal.ru, [framework interest] => Yii2)), [Sheet2] => Array([0] => Array([name] => Alex, [email] => alex@webgoal.ru, [framework interest] => Yii2), [1] => Array([name] => Example, [email] => alex@webgoal.ru, [framework interest] => Yii2)));

// данные с несколькими файлами и указанным листом или только одним листом
Array([file1] => Array([0] => Array([name] => Alex, [email] => alex@webgoal.ru, [framework interest] => Yii2), [1] => Array([name] => Example, [email] => alex@webgoal.ru, [framework interest] => Yii2)), [file2] => Array([0] => Array([name] => Alex, [email] => alex@webgoal.ru, [framework interest] => Yii2), [1] => Array([name] => Example, [email] => alex@webgoal.ru, [framework interest] => Yii2)));

// данные с несколькими файлами и несколькими листами
Array([file1] => Array([Sheet1] => Array([0] => Array([name] => Alex, [email] => alex@webgoal.ru, [framework interest] => Yii2), [1] => Array([name] => Example, [email] => alex@webgoal.ru, [framework interest] => Yii2)), [Sheet2] => Array([0] => Array([name] => Alex, [email] => alex@webgoal.ru, [framework interest] => Yii2), [1] => Array([name] => Example, [email] => alex@webgoal.ru, [framework interest] => Yii2))), [file2] => Array([Sheet1] => Array([0] => Array([name] => Alex, [email] => alex@webgoal.ru, [framework interest] => Yii2), [1] => Array([name] => Example, [email] => alex@webgoal.ru, [framework interest] => Yii2)), [Sheet2] => Array([0] => Array([name] => Alex, [email] => alex@webgoal.ru, [framework interest] => Yii2), [1] => Array([name] => Example, [email] => alex@webgoal.ru, [framework interest] => Yii2))));

```

