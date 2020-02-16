---
layout: post
title:  "Consuming a REST Service"
date:   2020-02-03 12:34:56 +1000
categories: PeopleCode REST
blurb: Example code, and a list of server configurations to check.

---
The code for calling a REST service is thankfully very simple. There is a much, much [longer way](http://deepakpeoplesoft.blogspot.com/2017/02/consume-rest-web-service-in-peoplesoft.html ) to call a REST service where you create Documents, sub Documents, Messages, Service Operations and on and on and on, but this example uses the [.ConnectorRequest method]([https://docs.oracle.com/cd/E92519_02/pt856pbr3/eng/pt/tpcr/langref_IntBrokerClassMethods-071247.html#u597a1438-7ff6-4da7-9d4b-7037d0ac5cfb](https://docs.oracle.com/cd/E92519_02/pt856pbr3/eng/pt/tpcr/langref_IntBrokerClassMethods-071247.html#u597a1438-7ff6-4da7-9d4b-7037d0ac5cfb)) found in the ```%IntBroker``` Class. There is an example and a little more information [here in PeopleBooks]([https://docs.oracle.com/cd/E92519_02/pt856pbr3/eng/pt/tiba/task_BypassingIntegrationEnginestoSendMessages-497cf6.html#u6c77a94a-e02b-42e1-b5df-37f6fde524d4](https://docs.oracle.com/cd/E92519_02/pt856pbr3/eng/pt/tiba/task_BypassingIntegrationEnginestoSendMessages-497cf6.html#u6c77a94a-e02b-42e1-b5df-37f6fde524d4)).

```
Local Message &Request, &Response;
Local boolean &b;

/* createRequest */
&Request = CreateMessage(Message.IB_GENERIC);
&Request.IBInfo.IBConnectorInfo.ConnectorName = "HTTPTARGET";
&Request.IBInfo.IBConnectorInfo.ConnectorClassName = "HttpTargetConnector";

/* setHTTPProperties */
&b = &Request.IBInfo.IBConnectorInfo.AddConnectorProperties("Method", "GET", %HttpProperty);
&b = &Request.IBInfo.IBConnectorInfo.AddConnectorProperties("URL", "https://api.example.com/api", %HttpProperty);

/* setHeaders */
&b = &Request.IBInfo.IBConnectorInfo.AddConnectorProperties("accept", "application/json", %Header);
&b = &Request.IBInfo.IBConnectorInfo.AddConnectorProperties("authorization", &yourTokenString, %Header);
&b = &Request.IBInfo.IBConnectorInfo.AddConnectorProperties("another-header", &anotherValue, %Header);

/* getResponse */
local string &json;
&Response = %IntBroker.ConnectorRequest(&Request, True);
&json = &Response.GetContentString()
```
There is a post [here]([https://evlpanda.github.io/peoplecode/json/2019/12/19/reading_json.html](https://evlpanda.github.io/peoplecode/json/2019/12/19/reading_json.html)) that covers reading the resulting json into an array.

This *should* work out-of-the-box, but sometimes servers are not configured correctly. If they aren't configured correctly you'll get *nothing* back from ```.GetContentString```. You will find the errors in a server's log file, somewhere.

Here is an **Environment Checklist**

- [ ] Service Operation IB_GENERIC is active. 
- [ ] Service Operation IB_GENERIC's only routing is active.
- [ ] PeopleTools > Web Profile  > Web Profile Config > <your_profile> > Authorized Sites > Add target URIs
- [ ] Any and all firewalls are configured.
- [ ] Ports (443 for example) are open.
