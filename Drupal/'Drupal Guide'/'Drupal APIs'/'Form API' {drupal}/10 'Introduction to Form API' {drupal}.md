♐ [](https://www.drupal.org/docs/drupal-apis/form-api/introduction-to-form-api)[https://www.drupal.org/docs/drupal-apis/form-api/introduction-to-form-api](https://www.drupal.org/docs/drupal-apis/form-api/introduction-to-form-api)

# Обзор

Классы формы реализуют [\\Drupal\\Core\\Form\\FormInterface](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Form%21FormInterface.php/interface/FormInterface/8.2.x) , а основной рабочий процесс формы определяется методами интерфейса `buildForm`, `validateForm` и `submitForm`. Когда форма запрашивается, она определяется как рендер-массив, часто называемый Form API массивом [form-array {drupal}](https://www.notion.so/form-array-drupal-3486e93f934248fcbada7173404cb7b2) или просто массивом `$form`. Массив `$form` преобразуется в HTML в процессе рендеринга и отображается конечному пользователю. Когда пользователь отправляет форму, запрос делается на тот же URL, на котором она была отображена, Drupal замечает входящие HTTP POST данные в запросе, и на этот раз вместо того, чтобы строить форму и отображать ее как HTML, он строит форму, а затем переходит к вызову соответствующих обработчиков валидации и отправки.

Определение форм как структурированных массивов, а не как прямого HTML, имеет много преимуществ, в том числе:

-   Согласованный вывод HTML для всех форм.
-   Формы, предоставляемые одним модулем, могут быть легко изменены другим модулем без сложного поиска и замены логики.
-   Сложные элементы форм, такие как загрузка файлов и виджеты голосования, могут быть инкапсулированы в многоразовые пакеты, включающие как логику отображения, так и логику обработки.

Существует несколько типов форм, обычно используемых в Drupal. Каждая из них имеет базовый класс, который вы можете расширить в своем собственном пользовательском модуле.

Во-первых, определите тип формы, которую необходимо собрать:

1.  Общая форма. Расширьте [FormBase](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Form%21FormBase.php/class/FormBase/8).
2.  Форма конфигурации, позволяющая администраторам обновлять настройки модуля. Расширьте [ConfigFormBase](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Form%21ConfigFormBase.php/class/ConfigFormBase/8).
3.  Форма для удаления содержимого или конфигурации, которая обеспечивает шаг подтверждения. Расширьте [ConfirmFormBase](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Form%21ConfirmFormBase.php/class/ConfirmFormBase/8).

FormBase реализует `[FormInterface](<https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Form%21FormInterface.php/interface/FormInterface/8.3.x>)`, и как ConfigFormBase, так и ConfirmFormBase расширяют FormBase, поэтому любые формы, расширяющие эти классы, должны реализовывать несколько обязательных методов.

# Обязательные методы

[FormBase](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Form%21FormBase.php/class/FormBase/8.3.x) реализует [FormInterface](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Form%21FormInterface.php/interface/FormInterface/8.3.x), и поэтому любая форма, имеющая `FormBase` в своей иерархии, должна реализовать несколько методов:

-   `[getFormId()](<https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Form%21FormInterface.php/function/FormInterface%3A%3AgetFormId/8.3.x>)`
-   `[buildForm()](<https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Form%21FormInterface.php/function/FormInterface%3A%3AbuildForm/8.3.x>)`
-   `[submitForm()](<https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Form%21FormInterface.php/function/FormInterface%3A%3AsubmitForm/8.3.x>)`

## \= **getFormId()**

`public function getFormId()`

Это нужно для того, чтобы вернуть строку, которая является уникальным идентификатором вашей формы. Пространство имён form ID базируется на имени вашего модуля.

Пример:

```php
  public function getFormId() {
    return 'mymodule_settings';
  }
```

## \= **buildForm()**

`public function buildForm(array $form, FormStateInterface $form_state)`

Это возвращает Form API массив [form-array {drupal}](https://www.notion.so/form-array-drupal-3486e93f934248fcbada7173404cb7b2) , который определяет каждый элемент, из которых состоит ваша форма.

Пример:

```php
  public function buildForm(array $form, FormStateInterface $form_state) {
    $form['phone_number'] = [
      '#type' => 'tel',
      '#title' => $this->t('Example phone'),
    ];
        
    $form['submit'] = [
      '#type' => 'submit',
      '#value' => $this->t('Submit'),
    ];

    return $form;
  }
```

# Валидация форм

После того, как пользователь заполнит форму и нажмет кнопку "Отправить", обычно требуется выполнить какую-то проверку данных, которые собираются. Для этого с помощью Drupal's Form API мы просто реализуем метод `validateForm` из `\\\\Drupal\\\\Core\\\\Form\\\\FormInterface` в нашем классе `ExampleForm`.

Значения формы, передаваемые пользователем, содержатся в объекте `$form_state` по адресу `$form_state->getValue('field_id')`, где ключом является 'field\_id', используемый при добавлении элемента формы в массив `$form` в `ExampleForm::buildForm()`. Мы можем выполнить пользовательскую проверку этого значения. Если Вам необходимо получить все представленные значения, Вы можете сделать это с помощью `$form_state->getValues()`.

Методы проверки формы могут использовать любую PHP обработку, необходимую для проверки того, что поле содержит нужное значение, и вызвать ошибку в случае, если оно является недействительным значением. В этом случае, поскольку мы расширяем класс `\\\\Drupal\\\\Core\\\\Form\\\\FormBase`, мы можем использовать `\\\\Drupal\\\\Core\\\\Form\\\\FormStateInterface::setErrorByName()` для регистрации ошибки на конкретном элементе формы и предоставления связанного с ней сообщения, объясняющего ошибку.

Когда форма отправляется, Drupal проходит через все обработчики валидации для формы, как обработчики валидации по умолчанию, так и любые обработчики валидации, добавленные разработчиками. При наличии ошибок HTML формы перестраивается, показываются сообщения об ошибках и выделяются поля с ошибками. Это позволяет пользователю исправить любые ошибки и повторно отправить форму. Если ошибок нет, то выполняются обработчики отправки формы.

Ниже приведен пример простого метода `validateForm()`:

```php
/**
 * {@inheritdoc}
 */
public function validateForm(array &$form, FormStateInterface $form_state) {
  if (strlen($form_state->getValue('phone_number')) < 3) {
    $form_state->setErrorByName('phone_number', $this->t('The phone number is too short. Please enter a full phone number.'));
  }
}
```

Если во время проверки формы не было зарегистрировано ни одной ошибки, Drupal продолжает обработку формы. На этом этапе предполагается, что значения внутри `$form_state->getValues()` являются действительными и готовы к обработке и использованию любым способом, необходимым нашему модулю для использования данных.

# Отправка форм / Обработка данных формы

Наконец, мы готовы использовать собранные данные и делать такие вещи, как сохранение данных в базе данных, отправка электронной почты или любые другие операции. Для этого с помощью Drupal's Form API нам нужно реализовать метод `submitForm` из `\\\\Drupal\\\\Core\\\\Form\\\\FormInterface` в нашем классе `ExampleForm`.

Как и в случае с приведенным выше методом валидации, значения, собранные при отправке формы, находятся в `$form_state->getValues()` и на данный момент можно предположить, что данные прошли валидацию и готовы к использованию. Например, доступ к значению нашего поля 'phone\_number' можно получить, обратившись к `$form_state->getValue('phone_number')`.

Приведем пример простого метода submitForm, который отображает значение поля 'phone\_number' на странице с помощью мессенджер-сервиса:

```php
/**
 * {@inheritdoc}
 */
public function submitForm(array &$form, FormStateInterface $form_state) {
  $this->messenger()->addStatus($this->t('Your phone number is @number', ['@number' => $form_state->getValue('phone_number')]));
}
```

Это простой пример обработки предоставленных данных формы. Для более сложных примеров посмотрите [некоторые из классов, расширяющих FormBase](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Form%21FormBase.php/class/uses/FormBase/8) в ядре.

Приведем полный пример класса формы:

Содержимое файла `/modules/example/src/Form/ExampleForm.php`, если модуль находится в `/modules/example`:

```php
<?php

namespace Drupal\\example\\Form;

use Drupal\\Core\\Form\\FormBase;
use Drupal\\Core\\Form\\FormStateInterface;

/**
 * Implements an example form.
 */
class ExampleForm extends FormBase {

  /**
   * {@inheritdoc}
   */
  public function getFormId() {
    return 'example_form';
  }

  /**
   * {@inheritdoc}
   */
  public function buildForm(array $form, FormStateInterface $form_state) {
    $form['phone_number'] = [
      '#type' => 'tel',
      '#title' => $this->t('Your phone number'),
    ];
    $form['actions']['#type'] = 'actions';
    $form['actions']['submit'] = [
      '#type' => 'submit',
      '#value' => $this->t('Save'),
      '#button_type' => 'primary',
    ];
    return $form;
  }

  /**
   * {@inheritdoc}
   */
  public function validateForm(array &$form, FormStateInterface $form_state) {
    if (strlen($form_state->getValue('phone_number')) < 3) {
      $form_state->setErrorByName('phone_number', $this->t('The phone number is too short. Please enter a full phone number.'));
    }
  }

  /**
   * {@inheritdoc}
   */
  public function submitForm(array &$form, FormStateInterface $form_state) {
    $this->messenger()->addStatus($this->t('Your phone number is @number', ['@number' => $form_state->getValue('phone_number')]));
  }

}
```

Идентификатор формы возвращается методом `getFormId()` класса формы. Метод конструктора вызывает `buildForm()` а также у нас есть специальные методы для валидации и отправки.

# Интеграция формы в запрос

Система маршрутизации позволяет формировать классы в качестве обработчиков маршрутов, в этом случае система маршрутизации заботится о инстанцировании этого класса и обращении к соответствующим методам. Чтобы интегрировать эту форму в структуру URI сайта Drupal, используйте маршрут, как показано ниже:

Содержимое файла `/modules/example/example.routing.yml`, если модуль находится в `/modules/example`:

```php
example.form:
  path: '/example-form'
  defaults:
    _title: 'Example form'
    _form: '\\Drupal\\example\\Form\\ExampleForm'
  requirements:
    _permission: 'access content'
```

Ключ `_form` сообщает системе маршрутизации, что предоставленное имя класса является классом формы, который должен быть инстанцирован и обработан как форма.

Обратите внимание, что класс формы и элемент маршрутизации - единственные две части, необходимые для того, чтобы эта форма работала; нет другого оборачивающего кода, который нужно было бы написать.

# Получение текущей формы вне роутов

Несмотря на то, что `drupal_get_form()`из Drupal 7 в Drupal 8 отсутствует, существует сервис FormBuilder, который может быть использован для получения и обработки форм. Эквивалент Drupal 8 для `drupal_get_form()` следующий:

```php
$form = \\Drupal::formBuilder()->getForm('Drupal\\example\\Form\\ExampleForm');
```

Аргументом, передаваемым в метод `getForm()`, является имя класса, который определяет форму и является реализацией `\\\\Drupal\\\\Core\\\\Form\\\\FormInterface`. Если необходимо передать в форму какие-либо дополнительные параметры, передайте их после имени класса.

Например:

```php
$extra = '612-123-4567';
$form = \\Drupal::formBuilder()->getForm('Drupal\\mymodule\\Form\\ExampleForm', $extra);
...
public function buildForm(array $form, FormStateInterface $form_state, $extra = NULL)
  $form['phone_number'] = [
    '#type' => 'tel',
    '#title' => $this->t('Your phone number'),
    '#value' => $extra,
  ];
  return $form;
}
```

В некоторых особых случаях может понадобиться манипулировать объектом `$form` до того, как `FormBuilder` вызовет метод вашего класса `buildForm()`, и в этом случае можно сделать `$form_object = new \\\\Drupal\\\\mymodule\\\\Form\\\\ExampleForm($something_special);`.

`$form_builder->getForm($form_object);`

# Изменение формы

Altering forms is where the Drupal 8 Form API reaches into basically the same hook-based approach as Drupal 7. You can use `hook_form_alter()` and/or `hook_form_FORM_ID_alter()` to alter the form, where the ID is the form ID you provided when defining the form previously.

```php
/**
 * Implements hook_form_FORM_ID_alter().
 */
function example2_form_example_form_alter(&$form, \\Drupal\\Core\\Form\\FormStateInterface $form_state) {
  $form['phone_number']['#description'] = t('Start with + and your country code.');
}
```

Реализацию `hook_form_FORM_ID_alter()` мы назвали по имени нашего модуля (example2), включая ID формы (`example_form`). Это тот же паттерн, который используется в изменении формы Drupal 7.

# See also

-   [Drupal 8 Form API reference](https://api.drupal.org/api/drupal/elements/)
-   [The Drupal 7 Form API](https://api.drupal.org/api/drupal/developer%21topics%21forms_api_reference.html/7.x)
-   [Routing system in Drupal 8](https://drupal.org/node/2092643) details the routing file keys

{ - done - 2021-02-11 }