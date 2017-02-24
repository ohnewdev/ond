---
layout: post
title: "Jenkins Installation "
categories: ["TIL"]
---



### WebService

(java2blog.com 을 통해 정리 겸 작성)

Soap 을 통해 시스템간 통신을 구현해야한다.


#### 용어

* Simple Object Access Protocol(SOAP) :
  SOAP 은 XML 메시지 포멧을 사용해서 시스템간 구조화된 정보를 주고 받는데 쓰는 프로토콜입니다.

* Web Service Description Language(WSDL) :
  XML파일로서, 어떻게 웹서비스를 수행할지에 대한 내용이 기술되어 있습니다.
  URI, port, method names, arguments, data types 등이 있습니다.
  * Description :   WSDL 2.0 file 의 a root element 입니다.
  * Types : 어떤 데이터 타입이 사용되었는지 기술.
  * Binding : Webservice 가 어떻게 network protocol 에 붙어 접근할 수 있는지 기술.
  * Interface : Client 에서 request를 통해 수행할 operations 을 기술하고 있습니다. 프로그램에서 method 같은 부분입니다.
  * Service : endpoint를 기술 하고 있습니다.
  * Endpoint : 접근할 수 있는 URI 주소입니다.
  * ##### Sample WSDL file:

```XML
<?xml version="1.0" encoding="UTF-8"?>  
<wsdl:definitions targetNamespace="http://webservices.javapostsforlearning.arpit.org" xmlns:apachesoap="http://xml.apache.org/xml-soap" xmlns:impl="http://webservices.javapostsforlearning.arpit.org" xmlns:intf="http://webservices.javapostsforlearning.arpit.org" xmlns:wsdl="http://schemas.xmlsoap.org/wsdl/" xmlns:wsdlsoap="http://schemas.xmlsoap.org/wsdl/soap/" xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
<!--WSDL created by Apache Axis version: 1.4  
Built on Apr 22, 2006 (06:55:48 PDT)-->  
<wsdl:types>  
<schema elementFormDefault="qualified" targetNamespace="http://webservices.javapostsforlearning.arpit.org" xmlns="http://www.w3.org/2001/XMLSchema">  
<element name="sayHelloWorld">  
 <complexType>  
  <sequence>  
   <element name="name" type="xsd:string"/>  
  </sequence>  
 </complexType>  
</element>  
<element name="sayHelloWorldResponse">  
 <complexType>  
  <sequence>  
   <element name="sayHelloWorldReturn" type="xsd:string"/>  
  </sequence>  
 </complexType>  
</element>  
</schema>  
</wsdl:types>  
<wsdl:message name="sayHelloWorldRequest">  
   <wsdl:part element="impl:sayHelloWorld" name="parameters"/>  
</wsdl:message>  
<wsdl:message name="sayHelloWorldResponse">  
   <wsdl:part element="impl:sayHelloWorldResponse" name="parameters"/>  
</wsdl:message>  
<wsdl:portType name="HelloWorld">  
   <wsdl:operation name="sayHelloWorld">  
      <wsdl:input message="impl:sayHelloWorldRequest" name="sayHelloWorldRequest"/>  
      <wsdl:output message="impl:sayHelloWorldResponse" name="sayHelloWorldResponse"/>  
   </wsdl:operation>  
</wsdl:portType>  
<wsdl:binding name="HelloWorldSoapBinding" type="impl:HelloWorld">  
   <wsdlsoap:binding style="document" transport="http://schemas.xmlsoap.org/soap/http"/>  
   <wsdl:operation name="sayHelloWorld">  
      <wsdlsoap:operation soapAction=""/>  
      <wsdl:input name="sayHelloWorldRequest">  
         <wsdlsoap:body use="literal"/>  
      </wsdl:input>  
      <wsdl:output name="sayHelloWorldResponse">  
         <wsdlsoap:body use="literal"/>  
      </wsdl:output>  
   </wsdl:operation>  
</wsdl:binding>  
<wsdl:service name="HelloWorldService">  
   <wsdl:port binding="impl:HelloWorldSoapBinding" name="HelloWorld">  
      <wsdlsoap:address location="http://localhost:8080/SimpleSOAPExample/services/HelloWorld"/>  
   </wsdl:port>  
</wsdl:service>  
</wsdl:definitions>
```



*  Universal Description, Discovery and Integration(UDDI) :
디렉토리 형태로 보여주기 위한 서비스 입니다.


#### 웹서비스 종류
: SOAP, Restful web service 가 있습니다.


