## Functional requirements

Map directly to the main user journey in the product.

### Determine the <mark>development stage</mark> of the product.

This clarifies the desired speed of iteration, performance, resilience and maintainability.

- [ ] _Proof of Concept (PoC)_: a small exercise to test an incomplete idea
- [ ] _Prototype_: an idea in early stages of development
- [ ] _Minimum Viable Product (MVP)_: a functional core solution that can be iterated on
- [ ] _Production-ready_: a fully-fledged product, with a large user base
- [ ] _Mission-critical_: a product that must function continuously to ensure business continuity

### Make a list of the main <mark>functional requirements</mark>.

These map to a typical user journey in the application. For example, for an e-commerce website, these are the steps from registration to checkout.


### Determine how many <mark>users per unit of time</mark> the system serves currently.

The unit of time can be month, day, second.

- [ ] Hundreds
- [ ] Thousands
- [ ] Hundred of thousands
- [ ] In the millions
- [ ] In the billions

### Determine the <mark>geographical location</mark> of the users. 

This influences the choice of network topology and Content Distribution Network (CDN).

- [ ] Users are co-located in a small geography, like a country
- [ ] Users are distributed across the world

### Determine how the system <mark>load fluctuates</mark> in the next six to twelve months.

This informs a choice of architecture that scales relatively easy without major refactoring.

- [ ] The system load will increase slowly
- [ ] The system load will increase by at least an order of magnitude

### Determine if the product has specific <mark>usage patterns</mark>.

This gives an idea about potential cost optimisations.

- [ ] The product has constant use throughout the year
- [ ] The product has bursts of activity, for example at certain times during the day
- [ ] The product has seasonal peaks, for example summer/winter

### Determine the <mark>input data type</mark> sent to the system.

This clarifies if the system processes and aggregates _homogenous_ or _heterogenous_ data.

### Determine the <mark>input data format</mark> sent to the system.

- [ ] Text/JSON/XML
- [ ] Binary

### Determine the <mark>output data type</mark> sent back to the clients.

- [ ] Text
- [ ] Images
- [ ] Audio/Video

### Determine the <mark>output data format</mark> sent back to the clients.

- [ ] Text/JSON/XML
- [ ] Binary

### Determine the <mark>data update frequency</mark> handled by the system.

- [ ] Data changes infrequently and can be processed in batches
- [ ] Data changes in real-time and is streamed continuously

### Determine the <mark>types of clients</mark> supported.

- [ ] Web browsers
- [ ] Smartphones and tablets
- [ ] Desktop applications
- [ ] Wearables
- [ ] IoT devices

### Determine if the system offers <mark>multi-platform consistent user experience</mark>.

<div class="note" markdown="1">

For example, a notification system will have different requirements than a financial app.

If users clear a notification on one device then switch to another, it's expected they won't see the same notification as new, even though it might take a few seconds to update.

On the other hand, it's common for banking apps to allow usage of one device at the time. If the
user switches devices, they will be expected to go through authentication and device authorisation before using the app on the new device.

</div>

- [ ] The user switches seamlessly between devices in the middle of a task
- [ ] The user uses the product on one device at a time

### Determine if the system works <mark>offline</mark>.

For example, like Google Calendar.

- [ ] The user uses the product offline
- [ ] The product is operational with an Internet connection

### Determine if the system supports <mark>real-time collaboration</mark>.

For example, like Google Docs.

- [ ] The user uses the product individually
- [ ] Multiple users are collaborating seamlessly in real time

## Operational requirements
{: .no_toc }

Describe how the system handles operational demands. Examples: failures, data and security requirements.

### Determine how the system should <mark>handle failure</mark>.

- [ ] The system operates with minimal interruption (highly-available)
- [ ] The system operates without any interruption (fault-tolerant)

### Determine if the product provides <mark>Service Level Agreements (SLAs)</mark>.

For ease of calculations, consider the expected uptime in round units. This informs choices for _redundancy_ and _recovery_.

- [ ] 99% (days of downtime per year)
- [ ] 99.9% (hours of downtime per year)
- [ ] 99.999% (minutes of downtime per year)
- [ ] 99.99999% (seconds of downtime per year)
- [ ] 99.9999999% (milliseconds of downtime per year)

### Determine the <mark>security standards</mark> to comply with.

<div class="note" markdown="1">

For example, a financial system might need PCI-DSS, CyberSecurity etc. 

This informs choices for security, auditing logging, monitoring, auditing, restricting dependencies with Common Vulnerabilities and Exposures (CVEs). 

It also helps estimating the amount of storage space required. If the data needs to be persisted for a certain amount of years and the auditing process is not real-time, older data can be archived to infrequent/cold storage, to optimise costs.

</div>

- [ ] The system must preserve logs for a certain period
- [ ] The system requires external pen-testing and auditing
- [ ] Auditing data doesn't need to be retrieved instantly

### Determine the <mark>data protection standards</mark> to comply with.

<div class="note" markdown="1">

For example, a medical system might need HIPAA and GDPR. This informs how Personal Identifiable Information (PII) is treated, for example masked or scrubbed.

The saying is that _the best way to deal with data privacy and security is not to store it at all_. 
Other considerations are data ethics and individual privacy. 

**Data Ethics Canvas**

It's a great [resource](https://theodi.org/article/data-ethics-canvas/) to start with when considering business, product and technical decisions about data.

**Data Reconstruction Theorem**

There's a mathematical theorem which guarantees that every piece of accurate information you release, however small, will inherently violate the privacy of the participants to some degree.

_The trade-off of data privacy is data accuracy_. To mitigate the DRT, a common approach is Differencial Privacy, which basically adds some random jitter to the data to make it _less accurate_.

**Federated Learning**

Another consideration to data privacy is if _raw data_ goes a centralised point for processing, for example, machine learning training. 

Federated learning solves this way by incrementally training a shared model with pulled resources where the model is combined into an average. The raw data never leaves the source, e.g. a smartphone.

- [ ] The system does not handle PII
- [ ] The system handles PII
- [ ] Data Ethics Canvas considered
- [ ] Differential Privacy considered
- [ ] Federated Learning considered

## Extended requirements
{: .no_toc }

Describe secondary functionality important for operations but not necessarily visible to the end-users. Examples: collecting metrics and analytics.

### Determine if the system uses <mark>data for business decisions</mark>.

- [ ] The business needs real-time analytics
- [ ] The business uses analytics but not real-time

### Determine if the business serves <mark>data to other third-parties</mark>. 

This informs choices for the API design. For example, API protocols, throttling, secure access etc.

- [ ] The business provides external APIs to third-party developers
- [ ] The business does not provide external APIs

### Determine if this system has <mark>cost constraints</mark>.

### Check if there are other <mark>specific requirements</mark> omitted in this list.