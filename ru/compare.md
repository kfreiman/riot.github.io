---
layout: ru
title: Сравнение Riot с React и Polymer
---

# **Riot** vs **React** & **Polymer**

Как Riot отличается от похожих проектов.

## React

React и идея "связанности" полсужили фундаментом для Riot. Согласно разработчикам Facebook:

> "Templates separate technologies, not concerns."

Шаблоны разделяют технологии, не ответственность.
Мы придерживаемся этого принципа. Мы стремимся прийти к созданию повторно используемых компонентов, а не шаблонов. Разделяя логику наших интерфейсов и их шаблоны, мы, на самом деле, лишь усложняем себе жизнь.

Объединяя шаблон с его логикой в одном компоненте, мы делаем всю систему чище. Спасибо React за эту идею!

Реакт отлично работает, и мы по-прежнему используем его в некоторых наших проектах. Но мы были обеспокоены размером React и его синтаксисом (*особенно* синтаксисом). Мы стали задумываться о том, что он может быть прощё; не только внутренне, но и для конечного пользователя.

### React синтаксис

Этот пример взят с домашней страницы React:


``` javascript
var TodoList = React.createClass({
  render: function() {
    var createItem = function(itemText) {
      return <li>{itemText}</li>;
    };
    return <ul>{this.props.items.map(createItem)}</ul>;
  }
});
var TodoApp = React.createClass({
  getInitialState: function() {
    return {items: [], text: ''};
  },
  onChange: function(e) {
    this.setState({text: e.target.value});
  },
  handleSubmit: function(e) {
    e.preventDefault();
    var nextItems = this.state.items.concat([this.state.text]);
    var nextText = '';
    this.setState({items: nextItems, text: nextText});
  },
  render: function() {
    return (
      <div>
        <h3>TODO</h3>
        <TodoList items={this.state.items} />
        <form onSubmit={this.handleSubmit}>
          <input onChange={this.onChange} value={this.state.text} />
          <button>{'Add #' + (this.state.items.length + 1)}</button>
        </form>
      </div>
    );
  }
});

React.render(<TodoApp />, mountNode);
```

JSX смешивает HTML и JavaScript. Вы можете использовать HTML где угодно внутри компонента; внутри методов и при определении свойств.


### Riot синтаксис

Теперь тоже самое с Riot:

``` html
<todo>
  <h3>TODO</h3>

  <ul>
    <li each={ item, i in items }>{ item }</li>
  </ul>

  <form onsubmit={ handleSubmit }>
    <input>
    <button>Add #{ items.length + 1 }</button>
  </form>

  this.items = []

  handleSubmit(e) {
    var input = e.target[0]
    this.items.push(input.value)
    input.value = ''
  }
</todo>
```

Этот пользовательский тег монтируется в страницу следующим образом:

``` html
<todo></todo>

<script>riot.mount('todo')</script>
```

### Похоже, но не одно и то же!

В Riot HTML и JavaScript используются гораздо более привычным способом. Оба находятся в одном компоненте, но аккуратно отделены друг от друга. При этом, HTML можно смешивать с выражениями JavaScript.

Мы не используем собственный язык, за исключением выражений в фигурных скобках.

Вам приходится работать с меньшим объёмом кода. Меньше скобок, запятых, системных методов и свойств. Строки можно исползовать как `"Hello {world}"` вместо `"Hello " + this.state.world`, и методы могут быть определны с помощью конпактного ES6 синтаксиса. Во всем - меньше.

Мы считаем, что синтаксис Riot - наиболее чистый способ отделения шаблона и его логики, который позволяет пользоваться всеми преимуществами изолированных, пригодных для повторного испоьзования компонентов.


### Обращения к DOM vs парсинг строк

Когда компонент инициализирован, React парсит строку, а Riot перебирает DOM-дерево.

Riot берёт выражения из DOM и сохраняет их в массив. Каждое выражение имеет указатель на на элемент DOM. При каждом выполнении этого выражения, оно сравнивается со значение в DOM. Когда значение меняется, элемент DOM обновляется. Планируется, что у Riot будет свой собственный виртуальный DOM, как в React, но гораздо проще.

Так как выражение может быть закешировано, цикл обновления очень быстр. Проход 100 или 1000 выражений обычно занимает 1мс и меньше.

Алгоритм React гораздо сложнее из-за того, что HTML может непредвиденно изменяться после каждого обновления. Получив эту проблему, разработчики Facebook проделали впечатляющую работу с ней.

Мы увидели, что сложность, связанную с определеним различий можно измежать.

