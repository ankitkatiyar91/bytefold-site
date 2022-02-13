---
layout: post
title: Load Balancing a grpc service on k8s
date: 2022-02-06 00:56:25.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Miscellaneous
tags:
- http2 load balancing
- kubernates
meta:
  _edit_last: '1'
author:
  login: ankitkatiyar91
  email: ankitkatiyar67@gmail.com
  display_name: Ankit Katiyar
  first_name: Ankit
  last_name: Katiyar
permalink: "/load-balancing-a-grpc-service-on-k8s/"
---
<!-- wp:paragraph -->

Load balancing a GRPC service on a Kubernetes environment could be a big challenge if you miss out on the fact that GRPC works on HTTP2 and HTTP2 does the magic of multiplexing and reuses the connection for multiple requests. While trying to scale a GRPC service found some interesting insights about how GRPC works with ClusterIP.

<!-- /wp:paragraph -->

<!-- wp:heading -->

## **Scenario**

<!-- /wp:heading -->

<!-- wp:image {"id":638,"sizeSlug":"large","linkDestination":"none"} -->

![]({{ site.baseurl }}/assets/images/2022/02/stage-1-1024x188.png)

<!-- /wp:image -->

<!-- wp:columns -->

<!-- wp:column {"width":"100%"} -->
<!-- wp:paragraph -->

We created a GRPC service and deployed it on k8s using a ClusterIP service which was exposed using an Nginx server. We did some load tests and figured out its vertical scale limits. so we decided to scale the number of pods on our service to achieve better results. As the[k8s documentation](https://kubernetes.io/docs/concepts/services-networking/service/) mentions

<!-- /wp:paragraph -->

<!-- /wp:column -->

<!-- /wp:columns -->

<!-- wp:quote -->

> By default, kube-proxy in userspace mode chooses a backend via a round-robin algorithm

<!-- /wp:quote -->

<!-- wp:paragraph -->

With this assumption we expected our horizontally scaled service to evenly distribute load across pods. When we did this we were surprised to see that most of the load was still going to 1 pod. We increased the number of pods but the pattern continues and the load is not evenly distributed.

<!-- /wp:paragraph -->

<!-- wp:heading -->

## Problem

<!-- /wp:heading -->

<!-- wp:paragraph -->

After spending hours on the internet and playing with various configurations, we found that GRPC needs HTTP2 and ClusterIP was using HTTP2 which was doing multiplexing. Because of this multiplexing, it was reusing existing connections and sending new requests to the same k8s Pod rather than creating a new connection to the second pod. This phenomenon continues with more pods and a few pods that get most of the load.

<!-- /wp:paragraph -->

<!-- wp:image {"id":640,"sizeSlug":"large","linkDestination":"none"} -->

![]({{ site.baseurl }}/assets/images/2022/02/stage-2-1-1024x389.png)  

_Un-even Load Distribution_

<!-- /wp:image -->

<!-- wp:heading -->

## Solution

<!-- /wp:heading -->

<!-- wp:paragraph -->

As we figured out the problem is with the load balancing system and a solution was achieved in two steps:

<!-- /wp:paragraph -->

<!-- wp:list -->

- Create a headless k8s service.
- Using a custom/external load balancer.

<!-- /wp:list -->

<!-- wp:paragraph -->

Let's look at both of these options

<!-- /wp:paragraph -->

<!-- wp:heading -->

## Creating headless k8s service

<!-- /wp:heading -->

<!-- wp:enlighter/codeblock {"language":"yaml"} -->

```
apiVersion: v1
kind: Service
metadata:
  name: headless-svc
spec:
  clusterIP: None # <= Don't forget!!
  selector:
    app: web
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

<!-- /wp:enlighter/codeblock -->

<!-- wp:paragraph -->

A Kubernetes headless service is a ClusterIP service that doesn't create a cluster IP for your service and exposes the IPs of all the pods that are created. With this approach, you can get the IPs of all the pods that are available for your App.

<!-- /wp:paragraph -->

<!-- wp:heading -->

## Using custom/external load balancer

<!-- /wp:heading -->

<!-- wp:paragraph -->

Once your headless service is available you can use any load balancer that has the capability to discover IPs from the ClusterIP service and do the load balancing. For us, [envoy](https://www.envoyproxy.io/) being part of our system came to our rescue. We were using envoy to support REST clients on our GRPC service.

<!-- /wp:paragraph -->

<!-- wp:image {"id":642,"sizeSlug":"large","linkDestination":"none"} -->

![]({{ site.baseurl }}/assets/images/2022/02/stage-3-1024x664.png)

<!-- /wp:image -->

<!-- wp:paragraph -->

**Example Envoy Config**

<!-- /wp:paragraph -->

<!-- wp:enlighter/codeblock {"language":"yaml"} -->

```
clusters:
  - name: headless-svc
    type: STRICT_DNS
    lb_policy: ROUND_ROBIN
    connect_timeout: 30s
    dns_lookup_family: V4_ONLY
    load_assignment:
      cluster_name: headless-svc
      endpoints:
        - lb_endpoints:
            - endpoint:
                address:
                  socket_address:
                    address: headless-svc
                    port_value: 80
```

<!-- /wp:enlighter/codeblock -->

<!-- wp:heading -->

## Summary

<!-- /wp:heading -->

<!-- wp:paragraph -->

Kubernetes provides a great abstraction to our deployments and sometimes it can make debugging load balancing and other internals of services very complex. HTTP2 brings some performance optimisation which can cause behaviours that are not good for some loads. Having an external load balancer that brings more control and clarity can be a boon. Envoy is one such proxy that can do a lot of powerful stuff with very little resources.

<!-- /wp:paragraph -->

