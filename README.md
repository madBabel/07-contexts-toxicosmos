## Objetivo
Trabajar con diferentes contextos disponibles durante las ejecuciones del flujo de trabajo.
## Apartados
### Uso de contextos de github & vars. Tareas:
1. Crear un archivo llamado 07-contexts.yaml en la carpeta .github/workflows en la raíz del repositorio. Los datos del workflow deben ser los siguientes:
   - nombre: 07 - Contexts. 
   - desencadentes:
      - push
      - workflow_dispatch
   - Trabajos:
     - `echo-data`, debería ejecutarse en ubuntu-latest y con dos steps:
       - `Show Info`, que imprime las siguientes líneas que contienen información del contexto de github (consejo: usar un script de varias líneas con varios comandos de eco):
          ```shell
          "Nombre del evento: <recupere el nombre del evento aquí>"
          "Ref: <recupere la referencia aquí>"
          "SHA: <recupere el sha de commit aquí>"
          "Actor: <recupere el nombre del actor aquí>"
          "Flujo de trabajo: <recupere el nombre del workflow aquí>"
          "ID de ejecución: <recupere el ID de ejecución aquí>"
          "Número de ejecución: <recupere el número de ejecución aquí>"
          ```
       - `Retrieve Variable`, que imprime dos lineas:
           - una que contiene el valor de una variable del repositorio denominada `MY_VAR`.
           - otra que contiene el valor de una variable del repositorio denominada `ORG_VAR`.
2. Crear una variable de repositorio denominada `MY_VAR`. Consultar la sección de TIPS a continuación para saber paso a paso cómo crear variables de repositorio. Establezcer el valor de esta variable en `hola mundo`.
3. Confirmar los cambios y subir (push) el código en la rama main. Inspeccionar el resultado de la ejecución del workflow. 
### Uso de contextos no válidos en GitHub Actions. Tareas:
1. Añadir una nueva linea de clave-valor al yaml, `"run-name"`. Ésta se indica n el nivel superior, justo entre 'name' y 'on'. El run-name permite definir el nombre de la ejecución del flujo de trabajo que aparece en la interfaz de usuario. Establecer el valor de run-name en `My custom workflow run name - ${{ runner.os }}`. 
2. Confirmar los cambios y enviar el código. Tómese un tiempo para inspeccionar el resultado de la ejecución del flujo de trabajo. ¿Qué mensaje de error apareció?
### Uso de contexto 'inputs' en GitHub Actions. Tareas:
1. Reemplace el run-name con `07 - Contexts | DEBUG - ${{ inputs.debug }}`
2. Agregue una configuración al evento "workflow_dispatch" para que pueda definir un parámetro de entrada para el workflow. Para hacer esto, agregue un nuevo valor 'input' al desdencandenate workflow_dispatch. El parámetro debe llamarse `debug`, tener un tipo booleano y un valor predeterminado falso. Si no está seguro de cómo hacerlo, consulte la sección de TIPS a continuación para conocer paso a paso cómo definir inputs para el evento workflow_dispatch.
3. Confirmar los cambios y subir (push) el código en la rama main. Inspeccionar el resultado de la ejecución del workflow. ¿Qué valor se completó para la parámetro de debug?
4. Ahora ejecute el workflow desde la interfaz de usuario y pruébelo con diferentes variaciones para la entrada de depuración. ¿Cómo afecta esto al resultado de las ejecuciones del flujo de trabajo?
### Uso de contexto 'env' en GitHub Actions. Tareas:
1. Añadir una nueva linea de clave al yaml, "env". Ésta se indica en el nivel superior, junto con 'name' y 'on'. Añada dos variables de entorno:
   - `MY_WORKFLOW_VAR`, con el valor establecido en `'workflow'`
   - `MY_OVERWRITTEN_VAR`, con el valor establecido en `'workflow'`
2. En el job 'echo-data', agregue una clave env para definir dos variables de entorno a nivel de job:
   - `MY_JOB_VAR`, con el valor establecido en `'job'`
   - `MY_OVERWRITTEN_VAR`, con el valor establecido en `'job'`
3. Añadir un paso adicional después del paso `'Retrieve Variable'` con el nombre `'Print Env Variables'`. Agregar una clave 'env' para definir una única variable de entorno `MY_OVERWRITTEN_VAR`, con el valor `'step'`, y además, que ejecute un script de varias líneas para imprimir la siguiente información en la pantalla:
   - `"Workflow env: <recupere el valor de la variable de entorno MY_WORKFLOW_VAR aquí>"`
   - `"Overwritten env:: <recupere el valor de la variable de entorno MY_OVERWRITTEN_VAR aquí>"`
4. Añadir otro paso adicional después del paso `'Print Env Variables'` con el nombre `'Print Env Variables by def'`. que ejecute un script de varias líneas para imprimir la siguiente información en la pantalla ( sin definir ninguna variable de entorno adicional):
    - `"Workflow env: <recupere el valor de la variable de entorno MY_WORKFLOW_VAR aquí>"`
    - `"Overwritten env: <recupere el valor de la variable de entorno MY_OVERWRITTEN_VAR aquí>"`
5. Confirme los cambios y envíe el código. Tómese un tiempo para inspeccionar el resultado de la ejecución del flujo de trabajo. ¿Cómo se sobrescribieron las variables env con respecto al workflow, el job y los steps?

