# Seminario-MundosVirtuales2425
Componentes del grupo: 
- Raúl Álvarez Pérez
- Juan Aday Siverio González
- Diego Rodríguez Martín
- Sergio Nicolás Segui

### 1. **Qué funciones se pueden usar en los scripts de Unity para llevar a cabo traslaciones, rotaciones y escalados.**
   
   En Unity, se pueden usar funciones de la clase `Transform` para manipular los objetos en el espacio 3D:
   - **Traslación**: Para mover un objeto, se usa `transform.Translate(Vector3 direction)` o modificando directamente la posición con `transform.position`.
   - **Rotación**: Para rotar un objeto, se usa `transform.Rotate(Vector3 eulerAngles)`, o estableciendo un ángulo específico con `transform.rotation`.
   - **Escalado**: La escala se ajusta con `transform.localScale = new Vector3(x, y, z)`.

   Cabe destacar que estas funciones se utilizan en los objetos cinemáticos, pues para los no cinemáticos debemos moverlos haciendo uso del `RigidBody`.

### 2. **Cómo trasladarías la cámara 2 metros en cada uno de los ejes y luego la rotas 30º alrededor del eje Y. ¿Obtendrías el mismo resultado en ambos casos? Justifica el resultado.**

   - **Primera Opción (Traslación y luego Rotación):**

     ```csharp
     transform.Translate(new Vector3(2, 2, 2));
     transform.Rotate(0, 30, 0);
     ```

   - **Segunda Opción (Rotación y luego Traslación):**

     ```csharp
     transform.Rotate(0, 30, 0);
     transform.Translate(new Vector3(2, 2, 2));
     ```

   **Resultado**: No se obtendrá el mismo resultado en ambos casos. En la primera opción, la traslación ocurre en el sistema de referencia original, antes de la rotación. En la segunda, la rotación afecta a los ejes locales de la cámara, haciendo que el desplazamiento no sea el mismo que el anterior.

### 3. **Sitúa la esfera de radio 1 en el campo de visión de la cámara y configura un volumen de vista que la recorte parcialmente.**

   Hay que colocar la esfera en el campo de visión ajustando su posición en el eje Z (delante de la cámara). Luego, ajustamos los valores de la proyección de la cámara en el inspector para cortar la esfera parcialmente (ajustamos `nearClipPlane` y `farClipPlane`).

### 4. **Sitúa la esfera de radio 1 en el campo de visión de la cámara y configura el volumen de vista para que la deje fuera de la vista.**

   Para sacar la esfera del campo de visión, ajustamos el `farClipPlane` de la cámara para que sea menor que la distancia a la esfera. También se podría cambiar la posición de la esfera o el `fieldOfView`.

### 5. **Cómo puedes aumentar el ángulo de la cámara. Qué efecto tiene disminuir el ángulo de la cámara.**

   Para cambiar el ángulo de visión:
   
   ```csharp
   Camera.fieldOfView = nuevoValor;
   ```

   **Efecto**: Un ángulo mayor amplía el campo de visión, mientras que un ángulo menor lo reduce, haciendo que los objetos parezcan más grandes o más pequeños.

### 6. **Es correcta la siguiente afirmación: Para realizar la proyección al espacio 2D, en el inspector de la cámara, cambiaremos el valor de `projection`, asignándole el valor de `orthographic`.**

   Sí, la afirmación es correcta. Para proyectar en 2D, podemos cambiar el valor de `projection` a `orthographic` en el inspector de la cámara.

### 7. **Especifica las rotaciones que se han indicado en los ejercicios previos con la utilidad `Quaternion`.**

   Usar `Quaternion.Euler(x, y, z)` permite aplicar rotaciones en Unity de manera precisa. Por ejemplo, la rotación que tuvimos que aplicar en el ejercicio 2 se podría hacer de la siguiente forma:
  
   ```csharp
   transform.rotation = Quaternion.Euler(0, 30, 0);
   ```

### 8. **¿Cómo puedes averiguar la matriz de proyección en perspectiva que se ha usado para proyectar la escena al último frame renderizado?**

   La matriz de proyección en perspectiva actual de la cámara puede obtenerse con:
   
   ```csharp
   Matrix4x4 perspectiveMatrix = Camera.main.projectionMatrix;
   ```

### 9. **¿Cómo puedes averiguar la matriz de proyección en perspectiva ortográfica que se ha usado para proyectar la escena al último frame renderizado?**

   Cambia la cámara a modo ortográfico:
  
   ```csharp
   Camera.main.orthographic = true;
   ```

   Luego usa:
  
   ```csharp
   Matrix4x4 orthoMatrix = Camera.main.projectionMatrix;
   ```

