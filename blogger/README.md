# Blogger tests

First of all we need to define what we want to test and what we want to observe

Definions:

I. Scaling observation
    1. Test API without any scaling technique and check how many RPS we can achieve depending on user and user hatch

    2. Test API with fixed 2 replicas and check how many RPS we can achieve depending on user and user hatch

    3. Test API with autoscaling 3 - 7 replicas and check how many RPS we can achieve depending on user and user hatch

II. Avaiability observation
    1. Test API with no rolling update technique selected and check avaiability of service

    2. Test API with one of rolling update technique and check avaiability of service

Having definition what we want to test we can choose tool to do it properly 
Locust - https://locust.io/
