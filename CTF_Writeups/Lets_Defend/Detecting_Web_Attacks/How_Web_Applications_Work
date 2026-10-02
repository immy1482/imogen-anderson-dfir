# How Web Applications Work

| Metadata | Details |
| :--- | :--- |
| **Topic / Area** | [Web Security / Fundamentals ] |
| **Platform / Course** | [LetsDefend] |
| **Completion Date** | [02-10-2026] |
| **Key Frameworks / Standards** | [OSI Model, HTTP Requests & Responses] |

---

## Summary

> **Overview:** This module breaks down the fundamental mechanics of the Hyper-Text Transfer Protocol (HTTP) operating at Layer 7 of the OSI model. It details the client-server request-response lifecycle, headers, cookie-based session management, response status codes, and how analyzing these elements helps identify anomalies or security incidents.

---

## Key Takeaways & Core Theory

* **OSI Model Placement:** HTTP operates at Layer 7 (Application Layer), relying on lower layers such as Ethernet (Layer 2), IP (Layer 3), TCP (Layer 4), and SSL/TLS (Layer 6) to establish connectivity and security before HTTP communication begins.
* **HTTP Request Structure:** Consists of a Request Line (Method + Path), Headers (Host, User-Agent, Cookie, etc.), an Empty Line (separator), and an optional Request Body (data/parameters).
* **HTTP Response Structure:** Consists of a Status Line (HTTP versions and HTTP Response Status Code), Response Headers (date, connection, server, content-type etc.) and Response Body (which contains the resource sent by the server and requested by the client.)


---

## Questions

#### Q1: What layer is HTTP on in the OSI model?
* **Answer:** `Application`
* **Notes / Explanation:** Even though on typical images of the OSI model, the application layer is on top - but when recieving data, the process is reversed, therefore making the top layer - *Application*, the 7th. 

#### Q2: Which HTTP Request header contains browser and operating system information?
* **Answer:** `Application`
* **Notes / Explanation:** The User-Agent string identifies the client's browser and operating system version. SOC analysts monitor this header to detect automated vulnerability scanners or anomalous clients.

#### Q3: What is the HTTP Response status code that indicates the request was successful?
* **Answer:** `200`
* **Notes / Explanation:** Status codes 200-299 are all successful responses, but the standard code of 200 is what is used for successful HTTP requests. 

#### Q4: Which HTTP Request Method ensures that the submitted parameters do not appear in the Request URL?
* **Answer:** `POST`
* **Notes / Explanation:** Unlike GET requests (which pass parameters directly in the URL query string), POST requests send parameters inside the Request Message Body.


#### Q5: Which HTTP Request header contains session tokens?
* **Answer:** `Cookie`
* **Notes / Explanation:** Websites / web applications store tokens and information about the user's device in the cookie header so the user can be kept authenticated across requests. 

---

## Link to the session and to other references

* [https://app.letsdefend.io/training/lesson_detail/how-web-applications-work-web-attacks-101](#)
* [https://www.cloudflare.com/learning/ddos/what-is-layer-7/](#)
* [https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Overview](#) 
