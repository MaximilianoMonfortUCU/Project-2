// Pines del sensor ultrasónico
const int trigPin = 9;
const int echoPin = 10;

// Pines del driver de motor
const int motor1Pin1 = 3;
const int motor1Pin2 = 4;
const int motor2Pin1 = 5;
const int motor2Pin2 = 6;

// Variables para la distancia
long duration;
int distance;

void setup() {
  // Configurar los pines del sensor ultrasónico
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  
  // Configurar los pines del motor
  pinMode(motor1Pin1, OUTPUT);
  pinMode(motor1Pin2, OUTPUT);
  pinMode(motor2Pin1, OUTPUT);
  pinMode(motor2Pin2, OUTPUT);
  
  // Iniciar comunicación serial
  Serial.begin(9600);
}

void loop() {
  // Calcular la distancia del obstáculo
  distance = getDistance();
  Serial.print("Distance: ");
  Serial.println(distance);
  
  if (distance > 5) {
    // Si no hay obstáculo a menos de 5 cm, avanzar
    moveForward();
  } else {
    // Si hay obstáculo a menos de 5 cm, girar
    rotate();
    delay(500); // Esperar un momento para completar el giro
  }
  
  delay(100); // Pequeña demora para estabilidad
}

int getDistance() {
  // Enviar un pulso ultrasonico
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  // Leer el tiempo que tarda en recibir el eco
  duration = pulseIn(echoPin, HIGH);
  
  // Calcular la distancia en cm
  distance = duration * 0.034 / 2;
  return distance;
}

void moveForward() {
  // Configurar los motores para avanzar
  digitalWrite(motor1Pin1, HIGH);
  digitalWrite(motor1Pin2, LOW);
  digitalWrite(motor2Pin1, HIGH);
  digitalWrite(motor2Pin2, LOW);
}

void rotate() {
  // Configurar los motores para girar
  digitalWrite(motor1Pin1, LOW);
  digitalWrite(motor1Pin2, HIGH);
  digitalWrite(motor2Pin1, HIGH);
  digitalWrite(motor2Pin2, LOW);
}
