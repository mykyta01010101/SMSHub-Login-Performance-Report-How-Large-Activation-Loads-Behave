# SMSHub Login Performance Report: How Large Activation Loads Behave

A workflow that performs well with one activation can behave differently once many requests are active at the same time.

The reason is simple: larger workloads introduce more states, more timing differences, and more opportunities for individual requests to fail.

This SMSHub Login performance report focuses on those changes and treats large-scale activation as a coordination problem rather than just a speed test.

## Build a Baseline First

Before increasing the workload, establish how a normal request behaves.

Measure the time required to create the activation, receive the number, get the SMS, and complete the process.

This baseline becomes the reference for later tests.

Without it, a result from a large batch is difficult to interpret because there is nothing to compare it with.

## Gradually Increase Concurrency

Large-scale testing works better when the workload is increased in stages.

Start with a small group of simultaneous activations, collect the results, then increase the number of active requests.

At each stage, compare the same metrics.

This can reveal whether performance changes gradually or only after the workload reaches a certain level.

## Measure Each Part of the Process

Total activation time is useful, but it is not enough for a detailed report.

Break it into separate intervals:

**Request creation → Number assignment**

**Number assignment → SMS delivery**

**SMS delivery → Completion**

This allows the test to identify where additional time is being introduced.

If allocation remains stable while SMS delivery becomes slower, the report can show that difference instead of treating the entire activation as one measurement.

## Request Tracking Becomes More Important

With several active requests, the order of events becomes unpredictable.

The second activation might receive its SMS before the first. A later request might finish while earlier requests remain pending.

Each activation therefore needs a unique identifier.

That identifier should connect the request with its number, status, timestamps, SMS result, and final outcome.

## Monitoring the Active Queue

A larger test can contain many different activation states at once.

A useful monitoring system should distinguish between:

* Requests being created
* Numbers awaiting assignment
* Activations waiting for SMS
* Completed activations
* Expired requests
* Failed requests
* Recovery attempts

This makes it much easier to see which activations still require attention.

## Testing the Effect of Load

Increasing the number of requests is useful only if the results are compared carefully.

For every workload level, track:

| Metric              | Purpose                        |
| ------------------- | ------------------------------ |
| Concurrent requests | Defines test intensity         |
| Assignment latency  | Measures number allocation     |
| SMS latency         | Measures delivery              |
| Completion rate     | Shows successful outcomes      |
| Expiration rate     | Identifies incomplete requests |
| Retry count         | Shows recovery activity        |

The same measurements should be retained for each stage of the test.

## One Failure Should Not Become a Batch Failure

Large workloads need independent recovery.

If one activation expires, it should be possible to identify that request without marking all other active requests as unsuccessful.

This requires individual timers and states.

It also makes the final report more accurate because each activation contributes its own result rather than being absorbed into a single batch status.

## Automation Under Load

Automation can remove repetitive work, but it also has to handle more edge cases as volume grows.

The software may need to create requests, poll their states, retrieve SMS messages, close successful activations, and recover failed ones.

The automation itself should be monitored.

An API error, timeout, or malformed response should be logged separately from an actual SMS delivery failure. Otherwise, different problems can appear identical in the final statistics.

## Avoiding the Average Trap

A single average can make a large workload look more stable than it actually is.

For example, most activations may complete within a short period while a small group takes substantially longer.

A better report identifies the normal range as well as delayed and failed cases.

The goal is to understand the shape of the results, not simply produce the lowest possible summary number.

## Creating a Repeatable Load Test

A practical testing cycle can look like this:

1. Run the baseline workload.
2. Increase concurrent activations.
3. Record stage-level timing.
4. Track every request independently.
5. Separate successful and failed activations.
6. Record recovery attempts.
7. Repeat the same workload levels.
8. Compare the results.

Repeating the stages makes it easier to distinguish a recurring pattern from an isolated event.

## What Large-Scale Testing Can Reveal

The most useful outcome is an understanding of where additional workload changes the workflow.

It may affect allocation, SMS delivery, request tracking, or recovery.

The test does not need to assume the answer in advance. It only needs to measure the same stages consistently and preserve enough information to compare them.

## Final Performance View

SMSHub Login performance at scale should be evaluated as a complete system of concurrent activations.

The important measurements include request timing, SMS latency, activation states, failures, and recovery.

A staged load test with request-level logging provides a much clearer picture of large activation workflows than a single speed measurement or one successful manual request.

