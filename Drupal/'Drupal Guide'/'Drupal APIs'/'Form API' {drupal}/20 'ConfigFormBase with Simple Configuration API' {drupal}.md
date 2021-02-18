[](https://www.drupal.org/docs/8/api/form-api/configformbase-with-simple-configuration-api)[https://www.drupal.org/docs/8/api/form-api/configformbase-with-simple-configuration-api](https://www.drupal.org/docs/8/api/form-api/configformbase-with-simple-configuration-api)

Этот тип формы используется для создания форм для страниц конфигурации на вашем Drupal сайте. Вы можете настроить формы конфигурации, которые позволят вам вносить изменения в функционал или представления или другие конфигурационные сущности.

Конфигурационные формы могут значительно помочь вам понять работу Simple Configuration API Drupal 8. В Drupal 8 вы не используете таблицу {variables} и `variable_get/set/delete()`, потому что конфигурация хранится в базе данных и синхронизируется с YML-файлами на диске для целей развертывания. Можно полностью отключить использование базы данных для хранения конфигураций, но в большинстве файловых систем это приводит к снижению производительности.

Simple Configuration API позволяет использовать такие объекты, как объекты $config для связи с YML файлом. Объект $config обрабатывает CRUD (Create/Read/Update/Delete) для YML файлов, поэтому вы просто используете методы `::get()`, `::set()` и `::save()`, и ваши данные будут храниться в файле `{module}.settings.yml`.

# Пример

1.  В файле `your_module.info.yml` вы определяете конфигурационный роут:

```yaml
...
configure: your_module.admin_settings
```

2.  В файле `your_module.routing.yml` , вы определяете сам роут:

```yaml
...
your_module.admin_settings:
  path: '/admin/config/your_module'
  defaults:
    _form: '\\Drupal\\your_module\\Form\\ModuleConfigurationForm'
    _title: 'your_module configuration screen'
  requirements:
    _permission: 'administer site configuration'
```

3.  В файле `your_module/src/Form/ModuleConfigurationForm.php` вы определяете форму:

```php
<?php

namespace Drupal\\your_module\\Form;

use Drupal\\Core\\Form\\ConfigFormBase;
use Drupal\\Core\\Form\\FormStateInterface;

/**
 * Определение формы конфигурирующей настройки модуля формы
 */
class ModuleConfigurationForm extends ConfigFormBase {

  /**
   * {@inheritdoc}
   */
  public function getFormId() {
    return 'your_module_admin_settings';
  }

  /**
   * {@inheritdoc}
   */
  protected function getEditableConfigNames() {
    return [
      'your_module.settings',
    ];
  }

  /**
   * {@inheritdoc}
   */
  public function buildForm(array $form, FormStateInterface $form_state) {
    $config = $this->config('your_module.settings');
    $form['your_message'] = [
      '#type' => 'textfield',
      '#title' => $this->t('Your message'),
      '#default_value' => $config->get('variable_name'),
    ];
    return parent::buildForm($form, $form_state);
  }

  /**
   * {@inheritdoc}
   */
  public function submitForm(array &$form, FormStateInterface $form_state) {
    $this->config('your_module.settings')
      ->set('variable_name', $form_state->getValue('your_message'))
      ->save();
    parent::submitForm($form, $form_state);
  }

}
```

{ - done - 2020-02-18 }