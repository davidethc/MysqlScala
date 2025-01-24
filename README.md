
## **¿Qué es JDBC y cuáles son sus componentes?**
JDBC (Java Database Connectivity) es una API de Java que permite a las aplicaciones conectarse e interactuar con bases de datos relacionales. Proporciona una forma estándar de ejecutar consultas SQL y manejar datos desde cualquier base de datos que tenga un controlador compatible.

### **Componentes principales de JDBC:**
1. **DriverManager**: Gestiona los controladores de bases de datos y establece conexiones.
2. **Connection**: Representa una conexión abierta con la base de datos.
3. **Statement**: Permite ejecutar consultas SQL.
4. **ResultSet**: Almacena los resultados de una consulta.
5. **PreparedStatement**: Extensión de `Statement` que permite consultas precompiladas y parametrizadas.

---

## **Librerías de Scala para conectarse a una base de datos relacional**
Aquí hay dos librerías populares que facilitan la conexión con bases de datos relacionales desde Scala:

| **Librería**          | **Características**                                                                 | **Ventajas**                                                    |
|------------------------|-------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| **Doobie**            | Basada en programación funcional, compatible con Cats. Ofrece un enfoque declarativo. | - Manejo seguro de errores.<br>- Integra fácilmente con Cats. |
| **Slick**             | DSL para consultas SQL en Scala, con soporte tanto para consultas reactivas como síncronas. | - Genera consultas tipadas.<br>- Compatible con múltiples BD. |

---

## **Pasos para conectar Scala a una base de datos MySQL**

### **1. Generar una base de datos en MySQL**
Ejecuta los siguientes comandos en MySQL para crear una base de datos y una tabla con datos de prueba:

```sql
CREATE DATABASE test_db;

USE test_db;

CREATE TABLE usuarios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50),
    correo VARCHAR(100)
);

INSERT INTO usuarios (nombre, correo)
VALUES 
    ('Juan Perez', 'juan.perez@gmail.com'),
    ('Ana Gomez', 'ana.gomez@gmail.com'),
    ('Luis Torres', 'luis.torres@gmail.com');
```
<img width="718" alt="image" src="https://github.com/user-attachments/assets/ef5ff3e7-fc4e-435a-a40b-69b70941e6ed" />
```scala
import java.sql.{Connection, DriverManager, ResultSet}

object MySQLConnection {
  def main(args: Array[String]): Unit = {

    val url = "jdbc:mysql://localhost:3306/test_db"
    val user = "root"
    val password = "Susanthc123"

    // Estableciendo la conexión
    var connection: Connection = null
    try {
      connection = DriverManager.getConnection(url, user, password)
      println("Conexión exitosa a la base de datos.")


      val statement = connection.createStatement()
      val resultSet: ResultSet = statement.executeQuery("SELECT * FROM usuarios")

      // Imprimir resultados
      while (resultSet.next()) {
        val id = resultSet.getInt("id")
        val nombre = resultSet.getString("nombre")
        val correo = resultSet.getString("correo")
        println(s"ID: $id, Nombre: $nombre, Correo: $correo")
      }
    } catch {
      case e: Exception => e.printStackTrace()
    } finally {
      if (connection != null) connection.close()
    }
  }
}

```
<img width="1310" alt="image" src="https://github.com/user-attachments/assets/5ecb8eec-d144-4ae2-834a-13d4d311440a" />
