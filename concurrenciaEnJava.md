                RESPUESTA 7a
Uno de los mecanismos de sincronización más versátiles en Java es el semáforo (Semaphore), que forma parte del paquete java.util.concurrent. Los semáforos se utilizan para controlar el acceso a un recurso compartido mediante el conteo de permisos. Son especialmente útiles cuando se desea limitar la cantidad de hilos que pueden acceder simultáneamente a una sección crítica del código, como una base de datos o una impresora compartida. A diferencia del uso de la palabra clave synchronized, los semáforos ofrecen un mayor control y flexibilidad en la gestión de la concurrencia.

En términos simples, un semáforo mantiene un conteo interno de "permisos". Los hilos deben adquirir un permiso antes de continuar (con el método acquire()), y lo liberan cuando terminan su tarea (con el método release()). Si no hay permisos disponibles, el hilo que intenta adquirir uno quedará bloqueado hasta que alguno se libere. Esto permite evitar condiciones de carrera y garantiza un acceso ordenado y limitado a los recursos compartidos.


                Respuesta 7b
public class EjemploSemaforo {
    private static final Semaphore semaforo = new Semaphore(2);

    public static void main(String[] args) {
        for (int i = 1; i <= 5; i++) {
            Thread hilo = new Thread(new Tarea(i));
            hilo.start();
        }
    }

    static class Tarea implements Runnable {
        private int id;

        public Tarea(int id) {
            this.id = id;
        }

        public void run() {
            try {
                System.out.println("Hilo " + id + " esperando permiso...");
                semaforo.acquire();
                System.out.println("Hilo " + id + " obtuvo permiso.");

                // Sección crítica
                Thread.sleep(2000);

                System.out.println("Hilo " + id + " liberando permiso.");
                semaforo.release();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}