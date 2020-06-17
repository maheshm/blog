---
categories: [tech-blog]
layout: post
title: Meddling with CA Certificates
---

I had two servers that were running Ubuntu and both had some legacy piece of code. One was a worker and other a web service. There was a job that was failing and I was trying to fix it. I forgot the fact that there was a worker machine and that the job was actually running in the woker. But anyway, after couple of minutes I figured out that the issue was that the ca-certficates had expired and we had the external service that depended on a HTTPS GET request and it was failing. I still could not figure out why the update-ca-certificates job did not run, but I some how managed to get the certificates to work and it was all fine in the web server.

It did not take long to realise that the job was still failing as it was running the code in the worker machine and I had not fixed it there. Now comes the tricky part.

I did exactly what I did in the web server and expected it to work, but voila!, it did not. After digging deeper I figured out that the two servers were running different versions of Ubuntu - web was on 16.04 and Worker on 14.04 (which is no more getting any updates!). Now I was not sure whether that could have been the reason. So I went and read the man pages of update-ca-certificates and figured out that it only copies stuff from `/usr/share/ca-certificates` directory, but it is the `ca-certificates` package that actually fetches the latest certificates.

A quick run of `dpkg-reconfigure ca-certificates` warned me that it was going to update all my root certificates. But soon I realised that it did not update any as the package itself was too old (2017). The same package on web machine was from 2019. Quick reaction was to download all the certificates from web and upload it on worker, but instead I tried to install the same package using dpkg (as apt would require me to do a dist-upgrade, or I am too naive to know how to do it). It automatically ran the `update-ca-certificate`service and phew!, I had the newer certificates in place and the job ran fine! Long time since I looked in to code behind a package in Debian! That was fun!

Will try to figure out why they are restricting the update of ca-certificates to the specific package and not keeping the source of certificates open.
