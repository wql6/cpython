# markdown 文档

#### 背景

> kafkaProducer 发送者有一份代码中提到

```text
 public Producer(String topic, Boolean isAsync) {
        Properties props = new Properties();
        props.put("bootstrap.servers", "localhost:9092");
        props.put("client.id", "DemoProducer");
        props.put("key.serializer", "org.apache.kafka.common.serialization.IntegerSerializer");
        props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
//        props.put(ProducerConfig.LINGER_MS_CONFIG, "100000");
        producer = new KafkaProducer<>(props);
        this.topic = topic;
        this.isAsync = isAsync;
    }

    public void run() {
        int messageNo = 1;
//        while (true) {
        for(int i = 0 ;i<10 ;i++){
            System.out.println("产生数据：" + i);
        String messageStr = "Message_" + messageNo;
        long startTime = System.currentTimeMillis();
        if (isAsync) { // Send asynchronously 异步发送
            producer.send(new ProducerRecord<>(topic,
                messageNo,
                messageStr), new DemoCallBack(startTime, messageNo, messageStr));
        } else { // Send synchronously   // 同步发送
            try {
                producer.send(new ProducerRecord<>(topic,
                    messageNo,
                    messageStr)).get();
                System.out.println("Sent message: (" + messageNo + ", " + messageStr + ")");
            } catch (InterruptedException | ExecutionException e) {
                e.printStackTrace();
            }
        }
        ++messageNo;
        }
```

其中producer.send\(new ProducerRecord&lt;&gt;\(topic, messageNo, messageStr\)\).get\(\)