### 10. **¿Cómo puedes obtener la matriz de transformación entre el sistema de coordenadas local y el mundial?**

    La matriz que convierte de coordenadas locales a mundiales se obtiene con:
    
    ```csharp
    Matrix4x4 localToWorld = transform.localToWorldMatrix;
    ```

### 11. **Cómo puedes obtener la matriz para cambiar al sistema de referencia de vista.**

    La matriz de vista se puede obtener con:
    
    ```csharp
    Matrix4x4 viewMatrix = Camera.main.worldToCameraMatrix;
    ```

### 12. **Especifica la matriz de la proyección usada en un instante de la ejecución del ejercicio 1 de la práctica 1.**

    Durante la ejecución, puedes revisar la matriz de proyección de la cámara en cualquier instante:
    
    ```csharp
    Matrix4x4 currentProjection = Camera.main.projectionMatrix;
    ```

### 13. **Especifica la matriz de modelo y vista de la escena del ejercicio 1 de la práctica 1.**

    - **Matriz de Modelo**: Se obtiene con `transform.localToWorldMatrix`.
    - **Matriz de Vista**: Es `Camera.main.worldToCameraMatrix`.

### 14. **Aplica una rotación en el `Start` de uno de los objetos de la escena y muestra la matriz de cambio al sistema de referencias mundial.**
    
    ```csharp
    void Start() {
        transform.Rotate(45, 0, 0);
        Debug.Log(transform.localToWorldMatrix);
    }
    ```

### 15. **¿Cómo puedes calcular las coordenadas del sistema de referencia de un objeto con las siguientes propiedades del `Transform`?**

    **Position** `(3, 1, 1)`, **Rotation** `(45, 0, 45)`

    Si tienes una posición `(3, 1, 1)` y una rotación `(45, 0, 45)`, puedes obtener la matriz completa con:
    
    ```csharp
    Vector3 position = new Vector3(3, 1, 1);
    Quaternion rotation = Quaternion.Euler(45, 0, 45);
    Matrix4x4 transformMatrix = Matrix4x4.TRS(position, rotation, Vector3.one);

    Vector3 localPoint = new Vector3(1, 0, 0);
    Vector3 worldPoint = transformMatrix.MultiplyPoint3x4(localPoint);
    Debug.Log("Coordenadas mundiales: " + worldPoint);
    ```

### 16. **Investiga sobre los modelos de iluminación que aplica Unity y resume las relaciones existentes con el modelo explicado en clase.**

  # Relación entre tipos de luz en Unity y conceptos de iluminación gráfica

#### 1. Point Light y Luz Local
En Unity, las **Point Lights** emiten luz desde un punto en todas las direcciones, con intensidad que disminuye según la ley del cuadrado inverso.  
En las transparencias, esto corresponde al concepto de **luz local**, donde la fuente de luz se encuentra en un punto definido y afecta a los objetos según su distancia.

---

#### 2. Spot Light y Foco
Los **Spotlights** de Unity emiten luz en forma de cono con dirección, apertura y atenuación controladas.  
Esto está directamente relacionado con el concepto de **foco** de la presentación, que describe una luz puntual con un área de influencia en forma de cono, definida por dirección y apertura.

---

#### 3. Area Light y Material Emisor
En Unity, las **Área Lights** iluminan desde un rectángulo, produciendo sombras suaves. Estas luces aparecen en tiempo real.  
La presentación describe luces basadas en **radiosidad** y energía emitida por áreas, que es el principio detrás de las luces de área. Además, se relacionan con los **materiales emisores**, ya que ambos distribuyen luz de manera difusa sobre su entorno.

---

#### 4. Emissive Materials y Materiales Emisores
Unity permite que los **materiales emisores** emitan luz según sus propiedades, contribuyendo a la iluminación global de la escena.  
Esto coincide con el tema de los **materiales emisores** dentro de la presentación, que emiten luz desde su superficie y afectan a los objetos estáticos en la escena.

---

#### 5. Ambient Light y Luz Ambiente
En Unity, la **Ambient Light** ilumina toda la escena de manera uniforme, sin una fuente de luz específica.  
En las transparencias, se describe como un componente del **modelo de iluminación local**, que simula un brillo constante en todos los objetos, similar a un "flash" en una habitación oscura.


### 17. **Indica las funciones de la API de Unity más importantes respecto a la iluminación.**

    - `Light` para crear y configurar luces.
    - `RenderSettings.ambientLight` para ajustar la luz ambiental.
    - `Material.SetFloat("_Glossiness")` y `Material.SetColor("_SpecColor")` para efectos de materiales iluminados.
    - `Lightmapping` y `LightProbe` para generar iluminación global y más realista en la escena.
