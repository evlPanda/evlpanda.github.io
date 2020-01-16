---
layout: post
title:  "A Simple REST Call"
date:   2020-01-16 12:34:56 +1000
categories: PeopleCode REST
blurb: Two examples of a simple REST call in PeopleCode

---

Two examples of a simple REST call in PeopleCode.

The first is undocumented, but provides the response code unlike the second:
```
Local Message &Request;
Local boolean &b;
&Request = CreateMessage(Operation.IB_GENERIC_REST_POST);
&Request.OverrideURIResource("https://api.example.com/v1/example");
&b = &Request.AddSegmentHeader("accept", "application/json");
&b = &Request.AddSegmentHeader("authorization", "Basic invalidlY2EtNWRjNS00ZTI1LWI5XWEtMTZiMzc0ZGM1OGJhOmVhZjQ1NDE5LTkyNTUtNDFlZi05NTdkLWVhNDlmNGI1MGRkMw==");

Local Message &Response;
&Response = %IntBroker.ProvideRestTest(&Request, "GET");

MessageBox(0, "", 0, 0, "response code: " | String(&Response.HTTPResponseCode) | ", content: " | &Response.GetContentString());
```
The second is [documented in PeopleBooks](https://docs.oracle.com/cd/E91187_01/pt855pbr2/eng/pt/tiba/task_BypassingIntegrationEnginestoSendMessages-497cf6.html?pli=ul_d133e61_tiba):

```
Local Message &Request;
&Request = CreateMessage(Message.IB_GENERIC);
&Request.IBInfo.IBConnectorInfo.ConnectorName = "HTTPTARGET";
&Request.IBInfo.IBConnectorInfo.ConnectorClassName = "HttpTargetConnector";
   
Local boolean &b;
&b = &Request.IBInfo.IBConnectorInfo.AddConnectorProperties("Method", "GET", %HttpProperty);
&b = &Request.IBInfo.IBConnectorInfo.AddConnectorProperties("URL", "https://utility.stage.api.aws.uq.edu.au/v1/health", %HttpProperty);
&b = &Request.IBInfo.IBConnectorInfo.AddConnectorProperties("accept", "application/json", %Header);
&b = &Request.IBInfo.IBConnectorInfo.AddConnectorProperties("authorization", "Basic invalidlY2EtNWRjNS00ZTI1LWI5XWEtMTZiMzc0ZGM1OGJhOmVhZjQ1NDE5LTkyNTUtNDFlZi05NTdkLWVhNDlmNGI1MGRkMw==", %Header);
   
Local Message &Response;
&Response = %IntBroker.ConnectorRequest(&Request, True);
If &Response.ResponseStatus = %IB_Status_Success Then
   Local XmlDoc &XMLResponse = &Response.GetXmlDoc();
Else
   MessageBox(0, "", 0, 0, &Response.IBException.ToString());
End-If;
```
