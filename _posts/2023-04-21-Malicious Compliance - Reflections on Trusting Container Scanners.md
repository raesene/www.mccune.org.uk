---
title: Malicious Compliance - Reflections on Trusting Container Scanners
type: post
category: talks
---

A commonly recommended best practice for security and compliance is to scan container images for vulnerabilities before allowing them to run inside a cluster. Many organizations codify allow/deny policies based on the results of these scans, using this policy-as-code approach to form the basis of trust. But what exactly are container scanners looking for? And can you always trust the results? Let’s break this down layer by layer, from an attacker perspective. Why do certain changes in the way images are built produce wildly varying results? Can the flexibility in how container images are built and distributed be used to alter or prevent scanning tools from being able to fully understand what's in a container? How might clever image builders use these tricks to avoid scrutiny from these tools? Join the hacker crew known as SIG-Honk, and let’s have some fun! Ian Coldwater, Duffie Cooley, Brad Geesaman, and Rory McCune will demonstrate some creative ways to intentionally bypass container image analysis and admission control detection. Attendees will walk away with a greater understanding of the limitations of tooling used to validate images, and learn how to create better security policies in their own environments. The results may surprise you!

This talk was delivered at Kubecon 2023, alongside Ian Coldwater, Duffie Cooley and Brad Geesaman.There's a recording from that event [here](https://youtu.be/9weGi0csBZM)
