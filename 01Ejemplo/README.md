# Ejemplos de Eventos y Streams en Servicios

En la programación reactiva aplicada a servicios, los eventos y los streams juegan un papel fundamental en la creación de sistemas altamente responsivos y eficientes. Aquí hay algunos ejemplos de eventos y streams en el contexto de servicios:

## Eventos

- **Evento de Solicitud HTTP:** Ocurre cuando un cliente realiza una solicitud HTTP a un servicio web para obtener o enviar datos.

- **Evento de Recepción de Mensajes:** En un sistema de mensajería en tiempo real, ocurre cuando un servicio recibe un mensaje de un cliente o de otro servicio.

- **Evento de Cambio de Estado:** Puede ocurrir cuando hay un cambio en el estado de un recurso o entidad en el sistema, como la actualización de un registro en una base de datos.

## Streams

- **Secuencia de Respuestas HTTP:** En un servicio web, puede representar una secuencia de respuestas a las solicitudes HTTP recibidas de los clientes.

- **Flujo de Datos de Transacciones:** En un sistema financiero, puede proporcionar una secuencia continua de transacciones realizadas por los usuarios.

- **Transmisión de Eventos de Registro:** En un sistema de registro o monitoreo, puede representar una secuencia de eventos registrados por los diferentes componentes del sistema.



# Ejemplos de Eventos y Streams en Servicios

En la programación reactiva aplicada a servicios, los eventos y los streams juegan un papel fundamental en la creación de sistemas altamente responsivos y eficientes. Aquí hay algunos ejemplos de eventos y streams en el contexto de servicios:

## Eventos

- **Evento de Solicitud HTTP:** Ocurre cuando un cliente realiza una solicitud HTTP a un servicio web para obtener o enviar datos. Por ejemplo:

  ```java
  @RestController
  public class UserController {
  
      @Autowired
      private UserService userService;
  
      @GetMapping("/users/{id}")
      public Mono<User> getUser(@PathVariable String id) {
          return userService.getUserById(id);
      }

  }


## Streams

- **Secuencia de Respuestas HTTP:** En un servicio web, puede representar una secuencia de respuestas a las solicitudes HTTP recibidas de los clientes. Por ejemplo:

  ```java
  @RestController
  public class UserController {
  
      @Autowired
      private UserService userService;
  
      @GetMapping("/users")
      public Flux<User> getAllUsers() {
          return userService.getAllUsers();
      }
  

  }

Flujo de Datos de Transacciones: En un sistema financiero, puede proporcionar una secuencia continua de transacciones realizadas por los usuarios.


## Mono

Este es un ejemplo básico de cómo usar Mono en el ámbito bancario utilizando Java y Reactor. En este ejemplo, mostraremos cómo obtener detalles de cuenta de forma reactiva.

## Obtener Detalles de Cuenta con Mono

```java
import reactor.core.publisher.Mono;

public class AccountService {
    public Mono<AccountDetails> getAccountDetails(String accountId) {
        // Simulación de lógica para obtener detalles de la cuenta de forma asincrónica
        return Mono.just(new AccountDetails(accountId, "John Doe", 1000.00));
    }
}

public class Main {
    public static void main(String[] args) {
        String accountId = "123456789";
        AccountService accountService = new AccountService();
        
        // Obtener detalles de cuenta de manera asincrónica y manejar el resultado
        accountService.getAccountDetails(accountId)
            .subscribe(accountDetails -> {
                // Mostrar los detalles de la cuenta en la interfaz de usuario
                System.out.println("Detalles de la cuenta:");
                System.out.println("ID de cuenta: " + accountDetails.getAccountId());
                System.out.println("Titular de la cuenta: " + accountDetails.getAccountHolder());
                System.out.println("Saldo de la cuenta: " + accountDetails.getBalance());
            });
    }
}

```json
{
  "Detalles de la cuenta": {
    "ID de cuenta": "123456789",
    "Titular de la cuenta": "John Doe",
    "Saldo de la cuenta": 1000.0
  }
}


## Flux

El siguiente código muestra cómo obtener transacciones en tiempo real de una cuenta bancaria utilizando Flux en Java y Reactor:

```java
import reactor.core.publisher.Flux;

public class TransactionService {
    public Flux<Transaction> getRealTimeTransactions(String accountId) {
        // Simulación de lógica para obtener transacciones en tiempo real
        return Flux.just(
                new Transaction(accountId, "Compra", 50.0),
                new Transaction(accountId, "Depósito", 100.0),
                new Transaction(accountId, "Retiro", -20.0)
        );
    }
}

public class Main {
    public static void main(String[] args) {
        String accountId = "123456789";
        TransactionService transactionService = new TransactionService();
        
        // Obtener transacciones en tiempo real y manejar cada nueva transacción
        transactionService.getRealTimeTransactions(accountId)
            .subscribe(transaction -> {
                // Mostrar la nueva transacción en la consola
                System.out.println("Nueva transacción:");
                System.out.println("Tipo: " + transaction.getType());
                System.out.println("Monto: " + transaction.getAmount());
            });
    }
}


```json
[
  {
    "Nueva transacción": {
      "Tipo": "Compra",
      "Monto": 50.0
    }
  },
  {
    "Nueva transacción": {
      "Tipo": "Depósito",
      "Monto": 100.0
    }
  },
  {
    "Nueva transacción": {
      "Tipo": "Retiro",
      "Monto": -20.0
    }
  }
]
