# HTTP Traffic Analysis Write-up

## 1. Objective

Analize HTTP request on Wireshark

## 2. Environment

**Tools Used:** 
- Wireshark 

---

## 3. Methodology

1.  Start to capture traffic
2.  Open www.expample.com
3.  Stop capturing traffic
4.  Filter http request
5.  Follow TCP Stream

---

## 4. Traffic Capture Overview

There are 2 HTTP requests and 2 corresponding responses.

The first request is a GET HTTP:

GET / HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:150.0) Gecko/20100101 Firefox/150.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate
Connection: keep-alive
Upgrade-Insecure-Requests: 1
DNT: 1
Sec-GPC: 1
Priority: u=0, i

And the response: 

HTTP/1.1 200 OK
Date: Wed, 06 May 2026 09:20:25 GMT
Content-Type: text/html
Transfer-Encoding: chunked
Connection: keep-alive
Server: cloudflare
Last-Modified: Fri, 01 May 2026 01:24:29 GMT
Allow: GET, HEAD
Age: 15
cf-cache-status: HIT
Content-Encoding: gzip
CF-RAY: 9f76e8b06cfcdbde-FRA

173
..........eQ.n.0...+X.j.....M	(..r..l.............Rr....wvf...	......m.........\#.V3....?p.E.'8 ...j......X./S.\..xF.Mq!.ccp....8
....L.Xl.,.x....Y.o...1....".7.z:-...8.W..2
.9...R....*....3...T&.......$.@UW..n..k..#^..z........J.h.].!........1-....[.'.`.oydHO..HI.
....(..y..v....S..wv...1.,<.!?.	...rO%~/...?....*-....1F..bd.....C.. w.I~....B....ZB..............p.-....
0

The server responds with `HTTP/1.1 200 OK`, indicating a successful request. 
The content type is `text/html` and it is compressed using gzip (`Content-Encoding: gzip`). 
The response is delivered using chunked transfer encoding, where `173` represents the chunk size in hexadecimal and `0` marks the end of the response. 

After this response the browser ask for favicon with 404 response.

Request:
GET /favicon.ico HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:150.0) Gecko/20100101 Firefox/150.0
Accept: image/avif,image/webp,image/png,image/svg+xml,image/*;q=0.8,*/*;q=0.5
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate
Connection: keep-alive
Referer: http://example.com/
DNT: 1
Sec-GPC: 1
Priority: u=6

Response:
HTTP/1.1 404 Not Found
Date: Wed, 06 May 2026 09:20:25 GMT
Content-Type: text/html
Transfer-Encoding: chunked
Connection: keep-alive
Server: cloudflare
Age: 1489
cf-cache-status: HIT
Content-Encoding: gzip
CF-RAY: 9f76e8b1a864dbde-FRA

173
..........eQ.n.0...+X.j.....M	(..r..l.............Rr....wvf...	......m.........\#.V3....?p.E.'8 ...j......X./S.\..xF.Mq!.ccp....8
....L.Xl.,.x....Y.o...1....".7.z:-...8.W..2
.9...R....*....3...T&.......$.@UW..n..k..#^..z........J.h.].!........1-....[.'.`.oydHO..HI.
....(..y..v....S..wv...1.,<.!?.	...rO%~/...?....*-....1F..bd.....C.. w.I~....B....ZB..............p.-....
0


### 4.1 Total Packets Observed
4 packets
---

### 4.2 Identified Flow

| #  | Type     | Description         |
|----|----------|---------------------|
| 71 | HTTP     | Request GET         |
| 75 | HTTP     | Response 200        | 
| 93 | HTTP     | Request GET Favicon |
| 96 | HTTP     | Response 404        |
|----|----------|---------------------|