В Riot HTML стуктура фиксированная. Только в переборах могут появляться и удаляться элементы. Но `div` не может стать `label`, к примеру. Riot обновляет выражения без сложных замен поддеревьев.

### Flux и маршрутизация

React работает только с UI, и это - хорошо. Все хорошие проекты имеют конкретный фокус.

Facebook рекомендует использовать [Flux](http://facebook.github.io/flux/docs/overview.html) для структурирования кода на стороне клиента. Это скорее патерн проектирования, сочетающий хорошии идеи, чем фреймворк.

Riot предоставляется в комплекте с пользовательскими тегами, событийной системой (observable) и маршрутизатором. Мы считаем это минимаьлным набором для создания приложения на клиенте. События привносят модульность, маршрутизатор заботится об URL и кнопке "назад", а пользовательские теги берут на себя UI.

Так же, как Flux, Riot является очень гибок и оставляет основные архитектурные решения для разработчом. Это просто библиотека, которая помогает вам достичь цели.

Вы можете создать Flux-подобную систему, используя observable и router, встроенные в Riot. Вообще-то, такие инструменты [уже есть](https://github.com/jimsparkman/RiotControl).

### 10x - 128x больше

React в 10 раз больше Riot.

<small><em>react.min.js</em> – 119KB</small>
<span class="bar red"></span>

<small><em>riot.min.js</em> – <span class="riot-size">{{ site.size_min }}KB</span></small>
<span class="bar blue" style="width: {{ site.size_min / 119 * 100 }}%"></span>

<br>

Маршрутизатор, рекомендованный React в 128 раз больше, чем в Riot.

<small><em>react-router.min.js</em> – 54.9KB</small>
<span class="bar red"></span>

<small><em>react-mini-router.min.js</em> – 8.6KB</small>
<span class="bar red" style="width: 15.6%"></span>

<small><em>riot.router.min.js</em> – 0.43KB</small>
<span class="bar blue" style="width: 0.7%"></span>

Правда, это сравнение немного несправедливо, ведь у [react-router](https://github.com/rackt/react-router) гораздо больше возможностей. Но представленные выше графики прекрасно иллюстрируют главную цель Riot: предоставить наиболее минималистичный API для работы.

Экосистема React более "фреймворковая" и это правоцирует громоздкие API. Компактная альтернатива [react-mini-router](https://github.com/larrymyers/react-mini-router) не пользуется популярностью в сообществе React.

# Polymer

Polymer взял и стандартные Wev компоненты и сделал их доступными для современных браузеров. Это позволяет вам создавать пользовательские теги в стандартной манере.

В принципе, Riot делает тоже самое, но иначе.

1. Riot обновляет только те элементы, у которых есть изменения, чтобы сократить манипуляции с DOM.

2. Polymer синтаксис более сложный и требует изученияю многих книг.

3. Отдельные компоненты импортируются через HTML `link rel="import"`. Приходится прибегать к последовательным XHR-запросам, что делает Polymer крайне медленным, если не использовать [vulcanize](https://github.com/polymer/vulcanize). Пользовательские теги в Riot импортируются через `script src` и множество тегов могут быть объеденены тем же способом, что и обычные js-файлы.

4. Нет возможности рендеринга на стороне сервера.


### В 11 раз больше

Polymer(v1.0.6) + WebComponents(v0.7.7) в 11 раз больше, чем Riot

<small><em>polymer.min.js</em> – 138KB</small>
<span class="bar red"></span>

<small><em>riot.min.js</em> – <span class="riot-size">{{ site.size_min }}KB</span></small>
<span class="bar blue" style="width: {{ site.size_min / 138 * 100 }}%"></span>

WeВеб-компоненты считаются [основой всех проблем polyfilling](http://developer.telerik.com/featured/web-components-arent-ready-production-yet/). Это главная причина, по которой Polymer нуждается в таком количестве кода.


### Экспериментальный

Polymer основан на экспериментальной технологии. Поддержка нативных Вэб-компонентов не представлена в Safari или IE. В IE они в статусе "на рассмотрении", в Safari - пока не известно. В некоторых [коммитах в WebKit](https://lists.webkit.org/pipermail/webkit-dev/2013-May/024894.html) отмечается, что они не планируют поддерживать стандартные Вэб-компоненты. И Polymer предоставляет полифилы только для _последних версий_ браузеров (IE 10+).


Polymer существует уже [больше двух лет](https://github.com/Polymer/polymer/commit/0452ada044a6fc5818902e685fb07bb4678b2bc2) и не получил за это время существенного признания. Неизвестно, будут ли когда-либо поддерживаться нативные Веб-компоненты.
