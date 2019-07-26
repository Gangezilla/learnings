# Notes from Distributed Systems Observability by Cindy Sridharan

[Distributed Systems Observability by Cindy Sridharan](https://distributed-systems-observability-ebook.humio.com/?utm_campaign=E-book%20Cindy%20download%201st%20round%202018&utm_source=Cindy%27s%20Promotion%20of%20the%20e-book&utm_medium=Cindy%27s%20Promotion%20of%20the%20e-book)

## Notes

### Chapter 1

- Observability is basically bringing better visibility into systems.
- Logs, metrics and traces are useful, but alone, they don't make for observable systems.
- Observability is a property of a system that acknowledges:
  - No complex system is ever fully healthy
  - Distributed systems are pathologically unpredictable
  - It's impossible to predict the states of partial failure various parts of a system can end up in
  - Failure must be embraced at all stages (design, implementation, testing, deployment, operation)
  - Easy debugging makes for easy maintenance and evolution of a system.
- Observability is a feature that needs to be baked into a system at the time of design. This means a system can be:

  - tested realistically (including in prod)
  - tested so that hard failures acn be surfaced during testing
  - deployed to enable easy roll backs and roll forwards.
  - able to report enough data about it's health when in production.

### Chapter 2

- Observability is a superset of monitoring and testing, providing info about unpredictable failure modes, and unlocking the ability to uncover deeper, systemic issues.
- Blackbox monitoring is observing a system from the outside (services like Pingdom)
- Whitebox monitoring is techniques of reporting data from inside a system.
- When a system fails, we can handle it in a few ways:
  - Tolerate it, with mechanism like eventual consistency or aggressive multitiered caching
  - Alleviate it, with graceful degradation mechanism like applying backpressure, retries, timeouts, circuit breaking and rate limiting
  - Trigger it deliberately, with load shedding in the event of increased load.
- Monitoring should provide a bird's eye view of the overall health of a system by exposing high level metrics of all components. It should also provide ability to drill down into components to aid diagnosis of a problem.
- Different methodologies to decide which monitoring signals are important:
  - Site Reliability Engineering book recommends "the four golden signals", latency, errors, traffic, and saturation.
  - USE methodology, measuring Utilisation, Saturation and Errors of system resources, like utilisation (available free memory), CPU run queue length (saturation), or device errors(errors)
  - RED methodology, Request rate, Error rate, Duration of request.
- The degree of a system's observability is the degree to which it can be debugged.
- Over distributed systems we don't have a debugger, so observability data is a replacement.
- This still requires a good understanding of the system which is aided by good abstractions, such as visualisation tooling.

### Chapter 3

- Verifying in envrionments similar to staging is like a dress rehearsal. It's good, but not quite the same as performing in front of a full house.
- This involves not just architecting for failure, but coding and testing for failure, instead of coding and testing for success.
- Coding for failure involves;
  - Understanding the operational semantics of the application
  - Understanding the operational characteristics of the dependencies.
  - Writing debuggable code.
- Operational semantics:
  - How is a service deployed, and using what tooling?
  - Does the service bind to port 0 or a standard port?
  - How an application handles signals
  - How the process starts on a given host
  - How it registers with service discovery
  - How it discovers upstreams
  - How the service is drained of connections
  - How graceful (or not) restarts are?
  - How is static and dynamic config fed to the process.
  - Concurrency model of the application
  - How the reverse proxy handles connections
- Understanding dependencies can lead to reliability gains from simple changes.
- Complex systems fail in complex ways. Relying on pre-production testing is ineffective in surfacing many issues.
- There are a few ways of testing in prod, as described in this image:

![Testing in production]("./testing-in-prod.png")

### Chapter 4

- The pillars of observability are logs, metrics and traces.

#### Logs

- Logs can be either:
  - Plaintext
  - Structured (JSON)
  - Binary
- Event logs are useful for uncovering emergent and unpredictable behaviours exhibited by components of a system.
- Logs perform really well to surface highly granular data, but apart from that can be really noisy and often not performant, especially if they're not async.
- Sampling is the technioque of picking a small subset of all generated logs to be processed and stored
- There are often striking similarities between questions a business might want answered and questions engineers might want answered when debugging.

#### Metrics

- Metrics are a numeric representation of data measured over intervals of time.
- The biggest drawback of both logs and metrics are that they're system scoped, making it hard to understand anything else other than what's happening inside a particular system.
- A single log doesn't give you much context of what's happening across all components of a system.

#### Tracing

- A trace is a representation of a series of causally related distributed events that encode the E2E request flow through a distributed system.
- Traces are a representation of logs.
- The basic idea behind tracing is straightforward - identify specific points that represent forks in execution flow, or a hop/fan out across network or process boundaries.
- Traces are used to identify the amouynt of work done at each layer.
- Tracing is really hard to retrofit into an existing infrastructure, because for it to be effective, every component in the request path needs to be modified to pass on tracing info.

- It makes sense to use logs, metrics and traces all together. This will make debugging significantly easier because you can reconstruct a codepath, and derive where an error stems from.

## Questions

- What's a roll forward?
- What's eventual consistency?
- What's aggressive multitiered caching?
- Graceful degradation, what's that all about? Backpressure? Circuit breaking?
- What's load shedding?
- What causes something to use up available free memory?
- What causes CPU run queue length to go up?
- When would a service bind to port 0 vs a standard port?
- What's a signal, and how does an application handle one?
- How does a process start?
- What's service discovery? How does an application register with it?
- What's an upstream? How does one discover them?
- How is a service drained of connections? What does this even mean?
- How is a restart graceful (or not)?
- Static and dynamic config?
- Concurrency models?
- How does a reverse proxy handle connections (pre-forked, threaded, process based)
- What is "strongly consistent"? Why woulkd I not necessarily want this in the context of "service discovery"?
- What's a service mesh?
