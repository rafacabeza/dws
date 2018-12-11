## Proyecto de tienda online: _shop17_
- Entidades:
 - Family
 -  Product
 -  Order
 - User
 -  Role (root, admin, user)
- Relaciones
 - Family-Product 1:N
 - Product-Order N:M
 - User-Order 1:N
 - Role-User 1:N

- Casos de uso
 - CRUD Completo de Familia 

### Lunes 23/1 iniciar proyecto y CRUD de familias
- Clona el proyecto del shop17 que encontrarás en bitbucket.
- Prepara el CRUD de la tabla familia: modelo Family, FamilyController (tipo resource), migración y seeder con varios registros de prueba.
- método index:
    -Si ajax: JSON
    -Si no vista con todo
    
- método create

- metodo store