##### SOAP
: 간단한 구조로는 다음과 같이 2개 구성요소가 있습니다.
* Client
* Service Provider
![그림][1]
[그림 - http://4.bp.blogspot.com][2]


이 다이어그램에서 처럼 클라이언트는 다음 정보등을 알아야 합니다.
	* 웹서비스 서버의 위치
	* Functions 사양과 return type
	* 통신 프로토콜
	* Input/output formats

Service Provider 는 위 의 내용을 담은 XML 양식을 생성해서 클라이언트 쪽에 전달하고, Client 는 Web service 에 접근할 수 있게 됩니다. 이때의 XML파일을 WSDL(Web Service Description Language) 이라고 합니다.


----------


#### WSDL

WSDL 을 통해 알수 있는 것은
* Port / Endpoint - 웹서비스 URL
* Input message format
* Output message format
* 따라야 하는 Security protocol
* 웹서비스에서 사용하는 프로토콜


----------


##### 웹서비스 접근 방법
2가지가 있다.

1. Service provider knows client :
   WSDL 을 Client 쪽에 전달하고, 그 WSDL 을 이용하여 웹서비스에 접근할 수 있다.

    ![enter image description here][3]

2. Service provider register its WSDL to UDDI :
   Service Provider 가 WSDL을 UDDI 에 등록하고 Client 가 UDDI 를 통해 웹서비스에 등록하는 경우입니다.
  UDDI 는 Universal Description, Discovery and Integration 의 약어로 디렉토리 서비스 입니다.
절차는
    1. Service Provider가 UDDI 에 WSDL 을 등록합니다.
    2. Client 는 UDDI 안에서 서비스를 찾습니다.
    3. UDDI 는 모든 Service Provider 들이 제공하는 service를 보여줍니다.
    4. Client 는 Service Provider 를 선택합니다.
    5. UDDI는 선탠된 Service Provider 의 WSDL 을 전달합니다.
    6. 이 WSDL 을 이용하여 Client 는 Web Service에 접근할 수 있습니다.

    ![enter link description here][4]


----------


##### SOAP web service example in java using eclipse
Hellow world 예제 입니다.
Eclipse 를 통해 통 WSDL, sub, endpoint 등을 작업을 수행할 수 있습니다.

1. Dynamic webproject 를 만듭니다. 톰캣서버도 지정했습니다.
![enter image description here][5]

#
2. package를 생성합니다. org.arpit.javapostsforlearning.webservices
![enter image description here][6]

#
3. HelloWorld.java 작성

    ```java
    package org.arpit.javapostsforlearning.webservices;

    public class HelloWorld {
    	public String sayHelloWorld(String name){
    		return "Hello world from " + name;
    	}
    }
    ```
#     
4. project->new->web service
    ![enter image description here][7]

#
5. 아래와 같이 설정

![enter image description here][8]
org.arpit.javapostsforlearning.webservices.HelloWorld
왼쪽 슬라이딩 부분을 Test service and Test Client level 단계까지 올립니다.
이렇게 하면,
(몇 번 화면이 꿈벅꿈벅하더니...
http://localhost:8080/SimpleSOAPExampleClient/sampleHelloWorldProxy/TestClient.jsp
으로 웹페이지가 보이고 )
그리고,
아래와 같이 SimpleSOAPExampleClient 로 프로젝트가 하나 더 자동으로 생성됩니다.

![enter image description here][9]

6. 그리고, start Server
 (위 생성과정에서 이미 톰캣서버를 통해 Service Provider, Client 가 모두 스타트 되어 있습니다.)
![enter image description here][10]

7. 스타트가 정상적으로 된다면 아래와 같은 화면을 볼 수 있을 것입니다.
   ![enter image description here][11]

Input 화면 부분에 이름을 넣으면, 아래 Result 에 결과가 잘 리턴되는 것을 테스트 해볼 수 있습니다.


계속....

----------



  * JAX-WS : Java API for XML Web services
  * Restful web services : a stateless client-server architecture in which the web services are viewed as resources and can be identified by their URIs. Web services client uses that URI to access the resource.
[Read more..](http://www.java2blog.com/2016/06/web-services-interview-questions.html#hgcouSe4vPgitRig.99)

[관련 Link]
 - [Web Service tutorial](http://www.java2blog.com/2013/03/web-service-tutorial.html)


  [1]: http://4.bp.blogspot.com/-Cz7P6bD6aJA/UVBl4j5c1gI/AAAAAAAAA7k/K5ieuSi1Kyc/s1600/SOAPWordPlane.jpg
  [2]: http://4.bp.blogspot.com%5D%28http://www.java2blog.com/2013/03/soap-web-service-tutorial.html
  [3]: http://3.bp.blogspot.com/-KpvjWmoMNfI/UVBmCe3HZtI/AAAAAAAAA7s/rIm3eNmR9R0/s1600/SOAPWordOneLevel.jpg
  [4]: http://2.bp.blogspot.com/-g3wRq-dxoak/UVBmKcL4X1I/AAAAAAAAA70/sWNJBuQpVWA/s1600/SOAPWordUDDIDiagram.jpg
  [5]: http://2.bp.blogspot.com/-_Ms8rDU8Rf0/UU4RhG5yAjI/AAAAAAAAA6k/V1oCx-SwiUE/s1600/NewProjectSOAPExample.gif
  [6]: http://1.bp.blogspot.com/-VDQx7-YVVXo/UU4RwYIT7PI/AAAAAAAAA6s/bA_xs1E89EQ/s1600/NewPackageSOAPExample.gif
  [7]: http://1.bp.blogspot.com/-6VwJVx0V1XI/UU4ShRAYHOI/AAAAAAAAA60/K8ANG-H-Dpk/s1600/newWebservice.gif
  [8]: http://1.bp.blogspot.com/-BHas7nLGUDU/UU4SxVLHXbI/AAAAAAAAA68/GIXmmf4UAl4/s1600/WebServiceApacheAxis.gif
  [9]: http://2.bp.blogspot.com/-7X4IiwtvBCc/UU4WLTU2ILI/AAAAAAAAA7M/JRM-SD6lsRw/s1600/SOAPClientCreated.bmp
  [10]: http://1.bp.blogspot.com/-aZBdfeyhP_A/UU4TiJTGkII/AAAAAAAAA7E/__2Gt9SrIDo/s1600/startServer.gif
  [11]: http://3.bp.blogspot.com/-aflUA272QuQ/UU4WtbqWpnI/AAAAAAAAA7U/hHOP1tRAvuA/s1600/testingWebService.gif
