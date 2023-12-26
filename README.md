# ARD.102b_Semaforo

Este projeto implementa um modelo de semáforo de carros e pedestres, utilizando POO e millis() para controle do tempo, além dos conceitos de máquinas de estado infinitas (ISM).

Diagrama sketch

![image](https://github.com/maf27br/ARD.102b_Semaforo/assets/68168344/14546d59-b4b8-4df8-b2c5-7f3175b7e373)

Código comentado:
```
//variáveis globais
unsigned long tempo0,tempo1,tempo2,tempo3, delayStart, delayWait;
int ledped_VM, ledped_VD, ledVM, ledAM, ledVD;
//cria classe
class semaforo_ME
{
  int ME_state;

  public:
    semaforo_ME(int p_ledpedVM, int p_ledpedVD, int p_ledVM, int p_ledAM, int p_ledVD, unsigned long t_carro,unsigned long t_alerta,unsigned long t_pedestre, int estado_inicial)
    {
      //pinos do farol
      //Pedestres
      ledped_VM = p_ledpedVM; //Vermelho do pedestre
      ledped_VD = p_ledpedVD; //Verde do pedestre
      //Carros
      ledVM = p_ledVM; //Led Farol Vermelo
      ledAM = p_ledAM; //Led Farol Amarelo
      ledVD = p_ledVD; //Led Farol Verde
      //tempos
      tempo0 = 10;
      tempo1 = t_carro;
      tempo2 = t_alerta;
      tempo3 = t_pedestre;
      ME_state = estado_inicial;
    }

    void aciona_ME()
    {
      if( delayWait > 0)
      { 
        if(millis() - delayStart < delayWait)
        {
          return;
        }
        else
        {
          delayWait = 0;
        };

      }
      
      
      switch (ME_state)
      {
        case 0: //estado inicial
          {
            digitalWrite(ledVD, LOW);
            digitalWrite(ledVM, LOW);
            digitalWrite(ledAM, LOW);
            digitalWrite(ledped_VD, LOW);
            digitalWrite(ledped_VM, LOW);
            Serial.println(F("case 0"));
            delayStart = millis();
            delayWait = tempo0;
            ME_state = 1;//muda estado aberto carros e fechado pedestres
            break;
          }
        case 1: //estado aberto carros e fechado pedestres
          {
            digitalWrite(ledVD, HIGH);
            digitalWrite(ledVM, LOW);
            digitalWrite(ledAM, LOW);
            digitalWrite(ledped_VD, LOW);
            digitalWrite(ledped_VM, HIGH);
            Serial.println(F("case 1"));
            delayStart = millis();
            delayWait = tempo1;
            ME_state = 2;//muda estado alerta carros e fechado pedestres            
            break;
          }
        case 2://alerta fechamento carros e fechado pedestres
          {
            digitalWrite(ledVD, LOW);
            digitalWrite(ledVM, LOW);
            digitalWrite(ledAM, HIGH);
            digitalWrite(ledped_VD, LOW);
            digitalWrite(ledped_VM, HIGH);
            Serial.println(F("case 2"));
            delayStart = millis();
            delayWait = tempo2;
            ME_state = 3;//muda estado aberto carros e fechado pedestres
            break;
          }
        case 3://fechado carros e aberto pedestres
          {
            digitalWrite(ledVD, LOW);
            digitalWrite(ledVM, HIGH);
            digitalWrite(ledAM, LOW);
            digitalWrite(ledped_VD, HIGH);
            digitalWrite(ledped_VM, LOW);
            Serial.println(F("case 3"));
            delayStart = millis();
            delayWait = tempo3;
            ME_state = 1;//muda estado aberto carros e fechado pedestres
            break;
          }
      }
    }

    void encerra_ME()
    {
      ME_state = 0;//desliga maquina
      digitalWrite(ledVD, LOW); //faz atualização do Led
      digitalWrite(ledVM, LOW); //faz atualização do Led
      digitalWrite(ledAM, LOW); //faz atualização do Led
      digitalWrite(ledped_VD, LOW); //faz atualização do Led
      digitalWrite(ledped_VM, LOW); //faz atualização do Led
    }
};

semaforo_ME ME1 (8,9,10,11,13, 20000, 5000, 15000, 0);

void setup() {
  // put your setup code here, to run once:
  Serial.begin(115200);
  pinMode(ledVM, OUTPUT);
  pinMode(ledVD, OUTPUT);
  pinMode(ledAM, OUTPUT);
  pinMode(ledped_VM, OUTPUT);
  pinMode(ledped_VD, OUTPUT);
  Serial.println((F("entrei no setup e carreguei os pinos")));
}

void loop() {
ME1.aciona_ME();
}
```
