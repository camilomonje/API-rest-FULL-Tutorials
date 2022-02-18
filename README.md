# Spring Boot JPA MySQL - Building Rest CRUD API example
"# API-rest-FULL-Tutorials" 

Se realiza la actividad agregar los endpoint propuestos por el Coah:

### Eliminar Tutorial por titulo

De acuerdo a esto se crea el Endpoint y el metodo *deleteTutorialByTitle*.

```java
@DeleteMapping("/tutorials/title/{title}")
    public ResponseEntity<String> deleteTutorialByTitle(@PathVariable("title") String title) {
        try {
            List<Tutorial> tutorials = tutorialRepository.findByTitle(title);
            for (Tutorial t : tutorials) {
                tutorialRepository.deleteById(t.getId());
            }
            return new ResponseEntity<>("Tutorials delete por Titulo ", HttpStatus.OK);
        } catch (Exception e) {
            return new ResponseEntity<>(HttpStatus.EXPECTATION_FAILED);
        }
    }
```

Luego de esto se realiza la prueba con Postman. Se elimina el curso con titulo Frances.

![image](https://github.com/camilomonje/API-rest-FULL-Tutorials/blob/main/images/ElminarporTitulo.JPG?raw=true)

------------


### Actualizar Tutorial por titulo

De acuerdo a esto se crea el Endpoint y el metodo *updateTutorialByTitle*.

```java
@PutMapping("/tutorials/title/{title}")
    public ResponseEntity<Tutorial> updateTutorialByTitle(@PathVariable("title") String title, @RequestBody Tutorial tutorial) {
		try {
            List<Tutorial> tutorials = tutorialRepository.findByTitle(title);
            if (!tutorials.isEmpty()){
                for (Tutorial t: tutorials) {
                    Tutorial _tutorial = t;
                    _tutorial.setTitle(tutorial.getTitle());
                    _tutorial.setDescription(tutorial.getDescription());
                    _tutorial.setPublished(tutorial.isPublished());
                    _tutorial.setPrice(tutorial.getPrice());
                    tutorialRepository.save(_tutorial);
                }
                return new ResponseEntity<>(HttpStatus.OK);
            }else{
                return new ResponseEntity<>(HttpStatus.NOT_FOUND);
            }
        }catch (Exception err){
            return new ResponseEntity<>(HttpStatus.NO_CONTENT);
        }
    }
```
Luego de esto se realiza la prueba con Postman. 

- Primero se consulta los datos con el metodo GET

![image](https://github.com/camilomonje/API-rest-FULL-Tutorials/blob/main/images/ActualizacionporTitulo_Antes.JPG?raw=true)

Donde observamos el curso con titulo "Chino", es el que vamos actualizar la descripción.

- Se realiza la actualización con el metodo que creamos.

![image](https://github.com/camilomonje/API-rest-FULL-Tutorials/blob/main/images/ActualizacionporTitulo.JPG?raw=true)

Donde observamos que el status nos da **200 OK**.

- Nuevamente se consulta los datos con el metodo GET para ver el cambio.

![image](https://github.com/camilomonje/API-rest-FULL-Tutorials/blob/main/images/ActualizacionporTitulo_Despues.JPG?raw=true)


------------

### Consultar por precio

De acuerdo a esto se crea el Endpoint y el metodo *getTutorialByPrice*.

```java
@GetMapping("/tutorials/price/{price}")
    public ResponseEntity<List<Tutorial>> getTutorialByPrice(@PathVariable("price") double price) {
        try {
            List<Tutorial> tutorials = tutorialRepository.findByPrice(price);
            if (!tutorials.isEmpty()){
                return new ResponseEntity<>(tutorials,HttpStatus.OK);
            }else{
                return new ResponseEntity<>(HttpStatus.NOT_FOUND);
            }
        }catch (Exception err){
            return new ResponseEntity<>(null, HttpStatus.INTERNAL_SERVER_ERROR);
        }
    }
```

Luego de esto se realiza la prueba con Postman. Se consulta por el precio 80.

![image](https://github.com/camilomonje/API-rest-FULL-Tutorials/blob/main/images/ConsultarporPrecio.JPG?raw=true)




