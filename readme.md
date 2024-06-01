# Todo List
Es un proyecto en el cual puedes agregar y eliminar 
tareas, cuenta también con validación de inputs
para que todas las casillas sean llenadas, esto
está desarrollado con  html, css y js.

## Estructura
- `index.html`: Contiene la estructura básica del HTML.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Formulario</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header class="header">
        <h1>Todo List</h1>
    </header>
    <main class="container">
        <article class="articleItems">
            <h2 class="title">Formulario</h2>
            <form id="formulario" class="formContainer">
                <div class="inputContainer">
                    <label for="task">Nombre del task:</label>
                    <input class="input" type="text" name="task" id="task">
                </div>
                <div class="inputContainer">
                    <label for="autor">Autor:</label>
                    <input class="input" type="text" name="autor" id="autor">
                </div>
                <input type="submit" class="inputSubmit" value="Enviar">
            </form>
            <div id="message"></div>
        </article>
        <aside class="asideItems">
            <h2 class="title">Listado de tareas</h2>
            <div id="containerList"></div>
        </aside>
    </main>
    <script src="/app.js"></script>
</body>
</html>
```


- `style.css`: Contiene los estilos para la aplicación.
```css
*,
::before,
::after {
    box-sizing: border-box;
}

body {
    background-color: black;
    color: white;
}

.container {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    gap: 80px;
    flex-wrap: wrap;
}

.formContainer {
    display: flex;
    flex-direction: column;
    gap: 20px;
    align-items: center;
}

.articleItems {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    border-radius: 20px;
    border: 1px red solid;
    padding: 10px;
}

.asideItems {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    border-radius: 20px;
    border: 1px red solid;
    padding: 10px;
    min-width: 200px;
}

.inputSubmit {
    width: 120px;
    background-color: blue;
    border-radius: 10px;
    padding: 10px;
}

.inputContainer {
    justify-content: space-between;
    display: flex;
    width: 320px;
}

.title {
    margin-top: 0;
}

.header {
    display: flex;
    justify-content: center;
}

.input {
    background-color: white;
    color: black;
}

.listItem {
    padding: 10px;
    min-width: 200px;
}

.itemContent {
    display: flex;
    justify-content: space-between;
    gap: 10px;
    align-items: center;
}

.deleteButton {
    width: 120px;
    background-color: red;
    border-radius: 10px;
    padding: 5px;
}

```

- `app.js`: Contiene la lógica de JavaScript para manejar la funcionalidad de la aplicación.
```js
let taskName = '';
let taskAutor = '';

const arrayTask = [];

const formValues = {
    'taskName': '',
    'taskAutor': '',
}

const taskNameInputElement = document.getElementById('task');
const taskAutorInputElement = document.getElementById('autor');
const taskFormElement = document.getElementById('formulario');
const containerListElement = document.getElementById('containerList');
const containerMessageElement = document.getElementById('message');

const clearInputStyle = (keyItem) => {
    switch (keyItem) {
        case 'taskName': taskNameInputElement.style.borderColor = 'black'; break;
        default : taskAutorInputElement.style.borderColor = 'black'; break;
    }
}

const taskChangeValues = (e, keyItem) => {
    clearInputStyle(keyItem)
    formValues[keyItem] = e.target.value
}

const validateInfo = () => {
    console.log({ formValues })

    return Object.values(formValues).every(value => !!value && value.trim() !== '');
}

const clearForm = () => {
    console.log('clear')
    taskFormElement.reset()
    formValues.taskName = '';
    formValues.taskAutor = '';
    containerMessageElement.innerHTML = '';
}

const deleteTask = (index) => {
    arrayTask.splice(index, 1);
    renderTaskList();
}

const renderTaskList = () => {
    console.log({ arrayTask })
    containerListElement.innerHTML = '';
    if (arrayTask.length === 0) {
        containerListElement.innerHTML = '<p>El listado de tareas está vacío</p>';
    } else {
        const ol = document.createElement('ol');
        arrayTask.forEach((task, index) => {
            const li = document.createElement('li');
            li.className = 'listItem';
            
            const div = document.createElement('div');
            div.className = 'itemContent';
            div.innerHTML = `<span>Task: ${task.taskName}, Author: ${task.taskAutor}</span>`;
            
            const deleteButton = document.createElement('button');
            deleteButton.textContent = 'Eliminar';
            deleteButton.className = 'deleteButton';
            deleteButton.addEventListener('click', () => deleteTask(index));
            
            div.appendChild(deleteButton);
            li.appendChild(div);
            ol.appendChild(li);
        });
        containerListElement.appendChild(ol);
    }
}

