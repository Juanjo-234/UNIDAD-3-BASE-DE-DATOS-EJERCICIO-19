# UNIDAD-3-BASE-DE-DATOS-EJERCICIO-19
Este diagrama entidad-relación (modelo relacional) representa la estructura lógica de un **sistema de financiamiento colectivo o micromecenazgo (crowdfunding)**, diseñado para administrar usuarios, creadores de proyectos, inversores o mecenas, los proyectos publicados, las recompensas ofrecidas y los aportes o contribuciones financieras.

A continuación se detalla qué representa cada tabla y cómo se relacionan entre sí:

### 1. Entidades Principales (Tablas)

* **`USUARIO`**: Almacena la información general de todas las personas registradas en la plataforma.


* *Campos:* `ID_USUARIO` (Clave Primaria), `NOMBRE`, `CORREO`, `ROL` y `DATOS_COBRO`.




* **`CREADOR`**: Subtipo o especialización de usuario que se enfoca en publicar y gestionar iniciativas de financiación.


* *Campos:* `ID_USUARIO` (Clave Primaria y Foránea hacia la tabla `USUARIO`) y `NOMBRE`.




* **`INVERSOR`**: Subtipo o especialización de usuario que aporta fondos a los proyectos.


* *Campos:* `ID_USUARIO` (Clave Primaria y Foránea hacia la tabla `USUARIO`) y `NIVEL_RIESGO`.




* **`PROYECTO`**: Contiene la información de las campañas de crowdfunding creadas en la plataforma.


* *Campos:* `CODIGO_PROYECTO` (Clave Primaria), `TITULO`, `CATEGORIA`, `DESCRIPCION`, `OBJETIVO_FINANCIERO`, `ESTADO`, `FECHA_INICIO`, `FECHA_FIN` e `ID_USUARIO_CREADOR` (Clave Foránea que indica qué usuario creó el proyecto).




* **`RECOMPENSA`**: Define los incentivos o contraprestaciones ofrecidas a los inversores según el monto de su aporte.


* *Campos:* `ID_RECOMPENSA` (Clave Primaria), `TITULO`, `DESCRIPCION`, `VALOR_MINIMO`, `CANTIDAD_MAXIMA` y `CODIGO_PROYECTO` (Clave Foránea).




* **`APORTE`**: Registra cada transacción o contribución económica realizada por un inversor hacia un proyecto y recompensa específica.


* *Campos:* `ID_APORTE` (Clave Primaria), `FECHA`, `HORA`, además de las claves foráneas `ID_USUARIO_INVERSOR`, `ID_RECOMPENSA` y `CODIGO_PROYECTO`.





---

### 2. Relaciones Principales

* **Usuarios y Roles:** La entidad central `USUARIO` se especializa mediante herencia o relación de 1 a 1 en perfiles de `CREADOR` o `INVERSOR` según corresponda.


* **Gestión de Campañas:** Un creador puede publicar múltiples proyectos de financiamiento, y cada proyecto pertenece a un único usuario creador (relación de 1 a N).


* **Recompensas del Proyecto:** Un proyecto ofrece diferentes niveles de `RECOMPENSA` para motivar las donaciones o inversiones.


* **Transacciones (Aportes):** La tabla `APORTE` vincula al inversor con la recompensa seleccionada y el proyecto correspondiente, permitiendo el registro y seguimiento de los fondos recaudados.
<img width="1421" height="819" alt="EJERCICIO 19-MR drawio" src="https://github.com/user-attachments/assets/b164ec51-5d53-4afc-96ce-f5b8e1aa4a3a" />

Este diagrama muestra el **proceso completo de normalización de bases de datos** aplicado a un sistema de financiamiento colectivo o micromecenazgo (*crowdfunding*), transformando una estructura inicial desnormalizada (Forma Normal 0) hasta llegar al diseño relacional optimizado en Tercera Forma Normal (3FN).

A continuación se detalla cada una de las fases representadas:

### 1. Fase Normal 0 (FN0) — Tabla No Normalizada

* Se parte de una **única tabla masiva y plana** que concentra toda la información mezclada en una sola estructura: datos de los proyectos, información del usuario creador y múltiples grupos repetitivos con colecciones de datos anidados dentro de las celdas (como la lista de recompensas ofrecidas y la lista de aportes realizados).


* **Problema:** Contiene grupos multivaluados y datos anidados en las celdas, lo que genera una alta redundancia de información, repetición masiva de registros de usuarios y proyectos, y graves dificultades para realizar operaciones de actualización o consultas eficientes.

---

### 2. Primera Forma Normal (1FN)

* Se eliminan los grupos repetitivos y se asegura que **todos los atributos sean atómicos** (cada celda contiene un único valor), expandiendo los registros para que cada fila represente una combinación individual y libre de valores anidados.


* Se obtiene una tabla tabular plana (`TABLA PLANTILLA`) donde cada campo es indivisible, definiéndose claves primarias compuestas, aunque todavía persiste una alta duplicidad de datos en la información de los usuarios, proyectos y recompensas.



---

### 3. Segunda Forma Normal (2FN)

* Se parte de la 1FN y se eliminan las **dependencias parciales**. Se separan los atributos basándose en sus claves principales correspondientes, desglosando la información en cuatro tablas independientes y normalizadas:


* **`TABLA PROYECTO`**: Almacena las campañas de financiamiento y sus objetivos financieros.


* **`TABLA RECOMPENSA`**: Contiene los incentivos o contraprestaciones vinculados a cada proyecto.


* **`TABLA APORTE`**: Registra las transacciones y contribuciones económicas realizadas.


* **`TABLA USUARIO`**: Gestiona los perfiles registrados en la plataforma y sus datos de cobro.





---

### 4. Tercera Forma Normal (3FN)

* Se eliminan las **dependencias transitivas**, garantizando que ningún atributo no clave dependa de otro atributo no clave y aplicando la especialización de roles por herencia. El diseño se consolida en un modelo relacional robusto y totalmente normalizado, interconectado mediante claves foráneas (FK), lo que asegura la integridad referencial para el sistema de micromecenazgo:


* **`TABLA USUARIO`**: Mantiene la información centralizada de todas las cuentas.


* **`TABLA CREADOR`** e **`TABLA INVERSOR`**: Subtipos especializados de usuario que separan los roles de quienes publican campañas y quienes aportan fondos (añadiendo el nivel de riesgo en los inversores).


* **`TABLA PROYECTO`**: Administra las iniciativas de financiamiento vinculadas a sus respectivos usuarios creadores.


* **`TABLA RECOMPENSA`**: Detalla los incentivos ofrecidos por proyecto.


* **`TABLA APORTE`**: Consolida de forma limpia el historial transaccional relacionando al inversor, la recompensa y el proyecto financiado.
<img width="4972" height="3107" alt="EJERCICIO 19-FASES DE NORMALIZACION drawio" src="https://github.com/user-attachments/assets/0fea1245-5ead-488c-8560-1ef854932307" />

