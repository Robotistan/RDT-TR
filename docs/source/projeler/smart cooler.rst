###########
Smart Cooler
###########

Giriş
-------------
Projemizde öncelikle Picobricks üzerindeki DHT11 sıcaklık ve nem sensörünün ölçtüğü sıcaklık değerlerini görüntüleyeceğiz. Daha sonra sonra bir sıcaklık sınırı belirleyerek DHT11 modülünden gelen sıcaklık değeri bu sınıra ulaştığında Picobricks’e bağlı DC motorun dönmeye başlaması, sıcaklık değeri belirlediğimiz sınırın altına indiğinde ise DC motorun durması için gerekli kodları yazacağız.

Projenin Detayları ve Algoritması
------------------------------

Yaz aylarında serinlemek için kış aylarında ısınmak için klimalar kullanılır. Klimalar ısıtma ve soğutma derecesini bulunduğu ortamın sıcaklığına göre ayarlamaktadır. Fırınlar yemeği pişirirken kullanıcının ayarladığı sıcaklık değerine çıkmaya ve o sıcaklığı korumaya çalışırlar. Bu iki elektronik cihazda sıcaklığı kontrol etmek için özel sıcaklık sensörleri kullanmaktadır. Ayrıca seralarda sıcaklık ve nem birlikte ölçülür. Bu iki değer  istenen düzeyde dengede kalabilmesi için fan ile hava akımını sağlanmaya çalışılır.

PicoBricks’te sıcaklığı ve nemi ayrı ayrı ölçebilir ve bu ölçümler ile çevreyle etkileşime girebilirsiniz. Bu projede PicoBricks ile sıcaklığa göre fan hızını otomatik ayarlayan bir serinletme sistemi hazırlayacağız. Böylelikle DC motor çalışma sistemini ve motor hız ayarı yapmayı öğreneceksin.



Bağlantı Diyagramı
--------------

.. figure:: ../_static/smart-cooler.png      
    :align: center
    :width: 500
    :figclass: align-center
    



Picobricks modüllerini herhangi bir kablo bağlantısı olmadan programlayabilir ve çalıştırabilirsiniz. Modülleri karttan ayırarak kullanacaksanız modül bağlantılarını verilen konektör kablolar ile yapmalısınız.

Projenin MicroPython Kodu
--------------------------------
.. code-block::

    from machine import Pin
    from picobricks import DHT11
    import utime

    LIMIT_TEMPERATURE = 20 #define the limit temperature

    dht_sensor = DHT11(Pin(11, Pin.IN, Pin.PULL_DOWN))
    m1 = Pin(21, Pin.OUT)
    m1.low()
    dht_read_time = utime.time()
    #define input-output pins

    while True:
    if utime.time() - dht_read_time >= 3:
        dht_read_time = utime.time()
        dht_sensor.measure()
        temp= dht_sensor.temperature
        print(temp)
        if temp >= LIMIT_TEMPERATURE:     
            m1.high()
            #operate if the room temperature is higher than the limit temperature
        else:
            m1.low()
            


.. tip::
  Eğer kodunuzun adını main.py olarak kaydederseniz, kodunuz her ``BOOT`` yaptığınızda çalışacaktır.
   
Projenin Arduino C Kodu
-------------------------------


.. code-block::

    #include <DHT.h>
    #define LIMIT_TEMPERATURE     27
    #define DHTPIN 11
    #define DHTTYPE DHT11

    DHT dht(DHTPIN, DHTTYPE);
    float temperature;

    void setup() {
    // put your setup code here, to run once:
    Serial.begin(115200);
    dht.begin();
    pinMode(21,OUTPUT);

        }

    void loop() {
    // put your main code here, to run repeatedly:
    delay(100);
    temperature = dht.readTemperature();
    Serial.print("Temp: ");
    Serial.println(temperature);
    if(temperature > LIMIT_TEMPERATURE){
    digitalWrite(21,HIGH);
    } 
    else{
    digitalWrite(21,LOW);    
        }


    }

Projenin MicroBlocks Kodu
------------------------------------
+---------------+
||smart-cooler1||     
+---------------+

.. |smart-cooler1| image:: _static/smart-cooler1.png



.. note::
    MicroBlocks ile kodlama yapmak için yukarıdaki görseli MicroBlocks Run sekmesine sürükleyip bırakmanız yeterlidir.
  

    
