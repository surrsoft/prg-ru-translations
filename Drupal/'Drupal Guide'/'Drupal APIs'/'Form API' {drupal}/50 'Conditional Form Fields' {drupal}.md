[](https://www.drupal.org/docs/drupal-apis/form-api/conditional-form-fields)[https://www.drupal.org/docs/drupal-apis/form-api/conditional-form-fields](https://www.drupal.org/docs/drupal-apis/form-api/conditional-form-fields)

Эта документация не окончена. [Add more information](https://www.drupal.org/node/3066337/edit).

Проперти Drupal's Form API `#states` позволяет легко показывать или скрывать, включать или отключать, требовать заполнение или сворачивать поля формы на основе значений, выбранных или введенных в другие поля этой формы или в любое другое место на странице.

# Обзор

Вы просто определяете обычный css селектор целевого поля в проперти #states вашего элемента поля формы. При изменении значения целевого поля изменится и состояние вашего элемента. Не имеет значения, находится ли это поле на одной и той же форме или нет.

Хотя вы также можете использовать [AJAX Callback](https://www.drupal.org/docs/8/api/javascript-api/ajax-forms) для достижения подобной функциональности, свойство #states позволяет создавать зависимые динамические поля с небольшим количеством кода.

Допустим, у вас есть выбранное поле, в котором пользователи выбирают свой любимый цвет. Вы не будете перечислять все цвета на планете, поэтому добавите опцию "Другие" в конце списка. Если пользователь выбирает "Другие", вы хотите отобразить текстовое поле, в котором пользователь может ввести свой любимый цвет.

![[50-01.gif]]

# Form API проперти #states

Документация API [drupal\_process\_states](https://api.drupal.org/api/drupal/core%21includes%21common.inc/function/drupal_process_states/8.7.x) гласит:

`state` означает определенное проперти элемента DOM, например, "visible" или "check". State может быть применено к элементу, в зависимости от state другого элемента на странице. В общем случае states зависят от HTML-атрибутов и свойств DOM-элемента, которые изменяются в результате взаимодействия с пользователем.

Поскольку states управляются только JavaScript, важно понимать, что все состояния применяются только на представлении, ни одно из состояний не затрагивает логику серверной стороны, и что они не будут применяться для посетителей сайта не имеющих поддержки JavaScript. Все модули, реализующие states, должны убедиться, что предусмотренная логика также работает без включённого JavaScript.

**States, которые могут быть применены к элементу поля формы:**

-   enabled
-   disabled
-   required
-   optional
-   visible
-   invisible
-   checked
-   unchecked
-   expanded
-   collapsed

**При проверке значений других полей могут использоваться следующие состояния:**

-   empty
-   filled
-   checked
-   unchecked
-   expanded
-   collapsed
-   value

**Следующие состояния существуют как для элементов, так и для внешних условий.** Они могут быть реализованы не во всех браузерах и могут ничего не изменить в элементе:

-   relevant
-   irrelevant
-   valid
-   invalid
-   touched
-   untouched
-   readwrite
-   readonly

При обращении к спискам выбора и радио-кнопкам или option-кнопкам во внешних условиях, необходимо использовать условие "value".

Более подробную информацию можно найти в [Drupal API Docs for drupal\_process\_states](https://api.drupal.org/api/drupal/core%21includes%21common.inc/function/drupal_process_states/8.7.x)(...). A [список всех элементов поля формы](https://api.drupal.org/api/drupal/elements/8.7.x) можно найти в [Form and render elements API docs](https://api.drupal.org/api/drupal/elements/8.7.x).

# Пример условных полей

Создадим поле выбора, в котором пользователи будут выбирать любимый цвет. В конце списка добавим опцию "Другой". Если пользователь выберет "Другой", то появится текстовое поле, в котором пользователь сможет ввести свой любимый цвет.

![[50-02.gif]]

```php

<?php

namespace Drupal\\ajaxfilters\\Form;

use Drupal\\Core\\Form\\FormBase;
use Drupal\\Core\\Form\\FormStateInterface;

class DefaultForm extends FormBase
{
  public function getFormId() {
    return 'default_form';
  }

  public function buildForm(array $form, FormStateInterface $form_state)
  {
    // создание списка радио-кнопок которые будут включать текстовое поле при выборе опции 'other'
    $form['colour_select'] = [
      '#type' => 'radios',
      '#title' => $this->t('Pick a colour'),
      '#options' => [
        'blue' => $this->t('Blue'),
        'white' => $this->t('White'),
        'black' => $this->t('Black'),
        'other' => $this->t('Other'),
      ],
      '#attributes' => [
        // задание статического имени и id чтобы мы могли легко обращаться к нем        
        // 'id' => 'select-colour',
        'name' => 'field_select_colour',
      ],
    ];

    // это текстовое поле будет отображаться только при выборе опции 'Other'
    $form['custom_colour'] = [
      '#type' => 'textfield',
      '#size' => '60',
      '#placeholder' => 'Enter favourite colour',
      '#attributes' => [
        'id' => 'custom-colour',
      ],
      '#states' => [
        // отображаем текстовое поле если выбрана радио-кнопка 'other'
        'visible' => [
          // не ошибитесь с :input для типа поля. Вы всегда будете использовать здесь :input, вне зависимости является ли для вас источником select, radio или checkbox
          ':input[name="field_select_colour"]' => ['value' => 'other'],
        ],
      ],
    ];

    // создание кнопки отправки
    $form['submit'] = [
      '#type' => 'submit',
      '#value' => $this->t('Sorry, I\\'m colour-blind'),
    ];

    return $form;
  }

  public function validateForm(array &$form, FormStateInterface $form_state)
  {
    parent::validateForm($form, $form_state);
  }

  public function submitForm(array &$form, FormStateInterface $form_state)
  {
    // отображение результата
    foreach ($form_state->getValues() as $key => $value) {
      drupal_set_message($key . ': ' . $value);
    }
  }

}
```

Теперь давайте немного улучшим этот пример. Убедитесь, что пользователь не может выбрать другой цвет, после того, как он выбрал "Другой" и ввел пользовательский любимый цвет. Для этого мы также добавим свойство #states в список радио-кнопок и скажем ему, чтобы включить это поле только в том случае, если значение текстового поля пользовательского цвета пустое.

![https://www.drupal.org/files/drupalformapistates2.gif](https://www.drupal.org/files/drupalformapistates2.gif)

```php
  public function buildForm(array $form, FormStateInterface $form_state)
  {
    //create a list of radio boxes that will toggle the  textbox
    //below if 'other' is selected
    $form['colour_select'] = [
      '#type' => 'radios',
      '#title' => $this->t('Pick a colour'),
      '#options' => [
        'blue' => $this->t('Blue'),
        'white' => $this->t('White'),
        'black' => $this->t('Black'),
        'other' => $this->t('Other'),
      ],
      '#attributes' => [
        //define static name and id so we can easier select it
        // 'id' => 'select-colour',
        'name' => 'field_select_colour',
      ],
      // добавление проперти #states для радио-кнопок
      '#states' => [
        'enabled' => [
          // включение радио-кнопок только если текстовое поле кастомного цвета пустое
          ':input[name="field_custom_colour"]' => ['value' => ''],
        ],
      ],
    ];

    //this textfield will only be shown when the option 'Other'
    //is selected from the radios above.
    $form['custom_colour'] = [
      '#type' => 'textfield',
      '#size' => '60',
      '#placeholder' => 'Enter favourite colour',
      '#attributes' => [
        //also add static name and id to the textbox
        'id' => 'custom-colour',
        'name' => 'field_custom_colour',
      ],
      '#states' => [
        //show this textfield only if the radio 'other' is selected above
        'visible' => [
          ':input[name="field_select_colour"]' => ['value' => 'other'],
        ],
      ],
    ];

    //create the submit button
    $form['submit'] = [
      '#type' => 'submit',
      '#value' => $this->t('Sorry, I\\'m colour-blind'),
    ];

    return $form;

  }
```

# См. также

[AJAX Forms](https://www.drupal.org/docs/drupal-apis/javascript-api/ajax-forms)

Использование обратных вызовов AJAX для динамического реагирования на ввод данных пользователем, изменения элементов формы, отображения диалоговых окон или запуска пользовательских функций Javascript.

[Form Render Elements](https://www.drupal.org/docs/drupal-apis/form-api/form-render-elements)

HTML5 и другие полезные рендер элементы формы.

{ - done - 2021-02-18 }