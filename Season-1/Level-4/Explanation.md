El código proporcionado muestra cómo realizar una operación de inserción en una base de datos SQLite utilizando la biblioteca `sqlite3` en Python. Sin embargo, también muestra un ejemplo de cómo este código puede ser vulnerable a ataques de inyección SQL y cómo se puede corregir utilizando declaraciones parametrizadas para prevenir tales ataques.

1. **Vulnerable Código:**
```python
con = sqlite3.connect('example.db')
user_input = "Mary'); DROP TABLE Users;--"
sql_stmt = "INSERT INTO Users (user) VALUES ('" + user_input + "');"
con.executescript(sql_stmt)
```
En este fragmento, `user_input` es una cadena que contiene un valor proporcionado por el usuario, y se concatena directamente en la consulta SQL sin ningún tipo de validación o escape. Esto puede permitir a un atacante inyectar código malicioso, como en el ejemplo donde se intenta eliminar la tabla `Users` usando la instrucción `DROP TABLE`.

2. **Seguro con Declaraciones Parametrizadas:**
```python
con = sqlite3.connect('example.db')
user_input = "Mary'); DROP TABLE Users;--"
con.execute("INSERT INTO Users (user) VALUES (?)", (user_input,))
```
En este caso, en lugar de concatenar directamente la cadena `user_input` en la consulta SQL, se utiliza una declaración parametrizada. La consulta SQL contiene un marcador de posición `?` donde se espera que se inserte el valor de `user_input`. Luego, cuando se ejecuta la consulta, se pasa una tupla `(user_input,)` como segundo argumento, que contiene el valor de `user_input`. La biblioteca `sqlite3` manejará automáticamente la interpolación segura de los valores proporcionados, evitando así la posibilidad de inyección de SQL.

