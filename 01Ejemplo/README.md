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

# Ejemplos de Mono y Flux en Servicios

## Mono

- **Gestión de Solicitud de Créditos:** Cuando un cliente solicita un préstamo o una línea de crédito, el proceso de evaluación y aprobación puede involucrar múltiples pasos y decisiones. Un flujo Mono podría utilizarse para representar la solicitud de crédito de un cliente y su estado de aprobación, que podría ser aprobado, rechazado o pendiente de revisión.

- **Notificaciones de Alerta de Saldo Bajo:** Se puede utilizar Mono para representar el estado del saldo de la cuenta de un cliente. Cuando el saldo de la cuenta cae por debajo de un umbral específico, se puede activar un flujo Mono para enviar una notificación de alerta al cliente, advirtiéndole sobre la situación y ofreciendo opciones para evitar cargos por sobregiro


- **Gestión de Errores y Excepciones:**  pueden surgir errores o excepciones durante el procesamiento de una transferencia nacional, como información incorrecta del beneficiario o fondos insuficientes en la cuenta del remitente. Un flujo Mono podría manejar estos casos de manera adecuada, proporcionando mecanismos para manejar errores, revertir transacciones si es necesario y notificar a las partes afectadas

## Flux

- **Gestión de Carteras de Inversión:**  Flux podría utilizarse para transmitir datos en tiempo real sobre los precios de mercado, el rendimiento de los activos y las noticias financieras relevantes, permitiendo a los gestores de cartera tomar decisiones informadas sobre la asignación de activos y la gestión del riesgo.

- **Detección y Prevención de Fraude:** Flux puede ser utilizado para analizar datos de transacciones bancarias en tiempo real y detectar patrones sospechosos que podrían indicar actividad fraudulenta, como transacciones inusuales o intentos de acceso no autorizado a cuentas. El sistema podría generar alertas instantáneas para que los equipos de seguridad bancaria investiguen y tomen medidas apropiadas para prevenir el fraude.


## Mono 

- **Gestión de Errores y Excepciones:**

Este proyecto Java demuestra cómo utilizar Mono para manejar errores y excepciones durante el procesamiento de una transferencia bancaria nacional.

###  Implementación con Mono

El método `procesarTransferencia` en la clase `TransferService` simula el procesamiento de una transferencia bancaria. Si se cumplen ciertas condiciones, como un monto de transferencia excesivo o la falta de un beneficiario, se lanzarán excepciones correspondientes. Estas excepciones son manejadas utilizando Mono, que puede emitir un valor tanto para el caso de éxito como para el caso de error.

```java
public class TransferService {
    
    public Mono<String> procesarTransferencia(String beneficiario, double monto) {
        // Simulación de procesamiento de transferencia
        if (monto > 10000) {
            // Si el monto supera un límite, se lanza una excepción
            return Mono.error(new RuntimeException("Monto excede el límite permitido"));
        } else if (beneficiario.isEmpty()) {
            // Si no se proporciona un beneficiario, se lanza una excepción
            return Mono.error(new IllegalArgumentException("El beneficiario no puede estar vacío"));
        } else {
            // Si la transferencia se procesa correctamente, se devuelve un mensaje de confirmación
            return Mono.just("Transferencia a " + beneficiario + " por $" + monto + " procesada correctamente");
        }
    }
    
    public static void main(String[] args) {
        TransferService service = new TransferService();
        
        service.procesarTransferencia("Juan Perez", 5000)
            .subscribe(
                System.out::println, // Manejo del caso de éxito
                error -> System.err.println("Error: " + error.getMessage()) // Manejo del error
            );
    }
}

```

## Flux

- **Detección y Prevención de Fraude**

Este proyecto Java utiliza Flux para analizar datos de transacciones bancarias en tiempo real y detectar patrones sospechosos que podrían indicar actividad fraudulenta. El sistema genera alertas instantáneas para que los equipos de seguridad bancaria investiguen y tomen medidas apropiadas para prevenir el fraude.

###  Implementación con Flux

El procesamiento de datos en tiempo real se realiza utilizando Flux para gestionar el flujo continuo de transacciones bancarias. Se aplican filtros y análisis en tiempo real para identificar patrones sospechosos, como transacciones inusuales o intentos de acceso no autorizado a cuentas.

```java
import reactor.core.publisher.Flux;

public class FraudDetectionService {
    
    public Flux<String> detectarFraude(Flux<Transaction> transacciones) {
        return transacciones
            .filter(transaction -> transaction.getAmount() > 10000) // Filtrar transacciones de monto alto
            .map(transaction -> "Alerta de fraude: Transacción inusual detectada - " + transaction.toString()); // Mapear transacciones sospechosas a mensajes de alerta
    }
    
    public static void main(String[] args) {
        Flux<Transaction> transacciones = // Obtener flujo de datos de transacciones bancarias en tiempo real
        
        FraudDetectionService service = new FraudDetectionService();
        
        service.detectarFraude(transacciones)
            .subscribe(System.out::println); // Mostrar alertas de fraude en la consola
    }
}
