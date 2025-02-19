---
layout: fs-post
title: <strong>Array HOF 연습 문제</strong>
categories: fastcampus-exercise
section: fastcampus-exercise
seq: 3
permalink: /:categories/:title
description:
---

* TOC
{:toc}

# 1. html 생성

todos 배열을 인수로 전달받아 html 문자열을 생성해 반환하는 함수를 작성하라.

for 문, for...in 문, for...of 문은 사용하지 않는다.

반환된 html 문자열의 태그 간 공백 문자는 html로서 동일하게 동작한다면 아래 문제의 console.log 출력과 다소 상이해도 무방하다. 예를 들어, 아래 두 html은 동일하게 동작한다.

```html
<li id="3">
  <label><input type="checkbox">HTML</label>
</li>
<li id="2">
  <label><input type="checkbox" checked>CSS</label>
</li>
<li id="1">
  <label><input type="checkbox">Javascript</label>
</li>
```

```html
<li id="3">
          <label><input type="checkbox" >HTML</label>
        </li><li id="2">
          <label><input type="checkbox" checked>CSS</label>
        </li><li id="1">
          <label><input type="checkbox" >Javascript</label>
        </li>
```


```javascript
const todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

const render = todos => { /* Do something */ };
console.log(render(todos));
/*
<li id="3">
  <label><input type="checkbox">HTML</label>
</li>
<li id="2">
  <label><input type="checkbox" checked>CSS</label>
</li>
<li id="1">
  <label><input type="checkbox">Javascript</label>
</li>
*/
```

<!--
const render = todos => {
  let html = '';

  todos.forEach(({ id, content, completed }) => {
    html += `
      <li id="${id}">
        <label><input type="checkbox" ${completed ? 'checked' : ''}>${content}</label>
      </li>`;
  });

  return html;
};

const render = todos =>
  todos
    .map(
      ({ id, content, completed }) => `
        <li id="${id}">
          <label><input type="checkbox" ${completed ? 'checked' : ''}>${content}</label>
        </li>`
    )
    .join('');

const render = todos =>
  todos.reduce(
    (html, { id, content, completed }) =>
      html +
      `<li id="${id}">
        <label><input type="checkbox" ${completed ? 'checked' : ''}>${content}</label>
      </li>`,
    ''
  );
-->

# 2. 특정 프로퍼티 값 추출

todos 배열과 todos 배열의 요소인 todo 객체의 프로퍼티 키(id, content, completed)를 문자열로 전달하면 todos 배열의 요소 중에서 해당 프로퍼티 값만을 추출한 배열을 반환하는 함수를 작성하라.

for 문, for...in 문, for...of 문, Array#forEach는 사용하지 않는다.

```javascript
const todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

const getValues = (todos, key) => { /* Do something */ };

console.log(getValues(todos, 'id')); // [3, 2, 1]
console.log(getValues(todos, 'content')); // ['HTML', 'CSS', 'Javascript']
console.log(getValues(todos, 'completed')); // [false, true, false]
```

<!--
const getValues = (todos, key) => todos.map(todo => todo[key]);
-->

# 3. 프로퍼티 정렬

todos 배열과 todos 배열의 요소인 todo 객체의 프로퍼티 키(id, content, completed)를 문자열로 전달하면 해당 프로퍼티 키로 todos 배열의 요소를 정렬해 반환하는 함수를 작성하라.

for 문, for...in 문, for...of 문, Array#forEach는 사용하지 않는다.

