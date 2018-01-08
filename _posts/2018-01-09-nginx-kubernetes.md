---
layout: post
title:  "nginx in kubernetes"
date:   2018-01-09 14:58:39 +0100
comments: true 
categories: kubernetes reverse-proxy nginx ingress ingress-controller
---

nginx conf example:
https://github.com/nginxinc/kubernetes-ingress/tree/master/examples/complete-example


simple config practice online: https://www.katacoda.com/courses/kubernetes/create-kubernetes-ingress-routes
k8s resource: Ingress
k8s deployment: Ingress-controller and service

An Ingress Controller is a daemon, deployed as a Kubernetes Pod, that watches the apiserver's /ingresses endpoint for updates
to the Ingress resource. Its job is to satisfy requests for Ingresses.

User tries to deploy: https://medium.com/@cashisclay/kubernetes-ingress-82aa960f658e


example on AWS: https://daemonza.github.io/2017/02/13/kubernetes-nginx-ingress-controller/
example on GCE: https://github.com/kubernetes/ingress-gce

design issues and good advices: http://danielfm.me/posts/painless-nginx-ingress.html