const updateValues = () => {
    arrayTask.push({ ...formValues });
    renderTaskList();
}

const sendErrorMessage = () => {
    const { taskName, taskAutor } = formValues;
    if (!taskName) {
        taskNameInputElement.style.borderColor = 'red';
    }
    if (!taskAutor) {
        taskAutorInputElement.style.borderColor = 'red';
    }

    containerMessageElement.innerHTML = '<p>Debes llenar todas las casillas</p>';
}

const submitElements = (e) => {
    e.preventDefault();
    const isValid = validateInfo();
    if (isValid) {
        updateValues();
        clearForm();
    } else {
        sendErrorMessage();
    }
}

taskNameInputElement.addEventListener('input', (e) => taskChangeValues(e, 'taskName'));
taskAutorInputElement.addEventListener('input', (e) => taskChangeValues(e, 'taskAutor'));
taskFormElement.addEventListener('submit', submitElements);

renderTaskList();

```


## Funciones
Se agregaron las siguientes funciones en el aplicativo

1. Limpiar los estilos del input una vez se hayan enviado los datos del formulario
```js
const clearInputStyle = (keyItem) => {
    switch (keyItem) {
        case 'taskName': taskNameInputElement.style.borderColor = 'black'; break;
        default : taskAutorInputElement.style.borderColor = 'black'; break;
    }
}
```

2.  La función de llenado de los inputs que reinicia los estilos
del input cuando estas cambiando el texto, esto ayuda tmb para
eliminar el borde cuando hay un error en el input
```js
const taskChangeValues = (e, keyItem) => {
    clearInputStyle(keyItem)
    formValues[keyItem] = e.target.value
}
```

3. Validación de inputs para verificar que todos los inputs estén
llenados

```js 
const validateInfo = () => {
    return Object.values(formValues).every(value => !!value && value.trim() !== '');
}
```

4. Limpiar los datos del input para resetearlos y poder escribir otra tarea
```js
const clearForm = () => {
    taskFormElement.reset()
    formValues.taskName = '';
    formValues.taskAutor = '';
    containerMessageElement.innerHTML = '';
}

```

5. Función para borrar una tarea que ya fue registrada
```js
const deleteTask = (index) => {
    arrayTask.splice(index, 1);
    renderTaskList();
}
```

6. Función para crear la lista de tareas o mostrar un mensaje
cuando no hay tareas registradas
```js
const renderTaskList = () => {
    containerListElement.innerHTML = '';
    if (arrayTask.length === 0) {
        containerListElement.innerHTML = '<p>El listado de tareas está vacío</p>';
    } else {
        const ol = document.createElement('ol');
        arrayTask.forEach((task, index) => {
            const li = document.createElement('li');
            li.className = 'listItem';
            
            const div = document.createElement('div');
            div.className = 'itemContent';
            div.innerHTML = `<span>Task: ${task.taskName}, Author: ${task.taskAutor}</span>`;
            
            const deleteButton = document.createElement('button');
            deleteButton.textContent = 'Eliminar';
            deleteButton.className = 'deleteButton';
            deleteButton.addEventListener('click', () => deleteTask(index));
            
            div.appendChild(deleteButton);
            li.appendChild(div);
            ol.appendChild(li);
        });
        containerListElement.appendChild(ol);
    }
}
```

7. Función para actualizar las tareas y llamar al renderizado de los items
```js
const updateValues = () => {
    arrayTask.push({ ...formValues });
    renderTaskList();
}
```

8. Función para mensaje de error cuando no se hayan llenado los inputs
```js
const sendErrorMessage = () => {
    const { taskName, taskAutor } = formValues;
    if (!taskName) {
        taskNameInputElement.style.borderColor = 'red';
    }
    if (!taskAutor) {
        taskAutorInputElement.style.borderColor = 'red';
    }

    containerMessageElement.innerHTML = '<p>Debes llenar todas las casillas</p>';
}
```

9. Función para enviar los datos al contenedor de listado de tareas
```js
const submitElements = (e) => {
    e.preventDefault();
    const isValid = validateInfo();
    if (isValid) {
        updateValues();
        clearForm();
    } else {
        sendErrorMessage();
    }
}
```