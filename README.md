<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo_text.svg" width="320" alt="Nest Logo" /></a>
</p>

## Description

An <a href="https://aws.amazon.com/sqs/" target="blank">AWS SQS</a> module for <a href="http://nestjs.com" target="blank">Nest</a>.

## Installation

```bash
$ npm i --save @coin-market-man/sqs
```

## Quick start

To consume data for a specific queue, you need to register it using the `SQSModule`, and provide a consumer class.

The consumer class is basically a class implementing the SQSMessageHandler interface, and decorated with `@SQSConsumer`. The `@SQSConsumer` annotation simply takes the name of the queue to consume.

For example:

```TS
// imports...

@SQSConsumer('test')
export class TestConsumer implements SQSMessageHandler {
  handleMessage(message: SQS.Message): void {
    console.log(`I received a message : ${message.Body}`);
  }
}

@Module({
  imports: [
    SQSModule.register([
      { name: 'test' }
    ])
  ],
  providers: [TestConsumer]
})
export class AppModule {}
```
The consumer can provide an async `handleMessage` method aswell:
```TS
@SQSConsumer('test')
export class TestConsumer implements SQSMessageHandler {
  constructor (private readonly myAwesomeService: MyAwesomeService) {}

  async handleMessage(message: SQS.Message): Promise<void> {
    await this.myAwesomeService.processMessage(body);
  }
}
```

## License

  Nest is [MIT licensed](LICENSE).
