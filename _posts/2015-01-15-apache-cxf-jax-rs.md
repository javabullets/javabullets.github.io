---
id: 388
title: 'Apache CXF -RESTful Services with  JAX RS'
date: '2015-01-15T20:35:06+00:00'
author: 'Martin Farrell'
layout: post
guid: 'https://glenware.wordpress.com/?p=388'
permalink: /apache-cxf-jax-rs/
thrive_post_fonts:
    - '[]'
categories:
    - Apache
    - 'Web Services'
tags:
    - 'Apache CXF'
    - CXF
---

The final feature of Apache CXF I would like to cover is the use of CXF for RESTful services JAX-RS(JSR-RS). The samples directory contains a number of different demonstrations, but I’ll discuss the basic demo.

**Web Service Implementation**

The web service implementation classes are defined using javax.ws.rs annotations – DELETE, GET, POST, PUT

\[sourcecode lang=”java”\] @Path("/customerservice/")  
@Produces("text/xml")  
public class CustomerService {  
 long currentId = 123;  
 Map&lt;Long, Customer&gt; customers = new HashMap&lt;Long, Customer&gt;();  
 Map&lt;Long, Order&gt; orders = new HashMap&lt;Long, Order&gt;();  public CustomerService() {  
 init();  
 }

 @GET  
 @Path("/customers/{id}/")  
 public Customer getCustomer(@PathParam("id") String id) {  
 System.out.println("—-invoking getCustomer, Customer id is: " + id);  
 long idNumber = Long.parseLong(id);  
 Customer c = customers.get(idNumber);  
 return c;  
 }

 @PUT  
 @Path("/customers/")  
 public Response updateCustomer(Customer customer) {  
 System.out.println("—-invoking updateCustomer, Customer name is: " + customer.getName());  
 Customer c = customers.get(customer.getId());  
 Response r;  
 if (c != null) {  
 customers.put(customer.getId(), customer);  
 r = Response.ok().build();  
 } else {  
 r = Response.notModified().build();  
 }

 return r;  
 }

 @POST  
 @Path("/customers/")  
 public Response addCustomer(Customer customer) {  
 System.out.println("—-invoking addCustomer, Customer name is: " + customer.getName());  
 customer.setId(++currentId);

 customers.put(customer.getId(), customer);

 return Response.ok(customer).build();  
 }

 @DELETE  
 @Path("/customers/{id}/")  
 public Response deleteCustomer(@PathParam("id") String id) {  
 System.out.println("—-invoking deleteCustomer, Customer id is: " + id);  
 long idNumber = Long.parseLong(id);  
 Customer c = customers.get(idNumber);

 Response r;  
 if (c != null) {  
 r = Response.ok().build();  
 customers.remove(idNumber);  
 } else {  
 r = Response.notModified().build();  
 }

 return r;  
 }

 @Path("/orders/{orderId}/")  
 public Order getOrder(@PathParam("orderId") String orderId) {  
 System.out.println("—-invoking getOrder, Order id is: " + orderId);  
 long idNumber = Long.parseLong(orderId);  
 Order c = orders.get(idNumber);  
 return c;  
 }

 final void init() {  
 Customer c = new Customer();  
 c.setName("John");  
 c.setId(123);  
 customers.put(c.getId(), c);

 Order o = new Order();  
 o.setDescription("order 223");  
 o.setId(223);  
 orders.put(o.getId(), o);  
 }

}  
\[/sourcecode\]

The RESTful acitons are simply mapped through the annotations @GET, @PUT, @POST and @DELETE, this allows the path and method to be associated –

&gt; @GET – @Path(“/customers/{id}/”) – public Customer getCustomer(@PathParam(“id”) String id)  
&gt; @PUT – @Path(“/customers/”) – public Response updateCustomer(Customer customer)  
&gt; @POST – @Path(“/customers/”) – public Response addCustomer(Customer customer)  
&gt; @DELETE – @Path(“/customers/{id}/”) – public Response deleteCustomer(@PathParam(“id”) String id)

The final piece of the jigsaw is to mark the returning POJO objects as XML elements using the @XmlRootElement tag –

\[sourcecode lang=”java”\] @XmlRootElement(name = "Order")  
public class Order {  // …  
 @GET  
 @Path("products/{productId}/")  
 public Product getProduct(@PathParam("productId")int productId) {  
 System.out.println("—-invoking getProduct with id: " + productId);  
 Product p = products.get(new Long(productId));  
 return p;  
 }

}  
\[/sourcecode\]

This also shows how a POJO can also be used to deliver RESTful requests by marking the getProduct method with @GET and the @Path annotation

**Putting It All Together**

The instructions to run this example are simply –

\[sourcecode\] mvn clean install  
mvn -Pserver (window 1)  
mvn -Pclient (window 2)  
\[/sourcecode\] You will get the output below –  
\[sourcecode\] Sent HTTP GET request to query customer info  
&lt;?xml version="1.0" encoding="UTF-8" standalone="yes"?&gt;&lt;Customer&gt;&lt;id&gt;123&lt;/id&gt;&lt;name&gt;John&lt;/name&gt;&lt;/Customer&gt;

Sent HTTP GET request to query sub resource product info  
&lt;?xml version="1.0" encoding="UTF-8" standalone="yes"?&gt;&lt;Product&gt;&lt;description&gt;product 323&lt;/description&gt;&lt;id&gt;323&lt;/id&gt;&lt;/Product&gt;

Sent HTTP PUT request to update customer info  
Response status code: 200  
Response body:

Sent HTTP POST request to add customer  
Response status code: 200  
Response body:  
&lt;?xml version="1.0" encoding="UTF-8" standalone="yes"?&gt;&lt;Customer&gt;&lt;id&gt;124&lt;/id&gt;&lt;name&gt;Jack&lt;/name&gt;&lt;/Customer&gt;  
\[/sourcecode\]

You can also test the GET requests in your browser using –

http://localhost:9000/customerservice/customers/123

Or –

http://localhost:9000/customerservice/orders/223/products/323