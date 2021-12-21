# 'Tutorial' {formik}

[https://formik.org/docs/tutorial](https://formik.org/docs/tutorial)

October 3, 2021

# Перед тем как начать

Добро пожаловать в учебник по Formik. Здесь вы узнаете все, что нужно знать для создания простых и сложных форм в React.

Если вы нетерпеливы и хотите просто начать локально хакать на своей машине, обратитесь к [60-секундный быстрый старт](https://formik.org/docs/tutorial#installation).

## = Что мы строим?

В этом уроке мы построим сложную форму подписки на рассылку с помощью React и Formik.

Вы можете посмотреть, что мы построим в итоге здесь: [Final Result](https://codesandbox.io/s/formik-v2-tutorial-final-ge1pt). Если код для вас непонятен, не волнуйтесь! Цель этого урока - помочь вам понять Formik.

## = Предусловия

Для полного понимания Formik и его работы вам потребуется знание HTML, CSS, [современного JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript) и [React](https://reactjs.org/) (и [React Hooks](https://reactjs.org/docs/hooks-intro.html)). В этом учебнике мы используем [стрелочные функции](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), [let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), [const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const), [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax), [destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment), [computed property names](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Computed_property_names) и [async/await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) . Вы можете использовать [Babel REPL](https://babeljs.io/repl/#?presets=react&code_lz=MYewdgzgLgBApgGzgWzmWBeGAeAFgRgD4AJRBEAGhgHcQAnBAEwEJsB6AwgbgChRJY_KAEMAlmDh0YWRiGABXVOgB0AczhQAokiVQAQgE8AkowAUAcjogQUcwEpeAJTjDgUACIB5ALLK6aRklTRBQ0KCohMQk6Bx4gA), чтобы проверить, во что компилируется код ES6.

# Настройки для учебника

Этот учебник можно пройти двумя способами: либо писать код в браузере, либо создать локальную среду разработки на своем компьютере.

## = Опция **1: Писать код в браузере**

Это самый быстрый способ начать работу!

Сначала откройте этот [Starter Code](https://codesandbox.io/s/formik-v2-tutorial-start-s04yr) в новой вкладке. В новой вкладке должны появиться ввод адреса электронной почты, кнопка отправки и некоторый код React. В этом уроке мы будем редактировать код React.

Пропустите второй вариант настройки и перейдите к разделу [Обзор](https://formik.org/docs/tutorial#overview-what-is-formik), чтобы получить общее представление о Formik.

## = Опция **2: Локальная среда разработки**

Это совершенно необязательно и не требуется для данного урока!

- **Optional: Instructions for following along locally using your preferred text editor**

  This setup requires more work, but allows you to complete the tutorial using an editor of your choice. Here are the steps to follow:

    1. Make sure you have a recent version of **[Node.js](https://nodejs.org/en/)** installed.
    2. Follow the **[installation instructions for Create React App](https://create-react-app.dev/)** to make a new project.

    ```
     npx create-react-app my-app
    ```

    1. Install Formik

    ```
     npm i formik
    ```

  Or

    ```
     yarn add formik
    ```

    1. Delete all files in the `src/` folder of the new project

  > Note:Don’t delete the entire src folder, just the original source files inside it. We’ll replace the default source files with examples for this project in the next step.
  >

    ```
    cd my-app
    cd src
    
    # If you’re using a Mac or Linux:
    rm -f *
    
    # Or, if you’re on Windows:
    del *
    
    # Then, switch back to the project folder
    cd ..
    ```

    1. Add a file named `styles.css` in the `src/` folder with **[this CSS code](https://codesandbox.io/s/formik-v2-tutorial-start-s04yr?file=/src/styles.css)**.
    2. Add a file named `index.js` in the `src/` folder with **[this JS code](https://codesandbox.io/s/formik-v2-tutorial-start-s04yr?file=/src/index.js:0-759)**.

  Now run `npm start` in the project folder and open `http://localhost:3000` in the browser. You should see an email input and a submit button.

  We recommend following **[these instructions](https://babeljs.io/docs/editors/)** to configure syntax highlighting for your editor.


## = Помогите, я застрял!

Если вы застряли, загляните на [GitHub Discussions](https://github.com/formik/formik/discussions) компании Formik. Кроме того, [Formium Community Discord Server](https://discord.gg/pJSg287) также является отличным способом быстро получить помощь. Если вы не получили ответа или застряли, пожалуйста, оформите проблему, и мы вам поможем.

# Обзор: Что такое Formik?

Formik - это небольшая группа компонентов React и хуков для создания форм в React и React Native. Она помогает справиться с тремя наиболее раздражающими моментами:

1. Получение значений в и из состояния формы
2. Валидация и сообщения об ошибках
3. Обработка отправки формы

Размещая все вышеперечисленное в одном месте, Formik позволяет упорядочить все - тестирование, рефакторинг и понимание ваших форм становится простым делом.

# Основы

Мы начнем с самого подробного способа использования Formik. Хотя это может показаться немного затянутым, важно увидеть, как Formik строится на самом себе, чтобы у вас было полное представление о том, какие есть возможности, и полная мысленная модель того, как это работает.

## = Простая форма подписки на новости

Представьте, что мы хотим добавить форму подписки на рассылку новостей для блога. Для начала наша форма будет иметь только одно поле с именем `email`. С Formik это всего лишь несколько строк кода.

```jsx
 import React from 'react';
 import { useFormik } from 'formik';
 
 const SignupForm = () => {
   // передаём хуку useFormik() начальные значения формы и функцию submit, 
   // которая будет вызвана при отправке формы
   const formik = useFormik({
     initialValues: {
       email: '',
     },
     onSubmit: values => {
       alert(JSON.stringify(values, null, 2));
     },
   });
   return (
     <form onSubmit={formik.handleSubmit}>
       <label htmlFor="email">Email Address</label>
       <input
         id="email"
         name="email"
         type="email"
         onChange={formik.handleChange}
         value={formik.values.email}
       />
 
       <button type="submit">Submit</button>
     </form>
   );
 };
```

Мы передаем `initialValues` нашей формы и функцию отправки (`onSubmit`) хуку `useFormik()`. Затем хук возвращает нам объект с состоянием формы и вспомогательными методами в переменной, которую мы назвали `formik`. На данный момент нас интересуют только следующие вспомогательные методы:

- `handleSubmit`: Обработчик отправки
- `handleChange`: Обработчик изменений, передаваемый каждому `<input>`, `<select>` или `<textarea>`
- `values`: Текущие значения нашей формы

Как вы можете видеть выше, мы передаем каждое из этих значений соответствующим `props...` и это все! Теперь у нас есть рабочая форма, работающая на Formik. Вместо того чтобы самостоятельно управлять значениями нашей формы и писать собственные обработчики событий для каждого ввода, мы можем просто использовать `useFormik()`.

Это довольно здорово, но при использовании только одного единственного input'а преимущества использования `useFormik()` неочевидны. Поэтому давайте добавим еще два инпута: по одному для имени и фамилии пользователя, которые мы будем хранить в форме как `firstName` и `lastName`.

```jsx
 import React from 'react';
 import { useFormik } from 'formik';
 
 const SignupForm = () => {
   // Обратите внимание, что мы должны инициализировать ВСЕ поля значениями. 
   // Они могут быть получены из props, но поскольку мы не хотим заполнять 
   // форму, мы просто используем пустую строку. Если мы этого не сделаем, 
   // React "наорет" на нас.
   const formik = useFormik({
     initialValues: {
       firstName: '',
       lastName: '',
       email: '',
     },
     onSubmit: values => {
       alert(JSON.stringify(values, null, 2));
     },
   });
   return (
     <form onSubmit={formik.handleSubmit}>
       <label htmlFor="firstName">First Name</label>
       <input
         id="firstName"
         name="firstName"
         type="text"
         onChange={formik.handleChange}
         value={formik.values.firstName}
       />
 
       <label htmlFor="lastName">Last Name</label>
       <input
         id="lastName"
         name="lastName"
         type="text"
         onChange={formik.handleChange}
         value={formik.values.lastName}
       />
 
       <label htmlFor="email">Email Address</label>
       <input
         id="email"
         name="email"
         type="email"
         onChange={formik.handleChange}
         value={formik.values.email}
       />
 
       <button type="submit">Submit</button>
     </form>
   );
 };
```

Если вы внимательно посмотрите на наш новый код, то заметите некоторые закономерности и симметрию *формирования*.

1. Мы повторно используем одну и ту же функцию обработчика изменений `handleChange` для каждого HTML-инпута
2. Мы передаем HTML-атрибуты `id` и `name`, которые *соответствуют* свойству, определенному в `initialValues`.
3. Мы получаем доступ к значению поля, используя то же имя (`email` -> `formik.values.email`).

Если вы знакомы с построением форм с помощью обычного React, вы можете представить, что `handleChange` в Formik работает следующим образом:

```jsx
 const [values, setValues] = React.useState({});
 
 const handleChange = event => {
   setValues(prevValues => ({
     ...prevValues,
     // мы используем name, чтобы указать Formik, какой ключ в 
     // `values` нужно обновить
     [event.target.name]: event.target.value
   });
 }
```

# Валидация

Хотя наша контактная форма работает, она не совсем полноценна; пользователи могут отправить ее, но она не сообщает им, какие поля (если таковые имеются) являются обязательными.

Если мы не против использования встроенной в браузер проверки HTML-ввода, мы можем добавить параметр `required` к каждому из наших input, указать минимальную/максимальную длину (`maxlength` и `minlength`), и/или добавить параметр `pattern` для проверки регексом для каждого из этих input. Отличный что мы можем делать это. Однако HTML-валидация имеет свои ограничения. Во-первых, она работает только в браузере! Так что это явно не подходит для React Native. Во-вторых, трудно/невозможно показать пользователю пользовательские сообщения об ошибках. В-третьих, он очень неудобен.

Как упоминалось ранее, Formik контролирует не только `values` вашей формы, но и ее валидацию и сообщения об ошибках. Чтобы добавить валидацию с помощью JS, давайте укажем пользовательскую функцию валидации и передадим ее как `validate` в хук `useFormik()`. Если существует ошибка, эта пользовательская функция валидации должна создать объект `error` вида соответствующего нашим `values`/`initialValues`. Снова ... *симметрия*... да да...

```jsx
 import React from 'react';
 import { useFormik } from 'formik';
 
 // Пользовательская функция валидации. Она должна возвращать объект 
 // ключи которого симметричны нашим values/initialValues
 const validate = values => {
   const errors = {};
   if (!values.firstName) {
     errors.firstName = 'Required';
   } else if (values.firstName.length > 15) {
     errors.firstName = 'Must be 15 characters or less';
   }
 
   if (!values.lastName) {
     errors.lastName = 'Required';
   } else if (values.lastName.length > 20) {
     errors.lastName = 'Must be 20 characters or less';
   }
 
   if (!values.email) {
     errors.email = 'Required';
   } else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
     errors.email = 'Invalid email address';
   }
 
   return errors;
 };
 
 const SignupForm = () => {
   // Передаем хуку useFormik() исходные значения формы, функцию validate, 
   // которая будет вызываться при изменении значений формы или блюре полей, 
   // и функцию submit, которая будет вызываться при отправке формы
   const formik = useFormik({
     initialValues: {
       firstName: '',
       lastName: '',
       email: '',
     },
     validate,
     onSubmit: values => {
       alert(JSON.stringify(values, null, 2));
     },
   });
   return (
     <form onSubmit={formik.handleSubmit}>
       <label htmlFor="firstName">First Name</label>
       <input
         id="firstName"
         name="firstName"
         type="text"
         onChange={formik.handleChange}
         value={formik.values.firstName}
       />
       {formik.errors.firstName ? <div>{formik.errors.firstName}</div> : null}
 
       <label htmlFor="lastName">Last Name</label>
       <input
         id="lastName"
         name="lastName"
         type="text"
         onChange={formik.handleChange}
         value={formik.values.lastName}
       />
       {formik.errors.lastName ? <div>{formik.errors.lastName}</div> : null}
 
       <label htmlFor="email">Email Address</label>
       <input
         id="email"
         name="email"
         type="email"
         onChange={formik.handleChange}
         value={formik.values.email}
       />
       {formik.errors.email ? <div>{formik.errors.email}</div> : null}
 
       <button type="submit">Submit</button>
     </form>
   );
 };
```

`formik.errors` заполняется через пользовательскую функцию валидации. По умолчанию Formik будет проверять валидность после каждого нажатия клавиши (событие изменения), после каждого события [blur](https://developer.mozilla.org/en-US/docs/Web/API/Element/blur_event), а также перед отправкой. Функция `onSubmit`, которую мы передали в `useFormik()`, будет выполнена только при отсутствии ошибок (т.е. если наша функция `validate` вернет `{}`).

# Посещённые поля

Хотя наша форма работает, и наши пользователи видят каждую ошибку, это не лучший пользовательский опыт для них. Поскольку наша функция валидации работает при каждом нажатии клавиши по отношению ко *всем* значениям формы, наш объект `errors` содержит *все* ошибки валидации в любой момент времени. В нашем компоненте мы просто проверяем, существует ли ошибка, а затем немедленно показываем ее пользователю. Это неудобно, поскольку мы будем показывать сообщения об ошибках для полей, которые пользователь еще даже не посетил. В большинстве случаев мы хотим показать сообщение об ошибке только после того, как пользователь закончит вводить текст в поле.

Аналогично `errors` и `values`, Formik отслеживает, какие поля были посещены. Он хранит эту информацию в объекте под названием `touched`, который также повторяет формат `values`/`initialValues`. Ключами `touched` являются имена полей, а значениями `touched` являются булевы `true`/`false`.

Чтобы воспользоваться преимуществами `touched`, мы передаем `formik.handleBlur` в пропс `onBlur` каждого инпута. Эта функция работает аналогично `formik.handleChange`, используя атрибут `name`, чтобы определить, какое поле нужно обновить.

```jsx
 import React from 'react';
 import { useFormik } from 'formik';
 
 const validate = values => {
   const errors = {};
 
   if (!values.firstName) {
     errors.firstName = 'Required';
   } else if (values.firstName.length > 15) {
     errors.firstName = 'Must be 15 characters or less';
   }
 
   if (!values.lastName) {
     errors.lastName = 'Required';
   } else if (values.lastName.length > 20) {
     errors.lastName = 'Must be 20 characters or less';
   }
 
   if (!values.email) {
     errors.email = 'Required';
   } else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
     errors.email = 'Invalid email address';
   }
 
   return errors;
 };
 
 const SignupForm = () => {
   const formik = useFormik({
     initialValues: {
       firstName: '',
       lastName: '',
       email: '',
     },
     validate,
     onSubmit: values => {
       alert(JSON.stringify(values, null, 2));
     },
   });
   return (
     <form onSubmit={formik.handleSubmit}>
       <label htmlFor="firstName">First Name</label>
       <input
         id="firstName"
         name="firstName"
         type="text"
         onChange={formik.handleChange}
         onBlur={formik.handleBlur}
         value={formik.values.firstName}
       />
       {formik.errors.firstName ? <div>{formik.errors.firstName}</div> : null}
 
       <label htmlFor="lastName">Last Name</label>
       <input
         id="lastName"
         name="lastName"
         type="text"
         onChange={formik.handleChange}
         onBlur={formik.handleBlur}
         value={formik.values.lastName}
       />
       {formik.errors.lastName ? <div>{formik.errors.lastName}</div> : null}
 
       <label htmlFor="email">Email Address</label>
       <input
         id="email"
         name="email"
         type="email"
         onChange={formik.handleChange}
         onBlur={formik.handleBlur}
         value={formik.values.email}
       />
       {formik.errors.email ? <div>{formik.errors.email}</div> : null}
 
       <button type="submit">Submit</button>
     </form>
   );
 };
```

Почти готово! Теперь, когда мы отслеживаем `touched`, мы можем изменить логику отображения сообщений об ошибках, чтобы показывать сообщение об ошибке данного поля только в том случае, если оно существует и если пользователь посетил это поле.

```jsx
 import React from 'react';
 import { useFormik } from 'formik';
 
 const validate = values => {
   const errors = {};
 
   if (!values.firstName) {
     errors.firstName = 'Required';
   } else if (values.firstName.length > 15) {
     errors.firstName = 'Must be 15 characters or less';
   }
 
   if (!values.lastName) {
     errors.lastName = 'Required';
   } else if (values.lastName.length > 20) {
     errors.lastName = 'Must be 20 characters or less';
   }
 
   if (!values.email) {
     errors.email = 'Required';
   } else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
     errors.email = 'Invalid email address';
   }
 
   return errors;
 };
 
 const SignupForm = () => {
   const formik = useFormik({
     initialValues: {
       firstName: '',
       lastName: '',
       email: '',
     },
     validate,
     onSubmit: values => {
       alert(JSON.stringify(values, null, 2));
     },
   });
   return (
     <form onSubmit={formik.handleSubmit}>
       <label htmlFor="firstName">First Name</label>
       <input
         id="firstName"
         name="firstName"
         type="text"
         onChange={formik.handleChange}
         onBlur={formik.handleBlur}
         value={formik.values.firstName}
       />
       {formik.touched.firstName && formik.errors.firstName ? (
         <div>{formik.errors.firstName}</div>
       ) : null}
 
       <label htmlFor="lastName">Last Name</label>
       <input
         id="lastName"
         name="lastName"
         type="text"
         onChange={formik.handleChange}
         onBlur={formik.handleBlur}
         value={formik.values.lastName}
       />
       {formik.touched.lastName && formik.errors.lastName ? (
         <div>{formik.errors.lastName}</div>
       ) : null}
 
       <label htmlFor="email">Email Address</label>
       <input
         id="email"
         name="email"
         type="email"
         onChange={formik.handleChange}
         onBlur={formik.handleBlur}
         value={formik.values.email}
       />
       {formik.touched.email && formik.errors.email ? (
         <div>{formik.errors.email}</div>
       ) : null}
 
       <button type="submit">Submit</button>
     </form>
   );
 };
```

## = Схема валидации с помощью **Yup**

Как вы можете видеть выше, валидация остается на ваше усмотрение. Не стесняйтесь писать свои собственные валидаторы или использовать сторонние библиотеки-помощники. Авторы Formik и большая часть его пользователей используют библиотеку [Jason Quense](https://github.com/jquense) [Yup](https://github.com/jquense/yup) для валидации схемы объекта. Yup имеет API, схожий с [Joi](https://github.com/hapijs/joi) и [React PropTypes](https://github.com/facebook/prop-types), но при этом достаточно маленький для браузера и достаточно быстрый для использования во время выполнения. Вы можете опробовать его здесь с помощью этого [REPL](https://runkit.com/jquense/yup).

Поскольку авторы/пользователи Formik так любят Yup, в Formik есть специальная конфигурация Yup под названием `validationSchema`, которая автоматически преобразует сообщения Yup об ошибках валидации в красивый объект, ключи которого соответствуют `values`/`initialValues`/`touched` (точно так же, как это должна делать любая пользовательская функция валидации). Итак, вы можете установить Yup из NPM/yarn следующим образом...

```
 npm install yup --save
 
 # или с помощью yarn
 
 yarn add yup
```

Чтобы увидеть, как работает Yup, давайте избавимся от нашей пользовательской функции валидации `validate` и перепишем нашу валидацию с помощью Yup и `validationSchema`:

```jsx
 import React from 'react';
 import { useFormik } from 'formik';
 import * as Yup from 'yup';
 
 const SignupForm = () => {
   const formik = useFormik({
     initialValues: {
       firstName: '',
       lastName: '',
       email: '',
     },
     validationSchema: Yup.object({
       firstName: Yup.string()
         .max(15, 'Must be 15 characters or less')
         .required('Required'),
       lastName: Yup.string()
         .max(20, 'Must be 20 characters or less')
         .required('Required'),
       email: Yup.string().email('Invalid email address').required('Required'),
     }),
     onSubmit: values => {
       alert(JSON.stringify(values, null, 2));
     },
   });
   return (
     <form onSubmit={formik.handleSubmit}>
       <label htmlFor="firstName">First Name</label>
       <input
         id="firstName"
         name="firstName"
         type="text"
         onChange={formik.handleChange}
         onBlur={formik.handleBlur}
         value={formik.values.firstName}
       />
       {formik.touched.firstName && formik.errors.firstName ? (
         <div>{formik.errors.firstName}</div>
       ) : null}
 
       <label htmlFor="lastName">Last Name</label>
       <input
         id="lastName"
         name="lastName"
         type="text"
         onChange={formik.handleChange}
         onBlur={formik.handleBlur}
         value={formik.values.lastName}
       />
       {formik.touched.lastName && formik.errors.lastName ? (
         <div>{formik.errors.lastName}</div>
       ) : null}
 
       <label htmlFor="email">Email Address</label>
       <input
         id="email"
         name="email"
         type="email"
         onChange={formik.handleChange}
         onBlur={formik.handleBlur}
         value={formik.values.email}
       />
       {formik.touched.email && formik.errors.email ? (
         <div>{formik.errors.email}</div>
       ) : null}
 
       <button type="submit">Submit</button>
     </form>
   );
 };
```

И снова, Yup является 100% опциональным. Тем не менее, мы предлагаем попробовать. Как вы можете видеть выше, мы выразили точно такую же функцию валидации всего 10 строками кода вместо 30. Один из основных принципов дизайна Formik - помочь вам оставаться организованными. Yup определенно помогает в этом - схемы очень выразительны, интуитивно понятны (поскольку они отражают ваши значения) и могут использоваться повторно. Независимо от того, используете вы Yup или нет, мы настоятельно рекомендуем вам использовать общие методы валидации в вашем приложении. Это обеспечит последовательную проверку общих полей (например, электронной почты, уличных адресов, имен пользователей, номеров телефонов и т.д.) и приведет к улучшению пользовательского опыта.

# Уменьшение шаблонного кода

## = **`getFieldProps()`**

Приведенный выше код очень четко определяет, что именно делает Formik. `onChange` -> `handleChange`, `onBlur` -> `handleBlur`, и так далее. Однако, чтобы сэкономить ваше время, `useFormik()` возвращает вспомогательный метод `formik.getFieldProps()`, чтобы ускорить подключение инпутов. Получив некоторую информацию на уровне поля, он возвращает вам точную группу `onChange`, `onBlur`, `value`, `checked` для данного поля. Затем вы можете распространить их на `input`, `select` или `textarea`.

```jsx
 import React from 'react';
 import { useFormik } from 'formik';
 import * as Yup from 'yup';
 
 const SignupForm = () => {
   const formik = useFormik({
     initialValues: {
       firstName: '',
       lastName: '',
       email: '',
     },
     validationSchema: Yup.object({
       firstName: Yup.string()
         .max(15, 'Must be 15 characters or less')
         .required('Required'),
       lastName: Yup.string()
         .max(20, 'Must be 20 characters or less')
         .required('Required'),
       email: Yup.string().email('Invalid email address').required('Required'),
     }),
     onSubmit: values => {
       alert(JSON.stringify(values, null, 2));
     },
   });
   return (
     <form onSubmit={formik.handleSubmit}>
       <label htmlFor="firstName">First Name</label>
       <input
         id="firstName"
         type="text"
         {...formik.getFieldProps('firstName')}
       />
       {formik.touched.firstName && formik.errors.firstName ? (
         <div>{formik.errors.firstName}</div>
       ) : null}
 
       <label htmlFor="lastName">Last Name</label>
       <input id="lastName" type="text" {...formik.getFieldProps('lastName')} />
       {formik.touched.lastName && formik.errors.lastName ? (
         <div>{formik.errors.lastName}</div>
       ) : null}
 
       <label htmlFor="email">Email Address</label>
       <input id="email" type="email" {...formik.getFieldProps('email')} />
       {formik.touched.email && formik.errors.email ? (
         <div>{formik.errors.email}</div>
       ) : null}
 
       <button type="submit">Submit</button>
     </form>
   );
 };
```

## = Использование **React Context**

Наш код выше снова очень четко определяет, что именно делает Formik. `onChange` -> `handleChange`, `onBlur` -> `handleBlur`, и так далее. Однако нам все равно придется вручную передавать каждому инпуту этот "prop getter" `getFieldProps()`. Чтобы сэкономить еще больше времени, Formik поставляется с API/компонентами [React Context](https://reactjs.org/docs/context.html), чтобы сделать жизнь проще, а код менее многословным: `<Formik />`, `<Form />`, `<Field />`, и `<ErrorMessage />`. Более явно, они неявно используют React Context для связи с состоянием/методами родительского `<Formik />`.

Поскольку эти компоненты используют React Context, нам необходимо создать [React Context Provider](https://reactjs.org/docs/context.html#contextprovider), который будет хранить состояние нашей формы и хелперы в нашем дереве. Если бы вы делали это самостоятельно, то это выглядело бы следующим образом:

```jsx
 import React from 'react';
 import { useFormik } from 'formik';
 
 // создание пустого контекста
 const FormikContext = React.createContext({});
 
 // помещаем все, что возвращает useFormik, в контекст
 export const Formik = ({ children, ...props }) => {
   const formikStateAndHelpers = useFormik(props);
   return (
     <FormikContext.Provider value={formikStateAndHelpers}>
       {typeof children === 'function'
         ? children(formikStateAndHelpers)
         : children}
     </FormikContext.Provider>
   );
 };
```

К счастью, мы сделали это для вас в компоненте `<Formik>`, который работает точно так же.

Давайте теперь заменим хук `useFormik()` на компонент/render-prop Formik `<Formik>`. Поскольку это компонент, мы преобразуем объект, переданный в `useFormik()`, в JSX, при этом каждый ключ станет пропсом.

```jsx
 import React from 'react';
 import { Formik } from 'formik';
 import * as Yup from 'yup';
 
 const SignupForm = () => {
   return (
     <Formik
       initialValues={{ firstName: '', lastName: '', email: '' }}
       validationSchema={Yup.object({
         firstName: Yup.string()
           .max(15, 'Must be 15 characters or less')
           .required('Required'),
         lastName: Yup.string()
           .max(20, 'Must be 20 characters or less')
           .required('Required'),
         email: Yup.string().email('Invalid email address').required('Required'),
       })}
       onSubmit={(values, { setSubmitting }) => {
         setTimeout(() => {
           alert(JSON.stringify(values, null, 2));
           setSubmitting(false);
         }, 400);
       }}
     >
       {formik => (
         <form onSubmit={formik.handleSubmit}>
           <label htmlFor="firstName">First Name</label>
           <input
             id="firstName"
             type="text"
             {...formik.getFieldProps('firstName')}
           />
           {formik.touched.firstName && formik.errors.firstName ? (
             <div>{formik.errors.firstName}</div>
           ) : null}
 
           <label htmlFor="lastName">Last Name</label>
           <input
             id="lastName"
             type="text"
             {...formik.getFieldProps('lastName')}
           />
           {formik.touched.lastName && formik.errors.lastName ? (
             <div>{formik.errors.lastName}</div>
           ) : null}
 
           <label htmlFor="email">Email Address</label>
           <input id="email" type="email" {...formik.getFieldProps('email')} />
           {formik.touched.email && formik.errors.email ? (
             <div>{formik.errors.email}</div>
           ) : null}
 
           <button type="submit">Submit</button>
         </form>
       )}
     </Formik>
   );
 };
```

Как вы можете видеть выше, мы убрали хук `useFormik()` и заменили его компонентом `<Formik>`. Компонент `<Formik>` принимает функцию в качестве своего дочернего элемента (он же [render prop](https://reactjs.org/docs/render-props.html)). Его аргументом является *точно* такой же объект, возвращаемый `useFormik()` (фактически, `<Formik>` вызывает `useFormik()` внутренне!) Таким образом, наша форма работает так же, как и раньше, только теперь мы можем использовать новые компоненты, чтобы выразить себя более лаконично.

```jsx
 import React from 'react';
 import { Formik, Field, Form, ErrorMessage } from 'formik';
 import * as Yup from 'yup';
 
 const SignupForm = () => {
   return (
     <Formik
       initialValues={{ firstName: '', lastName: '', email: '' }}
       validationSchema={Yup.object({
         firstName: Yup.string()
           .max(15, 'Must be 15 characters or less')
           .required('Required'),
         lastName: Yup.string()
           .max(20, 'Must be 20 characters or less')
           .required('Required'),
         email: Yup.string().email('Invalid email address').required('Required'),
       })}
       onSubmit={(values, { setSubmitting }) => {
         setTimeout(() => {
           alert(JSON.stringify(values, null, 2));
           setSubmitting(false);
         }, 400);
       }}
     >
       <Form>
         <label htmlFor="firstName">First Name</label>
         <Field name="firstName" type="text" />
         <ErrorMessage name="firstName" />
 
         <label htmlFor="lastName">Last Name</label>
         <Field name="lastName" type="text" />
         <ErrorMessage name="lastName" />
 
         <label htmlFor="email">Email Address</label>
         <Field name="email" type="email" />
         <ErrorMessage name="email" />
 
         <button type="submit">Submit</button>
       </Form>
     </Formik>
   );
 };
```

Компонент `<Field>` по умолчанию отображает компонент `<input>`, который, учитывая пропс `name`, неявно получает соответствующие пропсы `onChange`, `onBlur`, `value` и передает их элементу, а также любые пропсы, которые вы передаете ему. Однако, поскольку не все является input, `<Field>` также принимает несколько других props, чтобы позволить вам отобразить все, что вы хотите. Некоторые примеры...

```jsx
 // <input className="form-input" placeHolder="Jane"  />
 <Field name="firstName" className="form-input" placeholder="Jane" />
 
 // <textarea className="form-textarea"/></textarea>
 <Field name="message" as="textarea" className="form-textarea" />
 
 // <select className="my-select"/>
 <Field name="colors" as="select" className="my-select">
   <option value="red">Red</option>
   <option value="green">Green</option>
   <option value="blue">Blue</option>
 </Field>
```

React - это композиция, и хотя мы сократили много [prop-drilling](https://kentcdodds.com/blog/prop-drilling), мы все еще повторяем себя с `label`, `<Field>` и `<ErrorMessage>` для каждого из наших инпутов. Мы можем добиться большего с помощью абстракции! В Formik вы можете и должны создавать многократно используемые примитивные компоненты ввода, которые вы можете распространять по всему вашему приложению. Оказывается, у нашего компонента `<Field>` render-prop есть сестра, и ее зовут `useField`, которая будет делать то же самое, но через React Hooks! Посмотрите...

```jsx
 import React from 'react';
 import ReactDOM from 'react-dom';
 import { Formik, Form, useField } from 'formik';
 import * as Yup from 'yup';
 
 const MyTextInput = ({ label, ...props }) => {
   // useField() возвращает [formik.getFieldProps(), formik.getFieldMeta()], 
   // которые мы можем распространить на <input>. Мы можем использовать мета 
   // поля, чтобы показать сообщение об ошибке, если поле некорректно и оно 
   // было touched (т.е. посещено)
   const [field, meta] = useField(props);
   return (
     <>
       <label htmlFor={props.id || props.name}>{label}</label>
       <input className="text-input" {...field} {...props} />
       {meta.touched && meta.error ? (
         <div className="error">{meta.error}</div>
       ) : null}
     </>
   );
 };
 
 const MyCheckbox = ({ children, ...props }) => {
   // React обрабатывает инпуты типа radios и checkbox иначе, чем другие 
   // типы инпутов, select и textarea. Formik тоже так делает! Когда вы 
   // указываете `type` в useField(), он вернет вам правильный набор 
   // пропсов - пропс `checked` будет включен в `field` наряду с 
   // `name`, `value`, `onChange` и `onBlur`.
   const [field, meta] = useField({ ...props, type: 'checkbox' });
   return (
     <div>
       <label className="checkbox-input">
         <input type="checkbox" {...field} {...props} />
         {children}
       </label>
       {meta.touched && meta.error ? (
         <div className="error">{meta.error}</div>
       ) : null}
     </div>
   );
 };
 
 const MySelect = ({ label, ...props }) => {
   const [field, meta] = useField(props);
   return (
     <div>
       <label htmlFor={props.id || props.name}>{label}</label>
       <select {...field} {...props} />
       {meta.touched && meta.error ? (
         <div className="error">{meta.error}</div>
       ) : null}
     </div>
   );
 };
 
 // и теперь мы можем использовать их
 const SignupForm = () => {
   return (
     <>
       <h1>Subscribe!</h1>
       <Formik
         initialValues={{
           firstName: '',
           lastName: '',
           email: '',
           acceptedTerms: false, // добавится в наш checkbox
           jobType: '', // добавиться в наш select
         }}
         validationSchema={Yup.object({
           firstName: Yup.string()
             .max(15, 'Must be 15 characters or less')
             .required('Required'),
           lastName: Yup.string()
             .max(20, 'Must be 20 characters or less')
             .required('Required'),
           email: Yup.string()
             .email('Invalid email address')
             .required('Required'),
           acceptedTerms: Yup.boolean()
             .required('Required')
             .oneOf([true], 'You must accept the terms and conditions.'),
           jobType: Yup.string()
             .oneOf(
               ['designer', 'development', 'product', 'other'],
               'Invalid Job Type'
             )
             .required('Required'),
         })}
         onSubmit={(values, { setSubmitting }) => {
           setTimeout(() => {
             alert(JSON.stringify(values, null, 2));
             setSubmitting(false);
           }, 400);
         }}
       >
         <Form>
           <MyTextInput
             label="First Name"
             name="firstName"
             type="text"
             placeholder="Jane"
           />
 
           <MyTextInput
             label="Last Name"
             name="lastName"
             type="text"
             placeholder="Doe"
           />
 
           <MyTextInput
             label="Email Address"
             name="email"
             type="email"
             placeholder="jane@formik.com"
           />
 
           <MySelect label="Job Type" name="jobType">
             <option value="">Select a job type</option>
             <option value="designer">Designer</option>
             <option value="development">Developer</option>
             <option value="product">Product Manager</option>
             <option value="other">Other</option>
           </MySelect>
 
           <MyCheckbox name="acceptedTerms">
             I accept the terms and conditions
           </MyCheckbox>
 
           <button type="submit">Submit</button>
         </Form>
       </Formik>
     </>
   );
 };
```

Как вы можете видеть выше, `useField()` дает нам возможность подключить любой вид инпута React компонента к Formik, как если бы это было `<Field>` + `<ErrorMessage>`. Мы можем использовать его для создания группы повторно используемых input-ов, которые соответствуют нашим потребностям.

# Итоги

Поздравляем! Вы создали форму регистрации с помощью Formik, которая:

- Имеет сложную логику валидации и богатые сообщения об ошибках
- Правильно отображает сообщения об ошибках пользователю в нужное время (после блюра поля)
- Использует ваши собственные инпут компоненты, которые вы можете использовать в других формах вашего приложения.

Отличная работа! Мы надеемся, что теперь вы чувствуете, что у вас есть достаточное понимание того, как работает Formik.

Посмотрите на конечный результат здесь: [Final Result](https://codesandbox.io/s/formik-v2-tutorial-final-ge1pt).

Если у вас есть дополнительное время или вы хотите попрактиковаться в освоении Formik, вот несколько идей по улучшению формы регистрации, которые перечислены в порядке возрастания сложности:

- Отключить кнопку отправки, пока пользователь пытается отправить заявку (подсказка: `formik.isSubmitting`)
- Добавьте кнопку сброса с помощью `formik.handleReset` или `<button type="reset">`.
- Предварительное заполнение `initialValues` на основе строки запроса URL или реквизитов, переданных в `<SignupForm>`.
- Изменение цвета границы ввода на красный, когда поле имеет ошибку и не сфокусировано.
- Добавьте анимацию встряхивания для каждого поля, когда оно отображает ошибку и было посещено
- Сохраняйте состояние формы в [sessionStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage) браузера, чтобы прогресс формы сохранялся между обновлениями страницы.

В этом руководстве мы рассмотрели концепции Formik, включая состояние формы, поля, валидацию, хуки, render props и контекст React. Для более подробного объяснения каждой из этих тем ознакомьтесь с остальной частью [документации](https://formik.org/docs/%5B...slug%5D). Чтобы узнать больше об определении компонентов и хуков в учебнике, ознакомьтесь с [API reference](https://formik.org/docs/%5B...slug%5D).

{ - done - 2021-10-03 }
