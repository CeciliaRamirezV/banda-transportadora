# elevador
int A = 13;
int B = 14;
int C = 15;
int D = 16;
int E = 17;
int F = 18;
int G = 19;
void setup() {
pinMode(2,INPUT);
pinMode(3,INPUT);
pinMode(4,INPUT);
pinMode(5,INPUT);
pinMode(6,INPUT);
pinMode(7,INPUT);
pinMode(8,INPUT);
pinMode(9,INPUT);
pinMode(10,INPUT);
pinMode(11,OUTPUT); // MOTOR ARRIBA
pinMode(12,OUTPUT); // MOTOR ABAJO
pinMode(A,OUTPUT);
pinMode(B,OUTPUT);
pinMode(C,OUTPUT);
pinMode(D,OUTPUT);
pinMode(E,OUTPUT);
pinMode(F,OUTPUT);
pinMode(G,OUTPUT);
digitalWrite(11,LOW);
digitalWrite(12,LOW);
}

void loop() {  /* adquicisión*/
  //pisos
bool P1 = digitalRead(2); // piso 1
bool P2 = digitalRead(3); // piso 2
bool P3 = digitalRead(4); // piso 3
bool P4 = digitalRead(5); // piso 4
 // sensores
bool s1 = digitalRead(6); // sensor 1
bool s2 = digitalRead(7); // sensor 2
bool s3 = digitalRead(8); // sensor 3
bool s4 = digitalRead(9); // sensor 4
bool reset=digitalRead(10); // RESET
 // PROCESO
bool reset1 = (s1 ||reset);
bool reset2 = (s2 || reset);
bool reset3 = (s3 || reset);
bool reset4 = (s4 || reset);
                                                      //OPCIONES PARA SUBIR
 // DEL PISO 1 A TODOS LOS PISOS.
/*DEL PISO 1 AL PISO 2 , FLIPFLOP 1*/
static bool QP1 = 0;
bool AND1 = (s1 && P2);
bool Q1 = !(QP1 || AND1);
QP1 = !(Q1 || reset2);
/* DEL PISO 2 AL PISO 3, FLIPFLOP 2 */
static bool QP2 = 0;
bool AND2 = (s1 && P3);
bool Q2 = !(QP2 || AND2);
QP2 = !(Q2 || reset3);
/*DEL PISO 3 AL PISO 4, FLIPFLOP 3*/
static bool QP3 = 0;
bool AND3 = (s1 && P4);
bool Q3 = !(QP3 || AND3);
QP3 = !(Q3 || reset4);
// ACTIVACION DE MOTOR HACIA ARRIBA PRIMER CONDICION// 
bool MA_1 = (QP1 || QP2 || QP3);

//DEL PISO 2 A TODOS LOS DEMÁS
/* DEL PISO 2 AL PISO 3 FLIPFLOP 4 */
static bool QP4 = 0;
bool AND4 = (s2 && P3);
bool Q4 = !(QP4 || AND4);
QP4 = !(Q4 || reset3); 
/*DEL PISO 2 AL PISO 4 FLIPFLOP 5 */
static bool QP5 = 0;
bool AND5 = (s2 && P4);
bool Q5 = !(QP5 || AND5);
QP5 = !(Q5 || reset4);

// ACTIVACION DE MOTOR HACIA ARRIBA SEGUNDA CONDICION
bool MA_2 = (QP4 || QP5);

 // DEL PISO 3 AL PISO 4
 /*DEL PISO 2 AL PISO 4 FLIPFLOP 6 */
static bool QP6 = 0;
bool AND6 = (s3 && P4);
bool Q6= !(QP6 || AND6);
QP6 = !(Q6 || reset4);
// AVTIVACION DE MOTOR HACIA ARRIBA TERCERA CONDICION
bool MA_3 = QP6;


// ACTICACION FINAL DE ELEVADOR //
 bool MOTOR_ARRIBA = ( MA_1 || MA_2 || MA_3);
                                                      //OPCIONES PARA BAJAR
 // DEL PISO 4 A TODOS LOS DEMAS
 /*DEL PISO 4 AL PISO 3 FLIPFLOP 7 */
static bool QP7 = 0;
bool AND7 = (s4 && P3);
bool Q7= !(QP7 || AND7);
QP7 = !(Q7 || reset3);
/*DEL PISO 4 AL PISO 2 FLIPFLOP 8 */
static bool QP8 = 0;
bool AND8 = (s4 && P2);
bool Q8= !(QP8 || AND8);
QP8 = !(Q8 || reset2);
/*DEL PISO 4 AL PISO 1 FLIPFLOP 9 */
static bool QP9 = 0;
bool AND9 = (s4 && P1);
bool Q9= !(QP9 || AND9);
QP9 = !(Q9 || reset1);

// ACTIVACION DE MOTOR HACIA ABAJO PRIMERA CONDICION

bool MB_1 = ( QP7 || QP8 || QP9);

            // DEL PISO 3 A TODOS LOS DEMÁS
/*DEL PISO 3 AL PISO 2 FLIPFLOP 10 */
static bool QP10 = 0;
bool AND10 = (s3 && P2);
bool Q10= !(QP10 || AND10);
QP10 = !(Q10 || reset2);
/*DEL PISO 3 AL PISO 1 FLIPFLOP 11 */
static bool QP11 = 0;
bool AND11 = (s3 && P1);
bool Q11= !(QP11 || AND11);
QP11 = !(Q11 || reset1);

// ACTIVACION DE MOTOR HACIA ABAJO SEGUNDA CONDICION

bool MB_2 = ( QP10 || QP11);

/*DEL PISO 3 AL PISO 1 FLIPFLOP 12 */
static bool QP12 = 0;
bool AND12 = (s2 && P1);
bool Q12= !(QP12 || AND12);
QP12 = !(Q12 || reset1);

//ACTIVACION DE MOTOR HACIA ABAJO TERCERA CONDICION

bool MB_3 = QP12;

//ACTIVACION FINAL DE MOTOR HACIA ABAJO//

 bool MOTOR_ABAJO = (MB_1 || MB_2 || MB_3);
 bool APAGADO = (Q1 || Q2 || Q3 || Q4 || Q5 || Q6 || Q7 || Q8 || Q9 || Q10 || Q11 || Q12);
 // SEÑALIZACION
 digitalWrite(28, MOTOR_ARRIBA); // SUBE
 digitalWrite(12, MOTOR_ABAJO); // BAJA 
 if(s1)
 {
  digitalWrite(A,LOW);
  digitalWrite(B,HIGH);
  digitalWrite(C,HIGH);
  digitalWrite(D,LOW);
  digitalWrite(E,LOW);
  digitalWrite(F,LOW);
  digitalWrite(G,LOW);
 }
 else
 if (s2)
 {
  digitalWrite(A,HIGH);
  digitalWrite(B,HIGH);
  digitalWrite(C,LOW);
  digitalWrite(D,HIGH);
  digitalWrite(E,HIGH);
  digitalWrite(F,LOW);
  digitalWrite(G,HIGH);
 }
 else
 if(s3)
 {
  digitalWrite(A,HIGH);
  digitalWrite(B,HIGH);
  digitalWrite(C,HIGH);
  digitalWrite(D,HIGH);
  digitalWrite(E,LOW);
  digitalWrite(F,LOW);
  digitalWrite(G,HIGH);
 }
 else
 if(s4)
 {
  digitalWrite(A,LOW);
  digitalWrite(B,HIGH);
  digitalWrite(C,HIGH);
  digitalWrite(D,LOW);
  digitalWrite(E,LOW);
  digitalWrite(F,HIGH);
  digitalWrite(G,HIGH);
 }
}
