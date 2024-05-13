# Ejemplo de un Servicio con las Caracteristica de un Sistema Reactivo:

- **Contexto:** Se tiene un servicio bancario de transferencia de fondos utilizando programación reactiva.
El objetivo es crear un sistema que maneje las transferencias de fondos entre diferentes cuentas bancarias de forma eficiente y escalable. En lugar de procesar las transferencias de forma secuencial o bloqueante, se utiliza la programación reactiva para procesarlas de manera asíncrona y no bloqueante.


```java
import java.util.concurrent.Flow.*;
import java.util.concurrent.SubmissionPublisher;
```
### Clase para representar una transacción bancaria
```java
class Transfer {
    private String fromAccount;
    private String toAccount;
    private double amount;

    public Transfer(String fromAccount, String toAccount, double amount) {
        this.fromAccount = fromAccount;
        this.toAccount = toAccount;
        this.amount = amount;
    }

    // Getters y setters
}
```

### Clase principal del servicio bancario
```java
public class BancoReactivo {

    public static void main(String[] args) throws InterruptedException {
        // Creamos un publicador (Publisher) y un suscriptor (Subscriber)
        SubmissionPublisher<Transfer> publisher = new SubmissionPublisher<>();
        TransferSubscriber subscriber = new TransferSubscriber();

        // Conectamos el publicador con el suscriptor
        publisher.subscribe(subscriber);

        // Publicamos algunas transferencias de forma asíncrona
        publisher.submit(new Transfer("CuentaA", "CuentaB", 100.0));
        publisher.submit(new Transfer("CuentaB", "CuentaC", 50.0));
        publisher.submit(new Transfer("CuentaC", "CuentaA", 30.0));

        // Esperamos un momento para permitir que las transferencias se procesen
        Thread.sleep(1000);

        // Cerramos el publicador
        publisher.close();
    }

    // Implementación de un suscriptor para transferencias bancarias
    static class TransferSubscriber implements Subscriber<Transfer> {
        private Subscription subscription;

        @Override
        public void onSubscribe(Subscription subscription) {
            this.subscription = subscription;
            subscription.request(1); // Solicitamos una transferencia para comenzar
        }

        @Override
        public void onNext(Transfer transfer) {
            // Simulamos el procesamiento de la transferencia
            System.out.println("Procesando transferencia de " + transfer.fromAccount + " a " + transfer.toAccount + " por $" + transfer.amount);
            // En una aplicación real, aquí se realizarían las operaciones de transferencia en la base de datos u otro sistema externo

            subscription.request(1); // Solicitamos la siguiente transferencia
        }

        @Override
        public void onError(Throwable throwable) {
            throwable.printStackTrace();
        }

        @Override
        public void onComplete() {
            System.out.println("Procesamiento de transferencias completo");
        }
    }
}
```

### Características del Ejemplo:

1. **Hilos no bloqueantes**:
   - Utiliza la API Reactiva de Java para procesar las transferencias bancarias de manera asíncrona y no bloqueante.
   - El procesamiento de cada transferencia se realiza en el método `onNext()` del suscriptor, permitiendo que el hilo principal continúe con otras tareas sin esperar la finalización de cada transferencia.

2. **API de flujo reactivo (Reactive Stream API)**:
   - Se emplea `SubmissionPublisher` y las interfaces `Publisher` y `Subscriber` de la API de flujo reactivo para crear un flujo de datos de transferencias bancarias.
   - Facilita la transmisión asíncrona y no bloqueante de las transferencias desde el publicador hasta el suscriptor.

3. **Procesamiento de datos asíncrono**:
   - Cada transferencia bancaria se procesa de forma asíncrona en el suscriptor.
   - En el método `onNext()` del suscriptor, se simula el procesamiento de la transferencia, como registrarla en una base de datos.
   - Permite al servicio bancario manejar múltiples transferencias de manera concurrente y no bloqueante, mejorando la eficiencia y el rendimiento del sistema.
