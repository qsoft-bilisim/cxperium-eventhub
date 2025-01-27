# cxperium-eventhub
eventhub  is a comprehensive and centralized system that records and analyzes incoming event requests, subsequently directing them to relevant local services and efficiently distributing workloads.

Bu doküman, TypeScript ve NodeJS kullanılarak geliştirilecek olan EventBus sisteminin tasarım ve işleyişine dair detayları içermektedir. 

EventBus, entegre olayları (integration events) işleyerek bir veritabanına (muhtemelen MongoDB) yazacak ve belirli bir Message Queue (MQ) sistemi (örneğin, RabbitMQ veya Azure Service Bus) üzerinden bu olayları yollayacaktır.

## 2. Sistem Mimari ve Bileşenler

### 2.1 Out of Box
- **EventBus**: TypeScript ve NodeJS ile geliştirilecek.
- **Integration Event'leri**: Bir veritabanına (MongoDB) yazılacak. DB islemleri yine abstract yazilacaktir. Baska DB lerle de istenilirse calisilabilmelidir.
- **SendAsync**: Veritabanındaki kayıtları kullanarak işlem yapacak ve ilgili MQ'ya mesaj gönderecek.

### 2.2 Abstraction
- **Abstraction**: RabbitMQ veya başka bir Message Queue (MQ) kullanmak üzere soyutlanacak. Bu soyutlama, sisteme farklı bir MQ eklendiğinde, derleme zamanında (run-time'da değil) inject edilebilir olacak.
- **Farklı MQ Entegrasyonları**: Gerektiğinde farklı bir MQ ile sistem kolayca entegre edilebilir.

## 3. SendAsync Metodu

### 3.1 Görevleri
- **Mesajları Veritabanından Okuma**: `SendAsync` metodu, veritabanından mesajları okur. Ayni Session a sahip birden fazla mesaj olabilir. Sistem bu event mesajlarini sirayla okur ve sirayla calistirir.
- **Mesajları İşleme**: Mesajları anlamli bir hale getirir. (Mesajlar Std base e sahiptir, her mesaj kendi body ve attribute icerebilir ama bu std olmasina engle degildir.)
- **MQ'ya Mesaj Gönderme**: Mesajları MQ'ya Topic olarak yazar. (Cok onemli!)

### 3.2 Kullanımı
- **BaseEvent**: `SendAsync` metodu, `BaseEvent` sınıfından türetilmiş bir nesne kullanmak zorundadır.

## 4. Gelecekteki Geliştirmeler
- **Daha İyi MQ Entegrasyonları**: EventBus yapısı, gelecekte farklı MQ sistemleri ile de kolayca entegre olacak şekilde tasarlanmıştır.
- **Gelişmiş Hata Yönetimi**: Olayların ve mesajların güvenilir bir şekilde işlenmesi için hata yönetimi mekanizmaları eklenecektir.

