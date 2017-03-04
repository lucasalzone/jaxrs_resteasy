# Utilizzare JAX-RS con RESTEasy.
Su richiesta di alcuni miei colleghi, pubblico un'esempio di come utilizzare RESTEasy anzichè Jersey con JAX-RS.

## Introduzione
In questo esempio vedremo come utilizzare RESTEasy anzichè Jersey per esporre servizi JAX-RS.
Vi consiglio di leggere il post precedente se non avete mai implementato un servizio Restful, [Creare un servizio restful con Jersey](https://lucasalzone.wordpress.com/2017/02/12/restfull-webservice-with-jersey/).
Inoltre potete fare riferimento alla documentazione di JBOSS relativa all'implementazione RestEasy.

[RestEasy Jboss](http://docs.jboss.org/resteasy/docs/2.3.7.Final/userguide/pdf/resteasy-reference-guide-en-US.pdf)

## Download esempio
L'esempio contiene un progetto funzionante, con le seguenti API.


**GitHub** [download del codice](https://github.com/lucasalzone/jaxrs_resteasy)

## Le dependencies
Il progetto è stato creato come progetto Maven base e sono state aggiunge le seguenti **dependencies**:

```xml

		<dependency>
			<groupId>org.jboss.resteasy</groupId>
			<artifactId>resteasy-jaxrs</artifactId>
			<version>2.3.7.Final</version>
		</dependency>

		<dependency>
			<groupId>javax.json</groupId>
			<artifactId>javax.json-api</artifactId>
			<version>1.0</version>
		</dependency>
		<dependency>
			<groupId>org.glassfish</groupId>
			<artifactId>javax.json</artifactId>
			<version>1.0.4</version>
		</dependency>

```	
Di seguito una breve descrizione:

* **resteasy-jaxrs**: implementazione delle specifiche JAX-RS.
* **javax.json-api**: interfacce previste delle specifiche Json Processing
* **javax.json** (org.glassfish): implementazione javax.json

## Configurazione Servlet (web.xml)
Per poter utilizzare l'implementazione di RESTEasy, anche con questa implementazione è possibile configurare sia una servlet che un listener per esporre i servizi.

Di seguito la configurazione utilizzata, dove si può notare che è stato utilizzato il parametro **resteasy.scan** per dirgli di attivare lo scan su tutte le classi per cercare i servizi da esporre, oltre che il parametro **org.jboss.resteasy.plugins.server.servlet.HttpServletDispatcher** per dirgli su quale prefisso esporre il servizio, che di default è /*.


```	xml
	
	<servlet>stessa
		<servlet-name>resteasy-servlet</servlet-name>
		<servlet-class>org.jboss.resteasy.plugins.server.servlet.HttpServletDispatcher</servlet-class>
		<init-param>
			<param-name>resteasy.scan</param-name>
			<param-value>true</param-value>
		</init-param>
		<init-param>
			<param-name>resteasy.servlet.mapping.prefix</param-name>
			<param-value>/api</param-value>
		</init-param>
	</servlet>stessa

	<servlet-mapping>
		<servlet-name>resteasy-servlet</servlet-name>
		<url-pattern>/api/*</url-pattern>
	</servlet-mapping>
	
```		

## Resource definition
Per l'implementazione del servizio si può tranquillamente utilizzare quella presente nel post Creare un servizio restful con Jersey](https://lucasalzone.wordpress.com/2017/02/12/restfull-webservice-with-jersey/), che comunque riporto di seguito:

```java
@Path("hello") 
public class HelloWorld {
	@GET 
	@Produces("text/plain")
	public String getHello() {
		return "ciao Mondo";
	}
	...
}
```

## Test Resource
Inserendo sul browser l'url http://<context-pat>/api/hello, potremmo vedere il nostro servizio funzionante.
