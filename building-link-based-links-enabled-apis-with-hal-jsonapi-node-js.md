
# Building Link-Based and Links-Enabled APIs with HAL and JSON:API in Node.js
As an API developer focused on building maintainable and discoverable backends, I regularly challenge myself to learn new techniques. My latest project at [Hybrid Web Agency](https://hybridwebagency.com/) involved developing a hypermedia-driven Node API from the ground up.

The goal was two-fold - implement the HATEOAS standard comprehensively by adhering to the HAL and JSON:API specifications through testing. This would provide hands-on experience with these emerging best practices.

Over half a year, I created a fully functional order management system based on hypermedia principles. Evaluating response representations and link relations between order, product and customer resources allowed for an in-depth comparison of the specifications. This uncovered important differences in their approaches.

In this article, I will discuss the key insights gained from research and implementation. Code samples demonstrate how each specification structures related resources.

By sharing learnings from this process, my aim is to benefit other API designers exploring RESTful architecture techniques. Furthermore, continuously expanding my standards knowledge will strengthen my skills for building robust and evolvable backends.








### What is HATEOAS?

HATEOAS (Hypertext As The Engine Of Application State) defines an architectural style for RESTful APIs centered around hypermedia. It prescribes that clients use link relations within responses to autonomously navigate an API's resources.

Rather than mandating out-of-band knowledge of endpoints and relationships, HATEOAS services embed instructions clients follow to determine valid actions. For example, retrieving an order resource may disclose links to associated products, payments, or changes.

By including these hyperlinks, APIs provide the means for clients to programmatically discover what they can request next. This makes services highly explorable without predefining access paths.

Significantly, the API structure can evolve independently through new entities or enhanced payloads with minimal client impact. Previously unaware relationships and attributes become accessible via hypermedia prompts.

Adopting this "link-driven" design decouples clients from mandates to understand the conceptual API model upfront. It fosters independence from static definitions through an emphasis on fluid linkage between resources.

Overall, HATEOAS affords RESTful APIs inherent discoverability, resilience to change, and flexibility in how their domain can develop—guided solely by the hypertext exchanged through each request and response.



### Benefits of HATEOAS

By implementing the HATEOAS paradigm, REST APIs realize significant advantages:

**Interactive Documentation** - Responses implicitly document themselves through embedded hypermedia links, eliminating separate specification needs.

**Flexible Evolution** - The API can organically grow independent of clients by augmenting payloads with fresh link relations over time. 

**Resilience to Refactoring** - Modifying the domain model through new entities or consolidated routes is seamlessly recognized by all clients.

**Progressive Capability Exposure** - Hypermedia responses can reveal expanded functionality post-launch, adopting an inside-out design paradigm.

**Serendipitous Interface Exploration** - Clients may discover overlooked actions or attributes through dynamically following affinity relationships.

**Context-Driven Traversal** - HATEOAS frees clients to reactively prioritize resource transitions optimized for situational needs. 

Overall, the hypermedia-centric approach engenders inherently evolvable, teachable and long-lasting REST interfaces able to flexibly self-document while progressively exposing their full potential over the service lifecycle.



## Choosing a Hypermedia Representation

When implementing HATEOAS in a Node API, two popular options for representing hypermedia links are the Hypertext Application Language (HAL) format and the JSON:API specification.

### HAL 

The simple HAL format represents relationships between resources using a top-level "_links" property containing URLs.

   ````json
   {
     "_links": {
       "self": {"href": "/orders/1"},
       "items": {"href": "/orders/1/items"}
     }
   }
   ````
   
HAL gets the job done with minimal overhead, making it suitable for basic hypermedia needs.

### JSON:API

JSON:API defines a complete standard for JSON APIs, representing relationships through a dedicated "relationships" object holding related IDs and link data.

   ````
   "relationships": {
     "items": {
       "links": {
         "related": "/orders/1/relationships/items"  
       },
       "data": [...ids]
     }
   }
   ````
   
It supports richer feature sets but entails greater implementation complexity than HAL.





## Developing a HAL-Based API in Node.js

This section will outline how to build an API that complies with the HAL specification using Express.

### Including Necessary Libraries

The `express-hal-builder` module will be used to generate HAL representations.

```
npm install express-hal-builder
```

### Defining Resource Schemas

Schemas for Order and OrderItem resources are defined using object structures.

```js
const Order = {};
const OrderItem = {};
```

### Generating Hypermedia Links

The HAL builder library is used to represent entities and embed related links:

```js
app.get('/orders/:id', (req, res) => {

  const order = //...

  const builder = new HalBuilder();

  builder.representEntity(order)
    .childLink(order.items, '/orders/'+id+'/items');  

  res.json(builder.get());

});
```

This will establish the core components needed to develop a RESTful API following the HAL specification - defining resources, relating them through embedded links, and outputting the hypermedia representation.



## Exploring the Relationship Graph in a HAL API

Making initial GET requests to uncover the starting endpoints. Parsing responses to:

- Identify the available relationship types
- Extract the URIs used to link related resources
- Gradually reveal the domain model by traversing links

## Developing a Linked Data API following JSON:API

This guide demonstrates how to build a RESTful backend that represents relationships between resources as defined by the JSON:API specification using Node and Express.

### Importing Required Modules

The `jsonapi-serializer` library helps generate standardized JSON:API responses.

### Modeling Core Entity Types

Schemas define the structure of resources like Orders and LineItems.

### Generating JSON:API Documents

Responses relate data across entities by serializing a resource and:

1. Referencing associated relationships  

2. Embedding related resource identifiers

```js
app.get('/orders/:id', async (req, res) => {

  const order = await Order.findById(req.params.id);

  res.json(order.serialize({
    include: 'lineItems'
  }));

});
```

This establishes a foundation for developing a linked data API following JSON:API through standardized linkage of resources.

# Conclusion
In summary, developing RESTful APIs according to established specifications provides structure, discoverability and future-proofs applications for emerging technologies. Adhering to standards helps ensure intuitive and navigable services.

Building complex, specification-driven backends that connect resources through relationships requires diverse expertise. Partnering with an experienced Node.js firm can help expedite this process.

As a full-service Node.js development company located in Arlington, TX, we offer [Node.js Development Services in Arlington, TX](https://hybridwebagency.com/arlington-tx/custom-laravel-development-services/) to help build robust, specification-aligned APIs. Our senior engineers have extensive experience implementing HAL, JSON:API and other norms to represent relations between entities. Whether for prototyping, scaling existing services, or managing full projects, we aim to deliver production-ready REST endpoints on time and on budget.

By leveraging our expertise in Node.js, related frameworks and architectural best practices, your team can focus on core work while we ensure technical execution meets quality and performance standards. Contact us to discuss how our Arlington, TX-based services could benefit your project goals.

## References

- https://www.halspec.org/
The official specification website for HAL (Hypertext Application Language), detailing the standard.

- https://jsonapi.org/
The official website for the JSON:API specification, including documentation on compliant API design. 

- https://expressjs.com/en/advanced/best-practice-performance.html
Best practices guide from the Express.js website covering performance, security, and standard compliance for Node.js APIs.  

- https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design
Documentation from Microsoft on API design best practices, including modeling relationships and using standards.

- https://swagger.io/resources/building-blocks/hypermedia/
Swagger overview of hypermedia APIs and industry standards like HAL and JSON:API for enabling discoverability.


