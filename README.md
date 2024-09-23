# Trigger-TinCar
# Actualizar disponibilidad de un vehiculo cuando se realice un arrendamiento.
En este caso contamos con una base de datos que tiene una tabla llamada Parqueaderos y una Arrendamoentos donde se registra cada vez que un conductor arrienda el parqueadero.

      CREATE DATABASE TIN_CAR;
      CREATE TABLE Parqueaderos (
          id INT AUTO_INCREMENT PRIMARY KEY,
          direccion VARCHAR(255) NOT NULL,
          disponible BOOLEAN NOT NULL DEFAULT true
      );
      CREATE TABLE Arrendamientos (
          id_arrendamiento INT AUTO_INCREMENT PRIMARY KEY,
          id_parqueadero INT NOT NULL,
          id_conductor INT NOT NULL,
          fecha_inicio DATE NOT NULL,
          fecha_fin DATE NOT NULL,
          FOREIGN KEY (id_parqueadero) REFERENCES Parqueaderos(id)
      );

Ahora realizamos la insercion de los datos de nuestras tablas: 

      INSERT INTO Parqueaderos (direccion, disponible) 
      VALUES 
      ('Calle 123, Zona Norte', true),
      ('Avenida Principal 456, Centro', true),
      ('Carrera 78, Conjunto Residencial Verde', true),
      ('Calle 45, Barrio San Luis', true);
      INSERT INTO Arrendamientos (id_parqueadero, id_conductor, fecha_inicio, fecha_fin) 
      VALUES 
      (1, 101, '2024-09-22', '2024-09-25'),
      (2, 102, '2024-09-23', '2024-09-27'),
      (3, 103, '2024-09-24', '2024-09-28');


Al registrarse un nuevo arrendamoento  en la tabla "Arrendammientos", se actualice el estado del campo disponible del parqueadero a false (no disponible).

#Trigger Actualizar disponiblidad

      DELIMITER //
      CREATE TRIGGER actualizar_disponibilidad_parqueadero
      AFTER INSERT ON Arrendamientos
      FOR EACH ROW
      BEGIN
          UPDATE Parqueaderos
          SET disponible = false
          WHERE id = NEW.id_parqueadero;
      END //
      DELIMITER ;


      INSERT INTO Arrendamientos (id_parqueadero, id_conductor, fecha_inicio, fecha_fin)
      VALUES (1, 1001, '2024-09-22', '2024-09-30');

      
