# Ejercicio 2 — CC5002

**Nombre**: Tomás Canales

---

## Pregunta 1: Seguridad en el front-end (6 puntos)

Un compañero le muestra el siguiente código para un foro donde los usuarios pueden publicar comentarios:

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Foro DCC</title>
  <style>
    body { font-family: sans-serif; max-width: 600px; margin: 40px auto; }
    .comentario { border: 1px solid #ccc; padding: 10px; margin: 8px 0; border-radius: 4px; }
    .comentario .autor { font-weight: bold; color: #2c3e50; }
    .comentario .texto { margin-top: 5px; }
  </style>
</head>
<body>
  <h1>Foro DCC</h1>

  <div>
    <label for="autor">Nombre:</label><br>
    <input type="text" id="autor"><br><br>
    <label for="comentario">Comentario:</label><br>
    <textarea id="comentario" rows="3" cols="50"></textarea><br><br>
    <button id="btn-publicar">Publicar</button>
  </div>

  <hr>
  <h2>Comentarios</h2>
  <div id="lista-comentarios"></div>

  <script>
    const btnPublicar = document.getElementById("btn-publicar");
    const inputAutor = document.getElementById("autor");
    const inputComentario = document.getElementById("comentario");
    const lista = document.getElementById("lista-comentarios");

    btnPublicar.addEventListener("click", function() {
      const autor = inputAutor.value;
      const texto = inputComentario.value;

      if (autor === "" || texto === "") {
        alert("Complete todos los campos");
        return;
      }

      lista.innerHTML += `
        <div class="comentario">
          <span class="autor">${autor}</span>
          <div class="texto">${texto}</div>
        </div>
      `;

      inputAutor.value = "";
      inputComentario.value = "";
    });
  </script>
</body>
</html>
```

### 1.1 (2 puntos)

Este código tiene una vulnerabilidad de seguridad conocida como Cross-Site Scripting (XSS).

**a)** Explique en qué consiste esta vulnerabilidad **en el contexto específico de este código**: ¿qué línea(s) causan el problema y por qué?

las lineas que causan este problema consiste entre las lineas 43 y 48 del codigo, puesto a que el innerHTML interpreta cualquier texto como marcado HTML, por lo que permite bypassear la seguridad, esto implica que no escapa ni sanitiza los caracteres reservados, por lo que el navegador no lo trata como texto plano, si no como nodos y atributos del DOM

**b)** Escriba un ejemplo concreto de texto que un usuario malicioso podría ingresar en el campo "Comentario" para demostrar esta vulnerabilidad. Explique qué efecto tendría.

un ejemplo claro y simple es <script>alert('TE HICIERON XSS XDDDD');</script> donde se ejecuta directamente una alerta, violando la seguridad de la aplicacion web

### 1.2 (2 puntos)

Reescriba **solo la sección del código que agrega el comentario a la lista** (el contenido dentro del event listener, después de la validación) para que sea seguro contra XSS. No necesita reescribir todo el archivo.

```js
const div = document.createElement("div"){
div.className = "Comentario";
div.innerHTML = <span class="autor"></span><div class="texto"></div>;

div.querySelector(".autor").textContent = autor;
div.querySelector(".texto").textContent = texto;

lista.appendChild(div)

inputAutor.value = ""
inputTexto.value = ""
```

### 1.3 (2 puntos)

Considere el siguiente fragmento alternativo. ¿Es seguro contra XSS? Justifique su respuesta.

```js
const texto = inputUsuario.value;
const li = document.createElement("li");
li.innerHTML = texto;
document.getElementById("lista").appendChild(li);
```

No es seguro, a pesar de usar createElement y appendChild, el li.innerHTML = texto; permite que se pueda ingresar codigo malicioso a traves del input de los comentarios

---

## Pregunta 2: Eligiendo un framework (6 puntos)

### 2.1 (2 puntos)

En el desarrollo web moderno, los frameworks y librerías de JavaScript (como React, Vue, Angular, Svelte, entre otros) son herramientas fundamentales para construir aplicaciones.

Elija **uno** de estos frameworks o librerías. Investigue brevemente y responda:

**a)** ¿Qué problema concreto de desarrollo web resuelve este framework? No basta con decir "hace el desarrollo más fácil"; identifique un problema específico que enfrentaría al desarrollar con JavaScript vanilla y que el framework aborda.

**b)** Mencione una característica técnica principal del framework que eligió (por ejemplo: componentes, reactividad, sistema de templates, etc.) y explique brevemente cómo funciona.

### 2.2 (4 puntos)

Ahora piense en un proyecto web concreto que le gustaría construir (puede ser real o inventado). Ejemplos: un organizador de horarios, un catálogo de recetas, un chat, un tablero de tareas, una red social para el DCC, etc.

**a)** Describa su proyecto en 3-4 oraciones: qué hace, quién lo usaría, y qué funcionalidades principales tendría.

**b)** Explique por qué el framework que eligió en 2.1 sería una buena opción para este proyecto. Su justificación debe conectar al menos **dos** características o ventajas del framework con necesidades concretas de su aplicación.

**c)** Elegir un framework implica comprometerse con él a largo plazo. Mencione **dos** factores que consideraría antes de tomar esa decisión para un proyecto real (por ejemplo: documentación, comunidad, compatibilidad, curva de aprendizaje, mantenimiento, rendimiento, u otros). Para cada factor, explique brevemente por qué es importante.

---

## Rúbrica resumida

| Criterio | Puntos |
|----------|--------|
| 1.1 — Explicar vulnerabilidad XSS + ejemplo de ataque | 2 |
| 1.2 — Reescribir código de forma segura | 2 |
| 1.3 — Analizar si fragmento alternativo es seguro | 2 |
| 2.1 — Problema que resuelve el framework + característica técnica | 2 |
| 2.2 — Proyecto concreto + justificación + factores de decisión | 4 |
| **Total** | **12** |