<!-- 참고: [Array.prototype.sort](https://poiemaweb.com/fastcampus/array#91-arrayprototypesort) -->

```javascript
let todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false },
];

const sortBy = (todos, key) => { /* Do something */ };

todos = sortBy(todos, 'id');
console.log(todos);
/*
[
  { id: 1, content: 'Javascript', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'HTML', completed: false }
]
*/
console.log(sortBy(todos, 'content'));
/*
[
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'HTML', completed: false },
  { id: 1, content: 'Javascript', completed: false }
]
*/
console.log(sortBy(todos, 'completed'));
/*
[
  { id: 3, content: 'HTML', completed: false },
  { id: 1, content: 'Javascript', completed: false },
  { id: 2, content: 'CSS', completed: true }
]
*/
```

<!--
const sortBy = (todos, key) =>
  [...todos].sort((todo1, todo2) => (todo1[key] > todo2[key] ? 1 : todo1[key] < todo2[key] ? -1 : 0));
-->

# 4. 새로운 요소 추가

todos 배열과 새로운 요소(예를 들어 `{ id: 4, content: 'Test', completed: false }`)를 인수로 전달하면 todos 배열의 선두에 새로운 요소를 추가해 반환하는 함수를 작성하라.

for 문, for...in 문, for...of 문, Array#forEach, Array#unshift는 사용하지 않는다.

```javascript
let todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false },
];

const addTodo = (todos, newTodo) => { /* Do something */ };

todos = addTodo(todos, { id: 4, content: 'Test', completed: false });
console.log(todos);
/*
[
  { id: 4, content: 'Test', completed: false },
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
]
*/
```

<!--
const addTodo = (todos, newTodo) => [newTodo, ...todos];
-->

# 5. 특정 요소 삭제

todos 배열과 todos 배열에서 삭제할 요소의 id를 인수로 전달하면 해당 요소를 삭제하는 함수를 작성하라. id는 절대 중복되지 않는다.

for 문, for...in 문, for...of 문, Array#forEach, Array#splice는 사용하지 않는다.

```javascript
let todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

const removeTodo = (todos, id) => { /* Do something */ };

todos = removeTodo(todos, 2);
console.log(todos);
/*
[
  { id: 3, content: 'HTML', completed: false },
  { id: 1, content: 'Javascript', completed: false }
]
*/
```

<!--
const removeTodo = (todos, id) => todos.filter(todo => todo.id !== id);
-->

# 6. 특정 요소의 프로퍼티 값 반전

todos 배열과 todos 배열의 특정 요소의 id를 인수로 전달하면 해당 요소의 completed 프로퍼티 값을 반전하는 함수를 작성하라. id는 절대 중복되지 않는다.

for 문, for...in 문, for...of 문, Array#forEach는 사용하지 않는다.

<!-- hint) 기존 객체의 특정 프로퍼티를 변경/추가하여 새로운 객체를 생성하려면 [Object.assign](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 또는 [스프레드 문법](https://poiemaweb.com/fastcampus/spread-syntax#3-객체-리터럴-내부에서-사용하는-경우)을 사용한다. -->

```javascript
let todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

const toggleCompletedById = (todos, id) => { /* Do something */ };

todos = toggleCompletedById(todos, 2);
console.log(todos);
/*
[
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: false },
  { id: 1, content: 'Javascript', completed: false }
]
*/
```

<!--
const toggleCompletedById = (todos, id) =>
  todos.map(todo => (todo.id === id ? { ...todo, completed: !todo.completed } : todo));

const toggleCompletedById = (todos, id) =>
  // eslint-disable-next-line prefer-object-spread
  todos.map(todo => (todo.id === id ? Object.assign({}, todo, { completed: !todo.completed }) : todo));
-->

# 7. 모든 요소의 completed 프로퍼티 값을 true로 설정

todos 배열의 모든 요소의 completed 프로퍼티 값을 true로 설정하는 함수를 작성하라.

for 문, for...in 문, for...of 문, Array#forEach는 사용하지 않는다.

<!-- hint) 기존 객체의 특정 프로퍼티를 변경/추가하여 새로운 객체를 생성하려면 [Object.assign](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 또는 [스프레드 문법](https://poiemaweb.com/fastcampus/spread-syntax#3-객체-리터럴-내부에서-사용하는-경우)을 사용한다. -->

```javascript
let todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

const toggleCompletedAll = todos => { /* Do something */ };

todos = toggleCompletedAll(todos);
console.log(todos);
/*
[
  { id: 3, content: 'HTML', completed: true },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: true }
]
*/
```

<!--
const toggleCompletedAll = todos => todos.map(todo => ({ ...todo, completed: true }));
-->

# 8. completed 프로퍼티의 값이 true인 요소의 갯수 구하기

todos 배열에서 완료(completed: true)한 할일의 갯수를 구하는 함수를 작성하라.

for 문, for...in 문, for...of 문, Array#forEach는 사용하지 않는다.

```javascript
const todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

const countCompletedTodos = todos => { /* Do something */ };

console.log(countCompletedTodos(todos)); // 1
```

<!--
const countCompletedTodos = todos => todos.filter(todo => todo.completed).length;
-->

# 9. id 프로퍼티의 값 중에서 최대값 구하기

todos 배열의 id 프로퍼티 값 중에서 최대값을 구해 반환하는 함수를 작성하라. id는 반드시 양수다. todos 배열은 빈 배열안 경우 0을 반환한다.

for 문, for...in 문, for...of 문, Array#forEach는 사용하지 않는다.

```javascript
const todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false }
];

const getMaxId = todos => { /* Do something */ };

console.log(getMaxId(todos)); // 3
console.log(getMaxId([])); // 0
```

<!--
const getMaxId = todos => Math.max(...todos.map(todo => todo.id), 0);

const getMaxId = todos => (todos.length ? Math.max(...todos.map(todo => todo.id)) : 0);
-->
