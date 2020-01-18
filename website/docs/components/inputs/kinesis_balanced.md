---
title: kinesis_balanced
type: input
---

BETA: This input is a beta component and is subject to change outside of major
version releases.

Receives messages from a Kinesis stream and automatically balances shards across
consumers.


import Tabs from '@theme/Tabs';

<Tabs defaultValue="common" values={[
  { label: 'Common', value: 'common', },
  { label: 'Advanced', value: 'advanced', },
]}>

import TabItem from '@theme/TabItem';

<TabItem value="common">

```yaml
input:
  kinesis_balanced:
    stream: ""
    dynamodb_table: ""
    start_from_oldest: true
    region: eu-west-1
    batching:
      count: 1
      byte_size: 0
      period: ""
```

</TabItem>
<TabItem value="advanced">

```yaml
input:
  kinesis_balanced:
    stream: ""
    dynamodb_table: ""
    dynamodb_billing_mode: ""
    dynamodb_read_provision: 0
    dynamodb_write_provision: 0
    start_from_oldest: true
    region: eu-west-1
    endpoint: ""
    credentials:
      profile: ""
      id: ""
      secret: ""
      token: ""
      role: ""
      role_external_id: ""
    batching:
      count: 1
      byte_size: 0
      period: ""
      condition:
        static: false
        type: static
```

</TabItem>
</Tabs>

Messages consumed by this input can be processed in parallel, meaning a single
instance of this input can utilise any number of threads within a
`pipeline` section of a config.

Use the `batching` fields to configure an optional
[batching policy](/docs/configuration/batching#batch-policy).

### Metadata

This input adds the following metadata fields to each message:

```text
- kinesis_shard
- kinesis_partition_key
- kinesis_sequence_number
```

You can access these metadata fields using
[function interpolation](/docs/configuration/interpolation#metadata).

## Fields

### `stream`

`string` The Kinesis stream to consume from.

### `dynamodb_table`

`string` A DynamoDB table to use for offset storage.

### `dynamodb_billing_mode`

`string` A billing mode to set for the offset DynamoDB table.

### `dynamodb_read_provision`

`number` The read capacity of the offset DynamoDB table.

### `dynamodb_write_provision`

`number` The write capacity of the offset DynamoDB table.

### `start_from_oldest`

`bool` Whether to consume from the oldest message when an offset does not yet exist for the stream.

### `region`

`string` The AWS region to target.

### `endpoint`

`string` Allows you to specify a custom endpoint for the AWS API.

### `credentials`

`object` Optional manual configuration of AWS credentials to use. More information can be found [in this document](/docs/guides/aws).

### `credentials.profile`

`string` A profile from `~/.aws/credentials` to use.

### `credentials.id`

`string` The ID of credentials to use.

### `credentials.secret`

`string` The secret for the credentials being used.

### `credentials.token`

`string` The token for the credentials being used, required when using short term credentials.

### `credentials.role`

`string` A role ARN to assume.

### `credentials.role_external_id`

`string` An external ID to provide when assuming a role.

### `batching`

`object` Allows you to configure a [batching policy](/docs/configuration/batching).

```yaml
# Examples

batching:
  byte_size: 5000
  period: 1s

batching:
  count: 10
  period: 1s

batching:
  condition:
    text:
      arg: END BATCH
      operator: contains
  period: 1m
```

### `batching.count`

`number` A number of messages at which the batch should be flushed. If `0` disables count based batching.

### `batching.byte_size`

`number` An amount of bytes at which the batch should be flushed. If `0` disables size based batching.

### `batching.period`

`string` A period in which an incomplete batch should be flushed regardless of its size.

```yaml
# Examples

batching.period: 1s

batching.period: 1m

batching.period: 500ms
```

### `batching.condition`

`object` A [`condition`](/docs/components/conditions/about) to test against each message entering the batch, if this condition resolves to `true` then the batch is flushed.

