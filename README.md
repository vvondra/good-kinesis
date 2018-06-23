# good-kinesis-reporter

Kinesis and Kinesis firehose stream for Good by Hapi.js

![Current Version](https://img.shields.io/npm/v/good-kinesis-reporter.svg)

## Usage

`good-kinesis-reporter` is a write stream use to send events to AWS Kinesis and AWS Kinesis Firehose.

### Example

```javascript
// server.js

const Hapi = require('hapi');
const server = new Hapi.Server();
server.connection();

const options = {
    reporters: {
        kinesis: [{
            module: 'good-squeeze',
            name: 'Squeeze',
            args: [{ log: '*' }]
        }, {
            module: 'good-kinesis-reporter',
            name: 'Firehose', // can be either Kinesis or Firehose
            args: [
              { streamName: 'my-delivery-stream' } // only streamName is mandatory, the rest are the SDK client options
            ]
        }]
    }
};

const init = async () => {
    await server.register({
        register: require('good'),
        options
    });

    await server.start();
    console.log(`Server running at: ${server.info.uri}`);
};

process.on('unhandledRejection', (err) => {
    console.log(err);
    process.exit(1);
});

init();
```

## Using as a write stream without good and available options

Creates a new GoodKinesis or GoodFirehose object:

```javascript
const goodkinesis = require('good-kinesis-reporter');

const kinesisStream = goodkinesis.Kinesis({ streamName: 'my-kinesis-stream' });
const firehoseStream = goodkinesis.Firehose({ streamName: 'my-firehose-stream' });

Available options:

- `streamName` - Stream name

All options are passed to the [AWS SDK constructor](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Kinesis.html#constructor-property)

As best practice, configuring the AWS SDK client is recommended using environment variables or instance profile permissions in an EC2 context.

## License

[MIT](LICENSE.txt)
